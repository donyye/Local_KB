Windows Log 分析

Tuesday, July 19, 2016

3:43 PM

Mpsreport Log：

 

1. Event Log，一般看 System 和 Application 就够了。

\[Hostname\]\_evt_System : 包含系统重启记录

\[Hostname\]\_evt_Application : 检查应用是否出问题

 

2. General 里面的[ \[Hostname\]\_DRIVERS.TXT ]和[ \[Hostname\]\_Hotfix.txt]

[ \[Hostname\]\_msinfo32.nfo : ]系统的一些信息

 

3. 在Eve\_System里搜索：

 

Event ID : 6005 这是OS启动后肯定会记录的一个ID，然后往前找就可以看到重启前的一些东西。

 

Event ID : 6008 非正常重启系统时间。它有两种，一种是Bugcheck事件，另外一种是没有Bugcheck。前者一般会有Dump产生，后者往往是长按电源关机或者由于硬件问题造成的突然关机。

![[Technology_ALL_windows_case_001_Windows Log 分析_001.png]]

 

 

Event ID : 1074 是正常重启记录。所以不会有6008。如果使用idrac来shutdown或是重启系统的就只有一个1074。

 

Event ID: 6013 系统uptime时间，单位是秒。

Event ID: disk 150 系统收到lun空间快满信号，然后下线了lun但是ISCSI没断开，可手动上线

 

收集：

  ----------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -----------
  Event ID                            Issue                                                                                                                                                                                                                                                                                        Case
  6005                                这是OS启动后肯定会记录的一个ID，然后往前找就可以看到重启前的一些东西                                                                                                                                                                                                                                                                      
  6008                                非正常重启系统时间。它有两种，一种是Bugcheck事件，另外一种是没有Bugcheck。前者一般会有Dump产生，后者往往是长按电源关机或者由于硬件问题造成的突然关机。                                                                                                               
   1074   是正常重启记录。所以不会有6008。如果使用idrac来shutdown或是重启系统的就只有一个1074。                                                                                                                                                                         
  6013                                系统uptime时间，单位是秒。                                                                                                                                                                                                                    
  disk 150                            系统收到lun空间快满信号，然后下线了lun但是ISCSI没断开。可以手动上线lun。                                                                     975671992
  41                                  Kernel-power 系统已在未先正常关机的情况下重新启动。如果系统停止响应、发生崩溃或意外断电，则可能会导致此错误。                                                                                                                                                 
  1001                                已将转储的数据保存在: C:\\Windows\\MEMORY.DMP                                                                                                                                                                                                                                                 
  ----------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -----------

 

 

results\\General

[\[Hostname\]]\_RECOVERY.TXT 是否开启dump

CrashDumpEnabled        REG_DWORD        0x0[   \##]没有开启dump

CrashDumpEnabled        REG_DWORD       0x7[    ]\##已开启dump

![Machine generated alternative text: = \[Crashcontrol\] ! REG. EXE VERSION 3.0 HKEY LOCAL MACHINE\\System\\CurrentContr01Set\\Contr01\\CrashContr01 LogEvent REG DWORD Overwrite REG DWORD Aut0Reboot REG DWORD Dump File REG EXPAND SZ OXI OXI OXI %systemR00t%\\MEMORY. DMP DisableEmoticon Crash\' umpEnab1ed REG DWORD REG DWORD OXI OX7 SystemRoot r•llnl ump umpmr REG EXPAND SZ minidumpsCount REG DWORD OX32 ](attachments/Technology_ALL_windows_case_001_Windows%20Log%20分析_002.png)

 

查看系统是否有激活：

[\[Hostname\]]\_\_reg_ProductID.TXT\
ProductId        REG_SZ[       00477-OEM-8400101-10502][   \--\> 2008 R2 ]系统

\-\-\-\-\-\-\-\--

ProductId        REG_SZ       00252-20000-00116-AAOEM[    \-\--\> 2012 ]系统

\-\-\-\-\-\-\-\--

ProductId        [     REG_SZ              00377-40000-00451-AAOEM    \-\--]》2016

 

安装过软件的记录：

[\[Hostname\]]\_Installed_Software.TXT[  \| new: system\\xxxx\_]Win32_Product.txt

[\[Hostname\]]\_sym_ProgramFiles_SYS.TXT

 

安装过补丁的记录：

[\[Hostname\]\_Hotfix.txt] \| new: setup\\xxxx_get_hotfix.log

\# 日志显示，在10月24日客户有做过系统更新

PSComputerName      : PWB-GIS-ROAD1

InstalledOn         : 2022/10/24 と 12:00:00

\_\_PATH              : [\\\\PWB-GIS-ROAD1\\root\\cimv2:Win32_QuickFixEngineering.HotFixID=\"KB5018421\",ServicePackInEffect=](file://PWB-GIS-ROAD1/root/cimv2:Win32_QuickFixEngineering.HotFixID=%22KB5018421%22,ServicePackInEffect=)\"\"

 

......

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Unexpected Reboot有两种情况

一种是graceful shutdown，比如说某个程序造成Reboot。

还有一种是not graceful shutdown，比如说BSOD， BIOS/电源问题或硬件问题造成的Reboot。

如果是前者的话，一般可以从Eveng Log里推断出来是哪个程序造成的。后者除了BSOD，其他是没有任何线索的。

 

如果是BSOD的话，是有6008的，因为不是Graceful Shutdown，但也会有Bugcheck的记录。

BSOD是不是程序造成的，要看Memory Dump才知道。

如果有6008而没有Bugcheck的Event，

就说明不是程序Crash造成的，因为程序没有那么大的权限来直接Crash OS。

这个信息在System Event log 里：

像是这种记录：

Error     2019/4/29 17:49:16        Microsoft-Windows-WER-SystemErrorReporting  1001     None    The computer has rebooted from a bugcheck.  The bugcheck was: 0x000000d1 (0x0000000000000008, 0x0000000000000002, 0x0000000000000000, 0xfffff805f1a813b0). A dump was saved in: C:\\windows\\MEMORY.DMP. Report Id: fcf96355-d857-48e2-a5cb-63e0728918a1. 

 

[\[Hostname\]]\_DRIVERS.TXT 驱动版本

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

如果dump里面这项是0x9E，说明转存的dump文件很小，分析不出什么问题。

![[Technology_ALL_windows_case_001_Windows Log 分析_003.png]]

所以建议如下：

[\\\\10.74.202.248\\Share2\\Tools](file://10.74.202.248/Share2/Tools)

Install \'.NET Framework 3.5 Features\' from Roles and Features.

1. MPSReport with following option

\[X\] General

\[X\] Internet and Networking

\[X\] Business Networks

\[X\] Server Components

\[ \] Windows Update Services

\[ \] Exchange Servers

\[ \] SQL and other Data Stores (MDAC))

 

2. C:\\Windows\\MEMORY.DMP

 

![Machine generated alternative text: x scom.onwcom .NET Framework 3.5 E-±7 .NET Framework 2.0 API .71) C\] Hyper-V .NET Framewor 4.7 BitLocker BitLocker BranchCache Containers (4 Data Center Bridging Direct play D HTTP f-t.E-EB5 RPC C\] IIS web C\] Internet IP C\] iSNS Server C\] LPR MultiP0int Connector RAS Connection Manager Administration Kit (t Simple TCP/IP Services ](attachments/Technology_ALL_windows_case_001_Windows%20Log%20分析_004.png)

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Windows ISO 版本：

 

 

补丁下载：

后面跟KB4462926

<http://catalog.update.microsoft.com/v7/site/Search.aspx?q=4462926>

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Windows 系统无进入系统操作步骤：

1. 尝试进入安全模式：

如果F8进不了安全模式，可以在OS启动时关机，连关三次就可以进入安全模式。

进入安全模式后使用安全模式启动看是否可以。

如果不行下一步

2.打开 Recovery Console 然后运行下面命令

bcdedit /set  recoveryenabled no

![Machine generated alternative text: \[icrosoft Windows \[ 10. o. 14393\] : ndows \\ system32\> / set {default\] recoveryenabled no : ndows \\ system.32\>--- ](attachments/Technology_ALL_windows_case_001_Windows%20Log%20分析_005.png)

运行命令后重启，观察有否出现错误信息。有信息抓Screen shot并告知我们

如果没有错误下一步

3. 尝试修复Boot Recorder

Use Bootrec.exe in the Windows RE to troubleshoot startup issues

<https://support.microsoft.com/en-us/help/927392>

 

 

==================

KB，full dump 检查

<https://kb.dell.com/infocenter/index?page=content&id=SLN293335&actp=SEARCH&viewlocale=en_US&searchid=1413437893922>

 

 

[ ]请检查一下dump的设置是否设置正确

 

对于常见的蓝屏，请务必进入控制面板---》系统\-\--》高级---》启动和故障恢复-à将Dump模式设置为complete memory dump来确保死机的时候能够获得完整的memory dump来提供给我们分析死机原因，出现蓝屏之后请务必等待屏幕提示DUMP完成了再重启，

该设置如之下图片所示

![[Technology_ALL_windows_case_001_Windows Log 分析_006.jpg]]

 

对于一般性的系统内部卡死，请系统内部设置打开注册表位置HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Services\\kbdhid\\Parameters，在其下创建一个名字为CrashOnCtrlScroll的REG_DWORD值并将数值设置为1，必须重启才能让该设置生效，如果没有就自建一个。

如果发生系统内部卡死，比如能Ping通但是无法RDP进入，console也无显示或者显示冻结，可以按住键盘右边的CTRL键盘不放，然后按下SCROLL lock键，促使windows 系统crash并且自动产生memory dump， 请务必等待屏幕提示DUMP完成了再重启，不然可能会让DUMP文件不完整从而无法分析原因。

 

 

![[Technology_ALL_windows_case_001_Windows Log 分析_007.jpg]]

 

 ===================

<https://kb.dell.com/infocenter/index?page=content&id=SLN285552#2>

[https://docs.microsoft.com/zh-cn/windows-server/get-started-19/vm-activation-19](https://docs.microsoft.com/zh-cn/windows-server/get-started-19/vm-activation-19) MS 中文

AVMA 是虚拟机自动激活 

Automatic Virtual Machine Activation

The Hyper-V host must be running Windows Server 2016 Datacenter or Windows Server 2012 R2 Datacenter and be activated. AVMA is not supported in non-Datacenter editions.

这个功能在WindowsServer2012R2允许激活虚拟机通过Hyper-V主机。AVMA是可用在OEM和批量许可方案许可。一种AVMA客户端密钥可以被安装在无人值守安装WindowsVM上的OS或后安装。

AVMA在非Datacenter版中不受支持。

 

 

 

 

=========================

- Portable Diagnostics log: WIN-I9PHJVMQUK2_MachineInfo.xml

=======================================================

SerialNumber=\"00377-30001-50851-AAOEM\"         ////// 问题服务器上的操作系统是W2K16 OEM。

=======================================================

 

- Portable Diagnostics log: PC45_MachineInfo.xml

=======================================================

SerialNumber=\"00376-30000-00299-AA581\"            ////// 普通服务器上的操作系统不是W2K16 OEM。

=======================================================

 

 

 

1. 请让客户根据下面的方法下载2016最新补丁后再尝试安装切换语言。

先更新最新的SSU KB4503537然后再更新的LCU补丁

下载此SSU：

[http://catalog.update.microsoft.com/v7/site/Search.aspx?q=4503537 ](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=4503537)

LCU下载地址

[http://catalog.update.microsoft.com/v7/site/Search.aspx?q=4509475](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=4509475)

 

===============================

关于windwos域的描述：

DC 是：Domain Controller

成员是：Member Server

 

========================

Windwos 2016 进入安全模式。 当开机时还没出现windwos logo时，点击多次 F8，就能进入safe modern

![[Technology_ALL_windows_case_001_Windows Log 分析_008.png]]

 

 

 

===================

Dump 设置截图：

![[Technology_ALL_windows_case_001_Windows Log 分析_009.jpg]]

 

生成后dump log 的默认位置是 c:\\windows\\MEMORY.DMP

![[Technology_ALL_windows_case_001_Windows Log 分析_010.png]]

 

 

已使用 OneNote 创建。
