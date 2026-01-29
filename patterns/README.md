# 设计模式总览

> 创建可维护、可扩展的软件系统

---

## 目录

- [后端设计模式](../backend/design-patterns.md)
- [前端设计模式](../frontend/design-patterns.md)
- [分布式系统设计模式](../distributed/design-patterns.md)
- [AI 设计模式](../ai/design-patterns.md)
- [数据库设计模式](../database/design-patterns.md)

---

## 设计模式分类

### GoF 设计模式

**创建型模式**
- 单例模式 (Singleton)
- 工厂模式 (Factory)
- 建造者模式 (Builder)
- 原型模式 (Prototype)

**结构型模式**
- 适配器模式 (Adapter)
- 装饰器模式 (Decorator)
- 代理模式 (Proxy)
- 外观模式 (Facade)
- 桥接模式 (Bridge)
- 组合模式 (Composite)

**行为型模式**
- 策略模式 (Strategy)
- 观察者模式 (Observer)
- 责任链模式 (Chain of Responsibility)
- 命令模式 (Command)
- 状态模式 (State)
- 模板方法模式 (Template Method)
- 迭代器模式 (Iterator)
- 中介者模式 (Mediator)
- 备忘录模式 (Memento)
- 解释器模式 (Interpreter)
- 访问者模式 (Visitor)

### 领域特定模式

**后端模式**
- Repository 模式
- Service Layer 模式
- Unit of Work 模式
- Dependency Injection 模式

**前端模式**
- Container/Presenter 模式
- HOC 模式 (React)
- Hooks 模式
- Provider 模式
- Render Props 模式
- Compound Components 模式

**分布式系统模式**
- Outbox 模式
- Saga 模式
- CQRS 模式
- Event Sourcing 模式
- Circuit Breaker 模式
- Eventual Consistency 模式
- Sharding 模式
- Leader Election 模式

**AI 模式**
- RAG (Retrieval-Augmented Generation) 模式
- Agent 模式
- Chain-of-Thought 模式
- Few-Shot Learning 模式
- Vector Database 模式
- Embedding 模式
- Prompt Engineering 模式
- Tool Use / Function Calling 模式
- Multi-Agent 协作模式

**数据库模式**
- Active Record 模式
- Data Mapper 模式
- Identity Map 模式
- Unit of Work 模式
- Query Object 模式
- Repository 模式

---

## 设计模式选择指南

### 后端开发

| 场景 | 推荐模式 | 说明 |
|------|---------|------|
| 日志记录 | Singleton | 全局唯一的日志器 |
| 复杂对象创建 | Builder | 分步骤构建 |
| 跨服务调用 | Circuit Breaker | 防止级联失败 |
| 数据访问 | Repository | 封装数据访问逻辑 |
| 依赖管理 | Dependency Injection | 解耦和测试 |
| 消息处理 | Observer | 事件驱动架构 |

### 前端开发

| 场景 | 推荐模式 | 说明 |
|------|---------|------|
| 状态管理 | Observer/Provider | 响应式数据流 |
| 组件复用 | Render Props | 通过 props 传递 |
| 逻辑复用 | Hooks | 自定义 hooks |
| 条件渲染 | Strategy | 不同策略渲染 |
| 复杂表单 | Builder | 分步构建表单 |

### 分布式系统

| 场景 | 推荐模式 | 说明 |
|------|---------|------|
| DB 与消息一致性 | Outbox | 保证最终一致性 |
| 跨服务事务 | Saga | 补偿事务 |
| 读写分离 | CQRS | 优化性能 |
| 事件存储 | Event Sourcing | 完整事件历史 |
| 限流降级 | Circuit Breaker | 保护系统 |
| 服务发现 | Registry | 动态服务发现 |

### AI 开发

| 场景 | 推荐模式 | 说明 |
|------|---------|------|
| 知识库问答 | RAG | 检索增强生成 |
| 多步骤任务 | Chain-of-Thought | 分步推理 |
| 工具调用 | Tool Use/Function Calling | 结构化输出 |
| 多Agent协作 | Multi-Agent | 分工协作 |
| 提示词优化 | Few-Shot | 示例学习 |

---

## 设计模式原则

### SOLID 原则

- **S**ingle Responsibility - 单一职责
- **O**pen/Closed - 开闭原则
- **L**iskov Substitution - 里氏替换
- **I**nterface Segregation - 接口隔离
- **D**ependency Inversion - 依赖倒置

### DRY 原则

Don't Repeat Yourself - 避免重复代码

### KISS 原则

Keep It Simple, Stupid - 保持简单

### YAGNI 原则

You Aren't Gonna Need It - 不做不需要的事

---

## 相关文档

- [Java 微服务架构](../../playbooks/microservices/java-microservices-architecture.md)
- [React 项目指南](../../patterns/frontend/react-project-setup-guide.md)
- [Flutter 项目指南](../../patterns/frontend/flutter-project-setup-guide.md)
