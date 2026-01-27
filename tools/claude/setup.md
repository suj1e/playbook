# Claude Code 配置指南

> AI 编程助手，上下文感知，代码生成

---

## 1. 安装

### macOS
```bash
npm install -g @anthropic-ai/claude-code

# 或使用 Homebrew
brew install claude-code
```

### Linux
```bash
npm install -g @anthropic-ai/claude-code
```

### Windows
```powershell
npm install -g @anthropic-ai/claude-code
```

---

## 2. 初始化配置

```bash
# 进入项目目录
cd /path/to/project

# 初始化配置
claude-code init

# 这将创建 .claude/ 目录结构
```

---

## 3. 目录结构

```
.claude/
├── settings.local.json    # 本地设置（Git 忽略）
├── agents/                # 自定义 Agent
│   ├── backend-architect.md
│   ├── devops-engineer.md
│   └── ai-engineer.md
├── commands/              # 自定义 Slash Command
│   ├── architecture-review.md
│   ├── ultra-think.md
│   └── update-docs.md
├── skills/                # 自定义 Skill
│   └── git-pushing/
│       ├── SKILL.md
│       └── scripts/
│           └── smart_commit.sh
└── scripts/               # 工具脚本
    └── context-monitor.py
```

---

## 4. 配置文件

### settings.local.json

```json
{
  "permissions": {
    "allow": [
      "Bash(git status)",
      "Bash(git diff:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git push:*)",
      "Bash(git pull:*)",
      "Bash(git log:*)",
      "Bash(tree:*)",
      "Bash(wc:*)"
    ]
  },
  "statusLine": {
    "type": "command",
    "command": "bash -c 'input=$(cat); MODEL=$(echo \"$input\" | jq -r \".model.display_name\"); DIR=$(echo \"$input\" | jq -r \".workspace.current_dir\"); BRANCH=\"\"; if git rev-parse --git-dir >/dev/null 2>&1; then BRANCH=\" | $(git branch --show-current 2>/dev/null)\"; fi; echo \"[$MODEL] $(DIR##*/)$BRANCH\"'"
  }
}
```

---

## 5. 自定义 Agent

### Agent 模板

```markdown
---
name: my-agent
description: 专门处理某个领域的任务
---

# My Agent

你是 [角色描述]。

## 能力

- 能力 1
- 能力 2

## 工作流程

1. 第一步
2. 第二步
3. 第三步

## 注意事项

- 重要提醒 1
- 重要提醒 2
```

### 示例：后端架构师

```markdown
---
name: backend-architect
description: RESTful API 设计、微服务、数据库架构、可扩展性规划
---

# Backend Architect Agent

你是后端架构专家，专注于设计和审查后端系统架构。

## 能力

- RESTful API 设计
- 微服务架构
- 数据库设计（SQL/NoSQL）
- 性能优化
- 可扩展性规划

## 工作流程

1. 理解需求
2. 分析现有架构
3. 提出改进方案
4. 编写技术文档

## 注意事项

- 遵循 SOLID 原则
- 考虑可扩展性
- 关注性能和安全性
```

---

## 6. 自定义 Slash Command

### Command 模板

```markdown
---
name: my-command
description: 自定义命令描述
---

# My Command

## 使用场景

当用户需要 [某个功能] 时使用。

## 执行流程

1. 第一步
2. 第二步
3. 第三步
```

### 示例：架构审查

```markdown
---
name: architecture-review
description: Comprehensive architecture review with design patterns analysis and improvement recommendations
---

# Architecture Review Command

## 使用场景

当用户需要进行架构审查时使用。

## 执行流程

1. 分析当前架构
2. 识别设计模式
3. 检查依赖关系
4. 提出改进建议
5. 编写审查报告
```

---

## 7. 自定义 Skill

### Skill 模板

```markdown
---
name: my-skill
description: 技能描述
---

# My Skill

## 使用场景

当用户 [某个需求] 时自动激活。

## 实现步骤

1. 步骤一
2. 步骤二
3. 步骤三
```

### 示例：Git Pushing

```markdown
---
name: git-pushing
description: Stage, commit, and push git changes with conventional commit messages
---

# Git Push Workflow

## 使用场景

当用户需要提交和推送代码时自动激活。

## 实现步骤

1. 使用 smart_commit.sh 脚本
2. 自动生成符合规范的提交信息
3. 推送到远程仓库

## 使用方法

```bash
bash skills/git-pushing/scripts/smart_commit.sh "commit message"
```
```

---

## 8. 常用内置命令

### 代码操作

| 命令 | 功能 |
|------|------|
| `/commit` | 创建 Git commit |
| `/review` | 代码审查 |
| `/test` | 运行测试 |
| `/build` | 构建项目 |

### 文件操作

| 命令 | 功能 |
|------|------|
| `/read <file>` | 读取文件 |
| `/write <file>` | 写入文件 |
| `/edit <file>` | 编辑文件 |
| `/search <pattern>` | 搜索代码 |

### 分析操作

| 命令 | 功能 |
|------|------|
| `/analyze <file>` | 分析文件 |
| `/explain <code>` | 解释代码 |
| `/refactor <code>` | 重构代码 |
| `/optimize <code>` | 优化代码 |

---

## 9. 高级用法

### 上下文控制

```bash
# 限制上下文到特定文件
claude-code --include src/**/*.ts

# 排除特定文件
claude-code --exclude node_modules

# 设置最大上下文
claude-code --max-context 100000
```

### 交互模式

```bash
# 启动交互模式
claude-code

# 在交互模式中
> /help          # 显示帮助
> /agent <name>  # 切换 Agent
> /exit          # 退出
```

### 批处理模式

```bash
# 从文件读取指令
claude-code --batch instructions.txt

# 从管道读取
echo "帮我重构这个函数" | claude-code
```

---

## 10. 工作流集成

### Git Hook 集成

```bash
# .git/hooks/pre-commit
#!/bin/bash
claude-code --batch --command review

# 如果审查不通过，阻止提交
if [ $? -ne 0 ]; then
  echo "代码审查未通过，请修复后再提交"
  exit 1
fi
```

### CI/CD 集成

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Review Code
        run: claude-code --batch --command review
```

---

## 11. 环境变量

```bash
# API Key
export ANTHROPIC_API_KEY="your-api-key"

# 模型选择
export CLAUDE_MODEL="claude-opus-4-5-20251101"

# 上下文大小
export CLAUDE_MAX_CONTEXT="200000"

# 日志级别
export CLAUDE_LOG_LEVEL="debug"
```

---

## 12. 故障排查

**Q: 命令不生效？**
```bash
# 检查命令定义
claude-code --list-commands

# 检查命令语法
claude-code --validate-command my-command
```

**Q: Agent 无法启动？**
```bash
# 检查 Agent 定义
claude-code --list-agents

# 检查 Agent 语法
claude-code --validate-agent my-agent
```

**Q: 上下文过大？**
```bash
# 减少上下文
claude-code --max-context 50000

# 排除大文件
claude-code --exclude "*.log"
```

---

## 13. 最佳实践

1. **合理使用 Agent**：不同任务用不同 Agent
2. **自定义 Command**：常用操作封装成命令
3. **控制上下文**：只包含必要的文件
4. **版本控制配置**：`.claude/` 加入 Git
5. **保护敏感信息**：`settings.local.json` 加入 `.gitignore`

---

## 速查表

```bash
# 安装
npm install -g @anthropic-ai/claude-code

# 初始化
claude-code init

# 交互模式
claude-code

# 查看帮助
claude-code --help

# 列出命令
claude-code --list-commands

# 列出 Agent
claude-code --list-agents

# 执行命令
claude-code --command my-command

# 使用 Agent
claude-code --agent my-agent

# 批处理
claude-code --batch instructions.txt
```

---

## 相关文档

- [Claude Code 官网](https://claude.ai/code)
- [Claude API 文档](https://docs.anthropic.com/)
- [Agent 配置](./agents.md)
- [Command 配置](./commands.md)
