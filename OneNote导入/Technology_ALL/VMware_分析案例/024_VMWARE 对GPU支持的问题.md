VMWARE 对GPU支持的问题

Thursday, March 10, 2016

9:52 AM

  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------
  主题       FW: VMWARE 对GPU支持的问题
  发件人     Wang, Xing Fang
  收件人     CN XMN TS ENT L2 SME; APJ Ent Resolution Managers China
  抄送       CN XMN TS ENT L2 Coach
  发送时间   Thursday, March 10, 2016 9:16 AM
  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Fyi

 

XingFang Wang

Technical Support Manager

Dell \| Global Customer Support Services

office +86-592-818-5846

Email [Xing_Fang_Wang@Dell.com](mailto:Your%20name@Dell.com)

Visit us at [http://support.ap.dell.com](http://support.ap.dell.com/)

How am I doing?E-mail my manager at <Ernest_Lee@dell.com>

 

From: Jianwei, Chen - Dell Team

Sent: Wednesday, March 09, 2016 5:05 PM

To: Wan, Yun; Lin, Kean; CN XMN ASG

Subject: RE: VMWARE 对GPU支持的问题

 

Dell - Internal Use - Confidential 

Dear All

从VMware的官方兼容列表来看，如下图，DELL 支持vDGA的服务器型号以及GPU 型号。请参考!

查询链接：[http://www.vmware.com/resources/compatibility/search.php?deviceCategory=vdga](http://www.vmware.com/resources/compatibility/search.php?deviceCategory=vdga)

![[Technology_ALL_VMware_分析案例_024_VMWARE 对GPU支持的问题_001.jpg]]

 

From: Wan, Yun

Sent: 2016年3月9日 14:25

To: Jianwei, Chen - Dell Team; Lin, Kean; CN XMN ASG

Subject: 答复: VMWARE 对GPU支持的问题

 

Dell - Internal Use - Confidential 

只有选择vDGA才支持CUDA。

 

Virtual Dedicated Graphics Acceleration (vDGA)

This technology provides a user with unrestricted, fully dedicated access to a single vGPU. Although consolidation and management trade-offs are associated with dedicated access, vDGA offers the highest level of performance for users with the most intensive graphics computing needs. It enables the use of applications that run OpenGL 4.4, Microsoft DirectX 9, 10, or 11, and NVIDIA CUDA 5.0.

 

Regards,

Wan Yun / 万云 13501135684

 

发件人: Jianwei, Chen - Dell Team 

发送时间: 2016年3月9日 14:18

收件人: Lin, Kean \<[Kean_Lin@dell.com](mailto:Kean_Lin@dell.com)\>; CN XMN ASG \<[CNXMNASG@DELL.com](mailto:CNXMNASG@DELL.com)\>

主题: RE: VMWARE 对GPU支持的问题

 

Dell - Internal Use - Confidential 

Hi Kean

 

目前VMware 的vGPU 虚拟化只兼容到了NVIDIA 的GRID K1&K2，Tesla M6&M60，是不支持CUDA 的。

 

 

感谢您对VMware产品的关注和支持！！

 

Thanks & Best regards

陈   建伟

Chen Jianwei 

13466556105

Ext: 8671994

 

 

 

 

From: Lin, Kean

Sent: 2016年3月9日 14:02

To: CN XMN ASG

Subject: VMWARE 对GPU支持的问题

 

Dell - Internal Use - Confidential 

万能的组：

 

一个客户需要再在VM WARE的虚拟机上使用GPU /nvida CUDA加速特性。请问VM是否支持，那个版本以上可以支持呀。

 

 

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 林雄                                                                                                                                                                                                                                                                    | Kean Lin                                                                                                                                     |
|                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                          |
| 解决方案技术顾问,企业级解决方案                                                                                                                                                                       |                                                                                                                                                                          |
|                                                                                                                                                                                                                                                                                                     | Dell \| Greater China Consumer and SMB                                                                |
| 戴尔[\| ]大中华区消费及中小企业部                                                                                                                                                                                                                                                               | Dell (China) Co Ltd,  Building HaiCang No.1, No.613 WuYuan Bay Bussiness Center, Si Shui Dao Road, Huli District, Xiamen, China,  Zip 361015 |
|                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                          |
| 电话 +86 0592 8186683, 传真 +86 0592 8196683                                                                                                                                                                                           | Customer feedback \| How am I doing? Please contact my manager [Nathan_Chen@dell.com](mailto:King_Wang@dell.com)                                |
|                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                          |
| 戴尔（中国）有限公司 中国厦门市湖里区泗水道613号五缘湾商务营运中心海沧大厦1号楼，邮编：361015 |                                                                                                                                                                          |
|                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                          |
|                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                          |
|                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                          |
| 客户反馈\| 我表现如何？请联系我的经理 [Nathan_Chen@dell.com](mailto:King_Wang@dell.com)                                                                                                                                                                |                                                                                                                                                                          |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 

 

 

已使用 OneNote 创建。
