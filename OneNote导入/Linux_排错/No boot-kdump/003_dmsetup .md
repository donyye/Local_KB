dmsetup

2023年11月17日

10:31

dmsetup cre-ate test-sda1 -table \'0 82502217728 linear /dev/sda 2048\'\
这个dmsetup命令的作用是使用设备映射(device mapper)机制创建一个名为test-sda1的虚拟块设备,它映射到物理磁盘/dev/sda的分区中。

创建了一个名为test-sda1的虚拟块设备,该设备线性映射到物理盘/dev/sda上的分区内容,偏移为2048扇区,长度为80GB。

- \--table:提供映射表信息,定义如何映射。
- 0:映射表中的第一行/条目索引号,从0开始。
- 82502217728:这个设备的大小,约为80GB。
- linear:表示线性直接映射。
- /dev/sda:映射的源物理设备。
- 2048:源设备的偏移量,从2048扇区开始。

 

 

dmsetup table

![[No boot-kdump_003_dmsetup _001.png]]

 

 

dumpe2fs -h /dev/mapper/test-sda1

![[No boot-kdump_003_dmsetup _002.png]]

 

![[No boot-kdump_003_dmsetup _003.png]]

 

mkdir /mnt/test-sda1

Mount -o ro /dev/mapper/test-sda1 /mnt/test-sda1

 

![[No boot-kdump_003_dmsetup _004.png]]

 

 

当前的分区表是3999744 偏移量很大，是错误的。

![[No boot-kdump_003_dmsetup _005.png]]

 

 

 

Message 看到的 sector 就是在 2048 这个位置

Nov 16 11:32:16 CNSRVBJ105 kernel: blk_update_request: I/O error, dev sda, sector 2048 op 0x1:(WRITE) flags 0x800 phys_seg 1 prio class 0

Nov 16 11:32:16 CNSRVBJ105 kernel: Buffer I/O error on dev sda1, logical block 5156405248, lost sync page write

Nov 16 11:32:16 CNSRVBJ105 kernel: Buffer I/O error on dev sda1, logical block 0, lost sync page write

Nov 16 11:32:16 CNSRVBJ105 kernel: JBD2: Error -5 detected when updating journal superblock for sda1-8.

Nov 16 11:32:16 CNSRVBJ105 kernel: EXT4-fs (sda1): I/O error while writing superblock

Nov 16 11:32:16 CNSRVBJ105 kernel: EXT4-fs error (device sda1) in ext4_do_update_inode:5371: Journal has aborted

\
......

 

 

另外，55 aa 是MBR的分区格式，0到200是MBR来写的，在200以后是GPT，也就是EFI，所以这里看出之前是使用MBR的分区格式，后来改成了GPT，所以会有记录。

![[No boot-kdump_003_dmsetup _006.png]]

 

53 ef 是EXT4的标致头

 

 

 

 

 

 

 

 

![[No boot-kdump_003_dmsetup _007.png]]

 

![[No boot-kdump_003_dmsetup _008.png]]

 

 

 

 

已使用 OneNote 创建。
