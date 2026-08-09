CPU IERR

Tuesday, November 01, 2016

9:16 AM

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------------------------
    主题       Case Closed\|Normal Escalation\|R910\|CPU issue\|PROS\|sr:935754763
    发件人     Lin, Yongliang
    收件人     Li, Rentian
    抄送       CN XMN GSD TS ESG MGMT; CN XMN TS Server Coach
    发送时间   Monday, October 31, 2016 6:25 PM
    -------------------------------------- -------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Hi Rentian:

   

  I have done the RA .

   

  Issue:

  R910 has four CPU and show machine check error detected and an internal error (IERR)

   

  Solution: 

  1.  Check SEL Log:

  090A == 0000 1001 0000 1010

   

  Bus and Interconnect Errors

  000F 1PPT RRRR IILL

  PP=00:Local processor\* originated request

  T=1:REQUEST TIMED OUT

  RRRR=0000:Generic Error

  II=10: I/O

  LL=10: Level 2

   

  0400 == 0000 0100 0000 0000

  Internal Timer Error

   

  0F0F== 0000 1111 0000 1111

  Bus and Interconnect Errors

  000F 1PPT RRRR IILL

  PP=11:Generic

  T=1: REQUEST TIMED OUT

  RRRR=0000:Generic Error

  II=11: Other transaction

  LL=11: Generic

  1.  Suggest take CPU/MB/DIMM to check .
  2.  Upgrade MB/IDARC FW and disable C1E/C status config.

   

  Root Cause:

  Replace CPU4 and MB

   

  Comments:

  NA

   

  Closed email link：

  <http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

   

   

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

  发件人: Lin, Yongliang 

  发送时间: 2016年9月26日 9:36

  收件人: Li, Rentian \<Rentian_Li@Dell.com\>

  抄送: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  主题: 答复: R910\|CPU issue\|PROS\|sr:935754763

   

  Dell - Internal Use - Confidential 

  Hi Rentian:

   

  I will follow up.

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  \-\-\-\--邮件原件\-\-\-\--

  发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\] ]

  发送时间: 2016年9月26日 8:59

  抄送: CN XMN TS Server Escalation ; Li, Rentian 

  主题: R910\|CPU issue\|PROS\|sr:935754763

   

  Detail Symptom Descriptions：

   

  R910 4个cpu都报cpuierr错误

  machine check error detected

  an internal error (IERR)

  9/12 客户放静电，关闭节能 bios已为最新 清除日志

  9/22 机器再次报4个cpuierr

 

已使用 OneNote 创建。
