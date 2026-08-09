SUSE 网卡配置

2024年9月5日

9:22

SUSE11 XP4

linux-u6o3:/etc/sysconfig/network \# cat ifcfg-em1 

BOOTPROTO=\'dhcp\'

BROADCAST=\'\'        # 广播地址

ETHTOOL_OPTIONS=\'\'

IPADDR=\'\'     #IP地址

MTU=\'\'        #MTU，一般是1500

NAME=\'NetXtreme BCM5720 Gigabit Ethernet PCIe\' 

NETMASK=\'\'   #子网掩码

NETWORK=\'\'   # 网关

REMOTE_IPADDR=\'\'

STARTMODE=\'auto\'   #随机启动

USERCONTROL=\'no\'

 

\-\-\-\-\-\-\-\--SUSE_12\-\-\-\-\-\-\-\-\--

IPADDR=192.168.1.110 

NETMASK=255.255.255.0 

NETWORK=192.168.1.0 

BROADCAST=192.168.1.255

 

#ifconfig eth0 静态IP地址 netmask 掩码 up

#route add --net DNS的IP地址 netmask 掩码 gw 静态IP地址

 

临时添加IP：

ifconfig eth0 192.168.1.22 netmask 255.255.255.0 up

route add default gw 10.10.40.212   #添加默认路由，发现重启系统会消失。写到"/etc/rc.d/after.local"。

before.local 和 after.local

这个是由/etc/init.d/rc里面来控制的，发生在切换运行级别之前和之后。

结论：如果你想进入系统最后执行的一个变量和参数。应该写入文件after.local

==================

通过图形化修改： SUSE 12

\# yast2 network

===================

 

 

\-\-\-\-\-\-\-\--SUSE_15\-\-\-\-\-\-\-\-\--

网卡设置：

NAME=\'\'

BOOTPROTO=\'static\'

STARTMODE=\'auto\'

ZONE=\'\'

IPADDR=\'10.10.40.105/16\'

\# 这里不用添加网关

 

修改默认路由出外网：

![[__ SUSE ___007_SUSE 网卡配置_001.png]]

\# cat /etc/sysconfig/network/routes

default      10.10.40.212        -                -

例子：

![[__ SUSE ___007_SUSE 网卡配置_002.png]]

 

![[__ SUSE ___007_SUSE 网卡配置_003.png]]

 

添加 nameserver :

\# cat /etc/sysconfig/network/config

NETCONFIG_DNS_STATIC_SERVERS=\"8.8.8.8\"

\# 配置完成后就可以出外网了。

\-\-\-\-\--其它

\-\-\-\-\-\-\--添加/删除到指定网络的路由规则

route add -net 192.168.20.0 netmask 255.255.255.0 dev eth1

route del -net 192.168.20.0 netmask 255.255.255.0 dev eth1

\-\-\-\-\-\-\-\--添加/删除默认网关路由

route add default gw 192.168.1.1 eth0

route del default gw 192.168.1.1 eth0

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\--SUSE15 sp6 \-\-\-\-\-\-\-\-\-\-\-\-\-\--

[ ip route add default via 10.120.146.203 dev eth1 onlink][  \--\> ]默认网关临时添加的方法

\
\# vim /etc/resolv.conf \
nameserver 10.7.7.7

 

已使用 OneNote 创建。
