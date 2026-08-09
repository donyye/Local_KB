Lsi_mr3

Saturday, March 26, 2016

9:38 AM

 

 

5.5 U3版本系统镜像: [http://downloads.dell.com/FOLDER03611558M/1/VMware-VMvisor-Installer-5.5.0.update03-3568722.x86_64-Dell_Customized-A05.iso](http://downloads.dell.com/FOLDER03611558M/1/VMware-VMvisor-Installer-5.5.0.update03-3568722.x86_64-Dell_Customized-A05.iso)

6.0 U1版本系统镜像: [http://downloads.dell.com/FOLDER03611537M/1/VMware-VMvisor-Installer-6.0.0.update01-3568940.x86_64-Dell_Customized-A05.iso](http://downloads.dell.com/FOLDER03611537M/1/VMware-VMvisor-Installer-6.0.0.update01-3568940.x86_64-Dell_Customized-A05.iso)

稍后请您先安装测试一下看看这些版本是否会有异常.

 

另外,

1  以下是阵列卡最新固件程序,请您先下载好准备着,到时候上门工程师到现场会用的上.

阵列卡固件: [http://downloads.dell.com/FOLDER03008727M/1/SAS-RAID_Firmware_WN0HC_WN32_25.3.0.0016_A04.EXE](http://downloads.dell.com/FOLDER03008727M/1/SAS-RAID_Firmware_WN0HC_WN32_25.3.0.0016_A04.EXE)

 

2 附件是最新的阵列卡驱动程序,若是使用5.5 U3光盘安装之后依旧会出现紫屏,则您可以尝试在系统下更新此驱动测试

 

 

i.  先使用以下命令: esxcli storage core adapter list确认当前的阵列卡驱动的信息是否是lsi_mr3(此为vmware默认驱动,会引发问题). 之后使用WINSCP类似工具将附件驱动解压缩后包上传到ESXi主机的某一个目录，如/tmp

ii. 安装：esxcli software vib install --d  /tmp/megaraid_perc9-6.903.55.00-offline_bundle-3485212.zip 

iii.禁用原来的驱动：esxcli system module set \--enabled=false \--module=lsi_mr3

iv. 重启ESXi主机

v.  查看驱动是否重新安装成功：esxcli storage core adapter list

vi. 检查是否是类似如下输出

                               vii.              HBA Name  Driver          Link State  UID             Description

\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

vmhba2    megaraid_perc9  link-n/a    unknown.vmhba2  (0:3:0.0) LSI / Symbios Logic Dell PERC H730 Adapter

 

 

 

出现了错误：

 

![[Technology_ALL_VMware_分析案例_026_Lsi_mr3_001.jpg]]

 

有可能是没有进入维护模式安装

 

 

\-\-\-\-\-\-\-\-\-\-\--如何进入维护模式\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

<https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2125510>

要将 ESXi 主机置于维护模式，请右键单击该 ESXi 主机，然后单击"进入维护模式"。 或者，您也可以从 ESXi 主机命令行运行以下命令：

 

\# esxcli system maintenanceMode set \--enable true

 

运行以下命令以确保该主机处于维护模式：

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

退出维护模式： \# esxcli system maintenanceMode set \--enable false

 

\# esxcli system maintenanceMode get

 

您会看到以下输出：

 

Enabled（维护模式）

Disabled （退出维护模式）

 

启动成功安装看到的信息：

Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective. Reboot Required: true VIBs Installed:

 

运行以下命令重新引导 ESXi 主机：

 

\# esxcli system shutdown reboot -r \'replace driver\'

 

 

 

已使用 OneNote 创建。
