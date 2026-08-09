2023-new\|Windows 日志收集与分析

2022年8月8日

11:17

Shcollect

\
下载收集工具： [https://jpwinsup.github.io/mslog/other/tools/shacollector/shacollector.zip](https://jpwinsup.github.io/mslog/other/tools/shacollector/shacollector.zip)

解压后，放到c:\\tools\\目录下

以管理员身份打开c命令提示符，输入cd c:\\tools\\shacollector，确认这个目录下有shacollector.bat文件。

 

再运行下面的命令：

shacollector.bat support all c:\\mslog

将c:\\下mslog压缩一下，将压缩文件发给DELL。

注：服务器上压缩方法：对着要压缩的文件夹，右键-\>发送到-\>压缩（zipped）文件夹，就可以压缩日志文件。

 

![[Technology_ALL_windows_case_003_2023-new_Windows 日志收集与分析_001.png]]

 

 

1. 看系统是否OEM，在 setup 目录的 \*\_slmgr_dlv 文件

setup/ \*\_slmgr_dlv

![[Technology_ALL_windows_case_003_2023-new_Windows 日志收集与分析_002.png]]

 

![[Technology_ALL_windows_case_003_2023-new_Windows 日志收集与分析_003.png]]

这里 SLP[  ]是 system lock preinstall 的意思

这种是与OEM 厂商的 BIOS 匹配，匹配对的自动激活，不需要网络。大部分这种。

 

另外一种是 NONSLP ，是属于rok reseller option kit

这种是代理商卖的套件。需要支持

 

Windows Embedded 这种是定制的Windows，有提供支持，但是没有ISO和驱动等。

![[Technology_ALL_windows_case_003_2023-new_Windows 日志收集与分析_004.png]]

比如有一个case，艾默生的，他们按照 2016 发现没有 H750 的包，找我们没有用，他们公司会有这个镜像，如果他们没有要去找 PM 去解决。

 

2. 检查客户最近是否有打补丁

检查 setup/\*\_get-hotfix

如果补丁没有打成功，那就到 setup/CBS 目录里查看，有相关原因。

 

3. 杀毒软件导致的重启与内存泄漏

检查 fltmc/ 目录

 

4. 主要看的日志

eventlog/evtx 目前下有 \*\_evt_System 与 \*\_evt_Application

System: 1076

正常》Operating system: Upgrade (planned)

正常》Service Pack (计划内)

不正常》last unexpected shutdown ...

\-\-- hang issue \-\--

2019 - nonpaged pool depletion[  ]非分页池耗尽，也就是资源方面

2020 - paged pool depletion[   ]分页池耗尽，内存相关

129 - disk error and bus reset[  ]磁盘有问题

50/55 - nfts for harddisk error[  ]底层文件系统有问题，元数据有问题

7011 - service timeout[  ]某些服务超时

1 - source WHEA-Logger for fatal hardware error[  ]致命硬件错误

 

 

5. 性能日志

![[Technology_ALL_windows_case_003_2023-new_Windows 日志收集与分析_005.png]]

 

6.[  BSOD issue]

导致BSOD的问题大概两个方面

[  ]硬件与软件

据微软统计

70% 是由第三方驱动代码引起

10% 是由硬件问题

5% 是微软自己的问题

15% 是未知的问题导致，需要通过分析 dump 得知。

 

在 \*\_evt_System 可以筛选 BugCheck 来获得蓝屏代码

![[Technology_ALL_windows_case_003_2023-new_Windows 日志收集与分析_006.png]]

 

 

 

已使用 OneNote 创建。
