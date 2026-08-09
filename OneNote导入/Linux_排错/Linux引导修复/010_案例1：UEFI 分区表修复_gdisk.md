案例1：UEFI 分区表修复\_gdisk

2023年4月11日

16:17

UEFI + GPT 硬盘\
硬件自检完成后，BIOS 会从找到的第一个 EFI 分区中加载并执行 bootloader 程序 (\<EFI-partition\>/EFI/gentoo/grubx64.efi)。执行完 bootloader 之后，GRUB 就能找到 /boot 目录 (包含 GRUB 的附加模块、内核和 initramfs 映像) 所在的分区，并用自带的文件系统驱动读取 /boot 目录。最后 GRUB 会将内核和 initramfs 映像装载到内存中，并将控制权交接到内核。

 

由于GPT分区会在磁盘的最后34个扇区保存分区表等信息的备份，所有可用通过 gdisk 命令来修复已被破坏的分区表。

 

使用的测试环境是 vm RHEL8.7 系统 UEFI 

 

通过 DD 命令破坏分区表

\[root@localhost \~\]# dd if=/dev/zero of=/dev/sda bs=1 count=64 skip=446 seek=446

64+0 records in

64+0 records out

64 bytes copied, 0.000286747 s, 223 kB/s

\# 从第446个字节开始，用64个字节的零填充。

 

系统无法正常启动，如下图。它会循环这个过程

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_001.png]]

 

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_002.png]]

 

接下来是用了 LSI 来登录到系统。

 

然后看到sda就是原来的系统分分区，大小60G。

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_003.png]]

 

使用hexdump命令看奥前512已经被抹0了。

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_004.png]]

 

使用 gdisk 进行修复，一般系统自带会有。如果没有安装一下 gdisk-1.0.3-11.el8.x86_64

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_005.png]]

因为我们的是UEFI所以选的是2 GPT，然后"？"输出一下所有命令。

 

这里选择了 p 查看了分区情况，还是可用看到分区的，这可能与 UEFI 有分区的备份有关系。

然后选择"w"退出，这时它会询问你是否进行修复，选择"y"就自动修复并完成了。

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_006.png]]

 

 

一些对比：

修复前：

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_004.png]]

 

修复后：

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_007.png]]

 

 

修复前：

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_008.png]]

看不到sda的分区，只看到个设备

 

修复后：

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_009.png]]

能看到sda的分区信息

 

重启系统后能正常登录系统，修复成功。

 

==================

 

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_010.png]]

 

================== RHEL7.9

用R750尝试做一次这个实验，记录整个过程

[\[root@localhost \~\]# dd if=/dev/zero of=/dev/sd]c bs=1 count=64 skip=446 seek=446

64+0 records in

64+0 records out

64 bytes (64 B) copied, 0.0003479 s, 184 kB/s

因为sdc安装的是 RHEL7.9 的系统，里面还有其他盘安装了其它的系统。

重新后无法找到 redhat了。因为还有个ubuntu系统，跑去启动那个了。

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_011.png]]

 

进入ubuntu 系统后查看。

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_012.png]]

 

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_013.png]]

 

这个ubuntu是安装在 nvme0n1 上，所以看到正常的应该是这样。

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_014.png]]

 

尝试使用 gdisk 修复：

root@user1:\~# gdisk /dev/sdc 

GPT fdisk (gdisk) version 1.0.8

 

Partition table scan:

  MBR: MBR only

  BSD: not present

  APM: not present

  GPT: present

 

Found valid MBR and GPT. Which do you want to use?

 1 - MBR

 2 - GPT

 3 - Create blank GPT

 

Your answer: 2

Using GPT and creating fresh protective MBR.

 

Command (? for help): ?  

b        back up GPT data to a file

c        change a partition\'s name

d        delete a partition

i        show detailed information on a partition

l        list known partition types

n        add a new partition

o        create a new empty GUID partition table (GPT)

p        print the partition table

q        quit without saving changes

r        recovery and transformation options (experts only)

s        sort partitions

t        change a partition\'s type code

v        verify disk

w        write table to disk and exit

x        extra functionality (experts only)

?        print this menu

 

Command (? for help): p[   ][ \--\> ]查看了一下分区是否还在

Disk /dev/sdc: 937571968 sectors, 447.1 GiB

Model: DELLBOSS VD     

Sector size (logical/physical): 512/4096 bytes

Disk identifier (GUID): 5922691E-83FF-45A2-834B-1DD02A0E2D1C

Partition table holds up to 128 entries

Main partition table begins at sector 2 and ends at sector 33

First usable sector is 34, last usable sector is 937571934

Partitions will be aligned on 2048-sector boundaries

Total free space is 3645 sectors (1.8 MiB)

 

Number  Start (sector)    End (sector)  Size       Code  Name

[   1            2048          411647   200.0 MiB   EF00  EFI System Partition][  ]\--\> 分区还在

   2          411648         2508799   1024.0 MiB  0700  

   3         2508800       937570303   445.9 GiB   8E00  

 

Command (? for help): w[   ]\--\>w退出

 

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING

PARTITIONS!!

 

Do you want to proceed? (Y/N): y[  ]\--\> 退出时会提示你是否需要修复，选择y

OK; writing new GUID partition table (GPT) to /dev/sdc.

Warning: The kernel is still using the old partition table.

The new table will be used at the next reboot or after you

run partprobe(8) or kpartx(8)

The operation has completed successfully.[  \--\> ]修复完成

 

修复完后重新启动系统，会到之前的RHEL7.9

![[Linux引导修复_010_案例1：UEFI 分区表修复_gdisk_015.png]]

 

==完成==

 

 

使用 efibootmgr 测试：

\[root@7525 \~\]# efibootmgr

BootCurrent: 000E[  ][  ]\<\-- 目前启动的项目是 000E，它是[  Red Hat Enterprise Linux]

BootOrder: 000E,0002,0010,0000,0009,0008,000A

Boot0000\* ubuntu

Boot0002\* Embedded NIC 1 Port 1 Partition 1

Boot0003\* Virtual Floppy

Boot0004\* BRCM MBA Slot E101 v21.6.2

Boot0005\* Hard drive C:

Boot0006\* Virtual CD/DVD

Boot0007\* BRCM MBA Slot E100 v21.6.2

Boot0008\* ubuntu

Boot0009\* CentOS

Boot000A\* Virtual CD/DVD

Boot000C\* IBA 40G Slot 4100 v1142

Boot000E\* Red Hat Enterprise Linux

Boot0010\* VMware ESXi

 

删除 000E 项：

\[root@7525 \~\]# efibootmgr -b 000E -B

BootCurrent: 000E

BootOrder: 0002,0010,0000,0009,0008,000A,0001

Boot0000\* ubuntu

Boot0001\* Virtual Floppy

Boot0002\* Embedded NIC 1 Port 1 Partition 1

Boot0003\* Virtual Floppy

Boot0004\* BRCM MBA Slot E101 v21.6.2

Boot0005\* Hard drive C:

Boot0006\* Virtual CD/DVD

Boot0007\* BRCM MBA Slot E100 v21.6.2

Boot0008\* ubuntu

Boot0009\* CentOS

Boot000A\* Virtual CD/DVD

Boot000C\* IBA 40G Slot 4100 v1142

Boot0010\* VMware ESXi

 

系统重启后会找引导到的[  Boot Device]，如下。

\[root@7525 \~\]# efibootmgr

BootCurrent: 0001

BootOrder: 000A,0002,0010,0000,0009,0008,0001

Boot0000\* ubuntu

Boot0001\* EFI Fixed Disk Boot Device 1

Boot0002\* Embedded NIC 1 Port 1 Partition 1

Boot0003\* Virtual Floppy

Boot0004\* BRCM MBA Slot E101 v21.6.2

Boot0005\* Hard drive C:

Boot0006\* Virtual CD/DVD

Boot0007\* BRCM MBA Slot E100 v21.6.2

Boot0008\* ubuntu

Boot0009\* CentOS

Boot000A\* redhat

Boot000C\* IBA 40G Slot 4100 v1142

Boot0010\* VMware ESXi

 

通过 -o 参数调整 0008 ，Ubuntu 系统先启动。

\[root@7525 \~\]# efibootmgr -o 0008

BootCurrent: 0001

BootOrder: 0008

Boot0000\* ubuntu

Boot0001\* EFI Fixed Disk Boot Device 1

Boot0002\* Embedded NIC 1 Port 1 Partition 1

Boot0003\* Virtual Floppy

Boot0004\* BRCM MBA Slot E101 v21.6.2

Boot0005\* Hard drive C:

Boot0006\* Virtual CD/DVD

Boot0007\* BRCM MBA Slot E100 v21.6.2

Boot0008\* ubuntu

Boot0009\* CentOS

Boot000A\* redhat

Boot000C\* IBA 40G Slot 4100 v1142

Boot0010\* VMware ESXi

 

 

 

已使用 OneNote 创建。
