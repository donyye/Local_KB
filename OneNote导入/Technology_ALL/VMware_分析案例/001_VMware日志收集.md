VMware日志收集

2017年10月9日

15:47

1. vCenter support log 

a.        (在vCenter中选择导出日志，注意不需要选择ESXi主机，只需要确保选中"vCenter和web Client"然后导出)

<https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vcenter.install.doc/GUID-2516314E-1492-4BBA-9070-5560119996F4.html>

 

2. VR support log

收集方法请参见下面文章

a.        <https://recommender.vmware.com/solution/SOL-3776>

 

3. ESXI 日志收集

a.[     vm-support  ]命令产生的输出文件会存放在当前的目录中，被命名为esx-xxxx.tgz

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

通过vsphere web client 收集日志：

<https://kb.vmware.com/s/article/2032892?lang=zh_CN>

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

客户反馈horizon 重构 报错

客户反馈同一个admin发布重构任务，有的虚机成功，有的出现报错。

Order  809376845

VMware Horizon Advanced, 10 Pack, Named User, 1 Year, China

\-\-\--

请联系客户收集以下几个日志回来分析:

1\. vCenter server support log

2\. ESXi vm-support log

3\. View connection server log

4\. View composer server log

 

 

[https://kb.vmware.com/s/article/2032892](https://kb.vmware.com/s/article/2032892)

 

<https://kb.vmware.com/s/article/2081960?r=1&Quarterback.validateRoute=1&KM_Utility.getArticleData=1&KM_Utility.getGUser=1&KM_Utility.getArticleLanguage=1&KM_Utility.getArticle=1>

 

 

VMware license：

 

1. Horizon license （例子 order：480164303）

这个license（VMware Horizon Advanced） 有Horizon与vSAN。

<https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/products/horizon/vmware-horizon-pricing-packaging-guide.pdf>

![[Technology_ALL_VMware_分析案例_001_VMware日志收集_001.png]]

 

 

==============================================

 

收集 VMware Horizon View 日志和诊断信息

 

1. vm-support log 

在ESXI命令界面输入 vm-support命令收集日志。

 

2. view connection server log

要获取 View 连接服务器、View 安全服务器、View 传输服务器和注册服务器的诊断信息，请执行以下操作：

1)   登录到 View 连接服务器。

2)   导航到开始\>程序\>VMware。

3)   单击生成 View 连接服务器日志包。

在桌面上，文件夹 vdm-sdct 包含压缩的日志文件。

 

3. View Composer Server log

要获取 View Composer Server 的诊断信息，请执行以下操作：

1)   登录到 View Composer Server。

2)   导航到开始\>所有程序\>VMware\>VMware View Composer。

3)   选择创建 View Composer 支持日志。

将在桌面

 

4. view agent log

登录到安装了 View Agent 的虚拟机。

打开命令提示符并执行：

\"C:\\Program Files\\VMware\\VMware View\\Agent\\DCT\\support.bat\"

注意：在 View 4.0.1 及更高版本中，View Agent 支持包还会收集 Composer Guest Agent 日志。在 View 4.0 中，可以通过运行以下命令手动收集：

\"C:\\Program Files\\Common Files\\VMware\\View Composer Guest Agent\\svi-ga-support.vbs\"

在桌面上，文件夹 vdm-sdct 包含压缩的日志文件。

 

5. Horizon client log

以管理员身份打开命令行窗口，运行下面的命令来收集horizon client日志并发给我。

32-bit: C:\\Program Files\\VMware\\VMware Horizon View Client\\DCT\\support.bat

64-bit: C:\\Program Files (x86)\\VMware\\VMware Horizon View Client\\DCT\\support.bat

 

 

FYI：[https://kb.vmware.com/s/article/1017939?lang=zh_CN](https://kb.vmware.com/s/article/1017939?lang=zh_CN)

 

=========================================

\-\-\-\-\-\-\-\-\--VMware dump 检查与设置\-\-\-\-\-\-\-\-\-\-\--

目前检查了vm-support 日志，发现没找到在发生问题时的日志记录，原因与客户的scratch location的设置有关系。

 

这个主机的Scratch location 的设置，

 

                  (vim.option.OptionValue) {

                     dynamicType = \<unset\>,

                     dynamicProperty = (vmodl.DynamicProperty) \[\],

                     key = \'ScratchConfig.ConfiguredScratchLocation\',

                     value = \'/vmfs/volumes/5a8d1540-2f4297c8-91a3-801844e9eb3e\'

 

Scratch location 是设置在 /vmfs/volumes/5a8d1540-2f4297c8-91a3-801844e9eb3e ，但是 /vmfs/volumes 底下只有 5afe4352-1496e1cc-2a16-801844e9eb3e 和 fc25aa10-729fd985两个，所以日志没办法保存下來。

 

建议修改scratch location的设置，修改成正确的目录。

 

1.  Log in to vCenter Server using the vSphere Web Client.
2.  Click Hosts and Clusters, then select the specific host.
3.  Click the Manage tab.
4.  Click Settings \> System \> Advanced System Settings.
5.  Locate ScratchConfig.ConfiguredScratchLocation.
6.  Click Edit and add the path to the scratch directory.
7.  Reboot the host.

因为这次问题没有系统日志可查，如果之后问题有再次发生，需要从新收集日志进行分析。

 

 

 

![Machine generated alternative text: vSphere Web Client vrnware• vSphere Web Client U Administrator@VSPHERELOCAL Help Navigator / \< Back Ovcddonycom 111b DOONY esxi4ddonycom Nesxiiidonycom O Recent Objects Created esxj\$ddonycom vcddonycom 10.10.40s2 10.10.40.51 11b OOONY open-A esxi4.ddony.com Getting Stalled Summary VM Startup/Shutdown Agent VM Settings Swap file location Monitor C.\' \@Actions Configure Permissions Advanced System Settings VMS Datastores Networks update Manager Edit\... Q scratch Default VM Compatibility System Licensing Time Configuration Authentication Services Certificate Power Management Advanced System Setüngs System Resource Reservation Secu rity Profile System Swap Host Profile Hardware Processors Memory Recent Tasks Tse Check new notifications ScratchConfigConfiguredScratchLocation ScratchConfigCurrentScratchLocation SyslogÄoOal\]ogDir ,vmfsNoIumes/5ae0681a-ce05cd6f-7030-oooc2gc \[l/scratch/log The directory The directory cm Datastore pat esxi4.ddony.com - Edit Advanced System Settings Modifying configuration parameters is unsupported and can cause Continue only ifyou know what you are HBRChecksumUseAlloclnfo HBRChecksumZoneSize MemKemMinFreePct MigratePreallocLPages Scratch ConfigConfiguredScratchlsc„ ScratchConfigCurrentScratchLocation UserVarsProductLockerLocation VMFS3 OpenWithoutJournal VMkernelEootheapMetaPoison8y1e VMkernelEootpci8arAllocPolCy 32768 NmfsNolumes/5ae0681a-ce05cd6f-703 MmfsNolumes/5ae0681 a-ce05cd6f-7 /locker/packages/6 50/ Q location Use disk allocation info to help speed Size in regions of one checksum zona Percentage of host memory to reservi Attempt to prealloc destination pages The directory configured to be used fo The directory currently being used for Path to VMware Tools and vSphere Open file system when out of space fm Byte pattern used to poison red zones PCI BAR allocation policy, a---first-fit, 1 ](attachments/Technology_ALL_VMware_分析案例_001_VMware日志收集_002.png)

 

![Machine generated alternative text: vrnware• vSphere Web Client DOONY esxi4ddonycom esxl onycom open-A \$ RH75-01 RH75-02 RHELag-a RHELtg_C RHEL73-A 6:\] Storage-AA 6:\] uOuntu6_test Win2003 5b ESXI-54 5b ESXI-55 \$ VMware Center Serm Win2012R2-AD esxj4ddonycom Mvbuare vCenter Server vc.ddony.com 10104052 10.10.40.51 11b OOONY esxi4.ddony.com TCP/IP -iXö1 U Administrator@VSPHERELOCAL update Manager scratch ScratchConfigConfiguredScratchLoc„ ScratchConfigCurrentScratchLocation SyslogßloOal\]ogOir esxß.ddony.com - NmfsNolumes/5ae0681 a-ce05cd6f-7 MmfsNolumes/5ae0681a-ce05cd6f-T Il/scratch/log Lib ScratchConfig ConfiguredScratchLoc Scratch Config Currentscratchl_ocation UserVarsProductLockerLocation NmfsNolumes/5ae0681 a-ce05cd6f-703 NmfsNolumes/5ae0681a-ce05cd6f-7 /locker/packages/6 50/ Q location VMware Tools vSphere Client F-fiå ](attachments/Technology_ALL_VMware_分析案例_001_VMware日志收集_003.png)

 

 

 

===

 

1Yr ProSupport for Software, VMware HCI Kit, Standard, Per CPU

HCI=vsphere+vsan  

======

 

收集vCenter日志：

如果是Windows版本的vCenter，请进入该vCenter所在的windows桌面，点击"开始菜单"\-\-\--"程序"\-\-\-\-\-\-\--"VMware"\-\-\-\-\-\--"generate vcenter support log bundle" 来收集，收集时会跳出一个黑色命令行窗口自动收集，完成后会在桌面上生成压缩包。

 

如果是Linux版本的vCenter(VCSA)，请登陆[https://vCenter-IP:5480](https://vCenter-IP:5480)，登陆完成后右上角有个按钮是collect support log bundle，即可收集并下载support log.

 

=========

命令收集VACP 日志

<https://kb.vmware.com/s/article/1011641?lang=zh_CN>

使用命令行从 vCenter 6.0 或更高版本的 Server Appliance 或外部 Platform Services Controller 中收集支持包

打开控制台会话以进入 vCenter Server Appliance。

以管理用户身份（如 root）登录。

键入 shell.set \--enabled true，然后按 Enter。

键入 shell，然后按 Enter。

运行以下命令以将日志导出到 /storage/log/：

vc-support -l

 

 

已使用 OneNote 创建。
