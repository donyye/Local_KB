storage案例-1

2022年4月27日

11:51

<https://www.thegeeksearch.com/centos-rhel-removing-and-adding-new-disk-not-showing-correct-size-emc-storage/>

 

 

CentOS/RHEL: 删除和添加新磁盘，没有显示正确的大小(EMC 存储)

 

Issue

 

删除了两个20gb 的 lun，并向服务器提供了240gb 的新 lun。使用 EMC 存储。下面我们可以看到不正确的大小:

 

 

\# multipath -ll mpath1\
mpath1 (36000144000000010600aa302e54d98da) dm-19 EMC,Invista\
size=20G features=\'0\' hwhandler=\'0\' wp=rw\
\`-+- policy=\'queue-length 0\' prio=1 status=active\
 \|- 0:0:0:17 sdr 65:16 active ready running\
 \|- 0:0:1:17 sdmj 69:432 active ready running\
 \|- 1:0:1:17 sdble 128:1600 active ready running\
 \`- 1:0:0:17 sdaym 67:1440 active ready running

 

\# fdisk -l /dev/sdr

 

Disk /dev/sdr: 21.5 GB, 21475491840 bytes\
255 heads, 63 sectors/track, 2610 cylinders\
Units = cylinders of 16065 \* 512 = 8225280 bytes\
Sector size (logical/physical): 512 bytes / 512 bytes\
I/O size (minimum/optimal): 512 bytes / 512 bytes\
Disk identifier: 0xbaac3a38

Device Boot Start End Blocks Id System\
/dev/sdr1 1 2610 20964761 83 Linux

 

 

Solution

如果未使用 LUNs，请执行以下步骤:

使用的使用示例:

 

mpath1 (36000144000000010600aa302e54d98da) dm-19 EMC,Invista\
size=20G features=\'0\' hwhandler=\'0\' wp=rw\
\'-+- policy=\'queue-length 0\' prio=1 status=active\
\|- 0:0:0:17 sdr 65:16 active ready running\
\|- 0:0:1:17 sdmj 69:432 active ready running\
\|- 1:0:1:17 sdble 128:1600 active ready running\
\'- 1:0:0:17 sdaym 67:1440 active ready running

 

 

1\. 删除显示不正确大小的 scsi 设备:

例子:

echo 1 \> /sys/block/sdr/device/delete\
echo 1 \> /sys/block/sdmj/device/delete\
echo 1 \> /sys/block/sdble/device/delete\
echo 1 \> /sys/block/sdaym/device/delete

\# 此步骤是吧磁盘设备从内核中注销掉，确保没有进程在使用该设备。

 

 

2。删除所有显示不正确 lun 大小的 scsi 设备后，重新扫描服务器上的 scsi 设备。运行 Emulex 或 resccan-scsi-bus 提供的 Emulex 扫描脚本。或者下面的命令。

 

 

重新扫描 SCSI 主机:

 

\# for host in \`ls /sys/class/scsi_host\`\
do\
        echo \$; echo \"- - -\" \> /sys/class/scsi_host/\$/scan\
done

\#[  ]\"- - -\"表示所有的控制器，所有的通道和所有的LUN。

Or

ls /sys/class/scsi_host/ \| while read host ; do echo \"- - -\" \> /sys/class/scsi_host/\$host/scan ; done

 

\-\-\-\-\-\-\--

 

\# for host in \`ls /sys/class/fc_host\`\
do\
        echo \$; echo \"1\" \> /sys/class/fc_host/\$/issue_lip\
done

 

 

注意: 在重 IO 节点上传递 issue \_ lip 可能会导致挂起或恐慌情况。确保处于停机时间窗口。

 

 

3\. 在执行扫描之后，运行 multipath-ll 并验证 theLUN 现在的大小是否正确:

 

\# multipath -ll mpath1\
mpath1 (36000144000000010600aa302e54d98da) dm-19 EMC,Invista\
size=240G features=\'0\' hwhandler=\'0\' wp=rw\
\'-+- policy=\'queue-length 0\' prio=1 status=active\
\|- 0:0:0:17 sdr 65:16 active ready running\
\|- 0:0:1:17 sdmj 69:432 active ready running\
\|- 1:0:1:17 sdble 128:1600 active ready running\
\'- 1:0:0:17 sdaym 67:1440 active ready running

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

已使用 OneNote 创建。
