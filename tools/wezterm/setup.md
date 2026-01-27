# WezTerm 配置指南

> 跨平台 GPU 终端模拟器

---

## 1. 安装

### macOS
```bash
brew install --cask wezterm
```

### Linux
```bash
# Ubuntu/Debian
wget https://github.com/wez/wezterm/releases/download/20240127-170624-160e4711/wezterm-20240127-170624-160e4711.Ubuntu22.04.deb
sudo dpkg -i wezterm-*.deb
```

### Windows
```powershell
winget install wez.wezterm
```

---

## 2. 配置文件位置

| 平台   | 配置文件路径                               |
| ------ | -------------------------------------- |
| macOS  | `~/.wezterm.lua`                        |
| Linux  | `~/.wezterm.lua`                        |
| Windows | `%USERPROFILE%\.wezterm.lua` |

---

## 3. 基础配置

```lua
-- ~/.wezterm.lua
local wezterm = require('wezterm')
local config = wezterm.config.builder()

-- 字体配置
config.font = wezterm.font('JetBrains Mono', {
  weight = 'Regular',
  italic = false,
})
config.font_size = 14.0

-- 颜色方案（Tokyo Night）
config.color_scheme = 'Tokyo Night'

-- 窗口配置
config.window_padding = {
  left = 2,
  right = 2,
  top = 0,
  bottom = 0,
}
config.window_background_opacity = 0.9

-- Tab 栏
config.use_fancy_tab_bar = false
config.hide_tab_bar_if_only_one_tab = true

-- 默认 Shell
config.default_cwd = '~'

return config
```

---

## 4. 快捷键速查

| 操作           | 快捷键              |
| ------------ | ----------------- |
| 新建 Tab       | `Cmd+T` (macOS)    |
| `Ctrl+Shift+T` (Linux) |
| 切换 Tab       | `Cmd+[` / `Cmd+]`  |
| `Ctrl+Shift+[` / `Ctrl+Shift+]` |
| 关闭 Tab       | `Cmd+W`           |
| `Ctrl+Shift+W` |
| 水平分屏        | `Cmd+Shift+H`      |
| 垂直分屏        | `Cmd+Shift+V`      |
| 切换 Pane      | `Cmd+[` / `Cmd+]`  |
| 调整 Pane 大小  | `Cmd+Shift+↑↓←→`   |
| 复制           | `Cmd+C`           |
| 粘贴           | `Cmd+V`           |
| 放大/缩小       | `Cmd++` / `Cmd+-`  |
| 全屏           | `Cmd+Enter`       |

---

## 5. SSH Domains 配置

```lua
-- ~/.wezterm.lua
config.ssh_domains = {
  {
    name = 'prod-server',
    remote_address = 'user@prod.example.com',
    ssh_command = { 'ssh' },
  },
  {
    name = 'dev-server',
    remote_address = 'user@dev.example.com',
    ssh_command = { 'ssh', '-p', '2222' },
  },
}

-- 启动时连接
config.default_domain = 'dev-server'
```

使用：
```bash
wezterm cli ssh prod-server
wezterm connect prod-server
```

---

## 6. 多域名管理

```lua
-- 启动配置
config.launch_menu = {
  {
    label = 'Bash',
    args = { 'bash', '-l' },
  },
  {
    label = 'Zsh',
    args = { 'zsh', '-l' },
  },
  {
    label = 'Prod Server',
    args = { 'ssh', 'user@prod.example.com' },
  },
  {
    label = 'Docker',
    args = { 'docker', 'exec', '-it', 'container', 'bash' },
  },
}

-- 启动菜单: Cmd+Shift+P (macOS)
```

---

## 7. 常用代码片段

### 自定义快捷键

```lua
local keys = {
  -- 清屏
  { key = 'k', mods = 'CMD', action = wezterm.action.ClearScrollback('ScrollbackAndViewport') },
  -- 重命名 Tab
  { key = 'R', mods = 'CMD|SHIFT', action = wezterm.action.PromptInputLine {
    description = 'Enter new name for tab',
    action = wezterm.action_callback(function(window, pane, line)
      if line then
        window:active_tab():set_title(line)
      end
    end),
  }},
}

config.keys = keys
```

### 启动脚本

```lua
config.default_prog = { '/opt/homebrew/bin/zsh', '-l', '-c', 'source ~/.zshrc && zellij attach -c dev' }
```

---

## 8. 与 Zellij 配合

**推荐配置：让 WezTerm 管理 Tab，Zellij 管理 Pane**

```lua
-- WezTerm: 禁用 Pane 快捷键，让 Zellij 接管
config.keys = {
  { key = 'H', mods = 'CMD|SHIFT', action = wezterm.action.DisableDefaultAssignment },
  { key = 'V', mods = 'CMD|SHIFT', action = wezterm.action.DisableDefaultAssignment },
}
```

---

## 9. 故障排查

**Q: 中文字体显示乱码？**
```lua
-- 指定中文字体
config.font = wezterm.font('JetBrains Mono')
config.font_rules = {
  {
    intensity = 'Bold',
    italic = false,
    font = wezterm.font('Sarasa Gothic SC'),
  },
}
```

**Q: 颜色不生效？**
```bash
# 检查 shell 配置
echo $TERM
# 应该输出: wezterm
```

**Q: 配置文件不生效？**
```bash
# 检查语法错误
wezterm --config-file ~/.wezterm.lua lint-config
```

---

## 10. 进阶技巧

### 动态标题

```lua
wezterm.on('format-tab-title', function(tab, tabs, panes, config, hover, max_width)
  local pane = tab.active_pane
  local title = pane.title ~= '' and pane.title or '(empty)'

  -- 显示当前目录
  local cwd = pane.current_working_dir
  if cwd then
    title = cwd:match('([^/]+)$') .. ' | ' .. title
  end

  return { { Text = title } }
end)
```

### 状态栏

```lua
wezterm.plugin.require('https://github.com/sebanc/brackety.git')

config.status_bar = {
  left = { 'status', 'tabs' },
  right = { 'datetime' },
}
```

---

## 相关文档

- [WezTerm 官方文档](https://wezfurlong.org/wezterm/config/files.html)
- [配置文件示例](https://github.com/wez/wezterm/wiki/Config-Examples)
