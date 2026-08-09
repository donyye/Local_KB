USB配置kdump方法

Wednesday, January 29, 2014

1:07 PM

// 将USB盘插入新的服务器，配置kdump方法

 

1.连接usb设备到服务器，

 

2.格式化usb device，NOTE：USB已经格式化完毕，此步骤可以忽略，如果是新U盘需要按此操作

\# mkfs.ext3 /dev/usb_device_name

 

3. 察看usb device\'s UUID，如果USB盘是/dev/sdc1

\# blkid

/dev/sdc1: LABEL=\"/\" UUID=\"43307fc2-3433-4ad2-98a1-22375c2cf9e6\" TYPE=\"ext3\"

 

 

 

4.修改/etc/kdump.conf, 添加以下3条，

blacklistmegaraid_sas

ext3 UUID=43307fc2-3433-4ad2-98a1-22375c2cf9e6

core_collectormakedumpfile -c \--message-level 1 -d 31

 

5.重行启动服务kdump，重新生成/boot/initrd-kdump.img文件，（实际文件名根据实际状况确认）

 

6. 解压新生成的initrd-kdump.img文件

  \# mkdir /boot/test

  \# cd /boot/test/

  \# zcat ../initrd-kdump.img \| cpio -idv

 

7. 修改/boot/test/init文件, 加\#注解ohci-hcd.ko和uhci-hcd.ko两个模块

\# vi /boot/test/init

#Insmod /lib/modules/2.6.18-308.el5/ohci-hcd.ko

#Insmod /lib/modules/2.6.18-308.el5/uhci-hcd.ko

 

8. 修改/boot/test/etc/critical_disks文件，

只留sda，删除/加\#注解之外的其它设备名

 

9. 保存修改，重新打包生成initrdkdumpimg文件

\# find ./ \| cpio -c -o \> ../1.img

\# gzip ../1.img

\# cd ..

\# mv 1.img.gz  initrd-2.6.18-308.el5kdump.img

 

10. 重起kdump服务

\# servicekdump restart

 

11.触发crash测试

#echo c \> /proc/sysrq-trigger,

 

 

 

在系统下开启NMI功能

//检查NMI是否为开启

\[root@localhost \~\]# sysctlkernel.unknown_nmi_panic

kernel.unknown_nmi_panic = 0    \<\-- 表示为禁用状态

kernel.panic_on_unrecovered_nmi = 0

// 编辑配置文件在系统启用NMI

\[root@localhost \~\]# vim /etc/sysctl.conf

kernel.unknown_nmi_panic = 1   \<\-- 添加此行

kernel.panic_on_unrecovered_nmi = 1 \<\-- 添加此行

 

//重新加载设定

\[root@localhost \~\]# sysctl -p

 

在通过前面板的NMI按钮，即可触发一个dump。

 

已使用 OneNote 创建。
