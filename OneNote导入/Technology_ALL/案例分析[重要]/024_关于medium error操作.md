关于medium error操作

2015年1月30日

8:37

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534
  发件人     He, Mars
  收件人     Li, Jiangxiong; zhang1, Jackey
  抄送       Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME
  发送时间   2015年1月29日 17:32
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell Customer Communication

Done

已经联系了客户，带着客户在liveCD的OMSA中清除了bad block， 目前VD状态显示正常了；

 

From: Li, Jiangxiong

Sent: 2015年1月29日 16:34

To: He, Mars; zhang1, Jackey

Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

Subject: 答复: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Mars

尽快联系一下客户，TAM反馈客户安装OMSA有一些报错

看看客户是否可以通过livecd去操作，或者是开一个case给mouse看一下缺少什么组件

 

Li Jiangxiong

Enterprise Product Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

发件人: He, Mars 

发送时间: 2015年1月29日 16:29

收件人: zhang1, Jackey

抄送: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; Li, Jiangxiong

主题: RE: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Dell Customer Communication

Hi Jackey;

                请问后续要如何操作？ 

 

From: Li, Jiangxiong

Sent: 2015年1月28日 15:28

To: zhang1, Jackey; He, Mars

Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

Subject: 答复: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Hi Jackey

我通过日志分析，看到硬盘有medium error，导致VD bad block，OS在对VD进行读写的时候就会报错，需要清掉bad block

需要安装OMSA

可以通过OMSA web来清除

![[Technology_ALL_案例分析[重要]_024_关于medium error操作_001.png]]

或者通过OMSA命令清除

omconfig storage vdisk action=clearvdbadblocks controller=id vdisk=id

 

另外客户的固件版本比较低，建议升级到最新，最新的固件已经解决了我们发现的一些已知的issue，客户可能会遇到，升级方法可以下载SAS-RAID_Firmware_6P6KN_WN64_21.3.0-0009_A05.EXE通过idrac web界面升级

[http://www.dell.com/support/home/cn/zh/cnbsd1/Drivers/DriversDetails?driverId=6P6KN](http://www.dell.com/support/home/cn/zh/cnbsd1/Drivers/DriversDetails?driverId=6P6KN)

PERC H710P Mini Monolithic Adapter firmware release 21.3.0-0009

Dell PERC H710P Mini Monolithic Adapter firmware release 21.3.0-0009

补丁和增强功能

Fixes:

- Fixed an issue where an OS could change cache setting on a Virtual Drive unexpectedly.

- Fixed an issue where running a SATA hard drive dup on a hard drive larger than 2TB might cause the hard drive to come off line. 

- Fixed an issue where the design capacity PERC battery was shown incorrectly in the TTY log. Cosmetic only.

- Fixed an issue where the controller falsely reports that the battery charger has stopped working due to high temperature.

- Corrected an issue where the hard drives were polled too often.

- Fixed an issue where the TTY logs were being cleared after an OS shutdown.

- Corrects an issue where you can name a virtual disk with reserved characters in F2 BIOS mode.

- Corrects a couple of issues where drives could inadvertently be taken off line.

- Corrects an issue where the controller could hang after importing a Foreign Config when there was stale cache.

 

Enhancements:

- Adds the capability for the user to limit the SATA negotiated speed to 3G for direct-attached drives.

- Allows segmented downloads (i.e. Mode 7) to flash SATA hard drives if supported.

 

 

 

Li Jiangxiong

Enterprise Product Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

发件人: zhang1, Jackey 

发送时间: 2015年1月28日 14:22

收件人: He, Mars; Li, Jiangxiong

抄送: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

主题: RE: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Hi JiangXiong，

 

客户的公司名称是什么？

车之家，因为前期DELL在客户处的很多重大问题，现在生意正在慢慢恢复中，请务必重视。

 

多少台机器有这样的问题？把有问题的日志全部都收集过来。

把所有机器日志都收集过来，我们是否换种方式（查询机器出厂信息），如果我们是客户的，厂商这样的要求会使客户直接拒绝的。

 

 

另外建议客户通过OMSA去把VD bad block clear

客户现在没有安装OMSA，你的意思是让客户在所有机器上安装OMSA?清除掉坏块后，故障就解决了吗？

 

建议客户升级raid卡固件和硬盘固件到最新，考虑更换raid级别

升级固件的话我们要有证据去说服客户，考虑更换raid级别是很不合理的要求。

 

如果方便的话，我们电话沟通一下，谢谢！现在车之家正在准备一个10M的投标，所有的sales及sales老板都非常紧张，请重视，谢谢！

 

====================================

Jackey Zhang (张麟)

 

Technical Account Manager

Dell \| Global Support Service

Office: +86 10 58261221 ; Mobile: +86 18601378198 

How am I doing? Email my manager at <Xin_Zheng@dell.com>

 

 

 

 

From: He, Mars

Sent: Wednesday, January 28, 2015 2:02 PM

To: Li, Jiangxiong; zhang1, Jackey

Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

Subject: RE: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Dell Customer Communication

Hi Jiangxiong;

                这个case是上午TAM Jackey反馈过来的，TAM要求要先和他沟通清楚在联系客户；

                客户：车之家

                根据TAM Jackey的确认，客户由于应用要求高性能，必须采用RAID0;

                具体有多少台还在等TAM确认；

 

Hi Jackey;

                请问目前有多少台受影响呢？ 能否帮忙将所有受影响的机器的日志全收集过来呢？

 

From: Li, Jiangxiong

Sent: 2015年1月28日 13:55

To: He, Mars

Cc: zhang1, Jackey; Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

Subject: 答复: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Mars

客户的公司名称是什么？

多少台机器有这样的问题？把有问题的日志全部都收集过来

另外建议客户通过OMSA去把VD bad block clear

建议客户升级raid卡固件和硬盘固件到最新，考虑更换raid级别

 

 

Li Jiangxiong

Enterprise Product Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

发件人: Li, Jiangxiong 

发送时间: 2015年1月28日 13:44

收件人: He, Mars

抄送: zhang1, Jackey; Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

主题: 答复: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Mars

I will work on this case

 

Li Jiangxiong

Enterprise Product Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

发件人: He, Mars 

发送时间: 2015年1月28日 13:42

收件人: CN XMN TS Server Escalation

抄送: He, Mars; zhang1, Jackey

主题: R720XD\|HD issue\|PSP\|HDD Binding\| SR：906567534

 

Dell Customer Communication

Dear L2;

DELTA里发送邮件，收不到，我手工发送下；

以下case需要您的协助；

•  Detail Symptom Descriptions:

CENT OS 多块盘报IO 错误；RAID 0 per HDD;

• Troubleshooting Setups: 

check TTY:PD 0/4/5/7/8 report 3/11/01

heck part number:829T8   X12, 同批次5台，order:806081948

tell below info to TAM;

Toshiba 硬盘 (Harrier-2) 频繁出现Sense code 3/11/XX 导致的Predictive Failure . ( SR: 902380077  902129807 )

最近遇到二例关于客户抱怨东芝硬盘频繁出现Media Error 导致的故障。

受硬盘的硬盘型号：MG03SCA100, MG03SCA200, MG03SCA300 and MG03SCA400

对应硬盘的DELL备件号：

        D3YV6  HD,1T,ES,7.2K,3.5,T-HR2,E/C

       829T8  HD,2T,NL6,7.2K,3.5,T-HR2,E/C

        14X4H HD,3T,NL6,7.2K,3.5,T-HR2,E/C

        12GYY   HD,4T,NL6,7.2K,3.5,T-HR2,E/C

问题分析：两个客户的使用方式有相同点,都是单个硬盘配置RAID 0 . 由于Raid 0本身没有冗余校验，当硬盘的uncorrectable medium error

达到一定的阀值，对应的硬盘就会出现Predictive failure . 同时我们通过PG TEAM对故障硬盘做分析，当前的结论是FIX ON FAILURE .

后续建议：

a.)   升级硬盘的固件到目前最新DG04 的版本。(PS. 这个版本没有增强Sense code 3/11/xx 的问题)

b.)   建议客户配置带有冗余的阵列级别 如Raid 5 ,Raid 6 等。

KB:QNA43219 MD3 Array and PE servers - Errors on HARRIER-2 NL Toshiba 3TB MG03SCA300 (PN 14X4H) and 4TB MG03SCA400 (PN 12GYY) drives

• Current status:批量性问题, ESC TO L2.

• Must Collect Logs:TTY LOG 已经上传到delta.

 

 

Mars He

何  伟

Enterprise Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/) 

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \|一款软件插件，可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣零部件

回复邮件获取详细资料或点击 SupportAssist超链接了解更多信息！！

 

 

 

已使用 OneNote 创建。
