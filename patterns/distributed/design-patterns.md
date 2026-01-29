# 分布式系统设计模式

> 构建高可用、可扩展的分布式系统

---

## 目录

- [1. Outbox 模式](#1-outbox-模式)
- [2. Saga 模式](#2-saga-模式)
- [3. CQRS 模式](#3-cqrs-模式)
- [4. Event Sourcing 模式](#4-event-sourcing-模式)
- [5. Circuit Breaker 模式](#5-circuit-breaker-模式)
- [6. Eventual Consistency 模式](#6-eventual-consistency-模式)
- [7. Sharding 模式](#7-sharding-模式)
- [8. Leader Election 模式](#8-leader-election-模式)
- [9. Retry 模式](#9-retry-模式)
- [10. Rate Limiter 模式](#10-rate-limiter-模式)

---

## 1. Outbox 模式

### 1.1 意图

**问题：** 如何保证本地数据库和消息队列的原子性？

**解决方案：** Outbox Pattern 在同一数据库事务中同时写入业务表和 Outbox 表，由后台进程扫描 Outbox 表并发送消息。

### 1.2 架构

```
业务事务
  ↓
写业务表 + 写 Outbox（同一事务）
  ↓
事务提交
  ↓
后台进程扫描 Outbox
  ↓
发送到消息队列
  ↓
标记消息已发送
```

### 1.3 实现

```sql
-- Outbox 表结构
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

```java
// 业务服务
@Transactional
public void createOrder(Order order) {
    // 写业务表
    orderRepository.save(order);

    // 写 Outbox（同一事务）
    outboxRepository.save(new OutboxEvent(
        "ORDER_CREATED",
        "order.order.created.v1",
        order.getId().toString(),
        order.toJson()
    ));
}

// Quartz 后台任务
@Scheduled(cron = "*/10 * * * * ?")
public void scanOutbox() {
    List<OutboxEvent> events = outboxRepository.findByStatus("NEW", 100);
    events.forEach(event -> {
        try {
            kafkaTemplate.send(event.getTopic(), event.getBizId(), event.getPayload());
            event.markAsSent();
            outboxRepository.save(event);
        } catch (Exception e) {
            event.incrementRetry();
            if (event.getRetryCount() > 3) {
                event.markAsFailed();
            }
            outboxRepository.save(event);
        }
    });
}
```

### 1.4 优缺点

**优点：**
- 保证最终一致性
- 不引入分布式事务
- 消息不会丢失
- 支持重试和补偿

**缺点：**
- 需要额外的 Outbox 表
- 需要后台扫描进程
- 增加系统复杂度

---

## 2. Saga 模式

### 2.1 意图

**问题：** 如何保证跨服务的事务一致性？

**解决方案：** 通过定义一系列本地事务，每个本地事务执行一个业务操作，如果失败则执行补偿事务。

### 2.2 架构

```
Order Service: 创建订单（PENDING）
  ↓
Payment Service: 扣款
  ↓
Inventory Service: 扣减库存
  ↓
Order Service: 更新订单状态（COMPLETED）
```

如果第 3 步失败：
```
Inventory Service 扣减失败
  ↓
Payment Service: 退款
  ↓
Order Service: 取消订单
```

### 2.3 实现

```java
// 协调式 Saga
@Service
public class OrderSaga {

    public void execute(CreateOrderCommand cmd) {
        try {
            // Step 1: 创建订单
            Order order = orderService.create(cmd);

            // Step 2: 扣款
            paymentService.charge(order);

            // Step 3: 扣库存
            inventoryService.deduct(order);

            // Step 4: 完成订单
            orderService.complete(order);

        } catch (PaymentFailedException e) {
            orderService.cancel(cmd.getOrderId(), "支付失败");

        } catch (InventoryFailedException e) {
            paymentService.refund(cmd.getOrderId());
            orderService.cancel(cmd.getOrderId(), "库存不足");
        }
    }
}
```

### 2.4 编排式 Saga

```java
// 事件驱动 Saga
@KafkaListener(topics = "order.created.v1")
public class PaymentService {
    @KafkaListener(topics = "payment.success.v1")
    public void onPaymentSuccess(PaymentSuccessEvent event) {
        inventoryService.deduct(event.getOrder());
    }

    @KafkaListener(topics = "payment.failed.v1")
    public void onPaymentFailed(PaymentFailedEvent event) {
        orderService.cancel(event.getOrderId());
    }
}
```

---

## 3. CQRS 模式

### 3.1 意图

**问题：** 复杂查询性能差，读写职责不清。

**解决方案：** 命令查询职责分离（CQRS）。

### 3.2 架构

```
Command（写）
  ↓
Command Handler
  ↓
Write Model
  ↓
Write DB

Query（读）
  ↓
Query Handler
  ↓
Read Model
  ↓
Read DB（可缓存）
```

### 3.3 实现

```java
// Command（写）
public record CreateOrderCommand(
    String userId,
    List<OrderItem> items
) {}

public class OrderCommandHandler {
    @Autowired
    private OrderService orderService;

    public Long handle(CreateOrderCommand command) {
        return orderService.create(command);
    }
}

// Query（读）
public record GetOrderQuery(Long orderId) {}

public class OrderQueryHandler {
    @Autowired
    private OrderQueryService orderQueryService;

    public OrderDTO handle(GetOrderQuery query) {
        return orderQueryService.getById(query.orderId());
    }
}
```

### 3.4 优缺点

**优点：**
- 读写分离，各自优化
- 查询可以缓存
- 命令和查询模型分离

**缺点：**
- 系统复杂度增加
- 最终一致性
- 需要同步机制

---

## 4. Event Sourcing 模式

### 4.1 意图

**问题：** 无法重建历史状态。

**解决方案：** 以事件为中心存储，而不是存储当前状态。

### 4.2 架构

```
事件流
  ↓
Event Store
  ↓
Snapshot（快照）
  ↓
Read Model
```

### 4.3 实现

```java
// Event
public record OrderCreatedEvent(
    String orderId,
    String userId,
    List<OrderItem> items,
    LocalDateTime createdAt
) {}

// Event Store
public interface EventStore {
    void save(DomainEvent event);
    List<DomainEvent> getEvents(String aggregateId);
}

// Aggregate Root
public class Order {
    private List<DomainEvent> events = new ArrayList<>();

    public void create(OrderCommand command) {
        // 业务逻辑
        publishEvent(new OrderCreatedEvent(...));
    }

    private void publishEvent(DomainEvent event) {
        events.add(event);
        eventStore.save(event);
    }

    public void loadFromHistory(List<DomainEvent> events) {
        // 从事件重建状态
        for (DomainEvent event : events) {
            apply(event);
        }
    }
}
```

### 4.4 优缺点

**优点：**
- 完整的历史记录
- 事件溯源
- 时序调试
- 审计日志

**缺点：**
- 事件版本管理复杂
- 重构困难
- 最终一致性
- 存储成本高

---

## 5. Circuit Breaker 模式

### 5.1 意图

**问题：** 服务故障级联传播。

**解决方案：** 检测到故障时自动熔断，快速失败。

### 5.2 状态

```
Closed（关闭）
  ↓
失败率超过阈值
  ↓
Open（开启）
  ↓
等待恢复时间
  ↓
Half-Open（半开）
  ↓
验证是否恢复
  ↓
Closed 或 Open
```

### 5.3 实现

```java
@Configuration
public class ResilienceConfig {

  @Bean
  public Customizer<Resilience4JCircuitBreakerFactory> circuitBreakerFactory() {
    return factory -> factory.configureDefault(id -> new Resilience4JConfigBuilder(id)
        .circuitBreakerConfig(CircuitBreakerConfig.custom()
            .slidingWindowSize(10)
            .failureRateThreshold(50)
            .waitDurationInOpenState(Duration.ofSeconds(30))
            .permitNumberOfCallsInHalfOpenState(5)
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
  return UserDTO.empty();
}
```

### 5.4 策略

| 策略 | 说明 |
|------|------|
| 固定超时 | 单次请求超时时间 |
| 指数退避 | 重试等待时间递增 |
| 断路器 | 检测到故障立即熔断 |
| 舱壁隔离 | 限制并发请求数 |

---

## 6. Eventual Consistency 模式

### 6.1 意图

**问题：** 分布式系统中，如何保证最终一致性？

**解决方案：** 系统在一段时间内最终达到一致状态。

### 6.2 实现方式

```java
// 最终一致性检查
@Scheduled(fixedRate = 1)  // 每秒检查一次
public void checkConsistency() {
    List<Order> orders = orderRepository.findPending();

    orders.forEach(order -> {
        Payment payment = paymentService.findByOrderId(order.getId());
        Inventory inventory = inventoryService.findByOrderId(order.getId());

        if (payment.isPaid() && inventory.isDeducted()) {
            order.markAsCompleted();
        } else if (payment.isFailed() || inventory.isInsufficient()) {
            order.markAsCancelled();
        }
    });
}
```

---

## 7. Sharding 模式

### 7.1 意图

**问题：** 单表数据量过大，查询性能下降。

**解决方案：** 按分片键将数据分散到多个数据库。

### 7.2 分片策略

| 策略 | 说明 | 示例 |
|------|------|------|
| 水平分片 | 按数据行分散 | 用户 ID 取模 |
| 垂直分片 | 按范围分散 | 按时间范围 |
| 目录分片 | 按业务领域 | 按区域/业务类型 |

### 7.3 实现

```java
// ShardingSphere 配置
spring:
  shardingsphere:
    datasource:
      names: ds0, ds1
      sharding:
        tables:
          t_user:
            actual-data-nodes: ds0, ds1
            table-strategy: inline
            key-generator-name: snowflake

// 分片算法
public class UserIdShardingAlgorithm implements PreciseShardingAlgorithm<Long> {
    @Override
    public String doSharding(Collection<String> availableTargetNames, PreciseShardingStrategy<Long> shardingStrategy) {
        return "ds" + (id % 2);  // 简单取模分片
    }
}
```

---

## 8. Leader Election 模式

### 8.1 意图

**问题：** 分布式系统中，如何选出一个主节点？

**解决方案：** 通过算法选举出一个 Leader 负责协调。

### 8.2 算法

| 算法 | 特点 | 复杂度 |
|------|------|--------|
| Raft | 强一致性、高可用 | 高 |
| Zab | 强一致性、高性能 | 中 |
| Paxos | 简单、可靠 | 中 |
| Bully | 简单、低性能 | 低 |

### 8.3 实现

```java
// 使用 Curator 框架（ZooKeeper）
public class LeaderElection {

    private CuratorFramework client;
    private LeaderSelectorListenerAdapter listener;

    public void start() throws Exception {
        client = CuratorFrameworkFactory.newClient("localhost:2181");

        client.start();

        listener = new LeaderSelectorListenerAdapter() {
            @Override
            public void takeLeadership(CuratorFramework client) {
                becomeLeader();
            }

            @Override
            public void surrenderLeadership(CuratorFramework client) {
                becomeFollower();
            }
        };

    }
}
```

---

## 9. Retry 模式

### 9.1 重试策略

| 策略 | 说明 | 适用场景 |
|------|------|----------|
| 固定间隔 | 每次间隔相同 | 简单场景 |
| 指数退避 | 间隔递增 | 网络不稳定 |
| 随机抖动 | 添加随机值 | 避免雷群效应 |

### 9.2 实现

```java
@Configuration
public class RetryConfig {

  @Bean
  public RetryTemplate retryTemplate() {
    return new RetryTemplate(
        new SimpleRetryStrategy(
            2000,       // 初始间隔
            2.0,        // 倍数因子
            60000        // 最大间隔
        ),
        3  // 最大重试次数
    );
  }
}
```

---

## 10. Rate Limiter 模式

### 10.1 算法

| 算法 | 说明 | 优点 | 缺点 |
|------|------|------|------|
| 固定窗口 | 固定时间窗口计数 | 简单 | 边缘情况 |
| 滑动窗口 | 滑动时间窗口计数 | 平滑限流 | 实现复杂 |
| 漏桶 | 按令牌桶计数 | 简单 | 突发流量不均匀 |
| 令牌桶 | 以固定速率消费令牌 | 精确控制 | 内存消耗大 |

### 10.2 实现

```java
// Resilience4j RateLimiter
@Configuration
public class RateLimiterConfig {

  @Bean
    public Customizer<Resilience4JRateLimiterConfig> rateLimiterConfig() {
        return factory -> factory.configureDefault(id -> new Resilience4JConfigBuilder(id)
            .limitForPeriod(100)                    // 100 �请求/秒
            .limitRefreshPeriod(Duration.ofSeconds(1))  // 每秒刷新
            .timeoutDuration(Duration.ofSeconds(5))
            .build());
    }
}

// 使用
@RateLimiter(name = "api", fallbackMethod = "rateLimitFallback")
public ResponseEntity<?> apiEndpoint() {
    return ResponseEntity.ok().build();
}
```

---

## 速查表

### Outbox 模式
```sql
-- Outbox 表
CREATE TABLE outbox_event (
  id BIGINT PRIMARY KEY,
  event_type VARCHAR(64),
  topic VARCHAR(128),
  biz_id VARCHAR(64),
  payload JSON,
  status VARCHAR(16),
  retry_count INT
);
```

### Saga 模式
```java
// 协调式
try {
    step1();
    step2();
    step3();
} catch (Exception e) {
    compensate();
}

// 编排式
@KafkaListener(topics = "event.topic")
public void handleEvent() {
    // 处理事件
}
```

### CQRS 模式
```java
// Command
commandHandler.handle(command);

// Query
queryHandler.handle(query);
```

### Circuit Breaker 模式
```java
@CircuitBreaker(name = "service", fallbackMethod = "fallback")
public Result doWork() {
    return remoteService.call();
}
```

---

## 相关文档

- [Java 微服务架构](../../playbooks/microservices/java-microservices-architecture.md)
- [后端设计模式](../backend/design-patterns.md)
