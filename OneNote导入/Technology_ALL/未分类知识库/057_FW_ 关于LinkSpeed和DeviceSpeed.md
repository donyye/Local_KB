FW: 关于LinkSpeed和DeviceSpeed

Monday, September 26, 2016

9:22 AM

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       FW: 关于LinkSpeed和DeviceSpeed
  发件人     Yin, Gator
  收件人        CN XMN ASG
  发送时间   Saturday, September 24, 2016 12:55 PM
  附件       \<\<lsi-wp-databolt-performance.pdf\>\>
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------

 

分享给大家一个高水平的技术讨论。硬件和OS接口层面的深入讨论。

 

Gator Yin

ESG \| WebTech & Global Customer

Office +86 10 5826 1836

Mobile +86 1590 132 0537

 

From: Yang, Monty

Sent: Friday, September 23, 2016 7:32 PM

To: 范振国(基础平台部) \<fanzhenguo@didichuxing.com\>; Jin, Chao \<Chao_Jin@Dell.com\>

Subject: 答复: 关于LinkSpeed和DeviceSpeed

 

Hi 振国,

 

LSI 3108具备Datablot的feature.可以migrating to 12Gb/s SAS.

Aggregation enables 12Gb/s SAS to be introduced into an existing 6Gb/s storage .Array in a way that provides an immediate doubling of overall system performance.

In fact,with bandwidth aggregation ,there is no need to use any 12Gb/s SAS disks to achieve 12Gb/s SAS performance at the system level.

原文可以参考附件的文档

 

另外你通过 /c0 show all 可以看到LSI 3108的卡是支持改动link speed的。可以通过手动set link speed来设置速率。FW 设定默认应该是Auto，可以改成3.0,6.0,或12Gb

然后通过/c0/p0 show来看手动设置后是否锁定。

 

发件人: 范振国(基础平台部) \[[mailto:fanzhenguo@didichuxing.com](mailto:fanzhenguo@didichuxing.com)[\] ]

发送时间: 2016年9月23日 8:55

收件人: Yang, Monty \<[Monty_Yang@Dell.com](mailto:Monty_Yang@Dell.com)\>; 

主题: 答复: 关于LinkSpeed和DeviceSpeed

 

Hi Monty，

 

    回答你一下。。我说的"LinkSpeed"指的是在Dell的R730机器上面的截图。如下

 

![[Technology_ALL_未分类知识库_057_FW_ 关于LinkSpeed和DeviceSpeed_001.png]]

 

而PercCLI show的应该是硬盘背板的接口速率，即12Gb硬盘背板接口速率\-\-\-\-\--perccli  show 的Speed有两个，Device Speed指的是硬盘接口速率，LinkSpeed是链路速率。  而，link speed是由硬盘，背板，raid卡中最小的速率决定的。

 

所以请问一下，上图show的内容中，为什么LinkSpeed比DeviceSpeed要高呢？

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

![[Technology_ALL_未分类知识库_057_FW_ 关于LinkSpeed和DeviceSpeed_002.png]]

 

发件人: [Monty.Yang@dell.com](mailto:Monty.Yang@dell.com) \[[mailto:Monty.Yang@dell.com](mailto:Monty.Yang@dell.com)[\] ]

发送时间: 2016年9月23日 2:23

收件人: 范振国(基础平台部); 

主题: 答复: 关于LinkSpeed和DeviceSpeed

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Hi 振国,                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 其实Link Speed有不同定义，我的理解是你说的Link Speed 应该是PCI link speed,即Negotiated Link Speed。参考avago  doc如下\~ |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Actual Link Speed versus Negotiated Link Speed                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| In an actual system, additional factors can cause the actual negotiated speed and width to be lower than the                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| maximum supported by the slots or the storage controller. Therefore, you must verify the negotiated speed                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| and width. Read the PCIe Configuration Space to verify the capabilities and currently negotiated speed and width.                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 而PercCLI show的应该是硬盘背板的接口速率，即12Gb硬盘背板接口速率，谢谢！                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 发件人: 范振国(基础平台部) \[[mailto:fanzhenguo@didichuxing.com](mailto:fanzhenguo@didichuxing.com)[\] ]                                                                                                                                                                                                                                             |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 发送时间: 2016年9月22日 21:35                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 收件人: Yang, Monty \<[Monty_Yang@Dell.com](mailto:Monty_Yang@Dell.com)\>;                                                                                                                                                                                                                                                                                                                         |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 主题: 关于LinkSpeed和DeviceSpeed                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Hi Monty ，                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|    我看Dell的D机型（R730），用perccli查看单个硬盘（D机型是SSD）的信息 show all ，发现 Device Speed是6Gb，LinkSpeed 是12Gb（如下图）  |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   ----- -------------- -------------------                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   SSD   物理盘         S3610 800GB 6Gb/s                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|         Device Speed   6.0Gb/s                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|         Link Speed     12.0Gb/s                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|   ----- -------------- -------------------                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| :::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|    据我之前的了解，LinkSpeed指的是RAID卡和硬盘协商后的SAS速率；Device Speed指硬盘的原始设计速率。link speed由硬盘，背板，raid卡中最小的速率决定。                                                                                                                                                    |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|    那请问：Dell机器上，为什么LinkSpeed比DeviveSpeed的速率高？                                                                                                                                                                                                                                                                                                                                        |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ![[Technology_ALL_未分类知识库_057_FW_ 关于LinkSpeed和DeviceSpeed_002.png]]                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 

 

 

 

 

 

已使用 OneNote 创建。
