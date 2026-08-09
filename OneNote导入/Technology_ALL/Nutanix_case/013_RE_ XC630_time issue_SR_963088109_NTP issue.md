RE: XC630\|time issue\|SR:963088109\|NTP issue

2018年4月16日

12:25

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: XC630\|time issue\|SR:963088109\|NTP issue
    发件人     Huang, Antti
    收件人     Wang, Zhen1; Li, Lemon; APJ_SST
    抄送       CN XMN TS ENT L2 SME; Wang, Xing Fang; Xu, Meteor
    发送时间   2018年4月16日 11:21
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Zhen, Lemon,

   

  The Nutanix NTP issue was fixed while the webex today morning. Here share the details to you, also share to others once you encounter LINUX as a NTP client to sync to Windows NTP server, here is the t/s step and fix.

   

  Symptom:

   

  Linux NTP client failed to sync to Windows NTP server. ( no \"\*\" ahead of the NTP server IP means currently not synced)

  ![[Technology_ALL_Nutanix_case_013_RE_ XC630_time issue_SR_963088109_NTP iss_001.png]]

   

   

  t/s steps:

  1.  Go into ntpq console.
  2.  type: associations                                            \# to get association ID
  3.  type: rv \<assid\>                                                \# to get details of ntp server
  4.  find \"rootdisp\" number, if the number is higher than 1000, that will be rejected

  ![[Technology_ALL_Nutanix_case_013_RE_ XC630_time issue_SR_963088109_NTP iss_002.png]]

   

   

  fixed:

   

  Check Windows NTP server register key as following:

   

  HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config\\LocalClockDispersion

   

  If the value is the following, it can be concluded that the DC is not configured to report itself as a time source with the accuracy required by NTP:

  dword:0000000a.

   

  run the following command on ntp window server, it will set the reg key to 0

  w32tm /config /localclockdispersion:0 /update

   

  After set, check the rootdisp again in linux ntp client, if less than 1000, will means fixed, just wait for several minutes, linux client will be synced as following. ( a \* ahead of the ntp server IP )

  ![[Technology_ALL_Nutanix_case_013_RE_ XC630_time issue_SR_963088109_NTP iss_003.png]]

   

  ![[Technology_ALL_Nutanix_case_013_RE_ XC630_time issue_SR_963088109_NTP iss_004.png]]

   

   

   

  Regards,

   

  Antti Huang

  Software & Solutions Master Engineer

  APJ Solutions Support Team (SST)

  RHCE7 \| RHCE Openstack \| SAP HANA Specialist \|

  Nutanix NPP5 \| CCAH Hadoop \| MCSE \| VCP \| ITIL \| JNCIA

  Dell EMC \| Global Support & Deployment

   

  From: Wang, Zhen1

  Sent: Friday, April 13, 2018 11:16

  To: APJ_SST

  Cc: CN XMN TS ENT L2 SME; Wang, Xing Fang; Xu, Meteor

  Subject: XC630\|time issue\|SR:963088109\|NTP issue

   

   Hi SST, I'd like to open a new case for following project related problem. Please assign an engineer to help handle this case. SR: 963088109 Nutainx OS for Linux.

   

  "FAIL: The hypervisor is not synchronizing with any NTP server

  "

   

  BR。

  Wang Zhen \|王 震

  Enterprise Product Engineer

  Ent Tech Support, Great China Customer Support Services

  Dell EMC \| Support and Deployment Services Tech Center Server

  How am I doing? Please contact my <managerXing_Fang_wang@Dell.com>

   

   

 

已使用 OneNote 创建。
