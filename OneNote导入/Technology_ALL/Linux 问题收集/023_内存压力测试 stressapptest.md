内存压力测试 stressapptest

Monday, October 17, 2016

9:27 AM

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       答复:[  ]来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）
  发件人     Ye, Dony
  收件人     Chen9, Jack
  发送时间   Monday, October 17, 2016 9:00 AM
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Stressapptest 下载地址

<https://github.com/stressapptest/stressapptest>

 

 

解决方法：938524909

需要BIOS开启Node Interleaving（节点交叉存取），默认是disable的。

 

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_001.jpg]]

 

 

Node[  Interleaving ]子项选择里面

 

[          Enabled]：表示smp方式启用内存交错模式，smp的方式

 

[          Disabled]：表示启用NUMA，非一致访问方式访问​

 

 

Dell - Internal Use - Confidential 

 

你是说BIOS修改关闭CPU节能那些吗？

 

B R

Dony

 

发件人: Chen9, Jack 

发送时间: Saturday, October 15, 2016 11:54 AM

收件人: Ye, Dony \<dony_ye@Dell.com\>

主题: RE: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

Hi，

 

我测试好了，BIOS需要修改一下，这个值就会增加，方法已经发给客户了，客户HP的服务器CPU和内存都和DELL一样的，谢谢您

 

Jack

2016-10-15

 

From: Ye, Dony

Sent: Friday, October 14, 2016 5:36 PM

To: Chen9, Jack \<<Jack_Chen9@Dell.com>\>

Subject: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

 

Hi，Jack

 

这是我一台R820的测试结果，一共是 256GB的内存，插了32条。

内存的部分信息：

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_002.jpg]]

 

 

使用客户的命令测试结果：

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_003.jpg]]

另外我发现客户的内存总共只有64G，这是否有影响？HP那台机器的测试也是64G吗？

你那边的测试结果是怎样？

 

B R

Dony

 

发件人: Chen9, Jack 

发送时间: Thursday, October 13, 2016 3:38 PM

收件人: Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>

主题: FW: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

 

 

From: 刘亮 \[[mailto:dl.liu@qunar.com](mailto:dl.liu@qunar.com)[\] ]

Sent: Wednesday, October 12, 2016 2:43 PM

To: Liu, Lyndon \<<Lyndon_Liu@Dell.com>\>

Cc: [1319152662@qq.com](mailto:1319152662@qq.com); 朱巍 \<[wei.zhu@qunar.com](mailto:wei.zhu@qunar.com)\>; [lih@langckx.com](mailto:lih@langckx.com); [hem@langckx.com](mailto:hem@langckx.com); Quan, Terry \<[Terry_Quan@DELL.com](mailto:Terry_Quan@DELL.com)\>; Zhang1, Jingjing \<[Jingjing_Zhang1@DELL.com](mailto:Jingjing_Zhang1@DELL.com)\>; 高一楠 \<[yinan.gao@qunar.com](mailto:yinan.gao@qunar.com)\>

Subject: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

HI, lyndon:

 

HP对比机型是ProLiant DL360 Gen9，

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_004.jpg]]

 

Dell VS. HP cpu/内存型号信息见附件dell_dmidecode_cpu/memory.txt，hp_dmidecode_cpu/memory.txt

 

内存压力工具是stressapptest，开源的，地址[https://github.com/stressapptest/stressapptest](https://github.com/stressapptest/stressapptest)

 

Dell机器用stressapptest压测内存结果如下图：

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_005.jpg]]

 

Hp机器已经都上线了，没法截图压测数据了，以前记录的结果是41000，数据如下：

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_006.jpg]]

 

请结合上述信息分析下dell和hp性能差距如此之大的原因。

 

发件人: [Lyndon.Liu@dell.com](mailto:Lyndon.Liu@dell.com) \[[mailto:Lyndon.Liu@dell.com](mailto:Lyndon.Liu@dell.com)[\] ]

发送时间: 2016年10月12日 14:26

收件人: 刘亮

抄送: [1319152662@qq.com](mailto:1319152662@qq.com)

主题: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

您好 刘先生

 

麻烦您提供下HP服务器的具体机型及参数，比如CPU 内存等信息，另外提供下dell机器的日志，

 

及机器压力测试的对应截图，我们好做对比

 

进行分析。

 

感谢！

 

 

发件人: 1319152662 \[[mailto:1319152662@qq.com](mailto:1319152662@qq.com)[\] ]

发送时间: 2016年10月12日 14:02

收件人: Liu, Lyndon \<[Lyndon_Liu@Dell.com](mailto:Lyndon_Liu@Dell.com)\>

主题: 回复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

现在可以电话沟通了。18611088983，打这个电话。

以下为内存订单号，808501401如有问题，随时电话沟通。

 

 

 

1319152662

 

发件人： [Lyndon.Liu@dell.com](mailto:Lyndon.Liu@dell.com)

发送时间： 2016-10-12 12:46

收件人： [1319152662@qq.com](mailto:1319152662@qq.com)

主题： 答复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

Dell - Internal Use - Confidential 

您好

 

我已上班，如果您需要我联系的时候，请告知我下。

 

谢谢！

 

发件人: Liu, Lyndon 

发送时间: 2016年10月11日 17:59

收件人: \'1319152662\' \<[1319152662@qq.com](mailto:1319152662@qq.com)\>

主题: 答复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

您好

 

刚好我明天是13点上班，方便的话明天下午沟通下

 

发件人: 1319152662 \[[mailto:1319152662@qq.com](mailto:1319152662@qq.com)[\] ]

发送时间: 2016年10月11日 17:57

收件人: Liu, Lyndon \<[Lyndon_Liu@Dell.com](mailto:Lyndon_Liu@Dell.com)\>

主题: 回复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

刘工，

    相关的问题还是没有解决，明天（10.12）有时间，和客户一起再聊下这个问题！！

 

 

1319152662

 

发件人： [Lyndon.Liu@dell.com](mailto:Lyndon.Liu@dell.com)

发送时间： 2016-10-10 13:23

收件人： [1319152662@qq.com](mailto:1319152662@qq.com)

主题： 答复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

Dell - Internal Use - Confidential 

 

Intel(R) Xeon(R) CPU E5-2620 v3 @ 2.40GHz 的处理器支持的内存是1866，所以目前您2133的内存主频其实是工作在1866下。如第四张图片

 

E52620V3

[http://ark.intel.com/zh-cn/products/83352/Intel-Xeon-Processor-E5-2620-v3-15M-Cache-2_40-GHz](http://ark.intel.com/zh-cn/products/83352/Intel-Xeon-Processor-E5-2620-v3-15M-Cache-2_40-GHz)

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_007.jpg]]

 

 

E52620V4

[http://ark.intel.com/zh-cn/products/92986/Intel-Xeon-Processor-E5-2620-v4-20M-Cache-2_10-GHz](http://ark.intel.com/zh-cn/products/92986/Intel-Xeon-Processor-E5-2620-v4-20M-Cache-2_10-GHz)

 

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_008.jpg]]

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_009.jpg]]

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_010.jpg]]

 

 

发件人: Liu, Lyndon 

发送时间: 2016年10月10日 13:15

收件人: \'1319152662\' \<[1319152662@qq.com](mailto:1319152662@qq.com)\>

主题: 答复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

您好

 

日志分析如下

 

BIOS目前已经是最新的2.2.5版本，不过远程管理器IDRAC相对版本较低，是2.20.20，建议更新到最新的2.30.30.30.

 

F10固件更新方法

[http://www.dell.com/support/article/cn/zh/19/SLN289499](http://www.dell.com/support/article/cn/zh/19/SLN289499)

 

IDRAC固件

[http://downloads.dell.com/FOLDER03526203M/2/iDRAC-with-Lifecycle-Controller_Firmware_5GCHC_WN64_2.30.30.30_A00.EXE](http://downloads.dell.com/FOLDER03526203M/2/iDRAC-with-Lifecycle-Controller_Firmware_5GCHC_WN64_2.30.30.30_A00.EXE)

 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_011.jpg]]

 

发件人: 1319152662 \[[mailto:1319152662@qq.com](mailto:1319152662@qq.com)[\] ]

发送时间: 2016年10月10日 13:01

收件人: Liu, Lyndon \<[Lyndon_Liu@Dell.com](mailto:Lyndon_Liu@Dell.com)\>

主题: 回复: 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

刘工，附件为服务器的日志，请查收！

 

 

1319152662

 

发件人： [Lyndon.Liu@dell.com](mailto:Lyndon.Liu@dell.com)

发送时间： 2016-10-10 12:54

收件人： [1319152662@qq.com](mailto:1319152662@qq.com)

主题： 答复: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

Dell - Internal Use - Confidential 

R630 BIOS

 

[http://downloads.dell.com/FOLDER03917201M/1/BIOS_PFWCY_WN64_2.2.5.EXE](http://downloads.dell.com/FOLDER03917201M/1/BIOS_PFWCY_WN64_2.2.5.EXE)

 

Enhancements:

\- Enhanced the Correctable ECC memory error detection and logging feature. (2.1.6 and forward)

\- Updated the Intel Processor and Memory Reference Code to PLR4.

\- Updated the Intel Xeon Processor E5-2600 v4 Product Family Processor Microcode to version 0x1E.

\- Updated the Intel Xeon Processor E5-2600 v3 Product Family Processor Microcode to version 0x38.

\- Updated the Intel Trusted Execution Technology (Intel TXT) BIOS and SINIT Authenticated Code Module (ACM) to version 3.1.0.

\- The Intel TXT feature is supported with Trusted Platform Module (TPM) version 2.0.

\- Updated TPM version 2.0 support.

\- Updated the integrated Dell Remote Access Controller (iDRAC) Human Interface Infrastructure (HII) to version 2.40.40.05.

\- Added the new slot bifurcation BIOS Setup options.

\- Updated text in the BIOS Setup menu help content.

\- Changed the default setting of BIOS Setup option In-System-Characterization to Disabled.

 

Fixes:

\- The boot order may change after updating the BIOS version.

\- The cause of system internal error (IERR) is not getting logged.

 

 

 

发件人: Liu, Lyndon 

发送时间: 2016年10月10日 12:51

收件人: \'1319152662@qq.com\' \<[1319152662@qq.com](mailto:1319152662@qq.com)\>

主题: 来自戴尔技术支持工程师刘添锋 - (服务编号：466BTF2）

 

Dell - Internal Use - Confidential 

尊敬的Dell客户李先生: 

     您好! 我是之前处理您服务器问题的Dell工程师. 关于您的问题, 如果有任何后续方面需要我帮忙的地方, 请您直接回复我的邮件,我一定尽力帮您解决!

     真诚希望我们的服务能够令您满意, 祝您身体健康, 工作愉快!

v 我们的目标： 

                                                  

1.   用心帮助客户，努力一通电话尽快解决问题。

2.   用我们细致有效的服务，让客户花费较少的精力和付出努力的时间。

如果有任何您觉得很麻烦/困扰，多花比较多努力/精力或不好的体验，也还请Mail 回复给我，您的意见有助于改善我们的服务。

 

 

 

 

 

 

 

 

Lyndon_liu\|刘添锋

Enterprise Engineer

Enterprise Support Services

Dell \| Global Support and Deployment

如果您对我的服务有任何意见或建议,也可以联系我的经理   [Wei_Wang9@DELL.com](mailto:wei_wang9@dell.com) 

![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_012.jpg]]

[Dell TechDirect](http://techdirect.dell.com/)\|在线报修门户网站:提供在线报修,自助部件派单以及报修事件在线管理

使用快速服务代码, 直达售后团队。[获取及了解快速服务代码信息请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)

[![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_013.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Email/TagChange)

 

[![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_014.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Chat/NonConsumerForm?svcTag=8JQMKY1&segment=BSD&chatQueueName=CN.ZH.TS.ENT.CCC&chatQueueClient=UNIFIEDCLIENT&chatQueueMaxWaitTime=1000000&chatQueueUnifiedChatClientTemp=Dell&chatQueueLanguageId=18&productModel=PModel&IsOLIMTag=False)

 

[![[Technology_ALL_Linux 问题收集_023_内存压力测试 stressapptest_015.jpg]]](http://www.dell.com/support/article/cn/zh/19/SLN298781)

 

 

 

安全提示：本邮件非QUNAR内部邮件，请注意保护个人及公司信息安全，如有索取帐号密码等可疑情况请向 secteam发送邮件

 

已使用 OneNote 创建。
