# WezTerm 快捷键速查表

> 一图胜千言，一表胜千图

---

## macOS 快捷键

### Tab 管理

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 新建 Tab | `Cmd + T` | 创建新标签页 |
| 关闭 Tab | `Cmd + W` | 关闭当前标签页 |
| 切换 Tab | `Cmd + [` / `Cmd + ]` | 切换到上/下一个 Tab |
| 切换到 Tab N | `Cmd + 1-9` | 直接跳转到第 N 个 Tab |
| 重命名 Tab | `Cmd + Shift + R` | 自定义 Tab 名称 |
| 搜索 Tab | `Cmd + Shift + P` | 打开启动菜单 |

### Pane 管理

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 水平分割 | `Cmd + Shift + H` | 上下分屏 |
| 垂直分割 | `Cmd + Shift + V` | 左右分屏 |
| 切换 Pane | `Cmd + [` / `Cmd + ]` | 在 Pane 间切换 |
| 关闭 Pane | `Cmd + W` | 关闭当前 Pane |
| 调整大小 | `Cmd + Shift + ↑↓←→` | 调整当前 Pane 大小 |
| 最大化 Pane | `Cmd + Shift + M` | 最大化/恢复当前 Pane |

### 滚动与复制

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 向上滚动 | `Cmd + ↑` / `Shift + PageUp` | |
| 向下滚动 | `Cmd + ↓` / `Shift + PageDown` | |
| 滚动到顶部 | `Cmd + Home` / `Shift + Home` | |
| 滚动到底部 | `Cmd + End` / `Shift + End` | |
| 清屏 | `Cmd + K` | 清空屏幕内容 |
| 清屏历史 | `Cmd + Shift + K` | 清空屏幕和滚动缓冲 |
| 复制 | `Cmd + C` | 复制选中内容 |
| 粘贴 | `Cmd + V` | 粘贴剪贴板内容 |

### 窗口管理

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 新建窗口 | `Cmd + N` | 创建新窗口 |
| 全屏 | `Cmd + Enter` | 切换全屏 |
| 隐藏 | `Cmd + H` | 隐藏窗口 |
| 退出 | `Cmd + Q` | 退出 WezTerm |

### 字体大小

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 放大 | `Cmd + =` / `Cmd + +` | 增大字体 |
| 缩小 | `Cmd + -` | 减小字体 |
| 重置 | `Cmd + 0` | 恢复默认大小 |

---

## Linux/Windows 快捷键

将 macOS 的 `Cmd` 替换为 `Ctrl+Shift`：

| 操作 | 快捷键 |
|------|--------|
| 新建 Tab | `Ctrl + Shift + T` |
| 关闭 Tab | `Ctrl + Shift + W` |
| 切换 Tab | `Ctrl + Shift + [` / `]` |
| 水平分割 | `Ctrl + Shift + H` |
| 垂直分割 | `Ctrl + Shift + V` |

---

## 自定义快捷键示例

```lua
-- ~/.wezterm.lua
local keys = {
  -- 清屏（比 Cmd+K 更方便）
  { key = 'k', mods = 'CMD', action = wezterm.action.ClearScrollback('ScrollbackAndViewport') },

  -- 快速切换到常用 Tab
  { key = '1', mods = 'CMD|SHIFT', action = wezterm.action.ActivateTab(0) },
  { key = '2', mods = 'CMD|SHIFT', action = wezterm.action.ActivateTab(1) },
  { key = '3', mods = 'CMD|SHIFT', action = wezterm.action.ActivateTab(2) },

  -- 移动 Tab
  { key = '[', mods = 'CMD|SHIFT|ALT', action = wezterm.action.MoveTabRelative(-1) },
  { key = ']', mods = 'CMD|SHIFT|ALT', action = wezterm.action.MoveTabRelative(1) },

  -- 快速启动 SSH
  { key = 's', mods = 'CMD|SHIFT', action = wezterm.action.SpawnCommandInNewTab {
    args = { 'ssh', 'user@server' },
  }},
}

config.keys = keys
```

---

## 与 Zellij 配合使用

如果使用 Zellij，建议禁用 WezTerm 的 Pane 快捷键：

```lua
config.keys = {
  -- 禁用 Pane 相关快捷键
  { key = 'H', mods = 'CMD|SHIFT', action = wezterm.action.DisableDefaultAssignment },
  { key = 'V', mods = 'CMD|SHIFT', action = wezterm.action.DisableDefaultAssignment },
  { key = '[', mods = 'CMD', action = wezterm.action.DisableDefaultAssignment },
  { key = ']', mods = 'CMD', action = wezterm.action.DisableDefaultAssignment },
}
```

**职责分工：**
- **WezTerm**: Tab 管理、窗口管理、字体调整
- **Zellij**: Pane 管理、布局管理、会话持久化

---

## 快捷键记忆技巧

**Tab 操作**: 都在 `Cmd` 基础上
- `T` = Tab 新建
- `W` = Window/Close
- `[` `]` = 切换

**Pane 操作**: 都需要 `Shift`
- `H` = Horizontal split
- `V` = Vertical split

**滚动操作**: 方向键逻辑
- `↑` 向上滚动
- `Home` 回到顶部

---

## 相关文档

- [WezTerm 配置指南](./setup.md)
- [Zellij 快捷键](../zellij/keybindings.md)
