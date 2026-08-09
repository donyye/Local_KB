Vsphere log 收集截图

2018年10月15日

13:56

方法一：通过vCenter收集ESXi主机日志

 

1.通过Web Client登陆vCenter

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_001.png]]

 

2.点击"主机与集群"，右键点击vCenter或者特定主机，选择"导出系统日志"

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_002.png]]

 

3.如果选择的是vCenter，则需要选择需要收集日志的主机。如果需要收集全部主机的话，则所有主机全部选上。

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_003.png]]

 

4.收集vCenter日志与Web Client日志，请勾选下面的选项。

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_004.png]]

 

5. 点击Next后Select logs不需要收集。收集过程中会依次收集每台主机日志与vCenter日志。

 

 

方法二：通过管理界面收集vCenter日志

1.登陆vCenter的管理界面：https://VCIPADDRESS:5480,使用root用户登录

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_005.png]]

 

2. 选择生成日志包。

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_006.png]]

 

 

方法三：通过命令行收集vCenter日志

1. 使用root用户SSH登录vCenter，进入shell。

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_007.png]]

 

2. 执行vc-support -l 命令收集日志

![Machine generated alternative text: root@VC \[ \]# vc-support -l vm---support v3.2: 12:5ø:11, action threads 4 Adding ](attachments/Technology_ALL_VMware_分析案例_002_Vsphere%20log%20收集截图_008.png)

 

3. 在/storage/log/ 路径下下载日志

 

 

方法四：在Windows下收集vCenter日志

1.登陆vCenter Server 、安装的Windows。

2.点击开始--所有程序--VMware --生成vCenter日志包。

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_009.png]]

 

 

=====================

 

![Machine generated alternative text: \[D\] g Q vcsa.ddoonnyy.com vcsa.ddoonn v DDONY v ClusterD esxi_b esxi_c. esxi_d RHEL7 RHEL7 • vcsa.ddoonnyy.com ](attachments/Technology_ALL_VMware_分析案例_002_Vsphere%20log%20收集截图_010.png)

 

 

 

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_011.png]]

 

 

 

![[Technology_ALL_VMware_分析案例_002_Vsphere log 收集截图_012.png]]

 

 

![Machine generated alternative text: vcsa.ddoonnyy.com 2 esxl esxl esxl esxl b.ddoonnyy_co 02_ddoonnyy.c cddoonnyy_co d.ddoonnyy_co vCenter Sewer vSphere LJI fi\] ClusterDD ClusterDD ClusterDD 6.7.0 6.7.0 6.7.0 6.7.0 vCenter Sewer vSphere LJI Client center server vcenter server ](attachments/Technology_ALL_VMware_分析案例_002_Vsphere%20log%20收集截图_013.png)

 

已使用 OneNote 创建。
