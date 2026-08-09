vmware链路聚合

2022年7月26日

9:52

链路聚合有两种，如果是静态的，只开启ip hash就可以了。DVS 和 VS都能做。

如果是LACP，还要在VDS里面创建lag group，只能在 DVS上做。

 

如下，在DVS上新建一个 lag group。

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_001.png]]

 

 

名称随便，其它默认就可以

模式那边有活动和被动两种。

被动： 表示它可以获取或接收 LACP 消息 （默认被动）

活动：表示它可以发送 LACP 消息。

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_002.png]]

 

创建完成后：

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_003.png]]

 

 

在拓扑标签上看到：

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_004.png]]

 

 

 

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_005.png]]

 

或者是下面入口

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_006.png]]

 

 

如果把lag1移动到活动链路，其它都放到味使用的链路，否则 lacp 会起不来。

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_007.png]]

 

 

配置完成后

![[Technology_ALL_VMware_分析案例_140_vmware链路聚合_008.png]]

 

 

 

 

已使用 OneNote 创建。
