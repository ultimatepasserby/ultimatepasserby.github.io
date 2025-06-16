---
title: "Linux学习笔记"
date: 2025-06-15  # 可选，手动指定日期
layout: default      # 可选，指定布局（如 `post` 或 `default`）
categories: [Linux, Shell]  # 可选，添加分类
---
# Linux 学习笔记

## 一、Linux 基础概念
### 1. 操作系统核心
#### 内核（Kernel）
内核是 Linux 操作系统的核心，负责管理硬件资源，如内存、CPU、设备 I/O 等，同时提供系统服务，如文件管理、进程调度等。
**例子**：当我们运行一个程序时，内核会为该程序分配内存和 CPU 资源，确保程序能够正常运行。

#### 用户态与内核态
- **用户态**：运行应用程序，权限受限，不能直接操作硬件。
- **内核态**：执行特权指令，直接操作硬件。
- **切换方式**：系统调用、异常、中断。
**例子**：当我们在用户态下执行 `read` 系统调用读取文件时，会触发系统调用，使程序从用户态切换到内核态，由内核完成文件读取操作，然后再切换回用户态。

### 2. Linux 发行版分类
| **类型** | **代表系统** | **包管理器** |
| ---- | ---- | ---- |
| Debian 系 | Ubuntu | `apt`/`apt-get` |
| Red Hat 系 | CentOS, Fedora | `yum`/`dnf` |
| Arch 系 | Arch Linux, Manjaro | `pacman` |

**例子**：如果我们使用的是 Ubuntu 系统，要安装 Nginx 服务器，可以使用以下命令：
```bash
sudo apt install nginx
```

## 二、文件系统与目录结构
### 1. 核心原则
#### 一切皆文件
在 Linux 中，硬件设备、目录、进程等均抽象为文件。例如，`/dev/sda` 表示磁盘。
**例子**：我们可以使用 `ls` 命令查看 `/dev` 目录下的设备文件：
```bash
ls /dev
```

#### inode 机制
inode 存储文件元数据，如权限、大小、时间戳等，可以通过 `stat` 命令查看。
**例子**：查看 `test.txt` 文件的 inode 信息：
```bash
stat test.txt
```

### 2. 目录结构解析
| **目录** | **作用** |
| ---- | ---- |
| `/bin` | 基础命令（`ls`, `cat`） |
| `/etc` | 配置文件（`/etc/hostname` 改主机名） |
| `/home` | 用户主目录 |
| `/var` | 动态数据（日志文件 `/var/log`） |
| `/proc` | 虚拟文件系统，映射内核实时状态（如 `/proc/cpuinfo`） |

**例子**：查看当前系统的 CPU 信息：
```bash
cat /proc/cpuinfo
```

### 3. 文件类型与权限
#### 文件类型标识
- `-`：普通文件
- `d`：目录
- `l`：符号链接（软链接）
- `c`/`b`：字符/块设备文件

#### 权限管理
- 字符权限：`r`（读，4）、`w`（写，2）、`x`（执行，1）。
- 命令：`chmod u+x file`（添加用户执行权限）或 `chmod 755 file`。

**例子**：给 `test.sh` 文件添加用户执行权限：
```bash
chmod u+x test.sh
```

## 三、常用命令详解
### 1. 文件与目录操作
#### 增删改查
```bash
mkdir -p dir1/dir2    # 递归创建目录
find /home -name "*.log"  # 查找日志文件
cp -r dir1 dir2      # 递归复制目录
rm -rf tmp           # 强制删除非空目录
```

**例子**：在 `/tmp` 目录下递归创建 `test/dir` 目录：
```bash
mkdir -p /tmp/test/dir
```

#### 查看内容
- `cat`：全量输出
- `tail -f log.txt`：实时追踪日志。

**例子**：实时追踪 `app.log` 文件的日志：
```bash
tail -f app.log
```

### 2. 系统管理命令
#### 进程管理
```bash
ps aux | grep nginx  # 查看 Nginx 进程
top                 # 动态监控资源占用
kill -9 PID         # 强制终止进程
```

**例子**：查看所有 Nginx 进程：
```bash
ps aux | grep nginx
```

#### 服务管理
`systemctl start nginx`

**例子**：启动 Nginx 服务：
```bash
sudo systemctl start nginx
```

### 3. 网络工具
- 配置 IP：`ip addr add 192.168.1.10/24 dev eth0`
- 诊断工具：`ping`、`traceroute`、`netstat`。

**例子**：测试与 `www.google.com` 的网络连接：
```bash
ping www.google.com
```

## 四、系统管理与安全
### 1. 包管理器
#### 安装软件
- Ubuntu：`sudo apt install nginx`
- CentOS：`sudo yum install httpd`。

**例子**：在 CentOS 系统上安装 Apache 服务器：
```bash
sudo yum install httpd
```

### 2. 防火墙配置
```bash
firewall-cmd --add-port=80/tcp --permanent  # 开放 80 端口
firewall-cmd --reload                      # 重载配置
```

**例子**：开放 443 端口并重新加载防火墙配置：
```bash
firewall-cmd --add-port=443/tcp --permanent
firewall-cmd --reload
```

### 3. 安全实践
- 定期更新：`sudo apt update && sudo apt upgrade`。
- 密钥登录：
```bash
ssh-keygen -t ed25519                   # 生成密钥
ssh-copy-id user@host                   # 部署公钥
```

**例子**：生成 ed25519 密钥并将公钥部署到远程服务器：
```bash
ssh-keygen -t ed25519
ssh-copy-id user@192.168.1.100
```

## 五、Shell 编程与脚本
### 1. 基础语法
#### 变量
```bash
name="Linux"
```

#### 条件判断
```bash
if [ -f file.txt ]; then
    echo "文件存在"
fi
```

#### 循环
```bash
for i in {1..5}; do echo $i; done
```

**例子**：判断 `test.txt` 文件是否存在，如果存在则输出提示信息：
```bash
if [ -f test.txt ]; then
    echo "test.txt 文件存在"
fi
```

### 2. 输入输出重定向
- `command > output.log`：覆盖输出到文件。
- `command 2>&1`：合并标准错误与标准输出。

**例子**：将 `ls` 命令的输出覆盖到 `output.log` 文件中：
```bash
ls > output.log
```

## 六、磁盘与存储管理
### 1. 分区与格式化
```bash
fdisk /dev/sdb           # 分区操作
mkfs.ext4 /dev/sdb1      # 格式化为 ext4
mount /dev/sdb1 /mnt/data # 挂载到目录
```

**例子**：对 `/dev/sdb` 磁盘进行分区，将第一个分区格式化为 ext4 并挂载到 `/mnt/data` 目录：
```bash
fdisk /dev/sdb
mkfs.ext4 /dev/sdb1
mount /dev/sdb1 /mnt/data
```

### 2. 逻辑卷管理（LVM）
步骤：物理卷（PV）→ 卷组（VG）→ 逻辑卷（LV）。

### 3. Swap 交换空间
作用：扩展虚拟内存，防止物理内存不足。

## 七、开发环境部署
### 1. Git 配置
#### 关联 GitHub
```bash
git config --global user.email "your@email.com"
ssh -T git@github.com      # 测试 SSH 连接
```

**例子**：配置全局 Git 用户邮箱并测试与 GitHub 的 SSH 连接：
```bash
git config --global user.email "test@example.com"
ssh -T git@github.com
```

### 2. 文本编辑器 Vim
#### 三种模式
- 命令模式：移动光标、删除字符（`x`）。
- 输入模式（`i` 进入）：编辑文本。
- 末行模式（`:`）：保存（`:w`）、退出（`:q!`）。

**例子**：使用 Vim 打开 `test.txt` 文件，进入输入模式编辑文件内容，然后保存并退出：
```bash
vim test.txt
i  # 进入输入模式
# 编辑文件内容
Esc  # 退出输入模式
:wq  # 保存并退出
```

## 八、推荐资源
- 命令查询：[Linux 命令大全](http://man.linuxde.net/)
- 实战项目：[Linux-Tutorial](https://github.com/judasn/Linux-Tutorial)（含 Zookeeper、Docker 配置）。

## 补充内容

### 补充一：基础命令分类详解
#### 1. 文件与目录操作命令
| **命令** | **作用** | **常用参数** | **示例** |
| ---- | ---- | ---- | ---- |
| `ls` | 查看目录内容 | `-l`(详情) `-a`(隐藏文件) | `ls -la /etc` |
| `cd` | 切换目录 | `~`(家目录) `..`(上级) | `cd ~/projects` |
| `pwd` | 显示当前路径 | - | `pwd` |
| `touch` | 创建空文件/更新文件时间戳 | - | `touch test.txt` |
| `cp` | 复制文件/目录 | `-r`(递归) `-i`(交互) | `cp -r dir1 dir2` |
| `mv` | 移动/重命名文件 | `-f`(强制) | `mv old.txt new.txt` |
| `rm` | 删除文件 | `-r`(递归) `-f`(强制) | `rm -rf tmp/` (⚠️谨慎使用) |
| `find` | 文件搜索 | `-name`(名称) `-type`(类型) | `find / -name "*.conf"` |

#### 2. 文本查看与处理命令
| **命令** | **用途** | **关键技巧** |
| ---- | ---- | ---- |
| `cat` | 显示整个文件内容 | `cat file1 > file2` (覆盖重定向) |
| `head` | 显示文件头部 | `head -n 10 log.txt` (前 10 行) |
| `tail` | 显示文件尾部 | `tail -f app.log` (实时追踪日志) |
| `grep` | 文本搜索 | `grep "error" log.txt -C 3` (显示匹配行前后 3 行) |
| `awk` | 文本分析处理 | `awk '{print $1}' data.txt` (打印第一列) |
| `sed` | 流编辑器 | `sed 's/old/new/g' file` (全局替换) |

#### 3. 压缩与解压命令
```bash
tar -czvf archive.tar.gz dir/    # 压缩目录为 gzip 格式
tar -xzvf archive.tar.gz         # 解压 gzip 压缩包
unzip file.zip                   # 解压 zip 文件
```

### 补充二：用户与权限管理
#### 1. 用户/组管理命令
| **命令** | **功能** | **关键参数** | **示例** |
| ---- | ---- | ---- | ---- |
| `useradd` | 创建用户 | `-m`(创建家目录) `-s`(指定 Shell) | `useradd -m -s /bin/bash john` |
| `passwd` | 修改用户密码 | - | `passwd john` |
| `usermod` | 修改用户属性 | `-aG`(追加用户组) | `usermod -aG sudo john` |
| `userdel` | 删除用户 | `-r`(同时删除家目录) | `userdel -r john` |
| `groupadd` | 创建用户组 | - | `groupadd dev_team` |
| `groups` | 查看用户所属组 | - | `groups john` |

#### 2. 权限控制进阶
#### 权限继承机制
```bash
chmod g+s /shared_dir    # 设置目录的 SetGID 位，新文件继承父目录组
setfacl -m u:tom:rwx file # 通过 ACL 给单独用户授权
```

#### 特殊权限位
- `SetUID`(4)：允许用户以文件所有者权限执行（如 `/usr/bin/passwd`）
- `SetGID`(2)：目录中新文件继承组身份
- `Sticky Bit`(1)：防删除位（如 `/tmp` 目录）

```bash
chmod 4755 /usr/bin/myapp  # 设置 SetUID 位
```

### 补充三：进程与系统监控
#### 1. 进程管理命令
| **命令** | **功能** | **场景** |
| ---- | ---- | ---- |
| `ps` | 查看进程快照 | `ps aux \| grep nginx` |
| `top` | 动态资源监控（类似任务管理器） | 按 `P` 按 CPU 排序，按 `M` 按内存排序 |
| `htop` | 增强版 top（需安装） | 支持鼠标操作、树状视图 |
| `kill` | 终止进程 | `kill -15 PID`（优雅停止） |
| `pkill` | 按进程名终止 | `pkill -f "python app.py"` |
| `nohup` | 后台运行并忽略挂断信号 | `nohup ./start.sh &` |

#### 2. 系统资源查看
```bash
free -h          # 查看内存使用（-h 人性化单位）
df -Th           # 查看磁盘空间（-T 文件系统类型）
uptime           # 系统运行时间及平均负载
```

### 补充四：环境变量与启动配置
#### 1. 环境变量操作
```bash
echo $PATH              # 查看 PATH 变量
export JAVA_HOME=/opt/jdk  # 临时设置变量
echo 'export PATH=$PATH:/new_dir' >> ~/.bashrc  # 永久生效
source ~/.bashrc        # 立即加载配置
```

#### 2. 开机启动项管理
- Systemd 系统：`systemctl enable nginx`
- 传统 SysV：`update-rc.d apache2 defaults`

### 补充五：网络配置进阶
#### 1. 网络诊断工具
```bash
ss -tunlp            # 替代 netstat，查看端口占用
tcpdump -i eth0 port 80  # 抓取 80 端口数据包
mtr google.com       # 网络路由跟踪（结合 ping+traceroute）
```

#### 2. 防火墙深度配置
```bash
# 允许特定 IP 访问 22 端口
firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.1.100" port port="22" protocol="tcp" accept'
```

### 补充六：Shell 脚本实战案例
#### 自动备份脚本 (`backup.sh`)
```bash
#!/bin/bash
# 定义变量
BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d)
USER="mysql"

# 创建备份目录
mkdir -p $BACKUP_DIR/$DATE

# 备份 MySQL 数据库
mysqldump -u $USER -p'password' mydb > $BACKUP_DIR/$DATE/mydb.sql

# 压缩并删除 7 天前备份
gzip $BACKUP_DIR/$DATE/mydb.sql
find $BACKUP_DIR -type d -mtime +7 | xargs rm -rf
```

### 附录
#### 1. Linux 命令行常用快捷键
| 快捷键 | 功能说明 |
| ---- | ---- |
| tab | 自动补全命令或路径 |
| Ctrl+a | 将光标移动到命令行行首 |
| Ctrl+e | 将光标移动到命令行行尾 |
| Ctrl+f | 将光标向右移动一个字符 |
| Ctrl+b | 将光标向左移动一个字符 |
| Ctrl+k | 剪切从光标到行尾的字符 |
| Ctrl+u | 剪切从光标到行首的字符 |
| Ctrl+w | 剪切光标前面的一个单词 |
| Ctrl+y | 复制剪切命名剪切的内容 |
| Ctrl+c | 中断正在执行的任务 |
| Ctrl+h | 删除光标前面的一个字符 |
| Ctrl+d | 退出当前命令行 |
| Ctrl+r | 搜索历史命令 |
| Ctrl+g | 退出历史命令搜索 |
| Ctrl+l | 清除屏幕上所有内容在屏幕的最上方开启一个新行 |
| Ctrl+s | 锁定终端使之暂时无法输入内容 |
| Ctrl+q | 退出终端锁定 |
| Ctrl+z | 将正在终端执行的任务停下来放到后台 |
| !! | 执行上一条命令 |
| !数字 | 执行数字对应的历史命令 |
| !字母 | 执行最近的以字母打头的命令 |
| !$ / Esc+. | 获得上一条命令最后一个参数 |
| Esc+b | 移动到当前单词的开头 |
| Esc+f | 移动到当前单词的结尾 |

#### 2. man 查阅命令手册的内容说明
| 手册中的标题 | 功能说明 |
| ---- | ---- |
| NAME | 命令的说明和介绍 |
| SYNOPSIS | 使用该命令的基本语法 |
| DESCRIPTION | 使用该命令的详细描述，各个参数的作用，有时候这些信息会出现在 OPTIONS 中 |
| OPTIONS | 命令相关参数选项的说明 |
| EXAMPLES | 使用该命令的参考例子 |
| EXIT STATUS | 命令结束的退出状态码，通常 0 表示成功执行 |
| SEE ALSO | 和命令相关的其他命令或信息 |
| BUGS | 和命令相关的缺陷的描述 |
| AUTHOR | 该命令的作者介绍 |