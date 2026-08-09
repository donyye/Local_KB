RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

Monday, May 18, 2015

8:40 AM

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）
  发件人     W, Robin
  收件人     Lai, Flying
  抄送       Jiang, Yunguang; Ye, Dony
  发送时间   Friday, May 15, 2015 5:57 PM
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

这个比较简单，首先对比两台机器的负载，包括CPU/MEM/ IO等，然后判断是否有over load的情况，如果没有可能就是设置问题，刚好发现了CPU进入休眠状态，CPU休眠会导致延迟，所以就建议客户先把CPU休眠关了。

 

 

Thanks

Robin

 

From: Lai, Flying

Sent: 2015年5月15日 17:15

To: W, Robin

Cc: Jiang, Yunguang; Ye, Dony

Subject: RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

目前客户没有问题了，我已经将case close了。

谢谢。

能否麻烦您有空写下这个case的大概处理思路吗？

学习学习\~\~

 

From: W, Robin

Sent: Friday, May 15, 2015 4:45 PM

To: Lai, Flying

Cc: Jiang, Yunguang; Ye, Dony

Subject: RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

了解，看客户是否还有问题，没问题就关了吧。

 

 

 

Thanks

Robin

 

From: Lai, Flying

Sent: 2015年5月15日 16:43

To: W, Robin

Cc: Jiang, Yunguang; Ye, Dony

Subject: RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Robin

                刚刚回电客户，测试了将近一个月，根据客户的反馈目前都挺正常的。

                客户反馈目前所有改过两个修改的机器都正常，修改如下（如果只修改其中一项的话可能会达不到预期效果）

1.       tuned-adm profile latency-performance

2.       修改/boot/grub/grub.conf文件

反馈给您

谢谢提供帮助

 

From: 张文治 \[[mailto:zwz@fenbi.com](mailto:zwz@fenbi.com)[\] ]

Sent: Tuesday, April 21, 2015 6:24 PM

To: W, Robin; Jiang, Yunguang

Cc: Lai, Flying; 运维

Subject: Re:RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

今天成功的把cpu的频率调上去了，不过不是通过改grub和bios的方式，用邮件给出的方式改了重启后，cpu的频率仍然是省电模式。

 

调研了一下，用这个命令可以把cpu的频率提上去。

tuned-adm profile latency-performance

 

修改后的cpu信息如下：

cat /proc/cpuinfo   \| grep -i mhz

cpu MHz                     : 2601.000

cpu MHz                     : 2600.062

cpu MHz                     : 2600.718

cpu MHz                     : 2600.156

cpu MHz                     : 2598.843

cpu MHz                     : 2599.968

cpu MHz                     : 2599.406

cpu MHz                     : 2600.156

cpu MHz                     : 2599.781

cpu MHz                     : 2600.062

cpu MHz                     : 2600.156

cpu MHz                     : 2600.062

cpu MHz                     : 2599.968

cpu MHz                     : 2600.062

cpu MHz                     : 2599.500

cpu MHz                     : 2599.968

cpu MHz                     : 2599.968

cpu MHz                     : 2599.875

cpu MHz                     : 2599.968

cpu MHz                     : 2599.968

cpu MHz                     : 2599.968

cpu MHz                     : 2599.968

cpu MHz                     : 2599.968

cpu MHz                     : 2599.968

 

目前我这边先用老的参数跑程序。如果出现问题，我会在线跑一下tuned-adm profile latency-performance看看是不是马上可以改善。确定这个方法是不是有效，如果仍然有问题我会再跟大家继续沟通。

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"robin_w\"\<[robin_w@Dell.com](mailto:robin_w@Dell.com)\>;

Date:  Tue, Apr 21, 2015 08:55 AM

To:  \"zwz\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>; \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>; 

Cc:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Subject:  RE: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

张工你好，

Grub中添加这两个参数之后CPU的运行速度应该跟CPU的主频是一致的，如果不一致说明添加的参数没有生效。这个参数修改完成后需要重启操作系统才生效的，谢谢！！

 

 

 

Thanks

Robin

 

From: 张文治 \[[mailto:zwz@fenbi.com](mailto:zwz@fenbi.com)[\] ]

Sent: 2015年4月20日 20:15

To: W, Robin; Jiang, Yunguang

Cc: Lai, Flying

Subject: Re:RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

intel_idle.max_cstate=0 dle=poll和monitor/mwait修改之前没看到啥效果。

 

目前我把grub中的配置又加上了，继续观察一下，预期应该与这个关系不大。

 

在阵列性能有问题和没问题的情况下，cpu运行的速度都是1000+Mhz的速度。

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"robin_w\"\<[robin_w@Dell.com](mailto:robin_w@Dell.com)\>;

Date:  Mon, Apr 20, 2015 03:53 PM

To:  \"zwz\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>; \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>; 

Cc:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Subject:  RE: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

Hi 张工你好，

通过日志看当前系统是CentOS Linux release 7.1.1503 (Core)，CPU是E5-2620V3，CPU的主频是2.4GHz。但是当前运行的主频是1.5GHz，有些core甚至更低，这种现象是由于Linux中的intel idle driver导致的CPU休眠，这种情况下任务处于等待状态延迟很高，建议修改BIOS设置和Linux的grub.conf文件。

 

1，  修改BIOS设置：

S04这台机器的BIOS设置已经是最优状态，建议S03也按照如下的参数修改。

  --------------------------------------------------------------------------------- -----------------------------------------------------------------------
  System Profile                                        Custom
  CPU Power Management                                  Maximum Performance
  Memory Frequency                                      Maximum Performance
  Turbo Boost                                           Enabled
  Energy Efficient Turbo                                Disabled
  C1E                                                   Disabled
  C States                                              Disabled
  Collaborative CPU Performance Control                 Disabled
  Memory Patrol Scrub                                   Standard
  Memory Refresh Rate                                   1x
  Uncore Frequency                                      Maximum
  Energy Efficient Policy                               Performance
  Number of Turbo Boost Enabled Cores for Processor 1   All
  Number of Turbo Boost Enabled Cores for Processor 2   All
  Monitor/Mwait                                         Disabled
  --------------------------------------------------------------------------------- -----------------------------------------------------------------------

 

2，  修改/boot/grub/grub.conf文件

因为您使用的操作系统不是通过DELL购买，而是开源的CentOS，所以操作系统方面我们只提供友情支持，建议您在改动之前做好数据和设置的备份。

在/boot/grub/grub.conf中添加：intel_idle.max_cstate=0 dle=poll,以下是修改的模板供参考。

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_001.jpg]]

当前CPU运行的主频是1.5GHz

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_002.png]]

 

正常机器的CPU使用情况，CPU使用率很低，几乎没有任何等待。

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_003.jpg]]

 

异常机器的CPU使用情况，CPU使用率高于正常的机器，但是绝大部分时间等待状态

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_004.jpg]]

 

正常机器的IOPS在12000左右，相对比较平稳。

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_005.jpg]]

 

异常机器的IOPS跟正常机器差不多，但是浮动较大。

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_006.jpg]]

 

Thanks

Robin

 

From: 张文治 \[[mailto:zwz@fenbi.com](mailto:zwz@fenbi.com)[\] ]

Sent: 2015年4月20日 14:21

To: Jiang, Yunguang; W, Robin

Cc: Lai, Flying

Subject: Re:RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

已经全部上传了

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>;

Date:  Mon, Apr 20, 2015 01:30 PM

To:  \"zwz\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>; \"robin_w\"\<[robin_w@Dell.com](mailto:robin_w@Dell.com)\>; 

Cc:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Subject:  RE: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

 

 请通过这个[http://dtxdropbox.dell.com](http://dtxdropbox.dell.com) 上传文件  ,然后发邮件通知 [flying_lay@dell.com](mailto:flying_lay@dell.com) 和上级工程师 [robin_w@dell.com](mailto:robin_w@dell.com) 即可  

用户名和密码如下:

  -------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Login Name:    8yrvq42
  Password:      6ua21xj4
  Home Folder:   [\\\\dtxadmin.dell.com\\data\$\\upload_only\\8yrvq42](file://dtxadmin.dell.com/data$/upload_only/8yrvq42)
  -------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

From: 张文治 \[[mailto:zwz@fenbi.com](mailto:zwz@fenbi.com)[\] ]

Sent: Monday, April 20, 2015 10:59 AM

To: W, Robin

Cc: Jiang, Yunguang; Lai, Flying

Subject: Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

 

cc robin_w

 

请帮忙提供一个ftp上传最新的日志

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"张文治\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>;

Date:  Sun, Apr 19, 2015 11:08 AM

To:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Cc:  \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>; 

Subject:  Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

另有一台性能目前一直正常的机器的log，因为文件较大，麻烦提供一个ftp账号上传。之前的账号登陆后貌似不能用了

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"张文治\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>;

Date:  Sun, Apr 19, 2015 11:07 AM

To:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Cc:  \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>; 

Subject:  Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

这个机器我在系统reboot重启后，发现性能仍然存在问题。

后来又做了poweroff，在远程管理卡再开机后，性能恢复正常了。

 

重新收集上述日志如附件，是同一台机器性能变好的时候的log。

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"张文治\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>;

Date:  Sun, Apr 19, 2015 10:17 AM

To:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Cc:  \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>; 

Subject:  Re:RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

今天又出现了速度慢的情况，抓日志如下：

1：dset日志

2：阵列卡日志

3：iostat输出

4：nmon日志

5：我打开阵列卡的dpmstat监控，把lct ra ext hist等参数输出到文件ra lct ext hist供参考，命令如下

megacli -AdpSetProp -DPMenable -1 -aALL

 megacli  -DpmStat -Dsply lct -aALL  \>\> /tmp/lct

 megacli  -DpmStat -Dsply ra -aALL    \>\> /tmp/ra

 megacli  -DpmStat -Dsply ext -aALL  \>\> /tmp/ext

 megacli  -DpmStat -Dsply hist -aALL \>\>/tmp/hist

 

发现有问题的机器的几乎所有盘的io lct都很高，有时候会达到秒级

 

s04log.tar.gz 是有问题的服务器，升级过阵列卡的firmware

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>;

Date:  Wed, Apr 15, 2015 11:04 AM

To:  \"zwz\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>; 

Cc:  \"Yunguang_Jiang\"\<[Yunguang_Jiang@Dell.com](mailto:Yunguang_Jiang@Dell.com)\>; 

Subject:  RE: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

张先生

            根据我们二线工程师的建议，性能正常的时候抓日志可能就没有什么太大必要了。

            麻烦您在机器出现问题的时候帮忙获取下如下日志：

1.        正常机器的DSET/阵列卡以及NMON日志

2.       不正常机器的DSET/阵列卡以及NMON日志

非常感谢

 

NMON收集方法：

1，把附件解压后得到一个没有后缀的文件，把这个文件拷贝到centOS的一个临时目录，例如：/test

2，命令行切换到/test目录，添加执行权限：chmod 777 nmon_x86_64_centos6  

3，命令行切换到/test目录执行命令：./ nmon_x86_64_centos6  -fT -s 2 --c 3600 -m /test 

4，检查命令是否运行成功： ps --ef \|grep nmon \|wc --l   (如果返回结果是2表示运行成功，如果返回结果是1表示没有运行成功)

5，1小时后任务自动结束（可以通过第四步检查是否已经结束）结束后把/test目录下除了nmon_x86_64_centos6 以外的文件发给我。

 

 

Best Regards!

Flying Lai 赖志明

Enterprise Engineer

Dell \| Enterprise Support Services

 

From: 张文治 \[[mailto:zwz@fenbi.com](mailto:zwz@fenbi.com)[\] ]

Sent: Wednesday, April 15, 2015 7:48 AM

To: Lai, Flying

Subject: Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

重新上传至ftp了

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"张文治\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>;

Date:  Mon, Apr 13, 2015 02:09 PM

To:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>; 

Subject:  Re:RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

 

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>;

Date:  Mon, Apr 13, 2015 09:00 AM

To:  \"zwz\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>; 

Subject:  RE: Re:请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

张先生

            麻烦您使用附件word文档这个版本尝试收集下DSET日志。

            再麻烦您使用附件.zip里头的.sh脚本收集阵列卡日志。

            谢谢

 

5 -- 执行的输出画面及结果。

![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_007.png]]

 

 

Best Regards!

Flying Lai 赖志明

Enterprise Engineer

Dell \| Enterprise Support Services

 

From: 张文治 \[[mailto:zwz@fenbi.com](mailto:zwz@fenbi.com)[\] ]

Sent: Friday, April 10, 2015 5:39 PM

To: Lai, Flying

Subject: Re:请将日志上传到FTP（ST:3VGXQ42）

 

 

sosreport已经上传。

dset运行有问题，没有创建报告的选项，如下：

 

Choose an option:

 

            1) View DSET Release Notes

            Show the latest DSET release notes.

 

            2) Remote Provider

            Installs required components to allow reports to be generated from a remote system against this system.

 

            3) Quit

            Exits the installation process

 

Enter option (1-3):

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

张文治  DevOps@猿题库  [Tel:13581614504](Tel:13581614504)

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From:  \"Flying_Lai\"\<[Flying_Lai@Dell.com](mailto:Flying_Lai@Dell.com)\>;

Date:  Fri, Apr 10, 2015 10:41 AM

To:  \"zwz\"\<[zwz@fenbi.com](mailto:zwz@fenbi.com)\>; 

Subject:  请将日志上传到FTP（ST:3VGXQ42）

 

Dell - Internal Use - Confidential 

您好!

      我是今天接您电话的戴尔技术支持工程师赖志明,感谢您致电戴尔!

      有什么问题的话可以和我联系,我会尽量帮您解决的!

 

            FTP地址：[http://dtxdropbox.dell.com/](http://dtxdropbox.dell.com/)

  ------------- ----------
  Login Name:   3vgxq42
  Password:     2twikqut
  ------------- ----------

 

 

=====================================================================================================================================

       DELL 服务器操作系统安装汇总：

       <http://zh.community.dell.com/support_forums/poweredge/f/279/t/9593.aspx#9593>

 

       如果有任何您觉得很麻烦/困扰，多花比较多努力/精力或不好的体验，也还请Mail 回复给我，您的意见有助于改善我们的服务。

 

 

 

Best Regards!

赖志明

Flying Lai

企业级产品工程师

戴尔\|企业技术支持 

我的表现如何?请联系我的经理[Ray_Wong@dell.com](mailto:Ray_Wong@dell.com) 

[Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \|一款软件插件，可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣零部件

回复邮件获取详细资料或点击 SupportAssist超链接了解更多信息！！

戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_008.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_009.gif]]](http://www.weibo.com/techsupportdell)   [![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_010.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[个人记录_后续需要学习_008_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_RE_ Re_011.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

已使用 OneNote 创建。
