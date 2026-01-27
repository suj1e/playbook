# 开发工具手册

> 工欲善其事，必先利其器

本目录包含日常开发中常用的工具配置手册，涵盖终端、编辑器、CLI 工具等。

---

## 目录结构

```
tools/
├── wezterm/       # 终端模拟器
├── zellij/        # 终端复用器
├── neovim/        # 编辑器核心
├── lazyvim/       # Neovim 配置
├── claude/        # Claude Code 配置
├── cli/           # 命令行工具
└── workflows/     # 工作流指南
```

---

## 快速导航

| 工具 | 用途 | 核心优势 |
|------|------|----------|
| [WezTerm](./wezterm/) | GPU 终端模拟器 | 跨平台、性能好、配置强大 |
| [Zellij](./zellij/) | 终端复用器 | 会话持久化、布局灵活 |
| [Neovim + LazyVim](./neovim/) | 现代编辑器 | LSP 支持、插件生态丰富 |
| [Claude Code](./claude/) | AI 编程助手 | 上下文感知、代码生成 |
| [fzf](./cli/fzf.md) | 模糊查找 | 速度极快、交互友好 |
| [ripgrep](./cli/ripgrep.md) | 代码搜索 | 比 grep 快 10-100 倍 |
| [fd](./cli/fd.md) | 文件查找 | 比 find 快 10 倍 |

---

## 推荐安装顺序

1. **WezTerm** - 终端基础
2. **Zellij** - 会话管理
3. **Neovim + LazyVim** - 编辑器
4. **CLI 工具集** - fzf, ripgrep, fd, bat
5. **Claude Code** - AI 辅助

---

## 常见问题

**Q: 为什么用 WezTerm 而不是 iTerm2/Terminal?**
A: WezTerm 跨平台（macOS/Linux/Windows），配置文件同步方便，GPU 加速性能更好。

**Q: Zellij 和 tmux 的区别？**
A: Zellij 用 Rust 编写，配置更现代，内置布局系统，对新手更友好。

**Q: LazyVim 和 Nvchad 的选择？**
A: LazyVim 开箱即用，文档完善，适合快速上手。

---

## 相关链接

- [WezTerm 官网](https://wezfurlong.org/wezterm/)
- [Zellij 官网](https://zellij.dev/)
- [LazyVim 官网](https://www.lazyvim.org/)
- [Claude Code 官网](https://claude.ai/code)
