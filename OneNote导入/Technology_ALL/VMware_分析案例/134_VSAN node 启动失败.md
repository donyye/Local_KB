VSAN node 启动失败

2021年12月27日

14:07

问题：客户在vCenter上发现一个node的一个容量盘HDD出现了错误。盘更换后重启ESXI，发现卡在了这个画面上。 Initalizing SSD

 

当时找了一个KB，是需要等待

[https://docs.vmware.com/cn/VMware-vSphere/7.0/com.vmware.vsphere.vsan-monitoring.doc/GUID-F4E7760E-6DD6-47FF-B289-53C1167F7C1C.html](https://docs.vmware.com/cn/VMware-vSphere/7.0/com.vmware.vsphere.vsan-monitoring.doc/GUID-F4E7760E-6DD6-47FF-B289-53C1167F7C1C.html)

 

![[Technology_ALL_VMware_分析案例_134_VSAN node 启动失败_001.png]]

 

![[Technology_ALL_VMware_分析案例_134_VSAN node 启动失败_002.png]]

 

等待5\~6天后发现还是在这个画面没有重启。

 

 

![Machine generated alternative text: 2021-12-14104 .2341 .234 z 2021-12-14104 .2351 2351 2021-12-14104 .2371 2021-12-14104 .2371 2021-12-14104 .23BZ 2381 2021-12-14104 .240Z 2021-12-14104 .2401 24 IZ 2021-12-14104 .2431 2021-12-14104 .2441 2021-12-14104 .2451 245Z .2461 2021-12-14104 2021-12-14104 2881 2021-12-14104 : 59:28 .289 Z 2021-12-14104 .2891 .289Z 2021-12-14104 .3091 2021-12-14104 .3221 322Z 2021-12-14104 .3231 3231 2021-12-14104 .3231 2021-12-14104 .3251 .3251 2021-12-14104 .3251 2021-12-14104 : 59:28 .32SZ .3251 2021-12-14105 .5B6Z Virsto_credtelnstance: 163: INYO: Create neu Virsto instance (heapNane; virstolns INFO: Create neu Virsto instance (heapNdne: virstolnst INFO: Create neu Vir•sto instance (heaphane: virstolnst Virsto_CredteInstance: 163: ITO: Create neu Virsto instance (heapNdne: virstolnstd cpu121 Virsto_CreateInstance:163: INFO: Create neu Virsto instance (heapNene: virstolns Virsto_CredteInstance:163: INFO: Create neu Virsto instance (heapNome: virstolnst Virsto_CreateInstance:163: INFO: Create neu Virsto instance (heapNane: virstolnst cpu9:2101468)Global: Virsto_CreateInstance: 163: IWO: Create neu Virsto instance (heapName: virstolnsta cpu121 Virsto_CreateInstance:163: INFO: Create rru Virsto instance virstolns INFO: Create neu Virsto instance (heapNane: virstolnst Virsto_CreateInstance:163: INFO: Create rieu Virsto instance (heapNane: virstolnst Virst0_Createlnstance: 163: Create neu (heaphane: v irstolnsta cpu121 Virsto_CreateInstance: 163: INFO: Create Virsto instance virstolns INFO: Create neu Virsto instance (heapNane: virstolnst INFO: Create neu Virsto instance (heapName: virstolnst Virsto_credtelnstance: 163: IWO: Create neu Virsto instance (heaphdne: virstolnstd cpu121 disk-group u/ 520d699c-d?90-d429-69ff-6f2c0305681e do Virsto_CredteInstdnce:163: INFO: Create neu Virsto instance (heapNdne: virstolnst INFO: Create neu Virsto instance (heaphane: virstolnst Virsto_CredteInstance: 163: ITO: Create neu Virsto instance (heapNane: virstolnstd disk-group u/ SSD 5215bed2-aa2r-a18e-e2eO-39303c05504d on disk-group u/ SSD 52317881-d050-f218-2094-cc88144bbc09 on .248Z DON) disk-group u/ SSD 529e6c6a-433f-7ac4-ef31-a52be3f79d30 on .2ßBZ LSONSSDEnunCtj:2n: Finished reading SSD Log for disk 529e6c6a-499f-7ac4-ef91-aS2be3 SSDLOGEnunLog:1574: I-LOG: Total line: 39733 us, Read Tine: 37343 us. Process LSIWecouer•tJDispatch :2S67: LLOG recovery corgjlete 529e6c6a-499f :5618: Disk Event wrival for SSD 529e6c6a-499f-7ac4-ef91-a52be3f79d Doing plog recovery on SSI) 529e6c6a-499f-7ac4-ef91-a52be3f79d30 Finished reading SSI) Log for disk S20d699c-dBO-d429-69ff-6f2cß LLOC,: Total line: 63623 us. Read Tine: 62031 us. process cpu121 LI_OG recover-u complete 520d699c-d790-d429-69ff---6f2cß30S LSomLoqDiskEvent:5618: DiSk Event arrival for SSI) 520d63%-dBO-d429-69ff-6f2c030S Doing ploq recovery SSI) 520(fi99c-d730-d429-69ff-6f2c0305681e cpu87 Finished reading SSI) Loq for disk 5215bed2-dd2f-d1Be-e2eß-39303c cpu87. •2100626)Lsomconnon: SSDLOGEnunLog:1574: LLOC,: Total Tine: 74477 us, Read Tine: 70366 us. Process cpu66 LSONRecoveruDispatch:2567: LLOG recovery complete 5215bed2-dd2f-d18e-e2eO-39303cß55 LSONLogDiSkEvent:5618: Disk Event arrival ror SSI) 5215bed2-aa2f-a10e-e2eo-333U3c055 cpu66 cpu47 Doing plog recovery on SSD 5215bed2-dd2f-d18e-e2eO-39303c05504d LSONSSDEnunCb:227: Finished reading SSI) Log disk 52317881-a050-f218-2U94-cc8814 cpu53 cpuS3 SSDLOGEnunLog:1574: LLOG: Total line: 77085 us. Read Tine: 73032 us. Process cpu48 LLOG recovery complete 52317881-a050-f218-2094-ccB8144bb cpu48 111 Master-Bui : : Current nenbership id a124bB61-3U65-94d LSOHLoqDiskEvent :S618: Disk Event arr ival PI_OG Recover : 304: Doin 10 recove on Master-Bui ldHeartbeatnessaqe : 1766 : 156028\] 156023\] for SSD 52317881-a050-f218-2094-ccB8144bb SSI) 52317881-a050-f218-2094-ccB8144bbc09 : Sel?30eb-c3bO-4Sd8-3dS2-348ßOd4 :Current nenbership uu id a124b861-3865-94d ](attachments/Technology_ALL_VMware_分析案例_134_VSAN%20node%20启动失败_003.png)

 

![Machine generated alternative text: CPU ear eat essage : er U I urren nem er lip d 2021-12-19114. 2021-12-19111 : 14 .5861 021-12-19T12 .5957 2021-12-13112:59: 14 2021-12-19113 2021-12-19114 2021-12-19116 2021-12-19116 2021-12-19117 2021-12-19118:59: 14 2021-12-19119:59 : 14 2021-12-13113 •.59:14 2021-12-13120 2021-12-19121 2021-12-19121 . 2021-12-19122 2021-12-13122 2021-12-13123 14 .5861 . 1011 021-12-20100: 10 . 1231 2021-12-20100 :59: 14 Master-Bui 1766: \[56154\] cpu44 :209834? )NARNING: vnkusb: udev Ox43De221S9408. endpoint Ox82: inval id state 5: Fdi lure cpu44 vnkusb: \"dev Ox430e221594ß8, endpoint OXO: inval id state 5: Fai lure • 14 .5861 .586Z .5861 .5861 .5861 .5d6Z .5861 .586Z .5861 .586Z .5861 .586Z .5861 .586Z .5861 .5861 .5861 .5861 .5861 5861 .5861 .5861 .5861 .5861 cpu6 : 2100663 : cpu6 )cmms: cpul : 2100669 jcmmjs: )cmms: cpu4 : 2100663 : : 2100663 )CMDS: cpu4 : 2100663 : cpul : 2100663 )cmms: cpul : 2100669 )cmms: cpu2 : 2100663 )cmnos: : 2100663 )cmnos: cpu4 : 2100663 )CMNDS: cpu4 : 2100663 . cpu2 : 2100663 )cmms: cpu2 : 2100663 )cmms : cpul cpul : 2100663 )cmnos: cpu6 : 21B0663 : cpul : 2100663 )cmnos: cpu4 : 2100669 )cmms: cpu4 : 2100663 )cmnos: Noster-Bu i IdHeartbeatnessoqe NasterBu i ldHeartbeatnessage master-Bu i ldHeartbeatnessage Master-Bu i ldHeartbeatHessage master-Bu i ldHeartbeatnessage Nasterdu i ldHeartbeatnessaqe master-Bu i ldHeartbeatnessage HasterBu i ldHeartbeatHessaqe masterBu i ldHeartbeatnessaqe Master-Bu i ldHeartbeatnessaqe masterBu i ldHeartbeatnessage master-Bu i ldHeartbeatnessage master-Bu i ldHeartbeatnessaqe • i ldHeartbeatHessaqe NosterBu i ldHeartbeatnessage master-Bu i ldHeartbeatnessaqe masterBu i ldHeartbeatnessage master-Bu i ldHeartbeatnessage master-Bu i ldHeartbeatHessaqe HasterBu i ldHeartbeatnessaqe master-Bu i ldHeartbeatnessage master-Bu i ldHeartbeatHessage master-Bu i IdHeartbeatnessaqe :1758: \[56155\] :1758: \[56156\] :1758: :1758: \[56159\] :1758: \[56160\] :1758: \[56161\] :1758: \[56162\] :1758: \[56163\] :1758: \[56164\] master-Bui IdHeartbeatnessaqe.•1758: \[56165\] :Current nembership uuid :1758: \[56166\] : Current nenbership uuid d124b861-3865---94d :1766: (561551 : : 5e1730eb-c3bO-45d8-3d52-34800d4 : Current nenbership uuid :17662 \[56156 : : Se1730eb-c3bO-4Sd8-3dS2-34800d4 : Current nenbership id a124b861---386S---94d : 1766: : nenber : 5e173Ueb-c3bO-45dd-3d52-34dOOd4 :1758: \[56158\] :current nenbership uuid a124b861-3865-94d :1766: \[56158\] : : 5e1730eb-c3bO-45d8-3dS2-34800d4 :Current nenbership uuid : 1766: \[56159\] : : 5e1730eb-c3bO-45d8-3d52-34BOßd4 :Current nenbership uuid a124b861-3865---94d (56160\] : nenber : 5e1730eb-c3bO-45d8-3d52-34d00dA :1766: : Current nenbership uuid a124b861-3865-94d : 1766: \[56161 : : 5e1730eb-c3bO-4Sd8-3dS2-34800d4 : Current nembership uuid (561621 : : :1766: : Current nenbership uuid a124b861-3865-94d \[561631 : : 5e173Ueb-c3bO-45d8-3d52-34800dA :1766: : Current nenbership uuid : 1766: \[56164 : : 5e173Ueb-c3bO-4Sd8-3dS2-3480ßd4 :1766: \[56165\] : nenber : 5e1730eb-c3bO-45d8-3d52-34800d4 : Current nenbership uuid : 1766: \[56166\] :5e1730eb-c3bß-4Sd8-3d52-34800d4 cpu32 :209834? )NARNING: vnkusb: udev Ox43De22159408, endpoint Ox82: inval id state 5: Edi lure unkusb: udev ox430e221S94ß8, endpoint oxa: invalid state S: Failure cpu4 Master-Bui IdHeartbeatnessaqe : 1758: \[561671 :Current membership uuid IdHeartbeatnessaqe:1766: ](attachments/Technology_ALL_VMware_分析案例_134_VSAN%20node%20启动失败_004.png)

 

最后最后 SST检查是一个SSD坏了，拔掉SSD后正常进入系统。

 

已使用 OneNote 创建。
