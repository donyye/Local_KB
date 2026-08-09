Create-Suse-SBD

2024年3月28日

12:56

 

![[SUSE cluster_010_Create-Suse-SBD_001.png]]

 

s1:\~ \# rpm -qa \|grep sbd

sbd-1.4.1+20200113.4b617a1-1.55.x86_64

 

创建SBD盘：

s1:\~ \# sbd -d /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_005056a0a47902 create

Initializing device /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_005056a0a47902

Creating version 2.1 header on device 3 (uuid: 09aeee93-b07f-42cf-92e0-c2656e3c0ce8)

Initializing 255 slots on device 3

Device /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_005056a0a47902 is initialized.

\#如果盘有多路径创建方法：

<https://documentation.suse.com/zh-cn/sle-ha/15-SP3/html/SLE-HA-all/cha-ha-storage-protect.html#sec-ha-storage-protect-overview>

如果您的 SBD 设备驻留在多路径组上，请使用 -1 和 -4 选项来调整要用于 SBD 的超时。

 

 

s1:\~ \# sbd -d /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_005056a0a47902 dump

==Dumping header on disk /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_005056a0a47902

Header version     : 2.1

UUID               : 09aeee93-b07f-42cf-92e0-c2656e3c0ce8

Number of slots    : 255

Sector size        : 512

Timeout (watchdog) : 5

Timeout (allocate) : 2

Timeout (loop)     : 1

Timeout (msgwait)  : 10

==Header on disk /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_005056a0a47902 is dumped

 

 

s1:\~ \# grep -Ev \'\^\\s\*#\|\^\\s\*\$\' /etc/sysconfig/sbd

SBD_PACEMAKER=yes

SBD_STARTMODE=always

SBD_DELAY_START=no

SBD_WATCHDOG_DEV=/dev/watchdog

SBD_WATCHDOG_TIMEOUT=5

SBD_TIMEOUT_ACTION=flush,reboot

SBD_MOVE_TO_ROOT_CGROUP=auto

SBD_OPTS=

SBD_DEVICE=/dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47902[   ]\<\-- 手动添加

 

每个node都需要操作：启用和启动 SBD 服务

s1:\~ \# systemctl start sbd

S2:\~ \# systemctl start sbd

 

s1:\~ \# systemctl enable sbd

s2:\~ \# systemctl enable sbd

\# sbd的守护进程就是 sbd.service，手动重启这个服务没有用，需要通过上面两步就可以自动重启。

 

s1:\~ \# sbd -d /dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47902 list

0        s1        clear        

1        s2        clear        

 

 

SBD 测试：

测试发送信息：

s2:\~ \# sbd -d /dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47902 message s1 test

 

测试 reset node , 测试过可以成功

s2:\~ \# sbd -d /dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47902 message s1 reset

 

 

或者

s1:\~ \# crm node fence s2

Fencing s2 will shut down the node and migrate any resources that are running on it! Do you want to fence s2 (y/n)? y

 

 

crm_mon -r \--\> 会一直刷新cluster状态信息

crm configure edit \--\> 编辑HA配置文件

 

 

 

已使用 OneNote 创建。
