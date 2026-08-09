Workshop 实验报告

Thursday, December 12, 2013

8:35 AM

WORKSHOP主题：ESX5i connect MD3220 i

实验时间：2013-12-11 13:30-17:30

操作人员：Mars_he

实验环境：TR1004

实验步骤与截图：

链接实验室无线网络，隐藏SSID:TR1004 密码：Passw0rd

RDP远程桌面连接到10.206.210.30；用户名: student16；密码: 123456

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_001.png]]

登录https://10.206.210.43/cloud/org/glndccc/#/vmListPage?listId=AllVMListId&filterColumn=All

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_002.jpg]]

创建2008和ESXI的虚拟机

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_003.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_004.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_005.jpg]]

启动虚拟机安装MDSS和V_client

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_006.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_007.jpg]]

 

ESXI 部署完后重启，

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_008.jpg]]

设置ESXI的IP

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_009.jpg]]

 

2008上登录VSphere，输入ESXI服务器的IP、用户名和密码。

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_010.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_011.jpg]]

选择【configuration】

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_012.jpg]]

、

添加和设置网卡

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_013.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_014.png]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_015.jpg]]

添加ISCSI adapter， 注意WWN的位置，在MD中定义主机的时候会用到。

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_016.jpg]]

链接MD storage

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_017.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_018.jpg]]

MD 添加2个VD

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_019.jpg]]

定义主机

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_020.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_021.jpg]]

映射LUN给主机

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_022.png]]

映射好的主机和LUN如下：

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_023.jpg]]

以下为在vsphere中查看映射好的设备和路径：

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_024.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_025.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_026.jpg]]

在VSphere 的Storage项目添加映射的磁盘

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_027.jpg]]

 

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_028.jpg]]

添加后的磁盘如下：

![[Technology_ALL_未分类知识库_006_Workshop 实验报告_029.jpg]]

 

实验总结：

ESXI中链接MD 存储总体步骤和windows中一样， 不过ESXI需要借助VSphere来实现设备的添加和链接，通过这个实验我掌握了, ESXi服务器系统部署和系统配置、vSphere Client 修改ESXI的配置、iSCSI适配器的添加、iSCSI发起、查看挂载的Lun、查看多路径、添加已挂载的Lun到ESXi服务器的存储设备中。

 

已使用 OneNote 创建。
