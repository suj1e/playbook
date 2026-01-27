# Java 微服务架构设计方案（最终完整版）

> 定位：公司级 / 架构评审级 / 可长期演进
> 核心关键词：事件驱动、最终一致性、去中心化、可观测性

---

## 一、总体设计原则

* 单一职责（一个组件只解决一类问题）
* 事件优先（跨服务通过事件，而不是流程调用）
* 最终一致性（不引入分布式事务）
* 去中心化（避免强中心调度）
* 可重放（关键动作可补偿、可回放）

---

## 二、技术选型总览

| 分类      | 技术                   | 角色       |
| ------- | -------------------- | -------- |
| 注册 / 配置 | Nacos                | 服务发现、配置  |
| 网关      | Spring Cloud Gateway | 鉴权、限流、灰度 |
| HTTP 通信 | @HttpExchange        | 同步查询     |
| 消息队列    | **Kafka**            | 事件中枢     |
| 任务调度    | **Quartz（集群）**       | 时间驱动     |
| 数据库     | MySQL                | 各服务私有    |
| 缓存      | Redis + Caffeine     | 多级缓存     |
| 搜索      | Elasticsearch        | 搜索 / 分析  |
| 分布式 ID  | Snowflake            | 全局唯一     |
| 分布式锁    | Redisson             | 业务唯一性    |
| 限流      | Resilience4j         | 稳定性      |
| Trace   | Micrometer + OTEL    | 全链路追踪    |

---

## 三、整体架构（逻辑视图）

```
Client
  ↓
API Gateway（鉴权 / 限流 / Trace）
  ↓
业务微服务（User / Order / Payment / Job）
  ↓
Kafka（事件总线）
  ↓
下游消费者 / ES / 通知 / 补偿

说明：
- 每个服务拥有独立 DB / Redis
- Kafka 是跨服务状态传播中枢
```

---

## 四、服务通信规范（HTTP）

### 使用范围

* 只用于：同步查询
* 禁止：跨服务事务、流程编排

### 统一 HttpExchange

```java
@HttpExchange("/users")
public interface UserClient {
  @GetExchange("/{id}")
  UserDTO getById(@PathVariable Long id);
}
```

---

## 五、Kafka 事件驱动总规范

### 事件模型

```json
{
  "eventId": "snowflake-id",
  "eventType": "ORDER_CREATED",
  "bizId": "orderId",
  "occurredAt": "ISO-8601",
  "payload": {}
}
```

### Topic 命名

```
{domain}.{entity}.{action}.v{version}
```

示例：

```
order.order.created.v1
```

### Kafka 强制规范

* Key = bizId（保证顺序）
* Consumer 必须幂等
* 必须支持 DLQ
* 必须支持回放

---

## 六、定时任务与调度（Quartz × Kafka）

> 本章节用于规范：Quartz 如何与 Kafka 协作，实现时间驱动 + 状态驱动融合架构。

> **核心原则：** Quartz 决定"什么时候"，Kafka 决定"谁来处理"。

* Quartz：时间触发
* Kafka：状态扩散
* 业务服务：事件消费者

### 6.1 职责边界（强制）

| 模块     | 只允许做           |
| ------ | -------------- |
| Quartz | 定时触发           |
| Job    | 扫描 / 校验 / 产生事件 |
| Kafka  | 分发             |
| 业务服务   | 真正执行业务         |

❌ 禁止：

* Quartz 中写复杂业务
* Quartz 直接改业务状态（Outbox 除外）

### 6.2 Quartz 集群模型

```
多个 Job 实例
   ↓
共享 QRTZ_* 表
   ↓
DB 行锁
   ↓
同一时间只有一个实例执行
```

### 6.3 Quartz + Kafka 标准执行链路

```
Quartz 触发
  ↓
Job 扫描状态
  ↓
构造领域事件
  ↓
发送 Kafka
  ↓
消费者处理
  ↓
推进业务状态
```

### 6.4 经典场景：订单超时关闭

#### Job（只扫描）

```java
public class OrderTimeoutScanJob implements Job {
  public void execute(JobExecutionContext ctx) {
    List<Long> ids = orderRepo.findTimeoutOrders();
    ids.forEach(eventPublisher::publishTimeout);
  }
}
```

#### 发布事件

```java
kafkaTemplate.send(
  "order.order.timeout.v1",
  orderId.toString(),
  event
);
```

#### 消费处理

```java
@KafkaListener(topics = "order.order.timeout.v1")
public void onTimeout(OrderTimeoutEvent e) {
  orderService.closeOrder(e.getOrderId());
}
```

### 6.5 Quartz + Outbox（强可靠）

#### 使用场景

* 核心状态变更
* 不允许事件丢失

#### 扫描模型

```
业务写库 + 写 Outbox
   ↓
Quartz 扫描 NEW
   ↓
Kafka 投递
   ↓
标记 SENT
```

### 6.6 延迟任务 vs 定时任务

| 场景    | 方案             |
| ----- | -------------- |
| 精准定时  | Quartz         |
| 高吞吐延迟 | Kafka 延迟 Topic |
| 简单延迟  | Redis ZSet     |

### 6.7 防雪崩与安全设计

* Job 分片扫描
* Job 幂等
* Job 限流
* Job 不形成调用链

### 6.8 调度反模式（必须避免）

* 用 Quartz 执行业务逻辑
* Quartz 调 Quartz
* Job 串行流程编排
* Job 写多服务数据

### 6.9 小结

> Quartz 负责时间，Kafka 负责扩散，调度不执行业务，业务不依赖调度。

---

## 七、Outbox Pattern（强制）

### 使用场景

* DB 与 Kafka 必须一致
* 不接受事件丢失

### 表设计

```sql
CREATE TABLE outbox_event (
  id BIGINT PRIMARY KEY,
  event_type VARCHAR(64),
  topic VARCHAR(128),
  biz_id VARCHAR(64),
  payload JSON,
  status VARCHAR(16),   -- NEW / SENT / FAILED
  retry_count INT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

### 标准流程

```
业务事务
  ↓
写业务表 + 写 Outbox（同事务）
  ↓
提交成功
  ↓
Quartz 扫描 Outbox
  ↓
投递 Kafka
  ↓
标记 SENT
```

---

## 八、分布式 ID 设计（生产级）

### 主方案：Snowflake

* DB 主键：Snowflake
* Kafka Key：业务 ID（Snowflake）
* 禁止使用 UUID
* 禁止 DB 自增

### 兜底（极端场景）

* Redis INCR（仅非核心）

---

## 九、缓存设计（强规范）

### 9.1 多级缓存

```
Caffeine
  ↓
Redis
  ↓
Database
```

### 9.2 一致性策略

> Cache Aside

```
写：
  更新 DB
  删除 Redis
```

### 9.3 防护策略

| 问题 | 方案       |
| -- | -------- |
| 穿透 | 空值缓存     |
| 击穿 | 本地缓存 + 锁 |
| 雪崩 | TTL 随机   |

---

## 十、事件治理（公司级规范）

### 10.1 事件不可变原则

* 事件一经发布不可修改
* 只允许新增字段
* 不允许删除字段
* 不允许修改语义

### 10.2 版本演进

| 操作         | 是否允许 |
| ---------- | ---- |
| 新增字段       | ✅    |
| 删除字段       | ❌    |
| 新 Topic 版本 | ✅    |

---

## 十一、Trace & 可观测性（必须）

### 11.1 目标

* HTTP / Kafka / Quartz 全链路 Trace
* 一个请求一个 TraceId

### 11.2 规范

| 场景     | 方式                 |
| ------ | ------------------ |
| HTTP   | traceparent Header |
| Kafka  | Header 传递          |
| Quartz | Job 生成 Trace       |

---

## 十二、架构审查检查清单

| 模块       | 状态            |
| -------- | ------------- |
| Trace    | ✅ 已补          |
| Metrics  | 建议 Micrometer |
| DLQ      | 必须            |
| 事件回放     | 建议            |
| ADR 决策记录 | 建议            |

---

## 十三、终极总结

> 这是一个以 Kafka 为事件中枢、
> 以 Quartz 解决时间问题、
> 以 Outbox 保证一致性、
> 以 Trace 保证可观测性的现代微服务体系。

**核心原则：**
> Quartz 负责时间，Kafka 负责扩散，
> 调度不执行业务，业务不依赖调度。

---
