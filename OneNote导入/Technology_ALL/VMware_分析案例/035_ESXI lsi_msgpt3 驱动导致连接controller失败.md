ESXI lsi_msgpt3 驱动导致连接controller失败    

Wednesday, August 24, 2016

2:00 PM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       Case closed \|Know issue&Hot case \|ND3420\|ST:GTXXLD2\|path issue\|pros[  12GB SAS card issues with VMware ESX/ESXi 5.x & 6.x hosts]
    发件人     Xu, Xiaoming
    收件人     Tang, Keith
    抄送       CN XMN GSD TS ESG MGMT; CN XMN TS Server Coach; CCC Ent ProSupport Storage Agent Group
    发送时间   Wednesday, August 24, 2016 1:43 PM
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Keith:

     The case close now.

   

  Issue:

     Some SAS ports no function on controller.

   

  Root caused:

      It was know issue for 12GB with Vmware, need change the SAS driver from lsi_msgpt3 to mpt3sas.

   

  Analyzing:

  1.  Run "esxcli storage core adapter list", we found the DELL 12Gbps HBA driver were "lsi_msgpt3".

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_001.png]]

  1.  Download the mpt3sas driver and upload to system, then install it.

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_002.png]]

  1.  Change the driver from lsi_msgpt3 to mpt3sas.

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_003.png]]

  1.  Reboot the system, check it back to mpt3sas now, and the issue was gone.

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_004.png]]

   

  KB: SLN291842

  12GB SAS card issues with VMware ESX/ESXi 5.x & 6.x hosts

  ::: 
    --------------------------------------------- ---- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------
    Doc ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 SLN291842
    Version:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               8.0
    Status:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Published
    Published date:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        07/26/2016
    Updated:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               07/26/2016
    Categories:            [PowerVault MD3820f](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3820F) , [PowerVault MD3400](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3400) , [PowerVault MD3420](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3420) , [PowerVault MD3860i](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3860I) , [PowerVault MD3800i](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3800I) , [PowerVault MD3460](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3460) , [PowerVault MD3800f](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3800F) , [PowerVault MD3820i](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3820I) , [PowerVault MD3860f](http://kb.dell.com/infocenter/index?page=content&channel=SOLUTION&cat=ALL__PRODUCTS_ESUPRT_SER_STOR_NET_ESUPRT_POWERVAULT_POWERVAULT__MD3860F)    
    Available To:          Dell Internal                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
    Author:                [anthony_acosta](http://kb.dell.com/infocenter/index?page=user_profile&user=0fe181e01c0e432bb699b2fd1f06c379&rp=content&id=SLN291842&actp=SEARCH&viewlocale=en_US&searchid=1472001590639)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
    --------------------------------------------- ---- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------
  :::

   

  Description

  Issues observed that relate to this article:

  1.  The 12GB SAS card provided with the MD34xx doesn't appear within ESXi under the storage adapters. 
  2.  Intermittent connection to storage from the host. 
  3.  Single path available even though multiple cables/multiple HBA\'s used. 

   

  There are 2 possible cards shipped with Dell systems (or sold ala carte):

  - LSI 9300-8e, validated with 12Gen and below systems. 
    - Dell part numbers, 156NC & J91FN 
  - Dell 12Gbps SAS, validated with 13Gen systems. 
    - Dell part numbers, T93GD (Low profile) & 2PHG9 (Full height) 

  Solution

  For ESXi 5.5 and ESXi 6, using the latest version of the msgpt3 driver has a high success rate in resolving this issue for the customer. Since the msgpt3 driver is a Native Driver, it is preferred over the mpt3sas driver. However, in some instances the mpt3sas driver may be needed to resolve the issue. More information on \'Native Drivers\' can be found here: <http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2044993>

   

  You can check which driver versions are loaded by running the following commands via SSH to the host:

  esxcli software vib list \| grep mpt3sas

  esxcli software vib list \| grep msgpt3

   

  For the most current drivers available check the driver page: (Must use VMware credentials to download the files)

   

  LSI 9300-8e

  <http://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=34317&deviceCategory=io&details=1&keyword=9300&vioSolutions=Standard%20-%20IO%20Devices&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc>

   

  Dell 12Gbps SAS

  <https://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=34856&deviceCategory=io&details=1&VID=1000&DID=0097&SVID=1028&SSID=1f46&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc>

   

  How to install the (async) drivers in ESXi 5.x & 6.x: 

  <http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2005205>

  - Multiple methods listed with links depending on the circumstances. 
  - Suggested method: 
    - [https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2137853](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2137853) 

  How to install the drivers in ESXi 4.x:

   <http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1032936>

  - Suggested method outlined in section labeled' Existing ESXi/ESX installation using esxupdate and Datastore Browser' 

   

  After installing the driver and rebooting the host:

  - Navigate to the Configuration tab \> Storage Adapters link. 
  - You will see one of 2 names listed depending on the card used: 
    - Avago (LSI)3008 
    - Dell 12Gbps SAS HBA external 

   

  NOTE: 

  If the customer is running ESXi 5.5 or higher AND  the mpt3sas driver was installed earlier. Its required to disable the lsi_msgpt3 native driver in order to use the mpt3sas driver.

  run the following command then reboot the system:

   

  esxcli system module set \--enabled=false \--module=lsi_msgpt3

  Internal Notes

  Several times under ESXi 5.5 & 6.x the customer has added in the correct "mpt3sas" driver but is unable to see any mapped drives from storage.  The card appears as a storage adapter with a vmhba number.  When you look closely, however, you see that the driver in use is not "mpt3sas" but rather "lsi_msgpt3". 

   

  You can confirm the issue by checking the following file in a VM support bundle:

                  commands/localcli_storage-core-adapter-list.txt

  Or by running the following command on the host:

                  esxcli storage core adapter list

   

  You may see output like the following:

   

  \$ cat localcli_storage-core-adapter-list.txt

  HBA Name  Driver        Link State  UID                   Description

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  vmhba38   ahci          link-n/a    sata.vmhba38          (0:0:31.2) Intel Corporation Patsburg 6 Port SATA AHCI Controller

  vmhba39   ahci          link-n/a    sata.vmhba39          (0:0:31.2) Intel Corporation Patsburg 6 Port SATA AHCI Controller

  vmhba0    ahci          link-n/a    sata.vmhba0           (0:0:31.2) Intel Corporation Patsburg 6 Port SATA AHCI Controller

  vmhba1    megaraid_sas  link-n/a    unknown.vmhba1        (0:3:0.0) LSI / Symbios Logic Dell PERC H710P Mini

  vmhba2    lsi_msgpt3    link-n/a    pscsi.vmhba1          (0:4:0.0) LSI LSI Logic Fusi on-MPT 12GSAS SAS30008 PCI-Express

  vmhba32   usb-storage   link-n/a    usb.vmhba32           () USB

  vmhba33   usb-storage   link-n/a    usb.vmhba33           () USB

  vmhba34   usb-storage   link-n/a    usb.vmhba34           () USB

  vmhba35   ahci          link-n/a    sata.vmhba35          (0:0:31.2) Intel Corporation Patsburg 6 Port SATA AHCI Controller

  vmhba36   ahci          link-n/a    sata.vmhba36          (0:0:31.2) Intel Corporation Patsburg 6 Port SATA AHCI Controller

  vmhba37   ahci          link-n/a    sata.vmhba37          (0:0:31.2) Intel Corporation Patsburg 6 Port SATA AHCI Controller

  Note that the driver in use for vmhba2 shows up as "lsi_msgpt3".  

   

  That driver is part of the newer native drivers available in ESXi 5.5.  See <http://kb.vmware.com/kb/2044993> for more information. The native driver has priority over the "mpt3sas" driver. 

   

  To disable "lsi_msgpt3" run the following via the shell:

                  esxcli system module set \--enabled=false \--module=lsi_msgpt3

   

  You'll have to reboot.  If "mpt3sas" is installed the driver should be used in place for the 12Gb card.

   

   

   

  Xiaoming Xu

  ProSupport Senior Engineer

  Dell \| Enterprise Support Services

  Office + 86 592 818 0495

  How am I doing? Email my manager ([Renee_Mclean@dell.com](mailto:Renee_Mclean@dell.com)) with any feedback.

   

  [![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_005.jpg]]](http://www.dell.com/prodeploy)

    

  [![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_006.jpg]]](http://www.dell.com/certification)

   

  From: Wang, Andy King

  Sent: 2016年8月24日 9:13

  To: Tang, Keith; Xu, Xiaoming

  Cc: CN XMN EEC HK; Yu, Eric; Kar, Alex; CN XMN TS ENT L2 SME; TS_APJ_Storage_CHK_Directs

  Subject: RE: ND3420\|ST:GTXXLD2\|path issue\|pros

   

  Dell - Internal Use - Confidential 

  Xiaoming,

   

  Please help

   

  Andy

   

   

  From: Tang, Keith

  Sent: Tuesday, August 23, 2016 9:50 PM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: CN XMN EEC HK \<<CNXMNEECHK@Dell.com>\>; Yu, Eric \<<Eric_Yu@dell.com>\>; Kar, Alex \<<Alex_Kar@Dell.com>\>

  Subject: ND3420\|ST:GTXXLD2\|path issue\|pros

   

  Dell - Internal Use - Confidential 

  Dear L2 expert

   

  The case SR# 934894521,Tag# GTXXLD2\|MD3420\|path issue\| need you help

   

   

  a.     Detail Symptom Descriptions

  新机R730 安装了Dell的image VM6.0,安装了两张半高的SAS HBA，单线连接新MD3420， 问题连接第二个controller的第一个口是识别不到path。

   

  b.    Troubleshooting Steps

  收集到R730看不到硬件明显故障，两张HBA卡正常显示，MD3420的也没报硬件上的错误，单线测试每张SAS HBA Card的每个port（共2个）连到第1个controller 的4个SAS port都有path显示，连接SAS cable 接入的状态灯都是绿色的，当两张SAS HBA每个port连接到第二controller的第一个口时，认不到part，尝试更换过SAS cable一样认不到path，而且接入的线接口状态灯都是绿色的，后来更换第二controller，问题依旧，跟着对调controller，发现对调后的原来的第二controller是正常有path，对调后原来第一张controller的第一port没有part了，而且接线状态灯都是绿色的，也做了两条线测试还是在第二个controller的slot位上的第一个port没有part，现在查不出哪个问题，所以请L2帮忙分析。

  BIOS/Drive/FW版本:

  R730 BIOS version 2.1.7

  MD3420 controller firmware version：08.25.04.60

  查看firmware 我们的KB 上没有提到有issue。

   

  測試两条part的时候如图所示

  第个vmhba3第一个port连第一个controller第一port有part，vmhba3第二个port连接第二个controller的第1个port 没有path；也做过测试单线两个vmhab3，4的每个port接到第二controller 没有part。更换第二个controller 和对调controller问题都是在第二个controller slot位上的第一port都没有path。

   

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_007.jpg]]

  第个vmhba4第一个port连第一个controller第2port有part，vmhba4第二个port连接第二个controller的第2个port 有path；

   

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_008.jpg]]

   

  双线连接线状态灯是绿色的：

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_009.png]]

  单条SAS cable每个port 去测试，连接线状态灯是正常的，先单独接到第二个controller的第一个port，但就是没有path

  ![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_010.png]]

   

   

  c.     Current status

  客户公司名称:NEXUS SOLUTIONS LIMITED

  业务影响: 现在急需安装给最终客户，Urgency。

  升级的原因:未能查找出具体原因，且客户投诉要更换新机。

  /RM/TAM: Alex，kar

   

  d.     Must Collect Logs

  已经收集的日志：DSET Log and MD log

   

   

   

   

  Enterprise Product Engineer

  Keith_Tang

  Dell \| Enterprise Support Services, HK/Macau

  HK: 29693196 Pro-Support: 29693187

  Macau: 0800105 Pro-Support: 0800106

  Email Support: <CNXMNEECHK@Dell.com> 

  How am I doing? Please contact my manager <M_GUO@dell.com> with any feedback.

  [Dell TechDirect](http://www.techdirect.com/) \| Global portal to manage support cases and parts dispatching 

  For efficient problem resolution, get started today!

  [![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_011.png]]](http://en.community.dell.com/)

   

  [![[Technology_ALL_VMware_分析案例_035_ESXI lsi_msgpt3 驱动导致连接controller失败_012.png]]](http://www.dell.com/support/home/hk/zh/hkbsd1/?c=hk&l=zh&s=bsd&~ck=mn)

   

 

已使用 OneNote 创建。
