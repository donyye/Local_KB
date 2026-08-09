license激活问题

2017年9月27日

16:18

- ::: 
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: T330\|OS issue\|PROS\|SR 954077678 
    发件人     Yin, Guoxun
    收件人     Ye, Dony; Lin, Yongliang; Zhan, Yanbin
    抄送       Dong, Peter; Samuel, Su; CN XMN TS ENT L2 SME
    发送时间   2017年9月22日 9:35
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Yanbin,

  请客户分别在当前主机以及虚拟机上收集以下信息给我们:

   

  1.  当前安装了几个虚拟机？ 是否全部无法激活？
  2.  物理机系统是否已经激活？
  3.  请在主机上以管理员权限打开命令行，运行以下命令，把输出截图给我们，并注意是来自于主机
      a.  slmgr /dlv
      b.  systeminfo
  4.  请在虚拟机系统上以管理员权限打开命令行，运行以下命令，把输出截图给我们，并注意是来自于虚拟机
      a.  slmgr /dlv
      b.  systeminfo

   

   

  BR.

  Guoxun.

  From: Ye, Dony

  Sent: Friday, September 22, 2017 9:08 AM

  To: Lin, Yongliang \<Yongliang_Lin@Dell.com\>; Zhan, Yanbin \<Yanbin_Zhan@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: Dong, Peter \<Peter_Dong@dell.com\>; Samuel, Su \<Su_Samuel@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: 答复: T330\|OS issue\|PROS\|SR 954077678 

   

  Dell - Internal Use - Confidential 

  Hi, Guoxun

   

  请帮忙，谢谢！

   

  B R

  Dony

   

  发件人: Lin, Yongliang 

  发送时间: 2017年9月22日 8:39

  收件人: Zhan, Yanbin \<[Yanbin_Zhan@Dell.com](mailto:Yanbin_Zhan@Dell.com)\>; Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>

  抄送: Dong, Peter \<[Peter_Dong@dell.com](mailto:Peter_Dong@dell.com)\>; Samuel, Su \<[Su_Samuel@Dell.com](mailto:Su_Samuel@Dell.com)\>; CN XMN TS ENT L2 SME \<[CN_XMN_TS_ENT_L2_SME@Dell.com](mailto:CN_XMN_TS_ENT_L2_SME@Dell.com)\>

  主题: RE: T330\|OS issue\|PROS\|SR 954077678 

   

  Dell - Internal Use - Confidential 

  Dony:

   

  Help it

   

   

  Yongliang, Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

   

  From: Zhan, Yanbin

  Sent: Thursday, September 21, 2017 5:06 PM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>

  Subject: T330\|OS issue\|PROS\|SR 954077678

   

  Dell - Internal Use - Confidential 

   

                 Hi  L2

   

   

  a.     Detail Symptom Descriptions

  详细的故障现象描述:（服务器T330 OEM  2012 R2  (2CPU 2VMS )客户架设 虚机安装的2012 R2,但是使用机器 product key 无法激活，客户联系微软说通过[https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/01/07/automatic-virtual-machine-activation-in-windows-server-2012-r2/](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/01/07/automatic-virtual-machine-activation-in-windows-server-2012-r2/)这个共用KEY 可以激活但是故障依旧 。后续通过联系客户才了解到服务器已经出厂 1年3个月，客户一直以为自己买了才3个月。客户现在要求我们升级L2 ，确认是否有其他解决方案，他们现场还有10-20 台 T330 的 服务器，客户其他服务器还未测试。 

  故障的时间点 :

  是否可以复现故障 :客户为了测试product key 将本机系统 做修改 KEY的操作后。由于product key 失效，现在KEY哪里显为未空，系统未激活。）

  如何复现故障 有虚机导入照片，和实体机导入照片，还有本机上product key信息

   

  b.    Troubleshooting Steps

  建议客户使用PRODUCT KEY 激活虚拟机无法激活

   

  c.     Current status

  横河机电

  d.     Must Collect Logs

  所有设置照片已经发送到DELTA中

   

   

 

已使用 OneNote 创建。
