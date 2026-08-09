RE: workshop-share：OMNM+SNMP

Tuesday, November 05, 2013

8:42 AM

  -------------------------------------- ------------------------------------------------------------------------------------------
  主题       RE: workshop-share：OMNM+SNMP
  发件人     Wang, Xing Fang
  收件人     liang, changcai; CN XMN TS Networking; CCC XMN Enterprise ProSupport Storage
  抄送       Wang, Yaoting; Ma1, Joe; Zeng, Benjamin; Ding, Simon; Li3, David; CN XMN TS Server Coach
  发送时间   Monday, November 04, 2013 9:15 PM
  -------------------------------------- ------------------------------------------------------------------------------------------

 

Changcai

 

Good share!!

 

From: liang, changcai

Sent: Monday, November 04, 2013 8:46 AM

To: CN XMN TS Networking

Cc: Wang, Yaoting; Ma1, Joe; Zeng, Benjamin; Ding, Simon

Subject: workshop-share：OMNM+SNMP

 

Dell - Internal Use - Confidential 

 

OMNM

2013年11月1日

9:42

 

需求：

系统硬件：Dual core (2.8Ghz)、6GB RAM -64 Bit OS、100GB Disk Space、7200 RPM Disk

IE浏览器：Version 9  or  above

 

1、运行OMNM后，会有一个密码提醒的设置

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_001.jpg]]

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_002.jpg]]

 

2、 点击Resource Discovery可以添加设备到Managed Resouces里。

 

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_003.jpg]]

 

3、 点击Resource Discovery后弹出如下界面，设置交换机IP地址或地址段

 

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_004.jpg]]

 

4、设置Select  Authentication ，点击Create New，弹出如下界面

 

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_005.jpg]]

 

 

5、Authentication Name 可以随便设置个名称，Protocal Type可以选择很多，这里选择SNMPv2c。

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_006.jpg]]

 

 

6、选择SNMPv2c后，Authentication 项需要设置RW community。这里必须与交换机的设置一致。

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_007.jpg]]

 

 

7、在management 里确认port 为161。

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_008.jpg]]

 

8、点击Apply后出现下图界面，点击Execute。

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_009.png]]

 

10、点击Discover后开始搜索，搜索成功后会提示Success，并且没有报错。

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_010.png]]

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_011.png]]

 

如果交换机上没有配置SNMP，则会出现以下Authentication failure 错误。

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_012.png]]

 

 

11、成功Discover 到设备后，就可以将设备添加到Managed Resources。设备的Alarms 信息就会很直观的显示。

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_013.jpg]]

 

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_014.jpg]]

 

12、选中设备右键单击可以列出通过OMNM对设备所能实现的操作。

 

![Machine generated alternative text: Firmware Version 31.3.9 3.1.5.4 fr+x Software Version 3.13.9 315.4 ? ProScan fr + x O Search Namer Target(s) Overall Compliance Monitored Scheduled No data is available to display Network Tools fr x Managed Resources Home Pertorniance o\] Search Network Status Responding Responding a Model PowerConnect M6220 PowerConnect 1v16220 4\$ Actiors](attachments/Technology_ALL_未分类知识库_002_RE_%20workshop-share：OMNM+SNMP_015.jpg)

点击Actions后，列出能够对交换机进行的show、config、manage三项功能。

![[Technology_ALL_未分类知识库_002_RE_ workshop-share：OMNM+SNMP_016.png]]

 

 

交换机62xx系列交换机SNMP的配置示例：

 

console(config)# snmp-server community public RW

console(config)# snmp-server enable traps 

console(config)# snmp-server host 192.168.1.1 public  traps v2

console(config)# snmp-server host 192.168.1.1 public  informs

 

交换机55xx系列交换机SNMP的配置示例：

 

console(config)# snmp-server community public RW

console(config)# snmp-server enable traps 

console(config)# snmp-server host 192.168.1.1 version 2c public

 

 

注：192.168.1.1 即是安装OMNM服务器的地址，要确保OMNM服务器的地址和交换机的地址可以正常通信。

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

Created with Microsoft OneNote 2010

One place for all your notes and information

 

已使用 OneNote 创建。
