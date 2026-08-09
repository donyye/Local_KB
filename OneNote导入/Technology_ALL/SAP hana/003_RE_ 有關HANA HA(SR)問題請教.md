RE: 有關HANA HA(SR)問題請教

Tuesday, June 6, 2017

3:45 PM

  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------
  主题       RE: 有關HANA HA(SR)問題請教
  发件人     Huang, Antti
  收件人     adi林俊義
  抄送       Shih, Matt; Ye, Dony; Zhang2, Yue
  发送时间   Tuesday, June 6, 2017 3:00 PM
  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------

 

您好, 林先生,

 

从log的分析来看node1的em1网路断了5秒, 导致整个bond0断, 从而cluster发生brain split, 导致HANA failover到node2

bond0由em1, em2组成, 但em2一直是down的状态, 所以一旦em1断, 整个bond0就断了. 请您检查您的网络. 确保bond0的两个NIC都是正常工作的. 

另外em4工作在100Mb的状态.

 

========= node1 log ===========

[May 26 13:29:00 hanaprd01 kernel: \[3098788.455720\] ixgbe 0000:01:00.0: em1: ]NIC Link is Down

May 26 13:29:00 hanaprd01 kernel: klogd 1.4.1, \-\-\-\-\-\-\-\-\-- state change \-\-\-\-\-\-\-\-\--

May 26 13:29:00 hanaprd01 SAPHana(rsc_SAPHana_HNP_HDB00)\[38809\]: INFO: RA ==== end action monitor_clone with rc=8 (\<version\>0.151.3\</version\>) (7s)====

May 26 13:29:00 hanaprd01 kernel: \[3098788.525294\] bonding: bond0: link status definitely down for interface em1, disabling it

[May 26 13:29:00 hanaprd01 kernel: \[3098788.526313\] bonding: ]bond0: now running without any active interface !

May 26 13:29:05 hanaprd01 corosync\[8263\]:  \[TOTEM \] A processor failed, forming new configuration.

[May 26 13:29:05 hanaprd01 kernel: \[3098793.047278\] ixgbe 0000:01:00.0: em1: ]NIC Link is Up 10 Gbps, Flow Control: None

May 26 13:29:05 hanaprd01 kernel: \[3098793.142182\] bonding: bond0: link status definitely up for interface em1, 10000 Mbps full duplex.

May 26 13:29:05 hanaprd01 kernel: \[3098793.142185\] bonding: bond0: making interface em1 the new active one.

May 26 13:29:05 hanaprd01 kernel: \[3098793.143426\] bonding: bond0: first active interface up!

May 26 13:29:09 hanaprd01 su: (to hnpadm) root on none

May 26 13:29:11 hanaprd01 corosync\[8263\]:  \[CLM   \] CLM CONFIGURATION CHANGE

May 26 13:29:11 hanaprd01 corosync\[8263\]:  \[CLM   \] New Configuration:

May 26 13:29:11 hanaprd01 corosync\[8263\]:  \[CLM   \]   r(0) ip(10.18.72.115)

May 26 13:29:11 hanaprd01 corosync\[8263\]:  \[CLM   \] Members Left:

May 26 13:29:11 hanaprd01 corosync\[8263\]:  \[CLM   \]   r(0) ip(10.18.72.116)

 

![[Technology_ALL_SAP hana_003_RE_ 有關HANA HA(SR)問題請教_001.png]]

 

 

\-\-\-- node2 把 node1 fenced (reboot) \-\-\--

[May 26 13:29:16 hanaprd02 stonith-ng\[8230\]:   notice: log_operation: ][Operation \'reboot\' \[22174\] (call 3 from crmd.8234) for host \'hanaprd01\' with device \'STONITH-hana01\' returned: 0 (OK)]

May 26 13:29:16 hanaprd02 stonith-ng\[8230\]:   notice: remote_op_done: Operation reboot of hanaprd01 by hanaprd02 for crmd.8234@hanaprd02.768d5cde: OK

May 26 13:29:16 hanaprd02 crmd\[8234\]:   notice: tengine_stonith_callback: Stonith operation 3/36:1:0:7992b622-c5ff-40d9-bdcc-5140c72bdc2d: OK (0)

 

 

 

另外, 在node2也断了几秒并工作在100Mbps

![[Technology_ALL_SAP hana_003_RE_ 有關HANA HA(SR)問題請教_002.png]]

 

 

 

 

Regards,

 

Antti Huang \| 黄怀安

Software & Solutions Master Engineer

APJ Solutions Support Team (SST)

Dell EMC \| Global Support & Deployment

 

 

\-\-\-\--Original Message\-\-\-\--

From: Huang, Antti

Sent: Monday, June 05, 2017 9:54

To: \'adi林俊義\' \<jylin@oucc.com.tw\>

Cc: Shih, Matt \<Matt_Shih@Dell.com\>; Ye, Dony \<Dony_Ye@Dell.com\>; Zhang2, Yue \<Yue_Zhang2@Dell.com\>

Subject: RE: 有關HANA HA(SR)問題請教

 

您好, 林先生,

 

webex link:

[https://dell.webex.com/dell/sc30/t.php?MTID=s194467ab578df40f0e989fe0ebdd508a](https://dell.webex.com/dell/sc30/t.php?MTID=s194467ab578df40f0e989fe0ebdd508a)

 

 

suse OS log 抓取:

在每一台HANA上用command:  \"supportconfig -a\" 就可以抓取, 会自动生成一个zip file, 把它copy出来传给我们.

 

 

Regards,

 

Antti Huang \| 黄怀安

Software & Solutions Master Engineer

APJ Solutions Support Team (SST)

Dell EMC \| Global Support & Deployment

 

 

\-\-\-\--Original Message\-\-\-\--

From: adi林俊義 \[[mailto:jylin@oucc.com.tw](mailto:jylin@oucc.com.tw)[\] ]

Sent: Monday, June 05, 2017 9:46

To: Huang, Antti \<<Antti_Huang@Dell.com>\>

Cc: Shih, Matt \<<Matt_Shih@Dell.com>\>

Subject: RE: 有關HANA HA(SR)問題請教

 

黃先生您好,

 

1. 請問如何您抓取 \"supportconfig -a\"的 log? 操作步驟為何?

2. 請您郵寄WEBEX的URL及ID，現在可以讓您操控。

謝謝您!

 

Thanks & Regards,

林俊義敬上

 

\-\-\-\--Original Message\-\-\-\--

From: <Antti.Huang@dell.com> \[[mailto:Antti.Huang@dell.com](mailto:Antti.Huang@dell.com)[\] ]

Sent: Monday, June 5, 2017 9:41 AM

To: adi林俊義

Cc: <Matt.Shih@dell.com>

Subject: RE: 有關HANA HA(SR)問題請教

 

您好, 林先生,

 

今天上午可以webex, 下午已有安排.  

另外, 关于为什么会failover, 仍是需要检查日志, 请您抓取 \"supportconfig -a\"的 log并上传给Dell TS. 谢谢.

 

Regards,

 

Antti Huang \| 黄怀安

Software & Solutions Master Engineer

APJ Solutions Support Team (SST)

Dell EMC \| Global Support & Deployment

 

\-\-\-\--Original Message\-\-\-\--

From: adi林俊義 \[[mailto:jylin@oucc.com.tw](mailto:jylin@oucc.com.tw)[\] ]

Sent: Sunday, June 04, 2017 18:24

To: Huang, Antti \<<Antti_Huang@Dell.com>\>

Cc: Shih, Matt \<<Matt_Shih@Dell.com>\>

Subject: RE: 有關HANA HA(SR)問題請教

 

黃先生您好,

 

報修號碼為949138993

謝謝您!

 

\-\-\-\--Original Message\-\-\-\--

From: adi林俊義 

Sent: Sunday, June 04, 2017 6:12 PM

To: \'Antti.Huang@dell.com\'

Cc: <Matt.Shih@dell.com>

Subject: RE: 有關HANA HA(SR)問題請教

 

黃先生您好,

 

首先感謝您的回覆。

是否明天(6/5)可以連線至本公司查看情況? 

感謝您!

 

林俊義  敬上

\-\-\-\--Original Message\-\-\-\--

From: <Antti.Huang@dell.com> \[[mailto:Antti.Huang@dell.com](mailto:Antti.Huang@dell.com)[\]]

Sent: Sunday, June 04, 2017 10:11 AM

To: [Matt.Shih@dell.com](mailto:Matt.Shih@dell.com); adi林俊義

Subject: RE: 有關HANA HA(SR)問題請教

 

Hi 林先生，

 

需要抓取日志，开个case检查一下什么原因导致切换了。

另外，下面的步骤要先注册，不能停了node2来注册1

 

Antti.

 

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

发件人: Shih, Matt

发送时间: 2017年6月3日 11:57

收件人: adi林俊義

抄送: Huang, Antti

主题: RE: 有關HANA HA(SR)問題請教

 

Dell - Internal Use - Confidential

 

林先生:

 

剛打電話去貴公司找你，但你不在座位上。可以麻煩撥個電話給我，謝謝。

0910138883

 

 

 

 

Sincerely,

 

Matt Chia-Shine Shih (施嘉賢)

Technical Account Manager

Dell \| Global Support & Deployment, Global TAM Services, Taiwan Office +886 2 23766550, fax + 886 2 27369889 Mobile + 886 910138883 Address 20F, No. 218, Sec. 2, Dunghua South Rd., Taipei City 106, Taiwan How am I doing? Please email my Manager [Victor_Yeung@dell.com\<mailto:Victor_Yeung@dell.com](mailto:Victor_Yeung@dell.com%3cmailto:Victor_Yeung@dell.com)\>

 

\[Description: Description: Description: New tagline\]

 

 

From: adi林俊義 \[[mailto:jylin@oucc.com.tw](mailto:jylin@oucc.com.tw)[\]]

Sent: Saturday, June 3, 2017 11:46 AM

To: Huang, Antti \<<Antti_Huang@Dell.com>\>

Cc: Shih, Matt \<<Matt_Shih@Dell.com>\>

Subject: 有關HANA HA(SR)問題請教

Importance: High

 

黃先生您好,

 

您好,

 

1.本公司的HANA二部主機HA架構似乎又出問問題了，如下圖。這次是PRD1出問題。

\[<cid:image004.png@01D2DC54.10AFDC90>[\]]

 

2. 這次是PRD1(node 1)出問題。如上次與您連絡，因現在是PRD2為primary，若要,將node 1再次加入HA架構，是否步驟如下:

  2.1 Login Node2(PRD2)下指令/usr/sap/HNP/HDB00/HDB stop

  2.2 Login Node1(prd1)下指令 hdbnsutil  \--sr_register  \--name-hnp02  \--remoteHost=hanaprd01  \-- remoteInstance=00  \--mode=sync

 

3.期待您的回覆!

感謝您!

 

Thanks & Regards,

\[outlook簽名檔[\]]

 

 

已使用 OneNote 创建。
