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

#### 基础命令使用

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

#### 查看及切换目录

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

- `date` -> Display the current date and time：显示当前日期和时间

  - `date -s 'yyyy-mm-dd hh:mm:ss'` -> 修改时间

- `echo` -> Display a line of text：显示一行文字

- `poweroff` -> Shut down the system：关闭系统

- `reboot` -> Restart the system：重启系统

#### 快速编辑技巧

`Tab` 可以补全一些命令、选项、参数、文件路径、软件名、服务名

`Ctrl + L` 清屏

`Ctrl + C` 暂停当前执行的命令/废弃当前编辑的内容

`Alt + .` 或 `Esc + .` 或 方向键上 ，黏贴上一个命令的参数

#### 命令行基础

Linux 命令：用来实现某一类功能的指令或程序；命令的执行依赖于解释器（例如：/bin/）

内部命令：属于解释器的一部分

外部命令：解释器之外的其他程序

命令行格式：`命令字 [选项]... [参数]...`

### Linux 目录文件的基本管理

#### 查看及切换目录

- `pwd` -> Print Working Directory：查看当前工作目录

- `cd` -> Change Directory：切换工作目录

- `.` -> 当前目录

- `..` -> 上一级目录

- `~` -> 表示用户的家目录

- `~用户名` -> 表示指定用户的家目录

#### ls 列出目录内容/文件属性

- `ls` -> List Directory：查看当前目录内容

  - 格式：`ls [选项]... [目录或文件]...`

  - 常用命令选项：

    - `ls -l`：以长格式显示（显示详细信息）

    - `ls -a`：显示包括名称以 . 开头的隐藏文件

    - `ls -ld`：显示目录本身的属性

    - `ls -lh`：提供易读的容量单位（K、M）

    - `ls -R`：递归显示目录下的内容

#### 命令别名操作

- `alias` -> Create an alias for a command：创建命令别名

- `unalias` -> Delete an alias：删除命令别名

#### 创建/删除/移动目录

- `mkdir` -> Make Directory：创建目录

  - 格式：`mkdir [-p] [/path/]目录名`

- `rm` -> Remove（删除）：删除文件或目录

  - 格式：`rm [选项] 文件或目录`

  - 常用命令选项：

    - `-r`：递归删除目录及其内容

    - `-f`：强制删除文件或目录，无需确认

- `mv` -> Move（移动）：移动或重命名文件或目录

  - 格式：`mv [选项] 源文件或目录 目标文件或目录`

#### 复制与通配符

- `cp` -> Copy（复制）：复制文件或目录

  - 格式：`cp [选项] 源文件或目录 目标文件或目录`（复制支持两个以上参数，永远把最后一个参数作为目标目录）

  - 常用命令选项：

    - `-r`：递归复制目录及其内容

    - `-f`：强制覆盖已存在的文件或目录，无需确认

    - `-p`：保持原文件属性不变

- 通配符：针对不确定的文档名称，以特殊字符表示

  - `*`：任意多个任意字符

  - `?`：单个字符

  - `[a-z]`：多个字符或连续范围中的一个

  - `{a,min,xy}`：多组不同的字符串全匹配

#### 重定向与管道操作

- **重定向输出**：将前面命令的输出结果写入到文本文件

  - `>`：覆盖重定向

  - `>>`：追加重定向

- **管道操作**：将前面命令的输出结果，传递给后面命令当做后面命令的参数

  - `|`：管道操作符

#### find 精确查找

- 根据预设的条件递归查找对应的文件（proc 目录不占硬盘空间大小，在内存上存储数据）

- `find [目录] [条件1] [-a|-o] [条件2] ...`

  - 常用条件表示：

    - `-type`：类型（f 文件**黑**，d 目录**蓝**，l 链接文件**青**）

    - `-name`：文件名

    - `-iname`：不区分大小写的文件名

    - `-size +|-`：文件大小（k、M、G），大于/小于某大小

    - `-user`：文件所有者

    - `-mtime`：文件修改时间

  - `-a`：条件 1 和条件 2，和条件 3... 都得满足

  - `-o`：条件 1 或条件 2，或条件 3... 满足其一

- `wc`：统计文件行数、单词数、字节数、文件名

  - 常用命令选项：

    - `-l`：统计行数

    - `-w`：统计单词数

    - `-c`：统计字节数

- `-exec`：执行命令，并把匹配的文件名作为参数传递给命令

  - 格式：`find [目录] [条件] -exec [命令] {} \;`

### 压缩及归档

#### 归档和压缩

- 归档的含义：

  - 将许多零散的文件整理为一个文件

  - 文件总的大小基本不变

- 压缩的含义：

  - 按某种算法减小文件所占用空间的大小

  - 恢复时按对应的逆向算法解压

- 跨平台压缩命令

  - `zip 包名 源文件`

  - `unzip 包名 -d 解压目录`

- Linux 常见的压缩格式：

  - `gzip` -> `.gz`：快、压缩比例低

  - `bzip2` -> `.bz2`：中等

  - `xz` -> `.xz`：压缩比例高、速度慢

- 集成备份工具

  - `tar [选项] [/路径/压缩包名字] [被压缩归档的源数据] ...`

    - `-c`：创建归档

    - `-x`：释放归档

    - `-f`：指定归档文件名（必须放在所有选项的最后）

    - `-z`、`-j`、`-J`:调用 .gz、.bz2、.xz 格式的工具进行处理

    - `-t`:显示归档中的文件清单

    - `-C`(大写):指定释放的位置

  - `tar [选项] [/路径/压缩包名字] (-C) [释放的位置]`

#### grep 检索

**查找文本内容**

- 根据字符串模式提取文本行

  - `grep [选项] '匹配模式' 文件...`

  - `命令行 | grep [选项] '匹配模式'`

  - 常用命令选项：

    - `-v`：取反匹配

    - `-i`：忽略大小写

**grep 的查找条件**

- `^...`：以字符串开头

- `...$`：以字符串结尾

- `^$`：匹配空行

#### vim 文本编辑器

- 使用 vim 创建/修改文件

  - 若目标文件不存在，则新建空文件并编辑

  - 若目标文件已存在，则打开文件并编辑

- 模式：

  - 命令模式（默认模式）：按 `i` 进入插入模式，在插入模式按 `Esc` 进入命令模式

    - `G`：移动到文件末尾

    - `gg`：移动到文件开头

    - `yy`：复制（默认复制一行，nyy 复制 n 行）

    - `P`：光标的下一行粘贴

    - `dd`：删除一整行（默认删除一行，ndd 删除 n 行）

    - `u`：撤销

    - 查找关键字：

      - `/`：查找关键字，查找到存在内容会高亮显示，按 `n` 继续查找，按 `N` 反向查找，查找不存在内容会消除前面查找的高亮

  - 插入模式（输入模式、编辑模式）：在命令模式下按 `i` 进入插入模式，按 `Esc` 返回命令模式

  - 末行模式：在命令模式界面按 `:` 进入末行模式，在末行模式下可以输入命令，按 `Enter` 执行命令

    - `:q`：退出并保存文件

    - `:q!`：退出并丢弃文件（强制退出）

    - `:wq`：保存并退出

    - `:w`：保存文件

    - `:set nu`：显示行号

    - `:set nonu`：取消显示行号

#### mount 挂载操作

- Windows 系统访问光盘内容

  - 光盘 -> 光驱设备 -> CD 驱动器（双击）（访问点）

- Linux 系统访问光盘内容

  - 光盘 -> 光驱设备（文件形式） -> 目录（访问点 挂载点）

  - 快捷方式（/dev/sr0）

- `mount` 挂载操作：让目录成为**设备（光盘、硬盘等）**的访问点，通过系统的安装光盘获得软件包

  - `mount [选项] 设备路径 挂载点目录`

- **永久挂载自己的光驱**：

  - `vim  /etc/fstab`

  - 添加：`/dev/sr0  /susucdrom iso9660 defaults  0  0`

#### rpm 软件包

- RPM 包 **文件名特征**

  - 软件名-版本信息.操作系统.硬件架构.rpm

  - bash-4.2.46-30.el7.x86_64.rpm

  - 操作系统：e 代表企业版，l 代表 linux

- 查询已安装的 RPM 包软件信息

  - 格式：`rpm -q[子选项] 软件名`

  - 常用的子选项：

    - `-a`：显示所有已安装的软件包信息

    - `-i`：显示软件包的安装信息

    - `-l`：显示软件包的文件信息

    - `-e`：卸载软件包

- 安装/卸载 RPM 包软件

  - 格式：`rpm -i[选项] rpm 包名`

  - 常用命令选项：

    - `-v`：显示安装细节

    - `-h`：以#号显示安装进度

- 查询已安装的软件包某个目录/文件是哪个 RPM 带来的

  - 格式：`rpm -qf [文件路径]`

### Vim 编辑技巧

#### 命令模式基本操作

1. 光标跳转：

| 操作类型     | 按键指令            | 用 途              |
| ------------ | ------------------- | ------------------ |
| 移动光标     | ↑、↓、←、→          | 上、下、左、右     |
| 翻页         | PgDn 或 Ctrl+f      | 向下翻动一整页内容 |
|              | PgUp 或 Ctrl+b      | 向上翻动一整页内容 |
| 光标行内跳转 | Home 键或 ^、数字 0 | 跳转至行首         |
|              | End 键或 $ 键       | 跳转到行尾         |
| 光标行间跳转 | 1G 或 gg            | 跳转到文件的首行   |
|              | G                   | 跳转到文件的末尾行 |

2. 复制、粘贴、删除：

| 操作类型 | 按键指令       | 用 途                  |
| -------- | -------------- | ---------------------- |
| 复制     | yy、#yy        | 复制光标处的一行、# 行 |
| 粘贴     | p、P           | 粘贴到光标处之后、之前 |
| 删除     | x 或 Delete 键 | 删除光标处的单个字符   |
|          | dd、#dd        | 删除光标处的一行、# 行 |
|          | d^             | 从光标处之前删除至行首 |
|          | d$             | 从光标处删除至行尾     |

3. 查找、撤销、保存

| 操作类型 | 按键指令 | 用 途                         |
| -------- | -------- | ----------------------------- |
| 文本查找 | /word    | 向后查找字符串 “word”         |
|          | ?word    | 向前查找字符串 “word”         |
|          | n、N     | 跳至下一个/上一个找到的字符串 |
| 撤销编辑 | u        | 撤销最近的一次操作            |
|          | U        | 撤销当前行的所有修改          |
|          | Ctrl + r | 取消前一次撤销操作            |
| 保存退出 | ZZ       | 保存文件内容并退出 vim 编辑器 |

#### 末行模式基本操作

4. 存盘、退出、文件操作

| 操作类型   | 设置指令            | 用 途                  |
| ---------- | ------------------- | ---------------------- |
| 存盘及退出 | :w                  | 保存当前文件           |
|            | :q!                 | 放弃已有更改后强制退出 |
|            | :wq 或 :x           | 保存已有修改后退出     |
| 文件操作   | :w /root/newfile    | 另存为其它文件         |
|            | :r /etc/filesystems | 读入其他文件内容       |

5. 字符串替换

| 操作类型   | 设置指令         | 用 途                      |
| ---------- | ---------------- | -------------------------- |
| 行内替换   | :s/old/new       | 替换当前行第一个“old”      |
|            | :s/old/new/g     | 替换当前行所有的“old”      |
| 区域内替换 | :n,m s/old/new/g | 替换第 n-m 行所有的“old”   |
|            | :%s/old/new/g    | 替换文件内所有的“old”      |
| 操作确认   | :%s/old/new/gc   | 替换时加 c，每次需用户确认 |

6. 开关设置

| 设置指令        | 用 途         |
| --------------- | ------------- | ------------------------- |
| :set nu         | nonu          | 显示/不显示行号           |
| :syntax on      | off           | 启用/关闭语法高亮         |
| :set hlsearch   | no hlsearch   | 开启/关闭查询结果高亮显示 |
| :set autoindent | no autoindent | 启用/关闭自动缩进         |

#### Vim 高级操作

##### 预设配置

使用 `.vimrc` 配置文件

位于用户家目录可以预先设置一些 vim 参数

```
[root@localhost ~]# vim /root/.vimrc
set nu
...
:wq
```

#### Yum 仓库

**Yum 软件包管理**：自动解决依赖关系

Yum 称为软件包仓库，其作用为：为客户端自动解决依赖关系安装软件包

- 服务端：

  1.  软件包

  2.  仓库数据文件都在光盘内容中

  3.  利用

      - HTTP：超文本传输协议，传递软件包到客户端

      - FTP： 文本传输协议

客户端文件路径：`/etc/yum.repos.d/.repo`，在此路径下，错误的配置文件会影响正确的配置文件

#### Yum 命令工具

- 查询软件信息

  - 格式：`yum info 软件名`

- 查看软件列表

  - 格式：`yum list 软件名`

- 检索软件以及相关信息

  - 用法：

    - `yum search 关键字`

    - `yum info 软件名`

    - `yum provides 文档路径`

- 安装/卸载软件包

  - 格式：`yum [-y] 指令 软件名`

  - 指令：

    - `install`：安装软件包

    - `remove`：卸载软件包

- 清空 Yum 的缓存

  - `yum clean all`清空 yum 缓存

  - `yum repolist`列出仓库信息

#### Shell 基础与命令使用

**linux 目录结构**

1. 根-树形目录结构

`[root@localhost ~]# man hier  `

2. 常见一级目录的用途

- `/boot`：存放系统引导必需的文件，包括内核、启动配置

- `/bin、/sbin`：存放各种命令程序

- `/dev`：存放硬盘、键盘、鼠标、光驱等各种设备文件

- `/etc`：存放 Linux 系统及各种程序的配置文件

- `/root、/home/用户名`：分别是管理员目录、普通用户的默认家目录

- `/var`：存放日志文件、邮箱目录等经常变化的文件

- `/proc`：存放内存中的映射数据，不占用磁盘

- `/tmp`：存放系统运行过程中使用的一些临时文件

3. 历史命令

- `history`：查看历史命令列表

- `history -c`：清空历史命令

- `!n`：执行命令历史中的第 n 条命令

- `!str`：执行最近一次以 str 开头的命令

4. du 命令，统计文件的占用空间

- 格式：`du [选项]... [目录或文件]...`

- `-s`：只统计每个参数所占用的总空间大小

- `-h`：提供易读容量单位

5. date 命令，查看/调整系统日期时间

- `date +%F`：只查看年月日

- `date +%R`：查看时和分

- `date +"%Y-%m-%d %H:%M:%S"`：查看年月日时分秒

- `date -s "yyyy-mm-dd HH:MM:SS"`：设置系统日期时间

6. ln 创建软连接（若原始文件或目录被删除，连接文件将失效，软连接可存放在不同分区/文件系统）

- 格式：`ln -s 原始文件 软连接文件`

7. ln 创建硬链接（若原始文件被删除，连接文件仍可用，硬连接与原始文件必须在同一分区/文件系统）

- 格式：`ln 原始文件 硬连接文件`

8. `--help` 查看外部命令的帮助信息

### 用户和组概述

#### 账号控制

- 基于账号的访问控制：

  - 系统用户：登录操作系统，方便做权限的不同设置

  - 组：方便管理众多的用户，方便对用户进行分类

- 唯一标识：UID，GID（管理员 root 的 UID 为 0）
  llllll
- 组的分类：基本组、附加组（从属组）

  - 基本组：Linux 自己创建的组，与用户同名，系统自动将用户加入

  - 附加组（从属组）：管理员自建创造，管理员将用户加入

Linux 一个用户必须至少属于一个组

##### 用户账号创建

用户基本信息存放在 `/etc/passwd` 文件，每个用户记录一行，以 `:` 分割为 7 字段

`用户名:密码占位符:用户UID:基本组GID:描述信息:家目录（宿主目录）:解释器`

- 添加用户

  - `useradd [选项]... 用户名`

    - 常用命令选项：

      - `-u`：用户 id

      - `-d`：家目录路径

      - `-s`：登录解释器

      - `-G`：附加组

##### 用户密码设置

- 管理员给普通用户设置密码：

  - `passwd 用户名`

- 普通用户自己给自己更改密码：

  - `su -用户名`：切换用户，管理员切换用户不用密码，普通用户切换需要密码

#### 用户删改查

- `usermod [选项]... 用户名`

  - 常用命令选项：

    - `-u`：用户 id

    - `-d`：家目录路径

    - `-s`：登录 Shell

    - `-G`：附加组（重置附加组）

- `userdel [-r] 用户名`（`-r`：删除用户家目录）

#### 用户初始配置文件

`/etc/skel/`目录是用来存放新用户配置文件的目录，当我们添加新用户的时候，这个目录下的所有文件会自动被复制到新添加的用户的家目录下。通过修改、添加、删除/etc/skel 目录下的文件，我们可为新创建的用户供统一的、标准的、初始化用户环境。

#### 组管理

管理组成员

组成员信息存放在 `/etc/gshadow` 文件

- `groupadd [-g 组ID] 组名`

  - `组名字:密码占位符:组GID:组成员列表（组成员可以使0-N）`

- `gpasswd -a 用户名 组名`：添加用户到某个组

- `gpasswd -d 用户名 组名`：删除用户从某个组

- `-groupdel 组名`：删除组（不能删除基本组）

### 基本权限和归属

#### 权限的类别

- 访问方式（权限）

  - 读取：允许查看内容 `-read` r

  - 写入：允许修改内容 `-write` w

  - 执行：允许运行程序 `-execute` x

- 对于文本文件：

  - r 读取权限：cat less head tail

  - w 写入权限：vim > >>

  - x 可执行权限：Shell 脚本编写的时候可以赋予

- 权限适用对象(归属)

  – 所有者:拥有此文件/目录的用户`-user` u

  – 所属组:拥有此文件/目录的组`-group` g

  – 其他用户:除所有者、所属组以外的用户`-other` o

- 查看权限

  - `ls -l`

  - 输出的七个字段：**权限位**、**硬连接数**、**属主**、**属组**、**文件大小**、**修改时间**、**文件/目录名称**

#### 修改权限

- `chmod [选项]... +-=权限 目录/文档`

  - 常用命令选项：

    - `-R`：递归修改目录及其子目录权限

    - `-u`：所有者权限

    - `-g`：所属组权限

    - `-o`：其他用户权限

- 目录的权限：

  - `r`：能够 ls 浏览此目录内容

  - `w`：能够执行 `rm/mv/cp/mkdir/touch` 等操作

  - `x`：能都使用 `cd` 切换到此目录

- 权限位的 8 进制数表示：

表格转换为 Markdown 格式如下：

| 分组 | User 权限 |     |     | Group 权限 |     |     | Other 权限 |     |     |
| ---- | --------- | --- | --- | ---------- | --- | --- | ---------- | --- | --- |
| 字符 | r         | w   | x   | r          | -   | x   | r          | -   | x   |
| 数字 | 4         | 2   | 1   | 4          | 0   | 1   | 4          | 0   | 1   |
| 求和 | **7**     |     |     | **5**      |     |     | **5**      |     |     |

数字的形式修改权限：

- 7: rwx
- 6: rw-
- 5: r-x
- 4: r--
- 3: -wx
- 2: -w-
- 1: --x
- 0: ---

#### 归属控制

- `chown [-R] 属主 文档...`

- `chown [-R] :属组 文档...`

- `chown [-R] 属主:属组 文档...`

#### Set GID

- 附加在属组的 x 位上
- 属组的权限标识会变为 s（无执行权限显示大 S，有执行权限显示小 s）
- 适用于目录，Set GID 可以使目录下新增的文档自动设置与父目录相同的属组
- 传递所属组身份

#### Sticky Bit

Sticky Bit 粘滞位（若赋予其他人执行权限会覆盖 t 权限）

- 附加在其他人的 x 位上
- 其他人的权限标识会变为 t
- 适用于开放 w 权限的目录,可以阻止用户滥用 w 写入权限（禁止操作别人的文档）

#### Acl 策略

##### Acl 策略作用

- 文档归属的局限性
- 任何人只属于三种角色：属主、属组、其他人
- 无法实现更精细的控制

- 能够对个别用户、个别组设置独立的权限
- 大多数挂载的 EXT3/4、XFS 文件系统默认已支持

##### Acl 策略管理

设置 Acl 访问控制策略

- 使用 `getfacl`、`setfacl` 命令

### 计划任务和进程

#### Cron 计划任务

**周期性计划任务：cron 任务概述**

- 用途：按照设置的时间间隔为用户反复执行某一项固定的系统任务
- 软件包：cronie、crontabs
- 系统服务：crond
- 日志文件：/var/log/cron

**管理计划任务策略**

- 使用 `crontab` 命令

  - 编辑：`crontab -e [-u 用户名]`
  - 查看：`crontab -l [-u 用户名]`
  - 删除：`crontab -r [-u 用户名]`

#### 程序和进程

**程序**：静态的代码 仅占用磁盘空间
**进程**：动态执行的代码 占用 cpu 与内存
**PID**：进程编号

**查看进程树**

- `pstree`：查看进程树

- 格式：`pstree [选项] [PID或用户名]`

  - 常用命令选项：

    - `-a`：显示完整的命令行

    - `-p`：显示进程号

#### 查看进程

- `ps`：查看进程

  - 格式：`ps [选项]`

  - 常用命令选项：

    - `-a`：显示所有进程

    - `-u`：指定用户

    - `-x`：当前用户在所有终端下的进程

    - `-e`：显示系统内所有进程

    - `-f`：

- `top`：实时显示系统中进程的运行状态

  - `-d 刷新秒数`：刷新间隔

  - `-u 用户名`：指定用户

- `pgrep` —— Process Grep

  - 格式：`pgrep [选项]... 查询条件`

  - 常用命令选项：

    - `-l`：输出进程名,而不仅仅是 PID

    - `-u`：检索指定用户的进程

    - `-t`：检索指定终端的进程

    - `-x`：精确匹配完整的进程名

#### 进程管理

- 前台启动

  - 输入正常的命令行

  - 运行期间占用当前终端

- 后台启动

  - 在命令行末尾添加“&”符号,以正在运行的状态放入后台

  - 运行期间不占用当前终端

- `Ctrl + z` 组合键：挂起当前进程（暂停并转入后台）

  - `jobs`：查看后台任务列表

  - `bg [job id]`：激活后台被挂起的任务（后台运行）

  - `fg [job id]`：将后台任务恢复到前台运行

- `Ctrl + c` 组合键：终止当前进程

  - `kill [-9] 进程号`：强制终止进程

#### 日志管理

- 记录系统、程序运行中发生的各种事件

- 通过查看日志，了解、排除故障

- 信息安全控制的“依据”

- 由系统服务 rsyslog 统一记录、管理，日志消息采用文本格式，主要记录时间发生的事件、主机、进程、内容

- 常见的日志文件

`/var/log/messages` 记录内核消息、各种服务的公共消息

`/var/log/dmesg` 记录系统启动过程的各种消息

`/var/log/cron` 记录与 cron 计划任务相关的消息

`/var/log/maillog` 记录邮件收发相关的消息

`/var/log/secure` 记录与访问限制相关的安全消息

#### 日志分析

- 通用分析工具：

  - tail、tailf、less、grep 等文本浏览、检索命令

  - awk、sed 等格式化过滤工具

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

## Linux 作业操作

### Linux 基本知识

#### Linux 发行版与开源软件

##### Linux 发行版

- Red Hat Enterprise Linux 7/8/9
- CentOS 7/8
- Suse Linux Enterprise 14
- Debian Linux
- Ubuntu Linux 15.04/13.10
- Oracle Linux

##### 开源软件

- Apache HTTP Server
- MySQL Database
- PHP
- Libre Office
- Firefox
- Nginx

#### Linux 根目录与 /dev 目录的作用

- `/`：Linux 文件系统的起点，Linux 所有的文件都放在其中。

- `/dev`：存放硬盘、键盘、鼠标、光驱等各种设备文件。

#### Linux 系统中磁盘的表示方法

- 第 2 块 SCSI 接口的硬盘被表示为 `/dev/sdb`

- 此硬盘中的第 3 个主分区表示为 `/dev/sdb3`

- 第 4 个逻辑分区表示为 `/dev/sdb8`

#### Linux 中默认的文件系统

- EXT3：第 3 代扩展（Extended）文件系统，RHEL 5.x 系列之前默认。

- EXT4：第 4 代扩展（Extended）文件系统，RHEL 6.x 系列默认。

- XFS：日志文件系统，RHEL 7 和 8 系列默认文件系统。

- SWAP：交换文件系统，起虚拟内存的作用。

#### 控制台的切换

在 RHEL 系统的 TTY1 控制台中，可以按 `Ctrl+Alt+F2` 切换到 TTY2 控制台。

#### 查看当前 Linux 系统的内核版本

```
[root@localhost ~]# uname -r
3.10.0-957.el7.x86_64
```

#### 查看当前 Linux 主机的第一块网卡的 IP 地址

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

#### 查看当前 Linux 主机的 CPU 和内存信息

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

#### 查看根目录内容

```
[root@localhost ~]# ls /
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```

#### Linux 中延迟重启的命令

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

- **less** 或 **more**: 实现分页查看文件内容

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

### vim 文本编辑器高级应用、rpm 管理和 yum 搭建

#### 列出已安装的 rpm 软件包

```
rpm -qa
```

#### 查看 firefox 软件包的安装清单

```
rpm -ql firefox
```

#### 查询 ifconfig 命令程序所在的软件包

```
which ifconfig
rpm -qf /path/to/ifconfig
```

#### 查看 firefox 软件包的用途

```
rpm -qi firefox
```

#### 复制文件

```
cp /etc/passwd /opt/passwd.txt
```

#### vim 操作

1. **切换到最后一行**
   ```
   G
   ```
2. **切换到第 10 行**
   ```
   10gg
   ```
3. **切换到第一行**
   ```
   gg
   ```
4. **删除第 6 行**
   ```
   dd
   ```
5. **删除 1-5 行**
   ```
   5dd
   ```
6. **复制第一二行，粘贴到最后一行**
   ```
   2yy
   G
   p
   ```
7. **查找全文的 root 字符串**
   ```
   /root
   ```
8. **将全文的 a 替换成大写的 A**
   ```
   :%s /a/A/g
   ```
9. **给 10-20 行添加注释**
   1. 进入 VISUAL BLOCK 模式
      ```
      ESC
      Ctrl + V
      ```
   2. 使用箭头键选择需要注释的行
   3. 进入插入模式
      ```
      Shift + i
      ```
   4. 输入 # 注释符号
   5. 退出插入模式
      ```
      ESC
      ```
10. **显示行号**
    ```
    :set nu
    ```

#### 搭建 yum 源，安装 httpd 软件包

```
yum install httpd
```

#### Yum 仓库的作用

Yum 仓库可以完成安装、卸载、自动升级 rpm 软件包等任务，能够自动查找并解决 rpm 包之间的依赖关系。

#### 使用 zip 制作和解压文件

```
zip -r root.zip /root
unzip root.zip -d /home/susu/
```

#### > 覆盖写入

```
echo 1111 > 1.txt
```

#### >> 追加写入

```
echo 2222 >> 1.txt
```

#### 2> 覆盖写入错误信息

```
ls /abc 2> 3.txt
```

#### 2>> 追加写入错误信息

```
ls /abc 2>> 3.txt
```

#### &> 覆盖写入错误和正确信息

```
ls /abc /opt/ &> 4.txt
```

#### &>> 追加写入错误和正确信息

```
ls /abc /opt/ &>> 4.txt
```

#### rpm 常用选项

- -a: 列出已安装所有软件包
- -i: 查看指定软件的详细信息
- -l: 查看指定软件的文件安装清单

#### 查询软件是否安装

```
rpm -q firefox
```

#### 查询软件包详细信息

```
rpm -qi firefox
```

#### 统计安装的 RPM 包数量

```
rpm -qa | wc -l
```

#### 查询 ls 命令帮助信息

```
ls --help
```

### 用户及组管理

#### 用户的基本组与附加组

在 `/etc/passwd` 文件中，用户记录的 GID 所对应的组是这个用户的基本组，也称为私有组；除此以外，若 `/etc/group` 文件中某个其他组的成员也包括这个用户，那么这个组是这个用户的附加组，也称为公共组。

#### 用户的 UID 标识

在 Linux 系统中，管理员用户 root 的 UID 是 0，而新建普通用户的 UID 默认情况下会从 1000 开始。

#### 用户与组相关配置文件

Linux 用户的家目录、登录 Shell 等信息保存在 `/etc/passwd` 文件内，而加密的密码字符串、密码有效期等信息保存在 `/etc/shadow` 文件内。通过 `/etc/group` 文件可以查看系统中有哪些组账号，以及各个组包括那些成员用户。

#### useradd 命令常用选项

使用 `useradd` 命令添加用户账号时，常用的选项有：

- `-u`：指定 UID 标记号
- `-d`：指定宿主目录，缺省为 `/home/用户名`
- `-e`：指定帐号失效时间
- `-g`：指定所属的基本组（组名或 GID）
- `-G`：指定所属的附加组（组名或 GID）
- `-M`：不为用户建立并初始化宿主目录
- `-s`：指定用户的登录 Shell

#### useradd 命令的应用

当执行 `useradd 登录名` 新建一个用户账号，并为其设置密码以后，系统都做了哪些工作，以使得此用户能够登入并正常使用？

1.  修改 `/etc/group`、`/etc/gshadow`，添加与用户名同名的私有组记录。
2.  修改 `/etc/passwd` 文件，添加登录名、UID、GID、登录 Shell 等账号记录。
3.  修改 `/etc/shadow` 文件，添加加密的密码字串、密码有效期等相关记录。
4.  为用户在 `/home` 目录下创建宿主文件夹，名称与登录名相同。
5.  拷贝模板目录 `/etc/skel/` 下的文件到新用户的家目录下。

#### 用户账号的初始配置文件

用户账号的初始配置文件有：

- `~/.bash_profile`：文件中的命令将在该用户每次登录时被执行。
- `~/.bashrc`：文件中的命令会在每次加载 `/bin/bash` 程序时（当然也包括登录系统）被执行。
- `~/.bash_logout` 文件中的命令将在用户每次退出登录时被执行。
  上述文件适用于当前用户，而与之类似的 `/etc/profile`、`/etc/bashrc` 文件适用于所有本地用户。

#### 为某个用户设置永久别名

为 root 用户设置一个永久别名为 `myls='ls -lhd'`。

1.  `[root@svr5 /]# vim /root/.bashrc`
2.  `# .bashrc`
3.  `# User specific aliases and functions`
4.  `alias rm='rm -i'`
5.  `alias cp='cp -i'`
6.  `alias mv='mv -i'`
7.  `alias myls='ls -lhd'` //添加此行
8.  `.. ..`
    当开启新的命令行终端时，检查别名即已生效。
9.  `[root@svr5 /]# alias`
10. `alias cp='cp -i'`
11. `alias l.='ls -d .* --color=auto'`
12. `alias ll='ls -l --color=auto'`
13. `alias ls='ls --color=auto'`
14. `alias mv='mv -i'`
15. `alias myls='ls -lhd'`
16. `alias rm='rm -i'`
17. `alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'`
18. `[root@svr5 /]#`

#### 创建及修改 iamkiller 用户属性

新建一个名为 iamkiller 的本地用户账号，要求如下：

1.  宿主文件夹位于 `/opt/.private/iamkiller`。
2.  使用 `/sbin/nologin` 作为登录 Shell。
3.  将 UID 号指定为 1234。
4.  修改登录 shell 为 `/bin/bash`，清空登录密码。
5.  `[root@svr5 ~]# mkdir /opt/.private`
6.  `[root@svr5 ~]# useradd -d /opt/.private/iamkiller -s /sbin/nologin -u 1234 iamkiller`
7.  `[root@svr5 /]# usermod -s /bin/bash iamkiller`
8.  `[root@svr5 /]# passwd –d iamkiller`

#### 组的基本管理

创建一个名为 student 的组账号，将 GID 设为 5918，其他相关要求如下：

1.  添加 3 个成员用户 nsd001、nsd002、nsd003。
2.  将其中用户 nsd001 的 UID 设置为 0。
3.  添加组账号：

```
[root@svr5 ~]# groupadd -g 5918 student
```

2.  添加用户账号：

```
[root@svr5 ~]# useradd -G student nsd001
[root@svr5 ~]# useradd -G student nsd002
[root@svr5 ~]# useradd -G student nsd003
```

3.  强制将用户的 nsd001 设置为 0：

```
[root@svr5 ~]# vim /etc/passwd
```

```
.. ..
nsd001:x:0:1235::/home/nsd001:/bin/bash             //改 UID 为 0
nsd002:x:1236:1236::/home/nsd002:/bin/bash
nsd003:x:1237:1237::/home/nsd003:/bin/bash
```

或者

```
[root@svr5 ~]# usermod -u 0 -o nsd001
```

### 基本权限与归属关系

#### 基本访问权限

- **读取 (read**)：允许查看内容
- **写入 (write**)：允许修改内容
- **可执行 (execute**)：允许运行和切换

#### 基本归属关系

- **属主 (user**)：拥有此文件/目录的用户
- **属组 (group**)：拥有此文件/目录的组
- **其他用户 (other**)：除属主、属组以外的用户
- **所有用户 (all**)：以上三类归属合称

#### 修改用户属性的常用命令

- 若要修改文件或目录的基本权限，主要使用 **chmod** 命令工具；
- 若要修改属主、属组信息，主要使用 **chown** 命令工具。

#### 基本权限与归属关系的设置

1.  创建 `mydocs` 目录，并查看默认的权限、归属。

```
mkdir /opt/mydocs
ls -ld /opt/mydocs/
```

输出示例：

```
drwxr-xr-x 2 root root 4096 02-13 04:14 /opt/mydocs/
```

2.  使用 `chmod` 或 `chown` 进行调整，使 `mike` 用户具有 `rwx` 权限。

- 使用 `chmod`：

```
chmod o+rwx /opt/mydocs
ls -ld /opt/mydocs/
```

输出示例：

```
drwxr-xrwx 2 root root 4096 02-13 04:14 /opt/mydocs/
```

- 使用 `chown`：

```
chown mike /opt/mydocs
ls -ld /opt/mydocs/
```

输出示例：

```
drwxr-xr-x 2 mike root 4096 02-13 04:14 /opt/mydocs/
```

#### umask 命令的作用

`umask` 是 Bash Shell 的一个内部命令，可以设置一个权限掩码，用来确定用户新建文件或目录的缺省权限。直接执行 `umask` 可查看默认的权限掩码：

```
umask
```

输出示例：

```
0022
```

权限掩码由四个数字组成，将现有的默认存取权限减掉权限掩码后，即可获知建立文件时默认的权限。

- Linux 系统中 root 用户对目录默认的满权限为 777，默认的权限掩码设为 0022，两者后三位相减得到新建目录的权限为 755；

- Linux 中 root 用户对文件默认的满权限为 666，默认的权限掩码设为 0022，两者相减后可得新建文件权限为 644，可以看到用户新建的文件默认是没有可执行权限的，这是基于安全的考虑。

#### 特殊权限的介绍。

- SUID：占用属主的 x 位，对可执行程序/目录有效，当其他用户执行带 Suid 标记的程序时，将具有属主的身份和相应权限。

- SGID：占用属组的 x 位，对可执行程序/目录有效，当其他用户执行带 Sgid 标记的程序时，将具有属组的身份和相应权限。

- Sticky Bit：也称为粘滞位，占用 other 分组的 x 位，仅当用户对目录有 w 写入权限时生效，在设置了粘滞位的文件夹下，即便用户有写入权限，也不能删除或改名他人的文档。

#### 特殊权限的设置

拷贝系统中的 mkdir 程序为 mymd，为 mymd 命令程序设置 SUID 权限，使得普通用户能用它在 / 目录下创建子目录。

1. 拷贝、创建 mymd 程序，设置 SUID 权限。

```bash
[root@svr5 ~]# cp /bin/mkdir /bin/mymd
[root@svr5 ~]# chmod u+s /bin/mymd
[root@svr5 ~]# ls -l /bin/mymd
-rwsr-xr-x 1 root root 31664 08-24 16:02 /bin/mymd
```

2. 以普通用户登录，验证效果。

```bash
[zengye@svr5 ~]$ mymd /zydir
[zengye@svr5 ~]$ ls -ld /zydir
drwxrwxr-x 2 root zengye 4096 08-24 16:04 /zydir
```

#### ACL 权限的基本使用。

在 EXT3/4 文件系统中，可以利用内核及 acl 软件包的支持，为文件/目录设置额外的 ACL 访问控制，通过 setfacl 工具用来设置策略，getfacl 工具用来查看策略。

#### ACL 权限的常用选项。

为文件/目录设置 ACL 访问控制策略时，常用的选项如下所示，请补充各自的作用。

- -m：定义一条 ACL 策略

- -x：清除指定的 ACL 策略

- -b：清除所有已设置的 ACL 策略

- -R：递归设置 ACL 策略

- -d：为目录设默认权限（子文档自动继承）

#### setfacl 基本操作设置 ACL 策略。

1. 创建一个目录 /public/，将权限设置为 755

```bash
[root@test01 ~]# mkdir /public
[root@test01 ~]# chmod 755 /public/
```

2. 查看目录 /public/ 的 ACL 策略

```bash
[root@test01 ~]# getfacl /public/
```

3. 为目录 /public/ 设置 ACL 策略，使用户 zhangsan 有权限写入，验证效果

```bash
[root@test01 ~]# setfacl -m u:zhangsan:rwx /public/
```

4. 使用户 lisi 无任何权限，验证效果

```bash
[root@test01 ~]# setfacl -m u:lisi:- /public/
```

### 计划任务和进程

#### 周期性计划任务的作用是什么？

用途：按照设置的时间间隔为用户反复执行某一项固定的系统任务

#### 周期性计划任务的系统服务名称是什么？

crond

#### 以下 crontab 命令的常用选项分别是什么意思？

- `-e`: 编辑
- `-l`: 查看
- `-r`: 清除
- `-u`: 用户名

#### 周期性计划任务配置的的格式是什么？

```bash
分 时 日 月 周 命令
```

#### 按照要求，编写以下周期性计划任务：

1）使用 natasha 用户在每周的周三 19:30 执行`cat /etc/passwd >> /opt/nsd.txt`

```bash
30 19 * * 3 natasha cat /etc/passwd >> /opt/nsd.txt
```

2）使用 student 用户每 3 分钟执行`date >> /opt/date.txt`

```bash
*/3 * * * * student date >> /opt/date.txt
```

3）使用 harry 用户在每天的早上 9 点 10、9 点 20、9 点 30 执行`cat /etc/hosts >> /opt/host.txt`

```bash
10,20,30 9 * * * harry cat /etc/hosts >> /opt/host.txt
```

#### 什么是程序，什么是进程，区别是什么？

- **程序**：静态的代码，仅占用磁盘空间

- **进程**：动态执行中的代码，占用 CPU 与内存空间

#### 如何查看进程树，请写出命令格式，以及以下命令的选项的作用是什么？

- **查看进程树**: `pstree — Processes Tree`

- **格式**: `pstree [选项] [PID 或用户名]`

- **常用命令选项**

  - `-a`: 显示完整的命令行

  - `-p`: 列出对应 PID 编号

#### 使用 zhangsan 用户登入，vim 打开 123.txt 文件，不要编辑，也不退出，然后使用 root 用户查看 zhangsan 用户进程的 PID 号和完整的命令行？

```bash
root@localhost ~]# pstree -ap | grep zhangsan
```

#### 如何查看进程快照，请写出命令？

`ps aux`

#### 进程快照常用的组合中，想查看父进程的 PID 号，使用哪个命令和选项？

使用`ps`命令以及`-elf`三个选项

#### 使用 top 交互式查看进程状态工具，完成以下操作：

1）查看系统时间是多少

查看`top`命令的`12:49:28`部分

2）查看系统待机时间多长

查看`top`命令的`up  2:33,  1 user,  load average: 0.00, 0.01, 0.05`部分

3）查看登陆的用户有几个

查看`top`命令的`1 user`部分

4）查看 CPU 1、 5、15 平均的负载量是多少

查看`top`命令的`load average: 0.00, 0.01, 0.05`部分

5）查看当前系统的进程总数、查看正在运行的进程数量、查看在睡觉的进程数量、查看已暂停的进程数量、查看僵尸进程数量

查看`top`命令的`Tasks:  322 total,   1 running, 320 sleeping,   0 stopped,   1 zombie`部分

6）查看内存和 swap 空间总大小、查看已用空间大小、剩余空间大小

查看`top`命令的`KiB Mem :  8168920 total,   6638084 used,   1530836 free,   364256 buffers`和`KiB Swap:  8192000 total,        0 used,  8192000 free.   5413964 cached`部分

7）按照内存消耗大小降序排列，查看哪个进程消耗内存最高

按下`M`键

8）按照 CPU 消耗大小降序排列，查看哪个进程消耗 CPU 最高

按下`P`键

9）设置 top 命令的刷新时间为 2 秒

按下`1`键，然后输入`2`

#### 如何把运行的程序放入到后台执行？

在命令行末尾添加“&”符号，以正在运行的状态放入后台。

#### 相关命令作用

- ctrl + z 挂起当前进程（暂停并转入后台）

- jobs 查看后台任务列表

- fg 将后台任务恢复到前台运行

- bg 激活后台被挂起的任务

#### 杀死进程的命令

`Ctrl + c` 组合键，中断当前命令程序

`kill [-9] PID…`、`kill [-9]%后台任务编号`，-9 强制杀死

`killall [-9] 进程名…`

#### 日志的功能

- 记录系统、程序运行中发生的各种事件
- 通过查看日志，了解及排除故障
- 信息安全控制的“依据”

#### 系统服务记录日志

由 `rsyslog`（日志服务）记录我们的日志。

#### 常见的日志文件及其作用

- `/var/log/messages`：记录内核消息、各种服务的公共消息
- `/var/log/dmesg`：记录系统启动过程的各种消息
- `/var/log/cron`：记录与 cron 计划任务相关的消息
- `/var/log/maillog`：记录邮件收发相关的消息
- `/var/log/secure`：记录与访问限制相关的安全消息

#### 分析日志的常见工具及其作用

- `tail`、`tailf`、`less`、`grep` 等文本浏览/检索命令
- `awk`、`sed` 等格式化过滤工具

#### 查看用户登陆信息的命令

- `users`、`who`、`w` 命令

#### 查看登录记录

- 登陆成功：`last -2`
- 登录失败：`lastb -2`

#### 日志的优先级别

- `0 EMERG`（紧急）：会导致主机系统不可用的情况
- `1 ALERT`（警告）：必须马上采取措施解决的问题
- `2 CRIT`（严重）：比较严重的情况
- `3 ERR`（错误）：运行出现错误
- `4 WARNING`（提醒）：可能会影响系统功能的事件
- `5 NOTICE`（注意）：不会影响系统但值得注意
- `6 INFO`（信息）：一般信息
- `7 DEBUG`（调试）：程序或系统调试信息等

#### systemd 服务管理工具

- Linux 系统和服务管理器
  - 是内核引导之后加载的第一个初始化进程（PID=1）
  - 负责掌控整个 Linux 的运行/服务资源组合

#### 查看运行级别

- 查看当前系统有哪些运行级别：`systemctl -t target`

#### 切换运行级别

- 切换运行级别为文本字符模式：`systemctl isolate multi-user.target`
- 切换运行级别为图形模式：`systemctl isolate graphical.target`

#### 查看系统运行级别

- 查看系统的旧运行级别和当前运行级别：`runlevel`
- 查看当前系统默认的运行级别：`systemctl get-default`
