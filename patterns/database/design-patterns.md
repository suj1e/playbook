# 数据库设计模式

> 构建高性能、可扩展的数据访问层

---

## 目录

- [1. Active Record 模式](#1-active-record-模式)
- [2. Data Mapper 模式](#2-data-mapper-模式)
- [3. Identity Map 模式](#3-identity-map-模式)
- [4. Unit of Work 模式](#4-unit-of-work-模式)
- [5. Query Object 模式](#5-query-object-模式)
- [6. Repository 模式](#6-repository-模式)
- [7. Metadata Mapping 模式](#7-metadata-mapping-模式)
- [8. Inheritance Mapping 模式](#8-inheritance-mapping-模式)
- [9. Value Object 模式](#9-value-object-模式)
- [10. Lazy Loading 模式](#10-lazy-loading-模式)

---

## 1. Active Record 模式

### 1.1 意图

**问题：** 如何简化对象与数据库表的映射？

**解决方案：** Active Record 模式将数据访问逻辑直接嵌入到领域对象中。

### 1.2 实现

```java
// ActiveRecord 风格
@Entity
@Table(name = "users")
public class User extends ActiveRecord<User> {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;

    // 保存
    public User save() {
        return userRepository.save(this);
    }

    // 删除
    public void delete() {
        userRepository.deleteById(this.id);
    }

    // 查找
    public static User findById(Long id) {
        return userRepository.findById(id).orElse(null);
    }

    public static List<User> findByEmail(String email) {
        return userRepository.findByEmail(email);
    }
}

// 使用
User user = new User();
user.setName("张三");
user.setEmail("zhangsan@example.com");
user.save();  // 保存到数据库
```

### 1.3 优缺点

**优点：**
- 简单直观
- 减少样板代码
- 适合简单 CRUD

**缺点：**
- 领域模型与数据访问耦合
- 难以应对复杂业务逻辑
- 测试困难

### 1.4 适用场景

- 简单的 CRUD 应用
- 快速原型开发
- 数据模型直接对应业务对象

---

## 2. Data Mapper 模式

### 2.1 意图

**问题：** 如何分离领域对象和数据访问逻辑？

**解决方案：** Data Mapper 在领域对象和数据库之间建立映射层，保持对象纯净。

### 2.2 架构

```
领域对象 (Domain Object)
  ↓
Data Mapper
  ↓
数据库 (Database)
```

### 2.3 实现

```java
// 领域对象（纯净，无数据库依赖）
public class User {
    private Long id;
    private String name;
    private String email;

    // 纯业务逻辑方法
    public boolean isValidEmail() {
        return email != null && email.contains("@");
    }
}

// Data Mapper（处理数据映射）
public class UserMapper {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    public User findById(Long id) {
        String sql = "SELECT id, name, email FROM users WHERE id = ?";
        return jdbcTemplate.queryForObject(sql, new UserRowMapper(), id);
    }

    public void insert(User user) {
        String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
        jdbcTemplate.update(sql, user.getName(), user.getEmail());
    }

    public void update(User user) {
        String sql = "UPDATE users SET name = ?, email = ? WHERE id = ?";
        jdbcTemplate.update(sql, user.getName(), user.getEmail(), user.getId());
    }

    // RowMapper
    private static class UserRowMapper implements RowMapper<User> {
        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            User user = new User();
            user.setId(rs.getLong("id"));
            user.setName(rs.getString("name"));
            user.setEmail(rs.getString("email"));
            return user;
        }
    }
}

// 使用
User user = new User();
user.setName("张三");
user.setEmail("zhangsan@example.com");

userMapper.insert(user);  // 通过 Mapper 保存
```

### 2.4 优缺点

**优点：**
- 领域对象纯净
- 关注点分离
- 易于测试

**缺点：**
- 需要更多代码
- 学习曲线陡峭
- 性能开销

---

## 3. Identity Map 模式

### 3.1 意图

**问题：** 同一数据在内存中被多次加载，导致数据不一致和性能浪费。

**解决方案：** Identity Map 确保每个对象只被加载一次，并在内存中缓存。

### 3.2 实现

```java
// Identity Map
public class IdentityMap<T> {

    private Map<Long, T> map = new ConcurrentHashMap<>();

    public void add(Long id, T entity) {
        map.put(id, entity);
    }

    public T get(Long id) {
        return map.get(id);
    }

    public boolean contains(Long id) {
        return map.containsKey(id);
    }

    public void remove(Long id) {
        map.remove(id);
    }

    public void clear() {
        map.clear();
    }
}

// Repository 使用 Identity Map
@Repository
public class UserRepository {

    private IdentityMap<User> identityMap = new IdentityMap<>();

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Transactional
    public User findById(Long id) {
        // 先从 Identity Map 查找
        if (identityMap.contains(id)) {
            return identityMap.get(id);
        }

        // 从数据库加载
        String sql = "SELECT id, name, email FROM users WHERE id = ?";
        User user = jdbcTemplate.queryForObject(sql, new UserRowMapper(), id);

        // 放入 Identity Map
        identityMap.add(id, user);

        return user;
    }

    @Transactional
    public void save(User user) {
        if (user.getId() == null) {
            // 插入
            String sql = "INSERT INTO users (name, email) VALUES (?, ?) RETURNING id";
            Long id = jdbcTemplate.queryForObject(sql, Long.class,
                user.getName(), user.getEmail());
            user.setId(id);
        } else {
            // 更新
            String sql = "UPDATE users SET name = ?, email = ? WHERE id = ?";
            jdbcTemplate.update(sql, user.getName(), user.getEmail(), user.getId());
        }

        // 更新 Identity Map
        identityMap.add(user.getId(), user);
    }

    @Transactional
    public void delete(Long id) {
        jdbcTemplate.update("DELETE FROM users WHERE id = ?", id);
        identityMap.remove(id);
    }
}
```

### 3.3 Hibernate 一级缓存

Hibernate 内置了 Identity Map（一级缓存）：

```java
// Session 级别的 Identity Map
Session session = sessionFactory.openSession();

User user1 = session.get(User.class, 1L);  // 从数据库加载
User user2 = session.get(User.class, 1L);  // 从缓存加载

// user1 == user2 (同一对象)
```

---

## 4. Unit of Work 模式

### 4.1 意图

**问题：** 如何管理事务边界，确保多个数据库操作在同一事务中？

**解决方案：** Unit of Work 跟踪对象变化，在事务提交时一次性持久化所有变化。

### 4.2 架构

```
开始事务
  ↓
操作对象（增删改）
  ↓
Unit of Work 跟踪变化
  ↓
提交事务
  ↓
一次性持久化所有变化
```

### 4.3 实现

```java
// Unit of Work 接口
public interface UnitOfWork {
    void registerNew(Entity entity);
    void registerDirty(Entity entity);
    void registerDeleted(Entity entity);
    void commit();
    void rollback();
}

// Unit of Work 实现
@Component
public class UnitOfWorkImpl implements UnitOfWork {

    @Autowired
    private EntityManager entityManager;

    private Set<Entity> newEntities = new HashSet<>();
    private Set<Entity> dirtyEntities = new HashSet<>();
    private Set<Entity> deletedEntities = new HashSet<>();

    @Override
    @Transactional
    public void registerNew(Entity entity) {
        newEntities.add(entity);
    }

    @Override
    @Transactional
    public void registerDirty(Entity entity) {
        dirtyEntities.add(entity);
    }

    @Override
    @Transactional
    public void registerDeleted(Entity entity) {
        deletedEntities.add(entity);
    }

    @Override
    @Transactional
    public void commit() {
        try {
            // 插入新对象
            newEntities.forEach(entityManager::persist);

            // 更新脏对象
            dirtyEntities.forEach(entityManager::merge);

            // 删除对象
            deletedEntities.forEach(entityManager::remove);

            // 提交事务
            entityManager.flush();

            // 清空跟踪
            clear();

        } catch (Exception e) {
            rollback();
            throw new RuntimeException("事务提交失败", e);
        }
    }

    @Override
    public void rollback() {
        clear();
    }

    private void clear() {
        newEntities.clear();
        dirtyEntities.clear();
        deletedEntities.clear();
    }
}

// 使用
@Service
public class OrderService {

    @Autowired
    private UnitOfWork unitOfWork;

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private PaymentRepository paymentRepository;

    @Autowired
    private InventoryRepository inventoryRepository;

    public void createOrder(Order order, Payment payment, Inventory inventory) {
        // 注册变化
        unitOfWork.registerNew(order);
        unitOfWork.registerNew(payment);
        unitOfWork.registerDirty(inventory);

        // 一次性提交
        unitOfWork.commit();
    }
}
```

### 4.4 Hibernate Unit of Work

Hibernate Session 本身就是 Unit of Work：

```java
Session session = sessionFactory.openSession();
Transaction tx = session.beginTransaction();

try {
    // 操作对象
    User user = new User();
    user.setName("张三");
    session.save(user);  // registerNew

    Order order = session.get(Order.class, 1L);
    order.setStatus("COMPLETED");  // registerDirty

    session.delete(session.get(Product.class, 1L));  // registerDeleted

    // 提交事务（一次性持久化所有变化）
    tx.commit();

} catch (Exception e) {
    tx.rollback();
}
```

---

## 5. Query Object 模式

### 5.1 意图

**问题：** 如何将数据库查询逻辑封装成可复用对象？

**解决方案：** Query Object 将查询条件封装成对象，支持动态构建查询。

### 5.2 实现

```java
// Criteria API（Hibernate Query Object）
@Repository
public class UserRepository {

    @Autowired
    private EntityManager entityManager;

    public List<User> findByCriteria(String name, String email, Integer minAge) {
        CriteriaBuilder cb = entityManager.getCriteriaBuilder();
        CriteriaQuery<User> query = cb.createQuery(User.class);
        Root<User> root = query.from(User.class);

        List<Predicate> predicates = new ArrayList<>();

        if (name != null) {
            predicates.add(cb.like(root.get("name"), "%" + name + "%"));
        }

        if (email != null) {
            predicates.add(cb.equal(root.get("email"), email));
        }

        if (minAge != null) {
            predicates.add(cb.greaterThanOrEqualTo(root.get("age"), minAge));
        }

        query.where(predicates.toArray(new Predicate[0]));

        return entityManager.createQuery(query).getResultList();
    }
}

// Query Specification（Spring Data JPA）
public interface UserSpecification {

    static Specification<User> hasName(String name) {
        return (root, query, cb) ->
            name == null ? null : cb.like(root.get("name"), "%" + name + "%");
    }

    static Specification<User> hasEmail(String email) {
        return (root, query, cb) ->
            email == null ? null : cb.equal(root.get("email"), email);
    }

    static Specification<User> hasMinAge(Integer minAge) {
        return (root, query, cb) ->
            minAge == null ? null : cb.greaterThanOrEqualTo(root.get("age"), minAge);
    }
}

// 使用
List<User> users = userRepository.findAll(
    Specification.where(UserSpecification.hasName("张三"))
        .and(UserSpecification.hasMinAge(18))
);
```

### 5.3 Query By Example (QBE)

```java
// Spring Data JPA Query By Example
@Repository
public interface UserRepository extends JpaRepository<User, Long>, QueryByExampleExecutor<User> {
}

// 使用
User probe = new User();
probe.setName("张三");
probe.setEmail("zhangsan@example.com");

Example<User> example = Example.of(probe,
    ExampleMatcher.matching()
        .withIgnoreCase()
        .withStringMatcher(ExampleMatcher.StringMatcher.CONTAINING));

List<User> users = userRepository.findAll(example);
```

---

## 6. Repository 模式

### 6.1 意图

**问题：** 如何封装数据访问逻辑，提供类似集合的接口？

**解决方案：** Repository 模式将数据访问封装在领域层和数据映射层之间。

### 6.2 架构

```
领域层 (Domain Layer)
  ↓
Repository 接口
  ↓
Repository 实现
  ↓
数据映射层 (Data Mapper)
  ↓
数据库 (Database)
```

### 6.3 实现

```java
// Repository 接口
public interface UserRepository {

    User findById(Long id);

    List<User> findAll();

    User save(User user);

    void deleteById(Long id);

    List<User> findByEmail(String email);

    List<User> findByNameLike(String name);
}

// Repository 实现
@Repository
public class UserRepositoryImpl implements UserRepository {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public User findById(Long id) {
        String sql = "SELECT id, name, email FROM users WHERE id = ?";
        return jdbcTemplate.queryForObject(sql, new UserRowMapper(), id);
    }

    @Override
    public List<User> findAll() {
        String sql = "SELECT id, name, email FROM users";
        return jdbcTemplate.query(sql, new UserRowMapper());
    }

    @Override
    @Transactional
    public User save(User user) {
        if (user.getId() == null) {
            String sql = "INSERT INTO users (name, email) VALUES (?, ?) RETURNING id";
            Long id = jdbcTemplate.queryForObject(sql, Long.class,
                user.getName(), user.getEmail());
            user.setId(id);
        } else {
            String sql = "UPDATE users SET name = ?, email = ? WHERE id = ?";
            jdbcTemplate.update(sql, user.getName(), user.getEmail(), user.getId());
        }
        return user;
    }

    @Override
    @Transactional
    public void deleteById(Long id) {
        jdbcTemplate.update("DELETE FROM users WHERE id = ?", id);
    }

    @Override
    public List<User> findByEmail(String email) {
        String sql = "SELECT id, name, email FROM users WHERE email = ?";
        return jdbcTemplate.query(sql, new UserRowMapper(), email);
    }

    @Override
    public List<User> findByNameLike(String name) {
        String sql = "SELECT id, name, email FROM users WHERE name LIKE ?";
        return jdbcTemplate.query(sql, new UserRowMapper(), "%" + name + "%");
    }
}
```

### 6.4 Spring Data JPA Repository

```java
// Spring Data JPA 自动实现
public interface UserRepository extends JpaRepository<User, Long> {

    // 自动生成查询
    Optional<User> findByEmail(String email);

    List<User> findByNameContainingIgnoreCase(String name);

    // 自定义查询
    @Query("SELECT u FROM User u WHERE u.email = :email")
    Optional<User> findByEmailCustom(@Param("email") String email);

    // 原生 SQL
    @Query(value = "SELECT * FROM users WHERE email = :email", nativeQuery = true)
    Optional<User> findByEmailNative(@Param("email") String email);

    // 批量更新
    @Modifying
    @Query("UPDATE User u SET u.lastLogin = :now WHERE u.id = :id")
    int updateLastLogin(@Param("id") Long id, @Param("now") LocalDateTime now);
}
```

---

## 7. Metadata Mapping 模式

### 7.1 意图

**问题：** 如何减少对象-关系映射的重复代码？

**解决方案：** Metadata Mapping 通过元数据（注解、XML）定义映射关系。

### 7.2 注解映射

```java
// JPA 注解映射
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;

    @Column(name = "name", nullable = false, length = 100)
    private String name;

    @Column(name = "email", unique = true, nullable = false)
    private String email;

    @Column(name = "age")
    private Integer age;

    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "created_at")
    private Date createdAt;

    @Version
    @Column(name = "version")
    private Integer version;  // 乐观锁
}
```

### 7.3 XML 映射（MyBatis）

```xml
<!-- UserMapper.xml -->
<mapper namespace="com.example.mapper.UserMapper">

    <resultMap id="UserResultMap" type="User">
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="email" column="email"/>
        <result property="age" column="age"/>
    </resultMap>

    <select id="findById" resultMap="UserResultMap">
        SELECT id, name, email, age
        FROM users
        WHERE id = #{id}
    </select>

    <insert id="insert" parameterType="User">
        INSERT INTO users (name, email, age)
        VALUES (#{name}, #{email}, #{age})
    </insert>

    <update id="update" parameterType="User">
        UPDATE users
        SET name = #{name}, email = #{email}, age = #{age}
        WHERE id = #{id}
    </update>

    <delete id="deleteById">
        DELETE FROM users WHERE id = #{id}
    </delete>
</mapper>
```

---

## 8. Inheritance Mapping 模式

### 8.1 意图

**问题：** 如何将继承关系映射到关系数据库？

**解决方案：** 三种继承映射策略。

### 8.2 Single Table Inheritance

```java
// 单表继承
@Entity
@Table(name = "people")
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "person_type", discriminatorType = DiscriminatorType.STRING)
public abstract class Person {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;
}

@Entity
@DiscriminatorValue("EMPLOYEE")
public class Employee extends Person {

    private String department;

    private BigDecimal salary;
}

@Entity
@DiscriminatorValue("CUSTOMER")
public class Customer extends Person {

    private String company;

    private String address;
}

// 表结构
// CREATE TABLE people (
//   id BIGINT PRIMARY KEY,
//   person_type VARCHAR(31),  -- EMPLOYEE / CUSTOMER
//   name VARCHAR(100),
//   email VARCHAR(100),
//   department VARCHAR(50),   -- 仅 EMPLOYEE
//   salary DECIMAL(10,2),     -- 仅 EMPLOYEE
//   company VARCHAR(100),     -- 仅 CUSTOMER
//   address VARCHAR(255)      -- 仅 CUSTOMER
// );
```

### 8.3 Class Table Inheritance

```java
// 类表继承
@Entity
@Table(name = "people")
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Person {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;
}

@Entity
@Table(name = "employees")
@PrimaryKeyJoinColumn(name = "person_id")
public class Employee extends Person {

    private String department;

    private BigDecimal salary;
}

@Entity
@Table(name = "customers")
@PrimaryKeyJoinColumn(name = "person_id")
public class Customer extends Person {

    private String company;

    private String address;
}

// 表结构
// CREATE TABLE people (id BIGINT PRIMARY KEY, name VARCHAR(100), email VARCHAR(100));
// CREATE TABLE employees (person_id BIGINT PRIMARY KEY REFERENCES people(id), department VARCHAR(50), salary DECIMAL(10,2));
// CREATE TABLE customers (person_id BIGINT PRIMARY KEY REFERENCES people(id), company VARCHAR(100), address VARCHAR(255));
```

### 8.4 Concrete Table Inheritance

```java
// 具体表继承
@Entity
@Table(name = "employees")
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public class Employee extends Person {

    private String department;

    private BigDecimal salary;
}

@Entity
@Table(name = "customers")
public class Customer extends Person {

    private String company;

    private String address;
}

// 表结构
// CREATE TABLE employees (id BIGINT PRIMARY KEY, name VARCHAR(100), email VARCHAR(100), department VARCHAR(50), salary DECIMAL(10,2));
// CREATE TABLE customers (id BIGINT PRIMARY KEY, name VARCHAR(100), email VARCHAR(100), company VARCHAR(100), address VARCHAR(255));
```

### 8.5 策略对比

| 策略 | 优点 | 缺点 |
|------|------|------|
| Single Table | 查询快、简单 | 字段冗余、NOT NULL 约束困难 |
| Class Table | 规范化、支持约束 | 查询需要 JOIN |
| Concrete Table | 无冗余 | 查询基类需要 UNION |

---

## 9. Value Object 模式

### 9.1 意图

**问题：** 如何表示没有唯一标识的领域概念（如金额、地址）？

**解决方案：** Value Object 通过值相等性判断对象相等，不可变。

### 9.2 实现

```java
// Value Object（不可变、值相等）
@Embeddable
public class Money {

    @Column(name = "amount")
    private BigDecimal amount;

    @Column(name = "currency")
    private String currency;

    // 私有构造器
    private Money() {}

    public Money(BigDecimal amount, String currency) {
        this.amount = amount;
        this.currency = currency;
    }

    // 值相等
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Money money = (Money) o;
        return Objects.equals(amount, money.amount) &&
               Objects.equals(currency, money.currency);
    }

    @Override
    public int hashCode() {
        return Objects.hash(amount, currency);
    }
}

// 使用
@Entity
@Table(name = "orders")
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "amount", column = @Column(name = "total_amount")),
        @AttributeOverride(name = "currency", column = @Column(name = "currency"))
    })
    private Money total;

    // 业务逻辑
    public void addAmount(Money amount) {
        this.total = new Money(
            this.total.amount.add(amount.amount),
            this.total.currency
        );
    }
}

// Address Value Object
@Embeddable
public class Address {

    @Column(name = "street")
    private String street;

    @Column(name = "city")
    private String city;

    @Column(name = "zip_code")
    private String zipCode;

    @Column(name = "country")
    private String country;

    private Address() {}

    public Address(String street, String city, String zipCode, String country) {
        this.street = street;
        this.city = city;
        this.zipCode = zipCode;
        this.country = country;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Address address = (Address) o;
        return Objects.equals(street, address.street) &&
               Objects.equals(city, address.city) &&
               Objects.equals(zipCode, address.zipCode) &&
               Objects.equals(country, address.country);
    }

    @Override
    public int hashCode() {
        return Objects.hash(street, city, zipCode, country);
    }
}
```

---

## 10. Lazy Loading 模式

### 10.1 意图

**问题：** 如何避免加载不需要的数据，提高性能？

**解决方案：** Lazy Loading 延迟加载关联数据，只在需要时加载。

### 10.2 实现

```java
@Entity
@Table(name = "orders")
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String orderNumber;

    // LAZY 加载订单项
    @OneToMany(mappedBy = "order", fetch = FetchType.LAZY)
    private List<OrderItem> items = new ArrayList<>();

    // LAZY 加载客户
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "customer_id")
    private Customer customer;

    // EAGER 加载状态（通常需要）
    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "status_id")
    private OrderStatus status;
}

// 使用
Session session = sessionFactory.openSession();

Order order = session.get(Order.class, 1L);
// 此时只加载 order，不加载 items 和 customer

List<OrderItem> items = order.getItems();
// 此时触发 Lazy Loading，加载 items

session.close();
```

### 10.3 N+1 问题

```java
// N+1 问题示例
List<Order> orders = orderRepository.findAll();
// 1 条 SQL：SELECT * FROM orders

for (Order order : orders) {
    List<OrderItem> items = order.getItems();
    // N 条 SQL：SELECT * FROM order_items WHERE order_id = ?
    // 总共 1 + N 条 SQL
}
```

### 10.4 解决 N+1 问题

```java
// 方案 1：JOIN FETCH
@Query("SELECT o FROM Order o JOIN FETCH o.items WHERE o.id = :id")
Order findByIdWithItems(@Param("id") Long id);

// 方案 2：EntityGraph
@EntityGraph(attributePaths = {"items", "customer"})
Order findById(Long id);

// 方案 3：@BatchSize
@OneToMany(mappedBy = "order", fetch = FetchType.LAZY)
@BatchSize(size = 50)
private List<OrderItem> items;

// 方案 4：JPQL JOIN
List<Order> orders = entityManager.createQuery(
    "SELECT DISTINCT o FROM Order o LEFT JOIN FETCH o.items", Order.class)
    .getResultList();
```

---

## 速查表

### Active Record
```java
@Entity
public class User {
    public User save() { return userRepository.save(this); }
    public void delete() { userRepository.deleteById(this.id); }
    public static User findById(Long id) { return userRepository.findById(id).orElse(null); }
}
```

### Data Mapper
```java
// 领域对象（纯净）
public class User { }

// Mapper（处理数据访问）
public class UserMapper {
    public User findById(Long id) { }
    public void insert(User user) { }
}
```

### Repository
```java
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
```

### Unit of Work
```java
@Transactional
public void createOrder() {
    unitOfWork.registerNew(order);
    unitOfWork.registerNew(payment);
    unitOfWork.commit();  // 一次性提交
}
```

### Lazy Loading
```java
@OneToMany(fetch = FetchType.LAZY)
private List<OrderItem> items;

// 避免 N+1
@Query("SELECT o FROM Order o JOIN FETCH o.items")
Order findByIdWithItems(Long id);
```

---

## 相关文档

- [Java 微服务架构](../../playbooks/microservices/java-microservices-architecture.md)
- [后端设计模式](../backend/design-patterns.md)
- [分布式系统设计模式](../distributed/design-patterns.md)
