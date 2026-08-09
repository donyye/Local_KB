Nutanix进程卡住

2018年3月22日

8:41

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: XC630\|SW ISSUE\|PROS\|SR: 962416086
    发件人     Lim, Beng Teng
    收件人     Ye, Dony
    抄送       Shek, Marcus
    发送时间   2018年3月21日 17:05
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Dony,

   

  Please follow the below step to clear the "hung task"

  1.  Login to a CVM
  2.  Type : acli
  3.  Type : task.list

  This will list all the task include failed and hung task. Find the task UUID that you wish to cancel.

  1.  Type : task.cancel task_list=\<task uuid\>

  This command will cancel the "hung task"

   

  Login to Prism UI to check again.

   

  ![[Technology_ALL_Nutanix_case_011_Nutanix进程卡住_001.png]]

  Type: exit

  To exit from \<acropolis\> command

   

   

  Warmest regards,

   

  Lim Beng Teng

  Master Engineer - APJ Solutions Support (SST)

  Dell EMC \| Remote Services -- APJ

  VCAP6-DCV \| VCP6-DTM \| VCP6-CMA \| RHCE \| RHCVA \| NSS \| NPP

   

  From: Ye, Dony

  Sent: Wednesday, March 21, 2018 5:04 PM

  To: Lim, Beng Teng \<Beng_Teng_Lim@Dell.com\>

  Cc: Shek, Marcus \<Marcus_Shek@Dell.com\>

  Subject: 答复: XC630\|SW ISSUE\|PROS\|SR: 962416086

   

  Dell - Internal Use - Confidential 

  Hi, BT

   

  Customer want to know how can we cancel/remove this task in Prism Web Console.

  ![[Technology_ALL_Nutanix_case_011_Nutanix进程卡住_002.jpg]]

   

   

  The NCC log at attached.

   

  B R

  Dony

   

  发件人: Lim, Beng Teng 

  发送时间: 2018年3月21日 16:59

  收件人: Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>; APJ_SST \<[APJ_SST@Dell.com](mailto:APJ_SST@Dell.com)\>

  抄送: CN XMN TS ENT L2 SME \<[CN_XMN_TS_ENT_L2_SME@Dell.com](mailto:CN_XMN_TS_ENT_L2_SME@Dell.com)\>; Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\>

  主题: RE: XC630\|SW ISSUE\|PROS\|SR: 962416086

   

  I got this

   

  Warmest regards,

   

  Lim Beng Teng

  Master Engineer - APJ Solutions Support (SST)

  Dell EMC \| Remote Services -- APJ

  VCAP6-DCV \| VCP6-DTM \| VCP6-CMA \| RHCE \| RHCVA \| NSS \| NPP

   

  From: Ye, Dony

  Sent: Wednesday, March 21, 2018 4:55 PM

  To: APJ_SST \<<APJ_SST@Dell.com>\>

  Cc: CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>

  Subject: XC630\|SW ISSUE\|PROS\|SR: 962416086

   

  Hi SST, I'd like to open a new case for following project related problem. Please assign an engineer to help handle this case. SR:962416086 Nutanix OS for VMware.

 

已使用 OneNote 创建。
