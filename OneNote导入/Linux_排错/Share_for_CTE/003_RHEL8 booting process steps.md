RHEL8 booting process steps

2022年4月25日

15:57

![[Share_for_CTE_003_RHEL8 booting process steps_001.png]]

 

 

POST

BIOS

HDD(/dev/sda)

 

MBR \[简单地说，这个阶段包括将引导加载程序(MBR 和 GRUB/LILO)加载到内存中，以启动内核。[\]]

![[Share_for_CTE_003_RHEL8 booting process steps_002.png]]

前446是主引导，后64是分区表，最后6个字节是MBR校验时间戳。

\#注意，MBR不能直接加载kernel，因为它不知道文件系统的概念，因为每个文件会提供一个支持的我呢见系统驱动程序为引导程序，所以就有了下面的grub2，它提供了文件系统和文件系统驱动程序的信息。

 

GRUB2 (Grand Unified Bootloader) v2

它的功能是在启动时接管BIOS，然后加载本身，再将kernel加载到内存中。

一旦kernel被加载，GRUB的工作就完成了。

 

kernel, initramfs

被加载到内存里的一个小kernel, 它主要目的就时加载一些必须的硬件驱动。如阵列卡驱动。

 

kernel Initializes all hardware

当找到真正的kernel后，kernel所带的所有硬件驱动被激活，如网卡驱动等。

 

Executes /sbin/init

Systemd executes initrd.target

 

 

MBR与 GUID分区

MBR 最多4个住分区或3个住分区和一个扩展分区与多个逻辑分区。

GUID 是可以128个分区

 

 

已使用 OneNote 创建。
