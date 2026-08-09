Learning

2017年11月23日

13:36

Ubuntu 16.04.3[  + R640 + H740p]

 

Handle 0x0100, DMI type 1, 27 bytes

System Information

        Manufacturer: Dell Inc.

        Product Name: PowerEdge R640

        Version: Not Specified

       [ Serial Number: 2C88GL2]

        UUID: 4C4C4544-0043-3810-8038-B2C04F474C32

        Wake-up Type: Power Switch

        SKU Number: SKU=NotProvided;ModelName=PowerEdge R640

        Family: PowerEdge

 

 

root@ubuntu:\~# lsb_release -a

No LSB modules are available.

Distributor ID:        Ubuntu

Description:        Ubuntu 16.04.3 LTS

Release:        16.04

Codename:        xenial

root@ubuntu:\~#

 

root@ubuntu:\~# uname -a

Linux ubuntu 4.4.0-87-generic #110-Ubuntu SMP Tue Jul 18 12:55:35 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

root@ubuntu:\~#

 

root@ubuntu:\~# lspci -vvv \|grep 57416

\[PN\] Part number: BCM957416

\[PN\] Part number: BCM957416

 

root@ubuntu:\~# fdisk -l

Disk /dev/sda: 111.7 GiB, 119966990336 bytes, 234310528 sectors

Units: sectors of 1 \* 512 = 512 bytes

Sector size (logical/physical): 512 bytes / 4096 bytes

I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disklabel type: gpt

Disk identifier: D54A4ED2-8267-481E-88D4-DAD0D673B888

 

Device        Start       End   Sectors  Size Type

/dev/sda1      2048   1050623   1048576  512M EFI System

/dev/sda2   1050624  35729407  34678784 16.5G Linux filesystem

/dev/sda3  35729408 234309631 198580224 94.7G Linux swap

 

root@ubuntu:\~# lscpu 

Architecture:          x86_64

CPU op-mode(s):        32-bit, 64-bit

Byte Order:            Little Endian

CPU(s):                40

On-line CPU(s) list:   0-39

Thread(s) per core:    2

Core(s) per socket:    10

Socket(s):             2

NUMA node(s):          2

Vendor ID:             GenuineIntel

CPU family:            6

Model:                 85

Model name:            Intel(R) Xeon(R) Silver 4114 CPU @ 2.20GHz

Stepping:              4

CPU MHz:               2194.870

BogoMIPS:              4390.96

Virtualization:        VT-x

L1d cache:             32K

L1i cache:             32K

L2 cache:              1024K

L3 cache:              14080K

NUMA node0 CPU(s):     0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32,34,36,38

NUMA node1 CPU(s):     1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31,33,35,37,39

Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb intel_pt tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx avx512f rdseed adx smap clflushopt clwb avx512cd xsaveopt xsavec xgetbv1 cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts

 

root@ubuntu:\~# lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT

sda      8:0    0 111.7G  0 disk

├─sda1[   8:1    0   512M  0 part /boot/efi]

├─sda2[   8:2    0  16.5G  0 part /]

└─sda3[   8:3    0  94.7G  0 part \[SWAP\]]

sr0     11:0    1   825M  0 rom  

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

网卡驱动升级：

驱动下载地址：http://www.dell.com/support/home/cn/zh/cnbsd1/drivers/driversdetails?driverId=0W76G

root@ubuntu:/home/dony# ls

Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06  Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06.tar.gz

root@ubuntu:/home/dony# rm -rf Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06.tar.gz 

root@ubuntu:/home/dony# cd Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06# ls

driver_disks  NX  release.txt  tg3-3.137s.tar.gz

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06# tar xvf tg3-3.137s.tar.gz

tg3-3.137s/

tg3-3.137s/tg3_vmware.h

tg3-3.137s/README.TXT

tg3-3.137s/esx_ioctl.h

tg3-3.137s/ChangeLog

tg3-3.137s/RELEASE.TXT

tg3-3.137s/tg3_firmware.h

tg3-3.137s/LICENSE

tg3-3.137s/tg3_compat2.h

tg3-3.137s/tg3_compat.h

tg3-3.137s/Makefile

tg3-3.137s/tg3_vmware.c

tg3-3.137s/tg3.4

tg3-3.137s/makeflags.sh

tg3-3.137s/tg3.h

tg3-3.137s/tg3.c

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06# cd tg3-3.137s/

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s# ls

ChangeLog    LICENSE   makeflags.sh  RELEASE.TXT  tg3.c          tg3_compat.h    tg3.h         tg3_vmware.h

esx_ioctl.h  Makefile  README.TXT    tg3.4        tg3_compat2.h  tg3_firmware.h  tg3_vmware.c

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s# make

sh makeflags.sh /lib/modules/4.4.0-87-generic/build  \> tg3_flags.h

make -C /lib/modules/4.4.0-87-generic/build SUBDIRS=/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s modules

make\[1\]: Entering directory \'/usr/src/linux-headers-4.4.0-87-generic\'

  CC \[M\]  /home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s/tg3.o

  Building modules, stage 2.

  MODPOST 1 modules

  CC      /home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s/tg3.mod.o

  LD \[M\]  /home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s/tg3.ko

make\[1\]: Leaving directory \'/usr/src/linux-headers-4.4.0-87-generic\'

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s# make install

make -C /lib/modules/4.4.0-87-generic/build SUBDIRS=/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s modules

make\[1\]: Entering directory \'/usr/src/linux-headers-4.4.0-87-generic\'

  Building modules, stage 2.

  MODPOST 1 modules

make\[1\]: Leaving directory \'/usr/src/linux-headers-4.4.0-87-generic\'

gzip -c tg3.4 \> tg3.4.gz

mkdir -p //lib/modules/4.4.0-87-generic/updates;

install -m 444 tg3.ko //lib/modules/4.4.0-87-generic/updates;

install -m 444 tg3.4.gz /usr/share/man/man4;\\

 

root@ubuntu:/home/dony/Bcom_LAN_20.06.05.06_NX_Linux_Source_20.06.05.06/tg3-3.137s#

 

 

root@ubuntu:\~# modinfo tg3

filename:       /lib/modules/4.4.0-87-generic/updates/tg3.ko

firmware:       tigon/tg3_tso5.bin

firmware:       tigon/tg3_tso.bin

firmware:       tigon/tg3.bin

version:        3.137s

license:        GPL

description:    Broadcom Tigon3 ethernet driver

author:         David S. Miller (davem@redhat.com) and Jeff Garzik (jgarzik@pobox.com)

srcversion:     F08A5128C99D7720B9DC264

alias:          pci:v0000106Bd00001645sv\*sd\*bc\*sc\*i\*

alias:          pci:v0000173Bd000003EAsv\*sd\*bc\*sc\*i\*

alias:          pci:v0000173Bd000003EBsv\*sd\*bc\*sc\*i\*

alias:          pci:v0000173Bd000003E9sv\*sd\*bc\*sc\*i\*

 

 

已使用 OneNote 创建。
