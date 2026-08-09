VSAN RVC 日志收集

2020年10月10日

13:21

[ ]收VSAN RVC日志。

 

1. SSH 登陆到 vCenter 

![Machine generated alternative text: To escape to local shell, press \'Ctrl+Alt+\] \' . VMware vCenter Server Appliance 6.7.ø.44øøe Type: vCenter Server with an embedded Platform Services Controller Connected to service \* List APIs: \"help api list\" \* List Plugins: \"help pi list\" \* Launch BASH: \"shell\" Command\> Command\> shell Shell access is granted to root root@vcsa \[ ](attachments/Technology_ALL_VMware_分析案例_003_VSAN%20RVC%20日志收集_001.png)

 

2. 登陆 RVC 注意：使用有管理权限的账户，并且\@vcenteripaddress，例如 10.10.40.250 或者 localhost。

[ ]例如：[administrator@vsphere.local@](mailto:administrator@vsphere.local@10.117.23.249)10.10.40.250

![[Technology_ALL_VMware_分析案例_003_VSAN RVC 日志收集_002.png]]

 

3. 开启录屏功能, 指定路径到/tmp/rvc.log 

basic.screenlog -e -f /tmp/rvc.log

![[Technology_ALL_VMware_分析案例_003_VSAN RVC 日志收集_003.png]]

 

4. 收集 RVC 日志 vsan.support_information 1 

![[Technology_ALL_VMware_分析案例_003_VSAN RVC 日志收集_004.png]]

 

5. 关闭录屏功能 

basic.screenlog --d  

![[Technology_ALL_VMware_分析案例_003_VSAN RVC 日志收集_005.png]]

 

 

6. 从/tmp 下载输出文件

![[Technology_ALL_VMware_分析案例_003_VSAN RVC 日志收集_006.png]]

 

已使用 OneNote 创建。
