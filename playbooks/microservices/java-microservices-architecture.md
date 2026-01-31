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

## 三、服务拆分原则（架构评审必问）

### 3.1 什么时候拆

**必须拆的场景：**
* 多个团队并行开发，频繁冲突
* 单个服务代码量超过 50 万行
* 部署时间超过 30 分钟
* 不同模块有明显的性能要求差异（如 CPU 密集 vs IO 密集）
* 需要独立扩展的模块

**不应该拆的场景：**
* 团队规模小于 5 人
* 业务还在快速变化期
* 没有成熟的运维能力

### 3.2 拆分原则

| 原则       | 说明                     | 示例           |
| -------- | ---------------------- | ------------ |
| DDD 领域驱动 | 按业务边界拆，而非技术边界          | 订单域 vs 用户域     |
| 单一职责     | 一个服务只做一件事              | 支付服务只管支付     |
| 数据独立     | 每个服务有自己的 DB，禁止跨库join    | 订单库、用户库完全独立  |
| 高内聚低耦合   | 服务内部紧密关联，服务间尽量独立       | 用户服务不依赖订单服务  |

### 3.3 反模式（必须避免）

❌ 按技术层拆分（Controller 服务、Service 服务）
❌ 按数据库表拆分（User 表一个服务、Order 表一个服务）
❌ 微服务过多（小于 10 人的团队搞 20+ 服务）
❌ 服务间同步调用链过长（超过 3 层）

---

## 四、整体架构（逻辑视图）

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

## 五、服务通信规范（HTTP）

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

### 超时与重试

```java
@HttpExchange("/users")
public interface UserClient {

  // 默认超时 3s，重试 2 次
  @GetExchange("/{id}")
  @Timeout(3000)
  @Retry(maxAttempts = 3)
  UserDTO getById(@PathVariable Long id);

  // 只读接口可重试
  // 写操作禁止重试
}
```

### 服务调用铁律

* 必须设置超时时间（默认 3s）
* 必须设置重试策略（只读接口可重试）
* 禁止循环调用（A→B→C→A）
* 禁止长调用链（超过 3 层）

---

## 六、熔断、降级、限流（Resilience4j）

### 6.1 熔断（CircuitBreaker）

```java
// 熔断配置
@Configuration
public class ResilienceConfig {

  @Bean
  public Customizer<Resilience4JCircuitBreakerFactory> cbCustomizer() {
    return factory -> factory.configureDefault(id -> new Resilience4JConfigBuilder(id)
      .circuitBreakerConfig(CircuitBreakerConfig.custom()
        .slidingWindowSize(10)           // 滑动窗口大小
        .failureRateThreshold(50)        // 失败率阈值 50%
        .waitDurationInOpenState(Duration.ofSeconds(30))  // 开启状态等待 30s
        .permitNumberOfCallsInHalfOpenState(5)  // 半开状态允许 5 次调用
        .build())
      .build());
  }
}

// 使用
@CircuitBreaker(name = "userService", fallbackMethod = "getByIdFallback")
public UserDTO getById(Long id) {
  return userClient.getById(id);
}

private UserDTO getByIdFallback(Long id, Exception e) {
  log.error("用户服务熔断: {}", e.getMessage());
  return UserDTO.empty();  // 返回降级数据
}
```

### 6.2 限流（RateLimiter）

```java
// 限流配置：每秒最多 100 次
@RateLimiter(name = "userService", fallbackMethod = "getByIdFallback")
public UserDTO getById(Long id) {
  return userClient.getById(id);
}
```

### 6.3 舱壁隔离（Bulkhead）

```java
// 隔离配置：最多 10 个并发线程
@Bulkhead(name = "userService", fallbackMethod = "getByIdFallback")
public List<UserDTO> getAllUsers() {
  return userClient.getAll();
}
```

### 6.4 降级策略

| 场景           | 降级方案              |
| ------------ | ----------------- |
| 用户服务不可用      | 返回缓存数据            |
| 推荐服务不可用      | 返回默认推荐列表          |
| 搜索服务不可用      | 跳过搜索，只展示列表        |
| 支付服务不可用      | 提示"支付功能暂时不可用"     |

### 6.5 检查清单

- [ ] 所有外部调用都有熔断
- [ ] 有限流保护
- [ ] 有降级方案
- [ ] 有监控告警

---

## 七、认证授权体系

### 7.1 整体方案

**采用 JWT + Gateway 统一认证：**

```
Client
  ↓ (登录)
Gateway Auth Service → JWT
  ↓ (携带 JWT)
Gateway (验证 JWT) → 解析用户信息
  ↓ (透传 Header)
业务微服务 (信任 Gateway，读取用户信息)
```

### 7.2 JWT Token 设计

```json
// Header
{
  "alg": "RS256",
  "typ": "JWT"
}

// Payload
{
  "sub": "user-id",
  "name": "张三",
  "roles": ["USER", "ADMIN"],
  "exp": 1735689600,
  "iat": 1735686000
}

// Signature (RS256 非对称加密)
```

### 7.3 Gateway 认证逻辑

```java
@Component
public class AuthFilter implements GlobalFilter {

  @Override
  public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
    String token = exchange.getRequest().getHeaders().getFirst("Authorization");

    if (token == null) {
      return unauthorized(exchange);
    }

    try {
      // 验证 JWT
      Claims claims = jwtParser.parseClaimsJws(token.replace("Bearer ", ""))
                                  .getBody();

      // 透传用户信息到下游服务
      ServerHttpRequest mutatedRequest = exchange.getRequest().mutate()
        .header("X-User-Id", claims.getSubject())
        .header("X-User-Roles", String.join(",", claims.get("roles", List.class)))
        .build();

      return chain.filter(exchange.mutate().request(mutatedRequest).build());

    } catch (JwtException e) {
      return unauthorized(exchange);
    }
  }
}
```

### 7.4 业务服务读取用户信息

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

  @PostMapping
  public Order create(@RequestHeader("X-User-Id") String userId,
                      @RequestBody OrderRequest req) {
    // 直接使用 Gateway 透传的用户 ID，无需再次验证
    return orderService.create(userId, req);
  }
}
```

### 7.5 服务间调用认证

```java
// 方案一：内部服务之间使用固定的 Service Token
@FeignClient(name = "order-service", configuration = ServiceClientConfig.class)
public interface OrderClient {
  // ...
}

@Configuration
public class ServiceClientConfig {
  @Bean
  public RequestInterceptor serviceTokenInterceptor() {
    return template -> {
      template.header("X-Service-Token", "fixed-internal-token");
      template.header("X-Service-Name", "payment-service");
    };
  }
}

// 方案二：使用 mTLS（更安全）
```

### 7.6 权限控制

```java
// 方法级权限控制
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) { }

// 资源级权限控制
@PreAuthorize("@orderService.canAccess(#orderId, @userService.getCurrentUserId())")
public Order getOrder(Long orderId) { }
```

---

## 八、配置管理规范（Nacos）

### 8.1 配置分层

```
Namespace (命名空间)
  ├── dev (开发环境)
  ├── test (测试环境)
  └── prod (生产环境)

Group (分组)
  ├── order-service
  ├── user-service
  └── payment-service

Data ID (配置文件)
  ├── order-service.yaml       # 公共配置
  ├── order-service-dev.yaml   # 开发环境
  └── order-service-prod.yaml  # 生产环境
```

### 8.2 配置优先级

```
本地配置文件 < Nacos 公共配置 < Nacos 环境配置 < Nacos 动态配置
```

### 8.3 敏感配置加密

```yaml
# nacos-config.yaml
# ❌ 明文密码
spring.datasource.password=123456

# ✅ 加密密码（使用 Jasypt）
spring.datasource.password=ENC(encrypted_string)
```

### 8.4 配置变更流程

```
1. 在 Nacos 控制台修改配置
2. Nacos 推送变更到所有服务实例
3. 服务自动刷新 @RefreshScope Bean
4. 灰度验证（可选）
5. 全量发布
```

```java
@RefreshScope  // 支持配置动态刷新
@RestController
public class OrderController {

  @Value("${order.max-amount:10000}")
  private Long maxAmount;
}
```

### 8.5 配置规范

* 所有配置必须在 Nacos 管理，禁止硬编码
* 敏感信息必须加密
* 环境差异通过 Namespace 隔离
* 配置变更必须灰度验证

---

## 九、Kafka 事件驱动总规范

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

## 十、定时任务与调度（Quartz × Kafka）

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

### 6.2 Quartz 集群原理（深度解析）

#### 6.2.1 为什么需要集群

**单节点问题：**
```
单节点 Quartz
  ↓
服务器宕机 / 升级
  ↓
所有定时任务停止
  ↓
业务中断 ❌
```

**集群解决方案：**
```
多节点 Quartz 集群
  ↓
任意节点宕机，其他节点继续工作
  ↓
任务不中断 ✅
```

#### 6.2.2 集群原理核心：数据库作为协调中心

Quartz 集群不使用 ZooKeeper/Redis，而是**直接用数据库**协调：

```
节点 A                    节点 B                    节点 C
  ↓                        ↓                        ↓
  └──────────────┬───────────────────────┘
                 ↓
         共享数据库（MySQL）
                 ↓
         QRTZ_* 表（11张表）
                 ↓
    通过数据库行锁实现分布式协调
```

#### 6.2.3 核心表结构

| 表名                    | 作用                |
| --------------------- | ----------------- |
| QRTZ_JOB_DETAILS      | 存储任务详情            |
| QRTZ_TRIGGERS         | 存储触发器配置           |
| QRTZ_CRON_TRIGGERS    | 存储 Cron 表达式       |
| QRTZ_SCHEDULER_STATE  | 记录集群节点状态          |
| **QRTZ_LOCKS**        | **分布式锁表（核心）**      |
| QRTZ_FIRED_TRIGGERS   | 记录正在执行的任务         |

#### 6.2.4 抢占式调度原理（关键）

**问题：** 三个节点同时启动，谁来执行任务？

**答案：** 谁先抢到锁，谁执行

```
时刻 T0: 任务需要触发
  ↓
节点 A、B、C 同时发现任务到期
  ↓
同时执行: SELECT * FROM QRTZ_LOCKS WHERE SCHED_NAME = 'SchedulerName' AND LOCK_NAME = 'TRIGGER_ACCESS'
  ↓
同时执行: UPDATE QRTZ_LOCKS SET LOCK_NAME = 'TRIGGER_ACCESS' WHERE SCHED_NAME = 'SchedulerName' AND LOCK_NAME = 'TRIGGER_ACCESS'
  ↓
数据库行锁生效，只有一个 UPDATE 成功
  ↓
假设节点 A 抢到锁
  ↓
节点 A: 执行任务 ✅
节点 B: 抢锁失败，等待下次重试 ⏳
节点 C: 抢锁失败，等待下次重试 ⏳
```

#### 6.2.5 完整执行流程

```
1. 节点启动
   ↓
   向 QRTZ_SCHEDULER_STATE 插入本节点记录
   状态: STANDBY（待命）

2. 抢夺 Leader（可选）
   ↓
   通过 QRTZ_LOCKS 抢夺
   胜者成为 Leader，负责集群调度

3. 任务调度循环（每 20-30s）
   ↓
   3.1 查询待触发任务
       SELECT * FROM QRTZ_TRIGGERS WHERE NEXT_FIRE_TIME < NOW()

   3.2 抢执行锁
       UPDATE QRTZ_LOCKS SET LOCK_NAME = 'TRIGGER_ACCESS' WHERE LOCK_NAME = 'TRIGGER_ACCESS'
       (利用 DB 行锁，只有一个节点成功)

   3.3 锁成功的节点
       ↓
       a. 更新触发器状态: ACQUIRED
       b. 插入 QRTZ_FIRED_TRIGGERS
       c. 调用 Job.execute()
       d. 更新下一次触发时间: NEXT_FIRE_TIME
       e. 释放锁

   3.4 锁失败的节点
       ↓
       跳过，等待下一轮

4. 节点宕机检测
   ↓
   Leader 定期检查 QRTZ_SCHEDULER_STATE
   发现某节点超时未更新（last_checkin_time）
   ↓
   假设该节点死亡，重新分配其任务
```

#### 6.2.6 Spring Boot 配置示例

```yaml
spring:
  quartz:
    job-store-type: jdbc  # 使用 JDBC 存储
    jdbc:
      initialize-schema: never  # 生产环境设为 never
    properties:
      org:
        quartz:
          scheduler:
            instanceName: clusteredScheduler
            instanceId: AUTO  # 自动生成节点 ID
          jobStore:
            class: org.quartz.impl.jdbcjobstore.JobStoreTX
            driverDelegateClass: org.quartz.impl.jdbcjobstore.StdJDBCDelegate
            tablePrefix: QRTZ_  # 表前缀
            isClustered: true  # 启用集群 ⭐
            clusterCheckinInterval: 20000  # 节点心跳 20s
          threadPool:
            class: org.quartz.simpl.SimpleThreadPool
            threadCount: 10  # 每个节点 10 个线程
            threadPriority: 5
```

#### 6.2.7 集群关键参数说明

| 参数                                     | 说明              | 推荐值   |
| -------------------------------------- | --------------- | ----- |
| org.quartz.jobStore.isClustered        | 是否启用集群          | true  |
| org.quartz.jobStore.clusterCheckinInterval | 节点心跳间隔          | 20s   |
| org.quartz.scheduler.instanceId        | 节点 ID 生成策略       | AUTO  |
| org.quartz.scheduler.batchTriggerAcquisitionMaxCount | 一次获取任务数量（性能优化） | 50    |

#### 6.2.8 集群环境注意事项

**1. 时钟同步**
```bash
# 所有节点必须同步时间，否则任务会重复执行或漏执行
ntpdate -u time.nist.gov
```

**2. 数据库连接池**
```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20  # Quartz 集群需要更多连接
```

**3. 节点数量**
```
推荐: 2-3 个节点
原因: 节点过多会增加数据库压力，且抢锁竞争激烈
```

**4. 任务执行时长**
```java
// ❌ 错误: 任务执行时间超过调度间隔
@Scheduled(cron = "0 */1 * * * ?")  // 每分钟执行
public void longRunningJob() {
  Thread.sleep(70000);  // 执行 70 秒
}

// ✅ 正确: 任务执行时间远小于调度间隔
@Scheduled(cron = "0 */10 * * * ?")  // 每 10 分钟执行
public void shortJob() {
  // 执行几秒钟
}
```

#### 6.2.9 集群常见问题

**Q1: 为什么任务重复执行了？**
```
原因:
1. 时钟不同步
2. clusterCheckinInterval 设置过大
3. 任务执行时间超过调度间隔

解决:
1. 配置 NTP 同步
2. 缩短心跳间隔到 15-20s
3. 检查任务执行时长
```

**Q2: 节点宕机后任务会丢失吗？**
```
不会丢失。
原因:
- 任务状态存储在 QRTZ_TRIGGERS 表
- 节点宕机后，Leader 会检测并重新分配
- 下一次调度时正常执行
```

**Q3: 如何判断集群工作正常？**
```sql
-- 查看集群节点
SELECT * FROM QRTZ_SCHEDULER_STATE;

-- 正常输出示例:
-- | INSTANCE_NAME           | LAST_CHECKIN_TIME    |
-- | node-1689001234567      | 2024-07-10 10:30:25 |
-- | node-1689001234890      | 2024-07-10 10:30:20 |
-- | node-1689001235123      | 2024-07-10 10:30:28 |

-- LAST_CHECKIN_TIME 应该持续更新
```

#### 6.2.10 集群 vs 单机对比

| 特性   | 单机模式    | 集群模式              |
| ---- | ------- | ----------------- |
| 高可用  | ❌ 单点故障  | ✅ 节点故障自动转移         |
| 负载分担 | ❌ 单机承担  | ✅ 多节点分担            |
| 配置复杂度 | 简单      | 中等（需要共享数据库）         |
| 数据库压力 | 低       | 中（心跳 + 抢锁）          |
| 适用场景 | 小型项目    | 生产环境、关键业务          |

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

## 十一、Outbox Pattern（强制）

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

## 十二、分布式 ID 设计（生产级）

### 主方案：Snowflake

* DB 主键：Snowflake
* Kafka Key：业务 ID（Snowflake）
* 禁止使用 UUID
* 禁止 DB 自增

### 兜底（极端场景）

* Redis INCR（仅非核心）

---

## 十三、缓存设计（强规范）

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

## 十四、事件治理（公司级规范）

### 14.1 事件不可变原则

* 事件一经发布不可修改
* 只允许新增字段
* 不允许删除字段
* 不允许修改语义

### 14.2 版本演进

| 操作         | 是否允许 |
| ---------- | ---- |
| 新增字段       | ✅    |
| 删除字段       | ❌    |
| 新 Topic 版本 | ✅    |

---

## 十五、监控告警体系

### 15.1 三大支柱

| 支柱        | 工具               | 作用         |
| --------- | ---------------- | ---------- |
| Metrics   | Prometheus + Grafana | 数值指标监控     |
| Logging   | ELK / Loki        | 日志聚合分析     |
| Tracing   | Grafana Tempo       | 分布式链路追踪    |

### 15.2 关键指标（必须监控）

**业务指标：**
* 订单量、支付成功率、用户活跃度
* 核心接口 QPS、响应时间（P99 < 500ms）
* 错误率（< 0.1%）

**系统指标：**
* CPU 使用率 < 70%
* 内存使用率 < 80%
* DB 连接池使用率 < 80%
* Kafka 消费延迟 < 1s

### 15.3 告警规则

```yaml
# Prometheus 告警规则示例
groups:
  - name: service_alerts
    rules:
      # P0: 服务不可用
      - alert: ServiceDown
        expr: up{job="order-service"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "服务 {{ $labels.job }} 不可用"

      # P1: 错误率过高
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.01
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "服务 {{ $labels.job }} 错误率过高"

      # P2: 响应时间过长
      - alert: HighLatency
        expr: histogram_quantile(0.99, http_request_duration_seconds) > 0.5
        for: 10m
        labels:
          severity: warning
```

### 15.4 告警分级

| 级别  | 条件             | 响应时间   | 通知方式           |
| --- | -------------- | -----: | -------------- |
| P0  | 服务完全不可用        | 5 分钟   | 电话 + 钉钉/企微 + PagerDuty |
| P1  | 核心功能不可用        | 15 分钟  | 钉钉/企微 + 短信      |
| P2  | 性能下降、错误率上升     | 30 分钟  | 钉钉/企微         |
| P3  | 资源使用率过高（未影响业务） | 1 小时   | 钉钉/企微         |

---

## 十六、数据一致性补偿（Saga 模式）

### 16.1 Saga 模式概述

当业务流程需要跨多个服务时，如何保证最终一致性？

**示例：创建订单流程**
```
1. Order Service: 创建订单（PENDING）
2. Payment Service: 扣款
3. Inventory Service: 扣减库存
4. Order Service: 更新订单状态（COMPLETED）
```

如果第 3 步失败，需要补偿前面的操作。

### 16.2 Choreography Saga（编排式）

```java
// 每个服务监听上游事件，发布自己的事件
@Service
public class OrderService {

  public void createOrder(Order order) {
    orderRepository.save(order);
    // 发布订单创建事件
    kafkaTemplate.send("order.created.v1", order);
  }

  @KafkaListener(topics = "payment.failed.v1")
  public void onPaymentFailed(PaymentFailedEvent event) {
    // 补偿：取消订单
    Order order = orderRepository.findById(event.getOrderId());
    order.cancel("支付失败");
    kafkaTemplate.send("order.cancelled.v1", order);
  }
}

@Service
public class PaymentService {

  @KafkaListener(topics = "order.created.v1")
  public void onOrderCreated(OrderCreatedEvent event) {
    try {
      paymentService.deduct(event.getOrder());
      kafkaTemplate.send("payment.success.v1", event);
    } catch (Exception e) {
      kafkaTemplate.send("payment.failed.v1", event);
    }
  }
}
```

### 16.3 Orchestration Saga（协调式）

```java
// 协调器管理整个流程
@Service
public class CreateOrderSaga {

  public void execute(CreateOrderCommand cmd) {
    try {
      // Step 1: 创建订单
      Order order = orderService.create(cmd);

      // Step 2: 扣款
      paymentService.charge(order);

      // Step 3: 扣库存
      inventoryService.deduct(order);

      // Step 4: 完成订单
      orderService.complete(order.getId());

    } catch (PaymentFailedException e) {
      // 补偿：取消订单
      orderService.cancel(cmd.getOrderId(), "支付失败");
    } catch (InventoryFailedException e) {
      // 补偿：退款 + 取消订单
      paymentService.refund(cmd.getOrderId());
      orderService.cancel(cmd.getOrderId(), "库存不足");
    }
  }
}
```

### 16.4 补偿事务最佳实践

* 每个操作必须定义对应的补偿操作
* 补偿操作必须是幂等的
* 补偿操作可能失败，需要人工介入
* 记录完整的 Saga 执行日志

---

## 十七、服务版本管理

### 17.1 API 版本策略

**推荐：URL 版本控制**

```
/api/v1/users
/api/v2/users
```

**不推荐：Header 版本控制**
```
GET /users
Header: API-Version: v2
```

### 17.2 兼容性原则

* **向后兼容**：v2 发布后，v1 仍可用（至少 6 个月）
* **只增不减**：新增字段可以，删除/修改字段需要新版本
* **弃用通知**：在响应 Header 中标注弃用信息

```java
@GetMapping("/v1/users")
@Deprecated
public ResponseEntity<List<User>> getUsersV1() {
  return ResponseEntity.ok()
    .header("X-API-Deprecated", "true")
    .header("X-API-Sunset", "2025-12-31")
    .header("X-API-Recommended-Version", "v2")
    .body(userService.getUsers());
}
```

### 17.3 多版本并存策略

```java
// 使用策略模式支持多版本
public interface UserService {

  @GetMapping({"/v1/users", "/v2/users"})
  public ResponseEntity<?> getUsers(
    @RequestHeader("API-Version") String version
  ) {
    if ("v1".equals(version)) {
      return ResponseEntity.ok(userServiceV1.getUsers());
    } else {
      return ResponseEntity.ok(userServiceV2.getUsers());
    }
  }
}
```

---

## 十八、灰度发布

### 18.1 发布策略

| 策略       | 说明                  | 风险  |
| -------- | ------------------- | --- |
| 蓝绿部署     | 两套环境，一键切换           | 低   |
| 金丝雀发布    | 先小流量验证，再全量          | 中   |
| A/B 测试   | 按用户属性分流，对比效果       | 低   |
| 特性开关     | 代码控制功能开关，可随时回退      | 低   |

### 18.2 网关灰度配置

```yaml
# Spring Cloud Gateway 灰度配置
spring:
  cloud:
    gateway:
      routes:
        - id: order-service-v2
          uri: lb://order-service-v2
          predicates:
            - Path=/api/orders/**
            - Header=X-Gray-Release, v2-users  # 灰度用户
            - Weight=10  # 10% 流量到 v2

        - id: order-service-v1
          uri: lb://order-service
          predicates:
            - Path=/api/orders/**
```

### 18.3 灰度检查清单

- [ ] 核心接口回归测试通过
- [ ] 监控指标正常（错误率、响应时间）
- [ ] 灰度用户无异常反馈
- [ ] 数据库变更向后兼容
- [ ] 可一键回滚

---

## 十九、性能调优

### 19.1 JVM 参数（8C16G 示例）

```bash
java -server \
  -Xms8g -Xmx8g \
  -XX:+UseG1GC \
  -XX:MaxGCPauseMillis=200 \
  -XX:+HeapDumpOnOutOfMemoryError \
  -XX:HeapDumpPath=/logs/heapdump.hprof \
  -Xlog:gc*:file=/logs/gc.log:time,tags:filecount=10,filesize=100M \
  -jar app.jar
```

### 19.2 线程池配置

```yaml
spring:
  task:
    execution:
      pool:
        core-size: 8      # CPU 核心数
        max-size: 16      # 2x CPU
        queue-capacity: 1000
        keep-alive: 60s

  mvc:
    async:
      request-timeout: 30000  # 30s
```

### 19.3 数据库连接池（HikariCP）

```yaml
spring:
  datasource:
    hikari:
      minimum-idle: 10
      maximum-pool-size: 50
      idle-timeout: 600000
      max-lifetime: 1800000
      connection-timeout: 30000
      connection-test-query: SELECT 1
```

### 19.4 性能检查清单

- [ ] JVM 参数已优化
- [ ] 线程池大小合理
- [ ] 连接池配置合理
- [ ] 无 N+1 查询
- [ ] 热点数据已缓存
- [ ] 慢查询已优化
- [ ] GC 频率 < 1次/分钟

---

## 二十、安全加固

### 20.1 接口安全

| 措施       | 说明                    |
| -------- | --------------------- |
| HTTPS    | 生产环境强制使用              |
| 签名验证     | 关键接口验证请求签名             |
| 限流       | 防止 DDoS 攻击             |
| 参数校验     | @Valid 验证所有输入          |
| SQL 注入防护  | 使用参数化查询               |
| XSS 防护    | 前端输出转义                 |

### 20.2 敏感数据加密

```java
// 数据库字段加密（JPA）
@Entity
public class User {

  @Convert(converter = EncryptConverter.class)
  private String idCard;  // 身份证号加密存储

  @Convert(converter = EncryptConverter.class)
  private String phone;   // 手机号加密存储
}

// AES 加密转换器
public class EncryptConverter implements AttributeConverter<String, String> {
  @Override
  public String convertToDatabaseColumn(String attribute) {
    return AESUtils.encrypt(attribute);
  }
}
```

### 20.3 安全审计日志

```java
@Aspect
@Component
public class SecurityAuditAspect {

  @Around("@annotation(Auditable)")
  public Object audit(ProceedingJoinPoint pjp) throws Throwable {
    String user = SecurityContextHolder.getCurrentUserId();
    String operation = pjp.getSignature().getName();

    try {
      Object result = pjp.proceed();
      auditLogService.log(user, operation, "SUCCESS", null);
      return result;
    } catch (Exception e) {
      auditLogService.log(user, operation, "FAILED", e.getMessage());
      throw e;
    }
  }
}
```

### 20.4 安全检查清单

- [ ] 所有 API 需要认证（除公开接口）
- [ ] 敏感数据加密存储
- [ ] 关键操作有审计日志
- [ ] 定期安全扫描
- [ ] 依赖库无已知漏洞
- [ ] SQL 注入防护
- [ ] XSS/CSRF 防护

---

## 二十一、Trace & 可观测性（必须）

### 21.1 目标

* HTTP / Kafka / Quartz 全链路 Trace
* 一个请求一个 TraceId

### 21.2 规范

| 场景     | 方式                 |
| ------ | ------------------ |
| HTTP   | traceparent Header |
| Kafka  | Header 传递          |
| Quartz | Job 生成 Trace       |

### 21.3 Grafana Tempo 配置

#### 架构说明

```
应用服务
  ↓ (OTLP)
Tempo (Trace 存储)
  ↓
Grafana (UI 查询)
  ↓
Prometheus (Metrics 关联)
```

#### 应用配置（OTEL）

```yaml
# application.yml
management:
  tracing:
    sampling:
      probability: 1.0  # 100% 采样（生产环境可降低到 0.1）
  propagation:
    type: w3c  # 使用 W3C 标准

# OTEL 导出配置
spring:
  application:
    name: order-service
  otel:
    exporter:
      otlp:
        endpoint: http://tempo:4318  # Tempo OTLP endpoint
        protocol: grpc
    resource:
      attributes:
        service.name: ${spring.application.name}
        deployment.environment: ${spring.profiles.active:dev}
```

#### Maven 依赖

```xml
<dependencies>
    <!-- Micrometer Tracing -->
    <dependency>
      <groupId>io.micrometer</groupId>
      <artifactId>micrometer-tracing-bridge-brave</artifactId>
    </dependency>

    <!-- OTEL Exporter -->
    <dependency>
      <groupId>io.opentelemetry</groupId>
      <artifactId>opentelemetry-exporter-otlp</artifactId>
    </dependency>

    <!-- Spring Boot Starter -->
    <dependency>
      <groupId>io.micrometer</groupId>
      <artifactId>micrometer-registry-prometheus</artifactId>
    </dependency>
</dependencies>
```

#### Grafana 数据源配置

1. **添加 Tempo 数据源**
   - Configuration → Data Sources → Add data source
   - 选择 "Tempo"
   - URL: `http://tempo:3100`

2. **添加 Prometheus 数据源**（关联 Metrics）
   - URL: `http://prometheus:9090`

3. **查询 Traces**
   - 在 Grafana 中导航到 "Explore"
   - 选择 "Tempo" 数据源
   - 通过 TraceID、Service Name、Span Name 搜索
   - 查看 Waterfall 视图分析调用链路

4. **Metrics ↔ Traces 关联**
   - 从 Grafana Dashboard 点击 Metrics 图表
   - 选择 "Jump to Trace" 直接跳转到对应 Trace
   - 从 Trace 页面可以查看相关 Metrics

#### 查看 Trace 示例

```bash
# 1. 从日志中获取 TraceID
grep "trace_id" logs/app.log

# 2. 在 Grafana 中搜索 TraceID
# Navigate to Explore → Tempo → Search by TraceID

# 3. 分析调用链路
# Waterfall 视图显示每个 Span 的耗时
# 点击 Span 查看详细信息、Tags、Logs

# 4. 关联 Metrics
# 在 Trace 页面点击 "Connected Metrics"
# 查看该 Service/Trace 相关的性能指标
```

### 21.4 Trace 关键检查点

- [ ] OTEL 配置正确（endpoint 可达）
- [ ] Trace 采样率合理（开发 100%，生产 10%）
- [ ] traceparent Header 正确传递
- [ ] Grafana Tempo 数据源已配置
- [ ] 可以通过 TraceID 查询到完整调用链
- [ ] Metrics 与 Traces 可以关联查询

---

## 二十二、架构审查检查清单

| 模块         | 状态             |
| ---------- | -------------- |
| 服务拆分       | ✅ 已补充          |
| 熔断降级限流     | ✅ 已补充          |
| 认证授权       | ✅ 已补充          |
| 配置管理       | ✅ 已补充          |
| 监控告警       | ✅ 已补充          |
| 数据一致性补偿    | ✅ 已补充          |
| 服务版本管理     | ✅ 已补充          |
| 灰度发布       | ✅ 已补充          |
| 性能调优       | ✅ 已补充          |
| 安全加固       | ✅ 已补充          |
| Trace       | ✅ 已补           |
| Metrics     | ✅ 已补充 (Prometheus) |
| DLQ         | 必须             |
| 事件回放        | 建议             |
| ADR 决策记录    | 建议             |

---

## 二十三、终极总结

> 这是一个以 Kafka 为事件中枢、
> 以 Quartz 解决时间问题、
> 以 Outbox 保证一致性、
> 以 Trace 保证可观测性的现代微服务体系。

**核心原则：**
> Quartz 负责时间，Kafka 负责扩散，
> 调度不执行业务，业务不依赖调度。

---
