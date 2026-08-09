RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

2019年9月3日

16:03

  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping
  From      Huang, Antti
  To        Lai, Will; Yin, Victor; J1, Manoj Kumar
  Cc        Wang, Bob; W, King; Ye, Dony; Wang, Xing Fang
  Sent      2019年9月3日 15:50
  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Internal Use - Confidential

 

Hi Will,

 

经过分析, 我们得到的结论是一样的, bonding连续3秒没有发送LACPDU导致switch中断LACP group.

 

从tcpdump看, 交换机要求服务器第1秒发送一个LACPDU (fast), 而服务器只要求交换机每30秒发送一次(slow).

所以我们可以看到交换机每30秒才发一次. 而主机每秒都会发送.

![[Technology_ALL_Linux 问题收集_054_RE_ ST#1MP00Q2 _ SR#990684816 长鑫存储技朮有限公司_001.jpg]]

 

但在18:00:46最后一次主机发出后, 长达3秒没再发出. 交换机发现问题,  在3秒后连发了两个Expired通知要求服务器响应.  服务器立即响应, 但已经是第4秒了. ungroup发生在连续3次失去LACPDU.

![[Technology_ALL_Linux 问题收集_054_RE_ ST#1MP00Q2 _ SR#990684816 长鑫存储技朮有限公司_002.jpg]]

 

所以我们的建议是, 将服务器的LACP rate设置成和交换机一致. 交换机的设置是fast(每秒), 所以建议设置服务器的LACP rate为fast. 设置后, 交换机也会第秒发送LACPDU包. 就算丢一两个包, 服务器也能及时响应. 

另外, 也可以将交换机设置成slow (long timeout/30s), 这样在短时间上网络堵塞时, LACP不受影响.

目标是两边设置成一致.

 

在失联的那3秒中, 由于没有性能日志, 我们没法看到OS当时的网络状况. 我尝试找即有交换机日志, 也有OS sar日志的现有logs. 只有如下May 11有. 所以我看了当时的网络情况.

May 11 04:27:39 UTC %STKUNIT1-M:CP %LACP-5-PORT-GROUPED: PortChannel-013-Grouped: Interface Hu 1/13 joined port-channel 13.

May 11 04:27:39 UTC %STKUNIT1-M:CP %IFMGR-5-OSTATE_UP: Changed interface state to up: Po 13

May 11 04:27:39 UTC %STKUNIT1-M:CP %IFMGR-5-OSTATE_DN: Changed interface state to down: Po 13

May 11 04:27:39 UTC %STKUNIT1-M:CP %LACP-5-PORT-UNGROUPED: PortChannel-013-Ungrouped: Interface Hu 1/13 exited port-channel 13.

 

在4:30的性能日志里看到, 网络流量不大, 但有较大的广播包. 同时也发生了rxdrop. 但由于此默认的sar是每10分钟抓一次. 此数据暂无法说明什么, 仅做参考.

12:00:01 AM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s

04:30:01 AM     bond0   9793.09    912.32  13625.82  11701.15      0.00      0.00 7165809.28

 

12:00:01 AM     IFACE   rxerr/s   txerr/s    coll/s  rxdrop/s  txdrop/s  txcarr/s  rxfram/s  rxfifo/s  txfifo/s

04:30:01 AM     bond0      0.00      0.00      0.00      0.69      0.00      0.00      0.00      0.00      0.00

04:30:01 AM      p1p1      0.00      0.00      0.00      0.55      0.00      0.00      0.00      0.00      0.00

04:30:01 AM      p1p2      0.00      0.00      0.00      0.13      0.00      0.00      0.00      0.00      0.00

 

所以我建议, 将bonding的lacp rate改为fast, 并继续之前的抓包.  并在服务器上每秒监控网络情况. 将下面的两个命令重定向到指定文件, 如下. (命令在后台运行, 可以关闭ssh)

[\[root@test1 \~\]#] screen -dmS DEV sh -c \'sar -n DEV 1 \| while read p; do echo \"\$(date) \$p\" &\>\> \~/Net-Mon-Dev.txt; done\'

[\[root@test1 \~\]#] screen -dmS EDEV sh -c \'sar -n EDEV 1 \| while read p; do echo \"\$(date) \$p\" &\>\> \~/Net-Mon-EDev.txt; done\'

[\[root@test1 \~\]#] screen -ls

There are screens on:

     32596.EDEV (Detached)

     32431.DEV  (Detached)

2 Sockets in /var/run/screen/S-root.

 

[\[root@test1 \~\]#] ls \~/Net-Mon-\*

/root/Net-Mon-Dev.txt  /root/Net-Mon-EDev.txt

 

如要停止此两个命令的监控, 可操作如下:

[\[root@test1 \~\]#] screen -r DEV    #(Enter后按 Ctrl+c)

\[screen is terminating\]

[\[root@test1 \~\]#] screen -r EDEV   #(Enter后按 Ctrl+c)

\[screen is terminating\]

[\[root@test1 \~\]#] screen -ls       #(查看当前session已没有了)

No Sockets found in /var/run/screen/S-root.

 

 

\# 修改bonding的LACP rate:

[https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-using_channel_bonding](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-using_channel_bonding)

![[Technology_ALL_Linux 问题收集_054_RE_ ST#1MP00Q2 _ SR#990684816 长鑫存储技朮有限公司_003.jpg]]

 

当前ifcfg-bondx设置.

BONDING_OPTS=mode=802.3ad

改成:

BONDING_OPTS=\"mode=802.3ad lacp_rate=1\"

重启bond

验证bond已经使用LACP rate: fast

cat /proc/net/bonding/bond0

 

当如果问题再次发生时, 收集所有的抓包和上面的Net-Mon\*.txt

 

 

Regards,

 

Antti Huang

Senior Principal Engineer, Infrastructure & Client Solutions

Solutions Support Team (SST)

Dell EMC \| Support & Deployment Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| MCSE 2008 \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 9:00 ‒ 17:30 (GMT+8)

 

From: Lai, Will

Sent: Saturday, August 31, 2019 00:03

To: Huang, Antti; Yin, Victor; J1, Manoj Kumar

Cc: Wang, Bob; W, King; Ye, Dony; Wang, Xing Fang

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Internal Use - Confidential

 

Antti,

 

Got it. Thanks for your great support!

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: Huang, Antti

Sent: 2019年8月30日 18:23

To: Lai, Will; Yin, Victor; J1, Manoj Kumar

Cc: Wang, Bob; W, King; Ye, Dony; Wang, Xing Fang

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Internal Use - Confidential

 

Hi Will,

 

Just synced all the info with Bob and Victor. Will review and update you on Monday. Thanks.

 

Regards,

 

Antti Huang

Senior Principal Engineer, Infrastructure & Client Solutions

Solutions Support Team (SST)

Dell EMC \| Support & Deployment Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| MCSE 2008 \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 9:00 ‒ 17:30 (GMT+8)

 

From: Lai, Will

Sent: Friday, August 30, 2019 17:32

To: Yin, Victor; J1, Manoj Kumar

Cc: Wang, Bob; W, King; Ye, Dony; Wang, Xing Fang; Huang, Antti

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Internal Use - Confidential

 

Hi Victor, Manoj Kumar,

 

Does any progress in this case?

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: Yin, Victor

Sent: 2019年8月29日 17:39

To: Lai, Will; J1, Manoj Kumar

Cc: Wang, Bob; W, King; Ye, Dony; Wang, Xing Fang

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

HI will

 

I have discuss with Bob and Manoj , SST agree open case to redhat but need checked with antti , but antti is unavailable now, Manoj will replay us tomorrow.

 

 

Best Regards

Victor Yin

 

 

 

From: Yin, Victor

Sent: 2019年8月28日 18:40

To: Lai, Will; J1, Manoj Kumar

Cc: Wang, Bob; W, King; Ye, Dony; Wang, Xing Fang

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Dell - Internal Use - Confidential 

Hi Will

 

Manoj is working on this case .

SST suspect  seems customer did not configure LACP on the NIC bond interface via correct way so we need check few things on this before escalated to redhad formally .

 

Customer has some connections which is em1,em2,em3 and em4 and p1p1, P1P2

Customer created two bonds which is bond0 and bond1

For bond0 they configured P1p1 and P1p2

For bond1 they configured em1 and em2 interfaces

But in the actual nic information configurations he didnt mention anything about the bonding details

Because on that we  believe customer LACP configuration is not correct

 

please confirm with the customer that which method they used to configure LACP ,nmtui or nmcli

also check with customer that they did only LACP or they configured LACP with ether channel on the switch side as well

 

Best Regards

Victor Yin

 

 

 

From: Lai, Will

Sent: 2019年8月26日 14:20

To: Yin, Victor; J1, Manoj Kumar

Cc: Wang, Bob; W, King; Ye, Dony

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Internal Use - Confidential

 

Hi Victor, Manoj Kumar,

 

Does any progress in this case?

 

Thanks!

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: Yin, Victor

Sent: 2019年8月23日 13:29

To: Lai, Will; Wang, Bob; W, King; Ye, Dony; J1, Manoj Kumar

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Dell - Internal Use - Confidential 

Hi All

 

Loop Linux SST

MANOJ_KUMAR_J1 will help us check sos report to analysis this network issue.

 

 

Best Regards

Victor Yin

 

 

 

 

From: Lai, Will

Sent: 2019年8月23日 9:15

To: Wang, Bob; W, King; Ye, Dony; Yin, Victor

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Internal Use - Confidential

 

Hi Bob,

 

了解了，谢谢。

 

Hi Victor，

 

Dony这个Case，麻烦帮忙跟进一下，需要升级到Redhat。谢谢！

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: Wang, Bob

Sent: 2019年8月23日 0:10

To: Lai, Will; W, King; Ye, Dony

Subject: 答复: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Hi Will,

 

我们之前收集的很多日志，包裹sysinfo-snapshot和mstdump之类的数据都是提供给Mellanox研发，一方面给让他们了解客户的配置，另外一方面也让他们从这些数据来从网卡本身的角度来看是不是存在什么问题。现在综合这些数据， Mellanox研发并没有发现网卡在处理数据的时候本身有什么异常。所以，再结合LACP配置的关系，推断出这个问题现象可能更多的来自于Linux系统本身，或者是bond driver本身。因为按照Mellanox研发工程师的确认，网卡驱动本身并不控制LACP PDU的发送。这个工作是有bonding driver控制地。

 

所以，现在我们的发现的现象是LACP PDU在某个瞬间停止发出给交换机端了，这个现在看来很可能是bonding驱动的处理问题。所以还是要请redhat的工程师进来帮忙确认这个现象是不是一个已知的问题，redhat Linux本身对于bonding driver有没有对应的补丁。

 

如果退一步，即便这个问题还没法被认定为那个方面的的问题的话，那么至少在有了redhat支持进来后，他们应该可以提供针对于bonding driver的debug方法来确认到底问题出在哪里。

 

所以，综合这些情况，我觉得当前很有必要尽快升级case给Redhat。

 

谢谢！

 

Bob Wang

 

发件人: Lai, Will

发送时间: 2019年8月22日 14:58

收件人: Wang, Bob; W, King; Ye, Dony

主题: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping 

 

Dell Customer Communication - Confidential

 

Loop Linux L2 Dony

 

Hi Bob,

 

谢谢反馈。我们拿了很多日志给他们，包括Dump, sysinfo-snapshot，有什么其他的发现没有呢？

理解他们认为LACP PDU是Bond driver控制，因此引入Red Hat这个我觉得也算合理。但这个包需要通过网卡发出去，他们是否有什么动作能确认固件和驱动方面有没有问题。

 

也请Dony关注，看看怎么引入Linux厂商进来。或者我们电话讨论一下。谢谢！

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: Wang, Bob

Sent: 2019年8月22日 11:23

To: Lai, Will; W, King

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Dell Customer Communication - Confidential

 

HI Will,

 

昨晚, 我收到Mellanox研发的回复, 针对我们现在这个case, 他们看了我们提供的所有信息后,给我们的建议认为LACP PDU是由bond driver控制的, 并不是由网卡驱动控制的. 所以这个问题的后续分析需要让Redhat支持人员加入进来.可能需要他们提供bond driver的补丁来修复目前我们碰到的问题. 如下是他们针对这个问题的comments:

 

"In general, it\'s the higher level bonding driver that sends the lacp pdus. If you don\'t see the packets on the tcpdump on the host port, then Redhat should be involved."

 

另外, 针对于目前客户的LACP Rate在交换机端和服务器端的不一样的配置, Mellanox研发也提出了同样的建议:

 

"As indicated in the troubleshooting guide - It is recommended that both ends have the same LACP-PDU speed configuration."

 

谢谢!

Bob Wang

From: Lai, Will

Sent: Thursday, August 15, 2019 3:39 PM

To: Wang, Bob; W, King

Subject: FW: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Dell Customer Communication - Confidential

 

FYI.

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: JinBiao Wang \<<JinBiao.Wang@cxmt.com>\> 

Sent: 2019年8月15日 15:32

To: Lai, Will

Cc: Jacky Hsu; ZhiPeng Li; ZhiLin Huang; Alfred Lo; Hui Zhan

Subject: 答复: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

\[EXTERNAL EMAIL\]

Hi Will，

        20190815 14:51发生link down，附件中交换机debug日志，100Gb网卡抓包，请尽快分析。

 

发件人: [Will.Lai@Dell.com](mailto:Will.Lai@Dell.com) \[[mailto:Will.Lai@Dell.com](mailto:Will.Lai@Dell.com)[\] ]

发送时间: 2019年8月15日 12:16

收件人: JinBiao Wang \<[JinBiao.Wang@cxmt.com](mailto:JinBiao.Wang@cxmt.com)\>

抄送: Jacky Hsu \<[Jacky.Hsu@cxmt.com](mailto:Jacky.Hsu@cxmt.com)\>; ZhiPeng Li \<[ZhiPeng.Li@cxmt.com](mailto:ZhiPeng.Li@cxmt.com)\>; ZhiLin Huang \<[ZhiLin.Huang@cxmt.com](mailto:ZhiLin.Huang@cxmt.com)\>; Alfred Lo \<[Alfred.Lo@cxmt.com](mailto:Alfred.Lo@cxmt.com)\>; Hui Zhan \<[Hui.Zhan@cxmt.com](mailto:Hui.Zhan@cxmt.com)\>

主题: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

此为外部邮件，请注意内容是否涉及敏感信息

This is an external E-mail, please note if the content involves sensitive information. 

 

 

 

Dell Customer Communication - Confidential

 

HI JinBiao,

 

sysinfo-snapshot已经转给后端了，还在确认是否可用。另外，Mstdump只导出了一份，需要导出3次。

 

#mst start

#mst status（这将显示Mellanox适配器的设备名称）

#mstdump \<设备名称\> \> mstdump1.txt

#mstdump \<设备名称\> \> mstdump2.txt

#mstdump \<设备名称\> \> mstdump3.txt

 

另外，麻烦提供交换机、主机日志、交换机Debug输出、主机&交换机端在那个时间段的抓包，谢谢！

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: Lai, Will

Sent: 2019年8月14日 11:30

To: \'JinBiao Wang\'

Cc: Jacky Hsu; ZhiPeng Li; ZhiLin Huang; Alfred Lo; Hui Zhan

Subject: RE: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

Dell Customer Communication - Confidential

 

Hi JinBiao,

 

收到，辛苦了。日志我已经转给后端，有进展我再更新给各位。谢谢！

 

Best regards,

 

Will Lai

Resolution Manager, Great China Customer Support Services

DellEMC \| Support and Deployment Services

Dell Line: 8832747

Phone: +86 21 2203 2747

Email <Will_lai@Dell.com>

 

From: JinBiao Wang \<<JinBiao.Wang@cxmt.com>\> 

Sent: 2019年8月14日 9:46

To: Lai, Will

Cc: Jacky Hsu; ZhiPeng Li; ZhiLin Huang; Alfred Lo; Hui Zhan

Subject: 答复: ST#1MP00Q2 \| SR#990684816 长鑫存储技朮有限公司 z9100 Port-Channel flapping

 

\[EXTERNAL EMAIL\]

Hi Will，

        已隐去相关敏感信息，请帮忙分析。

 

 

已使用 OneNote 创建。
