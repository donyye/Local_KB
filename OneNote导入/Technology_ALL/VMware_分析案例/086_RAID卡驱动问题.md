RAID卡驱动问题

2018年4月3日

14:30

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       答复: R730XD\|RAID issue\|PSP\|SR:961735191[   ]华润银行(紧急)
    发件人     Han, Ruyang
    收件人     Wang, Xing Fang; Shen, Mouse; Yin, Guoxun; Ye, Dony
    抄送       Yang, Frank
    发送时间   2018年4月3日 11:02
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  Hi Xingfang

   

  IPS在3月29号给我update时说计划在4/17认证最新版本的7.703.18，下午开会可以再确认下有没有变化。

   

  Current status: 7.703.15.00 is available in VMWare HCL page. 7.703.18 version is available for download, but still under the process of VMWare HCL certification. According to current VMWare HCL certification schedule, target around 4/17. Compare to 7.703.15, this new 7.703.18 version has the same fix for this VSAN timeout issue. So, depends on customer\'s situation and request. Customer can either use this 7.703.15 version, or start download 7.703.18, or wait for 7.703.18 in HCL list eventually. According to last week update, customer had tried several systems with 703.15 driver, and no time out issue. But, VMWare kernal log shows \"0x71\" events. VMWare claims it requires Dell analysis. Based on our experience, there is no direct evidence so far pointing to H/W errors. Requires customer provide system logs after 703.15 driver installed. If these are some true issues, we will work on this 703.15 driver for sure. However, so far, the log we saw was still based on old driver version. We haven\'t got any logs based on 703.15 driver yet.

  Next Actions: Collect system logs based on 703.15 driver and check if any new issues. Continue monitor customer\'s situation. Update if customer has any new request or plan, such as when customer want to upgrade drivers.

  Roadblocks:

  Next Update Date: 7 days later

   

   

  Best Regards

  Ruyang Han

   

  发件人: Wang, Xing Fang 

  发送时间: Tuesday, April 3, 2018 10:58 AM

  收件人: Shen, Mouse \<Mouse_Shen@DELL.com\>; Han, Ruyang \<Ruyang_Han@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>; Ye, Dony \<dony_ye@Dell.com\>

  抄送: Yang, Frank \<Frank_Yang@Dell.com\>

  主题: RE: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Internal Use - Confidential

   

  我们知道吗？？

   

  XingFang Wang

  Manager Customer Support Services

  Great China

  Dell Services

  Office +86-592-818-5846

  Mobile +86-180-3023-3742

  Email [Xing_Fang_Wang@Dell.com](mailto:Your%20name@Dell.com)

   

  From: Yang, Frank

  Sent: Tuesday, April 3, 2018 10:56 AM

  To: Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>

  Subject: 答复: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Dear 王总，

   

  华润银行VSAN环境的Raid卡固件需要升级。由于现有最新版本其实也有bug，所以客户一直没做升级，等待更新的版本。

  All flash的出来至少1、2个月了，但Hybrid的到现在还没消息。咱们有什么途径去问问VMware那边，新版本什么时候release吗？

   

  多谢。

   

  Regards,

   

  Frank Yang 杨旭峰

  Technology Service Manager

  Office: +86 0755 2532 1146

  Mobile +86 177 0601 8631

  How am I doing? Email my manager: <Victor.Yeung@Dell.com>

   

  发件人: Su, Mison 

  发送时间: 2018年4月3日 10:24

  收件人: Yang, Frank \<[Frank_Yang@Dell.com](mailto:Frank_Yang@Dell.com)\>

  抄送: Samuel, Su \<[Su_Samuel@Dell.com](mailto:Su_Samuel@Dell.com)\>; Han, Ruyang \<[Ruyang_Han@Dell.com](mailto:Ruyang_Han@Dell.com)\>; An, Anderson \<[Anderson_An@Dell.com](mailto:Anderson_An@Dell.com)\>

  主题: 回覆: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  <https://www.vmware.com/resources/compatibility/detail.php?deviceCategory=vsanio&productid=34852&vcl=true>

   

  从vmware官网来看,还没发布Hybrid混合版本.

   

  ![[Technology_ALL_VMware_分析案例_086_RAID卡驱动问题_001.png]]

   

   

   

   

  寄件者: Yang, Frank 

  寄件日期: 2018年4月3日 10:00

  收件者: Su, Mison \<[mison_su@DELL.com](mailto:mison_su@DELL.com)\>

  副本: Samuel, Su \<[Su_Samuel@Dell.com](mailto:Su_Samuel@Dell.com)\>; Han, Ruyang \<[Ruyang_Han@Dell.com](mailto:Ruyang_Han@Dell.com)\>

  主旨: 答复: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Hi Mison

   

  能否帮忙查询下，混合配置的VSAN Raid卡固件目前通过VMware认证了吗。客户又一台重启了，希望尽快升级。

   

  谢谢。

   

  Regards,

   

  Frank Yang 杨旭峰

  Technology Service Manager

  Office: +86 0755 2532 1146

  Mobile +86 177 0601 8631

  How am I doing? Email my manager: <Victor.Yeung@Dell.com>

   

  发件人: Samuel, Su 

  发送时间: 2018年3月27日 10:24

  收件人: Yang, Frank \<[Frank_Yang@Dell.com](mailto:Frank_Yang@Dell.com)\>; Su, Mison \<[mison_su@DELL.com](mailto:mison_su@DELL.com)\>

  抄送: Han, Ruyang \<[Ruyang_Han@Dell.com](mailto:Ruyang_Han@Dell.com)\>

  主题: FW: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Hi

  Frank 

   

  我这里联系不上客户，手机关机了。

  不知道  客户那边进行的如何了？

   

  SU_SAMUEL      samuel

  How am I doing?Email my manager:[Richa_zeng@Dell.com](mailto:Richa_zeng@Dell.com)

   

  From: Su, Mison

  Sent: 2018年3月26日 17:32

  To: Samuel, Su \<<Su_Samuel@Dell.com>\>

  Subject: 轉寄: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

   

  寄件者: Han, Ruyang 

  寄件日期: 2018年3月8日 11:31

  收件者: Yang, Frank \<[Frank_Yang@Dell.com](mailto:Frank_Yang@Dell.com)\>; Su, Mison \<[mison_su@DELL.com](mailto:mison_su@DELL.com)\>

  副本: Chen, David \<[David_Chen@DELL.com](mailto:David_Chen@DELL.com)\>

  主旨: RE: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Dell - Internal Use - Confidential 

  VMware建议的驱动只通过了全闪存认证，混合配置的版本要老一点。

   

  VMware都是先认证全闪存，然后认证混合版本，会有时间差。

   

  最新的版本也有重要更新，终究也是要升级的，请客户评估一下要怎么选：

   

  1.  直接升级最新的all flash版本。
  2.  先升级已经通过hybrid认证的老版本，等最新版本通过认证后再次升级。
  3.  等最新版本通过hybrid认证后再做升级。

   

  ![[Technology_ALL_VMware_分析案例_086_RAID卡驱动问题_002.png]]

   

   

  Best Regards

  Ruyang Han

   

  From: Yang, Frank

  Sent: Thursday, March 8, 2018 9:58 AM

  To: Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Su, Mison \<<mison_su@DELL.com>\>

  Cc: Chen, David \<<David_Chen@DELL.com>\>

  Subject: 答复: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Dear Ruyang，

   

  最后确认下：

  阵列卡固件升级到：25.5.4.0006

  阵列卡驱动升级到：7.703.15.00-1OEM

   

  升级顺序：

  可在线升级，先升级一台的固件及驱动，然后再逐一依次升级，直至全部机器的固件、驱动升级完毕。

   

  以上如果没问题，我上午就告知客户去申请操作了。

   

  谢谢。

   

  Regards,

   

  Frank Yang 杨旭峰

  Technology Service Manager

  Office: +86 0755 2532 1146

  Mobile +86 177 0601 8631

  How am I doing? Email my manager: <Victor.Yeung@Dell.com>

   

  发件人: Han, Ruyang 

  发送时间: 2018年3月8日 9:40

  收件人: Su, Mison \<[mison_su@DELL.com](mailto:mison_su@DELL.com)\>

  抄送: Yang, Frank \<[Frank_Yang@Dell.com](mailto:Frank_Yang@Dell.com)\>; Chen, David \<[David_Chen@DELL.com](mailto:David_Chen@DELL.com)\>

  主题: RE: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Dell - Internal Use - Confidential 

  可以一台一台的交替升级，先升级一台确认正常后升级另一台，最终把一个VSAN中的所有节点升级上去。

   

   

  Best Regards

  Ruyang Han

   

  From: Su, Mison

  Sent: Thursday, March 8, 2018 9:32 AM

  To: Han, Ruyang \<<Ruyang_Han@Dell.com>\>

  Cc: Yang, Frank \<<Frank_Yang@Dell.com>\>; Chen, David \<<David_Chen@DELL.com>\>

  Subject: 回覆: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  \@Ruyang;

   

  是升级这个25.5.4.0006固件吗?Frank那边需要进一步确认:

  1.  VSAN的环境直接先升级一台测试会不会对整个VSAN有影响
  2.  客户可能没办法所有的一起升级,一次安排几台停机升级会不会对业务造成影响.

   

  - 紧急程度: Urgent
  - 发布日期: 2018-01-10
  - 版本号: 25.5.4.0006
  - 描述: Dell PERC H730/H730P/H830/FD33xS/FD33xD Mini/Adapter RAID Controllers firmware version 25.5.4.0006
  - 主要更新: 

  - 将6Gbps SATA驱动器的驱动器缓存默认值更改为禁用。这是为了与SATA驱动器行业一致。这可能会导致性能下降，尤其是在非Raid模式下。您必须执行冷重启以查看现有配置的变化。

  - 修复了特定固件FMU故障可能导致控制器挂起大约15秒的问题。搜索控制器记录字符串"fusionMUErrorIsr: FMU Error Status 00040000"。

  - 修复了SATA驱动器可能在VSAN环境中随机返回04/44/00检查条件的问题。在控制器日志中搜索字符串"Sense：4/44/00"。例如："07/24/17 11:00:55: C0:EVT#8446014-07/24/17 11:00:55: 113=Unexpected sense: PD 02(e0x20/s2) Path 500056b3fafedac2, CDB: 9e 10 00 00 00 00 00 00 00 00 00 00 00 20 00 00, Sense: 4/44/00"

  - 修复了一个问题，您可以看到可能导致性能问题甚至服务器崩溃的错误的多个SRAM可纠正错误。搜索字符串"SRAM errAddr"或"Correctable err，continue \..."的tty日志。例如："C0：SRAM errAddr c0023010 errAttrib 00000003"后跟"C0:Correctable err, continuing\..."

  - 修正外部存储有时会丢失时间同步的问题。

  - 修复了在引导过程中配置中有多个PERC控制器时会出现的随机RSOD的问题。

  - 修正电池在透明学习周期中可能无意中变成WT的问题。

  - 修复iDRAC LC日志中未显示VD删除和VD创建事件的问题。

  - 修正了当外部VD具有不同的安全密码短语时，有时候控制器无法解锁并导入多个安全的外部配置的问题。

   

   

   

  寄件者: Han, Ruyang 

  寄件日期: Thursday, March 8, 2018 9:03 AM

  收件者: Su, Mison \<[mison_su@DELL.com](mailto:mison_su@DELL.com)\>

  副本: Yang, Frank \<[Frank_Yang@Dell.com](mailto:Frank_Yang@Dell.com)\>; Chen, David \<[David_Chen@DELL.com](mailto:David_Chen@DELL.com)\>

  主旨: RE: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Dell - Internal Use - Confidential 

  Hi Team

   

  阵列卡reset是hot issue，升级阵列卡驱动固件是解决方案并且明确写在了release中，已经有很多用户遇到此问题并确认升级后有效果。

   

  请客户follow VMware的建议更新驱动固件，这也是我的建议。

   

   

  Best Regards

  Ruyang Han

   

  From: Lin, Yongliang

  Sent: Thursday, March 8, 2018 8:51 AM

  To: Su, Mison \<<mison_su@DELL.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; Yang, Frank \<<Frank_Yang@Dell.com>\>; Chen, David \<<David_Chen@DELL.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  Dell - Internal Use - Confidential 

  Hi Ruyang:

   

  Help it

   

   

  Yongliang, Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

   

  From: Su, Mison

  Sent: Thursday, March 8, 2018 8:48 AM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; Su, Mison \<<mison_su@DELL.com>\>; Yang, Frank \<<Frank_Yang@Dell.com>\>; Chen, David \<<David_Chen@DELL.com>\>

  Subject: R730XD\|RAID issue\|PSP\|SR:961735191 华润银行(紧急)

   

  邮件正文:

  a.     Detail Symptom Descriptions

  TSM Yang, Frank 来电:前几天我们用R730XD构建的vsan出现读写问题导致上面的数据库down了。经VMware诊断，是阵列卡reset导致的, VMware建议升级阵列卡驱动,详情请见附件

  当前客户做的是VSAN的环境需要评估升级驱动或固件是否有影响.

   

  b.    Troubleshooting Steps

  详细诊断步骤无

  维修记录: (单号/更换的部件/更换后的现象)无

  BIOS/Drive/FW版本:无

   

  c.     Current status

  客户公司名称:/业务影响:/升级的原因:/RM/TAM:

  升级的原因：Pending在哪里？遇到了什么困难？需要（SERVER/OS/Network/Storage）SME L2帮忙做什么？

  固件与驱动需要做评估,看升级驱动及固件是否有影响?硬件健康情况需要检查下。

  第一阶段先判断和处理本套VSAN。麻烦您帮忙抓取下本套VSAN机器的硬件日志，

  第二阶段再判断和升级其他几套VSAN。

   

  RM/TAM: Yang, Frank 

  d.     Must Collect Logs

  已经收集的日志：(请上传至SR下，若无法收集，请注明无法收集的原因):已经收集20台的TSR至附件

  常见日志类型参考(根据实际情况获取相应日志)：

  服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

  存储(参考)：MD/EQL/NAS/CML/DR/DL log;

   

   

   

   

   

   

   

   

   

   

   

   

  Mison_Su

  Enterprise Engineer, Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  [mison_su@dell.com](mailto:mison_su@dell.com)

   

 

已使用 OneNote 创建。
