NVMe Unable to install RHEL 7

2020年6月29日

8:48

Unable to install RHEL 7 on C6525 using an NVMe PCIe SSD (DPN: N2TFG).

无法使用 NVMe PCIe SSD (DPN: N2TFG)在 C6525上安装 RHEL 7

 

 

DESCRIPTION

描述

On a C6525 system with 256 Core CPUs and PM1733/1735 devices plugged in, RHEL 7.6 installation crashes.

在一个有256个核心 cpu 和 pm1733 / 1735设备的 C6525系统上，RHEL 7.6安装崩溃。

Using RHEL 7.7/7.8/7.9 installations does not crash, but NVMe device is not detected during and after installation.

使用 RHEL 7.7 / 7.8 / 7.9安装不会崩溃，但是在安装期间和之后不会检测到 NVMe 设备。

 

Analysis :

分析:

The issue started manifesting after upgrading device firmware from version 0.1.4 to 1.0.2. The difference in firmware versions are:

问题是在升级了0.1.4至1.0.2版本的设备固件后才开始显现。不同的固件版本有:

FW version -  0.1.4 -\> 63 I/O queues + 1 admin queue i.e. total 64 per port

Fw 版本-0.1.4-\> 63 i / o 队列 + 1个管理队列，即每个端口总共64个

FW version -  1.0.2-\> 256 I/O queues + 1 admin queue i.e. total 257 per port

 

With 257 queues, RHEL 7 tries to create 257 queues but fails. The issue is observed only in RHEL 7. The issue is not observed in RHEL 8.1/8.2  and SLES 15 SP1.

 

 

Fw version-1.0.2-\> 256 i / o queue + 1 admin queue，即每个端口总共257个队列，RHEL 7尝试创建257个队列，但失败了。 这个问题只在 RHEL 7中观察到。 在 RHEL 8.1 / 8.2和 SLES 15 SP1中没有观察到这个问题。

SOLUTION

解决方案

The workaround from Dell Engineering teams is to add 'nr_cpus=255' kernel boot parameter.

The workaround is needed only if the system has \>= 256 CPUs.

来自 Dell Engineering 团队的解决方案是添加' nr \_ cpus = 255'内核引导参数。 只有当系统具有 \> = 256个 cpu 时才需要解决方案。

Issue is not observed in RHEL 8.1/8.2 and SLES 15 SP1.

在 RHEL 8.1 / 8.2和 SLES 15 SP1中没有观察到这个问题。

The issue is not observed in Reventon/Sesto with 256C CPUs.

在 reventon / sesto 的256C cpu 中没有观察到这个问题。

INTERNAL_NOTES

内部笔记

JIT-166739

N2TFG

C6525

NVMe SSD

 

From \<<https://kb.dell.com/infocenter/index?page=content&id=SLN321593&actp=SEARCH&viewlocale=en_US&searchid=1593391286553>\>

 

已使用 OneNote 创建。
