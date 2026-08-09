RE: centos7.6 open-vm-tools 版本导致的异常

2019年4月15日

10:39

  ----------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject       RE: centos7.6 open-vm-tools 版本导致的异常
  From          Xiong, John
  To            Yin, Guoxun; CCC XMN Enterprise ProSupport SH; CN XMN TS ENT L2 SME
  Cc            Zhang, Janice
  Sent          2019年4月15日 9:54
  Attachments   \<\<Install or Upgrade open-vm-tools on CentOS7.6 .pdf\>\>
  ----------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Internal Use - Confidential

 

Attach the installation documentation.

 

 

From: Yin, Guoxun

Sent: Thursday, April 11, 2019 10:36

To: CCC XMN Enterprise ProSupport SH; CN XMN TS ENT L2 SME; Xiong, John

Cc: Zhang, Janice

Subject: FW: centos7.6 open-vm-tools 版本导致的异常

 

Thanks John for the wonderful investigation and sharing!!

Also CC team for reference.

 

 

 

BR.

Guoxun.

From: Xiong, John

Sent: 2019年4月11日 9:02

To: Yin, Guoxun

Subject: centos7.6 open-vm-tools 版本导致的异常

 

Internal Use - Confidential

 

Dear Guo Xun，

 

故障现象

open-vm-tools 10.2.5/CentOS7.6/Esxi 6.7 会出现下图的报错

![[Technology_ALL_VMware_分析案例_100_RE_ centos7.6 open-vm-tools 版本导致的异常_001.png]]

RHEL 官方说明

[https://bugzilla.redhat.com/show_bug.cgi?id=1672087](https://bugzilla.redhat.com/show_bug.cgi?id=1672087)

 

解决方案

1.临时修改方法

/etc/centos-release，7.6.1810用7.6替换

2.升级open-vm-tools

Open-vm-tools 的修复说明

 

![[Technology_ALL_VMware_分析案例_100_RE_ centos7.6 open-vm-tools 版本导致的异常_002.png]]

[https://github.com/vmware/open-vm-tools/commit/ba83c29fcd703ecb6a13a7767bad180033234aea](https://github.com/vmware/open-vm-tools/commit/ba83c29fcd703ecb6a13a7767bad180033234aea)

 

Best Regards

 

John Xiong(熊伟)

Enterprise Product Engineer, Infrastructure & Client Solutions Support

Dell EMC \| Support and Deployment Services

[John_Xiong@dell.com](mailto:John_Xiong@dell.com)

中文官方技术支持网站：[http://support.dell.com](http://support.dell.com/)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

如果您对我的服务有任何意见或建议,也可以联系我的经理wei_wang9@dell.com

![[Technology_ALL_VMware_分析案例_100_RE_ centos7.6 open-vm-tools 版本导致的异常_003.png]]

Please consider the environment before printing this email

 

 

已使用 OneNote 创建。
