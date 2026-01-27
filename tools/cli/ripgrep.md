# ripgrep (rg) 使用指南

> 比 grep 快 10-100 倍的代码搜索工具

---

## 1. 安装

### macOS
```bash
brew install ripgrep
```

### Linux
```bash
# Ubuntu/Debian
sudo apt install ripgrep

# CentOS/RHEL
sudo yum install ripgrep

# 或使用 cargo
cargo install ripgrep
```

---

## 2. 基础使用

```bash
# 搜索文本
rg "pattern"

# 在特定目录搜索
rg "pattern" /path/to/dir

# 递归搜索（默认）
rg "pattern" .

# 不递归
rg "pattern" --no-hidden
```

---

## 3. 常用选项

### 搜索控制

| 选项 | 说明 |
|------|------|
| `-i` | 忽略大小写 |
| `-s` | 大小写敏感 |
| `-w` | 全词匹配 |
| `-x` | 全行匹配 |
| `-F` | 固定字符串（非正则） |
| `-v` | 反向匹配（不包含） |

### 输出控制

| 选项 | 说明 |
|------|------|
| `-n` | 显示行号 |
| `--count` | 只显示匹配数量 |
| `--files-with-matches` | 只显示匹配的文件名 |
| `-l` | 同上 |
| `-L` | 只显示不匹配的文件名 |
| `--no-filename` | 不显示文件名 |
| `-H` | 始终显示文件名 |

### 搜索范围

| 选项 | 说明 |
|------|------|
| `-g` | Glob 模式过滤文件 |
| `-t` | 文件类型（py, rs, js 等） |
| `-T` | 排除文件类型 |
| `--hidden` | 搜索隐藏文件 |
| `--no-ignore` | 不尊重 .ignore 文件 |

---

## 4. 常用场景

### 搜索函数定义

```bash
# 搜索函数
rg "def function_name"

# 搜索类
rg "class MyClass"
```

### 搜索注释

```bash
# 搜索 TODO
rg "TODO" -t py

# 搜索 FIXME
rg "FIXME"
```

### 搜索特定文件类型

```bash
# 只在 Python 文件搜索
rg "pattern" -t py

# 只在 Rust 文件搜索
rg "pattern" -t rust

# 排除 JS 文件
rg "pattern" -T js
```

### 使用 Glob 模式

```bash
# 搜索 src 目录下的所有 .ts 文件
rg "pattern" -g "*.ts"

# 排除 node_modules
rg "pattern" -g "!node_modules"

# 只搜索测试文件
rg "pattern" -g "*test*"
```

---

## 5. 正则表达式

```bash
# OR 匹配
rg "foo|bar"

# AND 匹配（多个模式）
rg "foo" . | rg "bar"

# 捕获组
rg "(foo|bar)baz"

# 量词
rg "a{2,4}"  # 2-4 个 a
rg "a+"      # 1 个或更多
rg "a?"      # 0 个或 1 个
```

---

## 6. 替换

```bash
# 预览替换（不实际执行）
rg "foo" --replace "bar"

# 实际替换（需要 pipe 到 sed）
rg "foo" --files-with-matches | xargs sed -i 's/foo/bar/g'
```

---

## 7. 与其他工具配合

### 与 fzf 配合

```bash
# 搜索结果用 fzf 选择
rg "pattern" | fzf

# 搜索并打开文件
rg "pattern" -l | fzf | xargs nvim
```

### 与 bat 配合

```bash
# 高亮显示搜索结果
rg "pattern" --pass-through | bat
```

### 与 less 配合

```bash
# 分页显示
rg "pattern" --pretty | less
```

---

## 8. 配置文件

```bash
# ~/.ripgreprc

# 排除常见目录
--glob=!node_modules
--glob=!vendor
--glob=!.git
--glob=!target
--glob=!dist

# 默认选项
--smart-case
--hidden
--follow
```

---

## 9. 性能对比

| 工具 | 搜索 1000 万行代码耗时 |
|------|---------------------|
| grep | ~30s |
| ack | ~10s |
| ag   | ~5s |
| **rg** | **~1s** |

---

## 10. 实用技巧

### 搜索并上下文显示

```bash
# 显示匹配行前后 3 行
rg "pattern" -C 3

# 显示匹配行后 5 行
rg "pattern" -A 5

# 显示匹配行前 5 行
rg "pattern" -B 5
```

### 搜索并统计

```bash
# 统计匹配数量
rg "pattern" --count

# 按文件统计
rg "pattern" --count --sort-files
```

### 搜索并排序

```bash
# 按文件名排序
rg "pattern" --sort-files

# 按路径排序
rg "pattern" --sort=path
```

---

## 11. 常用组合命令

### 搜索 Git 修改文件中的内容

```bash
rg "pattern" $(git diff --name-only)
```

### 搜索最近修改的文件

```bash
rg "pattern" --files-with-matches | xargs ls -lt | head -10
```

### 搜索并编辑

```bash
# nvim 打开第一个匹配文件
nvim $(rg "pattern" -l | head -1)

# nvim 打开所有匹配文件
nvim $(rg "pattern" -l)
```

---

## 12. 故障排查

**Q: 搜索不到结果？**
```bash
# 检查是否被 .gitignore 排除
rg "pattern" --no-ignore

# 检查是否是隐藏文件
rg "pattern" --hidden
```

**Q: 搜索很慢？**
```bash
# 排除不必要的目录
rg "pattern" --glob '!node_modules' --glob '!.git'
```

---

## 13. 最佳实践

1. **使用文件类型过滤**：`-t py` 代替 `-g "*.py"`
2. **排除大目录**：node_modules, vendor, .git
3. **利用配置文件**：`~/.ripgreprc`
4. **配合 fzf**：交互式选择
5. **使用 --pretty**：更好的输出格式

---

## 速查表

```bash
# 基础搜索
rg "pattern"

# 忽略大小写
rg "pattern" -i

# 全词匹配
rg "pattern" -w

# 显示行号
rg "pattern" -n

# 只显示文件名
rg "pattern" -l

# 排除目录
rg "pattern" --glob '!node_modules'

# 上下文
rg "pattern" -C 3

# 文件类型
rg "pattern" -t py

# 搜索替换预览
rg "pattern" --replace "bar"
```

---

## 相关文档

- [ripgrep 官网](https://github.com/BurntSushi/ripgrep)
- [fzf 指南](./fzf.md)
- [fd 指南](./fd.md)
