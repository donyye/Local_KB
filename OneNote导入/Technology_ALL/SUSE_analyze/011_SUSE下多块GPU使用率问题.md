SUSE下多块GPU使用率问题

Friday, December 23, 2016

9:55 AM

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------
  主题       CASE CLOSED\|Normal Escalation\|C4130\|GPU issue\|PSP\|Email\|SR:939342066\|ST:5FG0LF2
  发件人     Chen9, Jack
  收件人     Feng, Ethan
  抄送       CN XMN TS ENT L2 SME; CN XMN GSD TS ESG MGMT; CN XMN TS Server Coach
  发送时间   Thursday, December 22, 2016 2:00 PM
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

 Hi Ethan,

 

I closed the case.

Issue:

GPU issue.

Solution: 

Don\'t replace any.

Root Cause:

N/A.

Comments:

 

On Linux, you can set GPUs to persistence mode to keep the NVIDIA driver loaded even when no applications are accessing the cards. This is particularly useful when you have a series of short jobs running. Persistence mode uses more power, but prevents the fairly long delays that occur each time a GPU application is started. It is also necessary if you've assigned specific clock speeds or power limits to the GPUs (as those changes are lost when the NVIDIA driver is unloaded). Enable persistence mode on all GPUS by running:

nvidia-smi -pm 1

 

 

Closed email link：

<http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

 

Jack Chen

2016-12-22

 

 

 

From: Lin, Yongliang

Sent: Thursday, December 01, 2016 8:56 AM

To: Feng, Ethan \<Ethan_Feng@DELL.com\>; Chen9, Jack \<Jack_Chen9@Dell.com\>

Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>; Huang, Julia \<Julia_Huang@Dell.com\>; Lin, Octopus \<octopus_lin@Dell.com\>; Luo, David \<David_Luo@DELL.com\>

Subject: 答复: C4130\|GPU issue\|PSP\|Email\|SR 939342066

 

Dell - Internal Use - Confidential 

Jack:

 

Help.

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

\-\-\-\--邮件原件\-\-\-\--

发件人: Feng, Ethan 

发送时间: 2016年12月1日 8:40

抄送: CN XMN TS Server Escalation ; Feng, Ethan ; Huang, Julia ; Lin, Octopus ; Luo, David 

主题: C4130\|GPU issue\|PSP\|Email\|SR 939342066

 

C4130上有4块儿NVIDIA M40显卡（我们暂时取下两块儿），

发现在GPU完全不用的情况下，其中一块儿显卡的处理单元始终被占满，重启也解决不了问题，

 

OS Open SUSE 11.3，没有使用GPU的情况下，

如果只接2个GPU，GPU2的处理单元显示为97%，另外一个显示为0，

如果4个GPU全上满，显示为编号4的GPU处理单元占满，

 

客户要求分析原因，升级L2协助

 

日志存放：\\\\xmntsdb03\\EntTS_Log\\939342066

 

 

 

公司：华为技术有限公司，

客户：杜勇，18311087906

TAM： David Luo

 

已使用 OneNote 创建。
