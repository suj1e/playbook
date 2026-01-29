# 后端设计模式

> 创建可维护、可扩展的后端系统

---

## 目录

- [1. 创建型模式](#1-创建型模式)
- [2. 结构型模式](#2-结构型模式)
- [3. 行为型模式](#3-行为型模式)
- [4. 企业级模式](#4-企业级模式)
- [5. Java Spring 实践](#5-java-spring-实践)

---

## 1. 创建型模式

### 1.1 单例模式 (Singleton)

**意图：** 确保一个类只有一个实例，并提供全局访问点。

```java
public class DatabaseConnection {
    private static volatile DatabaseConnection instance;

    private DatabaseConnection() {
        // 私有构造函数
    }

    public static DatabaseConnection getInstance() {
        if (instance == null) {
            synchronized (DatabaseConnection.class) {
                if (instance == null) {
                    instance = new DatabaseConnection();
                }
            }
        }
        return instance;
    }
}
```

**使用场景：**
- 数据库连接池
- 配置管理器
- 日志记录器
- 线程池

---

### 1.2 工厂模式 (Factory)

**意图：** 定义创建对象的接口，让子类决定实例化哪个类。

```java
// 抽象工厂
public interface PaymentFactory {
    Payment createPayment(PaymentType type);
}

// 具体工厂
public class AlipayFactory implements PaymentFactory {
    @Override
    public Payment createPayment(PaymentType type) {
        return new AlipayPayment();
    }
}

// 使用
PaymentFactory factory = new AlipayFactory();
Payment payment = factory.createPayment(PaymentType.ALIPAY);
```

**使用场景：**
- 支付方式选择
- 数据库连接创建
- 序列化器选择
- 框架集成

---

### 1.3 建造者模式 (Builder)

**意图：** 分步骤构建复杂对象。

```java
// Builder 类
public class UserBuilder {
    private String name;
    private String email;
    private int age;

    public UserBuilder name(String name) {
        this.name = name;
        return this;
    }

    public UserBuilder email(String email) {
        this.email = email;
        return this;
    }

    public UserBuilder age(int age) {
        this.age = age;
        return this;
    }

    public User build() {
        return new User(this);
    }
}

// 使用
User user = new UserBuilder()
    .name("张三")
    .email("zhangsan@example.com")
    .age(25)
    .build();
```

**使用场景：**
- 复杂对象构建
- 可选参数对象
- 不可变对象

---

## 2. 结构型模式

### 2.1 适配器模式 (Adapter)

**意图：** 将一个类的接口转换成客户期望的另一个接口。

```java
// 目标接口
public interface PaymentService {
    void pay(BigDecimal amount);
}

// 被适配者
public class ThirdPartyPayment {
    public void doPayment(BigDecimal amount) {
        // 第三方支付逻辑
    }
}

// 适配器
public class PaymentAdapter implements PaymentService {
    private ThirdPartyPayment payment;

    public PaymentAdapter(ThirdPartyPayment payment) {
        this.payment = payment;
    }

    @Override
    public void pay(BigDecimal amount) {
        payment.doPayment(amount);
    }
}
```

**使用场景：**
- 第三方接口适配
- 遗留系统适配
- 不同协议转换

---

### 2.2 装饰器模式 (Decorator)

**意图：** 动态地给对象添加功能。

```java
// 组件接口
public interface DataSource {
    void write(String data);
}

// 基础组件
public class FileDataSource implements DataSource {
    @Override
    public void write(String data) {
        // 写入文件
    }
}

// 装饰器
public class CompressionDecorator implements DataSource {
    private DataSource wrapped;

    public CompressionDecorator(DataSource wrapped) {
        this.wrapped = wrapped;
    }

    @Override
    public void write(String data) {
        // 先压缩再写入
        String compressed = compress(data);
        wrapped.write(compressed);
    }
}

// 使用
DataSource source = new FileDataSource();
source = new CompressionDecorator(source);
source.write("data");
```

**使用场景：**
- 数据压缩/加密
- 日志装饰
| 缓存装饰
- 性能监控

---

### 2.3 代理模式 (Proxy)

**意图：** 为其他对象提供一种代理以控制对这个对象的访问。

```java
// 接口
public interface UserService {
    User getUser(Long id);
}

// 真实对象
public class UserServiceImpl implements UserService {
    @Override
    public User getUser(Long id) {
        return userRepository.findById(id);
    }
}

// 代理对象
public class CachedUserService implements UserService {
    private UserServiceImpl realService;
    private Map<Long, User> cache = new ConcurrentHashMap<>();

    @Override
    public User getUser(Long id) {
        return cache.computeIfAbsent(id,
            key -> realService.getUser(id));
    }
}
```

**使用场景：**
- 远程代理
- 虚拟代理
- 保护代理
- 缓存代理

---

## 3. 行为型模式

### 3.1 策略模式 (Strategy)

**意图：** 定义一系列算法，把它们封装起来，并使它们可以互相替换。

```java
// 策略接口
public interface PaymentStrategy {
    void pay(BigDecimal amount);
}

// 具体策略
public class AlipayStrategy implements PaymentStrategy {
    @Override
    public void pay(BigDecimal amount) {
        // 支付宝支付逻辑
    }
}

public class WechatPayStrategy implements PaymentStrategy {
    @Override
    public void pay(BigDecimal amount) {
        // 微信支付逻辑
    }
}

// 上下文
public class PaymentContext {
    private PaymentStrategy strategy;

    public void setStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void executePayment(BigDecimal amount) {
        strategy.pay(amount);
    }
}
```

**使用场景：**
- 支付方式选择
- 算法选择（排序、搜索）
- 验证策略
- 序列化方式

---

### 3.2 观察者模式 (Observer)

**意图：** 定义对象间的一对多依赖关系，当一个对象状态改变时，所有依赖者都会收到通知。

```java
// Subject
public interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}

// Observer
public interface Observer {
    void update(String message);
}

// 具体实现
public class OrderService implements Subject {
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void notifyObservers() {
        observers.forEach(o -> o.update("Order created"));
    }
}

// 观察者
public class NotificationService implements Observer {
    @Override
    public void update(String message) {
        sendNotification(message);
    }
}
```

**使用场景：**
- 事件驱动架构
- 消息队列消费者
- 监听器模式
- 发布-订阅

---

### 3.3 责任链模式 (Chain of Responsibility)

**意图：** 为请求创建一个接收者对象链，每个接收者都包含对下一个接收者的引用。

```java
// 处理器
public abstract class ApprovalHandler {
    protected ApprovalHandler next;

    public abstract boolean handle(ApprovalRequest request);

    public ApprovalHandler linkWith(ApprovalHandler next) {
        this.next = next;
        return next;
    }

    public ApprovalHandler handle(ApprovalRequest request) {
        if (handle(request) || next == null) {
            return this;
        }
        return next.handle(request);
    }
}

// 具体处理器
public class ManagerHandler extends ApprovalHandler {
    @Override
    public boolean handle(ApprovalRequest request) {
        if (request.getAmount() < 1000) {
            return true;
        }
        return false;
    }
}

public class DirectorHandler extends ApprovalHandler {
    @Override
    public boolean handle(ApprovalRequest request) {
        if (request.getAmount() < 10000) {
            return true;
        }
        return false;
    }
}
```

**使用场景：**
- 审批流程
- 异常处理链
- 拦截器/过滤器
- 日志处理链

---

### 3.4 模板方法模式 (Template Method)

**意图：** 定义算法骨架，将某些步骤延迟到子类。

```java
public abstract class DataExporter {

    // 模板方法
    public final void export(String data) {
        connect();
        processData(data);
        saveToFile(data);
        disconnect();
    }

    protected abstract void connect();
    protected abstract void saveToFile(String data);

    private void processData(String data) {
        // 可选的通用处理
    }

    private void disconnect() {
        // 通用断开逻辑
    }
}

// 具体实现
public class DatabaseExporter extends DataExporter {
    @Override
    protected void connect() {
        // 数据库连接逻辑
    }

    @Override
    protected void saveToFile(String data) {
        // 保存到文件
    }
}
```

**使用场景：**
- 算法骨架
- 数据导出
- 报表生成
- 数据处理流程

---

## 4. 企业级模式

### 4.1 Repository 模式

**意图：** 封装数据访问逻辑，提供类似集合的接口。

```java
public interface UserRepository extends JpaRepository<User, Long> {

    // Spring Data JPA 提供基础 CRUD
    List<User> findByRole(String role);

    // 自定义查询方法
    @Query("SELECT u FROM User u WHERE u.email = :email")
    User findByEmail(@Param("email") String email);

    @Modifying
    @Query("UPDATE User u SET u.lastLogin = :now WHERE u.id = :id")
    int updateLastLogin(@Param("id") Long id, @Param("now") LocalDateTime now);
}

// 使用
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public User getUserByEmail(String email) {
        return userRepository.findByEmail(email)
            .orElseThrow(() -> new UserNotFoundException(email));
    }
}
```

**使用场景：**
- 数据访问层
- 领域驱动设计
- 测试替身

---

### 4.2 Service Layer 模式

**意图：** 封装业务逻辑，协调多个 Repository 和领域服务。

```java
@Service
@Transactional
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private PaymentService paymentService;

    @Autowired
    private InventoryService inventoryService;

    public Order createOrder(OrderRequest request) {
        // 业务逻辑
        Order order = new Order(request);

        // 库存检查
        inventoryService.checkInventory(order.getItems());

        // 订单创建
        order = orderRepository.save(order);

        // 扣减库存
        inventoryService.deductInventory(order.getItems());

        // 发起支付
        paymentService.charge(order);

        return order;
    }
}
```

**使用场景：**
- 业务逻辑封装
- 事务边界
- 跨 Repository 协调

---

### 4.3 Unit of Work 模式

**意图：** 管理业务事务，确保多个 Repository 操作在同一事务中。

```java
@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private InventoryRepository inventoryRepository;

    @Transactional
    public void createOrderWithItems(Order order, List<OrderItem> items) {
        // 所有操作在同一事务中
        orderRepository.save(order);

        items.forEach(item -> {
            inventoryRepository.save(item);
        });

        // 如果任何操作失败，全部回滚
    }
}
```

**使用场景：**
- 事务管理
- 数据一致性
- 跨 Repository 操作

---

### 4.4 Dependency Injection 模式

**意图：** 实现控制反转（IoC），解耦组件依赖关系。

```java
// 构造器注入
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// Setter 注入
@Service
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

// 字段注入
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

**使用场景：**
- 依赖管理
- 测试替身
- 配置外化

---

## 5. Java Spring 实践

### 5.1 单例 Bean

```java
@Configuration
public class AppConfig {

    @Bean
    @Singleton
    public ObjectMapper objectMapper() {
        return new ObjectMapper()
            .registerModule(new JavaTimeModule())
            .setSerializationInclusion(JsonInclude.Include.NON_NULL);
    }
}
```

### 5.2 工厂 Bean

```java
@Configuration
public class PaymentConfig {

    @Bean
    public PaymentFactory alipayFactory() {
        return new AlipayFactory();
    }

    @Bean
    @ConditionalOnProperty(name="payment.type", havingValue="alipay")
    public PaymentService alipayService() {
        return new AlipayService();
    }
}
```

### 5.3 策略 Bean

```java
@Configuration
public class StrategyConfig {

    @Bean
    @ConditionalOnProperty(name="payment.type")
    public PaymentStrategy paymentStrategy() {
        String type = environment.getProperty("payment.type");
        return switch (type) {
            case "alipay" -> new AlipayStrategy();
            case "wechat" -> new WechatPayStrategy();
            default -> throw new IllegalArgumentException("Unknown payment type");
        };
    }
}
```

### 5.4 代理 Bean

```java
@Aspect
@Component
public class LoggingProxy implements InvocationHandler {

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log.info("调用方法：{}", method.getName());

        Object result = method.invoke(target, args);

        log.info("方法返回：{}", result);
        return result;
    }
}

@Configuration
public class ProxyConfig {

    @Bean
    @Primary
    public UserService userServiceProxy(UserService realService) {
        return (UserService) Proxy.newProxyInstance(
            realService.getClass().getClassLoader(),
            realService.getClass().getInterfaces(),
            new LoggingProxy(realService)
        );
    }
}
```

---

## 速查表

### 创建型模式
```java
// 单例
EnumSingleton.INSTANCE

// 工厂
PaymentFactory factory = new AlipayFactory();
Payment payment = factory.createPayment();

// 建造者
User user = new UserBuilder().name("张三").build();
```

### 结构型模式
```java
// 适配器
PaymentAdapter implements PaymentService

// 装饰器
DataSource source = new CompressionDecorator(new FileDataSource());

// 代理
UserService proxy = new CachedUserService(new UserServiceImpl());
```

### 行为型模式
```java
// 策略
context.setStrategy(new AlipayStrategy());

// 观察者
subject.attach(observer);
subject.notifyObservers();

// 责任链
handler1.linkWith(handler2);
handler1.handle(request);

// 模板方法
exporter.export(data);
```

---

## 相关文档

- [Java 微服务架构](../../playbooks/microservices/java-microservices-architecture.md)
- [后端架构最佳实践](../backend/architecture.md)
