Disk-磁盘信息2-nvme槽位

2025年2月8日

14:18

通过sosreport日志与TSR日志确认PCI nvme 槽位信息

 

下面红色标红的是 十六进制的系统下看到的设备号，如果要与TSR日志对应，可以转换到十进制。

 

比如 b7 和 b8 的十六进制转换成十进制分别是 183 和 184

 

[ sosreport ]日志：\
sos_commands/block/ls\_-lanR\_.sys.block:\
lrwxrwxrwx[  1 0 0 0 Feb  5 16:15 nvme0n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys0/nvme0n1]

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme10c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:04.0/0000:b7:00.0/nvme/nvme6/nvme10c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme10n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys10/nvme10n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme11c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:0e.0/0000:bd:00.0/nvme/nvme10/nvme11c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme11n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys11/nvme11n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme1c65n1 -\> ../devices/pci0000:5d/0000:5d:00.0/0000:5e:00.0/0000:5f:04.0/0000:60:00.0/0000:61:05.0/0000:67:00.0/nvme/nvme1/nvme1c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme1n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys1/nvme1n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme2c65n1 -\> ../devices/pci0000:5d/0000:5d:00.0/0000:5e:00.0/0000:5f:04.0/0000:60:00.0/0000:61:0c.0/0000:6a:00.0/nvme/nvme2/nvme2c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme2n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys2/nvme2n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme3c65n1 -\> ../devices/pci0000:5d/0000:5d:00.0/0000:5e:00.0/0000:5f:04.0/0000:60:00.0/0000:61:0d.0/0000:6b:00.0/nvme/nvme3/nvme3c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme3n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys3/nvme3n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme4n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys4/nvme4n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme5c65n1 -\> ../devices/pci0000:5d/0000:5d:00.0/0000:5e:00.0/0000:5f:04.0/0000:60:00.0/0000:61:0f.0/0000:6d:00.0/nvme/nvme5/nvme5c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme5n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys5/nvme5n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme6c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:0d.0/0000:bc:00.0/nvme/nvme9/nvme6c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme6n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys6/nvme6n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme7c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:0f.0/0000:be:00.0/nvme/nvme11/nvme7c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme7n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys7/nvme7n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme8c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:05.0/0000:b8:00.0/nvme/nvme7/nvme8c65n1

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme8n1 -\> ../devices/virtual/nvme-subsystem/nvme-subsys8/nvme8n1

 

 

TSR log:\
 

PCI Devices

![[CPU-Disk-MEM-Performances_006_Disk-磁盘信息2-nvme槽位_001.png]]

 

 

对应TSR如下：

b7 \--\> 183:00:00[   \--\> Disk bay 7 ]说明第七个硬盘槽位

b8 \--\> 184:00:00[   \--\> Disk bay 6 ]说明第六个硬盘槽位

 

单独拿出现：

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme10c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:04.0/0000:b7:00.0/nvme/nvme6/nvme10c65n1

 

lrwxrwxrwx  1 0 0 0 Feb  5 16:15 nvme8c65n1 -\> ../devices/pci0000:ae/0000:ae:00.0/0000:af:00.0/0000:b0:04.0/0000:b1:00.0/0000:b2:05.0/0000:b8:00.0/nvme/nvme7/nvme8c65n1

 

 

 

 

 

已使用 OneNote 创建。
