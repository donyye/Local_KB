[\[RHEL9\]fences]-idrac

2024年4月28日

13:27

\# yum install fence-agents-all

\# pcs stonith list \--\>[  ]列出所支持的所有fence

\# pcs stonith describe fence_idrac \--\> 查看fence资源使用的参数

 

10.10.101.175[  \--\>   node1 10.10.40.176]

10.10.101.75[    \--\>   node2 10.10.40.178]\
 

设置 idraac fenc：[\
\[root@localhost \~\]# pcs stonith create fence_node1 fence_idrac ipaddr=\"10.10.101.175\" delay=\"0\" lanplus=\"1\" login=\"root\" passwd=\"calvin\" method=\"onoff\" pcmk_host_list=\"node1.abc.com\"]

Warning: stonith option \'ipaddr\' is deprecated and should not be used, use \'ip\' instead

Warning: stonith option \'login\' is deprecated and should not be used, use \'username\' instead

Warning: stonith option \'passwd_script\' is deprecated and should not be used, use \'password_script\' instead

 

新命令：

[\[root@localhost \~\]# pcs stonith create fence_node2 fence_idrac ip=\"10.10.101.75\" delay=\"5\" lanplus=\"1\" username=\"root\" password=\"calvin\" method=\"onoff\" pcmk_host_list=\"node2.abc.com\"]

 

配置成功后：

![[RHEL Cluster Test_004_[RHEL9]fences-idrac_001.png]]

 

[\[root@localhost \~\]# pcs stonith ]

  \* fence_node1        (stonith:fence_idrac):         Started node1.abc.com

  \* fence_node2        (stonith:fence_idrac):         Started node1.abc.com

\# 每一个node配置一个fenc资源，另外系统能访问到 idrac IP。

 

参数解释：

1）delay , 这个设置需要根据实际情况来，如果是主节点延时应该要低于辅节点，这样出现脑裂的时候它能先发起fenc动作，另外如果要设置kdump也需要加上它的延时时间，一般是加 15秒。

\# pcs stonith update fence_node2 delay=\"15\"[   ]\--[  \> ]修改，delay 改成 15秒。

2）lanplus, 1是开启 IPMI 通道。

3）method ，如果使用 cycle 会有提示再 pcs status 上，如下。所以选用 \"onoff\"

WARNINGS:

Following stonith devices have the \'method\' option set to \'cycle\' which is potentially dangerous, please consider using \'onoff\': \'fence_node1\', \'fence_node2\'

\# pcs stonith update fence_node1 method=\"onoff\"[  \<\-- ]修改完后就没有再报

 

\# pcs stonith describe fence_idrac[  ]查看此模块的具体参数

 

添加 fenc 后修改

\# pcs property set stonith-enabled=true

\# pcs property config

......

stonith-enabled: true

 

 

清楚fenc错误记录

[\[root@localhost \~\]# pcs resource cleanup]

Cleaned up all resources on all nodes

Waiting for 4 replies from the controller

\... got reply

\... got reply

\... got reply

\... got reply (done)

 

 

测试手动 fence node2

[\[root@localhost \~\]# pcs stonith fence node2.abc.com]

Node: node2.abc.com fenced[   \<\-- ]成功

查看状态会有记录：

![[RHEL Cluster Test_004_[RHEL9]fences-idrac_002.png]]

 

\# pcs config[  \<\-- ]查看集群配置文件

 

查看fence的配置与参数

[\[root@localhost \~\]# pcs stonith config fence_node1]

Resource: fence_node1 (class=stonith type=fence_idrac)

  Attributes: fence_node1-instance_attributes

    delay=5

    ip=10.10.101.175

    lanplus=1

    method=onoff

    password=calvin

    pcmk_host_list=node1.abc.com

    username=root

  Operations:

    monitor: fence_node1-monitor-interval-60s

      interval=60s

 

[\[root@localhost \~\]# pcs stonith config fence_node2]

Resource: fence_node2 (class=stonith type=fence_idrac)

  Attributes: fence_node2-instance_attributes

    delay=15

    ip=10.10.101.75

    lanplus=1

    method=onoff

    password=calvin

    pcmk_host_list=node2.abc.com

    username=root

  Operations:

    monitor: fence_node2-monitor-interval-60s

      interval=60s

 

 

》由于错误"Fence agent did not complete in time"，操作在 fence_rhevm 上失败

<https://access.redhat.com/solutions/6987563>

 

====================

测试idrac自身的可用性和连通性：

 

说明是可联通的

[\[root@localhost \~\]# ipmitool -I lanplus -U root -P calvin -H 10.10.101.175 chassis status]

System Power         : on

Power Overload       : false

Power Interlock      : inactive

Main Power Fault     : false

Power Control Fault  : false

Power Restore Policy : previous

Last Power Event     :

Chassis Intrusion    : active

Front-Panel Lockout  : inactive

Drive Fault          : false

Cooling/Fan Fault    : false

Sleep Button Disable : not allowed

Diag Button Disable  : not allowed

Reset Button Disable : not allowed

Power Button Disable : allowed

Sleep Button Disabled: false

Diag Button Disabled : false

Reset Button Disabled: false

Power Button Disabled: false

 

 

说明联通有问题

[\[root@localhost \~\]# ipmitool -I lanplus -U root -P calvin -H 10.10.101.75 chassis status]

Error: Unable to establish IPMI v2 / RMCP+ session

 

加个 -v 参数可以看到更多

[\[root@localhost \~\]# ipmitool -I lanplus -U root -P calvin -H 10.10.101.75 chassis status -v]

Get Auth Capabilities error

Error issuing Get Channel Authentication Capabilities request

Error: Unable to establish IPMI v2 / RMCP+ session

 

检测 idrac 设置

1）是否ipmi有enable。

![[RHEL Cluster Test_004_[RHEL9]fences-idrac_003.png]]

 

 

2）查看设置的用户是否有权限

![[RHEL Cluster Test_004_[RHEL9]fences-idrac_004.png]]

 

 

 

=========

配置时遇到的一个错误，再使用命令配置fenc的时候，idrac 密码参数用的 password\_scrip，这个参数的意思是运行脚本以检索密码，主要是为了安全，不用明文。

应该改成 password ,可以删除掉再重新配置。

 

当时的截图：

![[RHEL Cluster Test_004_[RHEL9]fences-idrac_005.png]]

 

 

 

Web UI    <https://x.x.x.x:2224>

 

![[RHEL Cluster Test_004_[RHEL9]fences-idrac_006.png]]

 

 

 

 

已使用 OneNote 创建。
