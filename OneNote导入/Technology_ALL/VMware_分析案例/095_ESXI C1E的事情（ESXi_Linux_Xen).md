ESXI C1E的事情（ESXi/Linux/Xen)

2018年11月21日

16:04

  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   C1E的事情（ESXi/Linux/Xen)
  From      Yin, Guoxun
  To        Zeng, Jackie; Xiong, John; Cheng, Felson; Huang, Dongwei
  Cc        CN XMN TS ENT L2 SME
  Sent      2018年11月21日 11:45
  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

顺便share给大家，

ESXi里面是有C1E的kernel参数的(没有CSTATS参数)，而且ESXi启动的时候会检测BIOS是否打开了C1E

![[Technology_ALL_VMware_分析案例_095_ESXI C1E的事情（ESXi_Linux_Xen)_001.png]]

 

检查Kernel parameter中的当前C1E设置

![[Technology_ALL_VMware_分析案例_095_ESXI C1E的事情（ESXi_Linux_Xen)_002.png]]

 

关闭ESXi中的C1E参数可以通过如下命令来执行，然后运行下/sbin/auto-backup.sh，重启后才会生效

![[Technology_ALL_VMware_分析案例_095_ESXI C1E的事情（ESXi_Linux_Xen)_003.png]]

 

 

目前来讲，在BIOS中关闭C1E/CSTATS对于现行版本的ESXi即有效（没有凭据文档，仅仅是数年来的CASE观察所得）

但是对于Linux系统来讲，BIOS关闭C1E Linux OS仍然可能会绕过BIOS启用C1E，Xen Server同理， 请注意如下KB和如下片段：

 

[https://access.redhat.com/solutions/202743](https://access.redhat.com/solutions/202743)

Why the OS might ignore BIOS settings

The OS might ignore BIOS settings based on the idle driver which is in use. If one uses intel_idle (the default on intel machines) the OS can ignore ACPI and BIOS settings, i.e. the driver can re-enable the C-states. In case one disables intel_idle and uses the older acpi_idle driver the OS should follow the BIOS settings. One can disable the intel_idle driver by:

- passing intel_idle.max_cstate=0 to kernel boot command line or
- passing idle=\* (where \* can be e.g. poll, i.e. idle=poll)

In such case the intel_idle will not load during boot and the machine should use acpi_idle driver. One can check what driver is in use by:

[Raw](https://access.redhat.com/solutions/202743)

\# cat /sys/devices/system/cpu/cpuidle/current_driver

intel_idle

 

 

已使用 OneNote 创建。
