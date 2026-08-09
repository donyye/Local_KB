关闭ESXi密码复杂度审核

Friday, December 30, 2016

2:05 PM

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------
  主题       关闭ESXi密码复杂度审核
  发件人     Yin, Guoxun
  收件人     CN XMN TS ENT L2 SME
  发送时间   Friday, December 30, 2016 1:59 PM
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------

 

Team，

ESXi5/6强制开启密码复杂度审核，在一些特殊情景下我们必须改成弱密码，

经过反复测试以下步骤可以达成目标，原KB给出了字段意义介绍但是并没有给出关闭密码审核的推荐字， 有此特殊需求的请注意。

 

 

 

Bacnkup the /etc/pam.d/passwd file to passwd.bak

Command: cp /etc/pam.d/passwd  /etc/pam.d/passwd.bak

 

Change the related section in /etc/pam.d/passwd as the below picture

Command: vi /etc/pam.d/passwd

![[Technology_ALL_VMware_分析案例_042_关闭ESXi密码复杂度审核_001.png]]

 

已使用 OneNote 创建。
