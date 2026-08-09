ESXi 主机下抓取Dset log和TTY获取方法 - tools[  - ]日志工具 ST FMD8Y2X

Tuesday, August 12, 2014

9:14 AM

  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Re: 转发: ESXi 主机下抓取Dset log和TTY获取方法 - tools[  - ]日志工具 ST FMD8Y2X
  发件人     [chris.lu@cn.atlascopco.com](mailto:chris.lu@cn.atlascopco.com)
  收件人     Wen, Jeffery
  发送时间   Friday, March 07, 2014 9:32 AM
  附件       \<\<dset.zip\>\>
  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

收集Dset log

 

1:登录Vcenter server ,选择对应主机，选择"configurtation" \-\--"security profile"\-\-\--"properties" ,选中"ssh"\-\-\-\-\--"options"\-\-\-\-\--"start and stop manaually", 然后选择下面的"start" ,确认ssh服务已经开启

![[Technology_ALL_VMware_分析案例_027_ESXi 主机下抓取Dset log和TTY获取方法 - tools  - 日志工_001.jpg]]

 

2：下载omsa offline bundle 组件，并执行以下命令安装

Esxi 5.1 OMSA:

[http://downloads.dell.com/FOLDER01816678M/1/OM-SrvAdmin-Dell-Web-7.3.0-588_A00.VIB-ESX51i.zip](http://downloads.dell.com/FOLDER01816678M/1/OM-SrvAdmin-Dell-Web-7.3.0-588_A00.VIB-ESX51i.zip)

 

文件名：OM-SrvAdmin-Dell-Web-7.3.0-588_A00.VIB-ESX51i.zip

 

假设上传至ESXi 主机的/tmp目录下，

命令：esxcli software vib install --d /tmp/OM-SrvAdmin-Dell-Web-7.1.0-5304.VIB-ESX50i_A00.zip

 

确认返回结果为安装成功，如有失败请截图返回邮件。

 

 

3：选择一台能够ping通ESXi 主机的window主机，下载Dset 3.3 并安装

URL：[http://support.dell.com/support/topics/global.aspx/support/en/dell_system_tool?c=us&cs=555&l=en&s=biz&\~ck=anavml](http://support.dell.com/support/topics/global.aspx/support/en/dell_system_tool?c=us&cs=555&l=en&s=biz&~ck=anavml)

选择windows 版本 

![[Technology_ALL_VMware_分析案例_027_ESXi 主机下抓取Dset log和TTY获取方法 - tools  - 日志工_002.png]]

 

运行安装程序，按照下图提示进行安装。

![[Technology_ALL_VMware_分析案例_027_ESXi 主机下抓取Dset log和TTY获取方法 - tools  - 日志工_003.png]]

 

![[Technology_ALL_VMware_分析案例_027_ESXi 主机下抓取Dset log和TTY获取方法 - tools  - 日志工_004.png]]

 

![[Technology_ALL_VMware_分析案例_027_ESXi 主机下抓取Dset log和TTY获取方法 - tools  - 日志工_005.png]]

 

 

安装完成后，在开始菜单中可以看到对应选项，请点右键，选择以管理员的身份来运行

 

![[Technology_ALL_VMware_分析案例_027_ESXi 主机下抓取Dset log和TTY获取方法 - tools  - 日志工_006.png]]

 

在出现的命令行窗口中，输入以下命令

 

DellSystemInfo.exe -s esx主机IP -u root -p root用户密码 -d hw,st,sw -r C:\\dset.zip

 

将产生的C:\\dset.zip 发给我们。

 

如果运行错误，请执行以下命令

DellSystemInfo.exe -s esx主机IP -u root -p root用户密码  hw,st,sw -r /tmp/dset.zip

此时日志存在了ESXi主机的/tmp目录下

请通过sftp下载/tmp/dset.zip日志包发给我们。

 

 

二：收集vm-support log

 

通过ssh 或者ESXi主机的local console

执行vm-support 命令， 系统会自动生成日志包，名字是主机名＋vmsupport+年月日.tgz 

请将此包通过sftp或者其他ssh远程工具下载后发给我们

 

 

 

[Dell TechDirect](http://www.techdirect.com/) \| 戴尔在线报修门户网站: 提供在线报修，自主部件派单以及在线管理报修事件 

回复邮件获取详细资料

 

 

已使用 OneNote 创建。
