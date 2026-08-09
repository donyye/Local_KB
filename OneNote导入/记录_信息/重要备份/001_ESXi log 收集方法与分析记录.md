ESXi log 收集方法与分析记录

2023年2月9日

14:03

收集ESX Server日志 

\[user@esx2host\]\$ cd /tmp

\[user@esx2host\]\$ /usr/bin/vm-support

ESXI 历史命令存放地方 ： /var/run/log/shell.log 

查看vm-support日志是什么时候收集的，检查 README文件的"Captured on"时间，或是action.log上的时间。

License 查找：

commands/vmware-vimdump\_-o\-\-\--U-dcui.txt.FRAG-00004

搜索 "licenseKey = "字样。

commands \--\> 记录一下命令结果

       \|

/commands/esxcfg-vmknic\_-l.txt  \--\> 查看ESXI的管理网络IP地址

       \|

/esxcfg-vswitch\_-l.txt   \--\> 查看每个portgroup的情况

       \|

/smbiosDump.txt  \--\> 记录一下BIOS版本等信息

       \| 

/uname\_-a.txt   \--\> VMkernel版本

       \|

/vmware\_-vl.txt  \--\> ESXi 版本

       \|

/localcli_software-vib-get.txt   \--\> 可以看到PERC卡驱动网卡驱动等版本

  \|

/localcli_software-profile-get.txt  \--\>ESXI安装时间 （Creation Time：xxxx）命令localcli software profile get

  \|

/nicinfo.sh.txt     \--\> 可以看到NIC的驱动版本信息、FW版本信息、NIC模块名字信息 vmkload_mod -s lsi_mr3 \|grep Version

       \|

/localcli_storage-core-adapter-list.txt   \--\> HBA 卡信息 localcli storage core adapter list

       \|

/esxcfg-module\_-q.txt   \--\> 查看ESXI系统当前已被加载的模块，像lsmod

       \|

/localcli_storage-core-device-list.txt  \--\> ESXI上每个lun的大小与状态，包括挂载的轮

\# cat commands/localcli_storage-core-device-list.txt \|grep -v Queue \|grep -B3 \'Size: \'

        \|

 /localcli_storage-nmp-device-list.txt  \--\> 查看多路径模式，关键字（Path Selection Policy:）

         \|

/esxcfg-mpath\_-b.txt           \--\> 查看多路径信息

   \|

/smbiosDump.txt    \--\> 查看什么机器，ST，CPU型号

         \|

/smartinfo.txt    \--\> 检查磁盘是否有错误。如果有会显示有 Write Error Count与Read Error Count 数值。数值超10000会有问题。

          \|

/esxcfg-info\_-a.txt.FRAG-00015   \--\> 查看系统安装在那个磁盘下"Boot Filesystem UUID" 。如果在""有发现usb.vmhba32，说明系统安装在SD卡上的。USB控制器一般是vmhba32。 vmhba2 有可能是BOSS卡。

grep -ir \'Boot Filesystem UUID\' commands/esxcfg-info\_-a.txt.\*

 

 var

       \|

       /run/log/ \*   \--\> 轮循的旧日志

       \|

      /log/\*  \--\> 当时的新日志

       \|

      /var/run/log/vmksummary.log   \--\> 有记录ESXI重启日志

        2017-09-14T17:00:50Z bootstop: Host is powering off    \# 正常关机

        2017-09-15T09:48:53Z mark: storage-path-claim-completed

        2017-09-15T09:49:10Z bootstop: Host has booted        \# 开机时的信息

        \-\-\-\-\--

       2021-04-01T03:50:53Z bootstop: Host is rebooting    \#正常reboot信息

        \-\-\--综合\-\--

egrep -ir \'Host is powering off\|Host has booted\|Host is rebooting\' 146/var/run/log/vmksummary.\*

如果是异常重启，那一定会在/var/有个core目录有日志。如果没有那说明没异常重启过，它有可能不是在系统上执行的关机，所以没记录。

使用命令搜索可能异常重启的问题，对照KB 

egrep -ir \'haTask-ha-host-vim.HostSystem.reboot\|DCUI: poweroff\|DCUI: reboot\|In PowerButton Helper\|Starting system\|loaded VMkernel\|Halting system\' ../var/run/log/

<https://kb.vmware.com/s/article/1019238?lang=zh_cn>

      \|

grep -ir \'Host is shutting down\' var/run/log/hostd\*

[2022-04-26T02:48:45.930Z info hostd\[2100820\] \[Originator@6876 sub=Vimsvc.ha-eventmgr\] Event 2393 :] Host is shutting down.

\#有人通过直接登录ESXi host client然后执行了图形界面下的ESXi主机关机

       \|

       /var/log/vmkernel.log   \--\> 核心 VMkernel 日志，包括设备发现、存储和联网设备和驱动程序事件以及虚拟机启动。

        关键字，mbMagic开机第一个记录信息

           \|

         /var/run/log/vobd.log  \--\> VMware ESXi观察日志（vobd）跟踪对ESXi主机配置和结果的所有更改。

         [https://yq.aliyun.com/articles/530925](https://yq.aliyun.com/articles/530925)

        \|

        /var/run/log/shell.log  \--\> 使用的命令记录，相当于Linux history。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

vmkernel 

      \* In PowerButton Helper  \--\> 手动安电源关机

      \* ESXi 5.1  ："VMB" 重启关键字

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

VM log 记录：

/vmfs/volumes/storage名字/vm名字/\*.log   

例如：

/vmfs/volumes/56934e0f-727d9dfc-0fb0-90b11c4e5e40/vm-lvmhec-nfs1/vmware.log

\--\> 这个log在VM运行时是不能查看的。

》虚拟机的一些启动状态，比如是否异常重启等。

grep -E -ir \'Hostname=\|CPU reset: soft\|Soft Off. Good-bye\|CPU reset: hard\|local poweroff\|state change request\|CPU reset: soft\|VMAutomation_Reset. Trying hard reset\|WinBSOD\|sleep state\|entered a standby sleep state\' vmfs/volumes/\<disk-name\>/\<vm name\>/vmware\*

find vmfs/ -depth -iname \'\*.vmdk\' \--\> list 所有的VM

对应KB查： [https://kb.vmware.com/s/article/1019064?lang=zh_CN](https://kb.vmware.com/s/article/1019064?lang=zh_CN) (1019064)

》虚拟机进入了休眠状态

Your guest has entered a standby sleep state. Use the keyboard or mouse while grabbed to wake it

vm快照：

进入/vmfs/volumes/datastore目录名字/vm目录名字，看里面有个ls开头的文本文，里面是理出了vm所有的文件。

快照文件一般是"xxxx-000001.vmdk"这样命名。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

VMware.log \#   

egrep -ir \'WinBSOD\|VMAutomation_Reset. Trying hard reset\|TOOLS_STAUS_UNMANAGED\|Receiving PowerState\|Your guest has entered a standby sleep state\|assuming app is down\|Power on failure messages\|LookupAndOpen\' vmfs/volumes/

+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 关键字                                       | 说明                                                                                                                                                                                                                                                                                                                                                                                                                               |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| WinBSOD                                      |  Windws VM 蓝屏                                                                                                                                                                                                                                                                    |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| VMAutomation_Reset. Trying hard reset        | HA 服务重启VM                                                                                                                                                                                                                                                                                                                                    |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| TOOLS_STAUS_UNMANAGED                        | 有可能VMware tool有异常                                                                                                                                                                                                                                                                                                                 |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Receiving PowerState.InitiateRset request    | VM 收到power state命令                                                                                                                                                                                                                                                                                |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Your guest has entered a standby sleep state | VM进入休眠模式                                                                                                                                                                                                                                                                                                                                                                 |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| assuming app is down                         | 通常指VM内的GuestOS（Windows）出现响应问题，此类问题可能由于暂时的资源不足，windowsOS内部问题等因素导致。 |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Power on failure messages                    | vm开机错误。log后面会描述原因                                                                                                                                                                                                                                                                        |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| LookupAndOpen                                | 会导致vm无法开机                                                                                                                                                                                                                                                                                                                        |
|                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                              | <https://kb.vmware.com/s/article/83291?lang=en_US>                                                                                                                                                                                                                                                                                                                                                                                 |
|                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                              | <https://www.nico-maas.de/?p=1923>                                                                                                                                                                                                                                                                                                                                                                                                 |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

重启ESXI web client 服务，先重启一下hostd服务再重启一下 esxui服务

/etc/init.d/hostd restart

/etc/init.d/esxui restart   \--》个别有问题的时候这个服务启动会非常的慢，大概10分钟。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

VCenter log:

/var/log/vmware/\...

\|\_    \... vsphere-ui/logs/vsphere_client_virgo.log  \# \[ [vSphere UI log \] ]客户在VC web做的操作记录，如有错误也会记录。

 

\|\_    \...vmdird/vmdird-syslog.log

VSAN log:

都在 commands 目录下 localcli_vsan\*

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\-\-\-\-\-\-\-- 网络上 \-\-\-\-\-\-\-\-\-\--

搜集的主要信息为：

/tmp/aamSupport.PID HA的信息(VMware的HA使用的是Legato AAM的模块)

/proc/vmware 统计信息

/proc/vmware/config 配置信息

/etc

/etc/sysconfig 系统配置信息

/vmfs/volumes VMFS信息

/var/log 日志信息

如果有crash情况发生，会再这里有个 

另外的一些命令的输出全部都在解压目录下的tmp目录中，如 fdisk, uptime, ifconfig,chkconfig以及一些esxcfg命令。

常用的几种方式还包括：

-x: 列出正在运行的VM, 实际上是通过ls /proc/vmware/vm/\*/names命令得到信息

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

log:

vmkernel.log        #kernel log

vmwarning.log     #错误信息log

vmsummarry.log  #ESXI是否在工作，ESXI重启时间

vobd.log              #storage、network 等日志 

vmfs

vm folder name  #VM log 

 

 

 

 

Fault Description: #GP Exception 13 in world \...\...  【内存分段错误，world 3521072是进程ID号】

Stack：在VMware KB里搜索 stack 里的关键字，如果有说明是以知的issue。

如搜索" Timer_GetCycles \...\...\"，如果没有可以再下个关键字搜索。

其实这里的信息就是dump的部分信息。

Dump status: 关于VMware的dump，如果你看到：

 

说明dump有保存成功。dump可以设置成本地或是远端。

 

VMware 名解释

HCL：硬件兼容性表 （high cost of living）

案例1，LINT1 NMI,  这是硬件触发的问题，SEL日志应该会有些记录

 

 

案例2， no heartbeat, CPU处理进程没有响应。ESXI版本bug可能比较大。最终要分析dump。

 

 

其它网站关于紫屏的分析案例：

<http://www.learnfuture.com/Extend/ArticleContent?id=586bc9fc-577f-4afb-8330-c77b8ea66b47>

 

 ==============

commands/localcli_storage-san-sas-list.txt

SasDevice:

   Device Name: vmhba0

   SAS Address: 54:cd:98:f0:80:1c:73:00

   Physical ID: 0

   Minimum Link Rate: 0 Mbps

   Maximum Link Rate: 0 Mbps

   Negotiated Link Rate: 0 Mbps

   Model Description: 

   Hardware Version: 

   OptionROM Version: 

   Firmware Version: 25.5.5.0005

   Driver Name: lsi_mr3

   Driver Version: 7.708.07.00

 

commands/localcli\_\--plugin-dir-usrlibvmwareesxcliint-device-driver-list.txt

Device Driver Status KB Article

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

vmnic3 i40en normal  

vmnic2 i40en normal  

vmhba4 qlnativefc normal  

vmhba3 qlnativefc normal  

vmhba0 lsi_mr3 normal  

vmnic1 ntg3 normal  

vmnic0 ntg3 normal  

vmhba2 vmw_ahci normal  

vmhba32 vmkusb normal  

vmhba1 vmw_ahci normal 

 

 

commands/localcli_storage-san-fc-list.txt

FcDevice:

   Adapter: vmhba3

   Port ID: 010600

   Node Name: 20:00:f4:e9:d4:58:dd:b6

   Port Name: 21:00:f4:e9:d4:58:dd:b6

   Speed: 16 Gbps

   Port Type: NPort

   Port State: ONLINE

   Model Description: QLE2692 Dual Port 16Gb FC to PCIe Gen3 x8 Adapter

   Hardware Version: 

   OptionROM Version: 3.62

   Firmware Version: 8.08.204 (d0d5)

   Driver Name: qlnativefc

Error getting field DriverVersion

 

FcDevice:

   Adapter: vmhba4

   Port ID: 010600

   Node Name: 20:00:f4:e9:d4:58:dd:b7

   Port Name: 21:00:f4:e9:d4:58:dd:b7

   Speed: 16 Gbps

   Port Type: NPort

   Port State: ONLINE

   Model Description: QLE2692 Dual Port 16Gb FC to PCIe Gen3 x8 Adapter

   Hardware Version: 

   OptionROM Version: 3.62

   Firmware Version: 8.08.204 (d0d5)

   Driver Name: qlnativefc

Error getting field DriverVersion

 

=====================

 

VCSA里的 /storage/archive

这个目录是一个数据库的归档目录，这个目前不能手动删除，他会自己动删除，所以就算满了也不影响使用。因为数据库活动频繁就会产生很多这种归档数据

 

=================

commands/esxcfg-scsidevs\_-m.txt

esxcfg-mpath -l

 

查看本地磁盘对应。通过查看SAS ID来对比TSR的SAS ID确认是那个盘。

 

已使用 OneNote 创建。
