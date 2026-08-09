UDEV-RHEL9

星期五, 2025年10月10日

上午 9:49

\# udevadm info \--query=all \--path=/block/nvme0n1 \|grep \'ID_SERIAL_SHORT\'

E: ID_SERIAL_SHORT=CN0WW56VFCP0036900RI

 

\# ls -la /dev/disk/by-id/ \| grep nvme

 

\# cat /etc/udev/rules.d/99-nvme-custom.rules

KERNEL==\"nvme\*\[0-9\]n\*\[0-9\]\", SUBSYSTEM==\"block\", ENV==\"disk\", ENV==\"CN0WW56VFCP0036900RI\", SYMLINK+=\"nvme100\"

 

使马上生效

\# udevadm control \--reload-rules;udevadm trigger \--type=devices \--action=change

 

FYI:

<https://access.redhat.com/solutions/4763241>

<https://access.redhat.com/solutions/5384031>

 

已使用 OneNote 创建。
