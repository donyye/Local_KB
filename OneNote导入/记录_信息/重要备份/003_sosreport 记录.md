sosreport 记录

2023年2月9日

14:08

![[记录_信息_重要备份_003_sosreport 记录_001.png]]

 

ELS Add-on 属于延长服务的订阅，要在支持结束前购买。

EUS 是基于指定RHEL小版本的订阅（在完全和维护期间的订阅）。在8上只在双算版本上有。

[https://access.redhat.com/zh_CN/support/policy/update_policies ](https://access.redhat.com/zh_CN/support/policy/update_policies%C2%A0)

sosreport命令的使用

<https://www.thegeekdiary.com/how-to-install-and-configure-sosreport-under-centos-rhel/>

查看是SO是否有订阅信息：

\# cat sos_commands/subscription_manager/subscription-manager_identity

This system is not yet registered. Try \'subscription-manager register \--help\' for more information.  \--》 说明没有订阅信息

![[记录_信息_重要备份_003_sosreport 记录_002.png]]

 

：：：磁盘信息类：：：

RHEL6 : cat sos_commands/filesys/lsblk  

RHEL7: sos_commands/block/lsblk

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\# 可查看dev下的设备信息

sos_commands/block/ls\_-lanR\_.dev

\# smartctl 检查

sos_commands/ata/smartctl\_-a\_.dev.\*

grep -ir \'Elements in grown defect list\' sos_commands/ata/

：：：系统设置方面：：：

》sos_commands/kernel/sysctl\_-a

sos_commands/kernel/modinfo\* \--\> 查看模块信息

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

sos_commands/startup/chkconfig\_\--list  # 6启动了那些服务

sos_commands/systemd/systemctl_list-unit-files  #7与8启动了那些服务

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

sos_commands/login/last_reboot   # 重启记录

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

》看门狗设置：

\# cat /proc/sys/kernel/watchdog

1

\# cat /proc/sys/kernel/nmi_watchdog

1

关闭watchdog： echo 0 \> XXXX

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

》第三方你模块没有GPL授权的原因，并且发生崩溃或BUG（）。 \| <http://cn.voidcc.com/question/p-zumwxtdv-qe.html>

 

\# cat /proc/sys/kernel/tainted 128# dmesg \| grep -i taint \[ 8306.955523[\] Pid: ]4511, comm: chrome Tainted: G[  D  ]3.9.10-100.fc17.i686.PAE #1 Dell Inc. \[ 8307.366310[\] Pid: ]4571, comm: chrome Tainted: G[  D  ]3.9.10-100.fc17.i686.PAE #1 Dell Inc. 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

NUMA的开启与关闭

\# cat proc/sys/kernel/numa_balancing

0   

#0是被关闭了[  #1是开启（默认是开启的）。 可以使用"echo 1 \> ]proc/sys/kernel/numa_balancing" 修改。

 

：：：网络方面：：：

\# cat sos_commands/networking/ethtool\_-S_eth\* \|grep drop  \--》查看网卡口是否有丢包记录

\# cat sos_commands/networking/ethtool\_-S\* \|egrep \'drop\|fdir_miss\|error\'

 

\[root@localhost sunfong\]# cat proc/net/softnet_stat

0000410f 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000

00001538 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000

00001b92 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000

00000c3e 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000

\...\...

每一行是每个核的情况，第一列是收到的报文，第二列是drop的报文，由于netdev_max_backlog队列溢出而被丢弃的包总数。第三列是net_rx_action中收包，

<https://www.bbsmax.com/A/l1dyPGVg5e/> 待考证

 

：：：多路径：：：

\> sos_commands/devicemapper/multipath\_-v4\_-ll   \--\> 多路径

\> sos_commands/multipath/multipath\_-l

\> cat etc/multipath/wwids

：：：GPU：：：

\# grep \'NVIDIA Corporation\' lspci 

31:00.0 3D controller \[0302\]: NVIDIA Corporation GA102GL \[RTX A40\] \[10de:2235\] (rev a1)

        Subsystem: NVIDIA Corporation Device \[10de:145a\]

e3:00.0 3D controller \[0302\]: NVIDIA Corporation GA102GL \[RTX A40\] \[10de:2235\] (rev a1)

        Subsystem: NVIDIA Corporation Device \[10de:145a\]

\-\-\-\--lsmod\-\-\--

nvidia              34033664  185 nvidia_uvm,nvidia_modeset

![[记录_信息_重要备份_003_sosreport 记录_003.png]]

 

Ubuntu

![[记录_信息_重要备份_003_sosreport 记录_004.png]]

 

：：：详细记录：：：

\-\-\-\-- dev 设备对应关系 \-\-\-\-\--

sda      8:0    0 278.9G  0 disk 

├─sda1   8:1    0   512M  0 part /boot

├─sda2   8:2    0  97.7G  0 part /usr

├─sda3   8:3    0    12G  0 part \[SWAP\]

├─sda4   8:4    0     1K  0 part 

└─sda5   8:5    0 168.7G  0 part /

sdd      8:48   0   3.3T  0 disk 

└─sdd1   8:49   0   3.3T  0 part /mpeg/mpeg3

sdb      8:16   0   3.8T  0 disk 

└─sdb1   8:17   0   3.8T  0 part /mpeg/mpeg1

sdc      8:32   0   3.8T  0 disk 

└─sdc1   8:33   0   3.8T  0 part /mpeg/mpeg2

00:00:01          DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util

00:10:01      dev8-16    516.66 262494.64   6582.34    520.80     23.54     45.57      1.79     92.37

00:10:01      dev8-32    707.29 350682.12  10362.89    510.46     36.20     51.19      1.36     96.06

00:10:01      dev8-48    157.91  72746.28   6836.18    503.96      3.49     22.10      2.80     44.23

00:10:01       dev8-0      4.94      7.82    705.70    144.50      0.00      0.80      0.27      0.13

00:20:01      dev8-16    468.55 237447.98   6561.01    520.77 30554119437881.44     42.27      1.95     91.59

00:20:01      dev8-32    630.55 318478.76  10363.20    521.51     31.13     49.38      1.55     97.50

00:20:01      dev8-48    150.38  68931.79   6856.20    503.97      3.19     21.23      2.85     42.88

命令查看HBA卡FW命令：

systool -c scsi_host -d \|grep fw_version

查看 "optrom_fw_version" 就是FW号

此命令由 sysfsutils 包提供。

FYI ：

<https://access.redhat.com/solutions/1264913>

 

\-\-\-\-\-\-- 网络流量 \-\-\-\-\-\--

 

![[记录_信息_重要备份_003_sosreport 记录_005.png]]

 

![[记录_信息_重要备份_003_sosreport 记录_006.png]]

 

![[记录_信息_重要备份_003_sosreport 记录_007.png]]

 

\-\-\-\-\-\-\-- RHEL7 查看网络是否有错误包 \-\-\-\-\-\-\-\-\--

\# cat sos_commands/networking/ip\_-s_link 

8: p5p1: \<BROADCAST,MULTICAST,UP,LOWER_UP\> mtu 9000 qdisc mq state UP mode DEFAULT qlen 1000

    link/ether 00:0e:1e:7c:6d:a0 brd ff:ff:ff:ff:ff:ff

    RX: bytes  packets  errors  dropped overrun mcast   

    495        5        0       5       0       5       

    TX: bytes  packets  errors  dropped carrier collsns 

    854        11       0       0       0       0       

9: p5p2: \<BROADCAST,MULTICAST,UP,LOWER_UP\> mtu 9000 qdisc mq state UP mode DEFAULT qlen 1000

    link/ether 00:0e:1e:7c:6d:a2 brd ff:ff:ff:ff:ff:ff

    RX: bytes  packets  errors  dropped overrun mcast   

    495        5        0       5       0       5       

    TX: bytes  packets  errors  dropped carrier collsns 

    936        12       0       0       0       0     

检查磁盘对应的UUID：

cat sos_commands/devicemapper/ls\_-laR\_.dev

机器上看到的

\[root@localhost zsdbsrv5\]# ls -laR /dev/disk/by-uuid/

/dev/disk/by-uuid/:

总计 0 

drwxr-xr-x 2 root root 100 07-21 09:35 .

drwxr-xr-x 5 root root 100 07-21 09:35 ..

lrwxrwxrwx 1 root root  10 07-21 09:35 1ae66815-772d-4717-a957-6a5a8199e6c5 -\> ../../sda3

lrwxrwxrwx 1 root root  10 07-21 09:35 23dc04c7-f03e-4715-b1dd-ffac4ac9af4f -\> ../../sda1

lrwxrwxrwx 1 root root  10 07-21 09:35 29eed1a4-a881-4597-9ecc-1f5925cb6b01 -\> ../../sda2

\[root@localhost zsdbsrv5\]# blkid -c .dev

/dev/sda1: LABEL=\"/\" UUID=\"23dc04c7-f03e-4715-b1dd-ffac4ac9af4f\" TYPE=\"ext3\" 

/dev/sda2: TYPE=\"swap\" UUID=\"29eed1a4-a881-4597-9ecc-1f5925cb6b01\" 

/dev/sda3: UUID=\"1ae66815-772d-4717-a957-6a5a8199e6c5\" TYPE=\"ext3\" 

or

\[root@localhost \~\]# blkid /dev/sda1

/dev/sda1: UUID=\"f759fa14-188f-4928-94d9-4afc6b6c81ea\" TYPE=\"ext4\" 

从新生成UUID并修改sda1的UUID：

\# uuidgen \| xargs tune2fs /dev/sda1 -U

也可以把 fstab 里找到的原 uuid 写回分区:

\# tune2fs -U c1b9d5a2-f162-11cf-9ece-0020afc76f16 /dev/sda5

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

HBA卡   \--\> 搜索关键字"sas"在dmesg可找到

    \|

mpt2sas \--\> H200

    \|

megaraid_sas \--\> LSI(H710)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

lspci \|grep -i intel  \--\> 查看是否有Intel网卡

lspci \|grep -i broadcom  \--\> 查看是否有博科网卡

 

 

\# cat sos_commands/system/crontab\_-l

磁盘超时时间：

[\[root@localhost \~\]# cat /sys/block/sda/device/timeout] 

90   【单位是秒】

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

主板P/N以及PPID.

![[记录_信息_重要备份_003_sosreport 记录_008.png]]

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

看Redhat当前内存的频率：

dmidecode \|grep -A16 \"Memory Device\"

\-\-\--

1，内存条数：

\[root@localhost \~\]# dmidecode\|grep -P -A5 \"Memory\\s+Device\"\|grep Size\|grep -v Range

        Size: 1024 MB   

        Size: 1024 MB

        Size: 1024 MB

        Size: 1024 MB

        Size: No Module Installed

        Size: No Module Installed

 

2，内存最大容量：

\[root@localhost \~\]# dmidecode\|grep -P \'Maximum\\s+Capacity\'

        Maximum Capacity: 24 GB

 

3，内存频率：

\[root@localhost \~\]# dmidecode\|grep -A16 \"Memory Device\"\|grep Speed

        Speed: 667 MHz

        Speed: 667 MHz

        Speed: 667 MHz

        Speed: 667 MHz

        Speed: Unknown

        Speed: Unknown

\[root@localhost \~\]# 

pmap

可以根据进程查看进程相关信息占用的内存情况，(进程号可以通过ps查看)如下所示：

\$ pmap -d 5647

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

kernel 层关闭mcelog "mce=off" 添加到 /boot/grub/grub.conf

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

检查磁盘块坏道

badblocks -b 4096 -c 16 -s /dev/sda5 -o log.txt

意思就是以4k为一个block，每一个block检查16次，将结果输入到log.txt文件，如果硬盘正常的话，log.txt是没有任何内容的，如果硬盘很 大，可以加一个-s参数来显示进度。

可以尝试使用badblock命令进行检查：

\# umount /dev/sdb             \--\> 需要先卸载磁盘

\# badblocks -n -vv /dev/sdb   \--\>  执行badblocks 命令

截出结果

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

首先确认是哪种光纤卡: lspci \| grep -i fibre 光纤卡基本上就以下两种: Emulex: lsmod \|grep lpfc qlogic: lsmod \|grep qla

命令测试1：

\# cat lsmod \|grep lpfc

lpfc                  623577  16 

scsi_transport_fc      52241  1 lpfc

 

命令测试2：

\# cat lspci \|grep -i fibre 05:00.0 Fibre Channel: Emulex Corporation Saturn-X: LightPulse Fibre Channel Host Adapter (rev 03) 05:00.1 Fibre Channel: Emulex Corporation Saturn-X: LightPulse Fibre Channel Host Adapter (rev 03) Product Name: Dell LPe1205-M 8Gb 2-port PCIe Fibre Channel Adapter \[V1\] Vendor specific: Dell LPe1205-M 8Gb 2-port PCIe Fibre Channel Adapter Product Name: Dell LPe1205-M 8Gb 2-port PCIe Fibre Channel Adapter \[V1\] Vendor specific: Dell LPe1205-M 8Gb 2-port PCIe Fibre Channel Adapter +-09.0-\[05\]\--+-00.0 Emulex Corporation Saturn-X: LightPulse Fibre Channel Host Adapter \| \\-00.1 Emulex Corporation Saturn-X: LightPulse Fibre Channel Host Adapter

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

判断是否第三方模块

也就是说只要 \'modinfo \<module_name\> \| grep filename\'  出现的路径是 /lib/modules/2.6.18.X.X /extra/ 就是地三方的模 块。

例子：

\# cat lib/modules/2.6.32-279.el6.x86_64/modules.dep \|grep extra

extra/usm/oracleacfs.ko: extra/usm/oracleoks.ko

extra/usm/oracleadvm.ko: extra/usm/oracleoks.ko

extra/usm/oracleoks.ko:

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Mellanox Connect X3 QDR 40Gbps InfiniBand Mezz Card

\[root@localhost 1ZFDB3X_05\]# cat sos_commands/kernel/modinfo_nfs_nfs_acl_ip6table_filter_ip6_tables_ebtable_nat_ebtab \|grep mlx

filename:       /lib/modules/2.6.18-274.el5/extra/mlnx-ofa_kernel/drivers/infiniband/hw/mlx4/mlx4_ib.ko

depends:        mlx4_core,mlx4_core,ib_core,ib_mad,ib_core

filename:       /lib/modules/2.6.18-274.el5/extra/mlnx-ofa_kernel/drivers/net/mlx4/mlx4_en.ko

depends:        mlx4_core,mlx4_core

filename:       /lib/modules/2.6.18-274.el5/extra/mlnx-ofa_kernel/drivers/net/mlx4/mlx4_core.ko

驱动下载地址：

[http://www.mellanox.com/page/products_dyn?product_family=26&menu_section=3](http://www.mellanox.com/page/products_dyn?product_family=26&menu_section=34)

下载分为ISO与tgz，听说ISO比较保险。

![[记录_信息_重要备份_003_sosreport 记录_009.png]]

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

检查是否安装OMSA

\# cat installed-rpms \|grep -i openmanage

\# cat installed-rpms \|grep -i omsa

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

光纤卡是没有 em1 几的，因为不是以太网卡，他是通过www挂过来的。

要找到卡的目前速度可以在dmidecode 里好到卡在那个PCI槽位，然后"Bus Address:"关键字，然后找的dmesg log，就会找到。

例子：

kernel: qla2xxx \[0000:05:00.0\]-00fb:1: QLogic QLE2562 - PCI-Express Dual Channel 8Gb Fibre Channel HBA.

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

判断是否4K磁盘

关键字：logical/physical

./sos_commands/block/fdisk\_-l\_.dev.nvme1n1:Sector size (logical/physical): 512 bytes / 512 bytes

 

  ------------------------------ --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  logical/physical: 512B/512B    傳統 512B 磁區硬碟
  logical/physical: 512B/4096B   先進格式化 [512e](http://wanggen.myweb.hinet.net/ech1/ech1.html?MywebPageId=2015131447392785721#af512e) 硬碟
  logical/physica: 4096B/4096B   先進格式化 [4Kn](http://wanggen.myweb.hinet.net/ech1/ech1.html?MywebPageId=2015131447392785721#af4k) 硬碟
  ------------------------------ --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

特別注意 Linux Kernel 版本 v2.6.31 且 parted 版本 2.1 以上才能正確識別先進格式化硬碟,且硬碟不能透過橋接 (如 SATA 轉 USB 或其他轉換)才能正確識別。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

查看那个进程使用swap比较多

\# grep VmSwap /proc/\*/status

/proc/1077/status:VmSwap:              0 kB

/proc/1670/status:VmSwap:              0 kB

/proc/1674/status:VmSwap:              0 kB

/proc/1701/status:VmSwap:              0 kB

/proc/1702/status:VmSwap:              0 kB

/proc/1704/status:VmSwap:              0 kB

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\# dmidecode -s system-serial-number

抓机器序列号

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

查看USB设备：cat /proc/bus/usb/devices

查看键盘和鼠标:cat /proc/bus/input/devices

\# cat proc/bus/input/devices 

I: Bus=0019 Vendor=0000 Product=0001 Version=0000

N: Name=\"Power Button\"   \--\> 电源

P: Phys=LNXPWRBN/button/input0

S: Sysfs=/devices/LNXSYSTM:00/LNXPWRBN:00/input/input0

U: Uniq=

H: Handlers=kbd event0 

B: EV=3

B: KEY=10000000000000 0

I: Bus=0017 Vendor=0001 Product=0001 Version=0100

N: Name=\"Macintosh mouse button emulation\"   \--\>麦金塔电脑鼠标按钮模拟

P: Phys=

S: Sysfs=/devices/virtual/input/input1

U: Uniq=

H: Handlers=mouse0 event1 

B: EV=7

B: KEY=70000 0 0 0 0

B: REL=3

\...\...

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

sos_commands/hardware/dmidecode 

Memory Device

        Array Handle: 0x1000

        Error Information Handle: Not Provided

        Total Width: 72 bits

        Data Width: 64 bits

        Size: 16384 MB

        Form Factor: DIMM

        Set: 19

        Locator: G3    #槽位

        Bank Locator: Not Specified

        Type: \<OUT OF SPEC\>

        Type Detail: Synchronous Registered (Buffered)

        Speed: 2133 MHz

        Manufacturer: 00AD063200AD    #内存制造商

        Serial Number: 245DB7A9

        Asset Tag: 011529B1

        Part Number: HMA42GR7MFR4N-TF

        Rank: 2

        Configured Clock Speed: 1333 MHz

与Dset对应：

![[记录_信息_重要备份_003_sosreport 记录_010.png]]

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

lspci 所列出的设备说明与举例

\# grep -eir \'SSD Controller\' lspci

1）SSD 控制卡，用来连接SSD磁盘，有多少条说明有多少个SSD盘，另外看开头设备名，如下，有4开头和8开头，说明有两张这种卡，每个卡是4块SSD盘。

43:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

44:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

45:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

46:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

84:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

85:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

86:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

87:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 171X (rev 03)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

RHEL7.x

查看此进程是否开机运行

[\[root@localhost 113\]# cat sos_commands/systemd/systemctl_list-units\_\--all \|grep -i network]

netconsole.service                                                                                             loaded inactive dead      SYSV: Initializes network console logging

network.service                                                                                                loaded active   exited    LSB: Bring up/down networking

NetworkManager-wait-online.service                                                                             loaded inactive dead      Network Manager Wait Online

NetworkManager.service                                                                                         loaded inactive dead      Network Manager     \# 说明没有开机运行

ntpd.service                                                                                                   loaded inactive dead      Network Time Service

rhel-import-state.service                                                                                      loaded active   exited    Import network configuration from initramfs

network-online.target                                                                                          loaded inactive dead      Network is Online

network.target                                                                                                 loaded active   active    Network   #说明有开机运行

nss-lookup.target                                                                                              loaded inactive dead      Host and Network Name Lookups

或者

\[root@localhost 8TV8WK2\]# cat sos_commands/systemd/systemctl_list-unit-files \|grep \'NetworkManager\'

NetworkManager-dispatcher.service            disabled      

NetworkManager-wait-online.service            disabled      

NetworkManager.service                        disabled  

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

网络正常的情况下，网卡有插网线，网卡灯不亮，网卡也up不起来。

![[记录_信息_重要备份_003_sosreport 记录_011.png]]

此问题的原因是NetworkManager没有开启，而网卡配置有配置 NM_CONTROLLED=\"yes\" 的原因。

改成  NM_CONTROLLED=\"no\" 就可以了。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

socket 问题：

\# netstat -ae \|grep [mysql](http://www.jbxue.com/db/mysql/)

tcp        0      0 aaaa:53045               192.168.12.13:mysql           TIME_WAIT   root       0

tcp        0      0 aaaa:53044               192.168.12.13:mysql           TIME_WAIT   root       0

tcp        0      0 aaaa:53051               192.168.12.13:mysql           TIME_WAIT   root       0

vi /etc/sysctl.conf

编辑文件，加入以下内容：

 

代码如下:

net.ipv4.tcp_syncookies = 1

net.ipv4.tcp_tw_reuse = 1

net.ipv4.tcp_tw_recycle = 1

net.ipv4.tcp_fin_timeout = 30

然后执行 /sbin/sysctl -p 让参数生效。

net.ipv4.tcp_syncookies = 1 表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；

net.ipv4.tcp_tw_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；

net.ipv4.tcp_tw_recycle = 1 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。

net.ipv4.tcp_fin_timeout 修改系統默认的 TIMEOUT 时间，单位秒。

 

根据TCP协议定义的3次握手断开连接规定，发起socket主动关闭的一方 socket将进入TIME_WAIT状态，TIME_WAIT状态将持续2个MSL(Max Segment Lifetime)

 

WWN信息查找，尝试下面：

sos_commands/devices/udevadm_info\_\--export-db

S: disk/by-id/wwn-0x6000d310047a5e000000000000000005

S: disk/by-label/APARCH01

S: disk/by-path/pci-0000:5e:00.0-fc-0x5000d310047a5e34-lun-1

E: DEVLINKS=/dev/disk/by-id/scsi-36000d310047a5e000000000000000005 /dev/disk/by-id/wwn-0x6000d310047a5e000000000000000005 /dev/disk/by-label/APARCH01 /dev/disk/by-path/pci-0000:5e:00.0-fc-0x5000d310047a5e34-lun-1

E: DEVNAME=/dev/sdb

E: DEVPATH=/devices/pci0000:5d/0000:5d:00.0/0000:5e:00.0/host15/rport-15:0-0/target15:0:0/15:0:0:1/block/sdb

\-\-\-\-\-\-\-\-\-\--

服务器的WWN一般都是20或者10开头的，50一般都是存储

服务器的如：

![[记录_信息_重要备份_003_sosreport 记录_012.png]]

 

 

：：：OMSA：：：

如果系统有安装OMSA软件也会有相应的log被系统记录：【案例SR#1019278959】

sos_commands/omsa/opt.dell.srvadmin.bin.omreport_system_alertlog

Severity : Critical

ID : 1404

Date and Time : Mon Mar 9 03:52:47 2020

Category : Instrumentation Service

Description : Memory device status is critical

Memory device location: B3

Possible memory module event cause:Single bit warning error rate exceeded,Single bit failure error rate exceeded,Multi bit error encountered

Severity      : Non-Critical

ID            : 1014

Date and Time : Mon Mar  9 03:52:09 2020

Category      : Instrumentation Service

Description   : System software event:

Description: A problem was detected during Power-On Self-Test (POST).

Date and time of action: Mon Mar  9 00:46:20 2020

 

Severity      : Critical

ID            : 1006

Date and Time : Mon Mar  9 03:52:09 2020

Category      : Instrumentation Service

Description   : Automatic System Recovery (ASR) action was performed

Action performed was: Reboot

Date and time of action: Wed Apr 14 23:14:36 2156

 

 

已使用 OneNote 创建。
