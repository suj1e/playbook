# AI 设计模式

> LLM 应用架构与最佳实践

---

## 目录

- [1. RAG 模式](#1-rag-模式)
- [2. Agent 模式](#2-agent-模式)
- [3. Chain-of-Thought 模式](#3-chain-of-thought-模式)
- [4. Few-Shot 模式](#4-few-shot-模式)
- [5. Vector Database 模式](#5-vector-database-模式)
- [6. Tool Use / Function Calling 模式](#6-tool-use--function-calling-模式)
- [7. Prompt Engineering 模式](#7-prompt-engineering-模式)
- [8. Multi-Agent 协作模式](#8-multi-agent-协作模式)
- [9. Embedding 模式](#9-embedding-模式)
- [10. Fine-tuning 模式](#10-fine-tuning-模式)

---

## 1. RAG 模式

### 1.1 概述

**RAG (Retrieval-Augmented Generation)** - 检索增强生成，结合检索和生成的混合模式。

### 1.2 架构

```
用户提问
  ↓
向量化 (Embedding)
  ↓
向量检索 (Vector Search)
  ↓
检索相关文档
  ↓
构建 Prompt (上下文 + 问题)
  ↓
LLM 生成回答
  ↓
返回结果
```

### 1.3 实现

```python
# 向量化
query_embedding = embed_model.encode(query)

# 向量检索
relevant_docs = vector_db.search(query_embedding, top_k=5)

# 构建 Prompt
context = "\n".join([doc.content for doc in relevant_docs])
prompt = f"""
基于以下文档回答问题：
{context}

问题：{query}
回答：
"""

# 生成
answer = llm.generate(prompt)
```

### 1.4 优化策略

| 策略 | 说明 |
|------|------|
| 分块检索 | 文档分段后分别检索 |
| 重排序 | 对检索结果重新排序 |
| 混合检索 | 结合关键词和向量检索 |
| 查询扩展 | 扩展用户查询提高召回率 |
| 上下文压缩 | 压缩检索到的上下文 |

### 1.5 适用场景

- 知识库问答
- 文档搜索
- 技术支持
- 企业知识管理

---

## 2. Agent 模式

### 2.1 概述

**Agent** - 具有感知、推理和行动能力的自主实体，能使用工具完成复杂任务。

### 2.2 架构

```
Agent
  ↓
感知 (Perceive)
  ↓
推理 (Reason)
  ↓
行动 (Act)
  ↓
反馈 (Feedback)
  ↓
循环...
```

### 2.3 核心 Agent 框架

```python
# LangChain Agent
from langchain.agents import initialize_agent, Tool

tools = [
    Tool(name="Search", func=search_tool),
    Tool(name="Calculator", func=calculator_tool),
    Tool(name="CodeExecutor", func=code_executor),
]

agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent_type="zero-shot-react-description",
    verbose=True
)

result = agent.run("搜索最新 AI 论文并总结核心观点")
```

### 2.4 Agent 类型

| 类型 | 特点 | 适用场景 |
|------|------|----------|
| ReAct Agent | 推理-行动循环 | 单步任务 |
| Plan-and-Execute Agent | 先规划后执行 | 多步任务 |
| Multi-Agent System | 多个 Agent 协作 | 复杂任务分解 |
| Autonomous Agent | 完全自主 | 长期运行任务 |

### 2.5 工具使用

```python
# 定义工具
from langchain.tools import tool

@tool
def search_api(query: str) -> str:
    """搜索 API 调用"""
    return api.search(query)

@tool
def database_query(sql: str) -> str:
    """数据库查询"""
    return db.execute(sql)

@tool
def code_executor(code: str) -> str:
    """代码执行"""
    exec(code, globals())
    return "执行完成"
```

---

## 3. Chain-of-Thought 模式

### 3.1 概述

**Chain-of-Thought** - 链式思考，通过中间推理步骤提高问题解决能力。

### 3.2 模板

```prompt
让我们一步步思考这个问题：

问题：{question}

思考过程：
1. 首先，理解问题的核心需求...
2. 然后，分析问题的关键约束...
3. 接着，考虑可能的解决方案...
4. 最后，评估并选择最佳方案...

最终答案：
```

### 3.3 实现方式

```python
# 方式一：Prompt 引导
cot_prompt = """
让我们一步步解决这个问题：
{question}

请逐步展示你的推理过程。
"""

# 方式二：自动推理
from langchain.chains import LLMChain

cot_chain = LLMChain(
    llm=llm,
    prompt=prompt_template,
    output_key="reasoning"
)

# 方式三：多步推理
from langchain.chains import SequentialChain

step1_prompt = ChatPromptTemplate(
    input_variables=["question"],
    template="问题分析：{question}"
)
step2_prompt = ChatPromptTemplate(
    input_variables=["analysis"],
    template="基于分析，生成方案：{analysis}"
)
```

### 3.4 优化技巧

| 技巧 | 说明 |
|------|------|
| 分步引导 | 明确要求"第一步、第二步..." |
| 思维链展示 | 要求模型展示推理过程 |
| 自我验证 | 让模型检查自己的答案 |
| 角色设定 | "你是一个专业的..." |

---

## 4. Few-Shot 模式

### 4.1 概述

**Few-Shot Learning** - 通过提供少量示例，让模型快速学习任务模式。

### 4.2 模板

```prompt
任务：情感分类

示例 1：
文本：今天天气真好！
情感：正面

示例 2：
产品质量太差了，非常失望。
情感：负面

示例 3：
服务态度还可以，但物流太慢。
情感：中性

现在分类以下文本：
文本：{text}
情感：
```

### 4.3 Few-Shot 类型

| 类型 | 说明 | 示例 |
|------|------|------|
| Zero-Shot | 无示例，直接提问 | 翻译这段文本 |
| One-Shot | 一个示例 | 仿照示例写代码 |
| Few-Shot | 少量示例 | 分类任务 |
| Many-Shot | 大量示例 | 学习新领域 |

### 4.4 设计原则

| 原则 | 说明 |
|------|------|
| 示例代表性 | 示例应覆盖不同场景 |
| 示例质量 | 示例应准确无误 |
| 提示清晰 | 任务描述要明确 |
| 避免偏差 | 示例应平衡各类别 |

---

## 5. Vector Database 模式

### 5.1 概述

**Vector Database** - 存储和检索高维向量的数据库，支持语义搜索。

### 5.2 主流方案

| 方案 | 特点 | 适用场景 |
|------|------|----------|
| Pinecone | 托管服务 | 快速上线 |
| Weaviate | 开源、GraphQL | 自建方案 |
| Milvus | 开源、高性能 | 大规模部署 |
| Chroma | 轻量级、嵌入 | 本地开发 |
| pgvector | PostgreSQL 插件 | 现有 PG 架构 |

### 5.3 实现示例

```python
# Chroma 本地向量数据库
import chromadb
from chromadb.config import Settings

# 创建客户端
client = chromadb.Client()

# 创建 collection
collection = client.create_collection("documents")

# 添加文档
collection.add(
    documents=["文档内容"],
    embeddings=[embedding_model.encode("文档内容")],
    metadatas=[{"source": "manual"}]
)

# 查询
results = collection.query(
    query_embeddings=[query_embedding],
    n_results=5
)
```

### 5.4 向量化策略

| 策略 | 说明 |
|------|------|
| 文本分段 | 按段落或固定长度分段 |
| 重叠分段 | 相邻段落有重叠 |
| 元数据关联 | 保存文档元数据 |
| 多向量索引 | 不同模型使用不同索引 |

---

## 6. Tool Use / Function Calling 模式

### 6.1 概述

**Tool Use / Function Calling** - LLM 通过调用外部工具/函数完成任务。

### 6.2 架构

```
用户请求
  ↓
LLM 理解意图
  ↓
选择工具/函数
  ↓
生成函数参数
  ↓
执行函数
  ↓
返回结果
  ↓
LLM 整理答案
```

### 6.3 函数定义

```python
# OpenAI Function Calling
import openai

functions = [
  {
    "name": "get_weather",
    "description": "获取当前天气",
    "parameters": {
      "type": "object",
      "properties": {
        "location": {
          "type": "string",
          "description": "城市名称"
        }
      },
      "required": ["location"]
    }
  }
]

response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "北京天气怎么样？"}],
    functions=functions
)
```

### 6.4 LangChain Tools

```python
from langchain.tools import tool

@tool
def search_database(query: str) -> str:
    """搜索数据库"""
    # 实现数据库搜索逻辑
    return f"搜索结果：{query}"

@tool
def calculate(expression: str) -> str:
    """计算数学表达式"""
    result = eval(expression)
    return f"计算结果：{result}"
```

---

## 7. Prompt Engineering 模式

### 7.1 基础原则

| 原则 | 说明 | 示例 |
|------|------|------|
| 清晰具体 | 明确说明需求 | "写一个排序算法" → "写一个快速排序算法" |
| 角色设定 | 设定专家角色 | "你是一个资深后端工程师" |
| 上下文提供 | 提供必要背景 | "在 Spring Boot 项目中..." |
| 输出格式 | 指定输出格式 | "以 JSON 格式返回" |
| 约束条件 | 明确限制条件 | "不使用第三方库" |

### 7.2 Prompt 模板

```markdown
# 角色设定
你是一个资深 {role}，擅长 {expertise}。

# 任务描述
{task_description}

# 约束条件
- 约束1
- 约束2

# 输出要求
- 输出格式：{format}
- 语言：{language}

# 示例（可选）
示例：
输入：{input_example}
输出：{output_example}

# 现在开始任务
{input}
```

### 7.3 高级技巧

**思维链 (CoT)**
```markdown
让我们一步步思考：
1. 首先分析...
2. 然后考虑...
3. 最后得出...
```

**自省 (Self-Consistency)**
```markdown
请自检你的答案：
1. 你的假设是什么？
2. 有什么反驳观点？
3. 如何改进？
```

**少样本提示 (Few-Shot)**
```markdown
示例1：
输入：...
输出：...

示例2：
输入：...
输出：...

现在处理：
输入：...
输出：
```

---

## 8. Multi-Agent 协作模式

### 8.1 概述

**Multi-Agent System** - 多个 Agent 协作完成复杂任务，每个 Agent 专注特定领域。

### 8.2 架构

```
用户请求
  ↓
主 Agent (Coordinator)
  ↓
  ├─→ Research Agent (搜索)
  ├─→ Coding Agent (编程)
  ├─→ Review Agent (审查)
  └─→ Testing Agent (测试)
  ↓
汇总结果
  ↓
返回用户
```

### 8.3 实现

```python
from langchain.agents import Agent, initialize_agent
from langchain.agents import AgentExecutor, create_openai_functions_agent

# 创建专业 Agent
research_agent = Agent(
    role="研究员",
    goal="搜索相关技术资料",
    tools=[search_tool, web_scraper]
)

coding_agent = Agent(
    role="程序员",
    goal="编写高质量代码",
    tools=[code_executor, file_reader]
)

review_agent = Agent(
    role="审查员",
    goal="审查代码质量",
    tools=[linter, test_runner]
)

# 创建协作流程
def multi_agent_workflow(task):
    # 1. Research
    research_result = research_agent.run(task)

    # 2. Coding
    code_result = coding_agent.run(research_result)

    # 3. Review
    review_result = review_agent.run(code_result)

    # 4. Finalize
    return review_result
```

### 8.4 协作模式

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| 层级式 | 主 Agent 分配任务 | 复杂项目开发 |
| 平行式 | Agent 并行工作 | 独立任务处理 |
| 辩论式 | Agent 相互辩论 | 方案评审 |
| 顺序式 | Agent 顺序处理 | 流水线任务 |

---

## 9. Embedding 模式

### 9.1 概述

**Embedding** - 将文本/图像/音频转换为高维向量，捕获语义信息。

### 9.2 主流模型

| 模型 | 维度 | 特点 | 适用场景 |
|------|------|------|----------|
| text-embedding-ada-002 | 1536 | OpenAI，高质量 | 通用文本 |
| bge-large-zh-v1.5 | 1024 | BGE，中文优化 | 中文文本 |
| text-embedding-3-small | 1536 | OpenAI，快速 | 实时应用 |
| voyage-large | 1024 | Voyage，长文本 | 长文档 |
| e5-large-v2 | 1024 | Intel，开源 | 本地部署 |

### 9.3 使用示例

```python
from sentence_transformers import SentenceTransformer

# 加载模型
model = SentenceTransformer('paraphrase-multilingual-MiniLM-L12-v2')

# 生成嵌入
embeddings = model.encode([
    "这是第一段文本",
    "这是第二段文本"
])

# 计算相似度
from sklearn.metrics.pairwise import cosine_similarity
similarity = cosine_similarity([embeddings[0]], [embeddings[1]])
```

### 9.4 优化策略

| 策略 | 说明 |
|------|------|
| 批量编码 | 提高编码效率 |
| 模型微调 | 适配特定领域 |
| 量化压缩 | 减少存储和计算 |
| 缓存嵌入 | 避免重复计算 |

---

## 10. Fine-tuning 模式

### 10.1 概述

**Fine-tuning** - 在预训练模型基础上，使用特定领域数据继续训练，提升模型性能。

### 10.2 微调类型

| 类型 | 说明 | 数据量 | 成本 |
|------|------|--------|------|
| Full Fine-tuning | 更新所有参数 | 大 | 高 |
| LoRA | 只训练低秩矩阵 | 小 | 低 |
| Prefix Tuning | 只训练前缀 | 小 | 低 |
| P-Tuning | 只训练提示词 | 小 | 低 |

### 10.3 LoRA 实现

```python
from peft import LoraConfig, TaskType, get_peft_model
from transformers import AutoModelForCausalLM

# 加载基础模型
base_model = AutoModelForCausalLM.from_pretrained("base-model")

# LoRA 配置
lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=8,                          # LoRA rank
    lora_alpha=32,
    target_modules=["q", "v"],
    lora_dropout=0.05,
)

# 应用 LoRA
model = get_peft_model(base_model, lora_config)

# 训练
model.fit(train_data)
```

### 10.4 微调流程

```
1. 数据准备
   - 收集领域数据
   - 数据清洗和标注
   - 数据集划分

2. 模型选择
   - 选择合适的基座模型
   - 评估模型容量

3. 超参数调优
   - 学习率
   - Batch size
   - 训练轮数

4. 训练
   - 监控训练指标
   - 防止过拟合

5. 评估
   - 在测试集上评估
   - 与基线对比
   - 部署上线
```

---

## 速查表

### RAG 模式
```python
# 检索增强生成
embedding = embed(query)
docs = vector_db.search(embedding, top_k=5)
answer = llm.generate(f"基于文档{docs}回答{query}")
```

### Agent 模式
```python
# LangChain Agent
from langchain.agents import initialize_agent

agent = initialize_agent(
    tools=[search_tool, calculator],
    llm=llm,
)
agent.run("任务描述")
```

### Chain-of-Thought
```prompt
让我们一步步思考：
1. 理解问题
2. 分析约束
3. 生成方案
4. 验证答案
```

### Few-Shot
```prompt
示例 1：
输入：...
输出：...

示例 2：
输入：...
输出：...

现在处理：
输入：{text}
输出：
```

---

## 相关文档

- [Claude Code 配置](../../tools/claude/setup.md)
- [RAG 实践](../../playbooks/ai/rag-implementation.md)
- [Agent 开发](../../playbooks/ai/agent-development.md)
