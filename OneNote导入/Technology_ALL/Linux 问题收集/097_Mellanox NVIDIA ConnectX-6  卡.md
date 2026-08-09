Mellanox NVIDIA ConnectX-6 [ ]卡

2024年1月15日

12:14

Mellanox NVIDIA ConnectX-6

 

\-\-\-\-\-\-\-\-\-\-\-- 硬件上的确认

如果是显示下面的信息，说明是带 Infiniband 功能的。

![[Technology_ALL_Linux 问题收集_097_Mellanox NVIDIA ConnectX-6  卡_001.png]]

 

在BIOS上是可以切换的，默认是 Infiniband。如果只想用以太网卡功能，那就需要切换一下。

![[Technology_ALL_Linux 问题收集_097_Mellanox NVIDIA ConnectX-6  卡_002.png]]

 

 

如果是这种描述，说明这个卡说明没有带[  ]Infiniband 功能，在BIOS上也不会有上图的设置。

Nvidia ConnectX-6 Lx Dual Port 10/25GbE SFP28, No Crypto, OCP NIC 3.0

 

\-\-\-\-\-\-\-\-\-\-\-- 系统上的确认

\# lspci -v \| grep Mellanox

86:00.0 Network controller \[0207\]: Mellanox Technologies MT28908A0 Family

Subsystem: Mellanox Technologies Device 0014

86:00.1 Network controller \[0207\]: Mellanox Technologies MT28908A0 Family

Subsystem: Mellanox Technologies Device 0014

 

驱动名字叫 ： mlx5_core

 

驱动安装方式：

<https://img-en.fs.com/file/user_manual/connectx-6-dx-ethernet-adapter-cards-user-manual.pdf>

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Mellanox NVIDIA ConnectX-6 驱动安装 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\[root@localhost \~\]# cat /etc/redhat-release

CentOS Linux release 7.9.2009 (Core)

 

 

\[root@localhost \~\]# lspci -v \|grep Mellanox

af:00.0 Ethernet controller: Mellanox Technologies MT2892 Family \[ConnectX-6 Dx\]

Subsystem: Mellanox Technologies Device 0058

af:00.1 Ethernet controller: Mellanox Technologies MT2892 Family \[ConnectX-6 Dx\]

Subsystem: Mellanox Technologies Device 0058

 

 

\[root@localhost \~\]# modinfo mlx5_core

filename:       /lib/modules/3.10.0-1160.el7.x86\_64/kernel/drivers/net/ethernet/mellanox/mlx5/core/mlx5_core.ko.xz

version:[        5.0-0]

license:        Dual BSD/GPL

description:    Mellanox 5th generation network adapters (ConnectX series) core driver

author:         Eli Cohen \<eli@mellanox.com\>

retpoline:      Y

rhelversion:    7.9

srcversion:     0863D8DB0A42249ABDB85F2

alias:          pci:v000015B3d0000A2D3sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000A2D2sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Esv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Dsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Csv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Bsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Asv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001019sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001018sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001017sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001016sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001015sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001014sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001013sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001012sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001011sv\*sd\*bc\*sc\*i\*

depends:        devlink,ptp,mlxfw

intree:         Y

vermagic:       3.10.0-1160.el7.x86_64 SMP mod_unload modversions

signer:         CentOS Linux kernel signing key

sig_key:        E1:FD:B0:E2:A7:E8:61:A1:D1:CA:80:A2:3D:CF:0D:BA:3A:A4:AD:F5

sig_hashalgo:   sha256

parm:           debug_mask:debug mask: 1 = dump cmd data, 2 = dump cmd exec time, 3 = both. Default=0 (uint)

parm:           prof_sel:profile selector. Valid range 0 - 2 (uint)

 

 

1. 通过Dell官网驱动进行安装

<https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=wr43r&oscode=rhe70&productcode=poweredge-r650xs>

![[Technology_ALL_Linux 问题收集_097_Mellanox NVIDIA ConnectX-6  卡_003.png]]

 

 

最后还是有三个包yum是没有的，无法安装。

yum install valgrind-devel libmnl-devel libnl3-devel

\# 最后无法完成

 

2. 通过官网下载的驱动 程序进行安装

<https://img-en.fs.com/file/user_manual/connectx-6-dx-ethernet-adapter-cards-user-manual.pdf>

使用官网提供的 MLNX_OFED_LINUX-23.10-1.1.9.0-rhel7.9-x86_64.iso 来安装。

 

mount -l ro,loop /root/MLNX_OFED_LINUX-23.10-1.1.9.0-rhel7.9-x86_64.iso /mnt/mlx

 

\[root@localhost \~\]# ll /mnt/mlx/

total 344

-r-xr-xr-x. 1 root root   3077 Dec  6 01:55 common_installers.pl

-r-xr-xr-x. 1 root root  25303 Dec  6 01:55 common.pl

-r-xr-xr-x. 1 root root  26975 Dec  6 01:55 create_mlnx_ofed_installers.pl

-r\--r\--r\--. 1 root root      8 Dec  6 01:55 distro

dr-xr-xr-x. 6 root root   2048 Dec  6 01:55 docs

-r-xr-xr-x. 1 root root   5418 Dec  6 01:55 is_kmp_compat.sh

-r\--r\--r\--. 1 root root    956 Dec  6 01:55 LICENSE

-r-xr-xr-x. 1 root root  21287 Dec  6 01:55 mlnx_add_kernel_support.sh

-r-xr-xr-x. 1 root root 226740 Dec  6 01:55 mlnxofedinstall

-r\--r\--r\--. 1 root root   1771 Dec  6 01:55 RPM-GPG-KEY-Mellanox

dr-xr-xr-x. 3 root root  18432 Dec  6 01:57 RPMS

dr-xr-xr-x. 2 root root   2048 Dec  6 01:55 src

-r-xr-xr-x. 1 root root  15844 Dec  6 01:55 uninstall.sh

 

 

\[root@localhost \~\]# /mnt/mlx/mlnxofedinstall \--skip-distro-check \--without-fw-update

Logs dir: /tmp/MLNX_OFED_LINUX.27804.logs

General log file: /tmp/MLNX_OFED_LINUX.27804.logs/general.log

Verifying KMP rpms compatibility with target kernel\...

Error: One or more required packages for installing MLNX_OFED_LINUX are missing.

Please install the missing packages using your Linux distribution Package Management tool.

Run:

yum install libusbx tcl fuse-libs tk

\[root@localhost \~\]#

[\[root@localhost \~\]# yum install libusbx tcl fuse-libs tk][   \--\> ]缺的包，用yum安装。

 

使用下面方式进行安装

[ \--skip-distro-check ][  ]跳过distro匹配检查

\--without-fw-update[  ]跳过固件升级

\[root@localhost \~\]# /mnt/mlx/mlnxofedinstall \--skip-distro-check \--without-fw-update

Logs dir: /tmp/MLNX_OFED_LINUX.29047.logs

General log file: /tmp/MLNX_OFED_LINUX.29047.logs/general.log

Verifying KMP rpms compatibility with target kernel\...

This program will install the MLNX_OFED_LINUX package on your machine.

Note that all other Mellanox, OEM, OFED, RDMA or Distribution IB packages will be removed.

Those packages are removed due to conflicts with MLNX_OFED_LINUX, do not reinstall them.

 

Do you want to continue?\[y/N\]:y

 

 

Starting MLNX_OFED_LINUX-23.10-1.1.9.0 installation \...

 

Preparing\...                          \########################################

mlnx-tools-23.10-0.2310119            \########################################

Preparing\...                          \########################################

libibverbs-2307mlnx47-1.2310119       \########################################

Installing mlnx-ofa_kernel RPM

Preparing\...                          \########################################

Updating / installing\...

mlnx-ofa_kernel-23.10-OFED.23.10.1.1.9########################################

Configured /etc/security/limits.conf

Installing kmod-mlnx-ofa_kernel 23.10 RPM

Preparing\...                          \########################################

kmod-mlnx-ofa_kernel-23.10-OFED.23.10.########################################

Installing mlnx-ofa_kernel-devel RPM

Preparing\...                          \########################################

Updating / installing\...

mlnx-ofa_kernel-devel-23.10-OFED.23.10########################################

Installing mlnx-ofa_kernel-source RPM

Preparing\...                          \########################################

Updating / installing\...

mlnx-ofa_kernel-source-23.10-OFED.23.1########################################

Installing kmod-kernel-mft-mlnx 4.26.1 RPM

Preparing\...                          \########################################

kmod-kernel-mft-mlnx-4.26.1-1.rhel7u9 \########################################

Installing knem RPM

Preparing\...                          \########################################

Updating / installing\...

knem-1.1.4.90mlnx3-OFED.23.10.0.2.1.1.########################################

Installing kmod-knem 1.1.4.90mlnx3 RPM

Preparing\...                          \########################################

kmod-knem-1.1.4.90mlnx3-OFED.23.10.0.2########################################

Installing xpmem RPM

Preparing\...                          \########################################

Updating / installing\...

xpmem-2.7.3-1.2310055.rhel7u9         \########################################

Installing kmod-xpmem 2.7.3 RPM

Preparing\...                          \########################################

kmod-xpmem-2.7.3-1.2310055.rhel7u9.rhe########################################

Installing kmod-iser 23.10 RPM

Preparing\...                          \########################################

kmod-iser-23.10-OFED.23.10.1.1.9.1.rhe########################################

Installing kmod-srp 23.10 RPM

Preparing\...                          \########################################

kmod-srp-23.10-OFED.23.10.1.1.9.1.rhel########################################

Installing kmod-isert 23.10 RPM

Preparing\...                          \########################################

kmod-isert-23.10-OFED.23.10.1.1.9.1.rh########################################

Installing libxpmem 2.7.3 RPM

Preparing\...                          \########################################

Updating / installing\...

libxpmem-2.7.3-1.2310055.rhel7u9      \########################################

Installing user level RPMs:

Preparing\...                          \########################################

ofed-scripts-23.10-OFED.23.10.1.1.9   \########################################

Preparing\...                          \########################################

rdma-core-2307mlnx47-1.2310119        \########################################

Preparing\...                          \########################################

librdmacm-2307mlnx47-1.2310119        \########################################

Preparing\...                          \########################################

libibumad-2307mlnx47-1.2310119        \########################################

Preparing\...                          \########################################

infiniband-diags-2307mlnx47-1.2310119 \########################################

Preparing\...                          \########################################

rdma-core-devel-2307mlnx47-1.2310119  \########################################

Preparing\...                          \########################################

libibverbs-utils-2307mlnx47-1.2310119 \########################################

Preparing\...                          \########################################

ibsim-0.12-1.2310055                  \########################################

Preparing\...                          \########################################

ibacm-2307mlnx47-1.2310119            \########################################

Preparing\...                          \########################################

librdmacm-utils-2307mlnx47-1.2310119  \########################################

Preparing\...                          \########################################

opensm-libs-5.17.0.MLNX20231105.d437ae########################################

Preparing\...                          \########################################

opensm-5.17.0.MLNX20231105.d437ae0a-0.########################################

Preparing\...                          \########################################

opensm-devel-5.17.0.MLNX20231105.d437a########################################

Preparing\...                          \########################################

opensm-static-5.17.0.MLNX20231105.d437########################################

Preparing\...                          \########################################

perftest-23.10.0-0.29.g0705c22.2310055########################################

Preparing\...                          \########################################

mstflint-4.16.1-2.2310055             \########################################

Preparing\...                          \########################################

mft-4.26.1-3                          \########################################

Preparing\...                          \########################################

srp_daemon-2307mlnx47-1.2310119       \########################################

Preparing\...                          \########################################

ibutils2-2.1.1-0.1.MLNX20231105.g79770########################################

Preparing\...                          \########################################

ibdump-6.0.0-1.2310055                \########################################

Preparing\...                          \########################################

dpcp-1.1.43-1.2310055                 \########################################

Preparing\...                          \########################################

ucx-1.16.0-1.2310119                  \########################################

Preparing\...                          \########################################

ucx-devel-1.16.0-1.2310119            \########################################

Preparing\...                          \########################################

sharp-3.5.1.MLNX20231116.7fcef5af-1.23########################################

Preparing\...                          \########################################

ucx-cma-1.16.0-1.2310119              \########################################

Preparing\...                          \########################################

ucx-ib-1.16.0-1.2310119               \########################################

Preparing\...                          \########################################

ucx-rdmacm-1.16.0-1.2310119           \########################################

Preparing\...                          \########################################

ucx-knem-1.16.0-1.2310119             \########################################

Preparing\...                          \########################################

ucx-xpmem-1.16.0-1.2310119            \########################################

Preparing\...                          \########################################

hcoll-4.8.3223-1.2310055              \########################################

Preparing\...                          \########################################

openmpi-4.1.7a1-1.2310055             \########################################

Preparing\...                          \########################################

mpitests_openmpi-3.2.21-8418f75.231005########################################

Preparing\...                          \########################################

mlnx-ethtool-6.4-1.2310055            \########################################

Preparing\...                          \########################################

mlnx-iproute2-6.4.0-1.2310055         \########################################

Preparing\...                          \########################################

rshim-2.0.17-0.g0caa378               \########################################

Preparing\...                          \########################################

clusterkit-1.11.442-1.2310055         \########################################

Preparing\...                          \########################################

ibarr-0.1.3-1.2310055                 \########################################

Preparing\...                          \########################################

mlnxofed-docs-23.10-1.1.9.0           \########################################

Device (af:00.0):

af:00.0 Ethernet controller: Mellanox Technologies MT2892 Family \[ConnectX-6 Dx\]

Link Width: x16

PCI Link Speed: 8GT/s

 

Device (af:00.1):

af:00.1 Ethernet controller: Mellanox Technologies MT2892 Family \[ConnectX-6 Dx\]

Link Width: x16

PCI Link Speed: 8GT/s

 

 

Installation finished successfully.

 

 

Preparing\...                          \################################# \[100%\]

Updating / installing\...

   1:mlnx-fw-updater-23.10-1.1.9.0    \################################# \[100%\]

 

Added \'RUN_FW_UPDATER_ONBOOT=no to /etc/infiniband/openib.conf

 

Skipping FW update.

To load the new driver, run:

/etc/init.d/openibd restart[     ]\--\> 安装完成后提示运行一下

\[root@localhost \~\]#

 

\[root@localhost \~\]# /etc/init.d/openibd restart

Unloading HCA driver:                                      \[  OK  \]

Loading HCA driver and Access Layer:                       \[  OK  \]

 

查看到已经变了

[\[root@localhost \~\]# modinfo mlx5_core ][   ]

filename:       /lib/modules/3.10.0-1160.el7.x86_64/extra/mlnx-ofa_kernel/drivers/net/ethernet/mellanox/mlx5/core/mlx5_core.ko

alias:          auxiliary:mlx5_core.eth-rep

alias:          auxiliary:mlx5_core.eth

basedon:        Korg 6.3-rc3

version:[        23.10-1.1.9]

license:        Dual BSD/GPL

description:    Mellanox 5th generation network adapters (ConnectX series) core driver

author:         Eli Cohen \<eli@mellanox.com\>

retpoline:      Y

rhelversion:    7.9

srcversion:     125C01E371A30A6BF4F48D7

alias:          pci:v000015B3d0000A2DFsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000A2DCsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000A2D6sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000A2D3sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000A2D2sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001023sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001021sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Fsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Esv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Dsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Csv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Bsv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d0000101Asv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001019sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001018sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001017sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001016sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001015sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001014sv\*sd\*bc\*sc\*i\*

alias:          pci:v000015B3d00001013sv\*sd\*bc\*sc\*i\*

depends:        devlink,mlx_compat,auxiliary,ptp,mlxfw,psample

vermagic:       3.10.0-1160.el7.x86_64 SMP mod_unload modversions

signer:         Mellanox Technologies signing key

sig_key:        61:FE:B0:74:FC:72:92:F9:58:41:93:86:FF:DD:9D:5C:A9:99:E4:03

sig_hashalgo:   sha256

parm:           num_of_groups:Eswitch offloads number of big groups in FDB table. Valid range 1 - 1024. Default 15 (uint)

parm:           debug_mask:debug mask: 1 = dump cmd data, 2 = dump cmd exec time, 3 = both. Default=0 (uint)

parm:           prof_sel:profile selector. Valid range 0 - 3 (uint)

parm:           probe_vf:probe VFs or not, 0 = not probe, 1 = probe. Default = 1 (bool)

 

 

已使用 OneNote 创建。
