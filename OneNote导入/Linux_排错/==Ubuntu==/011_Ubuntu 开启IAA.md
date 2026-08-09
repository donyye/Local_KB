Ubuntu 开启IAA

2025年3月13日

16:00

 

FYI:

<https://github.com/intel/idxd-config/issues/46>

<https://github.com/intel/intel-device-plugins-for-kubernetes/issues/1836>

 

<https://www.owalle.com/2024/10/09/dsa-how-to-use/>

 

root@user:\~# cat /etc/os-release

PRETTY_NAME=\"Ubuntu 24.04.2 LTS\"

NAME=\"Ubuntu\"

VERSION_ID=\"24.04\"

VERSION=\"24.04.2 LTS (Noble Numbat)\"

VERSION_CODENAME=noble

ID=ubuntu

ID_LIKE=debian

HOME_URL=\"https://www.ubuntu.com/\"

SUPPORT_URL=\"https://help.ubuntu.com/\"

BUG_REPORT_URL=\"https://bugs.launchpad.net/ubuntu/\"

PRIVACY_POLICY_URL=\"https://www.ubuntu.com/legal/terms-and-policies/privacy-policy\"

UBUNTU_CODENAME=noble

LOGO=ubuntu-logo

 

root@user:\~# uname -a

Linux user 6.8.0-55-generic #57-Ubuntu SMP PREEMPT_DYNAMIC Wed Feb 12 23:42:21 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

 

 

[ ===]第一部分IAA的开启 ===

\# vim /etc/default/grub

GRUB_DEFAULT=0

GRUB_TIMEOUT_STYLE=hidden

GRUB_TIMEOUT=0

GRUB_DISTRIBUTOR=\`( . /etc/os-release; echo \$ ) 2\>/dev/null \|\| echo Ubuntu\`

GRUB_CMDLINE_LINUX_DEFAULT=\"intel_iommu=on,sm_on no5lvl modules_load=vfio-pci systemd.unified_cgroup_hierarchy=1 cgroup_no_v1=all psi=1\"

GRUB_CMDLINE_LINUX=\"\"

\# 如果不添加 no5lvl [ ]无法开启。

 

\# grub-mkconfig -o /boot/efi/EFI/ubuntu/grub.cfg

 

root@user:\~# cat /proc/cmdline

BOOT_IMAGE=/vmlinuz-6.8.0-55-generic root=/dev/mapper/ubuntu\--vg-ubuntu\--lv ro intel_iommu=on,sm_on no5lvl modules_load=vfio-pci systemd.unified_cgroup_hierarchy=1 cgroup_no_v1=all psi=1

 

root@user:\~# cat /sys/bus/dsa/devices/dsa0/pasid_enabled

1

 

root@user:\~# dmesg \|grep -i \'idxd\'

\[    6.288681\] idxd 0000:75:01.0: Intel(R) Accelerator Device (v100)

\[    6.308570\] idxd 0000:f2:01.0: Intel(R) Accelerator Device (v100)

 

失败的记录：

root@user:\~# dmesg \|grep -i \'idxd\'

\[    6.184973\] idxd 0000:75:01.0: Unable to turn on user SVA feature.

\[    6.208009\] idxd 0000:75:01.0: Intel(R) Accelerator Device (v100)

\[    6.208366\] idxd 0000:f2:01.0: Unable to turn on user SVA feature.

\[    6.232842\] idxd 0000:f2:01.0: Intel(R) Accelerator Device (v100)

 

 

root@user:\~# lspci \|grep 0b25

75:01.0 System peripheral: Intel Corporation Device 0b25

f2:01.0 System peripheral: Intel Corporation Device 0b25

 

root@user:\~# lsmod \|grep -i \'idxd\'

idxd                  151552  1 iaa_crypto

idxd_bus               16384  2 idxd,iaa_crypto

 

root@user:\~# lspci -vvv -s 75:01.0

75:01.0 System peripheral: Intel Corporation Device 0b25

Subsystem: Intel Corporation Device 0000

Control: I/O- Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx-

Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast \>TAbort- \<TAbort- \<MAbort- \>SERR- \<PERR- INTx-

Latency: 0

NUMA node: 0

IOMMU group: 1

Region 0: Memory at 26fffff20000 (64-bit, prefetchable) \[size=64K\]

Region 2: Memory at 26fffff00000 (64-bit, prefetchable) \[size=128K\]

Capabilities: \[40\] Express (v2) Root Complex Integrated Endpoint, MSI 00

DevCap:        MaxPayload 512 bytes, PhantFunc 0

ExtTag+ RBE+ FLReset+

DevCtl:        CorrErr- NonFatalErr+ FatalErr+ UnsupReq+

RlxdOrd+ ExtTag+ PhantFunc- AuxPwr- NoSnoop+ FLReset-

MaxPayload 512 bytes, MaxReadReq 4096 bytes

DevSta:        CorrErr- NonFatalErr- FatalErr- UnsupReq- AuxPwr- TransPend-

DevCap2: Completion Timeout: Not Supported, TimeoutDis+ NROPrPrP- LTR+

 10BitTagComp+ 10BitTagReq+ OBFF Not Supported, ExtFmt+ EETLPPrefix+, MaxEETLPPrefixes 1

 EmergencyPowerReduction Not Supported, EmergencyPowerReductionInit-

 FRS-

 AtomicOpsCap: 32bit- 64bit- 128bitCAS-

DevCtl2: Completion Timeout: 50us to 50ms, TimeoutDis+ LTR- 10BitTagReq+ OBFF Disabled,

 AtomicOpsCtl: ReqEn-

Capabilities: \[80\] MSI-X: Enable+ Count=9 Masked-

Vector table: BAR=0 offset=00002000

PBA: BAR=0 offset=00003000

Capabilities: \[90\] Power Management version 3

Flags: PMEClk- DSI- D1- D2- AuxCurrent=0mA PME(D0-,D1-,D2-,D3hot-,D3cold-)

Status: D0 NoSoftRst+ PME-Enable- DSel=0 DScale=0 PME-

Capabilities: \[100 v2\] Advanced Error Reporting

UESta:        DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-

UEMsk:        DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt+ RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-

UESvrt:        DLP- SDES- TLP+ FCP- CmpltTO+ CmpltAbrt+ UnxCmplt- RxOF- MalfTLP+ ECRC- UnsupReq- ACSViol-

CESta:        RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr-

CEMsk:        RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+

AERCap:        First Error Pointer: 00, ECRCGenCap- ECRCGenEn- ECRCChkCap- ECRCChkEn-

MultHdrRecCap- MultHdrRecEn- TLPPfxPres- HdrLogCap-

HeaderLog: 00000000 00000000 00000000 00000000

Capabilities: \[150 v1\] Latency Tolerance Reporting

Max snoop latency: 0ns

Max no snoop latency: 0ns

Capabilities: \[160 v1\] Transaction Processing Hints

Device specific mode supported

Steering table in TPH capability structure

Capabilities: \[170 v1\] Virtual Channel

Caps:        LPEVC=1 RefClk=100ns PATEntryBits=1

Arb:        Fixed+ WRR32- WRR64- WRR128-

Ctrl:        ArbSelect=Fixed

Status:        InProgress-

VC0:        Caps:        PATOffset=00 MaxTimeSlots=1 RejSnoopTrans-

Arb:        Fixed- WRR32- WRR64- WRR128- TWRR128- WRR256-

Ctrl:        Enable+ ID=0 ArbSelect=Fixed TC/VC=fd

Status:        NegoPending- InProgress-

VC1:        Caps:        PATOffset=00 MaxTimeSlots=1 RejSnoopTrans-

Arb:        Fixed- WRR32- WRR64- WRR128- TWRR128- WRR256-

Ctrl:        Enable+ ID=1 ArbSelect=Fixed TC/VC=02

Status:        NegoPending- InProgress-

Capabilities: \[200 v1\] Designated Vendor-Specific: Vendor=8086 ID=0005 Rev=0 Len=24 \<?\>

Capabilities: \[220 v1\] Address Translation Service (ATS)

ATSCap:        Invalidate Queue Depth: 00

ATSCtl:        Enable+, Smallest Translation Unit: 00

Capabilities: \[230 v1\] Process Address Space ID (PASID)

PASIDCap: Exec- Priv+, Max PASID Width: 14

PASIDCtl: Enable+ Exec- Priv+

Capabilities: \[240 v1\] Page Request Interface (PRI)

PRICtl: Enable+ Reset-

PRISta: RF- UPRGI- Stopped+

Page Request Capacity: 00000200, Page Request Allocation: 00000200

Kernel driver in use: idxd

Kernel modules: idxd

 

[ ===]第二部分通过 accel-config 工具配置 ===

accel-config 是一个Linux应用程序，提供了配置DSA设备的命令行工具。

accel-config 下载： [https://github.com/intel/idxd-config](https://github.com/intel/idxd-config)

具体的安装可以查看 README.md 文件，路径如下：

idxd-config-accel-config-v4.1.8/README.md

 

开启 dsa 与 wq

root@user:\~# accel-config load-config -c idxd-config-accel-config-v4.1.8/contrib/configs/app_profile.conf -e

Enabling device dsa0

Enabling wq wq0.1

Enabling wq wq0.0

 

root@user:\~#  ls -la /dev/dsa/wq0.0 /dev/dsa/wq0.1

crw\-\-\-\-\-\-- 1 root root 509, 1 Mar 13 08:07 /dev/dsa/wq0.0

crw\-\-\-\-\-\-- 1 root root 509, 0 Mar 13 08:07 /dev/dsa/wq0.1

 

 

\#  accel-config config-device dsa0

\# accel-config enable-device dsa0

dsa0 is in enabled state already, skipping\...

failed in dsa0

enabled 0 device(s) out of 1

 

\# accel-config enable-wq dsa0/wq0.0

dsa0/wq0.0 is in enabled or locked mode, skipping\...

failed in dsa0/wq0.0

enabled 0 wq(s) out of 1

 

 

生成了一个配置文件：

\# accel-config save-config -s save_config.conf

 

 

查看配置的信息：

root@user:\~# accel-config list

\

[  {

    \"dev\":\"dsa0\",

    \"read_buffer_limit\":0,

    \"max_groups\":4,

    \"max_work_queues\":8,

    \"max_engines\":4,

    \"work_queue_size\":128,

    \"numa_node\":0,

    \"op_cap\":\"00000000,00000000,00000000,00000000,00000000,00000000,00000001,003f027d\",

    \"gen_cap\":\"0x40915f0107\",

    \"version\":\"0x100\",

    \"state\":\"enabled\",

    \"max_read_buffers\":96,

    \"max_batch_size\":1024,

    \"max_transfer_size\":2147483648,

    \"configurable\":1,

    \"pasid_enabled\":1,

    \"cdev_major\":509,

    \"clients\":0,

    \"groups\":\

[      {

        \"dev\":\"group0.0\",

        \"read_buffers_reserved\":0,

        \"use_read_buffer_limit\":0,

        \"read_buffers_allowed\":96,

        \"grouped_workqueues\":\

[          {

            \"dev\":\"wq0.0\",

       [     \"mode\":\"shared\",]

            \"size\":8,

            \"group_id\":0,

            \"priority\":10,

            \"block_on_fault\":0,

            \"max_batch_size\":32,

            \"max_transfer_size\":16384,

            \"cdev_minor\":1,

            \"type\":\"user\",

            \"name\":\"app1\",

            \"driver_name\":\"user\",

            \"threshold\":6,

            \"ats_disable\":-2,

            \"state\":\"enabled\",

            \"clients\":0

          }

        \],

        \"grouped_engines\":\

[          

        \]

      },

      {

        \"dev\":\"group0.1\",

        \"read_buffers_reserved\":0,

        \"use_read_buffer_limit\":0,

        \"read_buffers_allowed\":96,

        \"grouped_workqueues\":\

[          

        \],

        \"grouped_engines\":\

[          

        \]

      },

      ,

      

    \],

    \"ungrouped_engines\":\

[      ,

      

    \]

  }

\]

 

 

 

已使用 OneNote 创建。
