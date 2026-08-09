驱动或固件导致R920在ESXI紫屏issue

2015年3月11日

13:32

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       答复: CASE CLOSED 9LHRH42-SR907097352-R920-System unstable issue
    发件人     Li, Jiangxiong
    收件人     W, Robin; CN XMN TS ENT L2 SME
    发送时间   2015年3月11日 13:07
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Robin

  你再跟ips测试看看，目前客户2台机器都已经降级固件解决了，当然，现在又出现了新的固件，以后可以升级最新固件了

   

  Li Jiangxiong

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

  中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

  DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

  戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

   

  发件人: W, Robin 

  发送时间: 2015年3月11日 11:31

  收件人: CN XMN TS ENT L2 SME

  主题: RE: CASE CLOSED 9LHRH42-SR907097352-R920-System unstable issue

   

  Dell - Internal Use - Confidential 

   

  R920阵列卡固件的事情IPS有找PG确认，PG的答复是最新的固件可以用在R920上，列表里面之所以没有在发布的时候忘了把920加上去，R920单独的一个固件版本是因为当时是还没有13G的机器发布，所以旧版本里面只有R920。之前我处理了2个R920的esxi紫屏的case，一个是降级了固件升级阵列卡驱动，另外一个是只降级了固件，问题都解决了。

  年前的时候我用LAB里面的R920做了些测试，阵列用最新的固件，使用OEM的esxi5.5U2，上面创建了多个windows和linux虚拟机，在虚拟机上跑压力软件，压了3天也没出问题（之前客户遇到的是每天都会紫屏4\-\--5次）。

  如果是esxi的系统最好确认一下阵列卡的驱动模块，有些客户说是安装的OEM的实际上不是，只要阵列卡驱动模块对是不是OEM的都无所谓了。正确的驱动模块是：scsi-megaraid-perc9，默认的是：lsi-mr3

   

   

  Thanks

  Robin

   

  From: Li, Jiangxiong

  Sent: 2015年3月11日 11:11

  To: Xu, Hanson; Zhou, Meng

  Cc: CN XMN TS ENT L2 SME; CN XMN GSD TS ESG MGMT; CN XMN TS ENT L2 Coach; Wang, Xing Fang

  Subject: 推荐: CASE CLOSED 9LHRH42-SR907097352-R920-System unstable issue

   

  Dell - Internal Use - Confidential 

  Hi Hanson

  The case had been closed

  Solution：downgrade raid card FW fixed issue

   

  故障现象：客户反馈R920的机器，安装OEM的ESXi5.5的系统，机器出现不稳定宕机

  hardware报

  A bus fatal error was detected on a compent at bus 0 device3 function2.

  A bus fatal error was detected on a compent at bus 3 device0 function0.

   

  处理过程：

  1.  分析报错的地址指向raid卡
  2.  L1安排更换了raid卡无法解决，还是同样问题，升级给L2
  3.  L2通过日志分析，raid卡的FW是13G上的FW 25.2.1.0037（之前没有在R920的列表内）,在R920上的raid卡FW 25.2.0.0019，建议客户降低FW
  4.  降低FW 25.2.0.0019后观察2周解决问题

   

  总结：R920使用ESXi5需要升级bios 1.3.2以上，raid卡的FW 25.2.1.0037不适合R920，2015年3月10日出的最新固件25.2.2.-0004已经出现在R920的列表内，如果遇到问题，可以更新到此版本

   

  PERC H730/H730P/H830 Mini/Adapter RAID Controllers firmware version 25.2.2.-0004

  补丁和增强功能

  Fixes:

  \- Corrects issue with excessive PERC I/O timeouts and SATA SSDs falling offline under heavy I/O.

   

  Enhancements:

  \- Support for FD33xS and FD33xD controllers

  版本

  版本25.2.2-0004、A03

  发布日期

  10 三月 2015

  上次更新

  10 三月 2015

   

  Case Closed 邮件保存路径：

  [http://moss.ap.dell.com/sites/CCC%20PLE%20Enterprise%20L2%20Portal/L2%20Closed%20Case%20Summary/default.aspx](http://moss.ap.dell.com/sites/CCC%20PLE%20Enterprise%20L2%20Portal/L2%20Closed%20Case%20Summary/default.aspx)

   

   

  Li Jiangxiong

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

  中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

  DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

  戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

   

  发件人: Li, Jiangxiong 

  发送时间: 2015年2月13日 13:46

  收件人: Xu, Hanson

  抄送: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

  主题: 答复: R920\|System unstable issue\|PROS\| SR 907097352

   

  Dell - Internal Use - Confidential 

  Hanson

  I will work on this case

   

  Li Jiangxiong

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

  中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

  DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

  戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

   

  发件人: Xu, Hanson 

  发送时间: 2015年2月13日 11:39

  收件人: CN XMN TS Server Escalation

  主题: R920\|System unstable issue\|PROS\| SR 907097352

   

  Dell - Internal Use - Confidential 

  ·            Detail Symptom Descriptions

  详细的故障现象描述:

  A bus fatal error was detected on a compent at bus 0 device3 function2.

  A bus fatal error was detected on a compent at bus 3 device0 function0.

  故障问题发生时间段:

   

  更换阵列卡后新的报错：

  BIOS Manuufacturer: Dell Inc., BIOS Version: 1.3.2

  Model: PoweEdge R920, Serial Number: 9LHRH42. Tag:23.0 Asset Tag:unknown

  4 alerts: Processor 4 Status0: IERR, System Board 1 COMS Battery: Failed, IPMI SEL

  1 warning: Processor 3 VMSE PG0

  ·         Troubleshooting Steps

  详细的诊断步骤:DSET log 

   

  维修记录: (单号/更换的部件/更换后的现象)

  80901779751   perc card/

  BIOS Manuufacturer: Dell Inc., BIOS Version: 1.3.2

  Model: PoweEdge R920, Serial Number: 9LHRH42. Tag:23.0 Asset Tag:unknown

  4 alerts: Processor 4 Status0: IERR, System Board 1 COMS Battery: Failed, IPMI SEL

  1 warning: Processor 3 VMSE PG0

   

  Bios/Driver/FW及存储控制器相关FW版本:

  BIOS Version: 1.3.2

  PERC H730P Adapter   FW version 25.2.1.0037

  Idarc: 1.66.65 (Build 7)

   

  ·         Current status

  客户公司名称: 中山证券有限责任公司 信息技术部

  业务影响: 新机器

  升级的原因: R920 问题复杂无法判断

  RM/TAM: Zhang,Bowen

   

  ·         Must Collect Logs

  已收集的日志(请上传至SR下):

   

   

  Hanson Xu

  企业级产品工程师

  戴尔\|企业技术支持 

  我的表现如何? 请联系我的经理[:Meng_zhou@DELL.com](mailto:Meng_zhou@DELL.com)

  [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \| 一款软件插件, 可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣部件

  回复邮件获取详细资料或点击[Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx)超链接了解更多信息！

  戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

  ::: 
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    [![[Technology_ALL_VMware_分析案例_029_驱动或固件导致R920在ESXI紫屏issue_001.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[Technology_ALL_VMware_分析案例_029_驱动或固件导致R920在ESXI紫屏issue_002.gif]]](http://www.weibo.com/techsupportdell)   [![[Technology_ALL_VMware_分析案例_029_驱动或固件导致R920在ESXI紫屏issue_003.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[Technology_ALL_VMware_分析案例_029_驱动或固件导致R920在ESXI紫屏issue_004.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

   

 

已使用 OneNote 创建。
