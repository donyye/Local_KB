\|\--读 MBR 与 GTP 头

2025年9月17日

9:13

读 MBR 与 GTP 头

dd if=/dev/sda bs=1 count=512 \| hexdump -C 

or

\# dd if=/dev/sda of=/mnt/sda.mbr bs=512 count=1    

\# hexdump -C sda.mbr

直接看是：hexdump -C /dev/sda \|more

MBR:

![[No boot-kdump_012__--读 MBR 与 GTP 头_001.png]]

 

 

GPT:

dd if=/dev/sda of=sda.gpt bs=512 count=34   使用备份GPT分区表的命

hexdump -C sda.gpt

![[No boot-kdump_012__--读 MBR 与 GTP 头_002.png]]

恢复： dd if=sda.gpt of=/dev/sda bs=512 count=34

 

====================================

非交互手动修改 Start 与 End：

\# parted -s /dev/sda unit s mkpart primary 2048 1050623

 

把 sda 改成 MBR(dos) 分区

\# parted -s /dev/sda mklabel msdos

 

把 sda 改成 GPT[  ]分区

\# parted -s /dev/sda mklabel gpt

 

已使用 OneNote 创建。
