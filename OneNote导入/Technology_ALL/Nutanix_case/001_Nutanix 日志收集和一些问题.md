Nutanix 日志收集和一些问题

2015年11月10日

15:42

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: Case#: 919957923 ST: C53KZ72 Nutanix monitor issue[  ]
  发件人     Yin, Guoxun
  收件人     Li, Jiangxiong; Pan, Jadge; CN XMN TS Server Coach
  抄送       CN XMN TS ENT L2 SME
  发送时间   2015年11月10日 15:33
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi Jadge

Nutanix 问题不是Antti支持的，是他所在的SST team提供支持的，而且他们只接软件上的问题，硬件是我们来处理的。

针对下面CASE的描述，请先提醒客户，Nutanix节点上的SSD和SAS不允许热插拔，SSD故障离线后CVM会做相应处理，这跟突然把SSD拔掉的情景是不一样的，不建议这样测试。 

如果要移除硬盘，需要现在WEBECONSLE里面选择相应硬盘并确认删除，待执行完成后再在服务器上移除硬盘。

 

请收集以下日志来判断现在的软硬件情况：

 

1：Dset

2：请客户SSH登录Nutanix CVM，然后执行"   ncc health_checks run_all"命令，在执行完成后将产生的屏幕输出或者将/home/nutanix/data/logs/文件夹下面的ncc-output-latest.log 这个日志给我们。

              

英文版本：Login to the CVM via SSH tools, run command ncc health_checks run_all , wait until it finished （please send back the file of   /home/nutanix/data/logs/ncc-output-latest.log）

3: 如果客户可以操作，请他收集下Prism中关于节点警报的截图，以及现在的所有节点状态截图[  \<\-\--]后加

 

请收集NCC log：

SSH登录Nutanix CVM，然后执行 ncc health_checks run_all命令，在执行完成后将产生的屏幕输出或者将/home/nutanix/data/logs/文件夹下面的ncc-output-latest.log 这个日志给我们。

 

 

From: Li, Jiangxiong

Sent: 2015年11月10日 14:39

To: Pan, Jadge; Yin, Guoxun

Cc: CN XMN TS ENT L2 SME

Subject: RE: Case#: 919957923 ST: C53KZ72 Nutanix monitor issue

 

Dell - Internal Use - Confidential 

Guoxun

Please help on this case

 

 

Li Jiangxiong

 

 

From: Pan, Jadge

Sent: 2015年11月10日 14:05

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Subject: Case#: 919957923 ST: C53KZ72 Nutanix monitor issue

 

Dell - Internal Use - Confidential 

Hi L2 team,

 

For this case please help to assign to Antti Huang.

Question detail please see Delta log record.

 

Thanks!

 

Jadge Pan 

 

 =\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Kindly provide the following information

1.        Number of nodes in the cluster

2.        Hypervisor version and build number

3.        AOS version

4.        Problem description with screenshots if possible

5.        NCC output and other necessary logs

NCC收集方法。SSH登录Nutanix CVM，然后执行 ncc health_checks run_all命令，在执行完成后将在/home/nutanix/data/logs/文件夹下面有个ncc-output-latest.log文件。提供给我们

 

 

apj_sst[   ]\-\--\> SST team 群邮件

APJ_SST[         ][APJ_SST@dell.com](mailto:APJ_SST@dell.com)

 

 

 

Please collect the following information.

 

1, AOS (NOS) and NCC versions

Prism à upgrade software 

like the diagram below information

![[Technology_ALL_Nutanix_case_001_Nutanix 日志收集和一些问题_001.jpg]]

 

2\. Latest NCC logs (ncc-output-latest.log)

Login to the CVM via SSH tools, run command ncc health_checks run_all , wait until it finished （please send back the file of   /home/nutanix/data/ncc-output-latest.log）

 

3, Screen capture of the error in Prism

like the diagram below information

![[Technology_ALL_Nutanix_case_001_Nutanix 日志收集和一些问题_002.jpg]]

 

 

4\. Fault time log collection.

ncc log_collector run_all \--start_time=YYYY/MM/DD-HH:MM:SS \--end_time=YYYY/MM/DD-HH:MM:SS=1  

 

#problem time（YYYY/MM/DD-HH:MM:SS） 

 

ncc log_collector run_all \--last_no_of_days=3

获取最近3天的日志

 

========================================================

NUTANIX license注册问题，需要收集下面信息：

-All nodes service tag(所有节点ST): 

-Dell order number(Dell订单号码):

-Email registered to manage the licensing(管理License的邮箱):

-Customer name(最终客户姓名)：

-Contact(联系信息)：

-Company name(公司名称)：

-Cluster_summary_file：如下图位置点击生成

![[Technology_ALL_Nutanix_case_001_Nutanix 日志收集和一些问题_003.jpg]]

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ESXi的服务和Nutanix的服务是分开的，检查下客户是否有OEM vSphere，提供下订单号。

另外明确下以下问题：

1. "宕机"的详细情况：

a. 当时屏幕显示？

b.        能否Ping通？

c.        能否单独登录主机？

d.        VM当时的状态？

e.        怎么恢复的？

2. 业务不能没有切换到其他节点的详解：

a.        什么业务？

b.        客户期望的切换时指什么？

3. 收集带TTY的TSR log

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

从新设置prism密码：

Per phone discussed, please reset Prism password as below, and replace the \"xxxxxxx\" with your preferred password.

Example:

You can run the following command from any Controller VM in your Nutanix Cluster.

ncli user reset-password user-name=\"admin\" password=\"xxxxxxx\"

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

请尽快收集下面信息，完成后我会升级到SST工程师分析。

 

1. 客户的Nutanix系统是for 什么的(windows / KVM / VMware)？AOS版本是什么？

 

2. 请收集NCC log：

SSH登录Nutanix CVM，然后执行 ncc health_checks run_all命令，在执行完成后将产生的屏幕输出或者将/home/nutanix/data/logs/文件夹下面的ncc-output-latest.log 这个日志给我们。

 

3. Prism 上是否有报错，如下：

![[Technology_ALL_Nutanix_case_001_Nutanix 日志收集和一些问题_004.jpg]]

 

=====升级给SST模板=========

 

1.Case description

Nutanix OS for ESXI AHV Windows

 

2.Log collection status   

The NCC log has been collected.

The customer is gathering and will provide later.

The customer refuses to collect logs.

Log ： [\\\\xmntsdb03\\EntTS_Log\\Nutanix_Case_log](file://xmntsdb03/EntTS_Log/Nutanix_Case_log)[   ]

 

3.Hardware detection

No hardware error was found in the TSR.

The customer is collecting and will provide later.

The customer does not want to provide hardware logs, because he is reporting software problems and will provide them later if necessary.

The customer inquiries software issues, no hardware logs.

 

 

 

 

===============

Nutanix账号升级注册：

 

[\\\\XMNNX33FS02.XMN.APAC.DELL.COM\\xmn_gsd_entsme\$\\Nutanix](file://XMNNX33FS02.XMN.APAC.DELL.COM/xmn_gsd_entsme$/Nutanix)

 

 

===================

AOS升级以后请同时升级ncc和foundation,具体操作请参考如下link：

AOS upgrade link:

[https://portal.nutanix.com/#/page/docs/details?targetId=Web-Console-Guide-Prism-v510:upg-cluster-upgrade-aos-c.html](https://portal.nutanix.com/#/page/docs/details?targetId=Web-Console-Guide-Prism-v510:upg-cluster-upgrade-aos-c.html)

 

ncc upgrade link:

[https://portal.nutanix.com/#/page/docs/details?targetId=Web-Console-Guide-Prism-v510:ncc-cluster-ncc-upgrade-wc-t.html](https://portal.nutanix.com/#/page/docs/details?targetId=Web-Console-Guide-Prism-v510:ncc-cluster-ncc-upgrade-wc-t.html)

 

foundation upgrade link:

[https://portal.nutanix.com/#/page/docs/details?targetId=Web-Console-Guide-Prism-v510:upg-cluster-foundation-upgrade-wc-t.html](https://portal.nutanix.com/#/page/docs/details?targetId=Web-Console-Guide-Prism-v510:upg-cluster-foundation-upgrade-wc-t.html)

 

 

已使用 OneNote 创建。
