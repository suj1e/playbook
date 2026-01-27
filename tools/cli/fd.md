# fd 使用指南

> 比 find 快 10 倍的文件查找工具

---

## 1. 安装

### macOS
```bash
brew install fd
```

### Linux
```bash
# Ubuntu/Debian
sudo apt install fd-find

# 创建别名（Ubuntu 上命令是 fdfind）
alias fd=fdfind

# 或使用 cargo
cargo install fd-find
```

---

## 2. 基础使用

```bash
# 搜索文件名
fd "pattern"

# 搜索扩展名
fd ".txt"

# 搜索隐藏文件
fd ".env" --hidden

# 在特定目录搜索
fd "pattern" /path/to/dir
```

---

## 3. 常用选项

### 搜索控制

| 选项 | 说明 |
|------|------|
| `-i` | 忽略大小写 |
| `-s` | 大小写敏感 |
| `-g` | Glob 模式匹配 |
| `-e` | 扩展名过滤 |
| `-t` | 文件类型（f=file, d=dir, l=symlink） |
| `-E` | 正则表达式模式 |
| `--hidden` | 搜索隐藏文件 |
| `--no-ignore` | 不尊重 .ignore 文件 |

### 输出控制

| 选项 | 说明 |
|------|------|
| `-l` | 限制结果数量 |
| `-0` | 以 null 分隔（配合 xargs） |
| `-x` | 对每个结果执行命令 |
| `-H` | 搜索隐藏文件 |
| `-I` | 不尊重 .gitignore |

---

## 4. 常用场景

### 按扩展名搜索

```bash
# 搜索所有 .py 文件
fd -e py

# 搜索 .ts 和 .tsx 文件
fd -e ts -e tsx

# 搜索图片
fd -e png -e jpg -e jpeg -e gif
```

### 按类型搜索

```bash
# 只搜索文件
fd -t f "pattern"

# 只搜索目录
fd -t d "pattern"

# 只搜索符号链接
fd -t l "pattern"

# 搜索可执行文件
fd -t x -e sh
```

### 搜索隐藏文件

```bash
# 搜索所有 .env 文件（包括隐藏）
fd ".env" --hidden

# 搜索 dotfiles
fd "^\." --hidden
```

### 排除特定模式

```bash
# 排除 node_modules
fd "pattern" -E node_modules

# 排除多个目录
fd "pattern" -E node_modules -E dist -E build
```

---

## 5. 与其他工具配合

### 与 fzf 配合

```bash
# 搜索并用 fzf 选择
fd | fzf

# 搜索并打开
fd | fzf | xargs nvim

# 搜索目录并进入
fd -t d | fzf | cd
```

### 与 ripgrep 配合

```bash
# 搜索文件内容
fd "pattern" | xargs rg "content"

# 更简洁的方式
fd -e py | xargs rg "TODO"
```

### 与 bat 配合

```bash
# 搜索并预览
fd | fzf --preview 'bat --color=always {}'
```

---

## 6. 实用技巧

### 搜索并删除

```bash
# 预览要删除的文件
fd ".log"

# 删除所有 .log 文件
fd -e log -x rm

# 强制删除
fd -e log -X rm -f
```

### 搜索并移动

```bash
# 移动所有 .txt 到 archive 目录
fd -e txt -x mv {} archive/
```

### 搜索并压缩

```bash
# 压缩所有 .log 文件
fd -e log -x gzip
```

---

## 7. 高级用法

### Glob 模式

```bash
# 搜索所有匹配模式
fd -g "src/**/*.ts"

# 搜索测试文件
fd -g "*test*"

# 排除模式
fd -g "!node_modules/**"
```

### 正则表达式

```bash
# 使用正则
fd -E ".*\.test\.(ts|js)"

# 搜索数字开头的文件
fd -E "^[0-9]"
```

### 执行命令

```bash
# 对每个结果执行命令
fd -e py -x python -m pyflakes {}

# 并行执行（大写 X）
fd -e log -X gzip {}

# 多个参数
fd -e jpg -x convert {} {.}.png
```

---

## 8. Shell 函数

### zsh 配置

```bash
# ~/.zshrc

# 搜索并进入目录
cd() {
  if [[ $# -eq 1 ]]; then
    local dir=$(fd -t d -H "^$1" | head -1)
    if [[ -n "$dir" ]]; then
      builtin cd "$dir"
      return
    fi
  fi
  builtin cd "$@"
}

# 搜索并编辑
fe() {
  local file=$(fd -t f | fzf)
  [[ -n "$file" ]] && nvim "$file"
}

# 搜索并删除
fdel() {
  local files=$(fd | fzf -m)
  [[ -n "$files" ]] && rm -rf $files
}
```

---

## 9. 配置文件

```bash
# ~/.config/fd/ignore

# 排除常见目录
node_modules
.git
target
dist
build
vendor

# 排除常见文件
*.log
*.swp
*.swo
.DS_Store
```

---

## 10. 性能对比

| 工具 | 搜索 100 万文件耗时 |
|------|------------------|
| find | ~30s |
| **fd** | **~3s** |

---

## 11. 常用组合命令

### 搜索 Git 修改的文件

```bash
fd . $(git diff --name-only --diff-filter=d)
```

### 搜索并统计

```bash
# 统计文件数量
fd -e py | wc -l

# 统计代码行数
fd -e py | xargs wc -l
```

### 搜索并归档

```bash
# 压缩所有匹配文件
fd "pattern" -x tar -czf archive.tar.gz
```

---

## 12. 故障排查

**Q: 搜索不到结果？**
```bash
# 检查是否被 .gitignore 排除
fd "pattern" --no-ignore

# 检查是否是隐藏文件
fd "pattern" --hidden
```

**Q: 搜索很慢？**
```bash
# 排除大目录
fd "pattern" -E node_modules -E vendor
```

---

## 13. 最佳实践

1. **使用扩展名过滤**：`fd -e py` 比 `fd ".py$"` 更快
2. **排除大目录**：node_modules, vendor, .git
3. **利用配置文件**：`~/.config/fd/ignore`
4. **配合 fzf**：交互式选择
5. **使用 -x 执行**：批量操作

---

## 速查表

```bash
# 基础搜索
fd "pattern"

# 忽略大小写
fd "pattern" -i

# 扩展名
fd -e py

# 文件类型
fd -t f "pattern"
fd -t d "pattern"

# 隐藏文件
fd "pattern" --hidden

# 不尊重 .gitignore
fd "pattern" --no-ignore

# 执行命令
fd -e log -x rm

# 限制结果
fd "pattern" -l 10
```

---

## 相关文档

- [fd 官网](https://github.com/sharkdp/fd)
- [fzf 指南](./fzf.md)
- [ripgrep 指南](./ripgrep.md)
