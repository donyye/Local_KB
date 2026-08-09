ESXI-服务

2023年4月10日

13:43

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

停止hostd 服务顺序

/etc/init.d/vpxa stop

/etc/init.d/hostd  stop

\-\-\--启动

/etc/init.d/hostd start

/etc/init.d/vpxa start

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\
\
重启ESXI所有服务，在ESXI卡顿，命令也卡顿，或是输入任何命令返回""

services.sh restart

 

运行前需要确认：

1. ESXI 是否VSAN 的node,因为运行这个命令时 node 会在VC上失去连接，需要重启完成后再会被连接上。

2. 需要检查是否有lacp，如果有就会断开。

检查如下：

\# esxcfg-vswitch -l

 

\[root@localhost:\~\] localcli network vswitch dvs vmware lacp config get

Errors:

LACP is supported on VMware DVSwitch only, and there is no DVSwitch configured on host.

\## 说明没有配置 LACP

 

已使用 OneNote 创建。
