Linux安装Megacli导致系统crash

2023年8月23日

15:03

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       R740XD\|PERC H750 after installed CentOS 7.9 and have[  I/O error \-\-\--LKB#000194516]
  发件人     Luo, Jeason
  收件人     CN XMN TS ENT L2 SME
  发送时间   2021年12月15日 15:53
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi All,

 

如有遇到PERC H750新装OS后不稳定或比较异常的情况，请先检查是否有装Megacli工具或加载对应的模块。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Summary: R740XD+PERC H750+RAID1(600G HD\*2) after installed CentOS 7.9 and have I/O error

 

Symptoms:

1. after installed CentOS 7.9 and have I/O error

 

![[Technology_ALL_Linux 问题收集_093_Linux安装Megacli导致系统crash_001.jpg]]

 

2.IDRAC GUI show alert

 

![[Technology_ALL_Linux 问题收集_093_Linux安装Megacli导致系统crash_002.jpg]]

 

3.TSR log check BP Status: Degraded, BP firmware was up to date

 

![[Technology_ALL_Linux 问题收集_093_Linux安装Megacli导致系统crash_003.jpg]]

 

![[Technology_ALL_Linux 问题收集_093_Linux安装Megacli导致系统crash_004.png]]

 

Resolution：

After reinstall OS again and the issue was gone. Confirm before installed Megacli tool, now uninstalled the Megacli tool is normal.

 

 

 再次重新安装操作系统后，问题消失了。 确认之前安装了Megacli工具，现在卸载了Megacli工具正常。

 

 

 

 

===========================

 

We had come across cases that the system with PERC H755/H750 with MegaCli installed will crash during gathering sosreport.

If you face similar issue, uninstall MegaCli (Install Perccli) or exclude the MegaCli plugin during sosreport.

\# sosreport \--skip-plugins megacli

 

You may refer to the Knowledge Base below for more information.

 

[PERC H750 Run Megacli Stops Responding in RHEL7.9 or RHEL8.3 \| Salesforce](https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P000000XvuJQAS/view)

<https://access.redhat.com/solutions/6981451>

 

 

 

已使用 OneNote 创建。
