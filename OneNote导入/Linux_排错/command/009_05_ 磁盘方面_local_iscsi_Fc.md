05: 磁盘方面\_local_iscsi_Fc

2023年9月11日

12:00

1. 用于本地磁盘rescan，比如VM的磁盘扩大了。\
[\# echo 1 \> /sys/block/\[DEVICE\]/device/rescan]  

or

\# for i in sdb sdc sdd sde; do echo 1 \> /sys/block/\$i/device/rescan; done

 

2. iscsi 磁盘重新扫描

\# for i in \`ls /sys/class/scsi_host/\`; do echo \"- - -\" \>\> /sys/class/scsi_host/\$i/scan; done 

 

3. 用于FC 磁盘扫描

\# for i in \`ls /sys/class/fc_host\`; do echo \"1\" \>\> /sys/class/fc_host/\$i/issue_lip; done 

or

for i in /sys/class/fc_host/\*/issue_lip; do echo 1 \> \$i; done

or

使用 rescan-scsi-bus.sh

yum -y install sg3_utils\
scsi-rescan -mis

 

 

4. PCIE 的 remove 与 rescan

# echo \"1\" \> /sys/bus/pci/devices/0000\\:13\\:00.0/remove

\# echo \"1\" \> /sys/bus/pci/resca

 

5. lsblk  列出所有的块设备，而且还能显示他们之间的依赖关系

\# lsblk -f

 

 

![[command_009_05_ 磁盘方面_local_iscsi_Fc_001.png]]

 

扩容文件系统存储空间

某些文件系统类型，如ext4和xfs，支持在线调整文件大小操作。下面的过程概述了在线调整文件系统大小的一般步骤(假设使用了非分区lun)。

1. 在 LUNs 与 ME Storage Manager 中扩展大小。

 

2. 在主机系统上执行SCSI扫描，刷新每个LUN路径上的分区表，然后重新加载多路径设备。

\# resscan -scsi-bus.sh \--resize

 

3.重新加载多路径设备。如果禁用了多路径设备，请跳过此步骤。

\# multipathd -k \"resize map mpathb\"

or

\# multipathd resize map mpathb

 

4. 如果文件系统位于LVM之上，则展开逻辑卷。如果文件系统不在LVM中，则跳过此步骤。

\# lvresize -L \$SNEW_SIZE /dev/vgme4/1vme4

or

\# lvresize -r -l +100%free /dev/vgme4/1vme4

如果是在物理卷上 \# pvresize /dev/mapper/mpatha

 

5. 自动在线扩展文件系统大小至最大

\# xfs_growfs -d /me4_fs (for xfs)

或

\# resize2fs /dev/mapper/mpath(适用于ext4)

 

==============================

 

\> lshw 列出硬件详细信息，包括磁盘及控制器。

 

 

\> hdparm 查看和测试硬盘性能及识别信息。

[\[root@r760xd2 \~\]# hdparm -I /dev/sda]

 

/dev/sda:

 

ATA device, with non-removable media

Standards:

Likely used: 1

Configuration:

Logical                max        current

cylinders        0        0

heads                0        0

sectors/track        0        0

\--

Logical/Physical Sector size:           512 bytes

device size with M = 1024\*1024:           0 MBytes

device size with M = 1000\*1000:           0 MBytes

cache/buffer size  = unknown

Capabilities:

IORDY not likely

Cannot perform double-word IO

R/W multiple sector transfer: not supported

DMA: not supported

PIO: pio0

 

 

 

已使用 OneNote 创建。
