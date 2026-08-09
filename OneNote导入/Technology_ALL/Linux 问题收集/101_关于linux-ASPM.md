关于linux-ASPM 

2024年9月26日

11:35

ASPM 是 pci 设备的节能模式

 

查看ASPM是否有开启：

[\[root@localhost driver\]# ll /sys/block/]

total 0

lrwxrwxrwx 1 root root 0 Sep 26  2024 sda -\> ../devices/pci0000:00/0000:00:1d.0/0000:05:00.0/ata11/host11/target11:0:0/11:0:0:0/block/sda

lrwxrwxrwx 1 root root 0 Sep 26  2024 sdb -\> ../devices/pci0000:30/0000:30:02.0/0000:31:00.0/host1/target1:3:109/1:3:109:0/block/sdb

lrwxrwxrwx 1 root root 0 Sep 26  2024 sdc -\> ../devices/pci0000:30/0000:30:02.0/0000:31:00.0/host1/target1:3:111/1:3:111:0/block/sdc

 

 

[\[root@localhost driver\]# lspci -vvvxxx -s 0000:30:02.0]

30:02.0 PCI bridge: Intel Corporation Device 347a (rev 04) (prog-if 00 \[Normal decode\])

DeviceName: Root Port

Control: I/O+ Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr+ Stepping- SERR+ FastB2B- DisINTx+

Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast \>TAbort- \<TAbort- \<MAbort- \>SERR- \<PERR- INTx-

Latency: 0

Interrupt: pin A routed to IRQ 134

NUMA node: 0

Region 0: Memory at 22fffff20000 (64-bit, non-prefetchable) \[size=128K\]

Bus: primary=30, secondary=31, subordinate=31, sec-latency=0

I/O behind bridge: 00007000-00007fff \[size=4K\]

Memory behind bridge: a6900000-a69fffff \[size=1M\]

Prefetchable memory behind bridge: 00000000a6400000-00000000a65fffff \[size=2M\]

Secondary status: 66MHz- FastB2B- ParErr- DEVSEL=fast \>TAbort- \<TAbort- \<MAbort+ \<SERR- \<PERR-

BridgeCtl: Parity+ SERR+ NoISA- VGA- VGA16- MAbort- \>Reset- FastB2B-

PriDiscTmr- SecDiscTmr- DiscTmrStat- DiscTmrSERREn-

Capabilities: \[40\] Express (v2) Root Port (Slot-), MSI 00

DevCap:        MaxPayload 512 bytes, PhantFunc 0

ExtTag+ RBE+

DevCtl:        CorrErr- NonFatalErr- FatalErr+ UnsupReq-

RlxdOrd- ExtTag+ PhantFunc- AuxPwr- NoSnoop-

MaxPayload 512 bytes, MaxReadReq 4096 bytes

DevSta:        CorrErr- NonFatalErr- FatalErr- UnsupReq- AuxPwr- TransPend-

LnkCap:        Port #5, Speed 16GT/s, Width x8, ASPM not supported

ClockPM- Surprise+ LLActRep+ BwNot+ ASPMOptComp+

LnkCtl:        ASPM Disabled; RCB 64 bytes, Disabled- CommClk+

ExtSynch- ClockPM- AutWidDis- BWInt- AutBWInt-

LnkSta:        Speed 16GT/s (ok), Width x8 (ok)

TrErr- Train- SlotClk+ DLActive+ BWMgmt- ABWMgmt-

RootCap: CRSVisible+

RootCtl: ErrCorrectable- ErrNon-Fatal+ ErrFatal+ PMEIntEna+ CRSVisible+

RootSta: PME ReqID 0000, PMEStatus- PMEPending-

DevCap2: Completion Timeout: Range ABC, TimeoutDis+ NROPrPrP+ LTR-

 10BitTagComp+ 10BitTagReq- OBFF Not Supported, ExtFmt- EETLPPrefix-

 EmergencyPowerReduction Not Supported, EmergencyPowerReductionInit-

 FRS- LN System CLS Not Supported, TPHComp- ExtTPHComp- ARIFwd+

 AtomicOpsCap: Routing+ 32bit+ 64bit+ 128bitCAS+

DevCtl2: Completion Timeout: 65ms to 210ms, TimeoutDis- LTR- OBFF Disabled, ARIFwd+

 AtomicOpsCtl: ReqEn+ EgressBlck-

LnkCap2: Supported Link Speeds: 2.5-16GT/s, Crosslink- Retimer+ 2Retimers+ DRS-

LnkCtl2: Target Link Speed: 16GT/s, EnterCompliance- SpeedDis-

 Transmit Margin: Normal Operating Range, EnterModifiedCompliance- ComplianceSOS-

 Compliance De-emphasis: -6dB

LnkSta2: Current De-emphasis Level: -3.5dB, EqualizationComplete+ EqualizationPhase1+

 EqualizationPhase2+ EqualizationPhase3+ LinkEqualizationRequest-

 Retimer- 2Retimers- CrosslinkRes: unsupported

Capabilities: \[80\] Power Management version 3

Flags: PMEClk- DSI- D1- D2- AuxCurrent=0mA PME(D0+,D1-,D2-,D3hot+,D3cold+)

Status: D0 NoSoftRst+ PME-Enable- DSel=0 DScale=0 PME-

Capabilities: \[88\] Subsystem: Intel Corporation Device 0000

Capabilities: \[90\] MSI: Enable+ Count=1/1 Maskable- 64bit-

Address: fee00018  Data: 0000

Capabilities: \[100 v1\] Advanced Error Reporting

UESta:        DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-

UEMsk:        DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt+ RxOF- MalfTLP- ECRC- UnsupReq+ ACSViol+

UESvrt:        DLP+ SDES+ TLP+ FCP+ CmpltTO+ CmpltAbrt+ UnxCmplt- RxOF+ MalfTLP+ ECRC+ UnsupReq- ACSViol-

CESta:        RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr-

CEMsk:        RxErr+ BadTLP+ BadDLLP+ Rollover+ Timeout+ AdvNonFatalErr+

AERCap:        First Error Pointer: 00, ECRCGenCap+ ECRCGenEn+ ECRCChkCap+ ECRCChkEn+

MultHdrRecCap- MultHdrRecEn- TLPPfxPres- HdrLogCap+

HeaderLog: 40008001 3100000f fee00db8 00000000

RootCmd: CERptEn- NFERptEn- FERptEn-

RootSta: CERcvd- MultCERcvd- UERcvd- MultUERcvd-

 FirstFatal- NonFatalMsg- FatalMsg- IntMsg 0

ErrorSrc: ERR_COR: 0000 ERR_FATAL/NONFATAL: 0000

Capabilities: \[148 v1\] Access Control Services

ACSCap:        SrcValid+ TransBlk+ ReqRedir+ CmpltRedir+ UpstreamFwd+ EgressCtrl- DirectTrans-

ACSCtl:        SrcValid- TransBlk- ReqRedir- CmpltRedir- UpstreamFwd- EgressCtrl- DirectTrans-

Capabilities: \[180 v1\] Vendor Specific Information: ID=0003 Rev=0 Len=00a \<?\>

Capabilities: \[190 v1\] Downstream Port Containment

DpcCap:        INT Msg #0, RPExt+ PoisonedTLP+ SwTrigger+ RP PIO Log 4, DL_ActiveErr+

DpcCtl:        Trigger:0 Cmpl- INT- ErrCor- PoisonedTLP+ SwTrigger- DL_ActiveErr-

DpcSta:        Trigger- Reason:00 INT- RPBusy- TriggerExt:00 RP PIO ErrPtr:1f

Source:        0000

Capabilities: \[200 v1\] Secondary PCI Express

LnkCtl3: LnkEquIntrruptEn- PerformEqu-

LaneErrStat: 0

Capabilities: \[400 v1\] Data Link Feature \<?\>

Capabilities: \[410 v1\] Physical Layer 16.0 GT/s \<?\>

Capabilities: \[450 v1\] Lane Margining at the Receiver \<?\>

Kernel driver in use: pcieport

00: 86 80 7a 34 47 05 10 00 04 00 04 06 00 00 01 00

10: 04 00 f2 ff ff 22 00 00 30 31 31 00 70 70 00 20

20: 90 a6 90 a6 41 a6 51 a6 00 00 00 00 00 00 00 00

30: 00 00 00 00 40 00 00 00 00 00 00 00 ff 01 03 00

40: 10 80 42 00 22 80 00 00 44 51 00 00 84 40 7a 05

50: 40 00 84 30 00 00 00 00 c0 03 40 01 1e 00 01 00

60: 00 00 00 00 f7 07 01 00 66 00 00 00 1e 00 80 01

70: 44 00 1f 00 00 00 00 00 00 00 00 00 00 00 00 00

80: 01 88 03 c8 08 00 00 00 0d 90 00 00 86 80 00 00

90: 05 00 01 00 18 00 e0 fe 00 00 00 00 00 00 00 00

a0: 00 00 00 00 00 00 00 00 00 00 00 00 03 00 00 00

b0: 00 00 10 00 00 00 00 00 00 00 00 00 00 00 00 00

c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

d0: 00 00 00 00 09 00 00 00 00 00 00 00 00 78 00 00

e0: 66 77 05 00 00 00 00 00 00 00 00 00 00 00 00 00

f0: 00 00 00 00 00 00 00 00 02 00 00 00 00 00 00 00

 

过滤一下，看到系统并没有开启：

[\[root@localhost driver\]# lspci -vvvxxx -s 0000:30:02.0 \|grep -i aspm]

LnkCap:        Port #5, Speed 16GT/s, Width x8, ASPM not supported

ClockPM- Surprise+ LLActRep+ BwNot+ ASPMOptComp+

LnkCtl:        ASPM Disabled; RCB 64 bytes, Disabled- CommClk+

 

 

 

开启的方式，首先BIOS需要先开启，系统开启方式：

<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/power_management_guide/aspm#ASPM>

 

ASPM support can be enabled or disabled by the pcie_aspm kernel parameter, where pcie_aspm=off disables ASPM and pcie_aspm=force enables ASPM, even on devices that do not support ASPM. 

 

主动状态电源管理(ASPM)节省了 PCI Express (PCI Express 或 PCIe)子系统的电力，方法是在连接的设备不在使用中时，为 PCIe 链路设置一个较低的电源状态。ASPM 控制链路两端的电源状态，即使链路末端的设备处于完全通电状态，也可以节省链路中的电源。

当启用 ASPM 时，由于在不同电源状态之间转换链接所需的时间，设备延迟会增加。ASPM 有三个政策来确定电力状态:

 

Default：

根据系统上的固件(例如，BIOS)指定的缺省值设置 PCIe 链路电源状态。这是 ASPM 的默认状态。

 

powersave：

设置 ASPM 尽可能节省电力，而不管性能成本。

 

Performance：

禁用 ASPM 以允许 PCIe 链接以最高性能运行。

 

 

 

 

已使用 OneNote 创建。
