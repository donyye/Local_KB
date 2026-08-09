BIOS 与 UEFI

2025年9月17日

9:02

UEFI ：

[\[root@localhost \~\]# fdisk -l]

Disk /dev/nvme0n1: 447.1 GiB, 480036519936 bytes, 937571328 sectors

Units: sectors of 1 \* 512 = 512 bytes

Sector size (logical/physical): 512 bytes / 512 bytes

I/O size (minimum/optimal): 512 bytes / 512 bytes

Disklabel type: gpt

Disk identifier: 159D0E6F-0822-438A-BCC6-A0F8A02008A0

 

Device            Start       End   Sectors  Size Type

/dev/nvme0n1p1     2048    411647    409600  200M EFI System

/dev/nvme0n1p2   411648   2508799   2097152    1G Microsoft basic data

/dev/nvme0n1p3  2508800  35016703  32507904 15.5G Linux swap

/dev/nvme0n1p4 35016704 454447103 419430400  200G Microsoft basic data

 

 

[\[root@localhost \~\]# parted -s /dev/nvme0n1 unit s print]

Model: NVMe Device (nvme)

Disk /dev/nvme0n1: 937571328s

Sector size (logical/physical): 512B/512B

Partition Table: gpt

Disk Flags:

 

Number  Start      End         Size        File system     Name                  Flags

 1      2048s      411647s     409600s     fat16           EFI System Partition  boot, esp

 2      411648s    2508799s    2097152s    xfs                                   msftdata

 3      2508800s   35016703s   32507904s   linux-swap(v1)                        swap

 4      35016704s  454447103s  419430400s  xfs

 

非交互手动修改 Start 与 End：

parted -s /dev/sda unit s mkpart primary 2048 411647

 

BIOS：

[\[root@localhost \~\]# fdisk -l]

Disk /dev/sda: 17.2 GB, 17179869184 bytes, 33554432 sectors

Units = sectors of 1 \* 512 = 512 bytes

Sector size (logical/physical): 512 bytes / 512 bytes

I/O size (minimum/optimal): 512 bytes / 512 bytes

Disk label type: dos

Disk identifier: 0x000b81e6

 

   Device Boot      Start         End      Blocks   Id  System

/dev/sda1   \*        2048     2099199     1048576   83  Linux

/dev/sda2         2099200    33554431    15727616   8e  Linux LVM

 

[\[root@localhost \~\]# parted -s /dev/sda unit s print]

Model: VMware Virtual disk (scsi)

Disk /dev/sda: 33554432s

Sector size (logical/physical): 512B/512B

Partition Table: msdos

Disk Flags:

 

Number  Start     End        Size       Type     File system  Flags

 1      2048s     2099199s   2097152s   primary  xfs          boot

 2      2099200s  33554431s  31455232s  primary               lvm

 

 

对比：

![[No boot-kdump_011_BIOS 与 UEFI_001.png]]

 

备份分区与恢复：

》备份

\# sfdisk -d /dev/nvme0n1 \> /etc/nvme0n1.bak

![[No boot-kdump_011_BIOS 与 UEFI_002.png]]

 

》恢复

sfdisk /dev/nvme0n1 \< /etc/nvme0n1.bak

\# 恢复只是恢复分区表，不擦除数据，但是如果恢复了一个错误的分区表，那么磁盘上的数据也会无法访问。

 

比如下面故意恢复错的分区

会告诉你它原来是个 BOS 分区，但是还是被恢复成 GPT ，所以此sda数据已经无法读取

![[No boot-kdump_011_BIOS 与 UEFI_003.png]]

 

 

如果是 UEFI 损坏想修复请看： 案例1：UEFI 分区表修复\_gdisk

 

 

 

 

已使用 OneNote 创建。
