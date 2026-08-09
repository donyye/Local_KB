SDDC日志收集（S2D）

2020年8月10日

13:34

 

收集 SDDC 日志，针对 S2D 集群

参考：https：//docs.microsoft.com/zh-cn/windows-server/storage/storage-spaces/data-collection

 

1.在以下工作站上下载

<https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip>

 

2.提取文件夹PrivateCloud.DiagnosticInfo

 

3.将提取的文件夹复制到不连接互联网的服务器到此位置

C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\Modules

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_001.png]]

 

4.在服务器上导入模块

Import-Module PrivateCloud.DiagnosticInfo --Force

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_002.png]]

 

5.在服务器上，使用以下命令运行SDDC

Get-SddcDiagnosticInfo

![Machine generated alternative text: 2020-03-12- 31 --- Stop 202048-11m •32. C4Js-11T11.31 Of 2020-08-11711: 32 Localhos Ill I c-08-11T11 loc Ibos • 1122 41 --- Stop 1 18 c.8-11T11 231 mayC8-11T11 223.51 - stop H8-11T11 2020-08-1171:32 Sul start stut Info: RODE-002 mnE-003 - POOL, IJ0b8a \[J obo b32 b85 t\]: 1112 t\]: 1112. i EON-O&\--IITII zoa---C8-11T11 mm---C8-11T11 mmEC8-11T11 maHe-11T11 33 •37 : --- Stop stop --- Stop : 35 of • • stÜt : FKwminE . 11:28 58 up \_ log lag v 1.1.30 1.1.30 I --- Health CYeN : APEX-CLUSTER. i+ : 22 / 22 : Zip File C ZIP up ado we \\ Snt ](attachments/Technology_ALL_windows_case_070_SDDC日志收集（S2D）_003.png)

 

6. 收集完成后日志地址

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_004.png]]

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

==========================================

下面是的零散记录截图

 

 

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_005.png]]

 

 

 

 

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_006.png]]

 

 

 

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_002.png]]

 

 

 

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_007.png]]

 

 

 

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_008.png]]

 

 

![Machine generated alternative text: . st : Clu: tend phy81caLD sks Units 08 File . Filegyet sys It (John . on40.9 : - stop bi2 xoo:-ooe \[JobS4 XOZ:-OOS); Ora stat Ora. 28 : stut 010 : stut 2020-08-1171:B 16 , tJ0bte S too stoo Sto stop 2020-03-11T11:æ 2021E03-LLt1L:E 2020-38-IITII:B I : cor018t8d. Fil File d: File BF ten F It It Fi Fi mnE-cc2\] \[Job6L \'DDE-con : \[Jot62 IJoW3 \"DDE-c•c€l : OmO.3 • 00.33 xo\'H101\] \[Job-SB u.)DE-O4\]: cao.og : \[JobS9 0.0.0, b73 mnE-cc3\] mcAJ8-11r11 maHJ8-nr11 . stut uco-11T11 . stut mm-c8-11T11 20200-LLt1L:0 02 48-11m -08\--11m 2020-08-11711 48-11T11 mao-4\'8-11T-n:33 08-11T11 48-11T11 : copy VER \[Job?O \'DDE-COZI . : cop, COW , COW Copy VER Rep (110.0 . stut stut \[Job72 maHe-11T11 ZOEO-CE-IITII zom-C8-11T11 mm-C8-11T11 mm-ce-11T11 - mayC8-11T11 - --- Stop --- Stop --- Stop --- Stop stop Stop ma ](attachments/Technology_ALL_windows_case_070_SDDC日志收集（S2D）_009.png)

 

 

![Machine generated alternative text: s2D xoo:-jos \[JobLz : s2D NODE-ODS IJob140 loc Ono 42 . stzt 48 : stat - mao-08-11T11:32 c-08-11T11 c..11T11 •03:53 c-C8-11T11 AS-llrll .3104 2020-08-11711:32. c---38-11T11 Smry loc Ibos • 1135 mm---c8-11T11 - stop maÄ8-11T1122B .41 - stop : 46 Localhos 1112 2 - Pool, Detail, Info: mnE-001 xcDE-oos Info: WDE-006 Stoner \[J obo \[JobB3 bB6 bB3 692 695 t\]: 1122 v 1.1.30 s\' v 1.1.30 m C\*c8-11T11 zoa---C8-11T11 stop --- Stop --- Stop --- Stop : Get stut log : 3? : with : ce/11,\'2020 1122B 5B up : APEX-CLUSTER. ide 22 / 22 1 thT8 ](attachments/Technology_ALL_windows_case_070_SDDC日志收集（S2D）_010.png)

 

![Machine generated alternative text: 2020-03-12- 31 --- Stop 202048-11m •32. C4Js-11T11.31 Of 2020-08-11711: 32 Localhos Ill I c-08-11T11 loc Ibos • 1122 41 --- Stop 1 18 c.8-11T11 231 mayC8-11T11 223.51 - stop H8-11T11 2020-08-1171:32 Sul start stut Info: RODE-002 mnE-003 - POOL, IJ0b8a \[J obo b32 b85 t\]: 1112 t\]: 1112. i EON-O&\--IITII zoa---C8-11T11 mm---C8-11T11 mmEC8-11T11 maHe-11T11 33 •37 : --- Stop stop --- Stop : 35 of • • stÜt : FKwminE . 11:28 58 up \_ log lag v 1.1.30 1.1.30 I --- Health CYeN : APEX-CLUSTER. i+ : 22 / 22 : Zip File C ZIP up ado we \\ Snt ](attachments/Technology_ALL_windows_case_070_SDDC日志收集（S2D）_003.png)

 

![[Technology_ALL_windows_case_070_SDDC日志收集（S2D）_004.png]]

 

 

 

已使用 OneNote 创建。
