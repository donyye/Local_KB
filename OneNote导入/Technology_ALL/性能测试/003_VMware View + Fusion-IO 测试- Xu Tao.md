VMware View + Fusion-IO 测试- Xu Tao

Tuesday, May 27, 2014

9:39 AM

 

测试项目

xx银行现有VDI环境添加Fusion-IO设备并提升现有VDI环境IO性能

 

测试人员

Xu  Tao

 

测试时间

2014\. 5. 19

 

测试背景

[ ]

[ xx]银行已使用构建在Dell服务器及存储上的VMware View VDI环境在自己的开发环境中。目前用户计划在现有VDI环境中增加桌面容量，需要提升现有VDI环境的IO性能。SC给用户推荐了Fusion-IO高IO性能设备，此次就是在用户现有环境中加入Fusion-IO卡并评估其性能。

 

测试过程：

安装Fusion-IO驱动，如下：

![[Technology_ALL_性能测试_003_VMware View + Fusion-IO 测试- Xu Tao_001.png]]

[         ]在ESX内添加Datastor，如果报告Fusion-IO设备格式化报错，需要把Fusion-IO设备的block size从4K改成512。方式如下：

![[Technology_ALL_性能测试_003_VMware View + Fusion-IO 测试- Xu Tao_002.png]]

最后添加Datastore并且把高IO负载的虚拟机迁移到Fusion-IO设备上来。

![[Technology_ALL_性能测试_003_VMware View + Fusion-IO 测试- Xu Tao_003.png]]

 

 

已使用 OneNote 创建。
