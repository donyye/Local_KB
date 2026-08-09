|\_ Record-05

 

 

===========================

 

Hi, Depei

 

目前RHEL7.9 已经EOL了，所以Red Hat已经不再支持RHEL7，如果客户坚持要用7, 就只能买ELS, 而ELS的购买也只能向red hat直接购买。 

 

<https://access.redhat.com/support/policy/updates/errata>

 

![[My-Case_2025-03_006___ Record-05_001.png]]

 

 

的是为了客户体验，我们只能做单次分析，请与客户设置好期望值

 

===========================================

 

从系统日服分析，在3月5日 ，node01 在 09:52 左右发生了LVM监控超时，资源停止，Pacemaker 认为 HISTDB_LVM 资源异常，引发服务被切换其它的节点。

 

Node01:

Mar  5 09:52:01 RFMCSWB01 systemd: Started Session 2903374 of user xmcs.

Mar  5 09:52:01 RFMCSWB01 systemd: Started Session 2903375 of user xmcs.

Mar  5 09:52:01 RFMCSWB01 systemd: Started Session 2903376 of user xmcs.

[Mar  5 09:52:05 RFMCSWB01 lrmd[9098]: warning: HISTDB_LVM_monitor_10000 process (PID 57937) timed out]

[Mar  5 09:52:05 RFMCSWB01 lrmd[9098]: warning: HISTDB_LVM_monitor_10000:57937 - timed out after 30000ms]

[Mar  5 09:52:05 RFMCSWB01 crmd[9101]:   error: Result of monitor operation for HISTDB_LVM on RFMCSWB01: Timed Out]

Mar  5 09:52:05 RFMCSWB01 su: (to oracle) root on none

Mar  5 09:52:05 RFMCSWB01 systemd: Started Session c465362 of user oracle.

Mar  5 09:52:06 RFMCSWB01 oralsnr(HISTDB_ORALSNR)[58766]: INFO: Listener LISTENER_XMCSHIST stopped: #012LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 05-MAR-2025 09:52:05#012#012Copyright (c) 1991, 2020, Oracle.  All rights reserved.#012#012Connecting to (ADDRESS=(PROTOCOL=IPC)(KEY=xmcshist))#012The command completed successfully#012Last login: Wed Mar  5 09:50:41 CST 2025

Mar  5 09:52:06 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_ORALSNR on RFMCSWB01: 0 (ok)

Mar  5 09:52:06 RFMCSWB01 su: (to oracle) root on none

Mar  5 09:52:06 RFMCSWB01 systemd: Started Session c465363 of user oracle.

Mar  5 09:52:06 RFMCSWB01 su: (to oracle) root on none

Mar  5 09:52:06 RFMCSWB01 systemd: Started Session c465364 of user oracle.

Mar  5 09:52:11 RFMCSWB01 systemd: Removed slice User Slice of oracle.

Mar  5 09:52:11 RFMCSWB01 oracle(HISTDB_ORACLE)[58843]: INFO: Oracle instance xmcshist stopped: ORACLE instance shut down.

Mar  5 09:52:12 RFMCSWB01 oracle(HISTDB_ORACLE)[58843]: INFO: Cleaning up for xmcshist

Mar  5 09:52:12 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_ORACLE on RFMCSWB01: 0 (ok)

Mar  5 09:52:12 RFMCSWB01 IPaddr2(HISTDB_VIP)[59109]: INFO: IP status = ok, IP_CIP=

Mar  5 09:52:12 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_VIP on RFMCSWB01: 0 (ok)

Mar  5 09:52:12 RFMCSWB01 Filesystem(HISTDB_FILESYSTEM)[59163]: INFO: Running stop for /dev/my_vg/my_lv on /histdata

Mar  5 09:52:12 RFMCSWB01 Filesystem(HISTDB_FILESYSTEM)[59163]: INFO: Trying to unmount /histdata

Mar  5 09:52:12 RFMCSWB01 avahi-daemon[1715]: Withdrawing address record for 172.25.32.7 on bond0.

Mar  5 09:52:18 RFMCSWB01 Filesystem(HISTDB_FILESYSTEM)[59163]: INFO: unmounted /histdata successfully

Mar  5 09:52:18 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_FILESYSTEM on RFMCSWB01: 0 (ok)

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO: Deactivating volume group my_vg

Mar  5 09:52:44 RFMCSWB01 multipathd: dm-4: remove map (uevent)

Mar  5 09:52:44 RFMCSWB01 multipathd: dm-4: devmap not registered, can't remove

Mar  5 09:52:44 RFMCSWB01 multipathd: dm-4: remove map (uevent)

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO:  0 logical volume(s) in volume group "my_vg" now active

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO: LVM Volume my_vg is not available (stopped)

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO: Stripping tag, pacemaker

Mar  5 09:52:44 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_LVM on RFMCSWB01: 0 (ok)

......

 

Node02:

Mar  5 09:51:01 RFMCSWB02 systemd: Started Session 2903357 of user xmcs.

Mar  5 09:51:01 RFMCSWB02 systemd: Started Session 2903356 of user xmcs.

Mar  5 09:51:13 RFMCSWB02 crmd[9067]:  notice: State transition S_IDLE -\> S_POLICY_ENGINE

Mar  5 09:51:13 RFMCSWB02 pengine[9065]:  notice: On loss of CCM Quorum: Ignore

Mar  5 09:51:13 RFMCSWB02 pengine[9065]:  notice: Calculated transition 62302, saving inputs in /var/lib/pacemaker/pengine/pe-input-3525.bz2

Mar  5 09:51:13 RFMCSWB02 crmd[9067]:  notice: Transition 62302 (Complete=0, Pending=0, Fired=0, Skipped=0, Incomplete=0, Source=/var/lib/pacemaker/pengine/pe-input-3525.bz2): Complete

Mar  5 09:51:13 RFMCSWB02 crmd[9067]:  notice: State transition S_TRANSITION_ENGINE -\> S_IDLE

Mar  5 09:52:01 RFMCSWB02 systemd: Started Session 2903358 of user xmcs.

Mar  5 09:52:01 RFMCSWB02 systemd: Started Session 2903360 of user xmcs.

Mar  5 09:52:01 RFMCSWB02 systemd: Started Session 2903359 of user xmcs.

Mar  5 09:52:05 RFMCSWB02 crmd[9067]:  notice: State transition S_IDLE -\> S_POLICY_ENGINE

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: On loss of CCM Quorum: Ignore

Mar  5 09:52:05 RFMCSWB02 pengine[9065]: warning: Processing failed monitor of HISTDB_LVM on RFMCSWB01: unknown error

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Recover    HISTDB_LVM             (              RFMCSWB01 )

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_FILESYSTEM      (              RFMCSWB01 )   due to required HISTDB_LVM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_VIP             (              RFMCSWB01 )   due to required HISTDB_FILESYSTEM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORACLE          (              RFMCSWB01 )   due to required HISTDB_VIP start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORALSNR         (              RFMCSWB01 )   due to required HISTDB_ORACLE start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: Calculated transition 62303, saving inputs in /var/lib/pacemaker/pengine/pe-input-3526.bz2

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: On loss of CCM Quorum: Ignore

Mar  5 09:52:05 RFMCSWB02 pengine[9065]: warning: Processing failed monitor of HISTDB_LVM on RFMCSWB01: unknown error

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Recover    HISTDB_LVM             (              RFMCSWB01 )

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_FILESYSTEM      (              RFMCSWB01 )   due to required HISTDB_LVM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_VIP             (              RFMCSWB01 )   due to required HISTDB_FILESYSTEM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORACLE          (              RFMCSWB01 )   due to required HISTDB_VIP start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORALSNR         (              RFMCSWB01 )   due to required HISTDB_ORACLE start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: Calculated transition 62304, saving inputs in /var/lib/pacemaker/pengine/pe-input-3527.bz2

Mar  5 09:52:05 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_ORALSNR_stop_0 on RFMCSWB01

Mar  5 09:52:06 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_ORACLE_stop_0 on RFMCSWB01

Mar  5 09:52:12 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_VIP_stop_0 on RFMCSWB01

Mar  5 09:52:12 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_FILESYSTEM_stop_0 on RFMCSWB01

Mar  5 09:52:18 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_LVM_stop_0 on RFMCSWB01

Mar  5 09:52:44 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_LVM_start_0 on RFMCSWB01

Mar  5 09:52:45 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_LVM_monitor_10000 on RFMCSWB01

Mar  5 09:52:45 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_FILESYSTEM_start_0 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_FILESYSTEM_monitor_20000 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_VIP_start_0 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_VIP_monitor_10000 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_ORACLE_start_0 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_ORACLE_monitor_120000 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_ORALSNR_start_0 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_ORALSNR_monitor_10000 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Transition 62304 (Complete=19, Pending=0, Fired=0, Skipped=0, Incomplete=0, Source=/var/lib/pacemaker/pengine/pe-input-3527.bz2): Complete

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: State transition S_TRANSITION_ENGINE -\> S_IDLE

Mar  5 09:53:01 RFMCSWB02 systemd: Started Session 2903361 of user xmcs.

......

 

 

总结：

LVM 命令执行时间超过默认监控阈值（30秒）触发级联故障，路径设备子路径波动可能会影响到 LVM 监测，所以这个问题有下面几个可能。

 

1）出问题的时候突然有大量的IO负载或是CPU负载导致有路径抖动。[  ]（系统没记录到这方面的数据）

 

2）在此问题发生之前，node01有看到有USB设备插入，USB 设备插入导致的多路径异常与 HA 存储无直接关联，但可能通过 SCSI 总线抖动或多路径守护进程负载间接引发 LVM 监控超时。

相关日志记录：

Mar  5 09:34:03 RFMCSWB01 kernel: hid-generic 0003:0557:2304.0015: usb_submit_urb(ctrl) failed: -19

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: new SuperSpeed Gen 1 USB device number 9 using xhci_hcd

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: New USB device found, idVendor=03f0, idProduct=0825, bcdDevice= 2.83

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: Product: RDX Ext USB 3.0

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: Manufacturer: HPE

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: SerialNumber: 003C7131202F

Mar  5 09:34:07 RFMCSWB01 kernel: usb-storage 2-1:1.0: USB Mass Storage device detected

Mar  5 09:34:07 RFMCSWB01 kernel: scsi host22: usb-storage 2-1:1.0

Mar  5 09:34:08 RFMCSWB01 kernel: scsi 22:0:0:0: Direct-Access     HPE      RDX              0283 PQ: 0 ANSI: 6

Mar  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: Attached scsi generic sg10 type 0

Mar  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: Power-on or device reset occurred

[Mar  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] Attached SCSI removable disk]

Mar  5 09:34:08 RFMCSWB01 multipathd: sdg: add path (uevent)

Mar  5 09:34:08 RFMCSWB01 multipathd: sdg: failed to get path uid

Mar  5 09:34:08 RFMCSWB01 multipathd: uevent trigger error

Mar  5 09:34:18 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] 1953516976 512-byte logical blocks: (1.00 TB/931 GiB)

Mar  5 09:34:18 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA

Mar  5 09:34:18 RFMCSWB01 kernel: sdg: sdg1

Mar  5 09:34:40 RFMCSWB01 su: (to oracle) root on none

......

 

参考KB：

<https://access.redhat.com/solutions/1527673>

 

 

 

 

===============================

 

[root@rh85 01]# cat sos_commands/multipath/multipath\_-ll

mpatha (368ccf09800eb939f803bf3292218c21c) dm-2 DellEMC ,PowerStore      

size=13T features='1 queue_if_no_path' hwhandler='0' wp=rw

|-+- policy='queue-length 0' prio=50 status=active

| |- 13:0:1:1  sdc 8:32 active ready running

| `- 1:0:1:1   sdd 8:48 active ready running

`-+- policy='queue-length 0' prio=10 status=enabled

  |- 13:0:0:1  sde 8:64 active ready running

  `- 1:0:0:1   sdf 8:80 active ready running

 

 

[root@rh85 01]# cat sos_commands/block/lsblk

NAME                 MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT

sda                    8:0    0   2.5T  0 disk  

`-sda1                 8:1    0   2.5T  0 part  /data

sdb                    8:16   0 837.8G  0 disk  

|-sdb1                 8:17   0   200M  0 part  /boot/efi

|-sdb2                 8:18   0   485M  0 part  /boot

|-sdb3                 8:19   0    64G  0 part  

| `-rhel-swap        253:1    0    64G  0 lvm   [SWAP]

`-sdb4                 8:20   0 773.1G  0 part  

  `-VolGroup-lv_root 253:0    0 773.1G  0 lvm   /

sdc                    8:32   0  12.6T  0 disk  

|-sdc1                 8:33   0  12.6T  0 part  

`-mpatha             253:2    0  12.6T  0 mpath

  `-mpatha1          253:3    0  12.6T  0 part  

    `-my_vg-my_lv    253:4    0  12.6T  0 lvm   /histdata

sdd                    8:48   0  12.6T  0 disk  

|-sdd1                 8:49   0  12.6T  0 part  

`-mpatha             253:2    0  12.6T  0 mpath

  `-mpatha1          253:3    0  12.6T  0 part  

    `-my_vg-my_lv    253:4    0  12.6T  0 lvm   /histdata

sde                    8:64   0  12.6T  0 disk  

|-sde1                 8:65   0  12.6T  0 part  

`-mpatha             253:2    0  12.6T  0 mpath

  `-mpatha1          253:3    0  12.6T  0 part  

    `-my_vg-my_lv    253:4    0  12.6T  0 lvm   /histdata

sdf                    8:80   0  12.6T  0 disk  

|-sdf1                 8:81   0  12.6T  0 part  

`-mpatha             253:2    0  12.6T  0 mpath

  `-mpatha1          253:3    0  12.6T  0 part  

    `-my_vg-my_lv    253:4    0  12.6T  0 lvm   /histdata

 

 

 

 

[root@rh85 01]# grep -iE 'scsi|sdg|mpatha' var/log/messages | grep 'Mar  5 09:3[4-9]'

Mar  5 09:34:07 RFMCSWB01 kernel: scsi host22: usb-storage 2-1:1.0

Mar  5 09:34:08 RFMCSWB01 kernel: scsi 22:0:0:0: Direct-Access     HPE      RDX              0283 PQ: 0 ANSI: 6

Mar  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: Attached scsi generic sg10 type 0

Mar  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] Attached SCSI removable disk

Mar  5 09:34:08 RFMCSWB01 multipathd: sdg: add path (uevent)

Mar  5 09:34:08 RFMCSWB01 multipathd: sdg: failed to get path uid

Mar  5 09:34:18 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] 1953516976 512-byte logical blocks: (1.00 TB/931 GiB)

Mar  5 09:34:18 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA

Mar  5 09:34:18 RFMCSWB01 kernel: sdg: sdg1

Mar  5 09:35:31 RFMCSWB01 kernel: EXT4-fs (sdg1): mounted filesystem with ordered data mode. Opts: (null)

 

 

 

================

 

 

 

从系统日服分析，在3月5日 ，node01 在 09:52 左右发生了LVM监控超时，资源停止，Pacemaker 认为 HISTDB_LVM 资源异常，引发服务被切换其它的节点。

 

Node01:

Mar  5 09:52:01 RFMCSWB01 systemd: Started Session 2903374 of user xmcs.

Mar  5 09:52:01 RFMCSWB01 systemd: Started Session 2903375 of user xmcs.

Mar  5 09:52:01 RFMCSWB01 systemd: Started Session 2903376 of user xmcs.

Mar[  5 09:52:05 RFMCSWB01 lrmd[9098]: warning: HISTDB_LVM_monitor_10000 process (PID 57937) timed out]

Mar[  5 09:52:05 RFMCSWB01 lrmd[9098]: warning: HISTDB_LVM_monitor_10000:57937 - timed out after 30000ms]

Mar[  5 09:52:05 RFMCSWB01 crmd[9101]:   error: Result of monitor operation for HISTDB_LVM on RFMCSWB01: Timed Out]

Mar  5 09:52:05 RFMCSWB01 su: (to oracle) root on none

Mar  5 09:52:05 RFMCSWB01 systemd: Started Session c465362 of user oracle.

Mar  5 09:52:06 RFMCSWB01 oralsnr(HISTDB_ORALSNR)[58766]: INFO: Listener LISTENER_XMCSHIST stopped: #012LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 05-MAR-2025 09:52:05#012#012Copyright (c) 1991, 2020, Oracle.  All rights reserved.#012#012Connecting to (ADDRESS=(PROTOCOL=IPC)(KEY=xmcshist))#012The command completed successfully#012Last login: Wed Mar  5 09:50:41 CST 2025

Mar  5 09:52:06 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_ORALSNR on RFMCSWB01: 0 (ok)

Mar  5 09:52:06 RFMCSWB01 su: (to oracle) root on none

Mar  5 09:52:06 RFMCSWB01 systemd: Started Session c465363 of user oracle.

Mar  5 09:52:06 RFMCSWB01 su: (to oracle) root on none

Mar  5 09:52:06 RFMCSWB01 systemd: Started Session c465364 of user oracle.

Mar  5 09:52:11 RFMCSWB01 systemd: Removed slice User Slice of oracle.

Mar  5 09:52:11 RFMCSWB01 oracle(HISTDB_ORACLE)[58843]: INFO: Oracle instance xmcshist stopped: ORACLE instance shut down.

Mar  5 09:52:12 RFMCSWB01 oracle(HISTDB_ORACLE)[58843]: INFO: Cleaning up for xmcshist

Mar  5 09:52:12 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_ORACLE on RFMCSWB01: 0 (ok)

Mar  5 09:52:12 RFMCSWB01 IPaddr2(HISTDB_VIP)[59109]: INFO: IP status = ok, IP_CIP=

Mar  5 09:52:12 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_VIP on RFMCSWB01: 0 (ok)

Mar  5 09:52:12 RFMCSWB01 Filesystem(HISTDB_FILESYSTEM)[59163]: INFO: Running stop for /dev/my_vg/my_lv on /histdata

Mar  5 09:52:12 RFMCSWB01 Filesystem(HISTDB_FILESYSTEM)[59163]: INFO: Trying to unmount /histdata

Mar  5 09:52:12 RFMCSWB01 avahi-daemon[1715]: Withdrawing address record for 172.25.32.7 on bond0.

Mar  5 09:52:18 RFMCSWB01 Filesystem(HISTDB_FILESYSTEM)[59163]: INFO: unmounted /histdata successfully

Mar  5 09:52:18 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_FILESYSTEM on RFMCSWB01: 0 (ok)

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO: Deactivating volume group my_vg

Mar  5 09:52:44 RFMCSWB01 multipathd: dm-4: remove map (uevent)

Mar  5 09:52:44 RFMCSWB01 multipathd: dm-4: devmap not registered, can't remove

Mar  5 09:52:44 RFMCSWB01 multipathd: dm-4: remove map (uevent)

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO:  0 logical volume(s) in volume group "my_vg" now active

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO: LVM Volume my_vg is not available (stopped)

Mar  5 09:52:44 RFMCSWB01 LVM(HISTDB_LVM)[59365]: INFO: Stripping tag, pacemaker

Mar  5 09:52:44 RFMCSWB01 crmd[9101]:  notice: Result of stop operation for HISTDB_LVM on RFMCSWB01: 0 (ok)

......

 

Node02:

Mar  5 09:51:01 RFMCSWB02 systemd: Started Session 2903357 of user xmcs.

Mar  5 09:51:01 RFMCSWB02 systemd: Started Session 2903356 of user xmcs.

Mar  5 09:51:13 RFMCSWB02 crmd[9067]:  notice: State transition S_IDLE -\> S_POLICY_ENGINE

Mar  5 09:51:13 RFMCSWB02 pengine[9065]:  notice: On loss of CCM Quorum: Ignore

Mar  5 09:51:13 RFMCSWB02 pengine[9065]:  notice: Calculated transition 62302, saving inputs in /var/lib/pacemaker/pengine/pe-input-3525.bz2

Mar  5 09:51:13 RFMCSWB02 crmd[9067]:  notice: Transition 62302 (Complete=0, Pending=0, Fired=0, Skipped=0, Incomplete=0, Source=/var/lib/pacemaker/pengine/pe-input-3525.bz2): Complete

Mar  5 09:51:13 RFMCSWB02 crmd[9067]:  notice: State transition S_TRANSITION_ENGINE -\> S_IDLE

Mar  5 09:52:01 RFMCSWB02 systemd: Started Session 2903358 of user xmcs.

Mar  5 09:52:01 RFMCSWB02 systemd: Started Session 2903360 of user xmcs.

Mar  5 09:52:01 RFMCSWB02 systemd: Started Session 2903359 of user xmcs.

Mar  5 09:52:05 RFMCSWB02 crmd[9067]:  notice: State transition S_IDLE -\> S_POLICY_ENGINE

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: On loss of CCM Quorum: Ignore

Mar  5 09:52:05 RFMCSWB02 pengine[9065]: warning: Processing failed monitor of HISTDB_LVM on RFMCSWB01: unknown error

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Recover    HISTDB_LVM             (              RFMCSWB01 )

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_FILESYSTEM      (              RFMCSWB01 )   due to required HISTDB_LVM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_VIP             (              RFMCSWB01 )   due to required HISTDB_FILESYSTEM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORACLE          (              RFMCSWB01 )   due to required HISTDB_VIP start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORALSNR         (              RFMCSWB01 )   due to required HISTDB_ORACLE start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: Calculated transition 62303, saving inputs in /var/lib/pacemaker/pengine/pe-input-3526.bz2

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: On loss of CCM Quorum: Ignore

Mar  5 09:52:05 RFMCSWB02 pengine[9065]: warning: Processing failed monitor of HISTDB_LVM on RFMCSWB01: unknown error

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Recover    HISTDB_LVM             (              RFMCSWB01 )

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_FILESYSTEM      (              RFMCSWB01 )   due to required HISTDB_LVM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_VIP             (              RFMCSWB01 )   due to required HISTDB_FILESYSTEM start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORACLE          (              RFMCSWB01 )   due to required HISTDB_VIP start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice:  * Restart    HISTDB_ORALSNR         (              RFMCSWB01 )   due to required HISTDB_ORACLE start

Mar  5 09:52:05 RFMCSWB02 pengine[9065]:  notice: Calculated transition 62304, saving inputs in /var/lib/pacemaker/pengine/pe-input-3527.bz2

Mar  5 09:52:05 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_ORALSNR_stop_0 on RFMCSWB01

Mar  5 09:52:06 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_ORACLE_stop_0 on RFMCSWB01

Mar  5 09:52:12 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_VIP_stop_0 on RFMCSWB01

Mar  5 09:52:12 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_FILESYSTEM_stop_0 on RFMCSWB01

Mar  5 09:52:18 RFMCSWB02 crmd[9067]:  notice: Initiating stop operation HISTDB_LVM_stop_0 on RFMCSWB01

Mar  5 09:52:44 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_LVM_start_0 on RFMCSWB01

Mar  5 09:52:45 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_LVM_monitor_10000 on RFMCSWB01

Mar  5 09:52:45 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_FILESYSTEM_start_0 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_FILESYSTEM_monitor_20000 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_VIP_start_0 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_VIP_monitor_10000 on RFMCSWB01

Mar  5 09:52:46 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_ORACLE_start_0 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_ORACLE_monitor_120000 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Initiating start operation HISTDB_ORALSNR_start_0 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Initiating monitor operation HISTDB_ORALSNR_monitor_10000 on RFMCSWB01

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: Transition 62304 (Complete=19, Pending=0, Fired=0, Skipped=0, Incomplete=0, Source=/var/lib/pacemaker/pengine/pe-input-3527.bz2): Complete

Mar  5 09:52:59 RFMCSWB02 crmd[9067]:  notice: State transition S_TRANSITION_ENGINE -\> S_IDLE

Mar  5 09:53:01 RFMCSWB02 systemd: Started Session 2903361 of user xmcs.

......

 

在此问题发生之前，node01有看到有USB设备插入，USB 设备插入导致的多路径异常与 HA 存储无直接关联，但可能通过 SCSI 总线扰动或多路径守护进程负载间接引发 LVM 监控超时。

 

Mar  5 09:34:03 RFMCSWB01 kernel: hid-generic 0003:0557:2304.0015: usb_submit_urb(ctrl) failed: -19

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: new SuperSpeed Gen 1 USB device number 9 using xhci_hcd

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: New USB device found, idVendor=03f0, idProduct=0825, bcdDevice= 2.83

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: Product: RDX Ext USB 3.0

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: Manufacturer: HPE

Mar  5 09:34:07 RFMCSWB01 kernel: usb 2-1: SerialNumber: 003C7131202F

Mar  5 09:34:07 RFMCSWB01 kernel: usb-storage 2-1:1.0: USB Mass Storage device detected

Mar  5 09:34:07 RFMCSWB01 kernel: scsi host22: usb-storage 2-1:1.0

Mar  5 09:34:08 RFMCSWB01 kernel: scsi 22:0:0:0: Direct-Access     HPE      RDX              0283 PQ: 0 ANSI: 6

Mar  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: Attached scsi generic sg10 type 0

Mar[  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: Power-on or device reset occurred]

Mar[  5 09:34:08 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] Attached SCSI removable disk]

Mar  5 09:34:08 RFMCSWB01 multipathd: sdg: add path (uevent)

Mar  5 09:34:08 RFMCSWB01 multipathd: sdg: failed to get path uid

Mar  5 09:34:08 RFMCSWB01 multipathd: uevent trigger error

Mar  5 09:34:18 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] 1953516976 512-byte logical blocks: (1.00 TB/931 GiB)

Mar  5 09:34:18 RFMCSWB01 kernel: sd 22:0:0:0: [sdg] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA

Mar[  5 09:34:18 RFMCSWB01 kernel: sdg: sdg1]

Mar  5 09:34:40 RFMCSWB01 su: (to oracle) root on none

......

 

后续建议把不需要的设置加入到 multipath的 blacklist 里，防止此事情的发生。

比如：

blacklist {

[    device   # ]根据实际 vendor/product 标识过滤

}

 

 

 

 

filename:       /lib/modules/3.10.0-1160.el7.x86_64/kernel/drivers/scsi/qla2xxx/qla2xxx.ko.xz

firmware:       ql2700_fw.bin

firmware:       ql8300_fw.bin

firmware:       ql2600_fw.bin

firmware:       ql2500_fw.bin

firmware:       ql2400_fw.bin

firmware:       ql2322_fw.bin

firmware:       ql2300_fw.bin

firmware:       ql2200_fw.bin

firmware:       ql2100_fw.bin

version:        10.01.00.22.07.9-k

license:        GPL

description:    QLogic Fibre Channel HBA Driver

author:         QLogic Corporation

retpoline:      Y

rhelversion:    7.9
