Host 出现SSL签名错误

2019年12月31日

10:47

\[XC740XD\] \[Internal-Case: 47141072\] \[ST: 4VPYCT2\] \[SR: 1008031508\]

 

![Machine generated alternative text: nthcmgcld01a.mmton.com iCßR Update Manager nthcmgcId01a.nuuoton.com NTHCMGVCS01.nuvoton.com NTHCMGVCSOI v W.ITHCMGVCSOI nthcmgcldl\]l a.nuvoton com amluxl (disconnected) hqipqclt (disconnected) 6\] hqtvrdsl (disconnected) 6b hqtvrds10 6\] hqtvrdsl 3 (disconnected) 6b hqtvrds14 6bhqtvrds2 6\] hqtvrds3 (disconnected) 6bhqtvrds4 RI hmvrds5 (discnnnertern Hypervisor NC vwvare ESXi, 68.0, 9298722 DAI Inc. XC740Xd-12 htel(R) xeon(R) GOId 6152 CPU @ 2.1 OGHZ 88 26 CPU EX: 000 Hz 000 B 000 B vcenter server. 000 H: 0.00 Hz 000 B 000 B nthcmgcldfll a.nuvoton.com ](attachments/Technology_ALL_VMware_分析案例_104_Host%20出现SSL签名错误_001.png)

 

![[Technology_ALL_VMware_分析案例_104_Host 出现SSL签名错误_002.png]]

 

![[Technology_ALL_VMware_分析案例_104_Host 出现SSL签名错误_003.png]]

 

 

Root cause：

经客户确认，DNS上的主机名称的解释被idrac的IP取代，导致VC关了的node出现disconnect的问题。在更新完成DNS record后，在VC选择node重新连接，目前功能已经恢复正常。

 

solution：

从VC log可以看到 01a 和01c的主机指纹确实 已经发生变化，expected thumbprint跟PeerThumbprint不匹配，导致无法建立SSL连接，log片段在最下面有附上。

检查主机的log发现， 这个问题可能在12月18日就已经出现了，我们推荐客户是否可以回想下18日那天是否做过任何改动，

比如修改过DNS或者DNS中的HOST记录？  或者修改过ESXi的主机名？或者任何其他事务/变动？ 如果有明确的改动，是否可以还原到改动前？

ESXi日志中的日志条目已经轮换掉，无法检查到有效内容，所以在ESXi日志中无法追查到原因。

 

如果用户确定无法追溯到18日采取过什么行动，那么这个问题解决办法如下：

1.  检查nthcmgcld01c和nthcmgcld01a 的正向和反向的DNS解析记录是否完全正确,IP地址是否完全匹配

nslookup  nthcmgcld01c      （注：正向）

nslookup   esxi-ip address     （注：反向）

1.  在DNS正反向解析都OK的情况下，请在vCenter中的01c或者01a上点右键，在弹出的菜单中选择"Remove from inventory"，如果该选项允许执行并且能够完成，那么等待该任务完成后，将主机重新加入vCenter.

![[Technology_ALL_VMware_分析案例_104_Host 出现SSL签名错误_004.jpg]]

 

1.  如果vCenter提示该主机必须进入维护模式才能够完成" Remove from inventory",  请将该主机上的所有VM正常关机，（提醒，请务必注意NUTANIX CVM的关机顺序和预先检查Cluster状态/设置是否可以关机）
2.  然后直接登陆ESXi主机，将其进入维护模式，

命令行: esxcli system maintenanceMode set --enable true

1.  然后在vCenter中问题主机上点右键，选择"remove from inventory"，将其移出vCenter，带任务完成后，将主机重新加入vCenter.

 

 

日志记录：

VC1 (node x3)    

-nthcmgcld01c

 

vCenter record:

\--\> PeerThumbprint: 84:1A:DB:48:68:4E:CA:5E:78:5E:5B:67:F9:36:CB:FC:11:D0:AF:9A

\--\> ExpectedThumbprint: F3:73:7A:74:4C:FD:35:15:C6:BC:DD:4A:28:DC:D1:26:73:B0:C7:F0

 

 

 

VC2 (node x2)    

- nthcmgcld01a 

 

vCenter record:

\--\> PeerThumbprint: 64:8D:01:2D:3D:08:AC:DF:61:12:36:17:4D:77:96:44:58:B0:24:3B

\--\> ExpectedThumbprint: 56:50:27:09:0A:A2:96:9C:17:9A:4A:A2:AA:5A:10:DD:F4:C1:B7:5C

 

 

 

 

已使用 OneNote 创建。
