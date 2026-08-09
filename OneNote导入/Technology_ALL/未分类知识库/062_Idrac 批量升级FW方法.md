Idrac 批量升级FW方法

2018年11月9日

11:05

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   FW: DRM
  From      Shen, Mouse
  To        Ye, Dony
  Sent      2018年11月9日 11:00
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Internal Use - Confidential

 

 

 

首先在DRM里面, 把Dell 在线的数据库同步到本地

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_001.jpg]]

 

同步好的数据库. 同步数据库只需要下载catalog, 所以很快.

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_002.jpg]]

 

然后利用同步的数据库,  新建一个iDRAC inventory

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_003.jpg]]

 

因为用于iDRAC更新, 我只选择了Win64以及OS无关的更新包

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_004.jpg]]

 

 

导出这个Repo

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_005.jpg]]

 

 

导出的文件, 并且把这个文件夹设置为共享

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_006.jpg]]

 

 

使用racadm工具执行如下操作:

 

检查需要更新的项目:

 

C:\\\>racadm -r 10.100.182.253 -u root -p calvin update -f catalog.xml -l //10.114

.21.162/fcx630/ -u asia-pacific/mouse_shen -p XXX -t CIFS -a FALSE \--verifycatalog

 

Security Alert: Certificate is invalid - Certificate is not signed by Trusted Th

ird Party

Continuing execution. Use -S option for racadm to stop execution on certificate-

related errors.

RAC1118 : Update from repository operation has been initiated. Check the progres

s of the operation using \"racadm jobqueue view -i JID_573296844397\" command.

 

确认检查更新的项目完成

 

C:\\\>racadm -r 10.100.182.253 -u root -p calvin jobqueue view -i JID_573296844397

 

Security Alert: Certificate is invalid - Certificate is not signed by Trusted Th

ird Party

Continuing execution. Use -S option for racadm to stop execution on certificate-

related errors.

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- JOB \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\[Job ID=JID_573296844397\]

Job Name=Repository Update

Status=Completed

Start Time=\[Not Applicable\]

Expiration Time=\[Not Applicable\]

Message=\[RED001: Job completed successfully.\]

Percent Complete=\[NA\]

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

查看检查报告

 

C:\\\>racadm -r 10.100.182.253 -u root -p calvin update viewreport

 

Security Alert: Certificate is invalid - Certificate is not signed by Trusted Th

ird Party

Continuing execution. Use -S option for racadm to stop execution on certificate-

related errors.

ComponentType     = Firmware

ElementName       = Broadcom Gigabit Ethernet BCM5720 - F8:DB:88:5F:BD:DD

FQDD              = NIC.Integrated.1-3-1

Current Version   = 7.10.61

Available Version = 7.10.64

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ComponentType     = Application

ElementName       = Dell 32 Bit uEFI Diagnostics, version 4239, 4239A24, 4239.32

 

FQDD              = Diagnostics.Embedded.1:LC.Embedded.1

Current Version   = 4239A24

Available Version = 4239A27

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ComponentType     = Application

ElementName       = Dell OS Driver Pack, 15.07.07, A00

FQDD              = DriverPack.Embedded.1:LC.Embedded.1

Current Version   = 15.07.07

Available Version = 15.10.02

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ComponentType     = BIOS

ElementName       = BIOS

FQDD              = BIOS.Setup.1-1

Current Version   = 1.2.4

Available Version = 1.4.5

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

开始正式更新

 

C:\\\>racadm -r 10.100.182.253 -u root -p calvin update -f catalog.xml -l //10.114

.21.162/fcx630/ -u asia-pacific/mouse_shen -p XXX -t CIFS -a TRUE

 

Security Alert: Certificate is invalid - Certificate is not signed by Trusted Th

ird Party

Continuing execution. Use -S option for racadm to stop execution on certificate-

related errors.

RAC1118 : Update from repository operation has been initiated. Check the progres

s of the operation using \"racadm jobqueue view -i JID_573298256884\" command.

 

系统的更新任务. 每项更新任务, 都会创建一个新的download job. 需要重启的固件会停留在Scheduled状态等待重启, 最后, 会生成一个reboot的job 让系统重启, 以便更新完成.

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_007.jpg]]

 

 

如图, 重启后, BIOS更新中

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_008.jpg]]

 

随后是网卡

![[Technology_ALL_未分类知识库_062_Idrac 批量升级FW方法_009.jpg]]

 

 

 

已使用 OneNote 创建。
