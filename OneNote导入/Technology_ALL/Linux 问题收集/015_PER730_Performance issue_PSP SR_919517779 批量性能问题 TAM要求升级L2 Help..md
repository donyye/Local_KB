[ PER730\|Performance issue\|PSP SR:919517779 ]批量性能问题 TAM要求升级L2 Help.

2015年12月16日

11:07

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       recommend case closed:RE: PER730\|Performance issue\|PSP SR:919517779 批量性能问题 TAM要求升级L2 Help.
  发件人     W, Robin
  收件人     Guo, Yueliang
  抄送       Yang, Frank; CN XMN TS ENT L2 SME; CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   2015年12月16日 11:03
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Yueliang,

I had done the RA.

 

Issue:

磁盘多线程读性能差。

 

Solution：

This is working as design.

Sales will continue to handle this case from business side.

 

下图中sdb是使用6T 4KN的磁盘，其他的都是6T 512E的磁盘，同样的测试参数（块大小1M，8线程顺序读）

![[Technology_ALL_Linux 问题收集_015_PER730_Performance issue_PSP SR_919517779_001.png]]

 

Comments:

512E和4KN磁盘的case会越来越多，这两种磁盘和普通512N物理设计是不一样的，这种技术上的不同会给客户带来不一样的使用体验，以下总结了这两种磁盘的差别，希望大家对这两种磁盘有个正确的认识，在处理实际case的时候能用的上。

所有的4KN和512e都称为advanced format（AF），AF的优势或存在的意义是可以支持更大容量的磁盘。

4K的缺点是很多OS不支持，所以就有了512E的过度，但是512E的缺点是物理扇区都是4k，逻辑扇区是512字节，每读取或修改一个512扇区的是需要把整个4K的物理扇区全部读出来，这样会造成读取资源浪费，也就是这个case中客户遇到的多线程读性能很差。单线程顺序读影响不大，因为读出来的4K数据命中率基本是100%。

 

512E的操作模式如下图所示，中每个physical sector都是4K，每个PS包含8个逻辑sector，OS如果要读取其中的某一个逻辑sector就需要先把整个PS全部读到内存中。

![[Technology_ALL_Linux 问题收集_015_PER730_Performance issue_PSP SR_919517779_002.png]]

 

Thanks

Robin

 

From: Li, Jiangxiong

Sent: 2015年11月5日 16:57

To: Guo, Yueliang; W, Robin

Cc: Yang, Frank; CN XMN TS ENT L2 SME

Subject: RE: PER730\|Performance issue\|PSP SR:919517779 批量性能问题 TAM要求升级L2 Help.

 

Dell - Internal Use - Confidential 

Robin

Please help on this case

 

 

 

Li Jiangxiong

 

 

From: Guo, Yueliang

Sent: 2015年11月5日 16:42

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Cc: Guo, Yueliang \<<Yueliang_Guo@dell.com>\>; Yang, Frank \<<Frank_Yang@Dell.com>\>

Subject: PER730\|Performance issue\|PSP SR:919517779 批量性能问题 TAM要求升级L2 Help.

 

Dell - Internal Use - Confidential 

Dear L2.

 

TAM Frank_Yang con-call customer feedback.

1.这一批R730有60-80台，刚要上线时需要先用开源IO Zone tools测试读写性能，达到要求才能步曙应用， R730 ,H730阵列卡，配12pc 6T HD.2PC SSD config OS，开源IO Zone tools test Non-Raid mode 单线程 read/write可以达150-160M/S，但多进程并发读性能正常，写性能比较低70-80M/S ,配置 Raid mode 每个硬盘配Raid0,  写性能100M/S 读性能70M/s比较低.需要解决性能测试问题才能应用上线.OS is centos 6.5

 

2.客户安装开源测试软件iozone对磁片性能进行测试，首先是写操作。客户实验使用了500GB数据，分成96个进程，即每个硬盘同时8个进程写入数据，同时对12块硬盘进行写入，结果如下（基本正常）

3.进行硬盘读操作，发现性能不好，大部分在70MB/s的速度，正常应该达120MB左右。

Get Dest log had unload to delta. Check bios 1.3.6 idrac FW.2.20.20.20 H730 Raid card FW: 25.3.0.0016安装CentOS 6.5 64位系统

 

EU :基于上面日志，看要怎么去改善存储的性能。这个项目是中山电子政务云的二期扩容项目，而且目前这个项目处于设备安装调试，按照项目进度这周日（11月8号）就要完成60台R730XD分布式存储的安装。目前由于性能的问题，整个项目进度在这停滞。所以请麻烦尽快协助查找原因，给出相应解决方案.

 

重要客户，批量问题.TAM建议升级L2帮助.

 

郭跃亮 Guo,Yueliang

企业级产品工程师

戴尔\|企业级技术支持  

客户反馈\| 我表现如何？请联系我的经理[Johnson_Lw_Chen@dell.com](mailto:Johnson_Lw_Chen@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

 

 

已使用 OneNote 创建。
