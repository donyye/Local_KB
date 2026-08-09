RE: R730XD\|System unstable issue\|PSP\|Vmware\|953416823

2017年9月8日

15:42

- ::: 
    -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: R730XD\|System unstable issue\|PSP\|Vmware\|953416823
    发件人     Han, Ruyang
    收件人     Tong, Tom; Lin, Yongliang
    抄送       Wu, Sukie; CN XMN TS ENT L2 SME; Wang, Xing Fang; Luo, Gary
    发送时间   2017年9月8日 15:38
    -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  Hi Tom

   

  1.  首先，非常抱歉，升级log没有写清楚。关于这个case，这边上次已经跟您请教过，且已经将已知的issue第一时间给到客户升级阵列卡及驱动的建议（报修时机器可正常使用）；当时客户机器阵列卡固件为：25.5.0.0018，阵列卡驱动为：lsi_mr3，建议客户升级为：25.4.1.0004、scsi-megaraid-perc9 version 6.903.55.00-1OEM.550.0.0.1331820

   

  上次你也知道让客户升固件驱动，但是事只做了一半，升级L2的时候系统用的还是lsi_mr3,已经给你解释过了。这次还是一样，这个系统还是在用lsi_mr3:

  ![[Technology_ALL_VMware_分析案例_068_RE_ R730XD_System unstable issue_PSP_Vmwa_001.png]]

   

   

  1.  当前客户的需求是：由于机器在给到升级建议后又宕机了3次，客户怀疑机器硬件有问题，还表示目前服务器处于关机状态，服务器搭载有生产应用系统，严重影响公司的正常生产，已多次IB进来，要求尽快派单确认问题，由于这样的原因，升级L2想找您帮忙看下vm-support日志确认是否有硬件倾向，如果确认可能有硬件层面的问题这边将申请派单能够加快客户报修的进度

   

  问题1的事情都没做完，自然问题依旧。这个紫屏明显就是硬件层面问题，不用考虑软件。

  ![[Technology_ALL_VMware_分析案例_068_RE_ R730XD_System unstable issue_PSP_Vmwa_002.png]]

   

   

  1.  另外，由于VMware官网给出的该H730Mini的阵列卡的兼容驱动及固件与当前系统不同，最低只有5.5 U1，客户当前机器系统为：VMware ESXi 5.5.0 build-1331820，想找您确认给到客户的这个固件及驱动版本是否可以真正解决客户机器宕机的问题；

   

  一切兼容性查询以VMware HCL为准。下面是5.5的版本，低版本的OS支持低版本的阵列卡驱动固件，相应高的OS版本支持更高的驱动固件，没什么好解释的。

  <https://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=34859&deviceCategory=io&details=1&partner=23&releases=243&keyword=H730&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc>

  ![[Technology_ALL_VMware_分析案例_068_RE_ R730XD_System unstable issue_PSP_Vmwa_003.png]]

   

   

   

   

   

   

  Best Regards

  Ruyang Han

   

  From: Tong, Tom

  Sent: Friday, September 8, 2017 3:11 PM

  To: Han, Ruyang \<Ruyang_Han@Dell.com\>; Lin, Yongliang \<Yongliang_Lin@Dell.com\>

  Cc: Wu, Sukie \<Sukie_Wu@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>; Wang, Xing Fang \<Xing_Fang_Wang@DELL.com\>; Luo, Gary \<Gary_Luo@DELL.com\>

  Subject: 答复: R730XD\|System unstable issue\|PSP\|Vmware\|953416823

   

  Dell - Internal Use - Confidential 

   Dear ruyang

   

  1.  首先，非常抱歉，升级log没有写清楚。关于这个case，这边上次已经跟您请教过，且已经将已知的issue第一时间给到客户升级阵列卡及驱动的建议（报修时机器可正常使用）；当时客户机器阵列卡固件为：25.5.0.0018，阵列卡驱动为：lsi_mr3，建议客户升级为：25.4.1.0004、scsi-megaraid-perc9 version 6.903.55.00-1OEM.550.0.0.1331820

   

  1.  当前客户的需求是：由于机器在给到升级建议后又宕机了3次，客户怀疑机器硬件有问题，还表示目前服务器处于关机状态，服务器搭载有生产应用系统，严重影响公司的正常生产，已多次IB进来，要求尽快派单确认问题，由于这样的原因，升级L2想找您帮忙看下vm-support日志确认是否有硬件倾向，如果确认可能有硬件层面的问题这边将申请派单能够加快客户报修的进度

   

  1.  另外，由于VMware官网给出的该H730Mini的阵列卡的兼容驱动及固件与当前系统不同，最低只有5.5 U1，客户当前机器系统为：VMware ESXi 5.5.0 build-1331820，想找您确认给到客户的这个固件及驱动版本是否可以真正解决客户机器宕机的问题；

   

  出于以上几个原因，故升级L2协助确认下，以保证给出的建议有针对性并尽快解决客户问题，还请您帮忙确认下，谢谢！

   

  FTP链接：

  A login has been created for you, enabling access to the secure web based file transfer application - File Exchanger.

  The File Exchanger application can be accessed via a web browser using the following details:

  Homepage:         <https://fileexchanger.dell.com>

  Username:

  6M4MZG2 

  Password:

  enIWmIPB11

   

  VMware官网兼容性指南：

  <https://www.vmware.com/resources/compatibility/detail.php?deviceCategory=vsanio&productid=34859&vcl=true>

   

  ![[Technology_ALL_VMware_分析案例_068_RE_ R730XD_System unstable issue_PSP_Vmwa_004.png]]

   

  Best Regards.

  Tong Tom

   

  发件人: Han, Ruyang 

  发送时间: 2017年9月8日 13:40

  收件人: Lin, Yongliang \<[Yongliang_Lin@Dell.com](mailto:Yongliang_Lin@Dell.com)\>; Tong, Tom \<[Tom_Tong@Dell.com](mailto:Tom_Tong@Dell.com)\>

  抄送: Wu, Sukie \<[Sukie_Wu@Dell.com](mailto:Sukie_Wu@Dell.com)\>; CN XMN TS ENT L2 SME \<[CN_XMN_TS_ENT_L2_SME@Dell.com](mailto:CN_XMN_TS_ENT_L2_SME@Dell.com)\>; Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\>; Luo, Gary \<[Gary_Luo@DELL.com](mailto:Gary_Luo@DELL.com)\>

  主题: RE: R730XD\|System unstable issue\|PSP\|Vmware\|

   

  Dell - Internal Use - Confidential 

  Tom

   

  附件是你上上个月刚升级过的一模一样的case，而且这是反复在说的hot issue + know issue, 当时还电话教你半天我到现在都还记得，你这么快就忘了?

   

   

   

  Best Regards

  Ruyang Han

   

  From: Lin, Yongliang

  Sent: Friday, September 8, 2017 1:17 PM

  To: Tong, Tom \<<Tom_Tong@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>

  Cc: Wu, Sukie \<<Sukie_Wu@Dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: R730XD\|System unstable issue\|PSP\|Vmware\|

   

  Dell - Internal Use - Confidential 

  Hi ruyang:

   

  Help it .

   

   

  Yongliang, Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

   

  From: Tong, Tom

  Sent: Friday, September 8, 2017 11:44 AM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Deltamail_prod \<<Deltamail_prod@Dell.com>\>; Tong, Tom \<<Tom_Tong@Dell.com>\>; Wu, Sukie \<<Sukie_Wu@Dell.com>\>

  Subject: R730XD\|System unstable issue\|PSP\|Vmware\|

   

   a.  Detail Symptom Descriptions

  机器出现多次宕机，紫屏界面已回传，系统为：VMware ESXi 5.5.0 build-1331820，当前日志记录报错提示为：

  2017-09-08 02:59:01  A fatal error was detected on a component at bus 0 device 1 function 0.

  2017-09-08 02:59:00  A fatal error was detected on a component at bus 3 device 0 function 0.

  b\.   Troubleshooting Steps

  1. 已建议客户升级相关阵列卡固件及查看驱动版本

  2. 出厂未带系统，建议客户收集VM-support查看硬件层面故障

  3. 升级L2协助确认问题

  c\.     Current status

  客户公司名称: 深圳市富迅通贸易有限公司

  业务影响: 宕机多次

  升级的原因: 复杂问题

  d\.     Must Collect Logs

  已经收集TSR 上传至SR

 

已使用 OneNote 创建。
