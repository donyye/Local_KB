RE: 关闭ESXi密码复杂度审核

Wednesday, February 22, 2017

2:41 PM

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------
  主题       RE: 关闭ESXi密码复杂度审核
  发件人     Yin, Guoxun
  收件人     CN XMN TS ENT L2 SME
  发送时间   Wednesday, February 22, 2017 12:36 PM
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------

 

Team,

之前在6.0上的使用弱密码的办法已经不适用于6.5了，  6.5上实测下面的修改可以继续使用password之类的弱密码。

 

![[Technology_ALL_VMware_分析案例_048_RE_ 关闭ESXi密码复杂度审核_001.png]]

 

BR.

Guoxun.

From: Yin, Guoxun

Sent: 2016年12月30日 13:59

To: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: 关闭ESXi密码复杂度审核

 

Team，

ESXi5/6强制开启密码复杂度审核，在一些特殊情景下我们必须改成弱密码，

经过反复测试以下步骤可以达成目标，原KB给出了字段意义介绍但是并没有给出关闭密码审核的推荐字， 有此特殊需求的请注意。

 

 

 

Bacnkup the /etc/pam.d/passwd file to passwd.bak

Command: cp /etc/pam.d/passwd  /etc/pam.d/passwd.bak

 

Change the related section in /etc/pam.d/passwd as the below picture

Command: vi /etc/pam.d/passwd

![[Technology_ALL_VMware_分析案例_048_RE_ 关闭ESXi密码复杂度审核_002.png]]

 

已使用 OneNote 创建。
