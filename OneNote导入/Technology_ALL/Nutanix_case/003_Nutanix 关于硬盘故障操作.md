Nutanix 关于硬盘故障操作

Monday, September 26, 2016

4:57 PM

- ::: 
    -------------------------------------- ----------------------------------------------------------------------------------------
    主题       RE: XC630 \|Product query\|PROS\|SR 936802084 \|ST 1BY0DF2
    发件人     Yin, Guoxun
    收件人     Lin, Yongliang; Zheng, Merry
    抄送       CN XMN TS ENT L2 SME
    发送时间   Monday, September 26, 2016 4:52 PM
    -------------------------------------- ----------------------------------------------------------------------------------------
  :::

  具体操作，在下面：

   

  Hi Merry,

  请告知客户， Nutanix节点中的硬盘都是由CVM来管理控制的，硬盘故障后会有CVM自动在节点中踢出故障硬盘，这时候故障硬盘才可以被拔出。在正常运行过程中，绝对不允许人为的热插拔，这种测试方式不合理也没有必要。 现在如果客户已经认为插拔过硬盘了得话，那么该硬盘已经处于异常离开群集的状态。那么请他等待下，我们会提供重新导入硬盘的指导给他。

  至于第二个问题，Cluster中的节点关闭后， hypervisor 和CVM启动后，节点会自动加回Cluster，不需要执行添加动作。但是也请提醒客户 ，根据冗余度设置，如果为1的情况下，同一时间之允许一个节点被关闭，多余一个节点会导致Cluster无法工作。

   

  BR.

  Guoxun

  From: Lin, Yongliang

  Sent: 2016年9月26日 16:41

  To: Zheng, Merry \<Merry_Zheng@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: 答复: XC630 \|Product query\|PROS\|SR 936802084 \|ST 1BY0DF2

   

  Dell - Internal Use - Confidential 

  Hi guoxun:

   

  Help follow up .

   

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

  发件人: Zheng, Merry 

  发送时间: 2016年9月26日 16:35

  收件人: CN XMN TS Server Escalation \<[CNXMNTSServerEscalation@DELL.com](mailto:CNXMNTSServerEscalation@DELL.com)\>

  抄送: Zheng, Merry \<[Merry_Zheng@Dell.com](mailto:Merry_Zheng@Dell.com)\>

  主题: XC630 \|Product query\|PROS\|SR 936802084 \|ST 1BY0DF2

   

  Dell - Internal Use - Confidential 

  a.     Detail Symptom Descriptions

  详细的故障现象描述:客户来电咨询Nutanix软件的使用以及3个节点cluster在这个软件下的设置

  故障的时间点 :

  是否可以复现故障 :

  如何复现故障 :

   

  b.    Troubleshooting Steps

  详细的诊断步骤:

  1.  客户表示之前16\*1T硬盘，在nutanix软件上对一块硬盘点了移除，咨询什么添加回去
  2.  客户表示明天要通过这个软件演示给领导查看，热拔插硬盘后，在这个软件上什么恢复回去
  3.  cluster中的一个节点关闭后，通过此nutanix软件，什么再添加回去
  4.  客户很着急，希望尽快给他答复，因为明天就要演示给领导，谢谢！

  维修记录: (单号/更换的部件/更换后的现象)

  Bios/Driver/FW及存储控制器相关FW版本:

   

  c.     Current status

  客户公司名称:

   

  /业务影响:/升级的原因:/RM/TAM:

   

  d.     Must Collect Logs

  已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

  常见日志类型参考(根据实际情况获取相应日志)：

  服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

  存储(参考)：MD/EQL/NAS/CML/DR/DL log;

   

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--Disk removing and reseat procedure

  Before removing any disks for testing in future in future, you'll need to mark it for removal in the Prism Web UI under Hardware à Diagram using the Remove Disk option

  Then wait for any data/metadata present in the failed disk to be evacuated before physically removing the failed disk

  The disks must not be simply removed/pulled out without following the correct procedure

  Otherwise, potential issues could arise which could possibly lead to data corruption and data inconsistency

   

   

  1.  From the web console, click Hardware à Diagram, and hover the mouse over the drive that was replaced/reseated to view the details.

  The drive indicator should not be red. Failure messages should not be displayed when the drive is selected.

  ![[Technology_ALL_Nutanix_case_003_Nutanix 关于硬盘故障操作_001.jpg]]

  1.  If the drive label shows Unmounted Drive, click Repartition and Add.

  The following CAUTION message and the button are displayed only when the replacement drive contains data. The purpose is to prevent you from unintentionally using a drive that contains data.

  CAUTION: This action removes all data on the drive. If you are not certain that the correct replacement drive was inserted, do not repartition until you have confirmed that it contains no data that is needed.

  ![[Technology_ALL_Nutanix_case_003_Nutanix 关于硬盘故障操作_002.jpg]]

  1.  On the Web console, click Summary \> Disk Details to verify that the disk was added to the original storage pool.

  ![[Technology_ALL_Nutanix_case_003_Nutanix 关于硬盘故障操作_003.jpg]]

  If the cluster has only one storage pool, the disk is automatically added to that storage pool.

  1.  (Optional) If the drive was not added to the storage pool because more than one storage pool is in the cluster, then add the drive to the desired storage pool
      a.  In the web console, select Storage from the drop-down main menu (upper left of screen), click the Table tab, and then click the Storage Pool tab. 
      b.  Select the target storage pool and then click Update.

  ![[Technology_ALL_Nutanix_case_003_Nutanix 关于硬盘故障操作_004.jpg]]

  The Update Storage Pool window opens.

  1.  In the Capacity group, select the Use unallocated capacity check box to add the available unallocated capacity to this storage pool, and then click Save
  2.  Click Hardware à Diagram, select the drive, and confirm that it is in the correct storage pool

   

  Note: If it's an SSD, you'll also need to click on Enable Metadata Store after a new SSD is inserted or an existing SSD is reseated

   

 

已使用 OneNote 创建。
