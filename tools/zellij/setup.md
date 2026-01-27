# Zellij 配置指南

> 终端复用器，让终端会话持久化

---

## 1. 安装

### macOS
```bash
brew install zellij
```

### Linux
```bash
# Ubuntu/Debian
cargo install zellij

# 或使用预编译包
wget https://github.com/zellij-org/zellij/releases/latest/download/zellij-aarch64-unknown-linux-musl.tar.gz
tar -xzf zellij-*.tar.gz
sudo mv zellij /usr/local/bin/
```

---

## 2. 初始化配置

```bash
# 生成默认配置
zellij setup --dump-config > ~/.config/zellij/config.kdl

# 生成布局配置
zellij setup --dump-layout > ~/.config/zellij/layout/default.kdl
```

---

## 3. 目录结构

```
~/.config/zellij/
├── config.kdl           # 主配置
└── layout/
    ├── default.kdl       # 默认布局
    ├── dev.kdl           # 开发布局
    └── dashboard.kdl     # 仪表盘布局
```

---

## 4. 基础配置

```kdl
// ~/.config/zellij/config.kdl

default_shell "zsh"

// 禁用默认模式提示（减少干扰）
pane_frames false

// 主题（支持自定义）
theme "default"

// 鼠标支持
mouse_mode true

// 自动会话命名
session_serialization true

// Tab 位置
tab_position "top"
```

---

## 5. 快捷键速查

### 基础操作

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 进入帮助模式 | `Ctrl + g` 然后 `?` | 查看所有快捷键 |
| 进入会话管理 | `Ctrl + g` 然后 `s` | Session 管理 |
| 进入 Pane 管理 | `Ctrl + g` 然后 `p` | Pane 管理 |
| 进入 Tab 管理 | `Ctrl + g` 然后 `t` | Tab 管理 |
| 进入调整大小模式 | `Ctrl + g` 然后 `e` | Resize 模式 |

### Pane 操作

| 操作 | 快捷键 |
|------|--------|
| 水平分割 | `Ctrl + p` 然后 `-` |
| 垂直分割 | `Ctrl + p` 然后 `\|` |
| 切换 Pane | `Alt + ↑` / `↓` / `←` / `→` |
| 关闭 Pane | `Ctrl + p` 然后 `x` |
| 全屏 Pane | `Ctrl + Shift + x` |

### Tab 操作

| 操作 | 快捷键 |
|------|--------|
| 新建 Tab | `Ctrl + t` |
| 切换 Tab | `Alt + [` / `]` |
| 重命名 Tab | `Ctrl + g` 然后 `n` |

### 会话操作

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 创建新会话 | `zellij` | |
| 附加到会话 | `zellij attach <name>` | |
| 列出会话 | `zellij list-sessions` | |
| 后台运行 | `Ctrl + o` 然后 `d` | detach |
| 强制退出 | `Ctrl + a` 然后 `q` | quit（关闭所有 Pane） |

---

## 6. 自定义布局

### 开发布局（3 窗格）

```kdl
// ~/.config/zellij/layout/dev.kdl
layout {
    tab name="dev" {
        pane size=1 {
            borderless=true
            pane split_axes="horizontal" {
                pane size=2 borderless=true {
                    command "nvim"
                    args "."
                }
                pane size=1 borderless=true {
                    split_axes="vertical" {
                        pane {
                            command "zsh"
                        }
                        pane {
                            command "lazygit"
                        }
                    }
                }
            }
        }
    }
    tab name="server" {
        pane {
            command "npm"
            args "run", "dev"
        }
    }
}
```

使用：
```bash
zellij --layout dev.kdl
```

### 仪表盘布局（4 窗格）

```kdl
// ~/.config/zellij/layout/dashboard.kdl
layout {
    tab name="dashboard" {
        pane split_axes="vertical" {
            pane {
                split_axes="horizontal" {
                    pane { command "htop" }
                    pane { command "btop" }
                }
            }
            pane {
                split_axes="horizontal" {
                    pane {
                        command "zsh"
                        args "-c", "lazydocker"
                    }
                    pane { command "zsh" }
                }
            }
        }
    }
}
```

---

## 7. 与 WezTerm 配合

**推荐配置：分工明确**

| 功能 | 负责工具 | 快捷键前缀 |
|------|----------|-----------|
| Tab 管理 | WezTerm | `Cmd + ...` |
| Pane 管理 | Zellij | `Ctrl + p` / `Alt + ...` |
| 会话管理 | Zellij | `Ctrl + g` / `Ctrl + o` |

**WezTerm 配置调整：**
```lua
-- 禁用 WezTerm 的 Pane 快捷键，让 Zellij 接管
config.keys = {
  { key = 'H', mods = 'CMD|SHIFT', action = wezterm.action.DisableDefaultAssignment },
  { key = 'V', mods = 'CMD|SHIFT', action = wezterm.action.DisableDefaultAssignment },
  { key = '[', mods = 'CMD', action = wezterm.action.DisableDefaultAssignment },
  { key = ']', mods = 'CMD', action = wezterm.action.DisableDefaultAssignment },
}
```

---

## 8. 常用命令

```bash
# 启动默认会话
zellij

# 启动并命名
zellij --session my-project

# 附加到会话
zellij attach my-project

# 附加到最近的会话
zellij attach -c

# 列出所有会话
zellij list-sessions

# 强制关闭会话
zellij kill-session my-project

# 删除所有会话
zellij delete-all-sessions

# 使用自定义布局
zellij --layout ~/.config/zellij/layout/dev.kdl
```

---

## 9. 开发工作流示例

### 启动开发环境

```bash
# 1. 启动 WezTerm
open -a WezTerm

# 2. 在 WezTerm 中启动 Zellij
zellij --layout dev

# 得到：3 窗格布局（nvim / zsh / lazygit）

# 3. Cmd+T 新建 Tab，启动服务
npm run dev

# 4. Cmd+T 新建 Tab，连接到服务器
ssh user@server
```

### 断开会话（服务器端）

```bash
# SSH 到服务器
ssh user@server

# 启动 Zellij
zellij

# ... 工作 ...

# 断开：Ctrl + g 然后 d（detach）
# 继续在服务器后台运行

# 下次 SSH 回来，附加回会话
zellij attach -c
```

---

## 10. 故障排查

**Q: 快捷键不生效？**
```bash
# 检查是否有冲突
# Zellij 默认快捷键：Ctrl + g, Ctrl + p, Ctrl + t
# 如果冲突，可在 config.kdl 中修改
```

**Q: 会话丢失？**
```bash
# 检查会话序列化是否开启
# 在 config.kdl 中：
session_serialization true
```

**Q: 性能问题？**
```bash
# 关闭 Tab 渲染动画
tab_position "top"
pane_frames false
```

---

## 11. 高级技巧

### Tab 自动命名

```kdl
// config.kdl
default_shell "zsh"
// Tab 名称自动跟随当前目录
```

### 自动启动脚本

```bash
# ~/.zshrc
# 启动 Zellij（如果不在会话中）
if [[ -z "$ZELLIJ" ]]; then
  zellij attach -c dev
fi
```

### Tab 样式定制

```kdl
// config.kdl
themes {
    default {
        fg 255 255 255
        bg 30 30 30
        red 255 0 0
        green 0 255 0
        yellow 255 255 0
        blue 0 0 255
        magenta 255 0 255
        cyan 0 255 255
        white 255 255 255
    }
}
```

---

## 相关文档

- [Zellij 官方文档](https://zellij.dev/documentation.html)
- [布局指南](https://zellij.dev/documentation.html#layouts)
- [WezTerm 配置](../wezterm/setup.md)
