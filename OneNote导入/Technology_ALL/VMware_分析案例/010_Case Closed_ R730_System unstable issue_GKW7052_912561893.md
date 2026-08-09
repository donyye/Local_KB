Case Closed: R730\|System unstable issue\|GKW7052\|912561893

Monday, July 20, 2015

2:28 PM

  -------------------------------------- ------------------------------------------------------------------------------------------
  主题       Case Closed: R730\|System unstable issue\|GKW7052\|912561893
  发件人     Lian, Wenxiang
  收件人     Lau, Kelvin
  抄送       CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   Monday, July 20, 2015 2:11 PM
  -------------------------------------- ------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Kelvin,

 

I had done this RA.

 

Problem reported:

New R730 system unstable.

1, ESXi 5.5 PSOD

![[Technology_ALL_VMware_分析案例_010_Case Closed_ R730_System unstable issue_G_001.png]]

 

2, iDRAC shows error: PCI 1318 Fatal error on Bus 0 device 1 function 0 

 

 

Solution:

It is a known issue related to PERC9 driver, please refer to KB: SLN294326

 

Root Cause:

The PERC 9 series storage controllers on the R920 & 13G Servers utilize the "megaraid_perc9" driver. VMware\'s installation media  -as well as older versions of Dell\'s customized ISO- do not have this driver and ESXi defaults to \"lsi_mr3\", which causes a number of issues.

 

Comments:

新机器出现问题时，一定要注意查找KB看是否有已知问题，避免WUR的风险。

 

 

 

Thanks & Regards,

 

Wenxiang Lian

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

 

From: Li, Jiangxiong

Sent: Wednesday, June 17, 2015 17:37

To: Lau, Kelvin; Lian, Wenxiang

Cc: CN XMN EEC HK; CN XMN TS ENT L2 SME

Subject: RE: R730\|System unstable issue\|GKW7052\|912561893

 

Dell - Internal Use - Confidential 

Wenxiang

Please help on this case

 

 

Li Jiangxiong

 

 

From: Lau, Kelvin

Sent: 2015年6月17日 17:32

To: CN XMN TS Server Escalation

Cc: CN XMN EEC HK; Lian, Wenxiang

Subject: R730\|System unstable issue\|GKW7052\|912561893

 

Dell - Internal Use - Confidential 

Dear L2 expert

Below case need your help :

Detail Symptom Descriptions:

New server show error : PCI 1318 Fatal error on Bus 0 device 1 function 0 

 

Wenxiang is following this case , please help assign to Wenxiang 

 

 

Kelvin Lau

Enterprise Engineer

Dell \| Enterprise Support Services, HK/Macau

HK: 29693196 Pro-Support: 29693187

Macau: 0800105 Pro-Support: 0800106

Email support: <CNXMNEECHK@Dell.com> 

How am I doing? Please contact my manager <M_GUO@dell.com> with any feedback.

[Dell TechDirect](http://www.techdirect.com/) \| Global portal to manage support cases and parts dispatching 

For efficient problem resolution, get started today!

[![[Technology_ALL_VMware_分析案例_010_Case Closed_ R730_System unstable issue_G_002.jpg]]](http://en.community.dell.com/)

 

[![[Technology_ALL_VMware_分析案例_010_Case Closed_ R730_System unstable issue_G_003.jpg]]](http://www.dell.com/support/home/hk/zh/hkbsd1/?c=hk&l=zh&s=bsd&~ck=mn)

 

 

已使用 OneNote 创建。
