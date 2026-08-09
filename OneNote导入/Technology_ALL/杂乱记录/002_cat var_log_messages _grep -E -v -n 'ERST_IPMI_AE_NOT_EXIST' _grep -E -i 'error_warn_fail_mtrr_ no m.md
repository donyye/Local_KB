2015年12月18日

14:41

cat var/log/messages \|grep -E -v -n \'ERST\|IPMI\|AE_NOT_EXIST\' \|grep -E -i \'error\|warn\|fail\|mtrr: no more MTRRs available\|pcscd: winscard.c\|Currently unreadable\|stuck for\|evicted\|unsupported\| firmware bug\|bmc-watchdog\|fiid_obj_get\|dmar: DRHD\|Core power limit notification\|Medium Error\|SCSI layer issued\|failed to load because an unsupported\|Call Trace\|ADDRCONF\|EDAC MC0\|memory read on FATAL area OVERFLOW\|Machine Check Exception\|nobody cared\|detected conn error\|unknown partition table\|has an internal error\| diagnostic event occurred\|MDC/MDIO access timeout\|Checksum for group\|machine check error detected\|panic\|Giving out device to\|Ramping down queue depth to\|failed while handling\|Unqualified SFP\|not responding\|HeartbeatFramework\|avahi-daemon\'

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

cat var/log/messages \| g[rep -E -i \--color=always \'error\|reset\|tain\|cavailable\|pcscd: winscard.c\|Currently unreadable\|stuck for\|evicted\|unsupported\| firmware bug\|bmc-watchdog\|fiid_obj_get\|dmar: DRHD\|Core power limit notification\|Medium Error\|SCSI layer issued\|failed to load because an unsupported\|Call Trace\|ADDRCONF\|EDAC MC0\|memory read on FATAL area OVERFLOW\|Machine Check Exception\|nobody cared\|detected conn error\|unknown partition table\|has an internal error\| diagnostic event occurred\|MDC/MDIO access timeout\|Checksum for group\|machine check error detected\|panic\|Giving out device to\|Ramping down queue depth to\|failed while handling\|Unqualified SFP\|not responding\|HeartbeatFramework\|avahi-daemon\|allocation failure\|callbacks suppressed\|Power fault on Port 0 has\|pending while HBA reset\|File too large\|temperature/speed normal\|0x00070002\|NMI received for unknown reason\|memory error\|Unknown Error Bit\|DIMM\_\[A-Z\]\[0-9\]\*\|\[0-9\]\_Channel#\[0-9\]\_DIMM#\|\*\*\*\* Failed mbx\|this hardware has not undergone\|BUG: soft lockup\|A bus fatal error was detected\|Target requests logout within\|Recorded using libct_ffdc\|Watchdog detected hard\|error: return code\|sending diag reset\|possible memory allocation deadlock in kmem_alloc\|A processor failed\|bad extent address iblock\|firmware bug on this device\|No space left on device\|SFP+ or QSFP module type was detected\|SFP+ module type was detected\|RSA host key\|Error loading firmware\|Function start failed\|reservation conflict\|SIGSEGV\|do_reset failed for cmd\|failed to assign\|service-time\|Illegal Request\|Sense Key\|FAILED Result:\|Bad rss-counter\|Failed to allocate mem\|Function start failed\|INIT adapter done\|read-only\|Detected change\|link status up for interface\|bad entry in directory\|error \[0-9\] returned\|hard reset was asserted\|\_DIMM\|error -5 returned\|dev_watchdog\|-fs error\|possible SYN flooding\|Out of memory: Kill\|tainting kernel\|device state change\|DID_SOFT_ERROR\|failed command\|hard resetting link\|FW in FAULT\|Reset adapter\|error count since last fsck\|Logical block reference tag check failed\|Adapter failed to init\|firmware initialization failed\|dracut-initqueue timeout\|I/O error\|sense key\|SCSI error: return code\|WRITE SAME failed\|DID_ERROR\|Abort command issued\|PCIe error\|Unknown VPD Code\|The canary thread is apparently starving\|Saved core dump of\|Core power limit normal\|Kill process\|memory corruption\|0x31110e05\|High CPU load detected\|Space allocation failed write protect\|critical space allocation error\|Data Protect\|controller is down\|I/O error\|Async-tmf error\|TM IOCB failed\|DEVICE RESET FAILED\|tag check failed\|over heated\|tx_timeout\|transmit queue \[0-9\]\* timed out\|segfault at\|EINVAL\|Hung TX\|blocked FC remote port time out\|SMART Failure\|SAME failed\|Write same\|memory allocation deadlock\|OOM killer\|Device not ready\|detected capacity change from\|failure status\|Logical block reference tag check failed\|Controller encountered an error and was reset\|some SCSI devices might Offline uncorrectable sectors\|blk_update_request\|Buffer I/O error\|device removal in progress\|Reset adapter\|Detected Tx Unit Hang\|err_code:\|corrected\|Failed to rename network\|return code = 0x00010000\|return code =\|Power-on or device reset occurred\|reset\|MegaCli64\|remote port time out\|LOOP RESET ISSUED\|timing out command\|Abort command issued\|device reset failed\|reset succeeded\|Space allocation failed write protect\|duplicate address\|Controller in crit error\|SMART Failure:\|not in synchronization\']

 

 

\>\>\>

grep -E -n -A2 -B10 \'mock\|builder\|stopped.\|signal 15\' var/log/messages

\>\>\>

find . -depth -iname \"xxxx\"

 

\>\>\>存储与网络\<\<\<

grep -E -n -A2 -B5 \'compellent\|link is offline\|link is down\|checker reports path is down\|remaining active paths\|Link Down Event\|SCSI error: return code\|cannot make a connection to\' var/log/messages

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

checker reports path is down[  \#]多路径路径检查

remaining active paths[           \#]查看还有多少存活路径

blocked FC remote port time out \#不能连接到远程 FC端口，所以服务器不能访问这些端口后的设备。

iscsid:[  cannot make a connection to 192.x.x.x \#]网络断了iscsi无法连接到target。

\>\>\>

grep -ir \'Product Name\' .  \--\> look NIC product

grep -ir \'Product Name\' sos_commands/hardware/lspci

\#网卡名字重复出现多少说明有多少个网口。RHEL7 是 lspci -nvv

\>\>\>

sos_commands/pci/lspci\_-nvv[  \#]可以看到网卡信息

\>\>\>

cat sos_commands/hardware/lshal \|grep -E -A 4 \'bnx2\|bnx2fc\|bnx2i\|bnx2x\|cnic\|ixgbe\|tg3\|lpfc\|bfa\|bna\|cnic\' \|sort \|uniq -c

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

cat var/log/dmesg \|grep -E \'bnx2\|bnx2fc\|bnx2i\|bnx2x\|cnic\|ixgbe\|tg3\|lpfc\|bfa\|bna\|cnic\|mlx4_ib\'

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

bnx2.ko：以太网

bnx2fc.ko：光纤

bnx2i.ko：iscsi

bnx2x.ko:10G以太网卡[  \[QLogic \| Broadcom\]]

bfa.ko : brocade 825 FC 卡(Qlogic FC HBA fcpim ipfc)

bna.ko ： QLogic BR-series 10G PCIe Ethernet driver

cnic.ko： 较少见

ixgbe : Intel 10G FC (x520) PCI 扩展网络设置，以太网网卡

lpfc：Emulex Corporation FC 卡

mlx4_ib ：Mellanox ConnectX HCA InfiniBand driver

\# 目前只有Qlogic和emulex有光纤网卡，其它都是光纤以太网卡

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

bnx2 : BCM5709

tg3 : Broadcom 5720

Broadcom 57840 NetXtremeII 10 GigE[  \#]有FCOE功能

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\# cat /sys/class/fc_host/host\*/port_name[   \#]查看FC WWN

\>\>\>查看是否有碎片包\<\<\<

cat proc/net/snmp \|grep \'\^Ip:\' \| cut -f17 -d \' \'

\>\>\>

C1E、C States、MONITOR/MWAIT

grep -ir \'cpu MHz\' proc/cpuinfo[  \#]可以通过此命令检查各个CPU的频率是否一样。

\>\>\>

grep \'Serial Number\' dmidecode

\>\>\>

lspci \|grep -i raid[     \#]查看RAID卡型号

cat sos_commands/hardware/lspci \|grep -E -i \'megaraid_sas\|mpt3sas\|sas\'

\>\>\>

alias grep=\'grep \--color=auto\'

alias grep=\'fgrep \--color=auto\'

alias grep=\'egrep \--color=auto\'

\>\>\>

 dd if=/dev/zero bs=1024K  of=/dev/sda oflag=direct

使用dd命令最sda完整写一遍，主要检查磁盘是否有问题。

\>\>\>

cat lib/modules/2.6.32-279.el6.x86_64/modules.dep \|grep extra

\>\>\>

逻辑CPU# cat proc/cpuinfo \|grep \"processor\" \|wc -l

物理CPU# cat proc/cpuinfo \|grep \"physical id\" \|sort \|uniq \|wc -l

多少核心\# cat proc/cpuinfo \|grep \"cpu cores\" \|uniq

cat proc/cpuinfo \|grep \"physical id\" \|sort \|uniq \|wc -l;cat proc/cpuinfo \|grep \"cpu cores\" \|uniq;cat proc/cpuinfo \|grep \"processor\" \|wc -l

\>\>\>

\# lvdisplay\|awk  \'/LV Name/ /Block device/\'

dm-0 lv_root

dm-1 lv_swap

\>\>\>

blkid -c .dev[  \#]可查看到UUID与fs类型

cat sos_commands/filesys/blkid

\>\>\>

grep -E -i -A2 -B2[  \'Manufacturer\|Product\' var/log/messages  \# ]关键字，可查看USB设备牌子。

\>\>\>

cat lib/modules/2.6.32-279.el6.x86_64/modules.dep \|grep extra

\# 查看是否有第三方模块

\>\>\>

SLI 卡在Linux里的模块名字

megaraid_sas

mpt3sas

\>\>\>

GPU 查看

lspci \| grep -i nvidia

06:00.0 3D controller: NVIDIA Corporation GK210GL \[Tesla K80\] (rev a1)

07:00.0 3D controller: NVIDIA Corporation GK210GL \[Tesla K80\] (rev a1)

\-\-\-\-\-\-\--

\[root@RHEL70 48MRJ82\]# cat lsmod \|grep -i \'nvidia\'

nvidia               8604684  0

drm                   311588  5 ttm,drm_kms_helper,mgag200,nvidia

i2c_core               40325  5 drm,drm_kms_helper,mgag200,i2c_algo_bit,nvidia

 

 

\>\>\> SUSE\<\<\<

cat messages.txt \|grep -E -v -n \'ERST\|IPMI\|AE_NOT_EXIST\' \|grep -E -i \'error\|warn\|fail\|Cannot find  firmware file\|Members Left\|Abnormal termination\|NMI backtrace for cpu\|Ramping down queue depth to\|Abort command issued\|No route to host\'

\>\>\>

grep -E -n -A10 -B10 \'syslog-ng starting up\|shutting down\|starting up\' messages

\>\>\>

hana 重启检查：

grep -E -n -A10 -B10 \'syslog-ng starting up\|syslog-ng shutting down\' messages-20150602.txt

\>\>\>

lifecycle[  driver package #F10]引导安装需要的驱动包

 

 

\>\>\> ESXi \<\<\<

\# grep -E -ir \'vlan ID  used ports  uplinks\|does not support multiple paths per device\|Skipping the path\' .

\>\>\>

less var/run/log/vmkernel\* \|grep \'PowerButton\'[  \#]查看是否有按电源按钮情况

\-\-\-\-\--

ESXi 5.1[  ]："VMB"[  \#]重启关键字

\>\>\>

\# 检查iscsi是否有断开

grep -E -ir -A3 -B3 \'0/1 0x0 0x0 0x0 \| 2/0 0x2 0x4 0x3 \| 2/0 0x5 0x4 0x3 \| 2/0 0x2 0x4 0xa \| 0/7 0x0 0x0 0x0 \| H:0x0 D:0x2 \| 2/0 0x5 0x94 0x1 \| 2/0 0x5 0x25 0x0 \| 2/0 0x2 0x4 0x1\' /var/log/vmkernel.log

\>\>\>

PSOD[  (Purple Screen Of Death) ]紫萍死机

\>\>\>

Dell.com/ossupport

DCSS：服务：ProSupport Plus and Mission Critical: (7x24) 4-hour Onsite Service, Year 1-3

 

\>\>\>

\## TTY check \##

grep -E -i \'fail\|error\|puncture\|rebuild\|unexpected\|Failed-Unresponsive\|Impending failure\|is not a certified drive\|SAS DISCOVERY\|medium error\|0             0 0 0 \[\^D,L\]\'

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

PERC

iDRAC

delta

Life Cycle

minimal

canonical \[kə\'nɒnɪk(ə)l\]

 

 

 

====================================================================================

 

REDHAT 5.x

CENTOS 5/RHEL 5 ---------------------------------2.6.18-8

CENTOS 5.1/RHEL 5 Update 1------------------2.6.18-53

CENTOS 5.2/RHEL 5 Update 2------------------2.6.18-92

CENTOS 5.3/RHEL 5 Update 3------------------2.6.18-128

CENTOS 5.4/RHEL 5 Update 4------------------2.6.18-164

CENTOS 5.5/RHEL 5 Update 5------------------2.6.18-194

CENTOS 5.6/RHEL 5 Update 6------------------2.6.18-238

CENTOS 5.7/RHEL 5 Update 7------------------2.6.18-274

CENTOS 5.8/RHEL 5 Update 8------------------2.6.18-308

CENTOS 5.9/RHEL 5 Update 9------------------2.6.18-348

CENTOS 5.10/RHEL 5 Update 10---------------2.6.18-371

CENTOS 5.11/RHEL 5 Update 11---------------2.6.18-398

 

REDHAT 6.x

CENTOS 6.0/RHEL 6 Update 0------------------2.6.32-71

CENTOS 6.1/RHEL 6 Update 1------------------2.6.32-131

CENTOS 6.2/RHEL 6 Update 2------------------2.6.32-220

CENTOS 6.3/RHEL 6 Update 3------------------2.6.32-279

CENTOS 6.4/RHEL 6 Update 4------------------2.6.32-358

CENTOS 6.5/RHEL 6 Update 5------------------2.6.32-431

CENTOS 6.6/RHEL 6 Update 6------------------2.6.32-504

CENTOS 6.7/RHEL 6 Update 7------------------2.6.32-573

 

REDHAT 7.x

CENTOS 7.0/RHEL 7 Update 0--------- 3.10.0-123.e17

CENTOS 7.1/RHEL 7 Update 0--------- 3.10.0-229.e17

 

 

REDHAT 4.x

RHEL 3 Update 8------------------------------------2.4.21-47

RHEL 4 ------------------------------------------------2.6.9-5

RHEL 4 Update 1------------------------------------2.6.9-11

RHEL 4 Update 2------------------------------------2.6.9-22

RHEL 4 Update 3------------------------------------2.6.9-34

RHEL 4 Update 4------------------------------------2.6.9-42

RHEL 4 Update 5------------------------------------2.6.9-55

RHEL 4 Update 6------------------------------------2.6.9-67

RHEL 4 Update 7------------------------------------2.6.9-78

RHEL 4 Update 8------------------------------------2.6.9-89

RHEL 4 Update 9------------------------------------2.6.9-100

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Redhat 官网Kernel版本对照表：

<https://access.redhat.com/articles/3078>

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Suse kernel版本对照表：

<https://www.novell.com/support/kb/doc.php?id=3594951>

 

 

已使用 OneNote 创建。
