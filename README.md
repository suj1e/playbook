# Playbook

> 基础原理、操作指南、架构设计与最佳实践

## 目录结构

```
playbook/
├── fundamentals/      # 基础原理 / 知识底座
│   ├── networking/
│   ├── os/
│   ├── database/
│   └── distributed/
│
├── manuals/           # 手册 / How-to / 操作指南
│   ├── git/
│   ├── linux/
│   ├── docker/
│   ├── kubernetes/
│   └── cloud/
│
├── playbooks/         # ⭐ 核心：方案 & 实战
│   ├── system-design/
│   ├── microservices/
│   ├── performance/
│   ├── troubleshooting/
│   └── security/
│
├── patterns/          # 架构 / 设计模式
│   ├── backend/
│   ├── frontend/
│   └── distributed/
│
├── checklists/        # 清单（上线 / 排查 / 迁移）
│   ├── pre-release.md
│   ├── incident.md
│   └── migration.md
│
├── tools/             # 工具 & 配置
│   ├── nvim/
│   ├── lazyvim/
│   ├── wezterm/
│   ├── zellij/
│   ├── claude/
│   └── cli/
│
├── leetcode/          # ⭐ 力扣刷题手册
│   ├── 00-templates/  # 通用模板
│   ├── 01-array/      # 数组专题
│   ├── 02-string/     # 字符串专题
│   ├── 03-linked-list/# 链表专题
│   ├── 04-hash/       # 哈希表专题
│   └── ...
│
└── drafts/            # 草稿 / 未整理
```

## 各模块说明

| 模块 | 说明 |
|------|------|
| **fundamentals** | 计算机基础原理：网络、操作系统、数据库、分布式系统 |
| **manuals** | 常用工具操作手册：Git、Linux、Docker、Kubernetes 等 |
| **playbooks** | 核心实战方案：系统设计、微服务、性能优化、故障排查、安全 |
| **patterns** | 架构与设计模式：后端、前端、分布式系统常用模式 |
| **checklists** | 各类检查清单：上线前检查、事故处理、迁移清单等 |
| **tools** | 开发工具配置：Neovim、LazyVim、Wezterm、Zellij、CLI 工具等 |
| **leetcode** | ⭐ 力扣刷题手册：系统化的刷题指南，Java + Python 双语言实现 |
| **drafts** | 待整理的草稿内容 |

## 使用方式

```bash
# 克隆仓库
git clone https://github.com/sujie/playbook.git

# 进入目录
cd playbook

# 查找特定主题
find . -name "*.md" | xargs grep -l "关键词"
```

## 贡献指南

欢迎提交 Issue 和 Pull Request。

## License

MIT
