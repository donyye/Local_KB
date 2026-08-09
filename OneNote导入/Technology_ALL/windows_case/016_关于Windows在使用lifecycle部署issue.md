关于Windows在使用lifecycle部署issue

Wednesday, October 14, 2015

4:53 PM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       CASE CLOSED：R730XD\|ST：5VQ7172\|SR:917752777\|OS install\|PROS\|
    发件人     Mo, Jin
    收件人     Zhao, Cathy
    抄送       CN XMN TS ENT L2 SME; CN XMN GSD TS ESG MGMT; CN XMN TS Server Coach; Zeng, Mars
    发送时间   Wednesday, October 14, 2015 3:55 PM
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Hi Cathy：

  I closed the case.

  Issue:

  通过Lifecycle 部署系统，放入系统光盘，拷贝文件100%之后报错，提示：windows installation cannot continue because a required driver could not be installed.

  Root Cause:

  这是Win2008 OS的限制，RAM驱动空间默认大小是32M,     13G服务器安装有PCI的设备时（HBA），驱动程序大小超过了32M，因此导致OS安装失败。

  Solution:

  1.  移除所有的PCI设备(HBA)或者在BIOS里禁用PCI设备所在的相关插槽；
  2.  修改Win2008 安装镜像RAM驱动空间的大小；

   

    具体方法如下：

     修改RAM大小为256M或者更大：

      1.Extract the W2K8R2SP1 ISO image to D:\\W2K8R2SP1.

      2.Create a folder-mount in D:\\

      3.Copy boot.wim from D:\\W2K8R2SP\\source to D:\\

      4.open command line with adminitrator right and goto D:\\ drive   

      5.dism /mount-wim /wimfile:boot.wim /index:2 /mountdir:mount

      6.dism /image:D:\\mount /set-scratchspace:256

      7.dism /unmount-wim /mountdir:D:\\mount /commit

      8.copy boot.wim to D:\\W2K8R2SP1\\source folder

      9.rebuild ISO file.  oscdimg -u2 -m -bD:\\W2K8R2SP1\\boot\\etfsboot.com D:\\W2K8R2SP1 D:\\CW2K8R2SP1.ISO

   

  Closed email link：

  <http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

   

  Thanks

  Mojin

   

   

  From: Ruan, Garuda

  Sent: Tuesday, September 29, 2015 5:01 PM

  To: Mo, Jin \<<Jin_Mo@Dell.com>\>

  Cc: Wang, Andy King \<<Andy_King_Wang@dell.com>\>; Li, Jiangxiong \<<Jiangxiong_Li@DELL.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Zeng, Mars \<<Mars_Zeng@Dell.com>\>; Zhao, Cathy \<<Cathy_zhao@DELL.com>\>

  Subject: RE: R730XD\|ST：5VQ7172\|SR:917752777\|OS install\|PROS\|

   

  Dell - Internal Use - Confidential 

  Hi Jin,

   

  Please help on this case. Thanks.

   

  B.R

  Garuda

  \-\-\-\--邮件原件\-\-\-\--

  发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\] ]

  发送时间: 2015年9月29日 16:25

  收件人: Zhao, Cathy

  抄送: CN XMN TS Server Escalation; Zeng, Mars

  主题: R730XD\|ST：5VQ7172\|SR:917752777\|OS install\|PROS\|

   

  R730XD\|ST：5VQ7172\|SR:917752777\|OS install\|PROS\|

  1.客户采用UEFI模式安装windows 2008 R2

  2. Lifecycle 部署完成，放入系统光盘，拷贝文件100%之后，提示：windows installation cannot continue because a required driver could not be installed.

  3.客户拒绝更换光盘测试，抱怨TS技术水平差，要求oncall L2 处理

  4.客户自己尝试更新最新的os deploy package, same issue.

   

  5.升级RM建议先升级L2 确认是否能oncall cust. 解决问题

   

  升级L2. 由于客户不接受L1的支持，要求直接与二线工程师沟通，是否能把客户接上线后，oncall l2 给客户解释

 

已使用 OneNote 创建。
