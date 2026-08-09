案例6: shim 版本导致boot issue

2024年2月21日

16:21

[https://access.redhat.com/solutions/7016419](https://access.redhat.com/solutions/7016419)\
\
RHEL7/UEFI: Boot hangs after updating shim to \"shim-x64-15.6-3.el7_9\"

 

Environment

- Red Hat Enterprise Linux 7
  - UEFI

  <!-- -->

  - shim-x64-15.6-3.el7_9

Issue

- After updating latest patches, including shim-x64-15.6-3.el7_9, the system fails to boot, the Grub menu is not displayed and system hangs

Resolution

Red Hat is working on this issue, it is tracked by [BZ 2211029 - Shim 15.6 update breaks Broadwell compatibility](https://bugzilla.redhat.com/show_bug.cgi?id=2211029).

To help us understand the exact root cause, which seems to happen on specific hardware for now, please open a case on the Customer Portal referencing this solution and provide a sosreport of the affected system.

So far the issue has been seen on some NEC and Fujitsu hardware:

- Fujitsu
  - Primergy RX2540 M2 at BIOS level V5.0.0.11 R1.31.0
  - Primergy RX2530 M2 at BIOS level V5.0.0.11 R1.30.0
  - Primequest 2800B2 at BIOS level 01.92
  - Primequest 2800B3 at BIOS level 01.53
- NEC T110h, R110h-1, R120g-2E, T120g at unknown BIOS level

 

Procedure to recover the system

1.  Boot the system on the RHEL7.9 DVD in Troubleshooting Mode\
    [Raw](https://access.redhat.com/solutions/7016419#)\
    Troubleshooting \--\> Rescue a Red Hat Enterprise Linux\
    \
    Once booted, select:\
    [Raw](https://access.redhat.com/solutions/7016419#)\
    1) Continue
2.  Setup the networking
3.  Enter the chroot and downgrade shim-x64 package to shim-x64-15-11.el7 and its dependency\
    [Raw](https://access.redhat.com/solutions/7016419#)\
    sh-4.2# chroot /mnt/sysimage\
    bash-4.2# yum -y downgrade shim-x64 mokutil
4.  Exit twice to reboot and confirm the system boots properly\
    [Raw](https://access.redhat.com/solutions/7016419#)\
    bash-4.2# exit\
    sh-4.2# exit
5.  Add an exclusion to the yum configuration to avoid installing the packages\
    [Raw](https://access.redhat.com/solutions/7016419#)\
    \# grep \^exclude /etc/yum.conf\
    exclude=shim-x64-15.6-3.el7_9,mokutil-15.6-3.el7_9

Root Cause

The root cause seems an issue with the UEFI firmware shipped by the vendors, which happens when not enough space is available in the NVRAM to create the Shim variables.

 

根本原因似乎是供应商提供的 UEFI 固件存在问题，当 NVRAM 中没有足够的空间来创建 Shim 变量时，就会发生这种情况。

\-\-\-\-\-\-\-\-\-\--

Shim 是一个在 UEFI 系统中运行的预引导加载程序。它的主要作用是加载和执行另一个应用程序，通常是 GRUB 引导加载程序。

 

 

 

 

 

红屏：RSoD (Red Screen of Death) 

<https://access.redhat.com/solutions/5504801>

Boot failure with message: \"The system detected an exception during the UEFI pre-boot environment\"

 

Environment

- Red Hat Enterprise Linux 7
- shim versions: 15-7.el7_8 & 15-8.el7_8
- Red Hat Enterprise Linux 8
- shim version 15-14.el8_2 & 15-15.el8_2

Issue

- Boot failure with message: \"The system detected an exception during the UEFI pre-boot environment\"

启动失败，并显示消息："系统在 UEFI 预启动环境中检测到异常"

- RSoD (Red Screen of Death) on boot with updated grub shim

SoD（红屏死机）启动，更新了 grub 填充码

 

Resolution

- For RHEL 7, please update the shim to version 15-11.el7 or later.
- For RHEL 8, a fixed version of the shim is under development, and will be released soon. Please contact Red Hat Support if you need immediate access to this update.

 

 

 

已使用 OneNote 创建。
