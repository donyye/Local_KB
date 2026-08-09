Multipath-iscsi

2023年12月26日

9:05

配置iscsi

 

iscsiadm -m discovery -t st -p 10.10.40.244

iscsiadm -m discovery -t st -p 192.168.200.244

 

iscsiadm -m node -T iqn.2022.com.share:disk-1 -p 10.10.40.244 -l

iscsiadm -m node -T iqn.2022.com.share:disk-1 -p 192.168.200.244 -l

 

iscsiadm -m node -T iqn.2022.com.share.disk.dis-2 -p 10.10.40.244 -l

iscsiadm -m node -T iqn.2022.com.share:disk-1 -p 192.168.200.244 -l

 

iscsiadm -m node

 

[ ]\# cat /proc/scsi/scsi

......

Host: scsi34 Channel: 00 Id: 00 Lun: 02

[  Vendor: FreeNAS  Model: iSCSI Disk       Rev: 0123]

  Type:   Direct-Access                    ANSI  SCSI revision: 07

Host: scsi33 Channel: 00 Id: 00 Lun: 02

 [ Vendor: FreeNAS  Model: iSCSI Disk       Rev: 0123]

  Type:   Direct-Access                    ANSI  SCSI revision: 07

Host: scsi35 Channel: 00 Id: 00 Lun: 01

 [ Vendor: FreeNAS  Model: iSCSI Disk       Rev: 0123]

  Type:   Direct-Access                    ANSI  SCSI revision: 07

Host: scsi36 Channel: 00 Id: 00 Lun: 01

[  Vendor: FreeNAS  Model: iSCSI Disk       Rev: 0123]

  Type:   Direct-Access                    ANSI  SCSI revision: 07

 

 

彻底删除已连接的iSCSI存储设备：

service iscsi stop

rm -rf /var/lib/iscsi/nodes/\*

rm -rf /var/lib/iscsi/send_targets/\*

service iscsi start

 

 

====脚本====

echo \"== 1 ==\"

iscsiadm -m discovery -t st -p 10.10.40.211

iscsiadm -m discovery -t st -p 192.168.200.211

echo \"== 2 ==\"

iscsiadm -m node -T iqn.1992-04.com.emc:cx.virt2146y7c8rr.a0 -p 10.10.40.211 -l

echo \" \"

iscsiadm -m node -T iqn.1992-04.com.emc:cx.virt2146y7c8rr.a1 -p 192.168.200.211 -l

echo \" \"

echo \"== 3 ==\"

iscsiadm -m node -T iqn.1992-04.com.emc:cx.virt2146y7c8rr.a0 -p 10.10.40.211 -o update -n node.startup -v automatic

echo \" \"

iscsiadm -m node -T iqn.1992-04.com.emc:cx.virt2146y7c8rr.a1 -p 192.168.200.211 -o update -n node.startup -v automatic

echo \"OK\"

echo \" \"

echo \"== 4 ==\"

systemctl enable iscsid.service

systemctl enable iscsi.service

echo \" \"

echo \"== 5 ==\"

multipath -t \> /etc/multipath.conf

systemctl start multipathd.service

systemctl enable multipathd.service

echo \" \"

echo \"== 6 ==\"

multipath -ll

 

 

 

已使用 OneNote 创建。
