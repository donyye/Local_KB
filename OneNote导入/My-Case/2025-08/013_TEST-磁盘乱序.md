TEST-磁盘乱序

 

关于 Linux 系统盘符乱序的问题

环境 R750 + H755F + 

  ------------ --------
  RAID0        446.6G
  Non-RAID     3.5T
  BOSS-RAID1   447.1G
                
  ------------ --------

 

Linux 磁盘命名规则是通过kernel对设备的枚举而赋予设备名字。

大概过程是[  ]Non-RAID[  --\> ]Enclosure/Slot position order --\> Virtual Drives

 

所以我们安装系统的系统盘很可能不是赋予 sda 的名字，但是对于此磁问题可以通过下面方式进行调整。

 

[ Firmware Device Order ]介绍：

<https://infohub.delltechnologies.com/zh-cn/p/firmware-device-order-for-perc-h750-h755-h350-and-h355-storage-controllers-linux-only/>

 

 

![[My-Case_2025-08_013_TEST-磁盘乱序_001.png]]

 

==== RHEL8.X 测试 ====

BIOS 修改 Firmware Device Order 为 enabled

在修改了后，RHEL8.X 测试是可以先枚举到RAID设备，然后值 Non-RAID。

![[My-Case_2025-08_013_TEST-磁盘乱序_002.png]]

 

 

修改前：

![[My-Case_2025-08_013_TEST-磁盘乱序_003.jpg]]

 

 

scsi_mod.scan=sync

此选项添加到 Kernel 也不能改变发现磁盘顺序。

 

 

==== RHEL9.X 测试 ====

RHEL9.X 需要添加：sd_mod.probe="sync"

[ Firmware Device Order ]可以不需要修改，保持默认。

 

没修改前：

![[My-Case_2025-08_013_TEST-磁盘乱序_004.png]]

 

修改后：

![[My-Case_2025-08_013_TEST-磁盘乱序_005.png]]

\
 

![[My-Case_2025-08_013_TEST-磁盘乱序_006.png]]

 

然后 # grub2-mkconfig -o /boot/grub2/grub.cfg --update-bls-cmdline 保存修改。

 

只添加 sd_mod.probe="sync" 但是 firmware device order 不是enable 也不行。

 

![[My-Case_2025-08_013_TEST-磁盘乱序_007.png]]

 

 

==== RHEL10.X 测试 ====\
需要修改 firmware device order 为 enable 

 

修改后：

![[My-Case_2025-08_013_TEST-磁盘乱序_008.png]]

 

修改前：

![[My-Case_2025-08_013_TEST-磁盘乱序_009.png]]

 

所以目前测试出来系统不同版本会有不同的方式来解决测试乱序问题，但是Redhat官方是更建议使用 UUID 或是别名的方式来挂载和使用磁盘。

 

相关参考:

<https://infohub.delltechnologies.com/zh-cn/p/firmware-device-order-for-perc-h750-h755-h350-and-h355-storage-controllers-linux-only/>

 

<https://access.redhat.com/solutions/44389#scsi_sync>\
\
<https://docs.redhat.com/zh-cn/documentation/red_hat_enterprise_linux/8/html-single/managing_storage_devices/index#con_disadvantages-of-non-persistent-naming-attributes_assembly_overview-of-persistent-naming-attributes>

 

 

 

=============================

\
 

![[My-Case_2025-08_013_TEST-磁盘乱序_010.png]]

 

 

![[My-Case_2025-08_013_TEST-磁盘乱序_011.png]]

 

 

=== RHEL8.X ===\
Firmware Device Order 为 enabled

![[My-Case_2025-08_013_TEST-磁盘乱序_012.png]]

 

 

Firmware Device Order 为 disable

![[My-Case_2025-08_013_TEST-磁盘乱序_013.png]]

 

 

=== RHEL9.X ===

RHEL9.X 需要添加：sd_mod.probe="sync"

Firmware Device Order 不需要修改，disable 和 enabled 都可以。

 

Firmware Device Order enabled 但是没有添加 sd_mod.probe="sync" 

![[My-Case_2025-08_013_TEST-磁盘乱序_014.png]]

 

在没有添加 sd_mod.probe="sync" 之前

![[My-Case_2025-08_013_TEST-磁盘乱序_015.png]]

 

修改方法：

![[My-Case_2025-08_013_TEST-磁盘乱序_016.png]]

 

修改完成后：

![[My-Case_2025-08_013_TEST-磁盘乱序_017.png]]

 

\
==== RHEL10.X 测试 ====

只需要修改 firmware device order 为 enable 

![[My-Case_2025-08_013_TEST-磁盘乱序_018.png]]

 

 

所以从测试来看，不同系统的版本会有不同的表象，但是能确保所安装系统的引导盘位为 sda。

 

Redhat 所给出的建议是使用 UUID 或是别名的方式来挂载和使用磁盘。

 

相关参考:

<https://infohub.delltechnologies.com/zh-cn/p/firmware-device-order-for-perc-h750-h755-h350-and-h355-storage-controllers-linux-only/>

 

<https://access.redhat.com/solutions/44389#scsi_sync>\
\
<https://docs.redhat.com/zh-cn/documentation/red_hat_enterprise_linux/8/html-single/managing_storage_devices/index#con_disadvantages-of-non-persistent-naming-attributes_assembly_overview-of-persistent-naming-attributes>
