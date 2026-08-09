Nutanix 初始化还原

Monday, June 27, 2016

10:06 AM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------
    主题       RE: X6300\|Product query\|PROS\|SW issue\|SR931448702\|ST:GNK54D2
    发件人     Yin, Guoxun
    收件人     Lin, Yongliang; Hong, Yzf
    抄送       CN XMN TS ENT L2 SME
    发送时间   Wednesday, June 22, 2016 1:49 PM
    附件       \<\<Deployment Guide.pdf\>\>
    -------------------------------------- -----------------------------------------------------------------------------------------------
  :::

   

  Hi YZF,

  请跟用户确认下是用哪种Hypervisor,  KVM? ESXi?  Hyper-V? 然后回复我。

   

  恢复至出厂状态的方式如下：

   

  1.  确认所有节点没有任何数据需要保留，
  2.  按顺序依次重启节点，进行factory reset，注意一个节点未做完恢复时，不要启动另外一个节点的恢复，不然会互相干扰
  3.  重启自检过程中，按F11，进入BOOT manager, 选择"one-shot BIOS boot menu", 然后启动设备为如下图标示的设备， 之后请按照附件手册第16页第7步开始做，一直到初始化完成，期间机器会重启若干次，直到提示初始化完成前，请务必不要中断初始化过程！

  ![[Technology_ALL_VMware_分析案例_030_Nutanix 初始化还原_001.jpg]]

   

   

   

   

   

  From: Lin, Yongliang

  Sent: 2016年6月22日 13:02

  To: Hong, Yzf \<yzf_hong@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: 答复: X6300\|Product query\|PROS\|SW issue\|SR931448702\|ST:GNK54D2

   

  Dell - Internal Use - Confidential 

  Hi guoxun:

   

  Help

   

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  \-\-\-\--邮件原件\-\-\-\--

  发件人: Hong, Yzf 

  发送时间: 2016年6月22日 12:16

  抄送: CN XMN TS Server Escalation ; Hong, Yzf 

  主题: X6300\|Product query\|PROS\|SW issue\|SR931448702\|ST:GNK54D2

   

  X6300\|Product query\|PROS\|SW issue\|SR931448702\|ST:GNK54D2

   

   

  a\. Detail Symptom Descriptions

  详细的故障现象描述:

   

  客户咨询如何恢复Nutanix 

   

  1，Nutanix 使用不了，第一次初始化安装软件运行不正常，虚拟化界面显示软件安装失败，虚拟机

  2，客户咨询 联系售前michel_zhao 18892092800 ，尝试还原出厂设置，但是没有效果，还原后Nutanix 系统消失了

   

   

  故障问题发生时间段:第一次开机部署系统的时候

  b\. Troubleshooting Steps

  详细的诊断步骤: 

  维修记录: (单号/更换的部件/更换后的现象)

  Bios/Driver/FW及存储控制器相关FW版本:

  c\. Current status

  客户公司名称:陕西瑞金电子科技有限公司 ，西安市高新区站八一路1号汇鑫IBC大厦B座708 

   

  /业务影响:无法部署业务

   

  /升级的原因: Nutanix 软件问题

  /RM/TAM: NA

   

  d.Must Collect Logs 已收集的日志(请上传至SR下):NA

 

已使用 OneNote 创建。
