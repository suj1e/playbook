# bat 使用指南

> cat 的现代替代品，支持语法高亮和 Git 集成

---

## 1. 安装

### macOS
```bash
brew install bat
```

### Linux
```bash
# Ubuntu/Debian
sudo apt install bat

# CentOS/RHEL
sudo yum install bat

# 或使用 cargo
cargo install bat
```

### Windows
```powershell
scoop install bat
# 或
winget install sharkdp.bat
```

---

## 2. 基础使用

```bash
# 查看文件
bat file.txt

# 查看多个文件
bat file1.txt file2.txt

# 查看所有 .py 文件
bat *.py
```

---

## 3. 常用选项

### 输出控制

| 选项 | 说明 |
|------|------|
| `-n` | 显示行号（默认开启） |
| `--line-range` | 显示指定行范围 |
| `-l` | 只显示语法和文件信息 |
| `--wrap` | 自动换行（never/character/auto） |

### 显示控制

| 选项 | 说明 |
|------|------|
| `--style` | 显示风格（numbers,changes,header） |
| `--plain` | 纯文本模式（无语法高亮） |
| `--color` | 颜色控制（always/never/auto） |

### Git 集成

| 选项 | 说明 |
|------|------|
| `--diff` | 显示 Git diff |
| `--line-range` | 只显示修改的行 |

---

## 4. 常用场景

### 查看指定行

```bash
# 查看第 10-20 行
bat --line-range 10:20 file.txt

# 查看前 50 行
bat --line-range :50 file.txt

# 查看第 100 行到结尾
bat --line-range 100: file.txt
```

### 多文件查看

```bash
# 查看多个文件（带文件名标题）
bat file1.txt file2.txt file3.txt

# 查看所有配置文件
bat *.json
```

### 只显示语法信息

```bash
# 检测文件类型
bat -l file.txt

# 输出：plaintext
```

---

## 5. 与其他工具配合

### 与 fzf 配合

```bash
# 搜索并预览
fd | fzf --preview 'bat --color=always {}'

# 搜索文件并预览（只显示前 50 行）
fd | fzf --preview 'bat --color=always --line-range :50 {}'
```

### 与 git 配合

```bash
# 查看文件的 Git 变更
bat --diff file.txt

# 查看未提交的修改
git diff | bat

# 查看特定提交的变更
git show COMMIT | bat
```

### 与 ripgrep 配合

```bash
# 搜索并高亮显示
rg "pattern" --pass-through | bat

# 搜索并预览匹配的文件
rg "pattern" -l | xargs bat --line-range :50
```

---

## 6. 主题配置

```bash
# 查看可用主题
bat --list-themes

# 设置主题
export BAT_THEME="TwoDark"

# 临时使用主题
bat --theme=Monokai Extended file.txt
```

### 推荐主题

| 主题 | 特点 |
|------|------|
| `TwoDark` | 深色主题，适合终端 |
| `Monokai Extended` | Monokai 变体 |
| `Nord` | Nord 配色 |
| `GitHub` | GitHub 风格 |

---

## 7. 语法高亮

### 自动检测

```bash
# bat 自动检测文件类型
bat file.py  # Python 高亮
bat file.js  # JavaScript 高亮
```

### 手动指定

```bash
# 手动指定语法
bat -l python file.txt
```

### 支持的语言

bat 支持大多数编程语言和配置文件：
- Python, JavaScript, TypeScript, Go, Rust, Java, C/C++
- JSON, YAML, TOML, XML
- Markdown, HTML, CSS
- Shell 脚本
- Dockerfile, Kubernetes YAML
- 等等...

---

## 8. 分页查看

```bash
# 使用 less 分页
bat file.txt | less

# 或使用 bat 内置分页
bat --paging=always file.txt
```

---

## 9. 实用技巧

### 创建别名

```bash
# ~/.zshrc

# 用 bat 替代 cat
alias cat='bat'

# 带 Git diff 的 cat
alias catd='bat --diff'

# 只显示行号
alias catn='bat --style=numbers'
```

### 输出纯文本

```bash
# 禁用语法高亮
bat --plain file.txt

# 导出纯文本
bat --plain file.txt > output.txt
```

### 管道输入

```bash
# 管道输入
echo "Hello" | bat

# 查看环境变量
env | bat

# 查看 JSON 输出
curl -s https://api.github.com | bat -l json
```

---

## 10. 配置文件

```bash
# ~/.config/bat/config

# 主题
--theme="TwoDark"

# 显示样式
--style="numbers,changes,header"

# 自动分页
--paging="never"

# 线包装
--wrap="never"

# 默认行范围
--line-range="1:100"
```

---

## 11. 高级用法

### 自定义语法

```bash
# 添加自定义语法（如果 bat 不支持某种文件）
mkdir -p ~/.config/bat/syntaxes
cp my-syntax.json ~/.config/bat/syntaxes/
bat cache --build
```

### 导出带高亮的 HTML

```bash
# 导出 HTML
bat --color=always --style=plain file.txt > output.html
```

### 比较文件

```bash
# 并排比较
bat --diff file1.txt file2.txt
```

---

## 12. Shell 函数

### zsh 配置

```bash
# ~/.zshrc

# 搜索并查看
fbat() {
  local file=$(fd | fzf)
  [[ -n "$file" ]] && bat "$file"
}

# 搜索并查看前 50 行
fbat50() {
  local file=$(fd | fzf)
  [[ -n "$file" ]] && bat --line-range :50 "$file"
}

# 搜索并查看 Git diff
fbatdiff() {
  local file=$(git diff --name-only | fzf)
  [[ -n "$file" ]] && bat --diff "$file"
}
```

---

## 13. 故障排查

**Q: 颜色不显示？**
```bash
# 强制启用颜色
bat --color=always file.txt

# 检查终端支持
echo $TERM
```

**Q: 语法不正确？**
```bash
# 手动指定语法
bat -l python file.txt

# 查看检测到的语法
bat -l file.txt
```

**Q: Git diff 不显示？**
```bash
# 检查是否在 Git 仓库中
git status

# 强制显示 diff
bat --diff context=always file.txt
```

---

## 14. 最佳实践

1. **创建 cat 别名**：`alias cat='bat'`
2. **配置主题**：选择适合终端的主题
3. **使用 fzf 预览**：`fd | fzf --preview 'bat {}'`
4. **利用 Git 集成**：`bat --diff`
5. **限制行范围**：大文件只查看需要的部分

---

## 速查表

```bash
# 基础使用
bat file.txt

# 多文件
bat file1.txt file2.txt

# 指定行范围
bat --line-range 10:20 file.txt

# Git diff
bat --diff file.txt

# 指定语法
bat -l python file.txt

# 纯文本模式
bat --plain file.txt

# 设置主题
bat --theme=TwoDark file.txt

# 管道输入
echo "hello" | bat

# 查看 JSON
curl -s api.example.com | bat -l json
```

---

## 相关文档

- [bat 官网](https://github.com/sharkdp/bat)
- [fzf 指南](./fzf.md)
- [ripgrep 指南](./ripgrep.md)
