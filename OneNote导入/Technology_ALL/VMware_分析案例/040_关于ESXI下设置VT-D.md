关于ESXI下设置VT-D 

Wednesday, December 14, 2016

9:19 AM

  -------------------------------------- ----------------------------------------------------------------------
  主题       RE: R730\|CPU issue\|PROS\|SR:939782755 
  发件人     Yin, Guoxun
  收件人     Wang, Andy King; Chen, Ivan
  抄送       CN XMN TS ENT L2 SME
  发送时间   Wednesday, December 14, 2016 8:26 AM
  -------------------------------------- ----------------------------------------------------------------------

 

Hi  Ivan,

像下面说的这种虚拟机里面又嵌套KVM的应用方式也就是Nested Hypervisor 仅供自己实验，这是没有任何技术支持的，

ESXi下的的VM BIOS中并无VT-D的设置。

 

BR.

Guoxun.

From: Wang, Andy King

Sent: 2016年12月14日 8:24

To: Chen, Ivan \<Ivan_Chen@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: RE: R730\|CPU issue\|PROS\|SR:939782755

 

Dell - Internal Use - Confidential 

Guoxun,

 

Please help

 

Andy

From: Chen, Ivan

Sent: Tuesday, December 13, 2016 7:55 PM

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Subject: R730\|CPU issue\|PROS\|SR:939782755

 

Dell - Internal Use - Confidential 

Detail Symptom Descriptions

详细的故障现象描述: 

客户在esxi下运行rh_linux, 在linux下再运行另一个虚机，需要打开VT-D这个技术，但是在bios中没的找到相应的选项。现在第二层的虚机打不开。

客户已经在BIOS中打开了虚拟技术支持项。

客户用CPU-Z查看，没有显示VT-X, VT-D, 询问如何确认？

下图是服务器与笔记本的对比。

![[Technology_ALL_VMware_分析案例_040_关于ESXI下设置VT-D_001.jpg]]

  

![[Technology_ALL_VMware_分析案例_040_关于ESXI下设置VT-D_002.jpg]]

 

 

 

 

 

已使用 OneNote 创建。
