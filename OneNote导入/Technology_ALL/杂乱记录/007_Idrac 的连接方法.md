Idrac 的连接方法

2020年9月2日

10:58

Idrac 的连接方法参考：

 

1．通过一条普通安卓手机数据线（Micro B USB）连接服务器iDRAC Direct端口和管理机的USB端口。

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_001.jpg]]

 

2.接上数据线后管理机会自动安装Idrac Virtual NIC USB Device.驱动自动安装好后在"设备管理器"---"Network Adapter"上会多出一个Remote NDIS Compatible Device #2设备。

 

自动安装驱动截图：

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_002.jpg]]

 

安装好驱动后设备管理器截图：

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_003.jpg]]

 

3.等待几分钟后管理机会自动获取到169.254.0.4的IP，iDrac获取到的IP为169.254.0.3. （建议连接USB线之前断开无线网卡和有线网卡的连接），然后从管理机PING iDRAC IP测试。

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_004.jpg]]

 

4.在浏览器中输入iDRAC IP 169.254.0.3后登录到iDRAC的WEB 界面，附Idrac WEB界面截图。

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_005.jpg]]

 

5.如果接上usb线没有识别到设备，需要进入Idrac Settings---Media and USB Port Settings检查USB Management Port Mode是否为Idrac Direct.

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_006.jpg]]

 

6.更多详细信息请参考Idrac9用户手册中"使用 USB 端口进行服务器管理"（第266页）

[https://topics-cdn.dell.com/pdf/idrac9-lifecycle-controller-v3.00.00.00_users-guide_zh-cn.pdf](https://topics-cdn.dell.com/pdf/idrac9-lifecycle-controller-v3.00.00.00_users-guide_zh-cn.pdf)

![[Technology_ALL_杂乱记录_007_Idrac 的连接方法_007.jpg]]

 

已使用 OneNote 创建。
