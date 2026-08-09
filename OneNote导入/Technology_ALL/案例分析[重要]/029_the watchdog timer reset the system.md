the watchdog timer reset the system

2015年4月9日

9:35

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       推荐: R815\|unstable issue\|email\|pros\|SA\| 4K6TG32\| 908914503
  发件人     Wang, Andy King
  收件人     Zhang, Eileen
  抄送       CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   2015年4月9日 9:22
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Eileen，

Ipmi服务bug导致，卸载不需要使用的ipmi服务，问题解决。

 

Need improve:

针对不稳定问题，特别是有一些线索的case，要结合hardware日志，系统日志来分析判断，不要就是更换硬件，再更换硬件，然后就升级。

 

Case study: 

首先，百度下什么是watchdog：

watchdog是Linux看门狗。[\[1\]] 

Linux 自带了一个 watchdog 的实现，用于监视系统的运行，包括一个内核 watchdog module 和一个用户空间的 watchdog 程序。[\[1\]] 

内核 watchdog 模块通过 /dev/watchdog 这个字符设备与用户空间通信。用户空间程序一旦打开 /dev/watchdog 设备，就会导致在内核中启动一个 1分钟的定时器，此后，用户空间程序需要保证在 1分钟之内向这个设备写入数据，每次写操作会导致重新设定定时器。如果用户空间程序在 1分钟之内没有写操作，定时器到期会导致一次系统 reboot 操作。[\[1\]] 

[看门狗](http://baike.baidu.com/view/280158.htm),又叫 watchdog timer,是一个[定时器](http://baike.baidu.com/view/281961.htm)电路, 一般有一个输入,叫[喂狗](http://baike.baidu.com/view/839305.htm),一个输出到MCU的RST端,MCU正常工作的时候,每隔一段时间输出一个信号到喂狗端,给 WDT 清零,如果超过规定时间不喂狗(一般在程序跑飞时),WDT 定时超过,就会给出一个[复位信号](http://baike.baidu.com/view/4507848.htm)到MCU,使MCU复位. 防止MCU死机. 看门狗的作用就是防止程序发生死循环或者说程序跑飞。

 

了解了什么是watchdog后，你就可以明白我们在hardware log中看到the watchdog timer reset the system 并不代表就是硬件问题。

对比TTY log，系统message log，会发现每次重启之前都会有报错信息：

jysh3acs1 /usr/sbin/bmc-watchdog\[3478\]: fiid_obj_get: \'present_countdown_value\': data not available

再次百度一下就可以找到：

[http://swq499809608.blog.51cto.com/797714/1571544/](http://swq499809608.blog.51cto.com/797714/1571544/)

yum -y erase freeipmi OpenIPMI  ipmitool

 

 

Case Closed 邮件保存路径：

<http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

 

B.R

Andy King Wang

 

 

 

From: Li, Jiangxiong

Sent: Friday, April 3, 2015 1:52 PM

To: Wang, Andy King

Cc: Fan, Pei; Zhang, Eileen; Lin, Wenjie; CN XMN TS ENT L2 SME

Subject: 答复: R815\|unstable issue\|email\|pros\|SA\| 4K6TG32\| 908914503

 

Dell - Internal Use - Confidential 

Andy

Please help on this case

 

Li Jiangxiong

Enterprise Product Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

发件人: Zhang, Eileen 

发送时间: 2015年4月3日 13:44

收件人: CN XMN TS Server Escalation

抄送: Fan, Pei

主题: R815\|unstable issue\|email\|pros\|SA\| 4K6TG32\| 908914503

 

Dell - Internal Use - Confidential 

•               Detail Symptom Descriptions

详细的故障现象描述:

 

客户的机器在装完操作系统后，经常自动重启，一天平均有4，5次。通过drac查看到记录 the watchdog timer reset the system.此记录的时间与系统重启的时间一致。

 

故障问题发生时间段:

 

•             Troubleshooting Steps

详细的诊断步骤:

第一次安排服务单，更换了主板，升级BIOS， drac， disable cstate,C1E情况依旧。

确认在bios里watchdog的功能是关闭的。

第二次安排服务单上门，DSP收集了DSET日志

1，上传日志，DSET依旧只显示大量的错误，没有报错。

Fri Apr 03 10:54:18 2015 The watchdog timer reset the system. 

  Thu Apr 02 12:38:54 2015 The watchdog timer reset the system. 

  Thu Apr 02 07:34:53 2015 The watchdog timer reset the system. 

2，BIOS ver:3.2.1 ;IDRAC ver:1.95.00,没有派idrac卡，建议DSP先升级BIOS,IDRAC,USC。

咨询过值班L2后建议DSP：

1，刷完BIOS,IDRAC,USC后，关闭CSTATE,C1E ；

2，用LIVECD进入OMSA，在下图位置可以关掉watchdog的功能。

3，32位压力测试跑几遍看是否会报错或死机。

维修记录: (单号/更换的部件/更换后的现象)

 

Bios/Driver/FW及存储控制器相关FW版本:

 

•             Current status

客户公司名称:DATANG CATTSOFT CO LTD

业务影响:客户服务器已经上线，目前的情况已经影响业务。

升级的原因:

RM/TAM:

不稳定的问题，已经安排了两次服务单。

昨天晚上DSP上门咨询了值班L2.

晚班TS建议升级L2再帮忙看看日志。

•             Must Collect Logs

已收集的日志(请上传至SR下):

 

已经收集DSET日志及sosreport如附件。

 

 

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
| 张琳琳                                                                                                                                                                                                                                                                                        | Eileen Zhang                                                                    |
|                                                                                                                                                                                                                                                                                                                           |                                                                                                             |
| 企业级产品工程师                                                                                                                                                                                                                                                                                                          | Enterprise Product Engineer                                                                                 |
|                                                                                                                                                                                                                                                                                                                           |                                                                                                             |
| [戴尔]\| 企业级技术支持                                                                                                                                                                      |                                                                                                             |
|                                                                                                                                                                                                                                                                                                                           |                                                                                                             |
|                                                                                                                                                                                                                                                                                                                           | Customer feedback \| How am I doing? Please contact my manager <ray_wong@dell.com> |
|                                                                                                                                                                                                                                                                                                                           |                                                                                                             |
| 客户反馈\| 我表现如何？请联系我的经理 [ray_wong@dell.com](mailto:ray_wong@dell.com) |                                                                                                             |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+

 

[Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \| 一款软件插件, 可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣部件

回复邮件获取详细资料或点击[Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx)超链接了解更多信息！

戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [![[Technology_ALL_案例分析[重要]_029_the watchdog timer reset the system_001.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[Technology_ALL_案例分析[重要]_029_the watchdog timer reset the system_002.gif]]](http://www.weibo.com/techsupportdell)   [![[Technology_ALL_案例分析[重要]_029_the watchdog timer reset the system_003.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[Technology_ALL_案例分析[重要]_029_the watchdog timer reset the system_004.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

已使用 OneNote 创建。
