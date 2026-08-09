vsan_node 网络问题

2023年9月8日

10:54

Vsan skyline 有很多错误。

HA 也有错误，显示 node1 HA 有报错。

如下图：

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_001.png]]

 

从这里可以看到vsan 网络是 通过 vmnic0 和 vmnic1 通信

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_002.png]]

 

HA 报错

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_003.png]]

 

 

Vsan 的网络是在DSW上，是 vmk1

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_004.png]]

 

 

因为 vmnic0与 vmnic1 都是走的 vsan 网络，而状态也是 up 的，说明在ESXI看到是正常的。但是在ping 其它 node 时候不通，需要测试其它 node ，看是那个 node 的 vsan 网络有问题。

esxcfg-vmknic -l

esxcfg-nics -l

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_005.png]]

node1 不能 ping nide2 node3。

node2 与 node3 可以互相 ping ，但是无法 ping node1。

说明 node1 连接有问题。

 

检查 node1 是否有发送 ARP 包出现。这里显示是有的。

pktcap-uw \--uplink vmnic0 \--capture UplinkSndKernel -o -\| tcpdump-uw -enr -

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_006.png]]

 

在 node2 查看是那个网口在接收包。看到的是vmk1 的 vmic1。

vmtop 然后按 "n"到下面的界面

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_007.png]]

 

查看这个口的 MAC 地址。

esxcfg-vmknic -l

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_008.png]]

 

然后在 node2 收抓是否能收到来自 node1 包。结果是收不到。

pktcap-uw \--uplink vmnic1 \--capture UplinkRcvKernel \--srcmak 00:50:56:6c:d2:16 -o -\| tcpdump-uw -enr -

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_009.png]]

 

再去 node3 上看看。结果也还是无法收到 node1 发出的包。

pktcap-uw \--uplink vmnic1 \--capture UplinkRcvKernel \--srcmak 00:50:56:6c:d2:16 -o -\| tcpdump-uw -enr -

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_010.png]]

 

尝试换成 vmnic0 是有收到，但是目前通信的不0。

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_011.png]]

 

 

现在尝试在 node1 上把vmnic0 down 掉，然后再测试。

esxcli network nic down -n vmnic0

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_012.png]]

 

vmnic0 down 了后 node1 ping node2  通了

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_013.png]]

 

从 node2 ping node1 也没问题

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_014.png]]

 

回到 vsan 重新测试健康状态后发现很多错误已经消失。如下图：

![[VMware-排错_VSAN_排错_017_vsan_node 网络问题_015.png]]

 

最后得出的结论是，node1 上的 vmnic0 口有问题，需要检查后端连接的交换机是否有问题。

解决node 通信问题后，HA 的问题也解决了，因为开启 vsan 后，HA心跳也是走的 vsan 网络。

 

后续：

1. 目前node1与其它node通信已经回复，在同步中，暂时不不还原回去，因为还原会再次断开，对 vsan有影响。

2. 尝试更换 vmnic0 的网线是否可以，然后通过下面步骤把vmnic0重新up起来。

1\) esxcl network nic up -n vmnic0

2) 使用 esxtop 然后按 n 检查是否 vmk1 在运作在 vmnic0上。

3）vmkping -I vak1 10.10.30.70 看是否成功。

 

 

 

 

已使用 OneNote 创建。
