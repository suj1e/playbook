# fzf 使用指南

> 命令行模糊查找器，速度极快，交互友好

---

## 1. 安装

### macOS
```bash
brew install fzf

# 安装快捷键和自动补全
$(brew --prefix)/opt/fzf/install
```

### Linux
```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

---

## 2. 基础使用

### 交互式搜索

```bash
# 查找文件
fzf

# 查找并预览
fzf --preview 'bat --color=always {}'

# 多选模式
fzf -m
```

---

## 3. 快捷键（安装后）

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + T` | 在当前目录下搜索文件，插入到命令行 |
| `Ctrl + R` | 搜索命令历史 |
| `Alt + C` | 搜索并进入目录（cd） |

---

## 4. 常用组合

### 搜索并打开文件

```bash
# nvim 编辑
nvim $(fzf)

# code 编辑
code $(fzf)
```

### 搜索并删除

```bash
# 查找并删除文件
rm $(fzf)

# 查找并强制删除
rm -rf $(fzf)
```

### 搜索进程并杀死

```bash
kill $(ps aux | fzf | awk '{print $2}')
```

### Git 分支切换

```bash
git checkout $(git branch | fzf)
```

### Git 日志搜索

```bash
git log --oneline | fzf --preview 'git show {+1}'
```

---

## 5. 高级用法

### 预览功能

```bash
# 文件预览
fzf --preview 'bat --color=always {}' --preview-window=right:70%

# 目录预览
fzf --preview 'tree -C {}' --preview-window=right:70%

# Git 日志预览
git log --oneline | fzf --preview 'git show {+1}' --preview-window=down:70%
```

### 模糊匹配

```bash
# 模糊匹配（默认）
fzf

# 精确匹配
fzf +m

# 扩展匹配（空格分隔，空格=或）
fzf -e
```

### 搜索模式

```bash
# 搜索特定模式
fzf --filter "pattern"

# 排除特定模式
fzf --nth 1 --invert
```

---

## 6. 与 ripgrep 配合

```bash
# ripgrep + fzf = 代码搜索神器
rg --line-number --no-heading "pattern" | fzf

# 更好的体验：带预览
rg --line-number --no-heading "pattern" |
  fzf --delimiter : \
      --preview 'bat --color=always {1} --highlight-line {2}' \
      --preview-window 'right,70%,+{2}+3/3' \
      --bind 'enter:become(nvim {1} +{2})'
```

---

## 7. Shell 函数

### zsh 配置

```bash
# ~/.zshrc

# fzf 文件搜索
fe() {
  local files=$(fzf --query="$1" --multi --select-1 --exit-0)
  [[ -n "$files" ]] && ${EDITOR:-vim} $files
}

# fzf 目录切换
fd() {
  local dir=$(find ${1:-.} -path '*/\.*' -prune -o -type d -print 2>/dev/null | fzf)
  cd "$dir"
}

# fzf git 分支切换
fbr() {
  local branches branch
  branches=$(git branch) &&
  branch=$(echo "$branches" | fzf +m) &&
  git checkout $(echo "$branch" | sed "s/.* //")
}

# fzf 进程杀死
fkill() {
  local pid
  pid=$(ps -ef | sed 1d | fzf -m | awk '{print $2}')
  if [ "x$pid" != "x" ]; then
    echo $pid | xargs kill -${1:-9}
  fi
}
```

---

## 8. 常用选项

| 选项 | 说明 |
|------|------|
| `--height` | 高度（如 `--height 40%`） |
| `--reverse` | 反向显示（从上到下） |
| `--prompt` | 提示符（如 `--prompt "Files> "`） |
| `--pointer` | 指针符号（如 `--pointer "▶"`） |
| `--marker` | 多选标记（如 `--marker "✔"`） |
| `--border` | 边框（如 `--border rounded`） |
| `--preview` | 预览命令 |
| `--preview-window` | 预览窗口位置和大小 |

---

## 9. 实用技巧

### 搜索环境变量

```bash
printenv | fzf
```

### 搜索 Docker 容器

```bash
docker ps | fzf
```

### 搜索 npm scripts

```bash
cat package.json | jq -r '.scripts | keys[]' | fzf | xargs npm run
```

### 搜索 Git 修改的文件

```bash
git status --short | fzf -m | awk '{print $2}'
```

---

## 10. 性能优化

```bash
# 限制搜索范围
fzf --filepath-word

# 使用缓存
fzf --tiebreak=index

# 限制结果数量
fzf --tac --no-sort
```

---

## 11. 故障排查

**Q: fzf 启动很慢？**
```bash
# 排除特定目录
fzf --exclude-dir={node_modules,vendor,.git}
```

**Q: 预览不生效？**
```bash
# 检查预览命令语法
fzf --preview 'echo {}'
```

---

## 12. 最佳实践

1. **使用预览功能**：提高查找效率
2. **配合 ripgrep**：先搜索再定位
3. **自定义快捷键**：符合个人习惯
4. **编写 Shell 函数**：常用操作封装
5. **限制搜索范围**：排除不必要的目录

---

## 相关文档

- [fzf 官网](https://github.com/junegunn/fzf)
- [ripgrep 指南](./ripgrep.md)
- [fd 指南](./fd.md)
