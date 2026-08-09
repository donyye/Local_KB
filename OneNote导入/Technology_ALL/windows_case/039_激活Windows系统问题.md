激活Windows系统问题

2017年11月15日

18:13

- ::: 
    -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: R730\|os issue\|PROS\|SR 956697663 
    发件人     Yin, Guoxun
    收件人     L, Star; Lin, Yongliang
    抄送       Tian, LianGui; CN XMN TS ENT L2 SME
    发送时间   2017年11月15日 18:11
    -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Star,

  我们的OEM系统不需要激活工具的, 直接输入COA KEY就可以在线激活.

  Standard 版本的windows2012可以直接凭COA激活2个同样版本同样安装源的VM,   超过2个就需要单独购买license激活.

  目前我们建议客户直接重装, 因为在这种情况下修复是极为困难的,浪费时间也难以成功.

   

   

  BR.

  Guoxun.

  From: L, Star

  Sent: Wednesday, November 15, 2017 5:21 PM

  To: Yin, Guoxun \<guoxun_Yin@Dell.com\>; Lin, Yongliang \<Yongliang_Lin@Dell.com\>

  Cc: Tian, LianGui \<LianGui_Tian@dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: RE: R730\|os issue\|PROS\|SR 956697663

   

  Dell - Internal Use - Confidential 

  Dear guoxun

   

  1 选择安全模式立即报错，已经设置期望值重装os

   

  2 客户机器安装的是2012，客户在2012上安装虚机，本来想用激活工具激活虚机的，但客户误操作把激活工具用在2012上操作，之后就这样了。具体什么激活工具还没提供

   

  客户在网上看到如下，还抱有幻想，所以想让我们看看

   

  [https://zhidao.baidu.com/question/1238299988255057579.html](https://zhidao.baidu.com/question/1238299988255057579.html)

   

   

   

  From: Yin, Guoxun

  Sent: 2017年11月15日 16:59

  To: Lin, Yongliang \<<Yongliang_Lin@Dell.com>\>; L, Star \<<Star_L@DELL.com>\>

  Cc: Tian, LianGui \<<LianGui_Tian@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: R730\|os issue\|PROS\|SR 956697663

   

  Hi Star,

  请跟客户确认下具体情况:

  1.  安全模式是否完全不能进入? 选择后也马上bsod?
  2.  无法启动到桌面或者进入桌面马上bsod?

   

  如果以上答案都是是的话, 我们建议用户重装,  在以上情况下, 重装是解决问题最快最有效的办法. 

  另外请用户提供下操作失误是指什么?

   

   

  BR.

  Guoxun.

  From: Lin, Yongliang

  Sent: Wednesday, November 15, 2017 4:55 PM

  To: L, Star \<<Star_L@DELL.com>\>; Yin, Guoxun \<<guoxun_Yin@Dell.com>\>

  Cc: Tian, LianGui \<<LianGui_Tian@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: R730\|os issue\|PROS\|SR 956697663

   

  Dell - Internal Use - Confidential 

  Hi guoxun:

   

  Help

   

   

  Yongliang, Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

   

  From: L, Star

  Sent: Wednesday, November 15, 2017 4:32 PM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Tian, LianGui \<<LianGui_Tian@dell.com>\>

  Subject: R730\|os issue\|PROS\|SR 956697663

   

  Dell - Internal Use - Confidential 

  Dear l2

   

  邮件正文: 

  a.     Detail Symptom Descriptions

  详细的故障现象描述:客户反馈激活操作失误导致当前机器不断重启，报错0xc0000138

  ![[Technology_ALL_windows_case_039_激活Windows系统问题_001.png]]

  故障的时间点 :NA

  是否可以复现故障 :NA

  如何复现故障 :NA

   

  b.    Troubleshooting Steps

  详细的诊断步骤:

  1 shift+f8，选择安全模式和最后一次正确配置，都报同样的0xc0000138错误，选择修复计算机，失败

  2 出厂配置Windows Server 2012, Standard Edition, Factory Installed, No Media, 2 Socket, 2 VMs, S-Chinese

  维修记录: (单号/更换的部件/更换后的现象)

  Bios/Driver/FW及存储控制器相关FW版本:

   

  c.     Current status

  客户公司名称: 大连睿信达信息技术有限公司

  业务影响:业务停机

  升级的原因: Pending 在哪里？遇到了什么困难？需要（SERVER/OS/Network/Storage）SME L2帮忙做什么？\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--客户表示系统还有一些程序有用的，需要尝试恢复系统，OEM 系统2012，客户不想重装系统，尝试修复

  RM/TAM:

   

  d.     Must Collect Logs

  已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): NA

  常见日志类型参考(根据实际情况获取相应日志)：

  服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

  存储(参考)：MD/EQL/NAS/CML/DR/DL log;

   

  ![[Technology_ALL_windows_case_039_激活Windows系统问题_002.jpg]]

   

   

   

   

   

  Star L

  Enterprise Technical Support Engineer, Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  [star_l@dell.com](mailto:star_l@dell.com)

   

  如果您对我的服务有任何意见或建议,也可以联系我的经理 [Harvey_Jiang@dell.com](mailto:Harvey_Jiang@dell.com)

   

 

已使用 OneNote 创建。
