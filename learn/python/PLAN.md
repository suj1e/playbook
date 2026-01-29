# Python 学习路线规划

> 现代化的 Python 学习路径与最佳实践

---

## 目录

- [学习目标](#学习目标)
- [前置要求](#前置要求)
- [学习路线总览](#学习路线总览)
- [现代工具栈](#现代工具栈)
- [学习阶段](#学习阶段)
- [方向选择](#方向选择)
- [文档结构](#文档结构)
- [学习资源](#学习资源)
- [时间规划](#时间规划)

---

## 学习目标

### 核心目标

- ✅ 掌握 Python 3.10+ 语法和特性
- ✅ 熟练使用现代包管理工具（uv / pixi）
- ✅ 理解面向对象和函数式编程
- ✅ 掌握常用数据结构与算法
- ✅ 掌握异步编程基础
- ✅ 具备工程化思维（测试、代码质量、CI/CD）
- ✅ 能通过算法面试（LeetCode 中等题）

### 技能树

```
Python 核心技能
├── 语法基础
├── 数据结构与算法 ⭐ 面试必备
│   ├── 数组与字符串（双指针、滑动窗口）
│   ├── 链表操作
│   ├── 栈与队列
│   ├── 哈希表
│   ├── 树与图（遍历、搜索）
│   ├── 动态规划
│   ├── 回溯与贪心
│   └── Python 特有优化技巧
├── OOP 与设计模式
├── 异步编程
├── 装饰器与元编程
└── 测试与调试

工程化技能
├── 包管理（uv / pixi）
├── 虚拟环境
├── 依赖管理
├── 代码规范
├── 单元测试
├── CI/CD
└── 性能优化

方向技能（选一）
├── Web 开发
│   ├── FastAPI
│   ├── SQLAlchemy
│   └── 部署
│
├── AI / LLM
│   ├── OpenAI API
│   ├── LangChain
│   ├── RAG
│   └── Agent
│
├── 数据科学
│   ├── NumPy
│   ├── Pandas
│   └── 可视化
│
└── 自动化
    ├── 脚本开发
    ├── 爬虫
    └── DevOps 工具
```

---

## 前置要求

- 零基础或有一定编程经验
- 了解基本的命令行操作
- 有学习兴趣和耐心
- 准备好实践环境

---

## 学习路线总览

```
┌─────────────────────────────────────────────────────────────────┐
│                    Python 学习路线图                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  第1阶段：环境搭建与基础语法（2-3周）                            │
│  ├─ Python 安装与多版本管理（pyenv）                            │
│  ├─ 现代包管理工具（uv / pixi）⭐ 重点                          │
│  ├─ 虚拟环境与依赖管理                                          │
│  ├─ 语法基础（变量、运算符、控制流）                             │
│  └─ 数据类型（字符串、列表、字典、集合、元组）                    │
│                                                                 │
│  第2阶段：函数与模块化（2周）                                    │
│  ├─ 函数定义与参数                                              │
│  ├─ 作用域与闭包                                                │
│  ├─ 模块与包                                                    │
│  ├─ 异常处理                                                    │
│  └─ 文件操作                                                    │
│                                                                 │
│  第3阶段：数据结构与算法（4-6周）⭐ 面试必备                    │
│  ├─ 基础数据结构（数组、链表、栈、队列、哈希表）                  │
│  ├─ 树与图（二叉树、BST、图遍历、并查集）                        │
│  ├─ 核心算法（双指针、滑动窗口、二分查找、排序）                 │
│  ├─ 高级算法（动态规划、回溯、贪心）                            │
│  ├─ Python 算法库（collections、itertools、heapq）              │
│  └─ LeetCode 实战（对应 ../../leetcode/ 目录）                  │
│                                                                 │
│  第4阶段：面向对象与进阶（3-4周）                                │
│  ├─ 类与对象                                                    │
│  ├─ 继承与多态                                                  │
│  ├─ 魔术方法                                                    │
│  ├─ 装饰器                                                      │
│  ├─ 生成器与迭代器                                              │
│  ├─ 上下文管理器                                                │
│  └─ 异步编程基础                                                │
│                                                                 │
│  第5阶段：方向选择（8-12周，选一深入）                           │
│  ├─ Web 开发方向                                                │
│  ├─ AI / LLM 方向 🔥 热门                                       │
│  ├─ 数据科学方向                                                │
│  └─ 自动化方向                                                  │
│                                                                 │
│  第6阶段：工程化实践（持续）                                     │
│  ├─ 代码规范（Black、isort、ruff）                              │
│  ├─ 类型注解                                                    │
│  ├─ 单元测试（pytest）                                          │
│  ├─ 性能优化                                                    │
│  └─ CI/CD                                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 现代工具栈

### Python 版本

- **推荐：Python 3.11+**（性能最佳）
- **最低：Python 3.10**（LTS 风格，稳定）
- **最新：Python 3.13**（尝鲜）

### 多版本管理

| 工具 | 特点 | 推荐度 |
|------|------|--------|
| **pyenv** | 成熟、插件丰富 | ⭐⭐⭐⭐⭐ |
| **mise** | 跨语言、更快 | ⭐⭐⭐⭐ |

### 包管理工具（核心！）

| 工具 | 语言 | 定位 | 速度 | 适用场景 | 推荐度 |
|------|------|------|------|----------|--------|
| **uv** | Rust | Python 专用 | 🚀🚀🚀🚀🚀 | 纯 Python 项目 | ⭐⭐⭐⭐⭐ |
| **pixi** | Rust | 跨语言项目管理 | 🚀🚀🚀🚀🚀 | 数据科学/ML/跨语言 | ⭐⭐⭐⭐⭐ |
| **Poetry** | Python | 依赖管理 | 🚀🚀 | 传统项目 | ⭐⭐⭐ |
| **PDM** | Python | PEP 支持 | 🚀🚀 | 高级用户 | ⭐⭐⭐ |
| **rye** | Rust | 已被 uv 整合 | 🚀 | 历史工具 | ⭐⭐ |

#### uv vs pixi 选择指南

```
选择 uv，如果你：
✅ 开发纯 Python Web/后端项目
✅ 追求极致速度
✅ 想替代 pip + pip-tools + virtualenv

选择 pixi，如果你：
✅ 做数据科学/机器学习
✅ 需要管理非 Python 依赖（CUDA、系统库）
✅ 需要跨语言协作（Python + R + Node.js）
✅ 想替代 conda
```

### 开发工具

| 类别 | 工具 | 说明 |
|------|------|------|
| **编辑器** | Neovim / VS Code | 参考 tools/neovim |
| **LSP** | python-lsp-server / pyright | 代码补全 |
| **格式化** | Black / ruff format | 代码格式化 |
| **Lint** | ruff / pylint | 代码检查 |
| **类型检查** | mypy / pyright | 静态类型 |
| **测试** | pytest + pytest-cov | 单元测试 |
| **REPL** | ptpython / bpython | 交互式环境 |

---

## 学习阶段

### 第1阶段：环境搭建与基础语法（2-3周）

#### 周1：环境搭建
- [ ] 安装 pyenv
- [ ] 安装 Python 3.11+
- [ ] 学习 uv / pixi 使用
- [ ] 创建第一个项目
- [ ] 理解 pyproject.toml
- [ ] 掌握虚拟环境

#### 周2：语法基础
- [ ] 变量与常量
- [ ] 基本数据类型（int, float, str, bool）
- [ ] 运算符（算术、比较、逻辑）
- [ ] 控制流（if、for、while）
- [ ] 练习：写几个小脚本

#### 周3：数据类型
- [ ] 列表（List）操作
- [ ] 字典（Dict）操作
- [ ] 集合（Set）操作
- [ ] 元组（Tuple）
- [ ] 字符串高级用法
- [ ] 练习：数据统计脚本

**阶段项目：** 命令行待办事项应用

---

### 第2阶段：函数与模块化（2周）

#### 周1：函数
- [ ] 函数定义与调用
- [ ] 参数类型（位置、关键字、默认、可变）
- [ ] 返回值
- [ ] 作用域（LEGB 规则）
- [ ] 闭包
- [ ] lambda 函数

#### 周2：模块化
- [ ] 模块导入
- [ ] 包管理
- [ ] `__init__.py`
- [ ] 相对导入与绝对导入
- [ ] 异常处理（try/except/finally）
- [ ] 文件操作（读、写、路径处理）

**阶段项目：** 日志分析工具

---

### 第3阶段：数据结构与算法（4-6周）⭐ 面试必备

> 本阶段对应 `../../leetcode/` 目录内容，包含 12 个专题、100+ 题目详解

#### 学习资源

| 资源 | 说明 |
|------|------|
| [LeetCode 专题](../../leetcode/) | 本项目刷题手册，Java/Python 双语 |
| [Python 算法库](https://docs.python.org/zh-cn/3/library/) | collections、itertools、heapq |
| [算法可视化](https://visualgo.net/) | 动态演示算法执行过程 |

#### 周1：数组与字符串
- [ ] **双指针技巧**
  - 对撞指针（ lc-167 两数之和II）
  - 快慢指针（ lc-125 验证回文串）
  - Python 特性：逆序切片 `nums[::-1]`
- [ ] **滑动窗口**
  - 固定窗口（ lc-438 找字母异位词）
  - 可变窗口（ lc-209 最小子数组、lc-3 无重复字符最长子串）
  - Python 技巧：`collections.Counter`、`defaultdict`
- [ ] **二分查找**
  - 标准二分（ lc-704）
  - 左/右边界（ lc-35）
  - 旋转数组（ lc-33）
  - Python 模块：`bisect`

#### 周2：链表与哈希表
- [ ] **链表操作**
  - 反转链表（ lc-206）
  - 合并有序链表（ lc-21）
  - 环检测（ lc-141、lc-142）
  - Python 定义：
    ```python
    class ListNode:
        def __init__(self, val=0, next=None):
            self.val = val
            self.next = next
    ```
- [ ] **哈希表应用**
  - 两数之和（ lc-1）
  - 字母异位词分组（ lc-49）
  - 和为K的子数组（ lc-560）
  - Python 技巧：`dict.get()`、`setdefault()`、`collections.Counter`

#### 周3：栈队列与树
- [ ] **单调栈**
  - 每日温度（ lc-739）
  - 下一个更大元素（ lc-496）
  - 柱状图最大矩形（ lc-84）
  - Python：`list` 作为栈（`append`/`pop`）
- [ ] **树的遍历**
  - 前/中/后序（ lc-144/94/145）递归与迭代
  - 层序遍历（ lc-102）BFS
  - Python 定义：
    ```python
    class TreeNode:
        def __init__(self, val=0, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right
    ```

#### 周4：图与堆
- [ ] **图的遍历**
  - 岛屿数量（ lc-200）DFS/BFS
  - 课程表（ lc-207）拓扑排序
  - 网络延迟时间（ lc-743）Dijkstra
- [ ] **堆应用**
  - TopK 问题（ lc-215、lc-347）
  - 合并K个有序链表（ lc-23）
  - Python：`heapq` 模块、`heapify()`、`heappush/pop`

#### 周5-6：动态规划与回溯
- [ ] **DP 入门**
  - 爬楼梯（ lc-70）
  - 最长递增子序列（ lc-300）
  - 最长公共子序列（ lc-1143）
  - Python 技巧：列表推导式初始化 DP 表
- [ ] **回溯算法**
  - 全排列（ lc-46、lc-47）
  - 组合（ lc-77）
  - 子集（ lc-78）
  - N皇后（ lc-51）
  - Python 技巧：`path.copy()` 避免引用问题

#### Python 算法库速查

```python
# collections - 高级容器
from collections import Counter, defaultdict, deque

Counter('hello')              # 计数器
defaultdict(int)              # 默认值字典
deque([1,2,3])                # 双端队列（支持 O(1) 头部操作）

# itertools - 迭代工具
from itertools import combinations, permutations, accumulate

list(combinations([1,2,3], 2))   # 组合 C(3,2)
list(permutations([1,2,3]))      # 排列 3!
list(accumulate([1,2,3,4]))       # 累加 [1,3,6,10]

# heapq - 堆操作
import heapq

heapq.heappush(heap, item)        # 入堆
heapq.heappop(heap)               # 出堆
heapq.heapify(list)               # 原地建堆
heapq.nlargest(k, iterable)       # 最大 k 个
heapq.nsmallest(k, iterable)      # 最小 k 个

# bisect - 二分查找
import bisect

bisect.bisect_left(arr, x)        # 第一个 >= x 的位置
bisect.bisect_right(arr, x)       # 第一个 > x 的位置
bisect.insort(arr, x)             # 插入并保持有序
```

#### LeetCode 刷题策略

| 专题 | LeetCode 目录 | 必刷题目数 |
|------|---------------|-----------|
| 数组 | `01-array/` | 11 题 |
| 字符串 | `02-string/` | 8 题 |
| 链表 | `03-linked-list/` | 8 题 |
| 哈希表 | `04-hash/` | 6 题 |
| 栈队列 | `05-stack-queue/` | 9 题 |
| 树 | `06-tree/` | 11 题 |
| 堆 | `07-heap/` | 5 题 |
| 图 | `08-graph/` | 9 题 |
| 动态规划 | `09-dp/` | 13 题 |
| 回溯 | `10-backtracking/` | 7 题 |
| 贪心 | `11-greedy/` | 5 题 |

**刷题建议：**
1. 按专题顺序刷，不要随机
2. 每题先自己想 10 分钟，再看题解
3. 代码提交后，看讨论区的 Python 优秀解法
4. 总结模板和模式（如滑动窗口模板、快慢指针模板）
5. 一周后重做之前不会的题

**阶段项目：**
- LeetCode Easy 题：每天 2 道，连续 2 周
- LeetCode Medium 题：每天 1 道，连续 4 周
- 完成 3 个专题的全部题目（推荐：数组、链表、树）

---

### 第4阶段：面向对象与进阶（3-4周）

#### 周1：OOP 基础
- [ ] 类与对象
- [ ] 构造方法 `__init__`
- [ ] 实例属性与类属性
- [ ] 方法（实例、类、静态）
- [ ] 继承
- [ ] 多态

#### 周2：OOP 进阶
- [ ] 魔术方法（`__str__`、`__repr__`、`__eq__` 等）
- [ ] 属性装饰器（@property）
- [ ] 抽象基类（ABC）
- [ ] 组合与继承
- [ ] 设计模式入门

#### 周3：高级特性
- [ ] 装饰器（函数装饰器、类装饰器）
- [ ] 生成器（yield）
- [ ] 迭代器（`__iter__`、`__next__`）
- [ ] 上下文管理器（`with` 语句）
- [ ] 列表/字典/集合推导式

#### 周4：异步编程
- [ ] 异步基础（async/await）
- [ ] 事件循环
- [ ] 异步上下文管理器
- [ ] 异步迭代器
- [ ] 常用异步库（aiohttp、asyncpg）

**阶段项目：** 异步爬虫或 API 服务

---

### 第5阶段：方向选择（8-12周，选一）

#### Web 开发方向

```
学习内容：
├── FastAPI
│   ├── 路由与请求处理
│   ├── Pydantic 数据验证
│   ├── 依赖注入
│   ├── 中间件
│   └── WebSocket
│
├── 数据库
│   ├── SQLAlchemy
│   ├── Alembic 迁移
│   └── Redis 缓存
│
└── 部署
    ├── Docker
    ├── Nginx
    └── CI/CD

阶段项目：
├─ RESTful API 服务
├─ 认证授权系统
└─ 完整 Web 应用
```

---

#### AI / LLM 方向 🔥

```
学习内容：
├── API 调用
│   ├── OpenAI SDK
│   ├── Anthropic SDK
│   ├── 流式输出
│   └── 错误处理
│
├── RAG (检索增强生成)
│   ├── 文档加载
│   ├── 文本切片
│   ├── 向量数据库
│   ├── Embedding
│   └── 相似度检索
│
├── Agent 开发
│   ├── LangChain
│   ├── LlamaIndex
│   ├── 工具定义
│   ├── 记忆管理
│   └── 多 Agent 协作
│
└── 本地模型
    ├── Ollama
    ├── vLLM
    └── 模型量化

阶段项目：
├─ AI 聊天机器人
├─ 知识库问答系统
└─ AI Agent 助手
```

---

#### 数据科学方向

```
学习内容：
├── pixi 环境管理
│   ├── 跨语言依赖
│   ├── 环境复现
│   └── 团队协作
│
├── 数据处理
│   ├── NumPy
│   ├── Pandas
│   └── 数据清洗
│
├── 可视化
│   ├── Matplotlib
│   ├── Seaborn
│   └── Plotly
│
└── 机器学习
    ├── Scikit-learn
    ├── 模型训练
    └── 模型评估

阶段项目：
├─ 数据分析报告
├─ 机器学习预测模型
└─ 交互式 Dashboard
```

---

#### 自动化方向

```
学习内容：
├── 脚本开发
│   ├── 文件处理
│   ├── 日志分析
│   └── 定时任务
│
├── 网络操作
│   ├── requests
│   ├── httpx
│   └── aiohttp
│
├── 爬虫
│   ├── BeautifulSoup
│   ├── Playwright
│   └── 反爬虫策略
│
└── DevOps 工具
    ├── Ansible
    ├── API 脚本
    └── 监控脚本

阶段项目：
├─ 自动化运维工具
├─ 数据采集系统
└─ 监控告警平台
```

---

### 第6阶段：工程化实践（持续）

```
代码质量：
├── 代码规范
│   ├── Black（格式化）
│   ├── ruff（Lint + 格式化）
│   └── isort（导入排序）
│
├── 类型注解
│   ├── typing 基础
│   ├── mypy 检查
│   └── 类型存根
│
├── 测试
│   ├── pytest 基础
│   ├── fixture
│   ├── mock
│   ├── 覆盖率
│   └── TDD 实践
│
└── CI/CD
    ├── GitHub Actions
    ├── 自动化测试
    └── 自动化部署

性能优化：
├── 性能分析（cProfile）
├── 内存优化
├── 并发编程
└── Cython / Rust 扩展
```

---

## 文档结构

```
learn/python/
├── PLAN.md                      # 本文档 - 学习路线规划
│
├── setup/                       # 环境搭建（重点！）
│   ├── 01-installation.md       # Python 安装（pyenv）
│   ├── 02-package-managers.md   # 包管理工具深度对比
│   ├── 03-uv-guide.md          # uv 完全指南
│   ├── 04-pixi-guide.md        # pixi 完全指南
│   ├── 05-project-structure.md # 项目结构
│   └── 06-best-practices.md    # 最佳实践
│
├── fundamentals/                # 基础篇
│   ├── 01-basics.md            # 语法基础
│   ├── 02-data-types.md        # 数据类型
│   ├── 03-control-flow.md      # 控制流
│   ├── 04-functions.md         # 函数
│   ├── 05-modules.md           # 模块与包
│   └── 06-exceptions.md        # 异常处理
│
├── algorithms/                  # 算法篇（对应 leetcode/）
│   ├── README.md               # 算法学习总览
│   ├── 01-data-structures.md   # 基础数据结构
│   ├── 02-arrays-strings.md    # 数组与字符串
│   ├── 03-linked-list.md       # 链表
│   ├── 04-hash.md              # 哈希表
│   ├── 05-stack-queue.md       # 栈与队列
│   ├── 06-tree.md              # 树
│   ├── 07-graph.md             # 图
│   ├── 08-dp.md                # 动态规划
│   ├── 09-backtracking.md      # 回溯
│   └── 10-python-libs.md       # Python 算法库速查
│
├── advanced/                    # 进阶篇
│   ├── 01-oop.md               # 面向对象
│   ├── 02-decorators.md        # 装饰器
│   ├── 03-generators.md        # 生成器
│   ├── 04-context-managers.md  # 上下文管理器
│   ├── 05-async.md             # 异步编程
│   └── 06-metaprogramming.md   # 元编程
│
├── ecosystem/                   # 生态系统
│   ├── 01-web.md               # Web 开发（FastAPI）
│   ├── 02-data.md              # 数据科学（NumPy/Pandas）
│   ├── 03-ai-llm.md            # AI/LLM 🔥
│   │   ├── 01-openai-api.md
│   │   ├── 02-rag.md
│   │   ├── 03-langchain.md
│   │   ├── 04-agents.md
│   │   └── 05-local-models.md
│   └── 04-automation.md        # 自动化
│
├── best-practices/              # 最佳实践
│   ├── 01-code-style.md        # 代码风格
│   ├── 02-type-hints.md        # 类型注解
│   ├── 03-testing.md           # 测试
│   ├── 04-performance.md       # 性能优化
│   ├── 05-security.md          # 安全
│   └── 06-packaging.md         # 打包发布
│
├── cookbook/                    # 实战手册
│   ├── 01-common-tasks.md      # 常见任务
│   ├── 02-scripts.md           # 实用脚本
│   ├── 03-projects.md          # 项目案例
│   ├── 04-interview.md         # Python 面试题库
│   └── 05-algo-cheatsheet.md   # 算法速查表
│
└── manual/                      # 速查手册
    └── README.md               # Python 速查手册
```

---

## LeetCode 刷题对照表

| Python 算法章节 | 对应 LeetCode 专题 | 优先级 |
|-----------------|-------------------|--------|
| 02-arrays-strings.md | `01-array/`, `02-string/` | ⭐⭐⭐⭐⭐ |
| 03-linked-list.md | `03-linked-list/` | ⭐⭐⭐⭐⭐ |
| 04-hash.md | `04-hash/` | ⭐⭐⭐⭐ |
| 05-stack-queue.md | `05-stack-queue/` | ⭐⭐⭐⭐ |
| 06-tree.md | `06-tree/` | ⭐⭐⭐⭐⭐ |
| 07-graph.md | `08-graph/` | ⭐⭐⭐ |
| 08-dp.md | `09-dp/` | ⭐⭐⭐ |
| 09-backtracking.md | `10-backtracking/` | ⭐⭐⭐ |

**刷题顺序建议：**
1. 数组 → 字符串 → 链表（基础必会）
2. 哈希表 → 栈队列（进阶）
3. 树 → 图（面试高频）
4. 动态规划 → 回溯（高阶）

---

## 学习资源

### 官方文档

- [Python 官方文档](https://docs.python.org/zh-cn/3/)
- [PEP 索引](https://peps.python.org/)
- [uv 文档](https://docs.astral.sh/uv/)
- [pixi 文档](https://pixi.sh/)

### 推荐书籍

| 书名 | 难度 | 适合阶段 |
|------|------|----------|
| 《Python Cookbook》 | 中级 | 第2-3阶段 |
| 《流畅的 Python》 | 中高级 | 第3阶段 |
| 《Effective Python》 | 中高级 | 第5阶段 |
| 《Python Tricks》 | 中级 | 第2-3阶段 |

### 在线资源

- [Real Python](https://realpython.com/)
- [Python Morsels](https://pythonmorsels.com/)
- [Exercism Python Track](https://exercism.org/tracks/python)
- [LeetCode Python](https://leetcode.cn/)

### 视频教程

- [Python 3 教程 - 廖雪峰](https://www.liaoxuefeng.com/wiki/1016959663602400)
- [Python 进阶 - Microsoft](https://www.youtube.com/results?search_query=python+advanced)

---

## 时间规划

### 全职学习（5-7个月）

```
第1月：第1-2阶段（环境 + 基础语法）
第2月：第3阶段（数据结构与算法）⭐ 面试准备
第3月：第4阶段（面向对象与进阶）
第4-6月：第5阶段（方向深入）
持续：第6阶段（工程化）
```

### 业余学习（8-12个月）

```
第1-2月：第1-2阶段（环境 + 基础语法）
第3-4月：第3阶段（数据结构与算法）⭐ 重点
第5-6月：第4阶段（面向对象与进阶）
第7-10月：第5阶段（方向深入）
持续：第6阶段（工程化）
```

### 每周时间分配

```
理论学习：20%
算法练习：30%  (LeetCode 对应 ../../leetcode/)
动手编码：40%
复习总结：10%
```

---

## 学习建议

### 学习原则

1. **多动手，少观望**
   - 每个知识点都要写代码验证
   - 不要只看不练

2. **注重实践**
   - 每阶段至少完成一个项目
   - 尝试解决实际问题

3. **阅读源码**
   - 阅读优秀开源项目
   - 学习最佳实践

4. **持续更新**
   - 关注 Python 新特性
   - 关注工具链更新

### 避坑指南

| 陷阱 | 解决方案 |
|------|----------|
| 只学语法不练 | 每天至少写 50 行代码 |
| 陷入教程地狱 | 尽早开始项目实践 |
| 忽视代码质量 | 从一开始就用 Black + ruff |
| 不会用调试工具 | 学会用 pdb / breakpoint() |
| 不查文档 | 养成查官方文档习惯 |

### 练习建议

```python
# 每日练习
- 刷 1-2 道 LeetCode（对应 ../../leetcode/）
  - Easy：10 分钟内完成
  - Medium：30 分钟内完成
  - 重点关注 Python 优雅写法
- 写一个小脚本（文件处理、自动化）
- 阅读一段源码（优秀开源项目）

# 每周目标
- 完成 1 个小项目
- 写 1 篇学习笔记
- 参与 1 次代码审查
- 完成 1 个 LeetCode 专题（如数组、链表）

# Python 算法练习重点
- 掌握 collections、itertools、heapq、bisect 等标准库
- 学会用列表推导式、生成器简化代码
- 理解 Python 的内存模型（引用 vs 拷贝）
- 熟练使用装饰器和上下文管理器
```

---

## 进阶路线

完成本路线后，可以继续学习：

- 系统编程（C 扩展、Rust 扩展）
- 分布式系统
- 大数据处理（Spark、Dask）
- 深度学习（PyTorch、TensorFlow）
- 云原生（Kubernetes Operator 开发）

---

## 版本历史

- **v1.1** (2025-01-29) - 新增数据结构与算法学习阶段，整合 LeetCode 刷题内容
- **v1.0** (2025-01-29) - 初始版本，确定现代化工具栈

---

## 相关文档

- [LeetCode 刷题手册](../../leetcode/PLAN.md) - 12 个专题、100+ 题目详解，Java/Python 双语
- [AI 设计模式](../../patterns/ai/design-patterns.md)
- [后端设计模式](../../patterns/backend/design-patterns.md)
- [Linux 手册](../../manuals/linux/README.md)
- [Git 手册](../../manuals/git/README.md)
- [Neovim 配置](../../tools/neovim/lazyvim-setup.md)
