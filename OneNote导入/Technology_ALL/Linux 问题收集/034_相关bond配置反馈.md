相关bond配置反馈

2018年2月8日

12:25

- ::: 
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       FW: 回复： 回复：[  ]建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈
    发件人     Ruan, Garuda
    收件人     Ye, Dony
    发送时间   2018年2月8日 11:29
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  学习下

   

  有这个玩意嘛

   

   

  ethtool \--set-priv-flags

   

  From: Lv, Richard

  Sent: Thursday, February 8, 2018 11:26 AM

  To: Liu, Tianqiao \<Tianqiao_Liu@dell.com\>; Ruan, Garuda \<Garuda_Ruan@DELL.com\>

  Cc: Yu, Cheysi \<Cheysi_Yu@Dell.com\>; Wang, Xing Fang \<Xing_Fang_Wang@DELL.com\>; Wu, Xi \<Xi_Wu@Dell.com\>; Zheng, Xin \<Xin_Zheng@Dell.com\>

  Subject: FW: 回复： 回复： 建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈

   

  Dell - Internal Use - Confidential 

  Hi, Team.

   

  今天刚刚和网易，intel一起开会，已经明确问题所在。

  1.  自x710网卡以后，intel的网卡driver在收到ifconfig 网卡 down的命令后，不会关闭该物理网卡的物理链路，（通俗讲不会power off eth PHY）。
  2.  交换机认为该物理链接完好，没有做bond的数据传输切换，直到3次未接收到LACPDU的包后才知道该链路失效，导致延时超过1分钟以上。
  3.  解决办法：使用命令设置priv-flag。确保后续ifconfig down时可以关闭网卡的物理链路。\
      ethtool \--set-priv-flags \<dev\>  link-down-on-close on     

   

  客户询问：为什么x520是直接设定关闭物理PHY, 而x710则不会? 

  intel的答复是，现在很多厂家是使用share mode方式将网卡和BMC share在一起（NC-SI）, 如果power off eth PHY，会导致BMC无法通讯。

   

  Best Regards.

   

  Richard Lv (吕乐)

  Technology Service Manager

  Dell \| GSD.TAM.GC

  Mobile +86 180 7289 3030

   

  From: Li, Zhixuan \[[mailto:zhixuan.li@intel.com](mailto:zhixuan.li@intel.com)\]

  Sent: 2018年2月8日 10:57

  To: Yu, Cheysi \<<Cheysi_Yu@Dell.com>\>; <huanghaoran@corp.netease.com>; Lv, Richard \<<Richard_Lv@DELL.com>\>

  Cc: <zhaowenke@corp.netease.com>; <hzzhaoqingwu@corp.netease.com>; <hzhuangli@corp.netease.com>; <caoliwei@corp.netease.com>; <hzluozewen@corp.netease.com>; Cheng, Hao \<<Hao_Cheng@Dell.com>\>; Mai, Zhi Gang \<<Zhi_Gang_Mai@Dell.com>\>; Wu, Xi \<<Xi_Wu@Dell.com>\>; Nie, Ricky \<<Ricky_Nie@Dell.com>\>; Zhao, Yu Qiang \<<Yu_Qiang_Zhao@Dell.com>\>; Zheng, Xin \<<Xin_Zheng@Dell.com>\>; Jin, Michael \<<michael.jin@intel.com>\>

  Subject: RE: 回复： 回复： 建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈

   

  Michael发的命令:

   

  和X520相比较，X710在ifconfig down时并不一定会把PHY DOWN掉，因此物理链路并不一定会断开。

  如果期望物理端口在ifconfig down时DOWN掉，则可以执行下面的命令

  ethtool \--set-priv-flags \<dev\>  link-down-on-close on     

  然后再执行ifconfig down

   

   

   

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_001.png]]

  Leon(黎志煊) 

  Industry Technical Specialist (CSP) 

  ISG, Intel Corporation, PRC Geo

  Mobile: +86 18018714881

  Email: <zhixuan.li@intel.com>

   

  From: <Cheysi.Yu@dell.com> \[[mailto:Cheysi.Yu@dell.com](mailto:Cheysi.Yu@dell.com)\]

  Sent: Wednesday, February 7, 2018 4:36 PM

  To: <huanghaoran@corp.netease.com>; <Richard.Lv@dell.com>

  Cc: Li, Zhixuan \<<zhixuan.li@intel.com>\>; <zhaowenke@corp.netease.com>; <hzzhaoqingwu@corp.netease.com>; <hzhuangli@corp.netease.com>; <caoliwei@corp.netease.com>; <hzluozewen@corp.netease.com>; <H.Cheng@dell.com>; <Zhi.Gang.Mai@dell.com>; <Xi.Wu@dell.com>; <Ricky.Nie@dell.com>; <Yu.Qiang.Zhao@dell.com>; <Xin.Zheng@dell.com>

  Subject: RE: 回复： 回复： 建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈

   

   

   

   

   

  Loop Ricky and Yuqiang

   

   

   

   

   

  Best Regards

  Cheysi Yu

  TSM

   

   

  From: 黄浩然 \[[mailto:huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)[\] ]

  Sent: 2018年2月7日 16:32

  To: Lv, Richard \<<Richard_Lv@DELL.com>\>

  Cc: [zhixuan.li@intel.com](mailto:zhixuan.li@intel.com); Yu, Cheysi \<[Cheysi_Yu@Dell.com](mailto:Cheysi_Yu@Dell.com)\>; [zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com); 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>; [hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com); [caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com); [hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com); Cheng, Hao \<[Hao_Cheng@Dell.com](mailto:Hao_Cheng@Dell.com)\>; Mai, Zhi Gang \<[Zhi_Gang_Mai@Dell.com](mailto:Zhi_Gang_Mai@Dell.com)\>; Wu, Xi \<[Xi_Wu@Dell.com](mailto:Xi_Wu@Dell.com)\>; Zheng, Xin \<[Xin_Zheng@Dell.com](mailto:Xin_Zheng@Dell.com)\>

  Subject: 回复： 回复： 建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈

   

  吕工，您好！

        

            下午我们x710的测试结果如下：

            我们分别测试了x710使用802.3ad和balance-alb两种bond模式

            使用802.3ad模式down一个网口会有约70s的failure over，Link Failure Count会立即加1，ceph业务会受到影响

            使用balance-alb模式down一个网口会有约3-5s的failure over，Link Failure Count会立即加1，ceph业务也会受到影响

            

            LACP rate值我们和业务方商讨过，也参考过大量资料，修改这个值会导致在网络环境不稳定情况下出现频繁切换，对业务影响相当大，即使ifconfig down网口后不影响ceph，我们也无法采用此方式

   

            x520和x710同环境对比测试结果还请参考上午给您发的邮件，谢谢！

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月7日 16:25，[\<Richard.Lv@dell.com\>](mailto:Richard.Lv@dell.com) 写道： 

  Dell - Internal Use - Confidential 

  Hi, 黄工，

   

  X520和x710使用的网卡的driver是不同的。我们正在看能否找到实验环境。现在等待你x710的测试结果。

   

  此外如下测试区别在于，设置网卡down，是清掉了IFF_UP(administrative state)标志，拔掉网线，是清掉了IFF_RUNNING(operational state)标志。

  所以这两个测试方式的处理模式也是不一样的。

   

  ip link set down:

  iplink_modify: RTM_NEWLINK = 16 

      \|-\> rtnl_talk -\> sendmsg(send a message on a socket) -\> req-\>i.ifi_flags &= \~IFF_UP          //将IFF_UP标志位清零

   

  ifconfig down:

  clr_flag(ifr.ifr_name, IFF_UP);         //将IFF_UP标志位清零

   

  可以看到这两个操作都是设置网卡状态标志位IFF_UP

  在netdevice socket interface中定义了关于网卡的状态标志，其中IFF_UP和IFF_RUNNING两个状态的描述为：

  IFF_UP            Interface is running, admin up

  IFF_RUNNING       Resources allocated, RFC2863 operational state up

   

   

   

  Best Regards.

   

  Richard Lv (吕乐)

  Technology Service Manager

  Dell \| GSD.TAM.GC

  Mobile +86 180 7289 3030

   

  From: 黄浩然 \[[mailto:huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)[\] ]

  Sent: 2018年2月7日 12:02

  To: Lv, Richard \<<Richard_Lv@DELL.com>\>

  Cc: [zhixuan.li@intel.com](mailto:zhixuan.li@intel.com); Yu, Cheysi \<[Cheysi_Yu@Dell.com](mailto:Cheysi_Yu@Dell.com)\>; [zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com); 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>; [hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com); [caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com); [hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com); Cheng, Hao \<[Hao_Cheng@Dell.com](mailto:Hao_Cheng@Dell.com)\>; Mai, Zhi Gang \<[Zhi_Gang_Mai@Dell.com](mailto:Zhi_Gang_Mai@Dell.com)\>; Wu, Xi \<[Xi_Wu@Dell.com](mailto:Xi_Wu@Dell.com)\>; Zheng, Xin \<[Xin_Zheng@Dell.com](mailto:Xin_Zheng@Dell.com)\>

  Subject: 回复： 建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈

   

  吕工，你好！

   

            我们把x710网卡的机器换成x520后，使用完全一致的配置进行测试，未发现有failure over情况。我们多次测试使用的 LACP rate参数均为slow

              

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_002.png]]

           

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_003.png]]

          关于x710网卡使用ifconfig down后是否有延时，以及link failure count增加时间等我们稍后会测试给出反馈

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月7日 08:12，[\<Richard.Lv@dell.com\>](mailto:Richard.Lv@dell.com) 写道： 

  Dell - Internal Use - Confidential 

  黄工，你好！

   

  从配置的比对来看，双方的配置没有什么差别。

   

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_004.jpg]]

      

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_005.jpg]]

   

  但是从bond的状态来看

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_006.jpg]]

     

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_007.jpg]]

   

  X710网卡增加了 802.3ad info的信息。但是link failure count为0.

  麻烦能否增加如下测试，以便我们来隔离看看到底是那块出现问题。

  1， 使用ifconfig down 来测试网口，检查bond状态，看看多长时间，link failure count增加。

  一般情况下交换机LACP来判断超时是通过接收LACPDU的时间，短超时是1秒，长超时是30秒，在3倍LACP超时

  时间之后，如果还没有收到LACPDU，则认为端口失效。从测试的结果来看长达1分钟的IO停顿，基本上是3倍的

  超时。结合bond0 LACP rate: slow。应该是使用长超时30秒。看看后续能否修改成短超时1秒。

  2， 将bond的mode设置为6. 也就是balance-alb，（Adaptive load balancing）

  在此模式下，load balance与交换机无关。再使用ifconfig down，看此时是否有延时，以及link failure count增加。

  以排除交换机的LACP因素。

   

  目前该case已经升级到L2, 今天我们会和L2联系，看能否在厦门搭建一套实验环境，不过我们会采用redhat或centos来

  测试x710在802.3ad模式下的failure over 的情况。后续再和各位沟通

   

  Best Regards.

   

  Richard Lv (吕乐)

  Technology Service Manager

  Dell \| GSD.TAM.GC

  Mobile +86 180 7289 3030

   

  From: 黄浩然 \[[mailto:huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)[\] ]

  Sent: 2018年2月6日 19:09

  To: Lv, Richard \<<Richard_Lv@DELL.com>\>; <zhixuan.li@intel.com>

  Cc: Yu, Cheysi \<[Cheysi_Yu@Dell.com](mailto:Cheysi_Yu@Dell.com)\>; [zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com); 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>; [hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com); [caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com); [hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com); Cheng, Hao \<[Hao_Cheng@Dell.com](mailto:Hao_Cheng@Dell.com)\>

  Subject: 建德集群ceph存储适配x710网卡风险-x520相关bond配置反馈

   

  Dear  吕工&Li工：

            x520网卡bond配置如下：

  auto bond1

  iface bond1 inet manual

    bond_slaves eth0 eth1

    bond_mode 802.3ad

    bond_xmit_hash_policy layer2+3

    bond_miimon 100

    bond_updelay 200

    bond_downdelay 200

   

  /proc/net/bonding/bond1的部分信息如下：

  Bonding Mode: IEEE 802.3ad Dynamic link aggregation

  Transmit Hash Policy: layer2+3 (2)

  MII Status: up

  MII Polling Interval (ms): 100

  Up Delay (ms): 200

  Down Delay (ms): 200

   

  Slave Interface: eth0

  MII Status: up

  Speed: 10000 Mbps

  Duplex: full

  Link Failure Count: 7

   

  Slave Interface: eth1

  MII Status: up

  Speed: 10000 Mbps

  Duplex: full

  Link Failure Count: 5

   

   

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月6日 16:43，[黄浩然\<huanghaoran@corp.netease.com\>](mailto:huanghaoran@corp.netease.com) 写道： 

  Dear  吕工：

           您好！

           bond中配置的部分参数同您那不太一致，我提供下我们的bond设置(先提供x710网卡的配置供参考，x520的稍晚些发给您）

  auto bond0

  iface bond0 inet manual

    bond_slaves eth0 eth1

    bond_mode 802.3ad

    bond_xmit_hash_policy layer2+3

    bond_miimon 100

    bond_updelay 200

    bond_downdelay 200

  在/proc/net/bonding/bond0中部分配置为：

  Bonding Mode: IEEE 802.3ad Dynamic link aggregation

  Transmit Hash Policy: layer2+3 (2)

  MII Status: up

  MII Polling Interval (ms): 100

  Up Delay (ms): 200

  Down Delay (ms): 200

   

  802.3ad info

  LACP rate: slow

  Min links: 0

  Aggregator selection policy (ad_select): stable

   

  Slave Interface: eth0

  MII Status: up

  Speed: 10000 Mbps

  Duplex: full

  Link Failure Count: 0

   

  Slave Interface: eth1

  MII Status: up

  Speed: 10000 Mbps

  Duplex: full

  Link Failure Count: 0

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月6日 16:32，[\<Richard.Lv@dell.com\>](mailto:Richard.Lv@dell.com) 写道： 

  Dell - Internal Use - Confidential 

   

  浩然，你好！

   

  如果有可能，能否提供X520以及X710网卡的/proc/net/bonding/bond0（bond的名称），信息给我。谢谢。

   

   

  Best Regards.

   

  Richard Lv (吕乐)

  Technology Service Manager

  Dell \| GSD.TAM.GC

  Mobile +86 180 7289 3030

   

  From: Lv, Richard

  Sent: 2018年2月6日 16:21

  To: \'黄浩然\' \<[huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)\>; Yu, Cheysi \<[Cheysi_Yu@Dell.com](mailto:Cheysi_Yu@Dell.com)\>

  Cc: [zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com); 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>; [hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com); [caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com); [hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com); Cheng, Hao \<[Hao_Cheng@Dell.com](mailto:Hao_Cheng@Dell.com)\>

  Subject: RE: 回复： 回复： 建德集群ceph存储适配x710网卡风险

   

  Dell - Internal Use - Confidential 

  浩然，你好！

   

  按照我们刚才电话沟通，麻烦能否提供bond的配置文件给我。

  举例如下，主要是想了解bonding的模式，mode?, 以及miimon的值。谢谢。

   

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_008.jpg]]

   

   

   

   

  Best Regards.

   

  Richard Lv (吕乐)

  Technology Service Manager

  Dell \| GSD.TAM.GC

  Mobile +86 180 7289 3030

   

  From: 黄浩然 \[[mailto:huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)[\] ]

  Sent: 2018年2月6日 14:03

  To: Yu, Cheysi \<<Cheysi_Yu@Dell.com>\>

  Cc: [zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com); 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>; [hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com); Lv, Richard \<[Richard_Lv@DELL.com](mailto:Richard_Lv@DELL.com)\>; [caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com); [hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com); Cheng, Hao \<[Hao_Cheng@Dell.com](mailto:Hao_Cheng@Dell.com)\>

  Subject: 回复： 回复： 建德集群ceph存储适配x710网卡风险

   

  Dear  Yu：

            建德环境ceph集群x710网卡排查进度怎么样了，这个问题对业务影响还是非常大的，望有进展能及时邮件反馈，谢谢！

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月5日 18:19，[黄浩然\<huanghaoran@corp.netease.com\>](mailto:huanghaoran@corp.netease.com) 写道： 

  Dear  Yu:

            目前发现72台ceph存储机器都有这个现象

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月5日 17:36，[\<Cheysi.Yu@dell.com\>](mailto:Cheysi.Yu@dell.com) 写道： 

   

   

  浩然，现在发现有多少台的机器会有以下的现象？

   

   

   

   

   

   

   

  Best Regards

  Cheysi Yu

  TSM

   

   

  From: 黄浩然 \[[mailto:huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)[\] ]

  Sent: 2018年2月5日 15:47

  To: Yu, Cheysi \<<Cheysi_Yu@Dell.com>\>

  Cc: [zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com); 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>; [hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com); Lv, Richard \<[Richard_Lv@DELL.com](mailto:Richard_Lv@DELL.com)\>; [caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com); [hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com)

  Subject: 回复： 建德集群ceph存储适配x710网卡风险

   

  Dear Yu:

          您好！

          我们测试的ceph机器相关信息如下，如需补充请及时联系我，也请排查有相关进展及时反馈给我们，谢谢！

          操作系统：

          Description:    Debian GNU/Linux 8.9 (jessie)

          Release:        8.9

          Codename:       jessie

          kernel：       3.16.0-4-amd64

          X710驱动：

          driver: i40e

          version: 2.3.6

          firmware-version: 6.00 0x800034eb 18.3.6

          TSR日志见附件

   

   

  Thanks

  = = = = = = = = = == = =

  黄浩然

  公司：网易（杭州）网络有限公司

  部门：杭研院-运维部-系统运维组

  POPO：[huanghaoran\@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  手机：15951672021

  在2018年2月5日 14:41，[\<Cheysi.Yu@dell.com\>](mailto:Cheysi.Yu@dell.com) 写道： 

   

   

   

   

  好的，收到。

   

   

   

   

   

  Best Regards

  Cheysi Yu

  TSM

   

   

  From: zhaowenke \[[mailto:zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com)\]

  Sent: 2018年2月5日 14:39

  To: Yu, Cheysi \<<Cheysi_Yu@Dell.com>\>; <hzzhaoqingwu@corp.netease.com>; <hzhuangli@corp.netease.com>; Lv, Richard \<<Richard_Lv@DELL.com>\>; <caoliwei@corp.netease.com>

  Cc: <hzluozewen@corp.netease.com>; <huanghaoran@corp.netease.com>; zhaowenke \<<zhaowenke@corp.netease.com>\>

  Subject: Re: 建德集群ceph存储适配x710网卡风险

   

  \[remove other members\]

   

  hi， Cheysi：

   

    

  谢谢支持。  测试的话，@罗泽文也可以进一步支持。

   

   

   

  Thanks

  -WenKe

   

  发件人: \<[Cheysi.Yu@dell.com](mailto:Cheysi.Yu@dell.com)\>

  日期: 2018年2月5日 星期一 14:31

  至: \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>, \<[hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com)\>, \<[Richard.Lv@dell.com](mailto:Richard.Lv@dell.com)\>, \<[caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com)\>

  抄送: \<[ebs-dev@hz.netease.com](mailto:ebs-dev@hz.netease.com)\>, \<[hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com)\>, \<[hzganchenxi@corp.netease.com](mailto:hzganchenxi@corp.netease.com)\>, \<[chene@corp.netease.com](mailto:chene@corp.netease.com)\>, \<[hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com)\>, \<[Richard.Lv@dell.com](mailto:Richard.Lv@dell.com)\>, \<[zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com)\>, \<[huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)\>

  主题: RE: 建德集群ceph存储适配x710网卡风险

   

   

   

   

  赵工：

   

     收到，我会立即跟后台跟进，后续与黄浩然这边联系，有新的进展立即更新给各位，谢谢。

   

   

   

  Best Regards

  Cheysi Yu

  TSM

   

   

  From: 赵庆武 \[[mailto:hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)[\] ]

  Sent: 2018年2月5日 14:13

  To: 黄俐 \<[hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com)\>; Yu, Cheysi \<[Cheysi_Yu@Dell.com](mailto:Cheysi_Yu@Dell.com)\>; Lv, Richard \<[Richard_Lv@DELL.com](mailto:Richard_Lv@DELL.com)\>; 曹黎炜 \<[caoliwei@corp.netease.com](mailto:caoliwei@corp.netease.com)\>

  Cc: ebs-dev \<[ebs-dev@hz.netease.com](mailto:ebs-dev@hz.netease.com)\>; hzluozewen \<[hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com)\>; 甘辰希 \<[hzganchenxi@corp.netease.com](mailto:hzganchenxi@corp.netease.com)\>; 陈 谔 \<[chene@corp.netease.com](mailto:chene@corp.netease.com)\>; hzhuangli \<[hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com)\>; zhaowenke \<[zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com)\>; [huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)

  Subject: Re: 建德集群ceph存储适配x710网卡风险

   

  [\@Cheysi](mailto:Cheysi.Yu@dell.com) [\@Richard](mailto:Richard.Lv@dell.com),    针对R730XD服务器上的戴尔OEM的X710网卡，我们出现如邮件所述问题，请给与问题排查和支持，相关配合请可联系@黄浩然；

   

  请@黄俐 @黎炜 知晓此事情，多谢！

   

  ---－－－－－－－－－－－－－－－－－－－－－－－－－

  赵庆武   

  网易杭州研究院运维部－系统运维组

  [邮箱：hzzhaoqingwu@corp.netease.com](mailto:%E9%82%AE%E7%AE%B1%EF%BC%9Ahzzhaoqingwu@corp.netease.com)

  手机：13456856568

  地址：杭州市滨江区网商路599号 310052  

   

  发件人: 赵文科 \<[zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com)\>

  日期: 2018年2月5日 星期一 13:29

  收件人: 赵庆武 \<[hzzhaoqingwu@corp.netease.com](mailto:hzzhaoqingwu@corp.netease.com)\>, \"[huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)\" \<[huanghaoran@corp.netease.com](mailto:huanghaoran@corp.netease.com)\>

  抄送: ebs-dev \<[ebs-dev@hz.netease.com](mailto:ebs-dev@hz.netease.com)\>, 赵文科 \<[zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com)\>, hzluozewen \<[hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com)\>, 甘辰希 \<[hzganchenxi@corp.netease.com](mailto:hzganchenxi@corp.netease.com)\>, 陈 谔 \<[chene@corp.netease.com](mailto:chene@corp.netease.com)\>, 黄俐 \<[hzhuangli@corp.netease.com](mailto:hzhuangli@corp.netease.com)\>

  主题: 建德集群ceph存储适配x710网卡风险

   

  @庆武：

   

    根据之前的沟通，以下问题是建德Ceph集群使用X710网卡适配测试中的一个严重问题。 影响：若此问题出现，会影响用户IO长达1分钟之久。

    (之前@罗泽文 与 @黄浩然 已经在调试和测试了几天，目前仍然无法解决这个问题。)

   

   

    请安排SA同学以高优先级来彻底调查下原因吧。或者请厂商来帮忙解决该问题。或者这个问题可以邀请Intel的人帮忙一起分析一下？

   

   

    预案：

  1.         如果能通过调整配置或者软件的方法，能减少这部分的风险，可以继续使用X710网卡。

   

  2.         如果不能解决问题，需要将X710网卡替换成之前的82599网卡。

  1）           如果确定存储机器不能使用X710网卡，需要将之前发出采购的DELL的机器的网卡也需要进行如上的调整。

   

   

   

  Thanks

  -WenKe

     

   

  发件人: hzluozewen \<[hzluozewen@corp.netease.com](mailto:hzluozewen@corp.netease.com)\>

  日期: 2018年2月5日 星期一 12:50

  至: zhaowenke \<[zhaowenke@corp.netease.com](mailto:zhaowenke@corp.netease.com)\>

  抄送: ebs-dev \<[ebs-dev@hz.netease.com](mailto:ebs-dev@hz.netease.com)\>

  主题: 建德使用x710网卡适配ceph存储风险汇报

   

   

  从整体的测试情况看，功能、性能、稳定性暂无问题 ([http://jira.netease.com/browse/CLDNBS-675](http://jira.netease.com/browse/CLDNBS-675) )。

   

  在网卡异常情况下，存在一定的风险：

  1.使用ifconfig down模拟eth0故障故障

  2.使用ip link set 模拟eth0故障

  以上2项测试均会导致ceph存储集群出现osd心跳异常，触发peering，导致用户io hang的情况；

  对于ceph集群，osd会出现up\-\--\>down\-\--\>up的状态切换，用户侧监测到io hang持续接近1分钟。

   

  修改后，ceph状态如下：

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_009.png]]

   

  用户IO受到peering影响，导致无法进行IO：

  ![[Technology_ALL_Linux 问题收集_034_相关bond配置反馈_010.png]]

   

   

  对比之前的部署方式，不存在上述情况。

   

  以上问题已经知会\@SA黄浩然排查，希望给出详细的结论

  [http://doc.hz.netease.com/pages/viewpage.action?pageId=102894227](http://doc.hz.netease.com/pages/viewpage.action?pageId=102894227)

   

  3.使用拔网线模拟网卡故障

  拔出eth0或者eth1的网线，对ceph集群无影响

   

  2018-02-05

  hzluozewen

 

已使用 OneNote 创建。
