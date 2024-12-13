# Linux

## Linux 基础

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
bash
[root@localhost ~]# uname -r
3.10.0-957.el7.x86_64
```

### 查看当前 Linux 主机的第一块网卡的 IP 地址

```
bash
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
bash
[root@localhost ~]# cat /proc/meminfo           # 查看内存信息
MemTotal:        3861508 kB
MemFree:         1325248 kB
MemAvailable:    3309832 kB
Buffers:            4172 kB
Cached:          2135668 kB
SwapCached:            0 kB
Active:           931552 kB
Inactive:        1280392 kB
Active(anon):      73904 kB
Inactive(anon):    10064 kB
Active(file):     857648 kB
Inactive(file):  1270328 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:       4063228 kB
SwapFree:        4063228 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:         72272 kB
Mapped:            37780 kB
Shmem:             11864 kB
Slab:             204208 kB
SReclaimable:     122400 kB
SUnreclaim:        81808 kB
KernelStack:        4320 kB
PageTables:         4816 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     5993980 kB
Committed_AS:     318972 kB
VmallocTotal:   34359738367 kB
VmallocUsed:      201848 kB
VmallocChunk:   34359310332 kB
HardwareCorrupted:     0 kB
AnonHugePages:     12288 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      145216 kB
DirectMap2M:     4048896 kB
DirectMap1G:     2097152 kB

[root@localhost ~]# cat /proc/cpuinfo           # 查看 CPU 信息
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 151
model name      : 12th Gen Intel(R) Core(TM) i5-12490F
stepping        : 2
microcode       : 0x2c
cpu MHz         : 2995.201
cache size      : 20480 KB
physical id     : 0
siblings        : 3
core id         : 0
cpu cores       : 3
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 32
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb sha_ni xsaveopt xsavec xgetbv1 arat umip pku ospke gfni vaes vpclmulqdq spec_ctrl intel_stibp flush_l1d arch_capabilities
bogomips        : 5990.40
clflush size    : 64
cache_alignment : 64
address sizes   : 45 bits physical, 48 bits virtual
power management:

processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 151
model name      : 12th Gen Intel(R) Core(TM) i5-12490F
stepping        : 2
microcode       : 0x2c
cpu MHz         : 2995.201
cache size      : 20480 KB
physical id     : 0
siblings        : 3
core id         : 1
cpu cores       : 3
apicid          : 1
initial apicid  : 1
fpu             : yes
fpu_exception   : yes
cpuid level     : 32
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb sha_ni xsaveopt xsavec xgetbv1 arat umip pku ospke gfni vaes vpclmulqdq spec_ctrl intel_stibp flush_l1d arch_capabilities
bogomips        : 5990.40
clflush size    : 64
cache_alignment : 64
address sizes   : 45 bits physical, 48 bits virtual
power management:

processor       : 2
vendor_id       : GenuineIntel
cpu family      : 6
model           : 151
model name      : 12th Gen Intel(R) Core(TM) i5-12490F
stepping        : 2
microcode       : 0x2c
cpu MHz         : 2995.201
cache size      : 20480 KB
physical id     : 0
siblings        : 3
core id         : 2
cpu cores       : 3
apicid          : 2
initial apicid  : 2
fpu             : yes
fpu_exception   : yes
cpuid level     : 32
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb sha_ni xsaveopt xsavec xgetbv1 arat umip pku ospke gfni vaes vpclmulqdq spec_ctrl intel_stibp flush_l1d arch_capabilities
bogomips        : 5990.40
clflush size    : 64
cache_alignment : 64
address sizes   : 45 bits physical, 48 bits virtual
power management:
```

### 查看根目录内容

```
bash
[root@localhost ~]# ls /
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```

### Linux 中延迟重启的命令

RHEL 服务器 30 分钟后自动重启。

```
bash

[root@localhost ~]# shutdown -c +30
Broadcast message from root@localhost.localdomain (Fri 2024-12-13 11:01:26 CST):
+30
The system shutdown has been cancelled at Fri 2024-12-13 11:02:26 CST!
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
