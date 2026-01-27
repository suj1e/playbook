# 日常开发工作流

> 工具配合使用，提高开发效率

---

## 1. 启动开发环境

### 方案一：完整开发环境

```bash
# 1. 启动 WezTerm
open -a WezTerm

# 2. 新建 Tab（Cmd+T），启动 Zellij
zellij --layout dev

# 3. 得到 3 窗格布局：
#    - 上方：nvim（代码编辑）
#    - 下方左：zsh（命令行）
#    - 下方右：lazygit（Git 操作）

# 4. 新建 Tab（Cmd+T），启动服务
npm run dev

# 5. 新建 Tab（Cmd+T），连接到服务器
ssh user@server
```

### 方案二：轻量级开发

```bash
# 1. WezTerm 单窗口
# 2. 使用 Zellij 管理会话
zellij attach -c dev
```

---

## 2. 代码编辑工作流

### 文件导航

```bash
# 使用 fzf 查找文件
fd | fzf

# 使用 nvim 打开
nvim $(fd | fzf)

# 在 nvim 中使用 Telescope
<Space>ff
```

### 代码搜索

```bash
# 使用 ripgrep 搜索代码
rg "pattern" -l

# 使用 fzf 选择文件
rg "pattern" -l | fzf

# 使用 nvim 打开
nvim $(rg "pattern" -l | fzf)
```

### 快速跳转

```vim
" 在 nvim 中
:Telescope keymaps      " 查看快捷键
:Telescope buffers     " 查看缓冲区
:Telescope oldfiles    " 查看最近文件
:Telescope git_files   " 查看文件
:Telescope live_grep   " 搜索代码
```

---

## 3. Git 工作流

### 日常提交

```bash
# 1. 查看状态
git status

# 2. 查看 diff
git diff

# 3. 添加文件
git add .

# 4. 提交（使用 Claude Code）
claude-code --command git-pushing

# 或使用脚本
bash ~/.claude/skills/git-pushing/scripts/smart_commit.sh
```

### 代码审查

```bash
# 使用 LazyGit
lazygit

# 或使用 Zellij 布局
zellij --layout git-workflow

# 布局：
# - 上方：lazygit
# - 下方左：nvim（查看代码）
# - 下方右：终端
```

---

## 4. 调试工作流

### 本地调试

```bash
# 1. 启动服务
npm run dev

# 2. 查看日志
bat -f logs/app.log

# 3. 搜索错误
rg "ERROR" logs/

# 4. 分析堆栈
rg "at" logs/ -A 5
```

### 远程调试

```bash
# 1. SSH 到服务器
ssh user@server

# 2. 启动 Zellij 会话
zellij attach -c server

# 3. 查看日志
journalctl -u app -f

# 4. 搜索错误
rg "ERROR" /var/log/app/
```

---

## 5. 重构工作流

### 使用 Claude Code

```bash
# 1. 分析代码
claude-code --agent backend-architect --command analyze

# 2. 重构建议
claude-code --agent backend-architect --command refactor

# 3. 生成代码
claude-code --agent backend-architect --command generate
```

### 手动重构

```vim
" 在 nvim 中
1. 跳转到定义: gd
2. 查找引用: gr
3. 重命名: <Space>cr
4. 格式化: <Space>cf
5. 代码动作: <Space>ca
```

---

## 6. 文档编写工作流

### 使用 Claude Code

```bash
# 1. 生成文档
claude-code --command update-docs

# 2. 审查文档
claude-code --command architecture-review

# 3. 提交文档
bash ~/.claude/skills/git-pushing/scripts/smart_commit.sh
```

### 手动编写

```vim
" 在 nvim 中
1. 编辑 Markdown 文件
2. 实时预览: <Space>mp
3. 检查语法: <Space>mc
4. 格式化: <Space>cf
```

---

## 7. 故障排查工作流

### 日志查看

```bash
# 1. 实时查看
bat -f logs/app.log

# 2. 搜索错误
rg "ERROR" logs/ -A 5 -B 2

# 3. 统计错误
rg "ERROR" logs/ --count

# 4. 按时间过滤
rg "2024-01-10" logs/ -A 10
```

### 进程检查

```bash
# 1. 查找进程
ps aux | fzf | grep "node"

# 2. 杀死进程
kill $(ps aux | fzf | grep "node" | awk '{print $2}')

# 3. 查看端口
lsof -i :3000
```

### 网络检查

```bash
# 1. 检查端口
nc -zv localhost 3000

# 2. 检查 DNS
nslookup google.com

# 3. 测试连接
curl -I https://api.example.com
```

---

## 8. 工具组合技巧

### fzf + ripgrep

```bash
# 搜索文件并预览
rg "pattern" -l | fzf --preview 'bat --color=always {}'

# 搜索文件并编辑
rg "pattern" -l | fzf | xargs nvim
```

### fd + fzf

```bash
# 查找目录并进入
fd -t d | fzf | cd

# 查找文件并删除
fd -e log | fzf -m | xargs rm
```

### bat + fzf

```bash
# 查看文件
fd | fzf --preview 'bat --color=always {}'

# 查看前 50 行
fd | fzf --preview 'bat --color=always --line-range :50 {}'
```

### git + fzf

```bash
# 查看分支
git branch | fzf

# 切换分支
git branch | fzf | xargs git checkout

# 查看提交
git log --oneline | fzf --preview 'git show {+1}'
```

---

## 9. 快捷键记忆

### WeZTerm 快捷键

| 操作 | 快捷键 |
|------|--------|
| 新建 Tab | `Cmd + T` |
| 切换 Tab | `Cmd + [` / `]` |
| 关闭 Tab | `Cmd + W` |

### Zellij 快捷键

| 操作 | 快捷键 |
|------|--------|
| 新建 Tab | `Ctrl + t` |
| 水平分割 | `Ctrl + p` 然后 `-` |
| 垂直分割 | `Ctrl + p` 然后 `\|` |
| 切换 Pane | `Alt + ↑` / `↓` / `←` / `→` |
| Detach | `Ctrl + o` 然后 `d` |

### Neovim/LazyVim 快捷键

| 操作 | 快捷键 |
|------|--------|
| 查找文件 | `<Space>ff` |
| Live grep | `<Space>fg` |
| 跳转定义 | `gd` |
| 重命名 | `<Space>cr` |
| 格式化 | `<Space>cf` |

---

## 10. 效率提升技巧

### 1. 使用 Shell 别名

```bash
# ~/.zshrc
alias nv='nvim'
alias la='lazygit'
alias lz='zellij attach -c dev'
alias gs='git status'
alias gc='git commit'
alias gp='git push'
```

### 2. 使用 Shell 函数

```bash
# ~/.zshrc

# 搜索并编辑
fe() {
  local file=$(fd | fzf)
  [[ -n "$file" ]] && nvim "$file"
}

# 搜索并执行
fx() {
  local cmd=$(fc -ln -20 | fzf +s)
  [[ -n "$cmd" ]] && eval "$cmd"
}
```

### 3. 使用 Zellij 布局

```bash
# 创建常用布局
zellij layout save --current dev
zellij layout save --current git-workflow
zellij layout save --current debug

# 快速启动
zellij --layout dev
zellij --layout git-workflow
zellij --layout debug
```

### 4. 使用 LazyVim 会话

```vim
" 保存会话
:mksession project.vim

" 加载会话
:source project.vim

" 自动加载会话
" 在 lua/config/autocmds.lua
vim.o.sessionoptions="blank,buffers,curdir,folds,help,tabpages,winsize,terminal"
```

---

## 11. 典型一天的工作流

### 上午：开始工作

```bash
# 1. 启动环境
open -a WezTerm
zellij attach -c dev

# 2. 拉取最新代码
git pull

# 3. 查看任务
nvim TODO.md

# 4. 开始开发
<Space>ff  # 打开文件
```

### 下午：代码审查

```bash
# 1. 查看变更
git diff

# 2. LazyGit 审查
lazygit

# 3. 提交代码
bash ~/.claude/skills/git-pushing/scripts/smart_commit.sh
```

### 晚上：总结工作

```bash
# 1. 更新文档
claude-code --command update-docs

# 2. 提交代码
bash ~/.claude/skills/git-pushing/scripts/smart_commit.sh

# 3. 推送到远程
git push
```

---

## 相关文档

- [WezTerm 配置](../wezterm/setup.md)
- [Zellij 配置](../zellij/setup.md)
- [Neovim/LazyVim 配置](../neovim/lazyvim-setup.md)
- [Claude Code 配置](../claude/setup.md)
