Call Trace（hugetlbfs）

Monday, August 25, 2014

3:33 PM

ISSUE :[  \ WARNING: at fs/hugetlbfs/inode.c:xxx[ \]]

 

CASE:

kernel: WARNING: at fs/hugetlbfs/inode.c:951 hugetlb_file_setup+0x227/0x250() (Not tainted)

kernel: Hardware name: PowerEdge R710

kernel: Using mlock ulimits for SHM_HUGETLB deprecated

kernel: Modules linked in: autofs4 sunrpc bonding 8021q garp stp llc ipv6 uinput power_meter dcdbas microcode ses enclosure serio_raw iTCO_wdt iTCO_vendor_support i7core_edac edac_core sg bnx2 ext4 mbcache jbd2 dm_round_robin sr_mod cdrom sd_mod crc_t10dif pata_acpi ata_generic ata_piix qla2xxx scsi_transport_fc scsi_tgt megaraid_sas dm_multipath dm_mirror dm_region_hash dm_log dm_mod \[last unloaded: speedstep_lib\]

kernel: Pid: 12925, comm: oracle Not tainted 2.6.32-279.el6.x86_64 #1

kernel: Call Trace:

kernel: \[\<ffffffff8106b747\>\] ? warn_slowpath_common+0x87/0xc0

kernel: \[\<ffffffff8106b836\>\] ? warn_slowpath_fmt+0x46/0x50

kernel: \[\<ffffffff8114179c\>\] ? user_shm_lock+0x9c/0xc0

kernel: \[\<ffffffff811f7047\>\] ? hugetlb_file_setup+0x227/0x250

kernel: \[\<ffffffff8127cdb0\>\] ? sprintf+0x40/0x50

kernel: \[\<ffffffff81205622\>\] ? newseg+0x152/0x290

kernel: \[\<ffffffff81200881\>\] ? ipcget+0x61/0x200

kernel: \[\<ffffffff81142b4e\>\] ? remove_vma+0x6e/0x90

kernel: \[\<ffffffff810d69e2\>\] ? audit_syscall_entry+0x272/0x2a0

kernel: \[\<ffffffff812054b9\>\] ? sys_shmget+0x59/0x60

kernel: \[\<ffffffff812054d0\>\] ? newseg+0x0/0x290

kernel: \[\<ffffffff812054c0\>\] ? shm_security+0x0/0x10

kernel: \[\<ffffffff81204c20\>\] ? shm_more_checks+0x0/0x20

kernel: \[\<ffffffff8100b0f2\>\] ? system_call_fastpath+0x16/0x1b

kernel: \-\--\[ end trace ffd3d077263665de \]\-\--

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Previous version of this article suggested removing \'memlock\' limits from /etc/security/limits.conf.

This is wrong since it will prevent the application from using Huge Pages.

We\'ve requested to remove the kernel trace printed together with the warning message.

In the meantime please keep \'memlock\' limits in /etc/security/limits.conf and disregard the kernel trace since it won\'t have any effect on the functionality of your application.

 

以前的版本本文建议删除"MEMLOCK"从当属/etc/security/limits.conf限制。

这是错误的，因为它会阻止应用程序使用大内存页。

我们已经要求删除的内核跟踪与报警信息一起打印，

在此期间请保持当属/etc/security/limits.conf"MEMLOCK"限制和无视内核跟踪，因为它不会对你的应用程序的功能没有任何影响。

 

 

\-\-- chack customer limits file \-\--

 

\# vim etc/security/limits.conf

grid soft nproc 2047

grid hard nproc 16384

grid soft nofile 1024

grid hard nofile 65536

oracle soft nproc 2047

oracle hard nproc 16384

oracle soft nofile 1024

oracle hard nofile 65536

 

\# End of file

 

\*  soft  memlock   60397977

\*  hard  memlock 60397977

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

Redaht KB : [https://access.redhat.com/solutions/65846](https://access.redhat.com/solutions/65846)

 

已使用 OneNote 创建。
