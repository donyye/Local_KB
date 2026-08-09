GPU_K1_K2_issue

Monday, November 24, 2014

9:36 AM

  -------------------------------------- ------------------------------------------------------------------------------------
  主题       答复: CASE CLOSED FFV7D02-SR889986016 -R720 -GPU ISSUE
  发件人     Feng, Ethan
  收件人     Li, Jiangxiong; Wang, Andy King
  抄送       Ye, Dony
  发送时间   Tuesday, November 18, 2014 9:52 AM
  -------------------------------------- ------------------------------------------------------------------------------------

 

Jaingxiong：

 

看来内存是不满足的，LRDIMM的内存。

 

CPU: 115W

![[Technology_ALL_未分类知识库_026_GPU_K1_K2_issue_001.png]]

 

![[Technology_ALL_未分类知识库_026_GPU_K1_K2_issue_002.png]]

 

Memory：8x16GB LRDIMM

![[Technology_ALL_未分类知识库_026_GPU_K1_K2_issue_003.png]]

 

![[Technology_ALL_未分类知识库_026_GPU_K1_K2_issue_004.png]]

 

 

发件人: Li, Jiangxiong 

发送时间: 2014年11月18日 9:38

收件人: Wang, Andy King; Feng, Ethan

抄送: Ye, Dony

主题: 答复: CASE CLOSED FFV7D02-SR889986016 -R720 -GPU ISSUE

 

K2也不支持linux+KVM啊

![[Technology_ALL_未分类知识库_026_GPU_K1_K2_issue_005.png]]

特别要注意单独加GPU的case，有些情况是不能加的

比如CPU功率大于115W不支持加GPU

LRDIMM也不支持加GPU

这2个条件最容易被忽视的

 

 

 

发件人: Wang, Andy King 

发送时间: 2014年11月17日 19:14

收件人: Feng, Ethan

抄送: Ye, Dony; Li, Jiangxiong

主题: FW: CASE CLOSED FFV7D02-SR889986016 -R720 -GPU ISSUE

 

Ethan,

 

这个case和你遇到的故障应该是一致的，请核对。另外，K2不支持Linux系统，再和jiangxiong确认下。

 

Andy

 

From: Li, Jiangxiong

Sent: Tuesday, April 8, 2014 10:31 AM

To: Yang, Bright; Zhu, Hill

Cc: CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT; Wang, Xing Fang; Tang, Chris

Subject: CASE CLOSED FFV7D02-SR889986016 -R720 -GPU ISSUE

 

Dell Customer Communication

Hi Bright

The case had been closed

 

Solution: 此GPU不支持Linux系统

 

故障现象：

1.       用户出厂配了nvidia k1 gpu,打算装centos6.5

2.    到安装OS引导检查硬件部分，系统卡住，小液晶屏会报 pci1360 bus error on slot 6 reset pci card ,pci 1320 bus0device3fution 错误

3.    安装GPU Driver之后无法配置Xserver

 

处理方案：

1.    在安装OS的时候需要选择basic video driver，然后手动手动安装GPU Driver，这个是Linux的一个bug，[http://us.download.nvidia.com/XFree86/Linux-x86_64/319.32/README/commonproblems.html](http://us.download.nvidia.com/XFree86/Linux-x86_64/319.32/README/commonproblems.html)

2.    选择basic video driver安装并手动安装GPU Driver之后，无法配置Xserver，是由于这块K1的卡没有认证Linux系统，包括NVIDIA官网也没有认证，Vbios不支持，请参考附件和下面链接：[http://www.nvidia.com/content/grid/resources/NVIDIA_DELL_GRID_DS_AUG13_A4_LR.pdf](http://www.nvidia.com/content/grid/resources/NVIDIA_DELL_GRID_DS_AUG13_A4_LR.pdf)

![[Technology_ALL_未分类知识库_026_GPU_K1_K2_issue_006.png]]

 

 

 

Case Closed 邮件保存路径：

[http://moss.ap.dell.com/sites/CCC%20PLE%20Enterprise%20L2%20Portal/L2%20Closed%20Case%20Summary/default.aspx](http://moss.ap.dell.com/sites/CCC%20PLE%20Enterprise%20L2%20Portal/L2%20Closed%20Case%20Summary/default.aspx)

 

 

 

发件人: Lv, Zhiwei 

发送时间: Monday, February 24, 2014 2:03 PM

收件人: Li, Jiangxiong

抄送: Yang, Bright; CN XMN TS Server Coach

主题: 答复: R720\|GPU issue\|PSP\|SR 889986016

 

Dell Customer Communication

Hi Jiangxiong

Please help on this case.

 

Bright

針對下面這個問題,把之前給客戶solution的Sales, TSR也一起loop 進來.

 

3.董先生反映，购买之前跟我们确认可支持CENTEROS 6.5

 

Regards

Zhiwei Lv

Enterprise Product Engineer

 

\-\-\-\--邮件原件\-\-\-\-- 

发件人: Deltamail_prod 

发送时间: 2014年2月21日 17:08 

收件人: CN XMN TS Server Escalation 

抄送: Yang, Bright 

主题: R720\|GPU issue\|PSP\|SR 889986016 

 

1.用户出厂配了nvidia k1 gpu,打算装centos6.5 

2.到安装OS引导检查硬件部分，系统卡住，小液晶屏会报 pci1360 bus error on slot 6 reset pci card ,pci 1320 bus0device3fution 错误；

3.bios 2.1.3

confirm with SI 董先生 

1.目前无法插拔操作或做进一步日志收集 

2.需要我们进一步协助给出肯定的操作建议 

3.董先生反映，购买之前跟我们确认可支持CENTEROS 6.5 

esc to l2 for solution

 

已使用 OneNote 创建。
