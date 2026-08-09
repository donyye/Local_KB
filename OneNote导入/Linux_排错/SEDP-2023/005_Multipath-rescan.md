Multipath-rescan

2023年12月26日

9:06

\>\> 磁盘扩容

存储LUN扩容

 

lvextend -l +100%FREE /dev/mapper/vg0-lv0

 

 

\>\> 重新扫描磁盘

yum install  sg3_utils

 

rescan-scsi-bus.sh[   \#]扫描每个 scsi 主机设备或运行 rescan-scsi-bus.sh 脚本来检测新磁盘。

扫描后可以在 /dev/disk/by-id 目录下找到它们

rescan-scsi-bus.sh -r \#？？？

 

\-\-\-\-\-\-\-\-\--iscsi rescan\-\-\-\-\-\-\-\-\--

\# for host in \`ls /sys/class/scsi_host\`\
do\
        echo \$; echo \"- - -\" \> /sys/class/scsi_host/\$/scan\
done

\#[  \"- - -\"]表示所有的控制器，所有的通道和所有的LUN。

Or

ls /sys/class/scsi_host/ \| while read host ; do echo \"- - -\" \> /sys/class/scsi_host/\$host/scan ; done

 

\-\-\-\-\-\-\-\-\--disk rescan\-\-\-\-\-\-\-\-\--

\# for host in \`ls /sys/class/fc_host\`\
do\
        echo \$; echo \"1\" \> /sys/class/fc_host/\$/issue_lip\
done

 

 

 

\>\> 查看iSCSI信息命令

\# sg_readcap -16 /dev/sdb

 

 sg_luns -d /dev/sdc

 

[ FYI:]

How to resize multipath device in RHEL after it has been resized from storage?

<https://access.redhat.com/solutions/65399>

 

 

 

已使用 OneNote 创建。
