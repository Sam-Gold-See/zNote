# Linux

## Linux 基础知识

### Linux 目录结构

#### 与 Win 系统的差别

| 项目\类别        | Windows 系统  | Linux 系统 |
| ---------------- | ------------- | ---------- |
| 默认管理员账号   | Administrator | root       |
| 系统分区         | C:\           | /          |
| 数据盘的分区格式 | NTFS          | XFS        |
| 虚拟内存         | swapfile.sys  | SWAP 分区  |
| 路径分隔符号     | \             | /          |

#### 倒挂的树型结构

Linux 目录结构：是个倒挂的树型结构

`/bin` 存放二进制可执行文件，常用命令一般都在这里

`/etc` 存放系统管理和配置文件

`/home` 存放所有普通用户的家目录

`/usr` 存放系统应用程序

`/opt` 额外安装的可选应用程序包所放置的位置。比如，我们可以把 tomcat 等都
安装到这里

`/proc` 虚拟文件系统目录，是系统内存的映射，相当于是存储内存中的信息

`/root` 管理员的家目录

`/sbin` 存放二进制可执行文件，只有 root 才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如 ifconfig 等

`/dev` 用于存放设备文件

`/mnt` 系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统

`/boot` 存放用于系统引导时使用的各种文件

`/lib` 存放跟文件系统中的程序运行所需要的共享库及内核模块

`/tmp` 用于存放各种临时文件
`/var` 用于存放各种服务的日志文件、系统启动日志等

#### 磁盘与分区表示

在 linux 系统中一切皆文件

- 名称规则

  - hd,表示 IDE 设备

  - sd,表示 SCSI 设备（目前较多）

  - vd,表示虚拟设备 （虚拟机中可以见到）

### 基础命令使用

```
[root@localhost ~]#
[当前用户@主机名 当前所在位置]用户权限
```

#### 用户权限

- `$` 表示普通用户权限

- `#` 表示超级用户权限

#### 文件颜色表示含义

- 蓝色表示**目录**

- 绿色表示**可执行文件**

- 红色表示**压缩文件**

- 浅蓝色表示**链接文件**

- 白色表示**其他文件**

- 黄色是**设备文件**

- 黑色代表**文本文件**

### 查看及切换目录

**绝对路径**：以`\`开始（根目录）

**相对路径**：以当前目录为基础

- `pwd` -> Print Working Directory：查看当前工作目录，显示当前所在的位置

- `cd` -> Change Directory：切换工作目录

- `ls` -> List Directory：查看当前目录内容

  - 格式：`ls [选项]... [目录或文件]...`

  - 常用命令选项：`-l`：以长格式显示（显示详细信息）

  - `[root@localhost /]# ls -l /root` 显示/root 目录内容详细信息

  - `[root@localhost /]# ls -l /` 显示根目录内容详细信息

  - `[root@localhost /]# ls -lh /etc/passwd` 查看文件详细信息并加易读单位

  - `[root@localhost /]# ls -lA /boot/` 查看 boot 目录的所有文件

- `cat` -> Concatenate Files：查看文本文件内容

- `less` -> Less：分屏工具

- `lscpu` -> Display CPU information：查看 CPU 处理器信息

- `ifconfig` -> Display Network Interface information：查看网络接口信息

  - lo：本地回环地址 127.0.0.1 永远代表自己

- `Ctrl + C` -> 停止正在运行的命令

- `mkdir` -> Make Directory：创建目录

  - `mkdir [/path/]目录名` 在指定目录下创建目录

- `touch` -> Create an empty file：创建空文件

  - `touch [/path/]文件名` 在指定目录下创建空文件

- `grep` -> Global Regular Expression Print：全局正则表达式搜索，输出包含指定字符的行

  - `grep '查找条件' 目标文件`

- `head` -> Output the first part of files：显示文件开头内容

  - `head -n 文件名` 显示文件开头内容 n 行

- `tail` -> Output the last part of files：显示文件末尾内容

  - `tail -n 文件名` 显示文件末尾内容 n 行

- `poweroff` -> Shut down the system：关闭系统

- `reboot` -> Restart the system：重启系统

### 快速编辑技巧

`Tab` 可以补全一些命令、选项、参数、文件路径、软件名、服务名

`Ctrl + L` 清屏

`Ctrl + C` 暂停当前执行的命令/废弃当前编辑的内容

`Alt + .` 或 `Esc + .` 或 方向键上 ，黏贴上一个命令的参数

### 命令行基础

Linux 命令：用来实现某一类功能的指令或程序；命令的执行依赖于解释器（例如：/bin/bash）

内部命令：属于解释器的一部分

外部命令：解释器之外的其他程序

命令行格式：`命令字 [选项]... [参数]...`

## Linux 作业操作

### Linux 发行版与开源软件

#### Linux 发行版

- Red Hat Enterprise Linux 7/8/9
- CentOS 7/8
- Suse Linux Enterprise 14
- Debian Linux
- Ubuntu Linux 15.04/13.10
- Oracle Linux

#### 开源软件

- Apache HTTP Server
- MySQL Database
- PHP
- Libre Office
- Firefox
- Nginx

### Linux 根目录与 /dev 目录的作用

- `/`：Linux 文件系统的起点，Linux 所有的文件都放在其中。
- `/dev`：存放硬盘、键盘、鼠标、光驱等各种设备文件。

### Linux 系统中磁盘的表示方法

- 第 2 块 SCSI 接口的硬盘被表示为 `/dev/sdb`
- 此硬盘中的第 3 个主分区表示为 `/dev/sdb3`
- 第 4 个逻辑分区表示为 `/dev/sdb8`

### Linux 中默认的文件系统

- EXT3：第 3 代扩展（Extended）文件系统，RHEL 5.x 系列之前默认。
- EXT4：第 4 代扩展（Extended）文件系统，RHEL 6.x 系列默认。
- XFS：日志文件系统，RHEL 7 和 8 系列默认文件系统。
- SWAP：交换文件系统，起虚拟内存的作用。

### 控制台的切换

在 RHEL 系统的 TTY1 控制台中，可以按 `Ctrl+Alt+F2` 切换到 TTY2 控制台。

### 查看当前 Linux 系统的内核版本

```
[root@localhost ~]# uname -r
3.10.0-957.el7.x86_64
```

### 查看当前 Linux 主机的第一块网卡的 IP 地址

```
[root@localhost ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.147.128  netmask 255.255.255.0  broadcast 192.168.147.255
        inet6 fe80::91b:33b:6736:f3f  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:3e:9f:29  txqueuelen 1000  (Ethernet)
        RX packets 350602  bytes 518959134 (494.9 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 141431  bytes 8827802 (8.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

### 查看当前 Linux 主机的 CPU 和内存信息

```
[root@localhost ~]# cat /proc/meminfo           # 查看内存信息
MemTotal:        3861508 kB
MemFree:         1325248 kB
MemAvailable:    3309832 kB
Buffers:            4172 kB
Cached:          2135668 kB
...

[root@localhost ~]# cat /proc/cpuinfo           # 查看 CPU 信息
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 151
model name      : 12th Gen Intel(R) Core(TM) i5-12490F
...
```

### 查看根目录内容

```
[root@localhost ~]# ls /
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```

### Linux 中延迟重启的命令

RHEL 服务器 30 分钟后自动重启。

```
[root@localhost ~]# shutdown -c +30
Broadcast message from root@localhost.localdomain (Fri 2024-12-13 11:01:26 CST):
+30
The system shutdown has been cancelled at Fri 2024-12-13 11:02:26 CST!
```

### Linux 目录结构与文件操作

#### Linux 下颜色的作用

- 蓝色代表 **目录**

- 黑色代表 **文件**

#### Linux 目录结构的含义

- `/root`：超级管理员的家目录

- `/home`：存放所有普通用户的家目录

- `/dev`：用于存放设备文件

#### `cd ..` 代表

返回上一级目录

#### 绝对路径、相对路径的含义

- 绝对路径：以根目录起始的路径

- 相对路径：以当前路径为参照的路径

#### 查看当前工作目录下内容

`ls`

#### 显示/boot 目录内容

`ls /boot`

#### 显示/opt 目录内容

`ls /opt`

#### 在当前目录创建名字为 opt 的目录

`mkdir opt`

#### 切换到当前目录中的 opt 目录

`cd opt`

#### 切换到/opt 目录

`cd /opt`

#### 在当前目录创建 nsd2112 目录

`mkdir nsd2112`

#### 在/opt 目录下创建 nsd.txt 文件

`touch /opt/nsd.txt`

#### 请问同一个目录下可以存在同名的文件和目录吗

不可以

#### 删除当前目录下的 opt 目录

`rm -rf opt`

#### 把/opt 目录下的 nsd.txt 文件移动到/root 目录下

`mv /opt/nsd.txt /root`

#### 使用绝对路径和相对路径的方式删除 nsd.txt 文件

- `rm -rf /opt/nsd.txt`

- `cd /opt ; rm -rf nsd.txt`

#### 请给 10 分钟后关机起一个 byebye 的别名，并测试

`alias byebye='shutdown –h +10'`

#### 过滤/etc/passwd 文件中包含 root 的行，并把结果写入到/opt/root.txt 文件中。并检查该文件内容

```
grep 'root' /etc/passwd > /opt/root.txt
cat /opt/root.txt
```

#### 请统计出/boot/目录下，快捷方式的文件有多少个

`find /boot/ -type l|wc -l`

#### 请将/boot/目录下的以 vm 开头且必须是文件的文件，拷贝到以自己名字名命的文件夹下，并统计出有多少个

```
find /boot/ -name 'vm*' -a –type f | wc –l
mkdir /susu
find /boot/ -name 'vm*' -a –type f | wc –l -exec cp {} /susu \;
```

### Linux 文件查看、操作和管理命令

#### 查看文件内容常用命令

查看文本文件的内容，可使用以下命令，各自的特点如下：

- **cat**: 直接输出文件的所有内容

- **head**: 查看文件开头指定行数的内容

- **tail**: 查看文件末尾指定行数的内容

- **less 或 more**: 实现分页查看文件内容

- **grep**: 根据指定的条件检索文件，只输出符合条件的行

#### 截取文件内容与重定向操作

截取 `/etc/passwd` 文件首两行内容写入到 `/root/nsd.txt`，截取 `/etc/sysconfig/network` 最后一行内容追加写入到 `/root/nsd.txt`。

```
[root@svr5 /]# head -n 2 /etc/passwd > /root/nsd.txt
[root@svr5 /]# tail -n 1 /etc/sysconfig/network >> /root/nsd.txt
```

#### Linux 中管道“|”的作用

将前一个命令的输出结果，交由后面命令处理，当做后面命令的参数。

#### wc 常见的应用

统计 `/etc/login.defs` 配置文件有效配置的行数。

```
[root@svr5 /]# grep -v ^# /etc/login.defs | grep -v ^$ | wc -l
```

#### grep 常见的应用

提取出 `/etc/yum.conf` 文件的有效配置记录，即去掉空行、注释行。

```
[root@svr5 ~]# grep -vE "^#|^$" /etc/yum.conf
```

#### Linux 中的压缩与打包

Linux 中常见的压缩包格式及其相关命令工具如下：

- `.gz`: gzip

- `.bz2`: bzip2

- `.zip`: zip、unzip

- `.tar.gz` 或 `.tgz`: tar+gzip

- `.tar.bz2`: tar+bzip2

#### Linux 中打包常用选项

对于 `.tar.gz` 格式的归档压缩包，使用 `tar` 命令释放时选项可设为 `zxf` 或 `zxvf`；若创建这种类型的备份文件，使用 `tar` 命令制作时应使用 `zcf` 或 `zcvf` 选项。

#### Linux 中打包备份应用

使用 `tar` 备份整个 `/boot` 文件夹，制作成 `.tar.gz` 格式的压缩包，保存到 `/opt/` 目录下。

```
[root@svr5 ~]# tar zcf /opt/boot-backup.tar.gz /boot
```

#### Linux 中常见的搜索

变量 `PATH` 作用是命令的解释器。查看 `PATH` 变量的命令为 `echo $PATH`。

#### locate 与 which 命令

使用 `locate` 工具检索文件之前，应通过 `updatedb` 命令更新索引数据库；而使用 `which` 命令查找可执行文件时，其查找范围由环境变量 `PATH` 决定。

#### 简述 find 命令的格式及常见用法

命令格式：`find [查找范围] [查找条件]`
常见的查找条件设置：

- `-type`: 按文件类型查找

- `-name`: 按文件名称查找

- `-size`: 按文件大小查找

- `-mtime`: 按内容修改的时间

#### find 命令的应用

查找 `/usr/sbin/` 目录下大小超过 1MB 的所有程序文件。
列出 `/root` 目录下最近 15 天内改动过的文件（不包括目录）。
查找 `/boot` 目录下的连接文件，并以显示详细属性。

```
[root@svr5 ~]# find /usr/sbin -size +1M
[root@svr5 ~]# find /root -type f -a -mtime -15
[root@svr5 /]# find /boot/ -type l -exec ls -l {} \;
```

#### gzip 压缩、解压缩及 grep 查找应用

```
[root@localhost ~]# tar -czf /opt/abc.tar.gz /boot/ /home/
[root@localhost ~]# tar -xf /opt/abc.tar.gz -C /abc/
[root@localhost ~]# grep root /etc/passwd
```

#### grep 查找应用

```
[root@localhost ~]# grep ^root /etc/passwd
[root@localhost ~]# grep root$ /etc/passwd
```

#### vim 编辑器开启行号

末行模式下：

- `:set nu` 显示行号
- `:set nonu` 取消行号

#### mount 挂载操作的作用

让目录成为设备（光盘、硬盘等）的访问点，通过系统的安装光盘获得软件包。

#### 挂载操作示例

将 `/dev/cdrom` 挂载到 `/dvd/` 目录下。

```
[root@localhost ~]# mount /dev/cdrom /dvd/
```

#### rpm 命令常用的选项

- `-a`: 列出已安装所有软件包
- `-i`: 查看指定软件的详细信息
- `-l`: 查看指定软件的文件安装清单

#### rpm 命令应用示例

```
[root@localhost ~]# rpm -q firefox
[root@localhost ~]# rpm -qi firefox
[root@localhost ~]# rpm -qa
[root@localhost ~]# rpm -qa | wc -l
```

#### 查询命令帮助信息

```
ls --help
```

## 问题记录

### CentOS7 配置网络

先检查 VMware 网络，在 VM 界面左上角编辑处点击`虚拟网络编辑器`，点击`更改设置`提供管理员权限,查看 NAT 模式的 VMnet8 的`NAT设置`，记录子网 IP、子网掩码、网关 IP 三者数据保存退出

然后登录 CentOS 的 root 账号

输入如下指令进行配置网络

`vi /etc/sysconfig/network-scripts/ificg-ens33`

使用`i`指令进入 INSERT 编辑模式

对于`BOOTPROTO`属性的值更改为`static`

对于`ONBOOT`属性的值更改为`yes`

在文档末尾添加如下属性

```
IPADDR=XXX.XXX.XXX.A #前三者属性和子网IP一致，最后一位改为其他数
GATEWAY=XXX.XXX.XXX.XXX #全部属性和网关IP一致
NETMASK=XXX.XXX.XXX.XXX #全部属性和子网掩码一致
DNS1=8.8.8.8
DNS2=8.8.4.4
```

后按`esc`键退出 INSERT 模式，键入`:wq`指令保存（`w`）并且退出（`q`）

再输入如下指令配置网关 IP

`vi /etc/resolv.conf`

对于该空文档同样进入 INSERT 模式

输入如下内容

```
# Generated by NetworkManager
nameserver XXX.XXX.XXX.XXX #输入内容为网关IP
```

同样保存并且退出

若如上步骤无出错则配置成功，接下来进行验证

使用`ping www.baidu.com`指令，若没有报错则代表配置成功，使用`Ctrl + C`按键退出 ping 指令
