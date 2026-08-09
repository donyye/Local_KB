关于SSD cache满负载（SanDisk）

2016年1月11日

8:51

- ::: 
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: R730XD\|other software issue\|pros\| sr#922633498 ST#HYHVG82
    发件人     Lian, Wenxiang
    收件人     Li, Jiangxiong; hu, Changping; Chen, Eddy
    抄送       CN XMN TS ENT L2 SME
    发送时间   2016年1月8日 23:15
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   Changping,

   

  既然日志收集了就要学会去看，要不收集日志干啥你说对不对。

   

  目前DAS CACHE日志中没有显示软件有警告或者报错，目前版本1.3.1。

   

  从客户截图和日志来看，目前E盘分区大小54TB，SSD cache大小371G

  Volume 2

  ========

  Disk 3 Volume 0 (E:\\)

  OS Volume Name: [\\\\?\\Volume\\](file://%3f/Volume%7bc34d1214-e1e3-4e5c-8793-b48767f380a5%7d/)

  DOS Volume Name: \\Device\\HarddiskVolume8

  Size: 54.58 TB

  Boot Volume: No

  File System: NTFS

   

  从cache使用率来讲，目前已经满负荷使用，读写I/O 100%运行。

   

      I/O Summary

   

      Total I/O  . . . . . . . . . . . . =       60,840,932

      Reads I/O  . . . . . . . . . . . . =       38,397,762 ( 63.1%)

      Write I/O  . . . . . . . . . . . . =       22,443,170 ( 36.9%)

      Total Data Read MB . . . . . . . . =        8,585,018 ( 46.1%)

      Total Data Written MB  . . . . . . =       10,018,400 ( 53.9%)

   

  Info: Current time 01/08/2016 16:07

  Logical SSD 0x2d3fa80a828

      Name                 = PWSR252016 F Cache

      ROC Mode             = NO

      Ssd state            = ON

      Total size KB:       = 389937152   (371.87 GB)

      Dirty size KB:       = 348497828   (332.35 GB)

      Clean size KB:       = 4432            (0.00 GB)

      Free size  KB:       = 40519436     (38.64 GB)

      drive letter         = F:

      device name          = \\Device\\HarddiskVolume6

      volume guid          = [\\\\?\\Volume](file://%3f/Volume%7b22d0cd89-cdd0-41f9-920b-19130bb4f0fa%7d)

   

  Info: Current time 01/08/2016 16:07

   

  VOLUME #1

       drive letter        = E:

       dev_name            = \\Device\\HarddiskVolume8

       guid_name           = [\\\\?\\Volume](file://%3f/Volume%7bc34d1214-e1e3-4e5c-8793-b48767f380a5%7d)

       size                = 57226110 MB.

       lssd tag            = 0x000002d3fa80a828.

       descriptor          = [\\\\.\\Volume](file://./Volume%7bc34d1214-e1e3-4e5c-8793-b48767f380a5%7d)

   

       \*\*\*\* ACCELERATED: WRITE BACK \[0x00000004\] \*\*\*\*

   

  出现客户截图中的信息有两种情况：

  1.  刚开始使用IO并没有写入，需要做持续写入测试，如果正常情况下如下图：

  ![[Technology_ALL_未分类知识库_043_关于SSD cache满负载（SanDisk）_001.png]]

  1.  就如客户大量的IO写入导致SSD cache满负荷运转，所以命中率下降。

   

  如果客户对该解释有异议，可以新建一个100G或者50G的Volume然后将SSD cache绑定，然再做持续写入测试。

   

  1.  还有客户的应用也有一定的关系，das cache通常用于hot data加速， 像整个vhdx文件如果设置了IO的

  SanDisk DAS Cache software accelerates server applications by leveraging solid-state disk (SSD) memory installed on the host server to create a Read-Write cache for hot data -- the data that applications access most frequently. 

   

   

  SanDisk DAS Cache v1.3.1 run-time statistics

      ========================================

      Logical SSD tag       0x2d3fa80a828

      SSD Cache Section tag 0x2d3fa80a829

      SSD state = ON

   

      Stats interval 12/14/2015 18:18:43 - 01/08/2016 16:07:18

   

   

      Cache

      Total size MB  . . . . . . . . . . =          379,904

      In-use size MB . . . . . . . . . . =          340,334 ( 89.6%)

   

      I/O Summary

   

      Total I/O  . . . . . . . . . . . . =       60,840,932

      Reads I/O  . . . . . . . . . . . . =       38,397,762 ( 63.1%)

      Write I/O  . . . . . . . . . . . . =       22,443,170 ( 36.9%)

      Total Data Read MB . . . . . . . . =        8,585,018 ( 46.1%)

      Total Data Written MB  . . . . . . =       10,018,400 ( 53.9%)

   

      Read I/O Histogram (by length KB)

          1 - 8  . . . . . . . . . . . . =       17,737,670 ( 46.2%)

          8 - 32 . . . . . . . . . . . . =        9,040,158 ( 23.5%)

          32 - 64  . . . . . . . . . . . =        1,740,872 (  4.5%)

          64 - 128 . . . . . . . . . . . =        2,456,203 (  6.4%)

          128 - 256  . . . . . . . . . . =           49,834 (  0.1%)

          256 - 512  . . . . . . . . . . =              228 (  0.0%)

          512 - 1024 . . . . . . . . . . =        6,772,923 ( 17.6%)

          1024 - Any . . . . . . . . . . =          599,874 (  1.6%)

   

      Write I/O Histogram (by length KB)

          1 - 8  . . . . . . . . . . . . =       16,800,703 ( 74.9%)

          8 - 32 . . . . . . . . . . . . =        1,492,594 (  6.7%)

          32 - 64  . . . . . . . . . . . =          563,967 (  2.5%)

          64 - 128 . . . . . . . . . . . =          535,995 (  2.4%)

          128 - 256  . . . . . . . . . . =          510,395 (  2.3%)

          256 - 512  . . . . . . . . . . =        1,221,964 (  5.4%)

          512 - 1024 . . . . . . . . . . =          129,142 (  0.6%)

          1024 - Any . . . . . . . . . . =        1,188,410 (  5.3%)

   

      Caching Operations Summary

          Read Total I/O . . . . . . . . =       38,397,762

             Read Full Hit I/O . . . . . =                18,903,064 ( 49.2%)

             Read Partial Hit I/O  . . . =                        94 (  0.0%)

             Read Miss I/O . . . . . . . =                19,494,604 ( 50.8%)

          Read Total from SSD MB . . . . =          724,483 (  8.4%)

   

          Write Total I/O  . . . . . . . =       22,443,170

          Write Cache I/O  . . . . . . . =        4,848,382 ( 21.6%)

             Full Overwrite I/O  . . . . =                 4,576,044 ( 94.4%)

             Partial Overwrite I/O . . . =                     1,753 (  0.0%)

             No Overwrite I/O  . . . . . =                   270,585 (  5.6%)

          Write Bypass I/O . . . . . . . =       17,594,788 ( 78.4%)

             Length \> threshold  . . . . =                 2,475,559 ( 14.1%)

             Alignment . . . . . . . . . =                15,119,229 ( 85.9%)

          Write Cache MB . . . . . . . . =           24,169 (  0.2%)

             Full Overwrite MB . . . . . =                    21,934 ( 90.7%)

             Partial Overwrite MB  . . . =                        24 (  0.1%)

             No Overwrite MB . . . . . . =                     2,211 (  9.2%)

          Write Bypass MB  . . . . . . . =        9,994,230 ( 99.8%)

   

      Cache De-staging

          Reduction in Disk Write I/O  . =               21.6%

          Reduction in Disk Write Data . =                0.2%

          Flush Read SSD I/O . . . . . . =                0

          Flush Read SSD MB  . . . . . . =                0

          Flush Disk Writes I/O  . . . . =                0

          Flush Disk Writes MB . . . . . =                0

   

      Cache Utilization

          Read/Write Region Size MB  . . =          340,329 ( 89.6%)

          De-staged  Region Size MB  . . =                4 (  0.0%)

          Unused Size MB . . . . . . . . =           39,569 ( 10.4%)

   

      Metadata Snapshots

          Metadata Snapshots written . . =              330

          Metadata Snapshot Total Size MB=            3,321

          Metadata incremental MB  . . . =              526

   

      Metadata Paging

          Metadata Page Swap out I/O . . =                0

          Metadata Page Swap out MB  . . =                0

          Metadata Page Swap in I/O  . . =          471,401

          Metadata Page Swap in MB . . . =            1,841

   

      Current Outstanding Operations

          External I/O

             Reads in progress . . . . . =                0

             Writes in progress  . . . . =                0

          Flush I/O in progress  . . . . =                0

   

          SSD I/O in progress

             Queued reads  . . . . . . . =                0

             Queued writes . . . . . . . =                0

             Outstanding reads . . . . . =                0

             Outstanding writes  . . . . =                0

             Outstanding coalesced writes=                0

             Cache populate (read) . . . =                0

          Error Info

             Read errors . . . . . . . . =                0

             Write errors  . . . . . . . =                0

   

  Thanks & Regards,

   

  Wenxiang Lian

  Enterprise Senior Engineer

  Dell \| Global Support and Deployment

   

   

  From: Li, Jiangxiong

  Sent: Friday, January 08, 2016 16:31

  To: Lian, Wenxiang

  Cc: hu, Changping; CN XMN TS ENT L2 SME

  Subject: RE: R730XD\|other software issue\|pros\| sr#922633498 ST#HYHVG82

   

  Dell - Internal Use - Confidential 

  Wenxiang

  Please help on this case

  Jiangxiong

   

   

   

  \-\-\-\--Original Message\-\-\-\--

  From: <No_reply@dell.com> \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

  Sent: 2016年1月8日 16:26

  Cc: CN XMN TS Server Escalation ; hu, Changping

  Subject: R730XD\|other software issue\|pros\| sr#922633498 ST#HYHVG82

   

  1、客户有3台机器都用das cache，，部署了一段时间，，最近才发现此问题

  2、数据服务器 ，有部署 hyper ，，落地的数据为 .vhdx 

  3、a. F: 为SSD缓存设备

  b. E: 为HDD设备，.vhdx存放在这个卷上。

  4、客户疑问write IO 性能FLushes 0 OPS ACCELERATED WRITES 21%

  5、客户3台服务器同样配置，同样应用，同样的问题

  6、服务器购买三年服务的DAS cache license

  7、客户现在收集了das cache日志，及截图

  8、已建议客户再收集到dset日志

 

已使用 OneNote 创建。
