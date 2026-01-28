# Git 操作手册

> 版本控制必备技能

---

## 目录

- [1. 基础概念](#1-基础概念)
- [2. 基础操作](#2-基础操作)
- [3. 分支管理](#3-分支管理)
- [4. 远程操作](#4-远程操作)
- [5. 撤销与回退](#5-撤销与回退)
- [6. 合并与冲突解决](#6-合并与冲突解决)
- [7. 标签管理](#7-标签管理)
- [8. 常用技巧](#8-常用技巧)
- [9. 工作流](#9-工作流)
- [10. 故障排查](#10-故障排查)

---

## 1. 基础概念

### 1.1 Git 三大区域

```
工作区 (Working Directory)
  ↓ git add
暂存区 (Staging Area / Index)
  ↓ git commit
本地仓库 (Local Repository)
  ↓ git push
远程仓库 (Remote Repository)
```

### 1.2 Git 配置

```bash
# 查看配置
git config --list
git config --list --global

# 设置用户信息（首次使用必须）
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 设置编辑器
git config --global core.editor vim

# 设置默认分支名
git config --global init.defaultBranch main

# 设置代理
git config --global http.proxy http://proxy.example.com:8080
git config --global https.proxy http://proxy.example.com:8080

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

## 2. 基础操作

### 2.1 初始化仓库

```bash
# 初始化新仓库
git init

# 克隆远程仓库
git clone https://github.com/user/repo.git
git clone -b branch https://github.com/user/repo.git
git clone --depth 1 https://github.com/user/repo.git  # 浅克隆（只克隆最新提交）
```

### 2.2 查看状态

```bash
# 查看仓库状态
git status
git status -s                 # 短格式

# 查看文件变更
git diff                       # 工作区 vs 暂存区
git diff --staged              # 暂存区 vs 本地仓库
git diff HEAD                 # 工作区 vs 本地仓库
git diff file.txt              # 查看特定文件

# 查看提交历史
git log
git log --oneline             # 简洁显示
git log --graph --oneline --all # 图形化显示所有分支
git log --stat                 # 显示变更文件统计
```

### 2.3 添加与提交

```bash
# 添加文件到暂存区
git add file.txt
git add .                     # 添加所有文件
git add *.txt                  # 添加所有 txt 文件
git add -A                     # 添加所有文件（包括删除的）
git add -u                     # 只添加修改和删除的

# 提交
git commit -m "commit message"
git commit -am "commit message" # 添加所有已跟踪文件并提交
git commit --amend             # 修改最后一次提交

# 查看暂存区
git status
git diff --cached             # 查看暂存区内容
```

### 2.4 忽略文件

```bash
# .gitignore 示例
*.log
*.tmp
*.swp
*.swo
node_modules/
.DS_Store
target/
out/
dist/
.env.local

# 强制添加被忽略的文件
git add -f file.txt
```

---

## 3. 分支管理

### 3.1 分支操作

```bash
# 查看分支
git branch                     # 本地分支
git branch -r                  # 远程分支
git branch -a                  # 所有分支

# 创建分支
git branch feature-branch
git checkout -b feature-branch # 创建并切换

# 切换分支
git checkout main
git switch main                # Git 2.23+ 新语法

# 删除分支
git branch -d feature-branch  # 删除已合并分支
git branch -D feature-branch  # 强制删除

# 重命名分支
git branch -m old-name new-name
```

### 3.2 分支合并

```bash
# 合并分支
git merge feature-branch       # 合并到当前分支

# 变基（rebase）
git rebase main                # 将当前分支变基到 main
git rebase -i HEAD~3           # 交互式变基最近 3 个提交

# 取消变基
git rebase --abort

# 继续变基
git rebase --continue
```

### 3.3 分支最佳实践

```bash
# 功能分支工作流
git checkout -b feature/user-auth
# ... 开发 ...
git checkout main
git merge feature/user-auth
git branch -d feature/user-auth

# 主分支保护
git checkout main
git branch -M main             # 重命名当前分支为 main
```

---

## 4. 远程操作

### 4.1 远程仓库管理

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/user/repo.git

# 删除远程仓库
git remote remove origin

# 修改远程仓库 URL
git remote set-url origin https://github.com/user/new-repo.git
```

### 4.2 拉取与推送

```bash
# 拉取远程更新
git fetch origin                # 获取远程更新
git pull origin main           # 拉取并合并
git pull --rebase origin main  # 拉取并变基

# 推送到远程
git push origin main            # 推送当前分支
git push -u origin feature-branch  # 推送并设置上游
git push origin --all          # 推送所有分支
git push origin --delete old-branch  # 删除远程分支
```

### 4.3 远程分支跟踪

```bash
# 设置跟踪分支
git branch --set-upstream-to=origin/main main
git branch -u origin/main      # 简写

# 查看跟踪关系
git branch -vv

# 删除跟踪关系
git branch --unset-upstream main
```

---

## 5. 撤销与回退

### 5.1 撤销工作区修改

```bash
# 撤销工作区修改
git restore file.txt          # Git 2.23+
git checkout -- file.txt      # 旧版本

# 撤销所有工作区修改
git restore .
git checkout -- .

# 丢弃工作区修改（保留暂存区）
git checkout -- .
```

### 5.2 撤销暂存区

```bash
# 撤销单个文件的暂存
git restore --staged file.txt
git reset HEAD file.txt      # 旧版本

# 撤销所有暂存
git restore --staged .
git reset HEAD                # 旧版本
```

### 5.3 撤销提交

```bash
# 撤销最后一次提交（保留修改）
git reset --soft HEAD~1

# 撤销最后一次提交（保留工作区）
git reset --mixed HEAD~1
git reset HEAD~1              # 默认

# 撤销最后一次提交（丢弃所有修改）
git reset --hard HEAD~1

# 回退到指定提交（危险！）
git reset --hard commit-hash
```

### 5.4 恢复丢失的提交

```bash
# 查看所有操作记录
git reflog

# 恢复到指定提交
git reset --hard commit-hash

# 查看 dangling commits
git fsck --lost-found
git log refs/original/HEAD~1
```

---

## 6. 合并与冲突解决

### 6.1 合并冲突

```bash
# 查看冲突文件
git status

# 查看冲突内容
cat file.txt

# Git 冲突标记
<<<<<<< HEAD
当前分支的内容
=======
合并分支的内容
>>>>>>> feature-branch

# 解决冲突后
git add file.txt
git commit
```

### 6.2 冲突解决工具

```bash
# 使用 mergetool
git mergetool                   # 使用配置的工具
git mergetool --tool vimdiff   # 指定工具

# 常用 mergetool 配置
git config --global mergetool.cmd nvim
git config --global mergetool.keepBackup false

# 跳过冲突
git merge --abort               # 取消合并
git merge --skip               # 跳过冲突
```

### 6.3 变基冲突

```bash
# 变基过程中的冲突处理
git rebase main
# ... 解决冲突 ...
git add .
git rebase --continue

# 取消变基
git rebase --abort

# 跳过当前提交
git rebase --skip

# 交互式变基
git rebase -i HEAD~3
```

---

## 7. 标签管理

### 7.1 创建标签

```bash
# 创建轻量标签
git tag v1.0.0

# 创建附注标签
git tag -a v1.0.0 -m "Release version 1.0.0"

# 给指定提交打标签
git tag v1.0.0 commit-hash

# 查看标签
git tag
git show v1.0.0                # 查看标签信息
```

### 7.2 推送标签

```bash
# 推送指定标签
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 删除远程标签
git push origin --delete v1.0.0

# 删除本地标签
git tag -d v1.0.0
```

### 7.3 检出标签

```bash
# 查看标签版本
git checkout v1.0.0
git switch -d v1.0.0          # Git 2.23+

# 基于标签创建分支
git checkout -b release-1.0.0 v1.0.0
```

---

## 8. 常用技巧

### 8.1 交互式添加

```bash
# 交互式添加文件
git add -i

# 交互式暂存补丁
git add -p                      # 暂存部分修改
git add -p file.txt            # 暂存文件的部分修改
```

### 8.2 储藏工作

```bash
# 储存当前工作
git stash
git stash save "work in progress"

# 查看储藏列表
git stash list

# 应用储藏
git stash apply                 # 应用但不删除储藏
git stash pop                  # 应用并删除储藏

# 删除储藏
git stash drop                  # 删除最新储藏
git stash clear                # 清空所有储藏

# 应用指定储藏
git stash apply stash@{1}
```

### 8.3 清理工作

```bash
# 清理未跟踪文件
git clean -fd                   # 删除未跟踪文件和目录

# 清理未跟踪文件（预览）
git clean -fd -n                # 显示要删除的文件

# 清理已删除文件
git gc                          # 垃圾回收
git gc --prune=now              # 立即清理

# 彻底清理
git gc --aggressive --prune=now
```

### 8.4 查找提交

```bash
# 按消息查找
git log --grep="fix bug"
git log --all --grep="fix bug"

# 按作者查找
git log --author="John"

# 按文件查找
git log -- file.txt
git log --all -- file.txt

# 按内容查找
git log -S"function_name"

# 查找引入 bug 的提交
git log --since="1 week ago" --until="3 days ago"
git log --after="2024-01-01" --before="2024-12-31"

# 二分查找
git bisect start
git bisect bad                 # 标记当前版本为坏
git bisect good                # 标记当前版本为好
git bisect reset                # 重置二分查找
```

### 8.5 查看文件历史

```bash
# 查看文件历史
git log --follow file.txt

# 查看文件每一行的修改历史
git blame file.txt

# 查看文件的修改者
git shortlog -sn               # 按作者统计
```

### 8.6 Cherry-pick

```bash
# 挑选指定提交
git cherry-pick commit-hash

# 挑选多个提交
git cherry-pick hash1 hash2 hash3

# 挑选但不自动提交
git cherry-pick -n commit-hash
git cherry-pick --no-commit commit-hash

# 撤销 cherry-pick
git cherry-pick --abort
```

---

## 9. 工作流

### 9.1 功能分支工作流

```bash
# 1. 从 main 创建功能分支
git checkout main
git checkout -b feature/user-auth

# 2. 开发并提交
git add .
git commit -m "feat: add user authentication"

# 3. 合并回 main
git checkout main
git merge feature/user-auth

# 4. 删除功能分支
git branch -d feature/user-auth

# 5. 推送到远程
git push origin main
```

### 9.2 Git Flow 工作流

```bash
# 主分支：main, develop
# 功能分支：feature/*
# 发布分支：release/*
# 修复分支：hotfix/*

# 开始新功能
git checkout develop
git checkout -b feature/user-auth

# 完成功能
git checkout develop
git merge feature/user-auth

# 准备发布
git checkout -b release-1.0.0 develop
git checkout main
git merge release-1.0.0

# 紧急修复
git checkout -b hotfix-critical-bug main
# ... 修复 ...
git checkout main
git merge hotfix-critical-bug
git checkout develop
git merge hotfix-critical-bug
```

### 9.3 Pull Request 工作流

```bash
# 1. Fork 并克隆仓库
git clone https://github.com/your-username/repo.git

# 2. 创建功能分支
git checkout -b feature-branch

# 3. 推送到远程
git push -u origin feature-branch

# 4. 创建 Pull Request
# 在 GitHub/GitLab 上操作

# 5. 更新分支
git checkout main
git pull origin main
git checkout feature-branch
git rebase main
git push origin feature-branch --force

# 6. 合并 PR
# 在 GitHub/GitLab 上操作

# 7. 删除分支
git branch -d feature-branch
git push origin --delete feature-branch
```

### 9.4 Rebase 工作流

```bash
# 1. 保持分支最新
git checkout main
git pull

# 2. 变基功能分支
git checkout feature-branch
git rebase main

# 3. 解决冲突（如有）
git add .
git rebase --continue

# 4. 推送（可能需要强制推送）
git push origin feature-branch --force
```

---

## 10. 故障排查

### 10.1 常见问题

**Q: 提交信息写错了？**
```bash
# 修改最后一次提交信息
git commit --amend
git commit --amend -m "new message"

# 修改更早的提交信息
git rebase -i HEAD~3
# 将 pick 改为 reword
```

**Q: 推送被拒绝了？**
```bash
# 远程有新提交，本地不是最新的
git pull --rebase origin main
git push origin main
```

**Q: 忘记推送某个分支？**
```bash
# 恢复丢失的提交
git reflog
git checkout -b recovered-branch commit-hash
git push origin recovered-branch
```

**Q: 合并错误很多？**
```bash
# 取消合并
git merge --abort

# 使用 rebase 代替 merge
git rebase main
```

**Q: 文件被删除了，如何恢复？**
```bash
# 查看文件历史
git log -- file.txt

# 恢复文件
git checkout commit-hash -- file.txt
```

### 10.2 性能问题

```bash
# 仓库太大，克隆慢
git clone --depth 1 https://github.com/user/repo.git
git clone --single-branch https://github.com/user/repo.git

# 拉取大仓库
git fetch --depth 1

# 清理历史
git gc --aggressive --prune=now
```

### 10.3 权限问题

```bash
# Permission denied (publickey)
ssh-keygen -t ed25519 -C "your@email.com"
cat ~/.ssh/id_ed25519.pub
# 添加到 GitHub/GitLab SSH Keys

# 测试连接
ssh -T git@github.com
```

### 10.4 冲突解决

```bash
# 查看冲突文件
git diff --name-only --diff-filter=U

# 使用工具解决冲突
git mergetool
git mergetool --tool=vimdiff

# 手动解决后
git add .
git commit
```

---

## 11. 高级技巧

### 11.1 子模块

```bash
# 添加子模块
git submodule add https://github.com/user/repo.git lib/repo

# 克隆包含子模块的仓库
git clone --recursive https://github.com/user/repo.git
git submodule update --init --recursive

# 更新子模块
git submodule update --remote --merge
```

### 11.2 LFS (Large File Storage)

```bash
# 安装 Git LFS
git lfs install

# 追踪大文件
git lfs track "*.zip"
git lfs track "*.tar.gz"

# 提交 LFS 文件
git add .gitattributes
git commit -m "Add LFS tracking"
git push
```

### 11.3 Partial Clone（部分克隆）

```bash
# 只克隆特定目录
git sparse-checkout init
git sparse-checkout set src/
git sparse-checkout set package/

# 查看稀疏检出配置
git sparse-checkout list
```

### 11.4 Shallow Clone（浅克隆）

```bash
# 只克隆最近一次提交
git clone --depth 1 https://github.com/user/repo.git

# 只克隆最近 n 次提交
git clone --depth 5 https://github.com/user/repo.git

# 转为完整克隆
git fetch --unshallow
```

### 11.5 Worktree

```bash
# 创建工作树
git worktree add ../feature-branch feature-branch

# 列出工作树
git worktree list

# 删除工作树
git worktree remove ../feature-branch
```

---

## 12. 常用别名

```bash
# 设置别名
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'

# 使用别名
git co main                    # 等同于 git checkout main
git br                         # 等同于 git branch
git ci -m "message"           # 等同于 git commit -m "message"
```

---

## 13. 日志美化

```bash
# 更好的日志查看工具
git log --graph --oneline --decorate

# 使用 lg2 别名
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit"

# 自定义日志格式
git log --pretty=format:"%h - %an, %ar : %s"
```

---

## 速查表

### 基础操作
```bash
git init                      # 初始化
git clone <url>               # 克隆
git status                    # 状态
git add <file>                # 添加
git commit -m "msg"            # 提交
git push                      # 推送
git pull                      # 拉取
```

### 分支操作
```bash
git branch                    # 查看分支
git branch <name>             # 创建分支
git checkout -b <name>        # 创建并切换
git branch -d <name>          # 删除分支
git merge <name>              # 合并分支
```

### 撤销操作
```bash
git restore <file>            # 撤销工作区
git restore --staged <file>   # 撤销暂存
git reset HEAD~1              # 撤销最后一次提交
git reset --hard HEAD~1       # 强制回退
```

### 远程操作
```bash
git remote -v                 # 查看远程
git remote add <name> <url>   # 添加远程
git push -u <remote> <branch>  # 推送并跟踪
git pull --rebase             # 拉取并变基
git fetch --all               # 获取所有远程更新
```

### 查看操作
```bash
git log                        # 查看日志
git log --oneline             # 简洁日志
git diff                       # 查看差异
git show <commit>              # 查看提交
git blame <file>              # 查看文件历史
```

---

## 相关文档

- [Linux 手册](../linux/README.md)
- [Docker 手册](../docker/README.md)
- [GitHub CLI](../../tools/cli/git.md)
