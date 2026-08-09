VSAN 磁盘槽位定位

2022年11月1日

9:37

[如何确定被vSAN判定为故障硬盘的插槽位置 \| Dell US](https://www.dell.com/support/kbdoc/en-us/000190933?lang=zh)

<https://www.dell.com/support/kbdoc/en-us/000190933?lang=zh>

 

cat /commands/esxcfg-mpath\_-b.txt

![[Technology_ALL_VMware_分析案例_147_VSAN 磁盘槽位定位_001.png]]

 

通过黄色部分反查TSR，能找到具体槽位。

![[Technology_ALL_VMware_分析案例_147_VSAN 磁盘槽位定位_002.png]]

 

这边能看到是6槽位

![[Technology_ALL_VMware_分析案例_147_VSAN 磁盘槽位定位_003.png]]

 

具体位置：

![[Technology_ALL_VMware_分析案例_147_VSAN 磁盘槽位定位_004.png]]

 

 

===============================================

其它方法，在ESXI上运行 perccli 来确认

Exsi Perclli 下载

<https://www.dell.com/support/home/zh-cn/drivers/driversdetails?driverid=rkct0>

 

或者是在下面搜索 "VMware PERCCLI Utility For All Dell HBA/PERC Controllers \| Driver Detail"关键字获取

<https://www.dell.com/support/search/zh-cn#q=VMware%20PERCCLI%20Utility%20For%20All%20Dell%20HBA%2FPERC%20Controllers%20%7C%20Driver%20Detail&sort=relevancy&f:langFacet=%5Bzh,en%5D>

 

安装：

[\[root@localhost:\~\] esxcli software vib install -v /vmfs/volumes/SSD_1T_mouse/ISO_ALL/BCM_bootbank_vmware-perccli64-esxi8_007.2110.0000.0000-02.vib ]

Installation Result

   Message: Operation finished successfully.

   VIBs Installed: BCM_bootbank_vmware-perccli64-esxi8_007.2110.0000.0000-02

   VIBs Removed:

   VIBs Skipped:

   Reboot Required: false

   DPU Results:

\[root@localhost:\~\]

 

c0：c 是 controller 的意思，代表控制器编号，从 0 开始；

v0: v 是 virtual 的意思，代表逻辑磁盘编号，从 0 开始；

s0：s 是 slot 的意思，代表物理磁盘编号，从 0 开始；

e0：e 是 enclosure 的意思，从 0 开始。

 

[\[root@localhost:\~\] /opt/lsi/perccli64/perccli64 /c0/v1 show all]

CLI Version = 007.2110.0000.0000 Sep 27, 2022

Operating system = VMkernel 8.0.1

Controller = 0

Status = Success

Description = None

 

 

/c0/v1 :

======

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

DG/VD TYPE  State Access Consist Cache Cac sCC       Size Name    

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

1/1   RAID0 Optl  RW     Yes     RWBD  -   OFF 558.375 GB Vdisk_2

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

VD=Virtual Drive\| DG=Drive Group\|Rec=Recovery

Cac=CacheCade\|Rec=Recovery\|OfLn=OffLine\|Pdgd=Partially Degraded\|Dgrd=Degraded

Optl=Optimal\|dflt=Default\|RO=Read Only\|RW=Read Write\|HD=Hidden\|TRANS=TransportReady

B=Blocked\|Consist=Consistent\|R=Read Ahead Always\|NR=No Read Ahead\|WB=WriteBack

FWB=Force WriteBack\|WT=WriteThrough\|C=Cached IO\|D=Direct IO\|sCC=Scheduled

Check Consistency

 

 

PDs for VD 1 :

============

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

EID:Slt DID State DG       Size Intf Med SED PI SeSz Model            Sp Type

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

64:1      1 Onln   1 558.375 GB SAS  HDD N   N  512B AL13SXB60EN      U  -    

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

EID=Enclosure Device ID\|Slt=Slot No\|DID=Device ID\|DG=DriveGroup

DHS=Dedicated Hot Spare\|UGood=Unconfigured Good\|GHS=Global Hotspare

UBad=Unconfigured Bad\|Sntze=Sanitize\|Onln=Online\|Offln=Offline\|Intf=Interface

Med=Media Type\|SED=Self Encryptive Drive\|PI=PI Eligible

SeSz=Sector Size\|Sp=Spun\|U=Up\|D=Down\|T=Transition\|F=Foreign

UGUnsp=UGood Unsupported\|UGShld=UGood shielded\|HSPShld=Hotspare shielded

CFShld=Configured shielded\|Cpybck=CopyBack\|CBShld=Copyback Shielded

UBUnsp=UBad Unsupported\|Rbld=Rebuild

 

 

VD1 Properties :

==============

Strip Size = 256 KB

Number of Blocks = 1170997248

Span Depth = 1

Number of Drives Per Span = 1

Write Cache(initial setting) = WriteBack

Disk Cache Policy = Disk\'s Default

Encryption = None

Data Protection = None

Active Operations = None

Exposed to OS = Yes

OS Drive Name = N/A

Creation Date = 03-09-2020

Creation Time = 06:21:51 AM

Emulation type = default

Cachebypass size = Cachebypass-64k

Cachebypass Mode = Cachebypass Intelligent

Is LD Ready for OS Requests = Yes

SCSI NAA Id = 6d0946608117850026e347ffe278475c

Unmap Enabled = N/A

 

perccli的scsi naa.id 就是OS里面这个raid0磁盘的naa.id

 

<https://yfeng9186.gitee.io/blog/2020/6/%E5%9C%A8-dell-%E6%9C%8D%E5%8A%A1%E5%99%A8-esxi-%E4%B8%8B%E4%BD%BF%E7%94%A8-perccli-%E5%B7%A5%E5%85%B7/#2-%E5%9C%A8-ESXI-%E4%B8%8A%E5%AE%89%E8%A3%85-PercCli-%E5%B7%A5%E5%85%B7>

 

 

 

已使用 OneNote 创建。
