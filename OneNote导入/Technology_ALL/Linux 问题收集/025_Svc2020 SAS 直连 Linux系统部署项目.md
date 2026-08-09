Svc2020 SAS 直连 Linux系统部署项目

Monday, April 24, 2017

11:01 AM

- From: Zhang1, Taotao

  Sent: Sunday, December 11, 2016 1:03 PM

  To: GC_GIDS_FE \<<GC_GIDS_FE@Dell.com>\>

  Cc: Yao, Richard \<<Richard_Yao@Dell.com>\>; Yue, Aaron \<<Aaron_Yue@DELL.com>\>

  Subject: 韶关电信局Svc2020 SAS 直连 Linux系统部署项目\--总结反省

   

  Dear all,

   

  上上周做了韶关电信局的一个项目，SCV2020 存储采用SAS 直连rhel 6.5系统环境(官方最佳实践中提示SAS直连环境推荐6.6版本，但客户执意安装6.5版本)，针对多路径的配置和类似于kernel: Buffer I/O error on device sdb, logical block 0报错处理，有以下几点说明 ：

   

  注：我在现场忽略了下文中第二点需要对Volume级别的多路径策略也进行修改，让客户对我们的部署服务很不满意，我在此深刻反省，也提醒兄弟们在以后的类似项目中注意。 

   

  一：在multipath.conf文件中需要按如下方式添加一段设置（CMl针对rhel6最佳实践第39页）：

       

  ![[Technology_ALL_Linux 问题收集_025_Svc2020 SAS 直连 Linux系统部署项目_001.jpg]]

  二：针对Volume级别的设置，SAS直连环境需要将默认的"round-robin 0"修改为"Service time 0" 

          

  ![[Technology_ALL_Linux 问题收集_025_Svc2020 SAS 直连 Linux系统部署项目_002.jpg]]

   

  三：在执行fdisk --l扫描磁盘或者重启系统类似操作，会在/var/log/messages日志文件中产生大量如下报错

  Error日志类型一

  kernel: end_request: I/O error, dev sdc, sector 0

  kernel: end_request: I/O error, dev sde, sector 0

  kernel: end_request: I/O error, dev sdj, sector 0

  kernel: end_request: I/O error, dev sdl, sector 0

  Error日志类型二

  kernel: Buffer I/O error on device sdb, logical block 1

  kernel: Buffer I/O error on device sdb, logical block 2

  kernel: Buffer I/O error on device sdb, logical block 3

   

  针对上述报错，相关文档和redhat官方给出的解释是：只要存储模式在Active/Passive状态的情况下，都会产生上述报错信息,redhat 官方给出了建议是不会对系统运行产生任何问题，可以安全的忽略。

   

  （Storage array operating in active/passive mode (see root cause for details)，

  I/O errors caused by unintentional access to passive paths are not harmful and should not cause any issues on a system.  They can be safely ignored.

   

   

  附：从Redhat官网下载的对应内容：

   Why do I see I/O errors on a RHEL system using devices from an active/passive storage array? 

  Solution Verified - Updated May 1 2015 at 3:26 AM -

  [English](https://access.redhat.com/solutions/18746)

  Environment

  - Red Hat Enterprise Linux(RHEL) 4
  - Red Hat Enterprise Linux(RHEL) 5
  - Red Hat Enterprise Linux(RHEL) 6
  - Storage array operating in active/passive mode (see root cause for details)

  Issue

  - I see I/O errors in my system logs from some of the devices on my storage array / SAN 
  - When I run LVM commands, I get I/O errors against some of the devices from my storage array 
  - Error message \"end_request: I/O error, dev sdb, sector 0\" found in messages file
  - Error message \"Buffer I/O error on device sdc, logical block 0\" found in messages file

  <!-- -->

  - Additional messages about \"sense key: Not Ready\" may also be found:

  [Raw](https://access.redhat.com/solutions/18746)

   

      kernel: end_request: I/O error, dev sdc, sector 0

   

      kernel: sd 0:0:0:3: Device not ready: \<6\>: Current: sense key: Not Ready

   

      kernel:     Add. Sense: Logical unit not ready, manual intervention required

  - These end_request: I/O error messages are also logged on system console during boot process
    - On system with multipath configured, running LVM commands sometime hang or take long time and shows following error messages:

  [Raw](https://access.redhat.com/solutions/18746)

  /dev/sde: read failed after 0 of 4096 at 0: Input/output error

  Resolution

  Note: The following applies only to I/O errors caused by accessing passive paths.  See the Root Cause and Diagnostic Steps for more information on determining whether this applies to your environment.

  - One way to cut down on the number of spurious I/O errors in the system logs is to avoid scanning passive paths with LVM commands.  This can be done with a filter in /etc/lvm/lvm.conf that [only scans devices from device-mapper-multipath, EMC PowerPath, Hitachi HDLM, or another multipath solution](https://access.redhat.com/solutions/2989), and avoids the underlying SCSI device nodes. 
  - I/O errors may be caused by any utility or program that accesses passive storage paths, so it may be necessary to configure or run them in such a way that avoids these devices.  For instance, rather than using \'fdisk -l\', specify an individual device such as \'fdisk -l /dev/mapper/mpatha.
    - Some storage arrays, such as the EMC Clariion, offer an option to enable a type of active/active mode known as ALUA.  With ALUA, path groups are established with different priorities.  Multipath software such as device-mapper-multipath will recognize these path groups and send I/O to the higher priority paths, but if I/O does end up going down a passive path it may not generate an I/O error.  If your array supports such a mode, enabling it may prevent these I/O errors.  This different access method generally requires a configuration change in the multipath software as well. 

  Note: I/O errors caused by unintentional access to passive paths are not harmful and should not cause any issues on a system.  They can be safely ignored.

  Root Cause

  - Storage arrays in a SAN are generally set up in a redundant fashion such that hosts can access logical units (LUNs) over one of many different paths.  Typically these arrays operate in one of two different modes: active/active or active/passive.  With an active/active array, I/O can be sent down any one of the paths to a LUN and it will be processed by that controller. With active/passive arrays, one controller is considered the primary for each LUN, while the other controller is a backup.  Some of these arrays will accept I/O for a LUN over the backup controller, but it will not be optimized (i.e. worse performance).  However other active/passive arrays will not accept any I/O on the backup controller for a LUN, and thus any commands sent to it will result in an I/O error.
  - In RHEL, there are a number of different commands and utilities that can send I/O to different devices, such as LVM, udev, fdisk, etc, not to mention applications such as databases, web servers, etc.  If any of these were to issue I/O to a passive path on an array that does not accept it, it would cause an I/O error in the logs.  The messages are harmless and do not indicate a problem, but they may fill up the logs or causes unwarranted concern.  As a result, some may wish to try to avoid these errors by preventing applications from accessing the passive paths.  Typically, filtering devices out from LVM will cause the majority of these errors to go away.  Likewise, avoiding commands like \'fdisk -l\' that scan all devices can reduce their frequency.  Finally, configuring any user applications that scan or access multiple devices to only access the appropriate active path or the logical multipath device (/dev/mapper/mpath\*, /dev/emcpower\*, /dev/sddlma\*, etc) can cut down on the errors as well.

  Diagnostic Steps

  With active/passive arrays, it is important to be able to distinguish between passive-path I/O errors and those that occur on active paths and indicate an actual problem.  The following  steps may help in diagnosing whether an I/O error is caused by a passive path:

  - Determine if your array is active/passive, and if so, if I/O down a  passive path will generate I/O errors.  This might be found in the  vendor\'s documentation, or can be determined by accessing a known  healthy, passive path and seeing if it generates I/O errors.  For example:

  [Raw](https://access.redhat.com/solutions/18746)

   

       \# fdisk -l /dev/sdb

  - Determine which paths for a given LUN are passive.
    - [If using  device-mapper-multipath, you can look at the output of \'multipath -ll  \[map name\]\' and look at which devices are contained in the passive path  group (the one with the lower prio score):]

  [Raw](https://access.redhat.com/solutions/18746)

              \# multipath -ll mydevicemydevice (16046017086137300787192b56e2fde11) dm-4 DGC,RAID 5

              \[size=266G\]\[features=1 queue_if_no_path\]\[hwhandler=1 emc\]\[rw\]

              \\\_ round-robin 0 \[prio=2\]\[active\]

               \\\_ 3:0:1:0 sdc 8:32  \[active\]\[ready\]

               \\\_ 4:0:1:0 sde 8:64  \[active\]\[ready\]

              \\\_ round-robin 0 \[prio=0\]\[enabled\]

               \\\_ 3:0:0:0 sdb 8:16  \[active\]\[ready\]

               \\\_ 4:0:0:0 sdd 8:48  \[active\]\[ready\]

  In this example, devices /dev/sdb and /dev/sdd from an EMC Clariion (aka DGC RAID 5)  are in the passive path group (denoted by the lower prio score of 0),  and thus I/O errors referencing those devices may be safely ignored.

  - If using EMC PowerPath, you can look at the output of \'powermt display dev=all\' and see which devices have a storage interface (Stor Interf.) different than the current owner. 

  [Raw](https://access.redhat.com/solutions/18746)

  Pseudo name=emcpowera

  CLARiiON ID=99-0000-000 \[\~physical\]

  Logical device ID=600601F0D057000018FC7845F46FE011 \[LUN 0\]

  state=alive; policy=BasicFailover; priority=0; queued-IOs=0;

  Owner: default=SP B, current=SP B Array failover mode: 1

  ==============================================================================

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-- Host \-\-\-\-\-\-\-\-\-\-\-\-\-\--   - Stor -   \-- I/O Path \--  \-- Stats \-\--

  ###  HW Path               I/O Paths    Interf.   Mode    State   Q-IOs Errors

  ==============================================================================

    12 qla2xxx                  sdd       SP-A    active  alive       0      0

    11 qla2xxx                  sdj       SP-B    active  alive       0      0

    10 qla2xxx                  sdg       SP-A    active  alive       0      0

    13 qla2xxx                  sdk       SP-B    active  alive       0      0\`

   

                                                                                             最佳实践中提到的支持的redhat操作系统版本

  ![[Technology_ALL_Linux 问题收集_025_Svc2020 SAS 直连 Linux系统部署项目_003.jpg]]

 

已使用 OneNote 创建。
