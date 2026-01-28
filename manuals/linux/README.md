# Linux 操作手册

> 日常开发必备的 Linux 命令与技巧

---

## 目录

- [1. 文件系统操作](#1-文件系统操作)
- [2. 进程管理](#2-进程管理)
- [3. 网络操作](#3-网络操作)
- [4. 系统监控](#4-系统监控)
- [5. 文本处理](#5-文本处理)
- [6. 权限管理](#6-权限管理)
- [7. 软件包管理](#7-软件包管理)
- [8. 日志查看](#8-日志查看)
- [9. 故障排查](#9-故障排查)
- [10. 常用技巧](#10-常用技巧)

---

## 1. 文件系统操作

### 1.1 基础操作

```bash
# 查看当前目录
pwd

# 切换目录
cd /path/to/dir
cd ~          # home 目录
cd -          # 上一个目录

# 列出文件
ls           # 简单列表
ls -la        # 详细列表（包含隐藏文件）
ls -lh        # 人类可读大小
ls -lt        # 按修改时间排序
ls --tree     # 树形结构（需要安装 tree 或用 exa）
```

### 1.2 文件操作

```bash
# 创建文件
touch file.txt
echo "content" > file.txt

# 复制文件
cp src.txt dst.txt
cp -r src_dir/ dst_dir/     # 递归复制目录

# 移动/重命名
mv old.txt new.txt
mv file.txt /path/to/dir/

# 删除文件
rm file.txt
rm -rf dir/                # 递归强制删除目录（慎用！）

# 查找文件
find . -name "*.log"         # 按名称查找
find . -type f -name "*.txt" # 只查找文件
find . -mtime -7             # 7天内修改的文件

# 更好的替代工具（推荐）
fd "pattern"                 # 比 find 快
fzf                          # 交互式查找
```

### 1.3 目录操作

```bash
# 创建目录
mkdir dir
mkdir -p path/to/dir        # 递归创建

# 删除空目录
rmdir dir

# 查看目录结构
tree -L 2                    # 显示 2 层
tree -d                       # 只显示目录
eza -T                        # 使用 eza 替代 tree
```

### 1.4 文件内容查看

```bash
# 查看文件内容
cat file.txt                 # 全部输出
less file.txt                # 分页查看（推荐）
head -n 10 file.txt          # 前 10 行
tail -n 10 file.txt          # 后 10 行
tail -f file.log             # 实时跟踪（日志常用）

# 更好的替代工具
bat file.txt                 # 语法高亮
bat -n file.txt              # 显示行号
bat -l file.txt              # 只显示语法信息
```

---

## 2. 进程管理

### 2.1 进程查看

```bash
# 查看进程
ps aux                        # 所有进程
ps aux | grep "nginx"        # 过滤进程
ps -ef                        # 完整格式

# 实时进程监控
top                           # 交互式
htop                          # 更好（推荐）
btop                          # 最推荐

# 查看特定进程
pgrep -f "nginx"              # 查找进程 PID
pidof nginx                   # 查看 PID
```

### 2.2 进程控制

```bash
# 后台运行
command &                     # 后台运行
nohup command &              # 忽略 HUP 信号
command &                     # 后台运行并继续输入

# 查看后台任务
jobs                           # 列出后台任务
fg %1                          # 前台切换任务 1
bg %1                          # 后台切换任务 1

# 终止进程
kill PID                      # 正常终止
kill -9 PID                   # 强制终止
killall nginx                 # 终止所有 nginx 进程

# 更好的替代
pkill -f "pattern"            # 按名称杀死
fzf                           # 交互式选择进程杀死
```

### 2.3 服务管理

```bash
# systemd 服务（现代 Linux）
systemctl status nginx        # 查看状态
systemctl start nginx         # 启动
systemctl stop nginx          # 停止
systemctl restart nginx       # 重启
systemctl enable nginx        # 开机启动
systemctl disable nginx       # 禁用开机启动

# 查看服务日志
journalctl -u nginx           # 查看 nginx 日志
journalctl -u nginx -f         # 实时跟踪
journalctl -u nginx --since "1 hour ago"  # 最近1小时
```

---

## 3. 网络操作

### 3.1 连接测试

```bash
# Ping
ping google.com               # 测试连通性
ping -c 4 google.com          # ping 4 次

# 查看端口
nc -zv localhost 3000        # 测试端口
lsof -i :3000                 # 查看端口占用
netstat -tuln | grep 3000    # 查看监听端口

# 更好的替代
ss -tuln                      # 现代替代 netstat
```

### 3.2 网络信息

```bash
# 查看网络接口
ifconfig                       # 传统
ip a                           # 现代（推荐）

# 查看路由
route -n                       # 传统
ip route                       # 现代

# 查看连接
netstat -antp                 # 传统
ss -antp                       # 现代（推荐）
```

### 3.3 文件传输

```bash
# SSH 连接
ssh user@host
ssh -p 2222 user@host         # 指定端口

# SCP 传输
scp local.txt user@host:/path/
scp user@host:/path/file.txt ./

# 更好的替代
rsync -avz src/ user@host:dst/  # 同步目录
```

### 3.4 下载文件

```bash
# wget
wget https://example.com/file.zip

# curl（更强大）
curl -O https://example.com/file.zip
curl -L -o output.zip https://example.com/file.zip

# 断点续传
wget -c https://example.com/large.zip
```

---

## 4. 系统监控

### 4.1 资源监控

```bash
# CPU 监控
top
htop
btop                          # 最推荐
vmstat 1                      # 每秒输出

# 内存监控
free -h                       # 人类可读
vmstat -s                     # 虚拟内存统计

# 磁盘监控
df -h                         # 磁盘使用
du -sh dir/                   # 目录大小
du -sh * | sort -h            # 按大小排序
ncdu                          # 交互式磁盘分析（推荐）

# IO 监控
iotop                         # IO 监控
iostat -x 1                   # IO 统计
```

### 4.2 系统信息

```bash
# 系统信息
uname -a                      # 系统信息
cat /etc/os-release          # 系统版本
hostname                      # 主机名
uptime                        # 运行时间

# 硬件信息
lscpu                         # CPU 信息
lsblk                         # 磁盘信息
free -h                       # 内存信息
lspci                         # PCI 设备
lsusb                         # USB 设备
```

---

## 5. 文本处理

### 5.1 搜索

```bash
# grep（基础）
grep "pattern" file.txt
grep -i "pattern" file.txt    # 忽略大小写
grep -r "pattern" .           # 递归搜索
grep -n "pattern" file.txt    # 显示行号

# ripgrep（更快，推荐）
rg "pattern"                  # 搜索当前目录
rg -i "pattern"               # 忽略大小写
rg -l "pattern"               # 只显示文件名
rg -t py "pattern"            # 只在 Python 文件搜索
rg -C 3 "pattern"             # 显示上下 3 行

# 常用组合
rg "pattern" | fzf           # 交互式搜索
rg "pattern" -l | xargs nvim # 搜索并编辑
```

### 5.2 流编辑

```bash
# sed（流编辑器）
sed 's/old/new/g' file.txt    # 替换
sed -i 's/old/new/g' file.txt # 直接修改文件
sed -n '10,20p' file.txt     # 打印 10-20 行

# 常用操作
sed 's/^#//g' file.txt       # 移除注释
sed '/pattern/d' file.txt     # 删除匹配行
sed '10d' file.txt            # 删除第 10 行
```

### 5.3 文本统计

```bash
# wc（统计）
wc -l file.txt               # 行数
wc -w file.txt               # 单词数
wc -c file.txt               # 字节数

# 排序
sort file.txt                 # 升序
sort -r file.txt              # 降序
sort -u file.txt              # 去重并排序
sort -k2 -n file.txt          # 按第 2 列数字排序

# 去重
uniq file.txt                 # 去重（需要先排序）
sort file.txt | uniq          # 组合使用
sort file.txt | uniq -c        # 统计重复次数
```

---

## 6. 权限管理

### 6.1 基础权限

```bash
# 修改权限
chmod 755 file.sh            # rwxr-xr-x
chmod +x file.sh              # 添加执行权限
chmod -R 755 dir/             # 递归修改

# 权限数字表示
# 7 = rwx (4+2+1)
# 6 = rw- (4+2)
# 5 = r-x (4+1)
# 4 = r--
# 0 = ---
```

### 6.2 文件所有者

```bash
# 修改所有者
chown user:group file.txt
chown user file.txt           # 只修改所有者
chown :group file.txt         # 只修改组
chown -R user:group dir/      # 递归修改
```

### 6.3 特殊权限

```bash
# SUID（Set User ID）
chmod 4755 file               # 用户执行时以文件所有者身份

# SGID（Set Group ID）
chmod 2755 dir                # 目录中新建文件继承目录的组

# Sticky Bit（防删除位）
chmod 1755 shared_dir/        # 只有所有者能删除自己的文件
```

---

## 7. 软件包管理

### 7.1 Debian/Ubuntu (apt)

```bash
# 更新包列表
sudo apt update

# 安装软件
sudo apt install package

# 删除软件
sudo apt remove package
sudo apt purge package        # 删除配置文件

# 更新系统
sudo apt upgrade
sudo apt dist-upgrade

# 搜索包
apt search keyword
apt list --installed          # 已安装的包

# 清理
sudo apt autoremove           # 删除不需要的包
sudo apt autoclean            # 清理缓存
```

### 7.2 CentOS/RHEL (yum/dnf)

```bash
# 更新包列表
sudo yum update

# 安装软件
sudo yum install package

# 删除软件
sudo yum remove package

# 搜索包
yum search keyword

# 清理
sudo yum autoremove
sudo yum clean all
```

### 7.3 macOS (Homebrew)

```bash
# 安装
brew install package

# 删除
brew uninstall package

# 搜索
brew search keyword

# 更新
brew update
brew upgrade
```

---

## 8. 日志查看

### 8.1 常用日志位置

```bash
# 系统日志
/var/log/syslog               # macOS
/var/log/messages             # RHEL
/var/log/syslog               # Debian

# 认证日志
/var/log/auth.log             # Ubuntu
/var/log/secure               # RHEL

# 应用日志
/var/log/nginx/               # Nginx
/var/log/mysql/               # MySQL
/var/log/app/                 # 自定义应用
```

### 8.2 日志查看技巧

```bash
# 实时跟踪
tail -f file.log              # 基础
tail -f file.log | grep "ERROR"  # 过滤
tail -f file.log | bat        # 高亮显示

# 更好的替代
bat -f file.log               # bat 不支持 -f，使用 tail -f | bat
tail -f file.log | bat -l json   # JSON 日志高亮

# 查看最近的日志
tail -n 100 file.log

# 查看特定时间的日志
grep "2024-01-10 10:" file.log

# 统计日志
rg "ERROR" file.log -c
rg "ERROR" file.log --count-matches

# 时间段过滤
rg "2024-01-10" file.log -A 10 -B 2
```

### 8.3 Journalctl（systemd 日志）

```bash
# 查看系统日志
journalctl -xe                  # 查看上次启动的错误
journalctl -u nginx             # 查看 nginx 服务日志
journalctl -u nginx -f          # 实时跟踪
journalctl --since "1 hour ago" # 最近1小时
journalctl -p err -b            # 查看所有错误（从上次启动）

# 持久化日志（重启后保留）
sudo journalctl --vacuum-time=30days
```

---

## 9. 故障排查

### 9.1 磁盘空间

```bash
# 查看磁盘使用
df -h

# 查找大文件
du -sh * | sort -rh | head -10
ncdu                          # 交互式分析

# 清理日志
journalctl --vacuum-time=7days
rm /var/log/*.gz               # 删除压缩日志
```

### 9.2 内存问题

```bash
# 查看内存使用
free -h
vmstat -s

# 查看进程内存占用
ps aux --sort=-%mem | head -10
htop                          # 交互式查看
```

### 9.3 CPU 问题

```bash
# 查看 CPU 使用
top
htop

# 查看进程 CPU 占用
ps aux --sort=-%cpu | head -10

# 查看负载
uptime
cat /proc/loadavg
```

### 9.4 网络问题

```bash
# 测试连通性
ping -c 4 google.com
traceroute google.com
mtr google.com                # 更好

# 查看端口
netstat -tuln | grep 3000
ss -tuln | grep 3000
lsof -i :3000

# DNS 问题
nslookup google.com
dig google.com

# 防火墙
sudo ufw status              # Ubuntu
sudo firewall-cmd --list-all  # CentOS
```

### 9.5 进程问题

```bash
# 查看进程状态
ps aux | grep "process"
systemctl status service
journalctl -u service -xe

# 重启服务
systemctl restart service
systemctl reload service      # 重载配置

# 清理僵尸进程
ps aux | grep Z
kill -9 PID
```

---

## 10. 常用技巧

### 10.1 命令行效率

```bash
# 自动补全
tab                           # 补全命令/路径
双击 tab                       # 列出所有可能

# 命令历史
history                        # 查看历史
Ctrl+r                         # 搜索历史命令
!!                            # 执行上一条命令
!n                            # 执行第 n 条命令

# 别名
alias ll='ls -la'
alias gs='git status'
alias nv='nvim'

# 持久化别名（写入 ~/.bashrc 或 ~/.zshrc）
echo "alias ll='ls -la'" >> ~/.bashrc
```

### 10.2 管道与重定向

```bash
# 管道（|）
cmd1 | cmd2                   # cmd1 的输出作为 cmd2 的输入
cat file.txt | grep "error"
rg "pattern" | wc -l

# 重定向（>）
cmd > file.txt                # 输出到文件（覆盖）
cmd >> file.txt               # 追加到文件
cmd 2> error.txt              # 错误输出到文件
cmd &> file.txt               # 所有输出到文件
cmd < file.txt                # 从文件读取输入

# 常用组合
rg "ERROR" logs/ | head -20
find . -name "*.log" | xargs rm
find . -type f | fzf | xargs bat
```

### 10.3 后台任务

```bash
# 后台运行
command &

# 保持后台运行
nohup command &               # 忽略 HUP 信号
nohup command > out.log 2>&1 &  # 输出到日志

# 查看后台任务
jobs

# 前后台切换
fg %1                          # 前台
bg %1                          # 后台

# 离开会话不结束
tmux                          # 终端复用器
zellij                        # 终端复用器
screen                        # 传统
```

### 10.4 快捷键

```bash
# Ctrl 快捷键
Ctrl+C                         # 中断当前命令
Ctrl+Z                         # 暂停当前命令（fg 恢复）
Ctrl+D                         # 退出当前 shell
Ctrl+L                         # 清屏
Ctrl+A                         # 光标到行首
Ctrl+E                         # 光标到行尾
Ctrl+U                         # 删除到行首
Ctrl+K                         # 删除到行尾
Ctrl+W                         # 删除一个单词
Ctrl+R                         # 搜索历史命令

# tmux 快捷键（推荐）
Ctrl+b c                       # 新建窗口
Ctrl+b n                       # 下一个窗口
Ctrl+b p                       # 上一个窗口
Ctrl+b "                       # 重命名窗口
Ctrl+b ,                       # 重命名会话
```

### 10.5 性能优化

```bash
# 使用 fd 代替 find
fd "pattern"                   # 比 find 快 10 倍

# 使用 rg 代替 grep
rg "pattern"                   # 比 grep 快 10-100 倍

# 使用 bat 代替 cat
bat file.txt                   # 语法高亮

# 使用 exa/eza 代替 ls
eza -la                        # 更好的 ls

# 使用 zoxide 代替 cd
z                             # 智能跳转

# 使用 fzf 交互式
fd | fzf                       # 交互式查找
```

---

## 11. 实用脚本

### 11.1 端口占用查找

```bash
# 查看端口占用
lsof -i :3000
netstat -tuln | grep 3000
ss -tuln | grep 3000

# 杀死占用端口的进程
kill -9 $(lsof -t -i:3000)
```

### 11.2 批量重命名

```bash
# 批量修改扩展名
rename .txt .md *.txt

# 或使用循环
for f in *.txt; do mv "$f" "${f%.txt}.md"; done
```

### 11.3 清理缓存

```bash
# 清理 apt 缓存
sudo apt clean
sudo apt autoclean

# 清理 yarn 缓存
yarn cache clean

# 清理 Docker
docker system prune -a
```

---

## 12. 系统维护

### 12.1 定期任务

```bash
# 查看当前用户的定时任务
crontab -l

# 编辑定时任务
crontab -e

# 系统级定时任务
sudo crontab -e
ls /etc/cron.*
```

### 12.2 系统更新

```bash
# Ubuntu/Debian
sudo apt update
sudo apt upgrade
sudo apt autoremove

# CentOS/RHEL
sudo yum update
sudo yum upgrade

# macOS
brew update
brew upgrade
brew cleanup
```

---

## 速查表

### 文件操作
```bash
ls                          # 列出文件
cd dir                      # 切换目录
pwd                         # 当前目录
cp src dst                  # 复制
mv src dst                  # 移动
rm file                     # 删除
mkdir dir                   # 创建目录
rmdir dir                   # 删除空目录
```

### 文件查看
```bash
cat file                    # 查看全部
less file                   # 分页查看
head -n 10 file             # 前 10 行
tail -n 10 file             # 后 10 行
tail -f file                # 实时跟踪
bat file                    # 语法高亮
```

### 进程管理
```bash
ps aux                      # 查看进程
top                         # 实时监控
htop                        # 更好的监控
kill PID                    # 终止进程
killall name                # 终止所有
bg/fg                       # 后台/前台
```

### 系统监控
```bash
df -h                       # 磁盘使用
free -h                     # 内存使用
du -sh dir                  # 目录大小
uptime                      # 运行时间
```

### 搜索
```bash
rg "pattern"                # 搜索代码
find . -name "*.log"         # 查找文件
fd "pattern"                # 查找文件（更快）
fzf                         # 交互式查找
```

---

## 相关文档

- [Git 手册](../git/README.md)
- [Docker 手册](../docker/README.md)
- [Kubernetes 手册](../kubernetes/README.md)
- [ripgrep 指南](../../tools/cli/ripgrep.md)
- [fzf 指南](../../tools/cli/fzf.md)
