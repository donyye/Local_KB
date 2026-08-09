EXSI hang 住

2015年3月9日

10:50

-  

   

   

   

   

  ::: 
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 
    发件人     Yin, Guoxun
    收件人     Liu, Lester
    抄送       Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong
    发送时间   2015年3月9日 10:43
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Lester

   

  从日志看，在ESXi Hang住前，本地存储有暂时离线，Hostd处于无响应状态，初步判断此为主机Hang的主要原因。

   

  2015-03-01T15:25:57.248Z cpu2:34897 opID=581e7721)WARNING: J3: 1752: Failed to reserve space for journal on 54355e20-c23f21cf-fbf8-b82a72dcc91c : Timeout

  2015-03-01T15:26:06.298Z cpu2:34897 opID=581e7721)WARNING: HBX: 2363: Failed to cleanup VMFS heartbeat on volume54355e20-c23f21cf-fbf8-b82a72dcc91c: No connection

  2015-03-01T20:06:08.217Z cpu2:34897 opID=581e7721)WARNING: HBX: 2363: Failed to cleanup VMFS heartbeat on volume54355e20-c23f21cf-fbf8-b82a72dcc91c: No connection

   

   

  介于ESXi主机是出于Hang状态，且日志并无其他明显错误信息，我建议务必执行以下步骤，请反馈每一步是否执行和解过。

   

  1.  收集TTY日志一份
  2.  将BIOS/IDRAC升级到最新，并确保H730PFW不低于25.2.0.0019
  3.  更新附件中的H730P的驱动，方法如下：

  上传附件驱动至ESXi主机的/tmp目录下，运行以下命令并观察返回的命令提示是否已经安装成功

  esxcli software vib install --d /tmp/ megaraid_perc9\_-6.901.57.00-offline_bundle-2501818.zip

  1.  在BIOS中开启NMI开关。电源管理设置为Maximum performance, 确认C1E/CSTATS已经关闭
  2.  配置IDRAC IP，以备下次问题出现时候远程检查主机状态

   

  另外此CASE需要后续观察，请告知用户我们会后续提供服务直到确认问题解决。

  请提醒用户下次如果仍然出现hang住的情况，马上通知我们，需要通过webex检查ESXi状态，并且抓取Dump，切不可自行重启或者做其他安排。

   

   

  另外友情提示用户务必设置ESXi主机名和vCenter managed name/IP，并且确保能够正常进行正反向解析，不然以后即使主机没有死机也会出现vCenter中主机离线无法管理的问题。

  相关设置位置可参见以下图片。

   

  vCenter Managed IP/hostname

  ![[Technology_ALL_VMware_分析案例_007_EXSI hang 住_001.jpg]]

   

   

  ESXi主机名

  ![[Technology_ALL_VMware_分析案例_007_EXSI hang 住_002.jpg]]

   

   

   

  From: Liu, Lester

  Sent: 2015年3月6日 14:55

  To: Yin, Guoxun

  Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

  Subject: 答复: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  Guoxun

     用户描述最近一次宕机出现在3月2日，请您着重看一下，谢谢！

   

  发件人: Yin, Guoxun 

  发送时间: 2015年3月6日 14:44

  收件人: Liu, Lester

  抄送: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

  主题: RE: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  OK， 所有情形已澄清，检查完日志后再说。

   

  From: Liu, Lester

  Sent: 2015年3月6日 14:41

  To: Yin, Guoxun

  Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

  Subject: 答复: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  用户表示故障vcenter有响应，vcenter本身有响应，安装在用户另外一台R720上，r720是win2008R2系统，所有虚拟机都是在2台R920上，用户出现问题是可以ping通vmware的管理IP，但是所有虚拟机的ip无法ping通谢谢！

   

  发件人: Yin, Guoxun 

  发送时间: 2015年3月6日 14:31

  收件人: Liu, Lester

  抄送: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

  主题: RE: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  OK，在出现问题的时候，vCenter本身是否还有响应？ vCenter装在什么机器上？如果是虚拟机，是放在这两个R920上的吗？

   

  From: Liu, Lester

  Sent: 2015年3月6日 14:27

  To: Yin, Guoxun

  Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

  Subject: 答复: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  Dear guoxun

    是否有查看我提供的日志和报错截图，截图中有出现问题的时候vcenter的状态，谢谢!

  1.  vCenter无法管理即主机无响应，详细请参考截图：[\\\\dtxadmin.dell.com\\data\$\\upload_only\\15915714868](file://dtxadmin.dell.com/data$/upload_only/15915714868)
  2.  当出现问题的时候，问题主机的所有虚拟机都不能ping通，2台R920都出现过这个问题，其他的服务器没有此问题
  3.  虚拟机无响应时，ESXI主机是脱机状态，本地停留在vmware控制台上，键盘没有反应，没有紫屏的现象

   

  发件人: Yin, Guoxun 

  发送时间: 2015年3月6日 14:16

  收件人: Liu, Lester

  抄送: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

  主题: RE: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  Lester

  下面这句话表达有问题，有很多歧义。

  用户2台服务器在dell官网下载esxi5.5U1，安装使用一段时间后出现vCenter无法管理的问题，虚拟机无响应，用户现场查看服务器没有硬件报警

   

  请确认并详细回复下以下问题：

  1.  vCenter无法管理的问题是什么意思？ vCenter自身无法使用还是有其他情况？ 请描述清楚
  2.  虚拟机无法响应怎么理解，当时虚拟机处于什么状态，能否ping通？ 能否RDP?  所有的VM都这样还是就个别VM如此？
  3.  虚拟机无响应时，ESXI主机什么状态？本地有否显示？ 能否ping通？ 键盘有否反应？ 有否紫屏？

   

   

  From: Li, Jiangxiong

  Sent: 2015年3月6日 13:01

  To: Yin, Guoxun

  Cc: Liu, Lester; Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

  Subject: 答复: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  Dell - Internal Use - Confidential 

  Guoxun

  Please help on this case

   

  Li Jiangxiong

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

  中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

  DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

  戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

   

  发件人: Liu, Lester 

  发送时间: 2015年3月6日 12:46

  收件人: CN XMN TS Server Escalation

  抄送: Liu, Lester

  主题: R720\|OS issue\|(PROS)\| ST:7TLXZ22 SR:908203147 

   

  Dell - Internal Use - Confidential 

  1.Detail Symptom Descriptions-详细的故障现象描述：

  用户2台服务器在dell官网下载esxi5.5U1，安装使用一段时间后出现vCenter无法管理的问题，虚拟机无响应，用户现场查看服务器没有硬件报警，显示器停在vmware管理界面，鼠标键盘无法操作，用户强行重启后正常，用户机器不定时出现此问题

  2.Troubleshooting Setups-排错步骤：

  建议用户收集dset和vm support日志

  3.Current status-当前状态:

  分析dset日志没有发现硬件的问题，建议用户升级bios和drac到最新，升级L2帮忙分析系统日志

  4.Must Collect Logs-必须收集的日志 ;

  dset and vm support log

  [\\\\dtxadmin.dell.com\\data\$\\upload_only\\15915714868](file://dtxadmin.dell.com/data$/upload_only/15915714868)

   

  Liu, Lester

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

  How am I doing? Email my manager [Richa_zeng@dell.com](mailto:Richa_zeng@dell.com) with any feedback

  [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \|一款软件插件，可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣零部件

  回复邮件获取详细资料或点击 SupportAssist超链接了解更多信息！！

  戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

  ::: 
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    [![[Technology_ALL_VMware_分析案例_007_EXSI hang 住_003.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[Technology_ALL_VMware_分析案例_007_EXSI hang 住_004.gif]]](http://www.weibo.com/techsupportdell)   [![[Technology_ALL_VMware_分析案例_007_EXSI hang 住_005.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[Technology_ALL_VMware_分析案例_007_EXSI hang 住_006.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

   

 

已使用 OneNote 创建。
