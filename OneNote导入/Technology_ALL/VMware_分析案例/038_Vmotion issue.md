Vmotion issue

Wednesday, December 07, 2016

11:18 AM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------
    主题       RE: R710\|Product query\|PROS\| SR：939782947 
    发件人     Yin, Guoxun
    收件人     S, Youzhi; Zhuang, Jarvis
    抄送       CN XMN TS ENT L2 SME; Wang, Andy King
    发送时间   Wednesday, December 07, 2016 11:13 AM
    -------------------------------------- -----------------------------------------------------------------------------------------------------------
  :::

   

  两位:

  如下面所说，请确认下是否为OEM，这是 基本的检查和sense.

   

  就下面说的问题，重置BIOS是无法解决问题。

   

  建议如下:

  1.  vMotion所涉及的源机和目的及是否为同一代机器？  CPU是否为同型号(V3/V4)？
      a.  若机型相同，CPU也为同型号，同一代，那么请检查源机和目标机的BIOS中的每一项设置，都必须一致。 同时BIOS版本也尽量保持一致。
      b.  若机型不同，CPU非同型号或者同型号但是不同代，那么除了要做到上述的要求外，必须在Cluster中开启EVC模式才可以做vmotion，如果是 OEM  vSphere，我们全程support，如果非OEM，建议客户找Vender Support.

   

   

   

   

  From: Wang, Andy King

  Sent: 2016年12月7日 11:05

  To: S, Youzhi \<Youzhi_S@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: Zhuang, Jarvis \<Jarvis_Zhuang@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: RE: R710\|Product query\|PROS\| SR：939782947 

   

  Dell - Internal Use - Confidential 

  Youzhi,

   

  请确认是否有OEM?

  如果没有OEM，建议客户联系vmware获取软件方面的技术支持或网络查询相关资料。

   

  Guoxun，

   

  FYI

   

  Andy

   

  From: S, Youzhi

  Sent: Wednesday, December 7, 2016 10:58 AM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: S, Youzhi \<<Youzhi_S@Dell.com>\>; Zhuang, Jarvis \<<Jarvis_Zhuang@Dell.com>\>

  Subject: R710\|Product query\|PROS\| SR：939782947 

   

  Dell - Internal Use - Confidential 

  Dear L2,

  Below case need your help.

   

  a.     Detail Symptom Descriptions

  详细的故障现象描述:

  VMware esxi 6.0 OS

  vmotion 時, 出現下列訊息:

  ![[Technology_ALL_VMware_分析案例_038_Vmotion issue_001.jpg]]

   

  b.    Troubleshooting Steps

  详细的诊断步骤:

  BIOS版本:6.4.0

  CPU型號：E5520

  VM ESXI：ESXI 6.0 U2

  重置了BIOS，same issue

  Check F2---processor setting\-\--virtualization technology  enable。 

  c.     Current status

  客户公司名称:/业务影响:/升级的原因:/RM/TAM:

   

  d.     Must Collect Logs

  已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

  常见日志类型参考(根据实际情况获取相应日志)：

  服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

  存储(参考)：MD/EQL/NAS/CML/DR/DL log;

   

   

  Sun, Youzhi

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

  How am I doing?E-mail my manager at [Mars_Zeng@dell.com](mailto:Mars_Zeng@dell.com)

  戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问

  [www.dell.com.cn/home](http://www.dell.com.cn/home)

  ::: 
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    [![[Technology_ALL_VMware_分析案例_038_Vmotion issue_002.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[Technology_ALL_VMware_分析案例_038_Vmotion issue_003.gif]]](http://www.weibo.com/techsupportdell)   [![[Technology_ALL_VMware_分析案例_038_Vmotion issue_004.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[Technology_ALL_VMware_分析案例_038_Vmotion issue_005.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

   

 

已使用 OneNote 创建。
