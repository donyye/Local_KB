关于ESXI用PERC9出现crash问题 

2017年10月31日

10:21

- ::: 
    -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       FW: PERC9 firmware 25.5.3.0005 \| VSAN unstable 
    发件人     Han, Ruyang
    收件人     CN XMN TS ENT L2 SME; Lin, Yongliang; Wang, Xing Fang
    发送时间   2017年10月30日 15:00
    -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  Hi Team

   

  下面是我给TAM关于PERC crash/reset问题的解释，大家看一下有没有别的意见。

   

  如果没问题我们Team内部也统一口径。

   

   

  Best Regards

  Ruyang Han

   

  From: Cui, Hugh

  Sent: Wednesday, October 25, 2017 5:35 PM

  To: Han, Ruyang \<Ruyang_Han@Dell.com\>; Lv, Richard \<Richard_Lv@DELL.com\>; Xu, Kacee \<Kacee_Xu@Dell.com\>

  Cc: Wang, Xing Fang \<Xing_Fang_Wang@DELL.com\>

  Subject: 答复: PERC9 firmware 25.5.3.0005 \| VSAN unstable 

   

  Ruyang,

   

  非常感谢你的总结和解释，很有帮助。

   

  Best Regards

   

  Hugh Cui

  崔向斌

  Technology Service Manager

  Dell \| Account Management Services

  office +86 20 3819 3020,  mobile +86 139 0240 2047

   

  发件人: Han, Ruyang 

  发送时间: 2017年10月25日 17:00

  收件人: Cui, Hugh \<[hugh_cui@DELL.com](mailto:hugh_cui@DELL.com)\>; Lv, Richard \<[Richard_Lv@DELL.com](mailto:Richard_Lv@DELL.com)\>; Xu, Kacee \<[Kacee_Xu@Dell.com](mailto:Kacee_Xu@Dell.com)\>

  抄送: Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\>

  主题: RE: PERC9 firmware 25.5.3.0005 \| VSAN unstable 

   

  Dell - Internal Use - Confidential 

  Hi Hugh

   

  文字描述说来话长，可以先看看，如果需要可以电话讨论。

   

  1.  大量的Timeout产生的原因还没有定论，PERC engineer还在研究，TS L2接触不到他们针对底层问题的工作。

  硬盘 timeout当然可能是硬件原因（信号，坏道，机械部件等），硬盘产生少量timeout很常见，发生时硬盘可以尝试retry，如果失败阵列卡可以reset此硬盘使之恢复正常工作，这些动作可以在极端的时间内完成，在OS和应用层很难会有感觉。

  但我们发现在极短的时间内产生大量的timeout问题仅在VSAN环境中才有，在多种不同vendor和型号的HDD和SSD都有，而其它OS和非VSAN的vsphere环境下极少见。我们还发现 阵列卡会收到来自OS的无效指令，但VMware TS L3不承认是OS的问题，认为就算是有也是在PERC driver层面。

   

  1.  解决阵列卡崩溃的问题能很大程度上解决整个问题或缓解故障影响。Timeout和阵列卡崩溃其实是把故障原因的分解为两个问题来解决，能确定的是无论如何阵列卡都不应该崩溃，这也是短时间内有手段可以规避方向，只要阵列卡不崩溃个别硬盘出现timeout还是有挽回手段比如retry和timeout，最坏情况是某一个硬盘掉线，而不像阵列卡崩溃时整个节点的所有disk group故障，当OS装在PERC上时OS甚至会hang up。

   

  1.  关于友商。我想或许友商没有firmware crash的问题，但我个人了解多少应该有一些disk unstable的问题。因为VMware这两年发布过多个和disk unstable相关的KB且在持续的更新内容，比如调整各种内核参数，并不是针对DELL一家，以下KB供参考：

  <https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2150754&sliceId=1&docTypeID=DT_KB_1_1&dialogID=519282848&stateId=1%200%20519284820>

  <https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2148502>

  <https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2118895>

  <https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2129050&sliceId=1&docTypeID=DT_KB_1_1&dialogID=519282848&stateId=1%200%20519284820>

   

  1.  另外有一部分故障是因为客户部署VSAN时未遵循最佳实践或错误的配置导致。

   

   

   

  September 2017

  - [vSAN 6.6 Ondisk upgrade to version 5 fails with the error \"A general system error occurred: Unable to complete Sysinfo operation\...\" (2151316)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2151316) 
  - [Hosts that are not configured for vSAN report events related to com.vmware.vsan.witnesshoststate (2151381)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2151381) 
  - [Duplicate SCSI IDs causing SATA drives in drive bay #1 to go missing from ESXi when running the nhpsa driver on Gen 9 HPE Synergy compute modules, HPE ProLiant DL-series servers that include a SAS expander (2150104)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150104) 
  - [vSAN 2017 Quarterly Advisory for Q2 (2150957)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150957) 
  - [NetApp ONTAP Select vNAS for VMware vSAN (2151182)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2151182) 
  - [Setting up active-passive dual pathing with vSAN and vSphere (2151225)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2151225) 
  - [Understanding vSAN memory consumption in ESXi 6.5.0d/ 6.0 U3 and later (2113954)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2113954)

   

  August 2017

  - [Diskgroups fail to mount due to heap exhaustion (2150566)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150566) 
  - [vSAN virtual machines report the status as Not applicable (2150571)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150571) 
  - [vSAN Support for HPE Gen9 Server Locator LEDs (2151284)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2151284)

   

  July 2017

  - [vSAN performance diagnostics (2148770)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148770) 
  - [vSAN CLOMD daemon may fail when trying to repair objects with 0 byte components (2149968)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149968) 
  - [ESXi 6.x vSAN host experiences a purple diagnostic screen at bora/modules/vmkernel/lsomcommon/ssdlog/ssdopslog.c:199 (2146345)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146345) 
  - [High value of file system overhead observed in the vSAN capacity breakdown with dedup enabled (2150999)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150999) 
  - [vCenter Server task list shows several erroneous messages stating: Update vSAN configuration (2150749)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150749) 
  - [vSAN Cluster Incorrectly Displays Network Mode as Multicast (2150523)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150523) 
  - [vSAN Host Not Contributing Stats reports with SSL error (2150570)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150570) 
  - [vSAN health test fails on Performance Service (2150589)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150589) 
  - [Troubleshooting vSAN Witness Node Isolation (2150433)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150433) 
  - [Build numbers and versions of VMware vSAN (2150753)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150753)

   

  June  2017

  - [ESXi 6.x vSAN host experiences a purple diagnostic screen at bora/modules/vmkernel/lsomcommon/ssdlog/ssdopslog.c:199 (2146345)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146345) 
  - [bytesToSync values appear incorrectly for RAID5/6 objects (2150395)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150395) 
  - [Using objtool on a vSAN witness host might cause an ESXi host to fail with a Purple Screen Of Death (2150396)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150396) 
  - [False Alarm for the vSAN Health Check \'All hosts have a Virtual SAN vmknic configured\' (2150390)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150390) 
  - [SSD congestion might cause multiple virtual machines to become unresponsive (2150389)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150389) 
  - [vSAN Datastores are inaccessible in vSphere 6.0 update 2 and update 3 (2150387)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150387) 
  - [ESXi host part of a vSAN cluster shows error: NMI: 709: NMI IPI received. Was eip(base):ebp:cs / Bucketlist_LowerBound@LSOMCommon (2150189)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150189) 
  - [vSAN CLOMD daemon may fail when trying to repair objects with 0 byte components (2149968)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149968) 
  - [Cannot unmount temporary datastore used for vSAN traces from vSAN cluster ESXi hosts (2150328)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150328) 
  - [\"The ramdisk \"vsantraces\" is full\" error reports in vSAN logging (2150320)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150320) 
  - [vSAN node fails to enter the maintenance mode in current state (2150271)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150271) 
  - [Heavy resync traffic may cause VM IO performance degradation (2150101)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150101) 
  - [Support for multiple vmknics with vSAN (2149967)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149967) 
  - [Configuring vSAN Unicast networking from the command line. (2150303)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150303) 
  - [Understanding Congestion in vSAN (2150260)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150260)

   

  May 2017

  - [Initializing vSAN during boot takes a longer time (2149115)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149115) 
  - [vSAN proactive rebalance (2149809)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149809) 
  - [Back up and restore of VMs deployed on a vSAN datastore using DELL EMC Avamar (2149872)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149872) 
  - [Veeam backup and replication with vSAN datastores (2149874)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149874) 
  - [Migrating a vSAN host from one vCenter to an another vCenter (2150326)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2150326)

   

  April 2017

  - [Understanding vSAN Datastore Encryption vs. VMcrypt Encryption (2148947)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148947) 
  - [VMware vSAN upgrade best practices (2146381)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146381) 
  - [vSAN upgrade requirements (2145248)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145248) 
  - [Supported upgrade paths for vSAN 6.6 (2149840)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149840) 
  - [Understanding vSAN on-disk format versions and compatibility (2145267)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145267) 
  - [Dying Disk Handling (DDH) in vSAN 6.6 (2148358)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148358) 
  - [Transient alarms might be observed during vSAN reconfiguration operations (2149676)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149676) 
  - [Request to edit vSAN iSCSI target service might appear when iSCSI is not enabled (2149678)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149678) 
  - [A virtual machine on a vSAN datastore might be renamed when vSAN becomes inaccessible (2149706)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149706)

   

  March 2017

  - [High frequency of read operations on VMware Tools image may cause SD card corruption (2149257)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149257) 
  - [vSAN 6.2 increase in used disk space with All-Flash and Deduplication (2147343)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2147343) 
  - [Permanently Decommissioning a node from a vSAN Cluster (2148975)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148975) 
  - [Shutting down and powering on a vSAN 6.x Cluster when vCenter Server is running on top of vSAN (2142676)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2142676) 
  - [Redirecting vSAN trace-level messages to a syslog server (2145556)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145556) 
  - [Understanding vSAN on-disk format versions and compatibility (2145267)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145267) 
  - [Updating the vSAN HCL database manually (2145116)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145116) 
  - [How to use and interpret performance statistics collected using vSAN Observer (2064240)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2064240) 
  - [How to add a host back to a vSAN cluster after an ESXi host rebuild (2059091)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2059091) 
  - [Required vSAN and ESXi configuration for controllers based on the LSI 3108 chipset (2144936)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2144936) 
  - [Support for running ESXi/ESX as a nested virtualization solution (2009916)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2009916) 
  - [vSAN certification status of Dell PERC H330 controllers (2149392)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149392) 
  - [vSAN Health Check Information (2114803)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2114803) 
  - [Upgrading vSAN On-Disk format to 3.0 may fail in small vSAN clusters or ROBO/stretched clusters (2144944)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2144944) 
  - [Deploying vSAN on certain Supermicro controllers fails (2083828)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2083828) 
  - [Creating a vSAN Disk Group fails with the error: General Virtual SAN error (2145718)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145718) 
  - [vSAN on disk upgrade fails at 10% (2144881)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2144881) 
  - [False \"Virtual SAN health\" alarms triggered, Web Client/RVC checks return a green/passed state (2149167)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149167)

   

  February 2017

  - [Destage process can result in poor performance in vSAN deduplication environments (2149066)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149066) 
  - [vSAN host may encounter a purple diagnostic screen during resync operations if resync is paused (2149130)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149130) 
  - [vSAN host may encounter a purple diagnostic screen during performance statistics updates (2148420)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148420) 
  - [vSAN performance enhancements delivered with vSphere 6.0 Update 3 and vSphere 6.5.0d(vSAN 6.6) (2149127)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149127) 
  -  [ESXi host fails to rejoin VMware vSAN cluster following reboot (2148122)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148122) 
  - [ESXi 6.5 not populated when exporting a support bundle in the Web Client (2148382)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148382) 
  - [vSAN upgrade requirements (2145248)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145248) 
  - [ESXi host version requirements in a vSAN cluster (2148493)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148493) 
  - [Best practices for using vSAN 6.5 iSCSI devices (2148216)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148216) 
  - [\"Virtual SAN Disk Balance\" warning alarm during vSAN health check (2148484)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148484) 
  - [vSAN Health check warning for controller driver version containing string patterns with \"-1OEM\" or "VMW" (2148615)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148615) 
  - [\"Virtual SAN objects available for provisioning\" task fails (2148607)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148607) 
  - [Identifying specific disk failure in a vSAN deduplication cluster (2149067)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149067) 
  - [Modification of congestion-related vSAN advanced parameters (2149096)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2149096) 
  - [Changing the multicast address used for a VMware vSAN Cluster (2075451)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2075451) 
  - [Hot unplug and plug of disks on HPE Gen8 and Gen9 IO controllers (2077426)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2077426) 
  - [vSAN Performance Graphs in the vSphere Web Client (2144493)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2144493) 
  - [Collecting diagnostic information from Broadcom MegaRAID controllers (2146429)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146429) 
  - [Increasing driver logging verbosity on Broadcom HBAs (2146431)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146431) 
  - [Best practices when using vSAN and non-vSAN disks with the same storage controller (2129050)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2129050) 
  - [Using VMware vSAN deduplication with VMware Horizon View (2144615)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2144615) 
  - [vSAN disk group creation may fail when using large cache tier drives (2146495)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146495)

   

  January 2017

  - [vSAN hosts may encounter a purple screen during object cleanup operations (2147925)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2147925) 
  - [vSAN 6.2 increase in used disk space with All-Flash and Deduplication (2147343)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2147343) 
  - [Component metadata health check fails with invalid state error (2145347)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2145347) 
  - [vSAN PCIE/NVMe SSDs Receive Warning for HCL Health Check (2146676)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2146676) 
  - [FAQ: Support statement for 512e and 4K Native drives for VMware vSphere and vSAN (2091600)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2091600) 
  - [Redirecting system logs to a vSAN object causes an ESXi host lock up (2147541)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2147541) 
  - [Best practices for vSAN implementations using Dell PERC H730 or FD332-PERC storage controllers (2109665)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2109665) 
  - [Deployment guidelines for running VMware vSAN and VMware vSphere VMFS datastores on a Dell H730 controller with the lsi_mr3 driver (2136374)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2136374) 
  - [Understanding the Proactive Storage Stress Test for vSAN (2147074)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2147074) 
  - [Add All-Flash support to vSAN 6.0/6.1/6.2 STANDARD licenses (2148560)](https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2148560)

   

   

   

  Best Regards

  Ruyang Han

   

  From: Cui, Hugh

  Sent: Wednesday, October 25, 2017 3:06 PM

  To: Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lv, Richard \<<Richard_Lv@DELL.com>\>; Xu, Kacee \<<Kacee_Xu@Dell.com>\>

  Cc: Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>

  Subject: 答复: PERC9 firmware 25.5.3.0005 \| VSAN unstable 

   

  Ruyang,

   

  到底我们对这reset timeout的问题归类为我们的阵列卡问题还是系统的问题？ 按我的理解这版固件只是优化在大量出现timeout的情况下不会那么容易出现整列卡崩溃，但timeout的情况仍然会出现，这是治标不治本的。我们友商有类似问题发生吗？根据我能了解的范围，还没有。还有昨天会上好像也说了，其中调整timeout值那个建议也只是根据经验不具针对性，不同的版本表现也是不一样的，这些能总结一下发出来，让我们跟客户交流的时候有个统一口径，底气也会足一点。

   

  Best Regards

   

  Hugh Cui

  崔向斌

  Technology Service Manager

  Dell \| Account Management Services

  office +86 20 3819 3020,  mobile +86 139 0240 2047

   

  发件人: Han, Ruyang 

  发送时间: 2017年10月25日 14:44

  收件人: Lv, Richard \<[Richard_Lv@DELL.com](mailto:Richard_Lv@DELL.com)\>; Xu, Kacee \<[Kacee_Xu@Dell.com](mailto:Kacee_Xu@Dell.com)\>; Cui, Hugh \<[hugh_cui@DELL.com](mailto:hugh_cui@DELL.com)\>

  抄送: Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\>

  主题: RE: PERC9 firmware 25.5.3.0005 \| VSAN unstable 

   

  Dell - Internal Use - Confidential 

   

  Dell - Internal Use - Confidential 

  Hi All

   

  上个月我们官网发布了PERC9最新版本固件：25.5.3.0005，重要性为：紧急。

  解决RAID controller crash reset问题是重点之一，Release有明确说明，同时我们期望这一版firmware可以解决VSAN环境下阵列卡不稳定的issue。

  注意此版本还未在VMware HCL中更新，预计本月底或下月初会完成认证。

   

   

  另外总结一下VSAN环境下阵列卡不稳定的issue的故障表现：

  1.  VSAN节点的disk group状态异常。
  2.  TTY log中可以看到controller firmware crash/reset信息。
  3.  Raid controller crash后可能reset失败，导致完全丢失raid卡信息，无法抓取到TTY log。
  4.  重启服务器后节点恢复正常，短时间内故障不会复现。

   

   

  [http://www.dell.com/support/home/cn/zh/cnbsd1/drivers/driversdetails?driverId=C58TW](http://www.dell.com/support/home/cn/zh/cnbsd1/drivers/driversdetails?driverId=C58TW)

  ![[Technology_ALL_VMware_分析案例_077_关于ESXI用PERC9出现crash问题_001.png]]

   

   

   

   

  Best Regards

  Ruyang Han

   

 

已使用 OneNote 创建。
