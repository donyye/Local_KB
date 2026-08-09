Linux DD方式安装驱动

2023年3月6日

20:55

- LKB 000201632

   

  旧版本的 Linux 操作系统没有内置 PERC11 驱动程序，导致安装无法检测到带有 PERC11 的虚拟磁盘。

   

  我们可以从目标操作系统的驱动程序源代码构建 PERC11 驱动程序 megaraid_sas。

  以下是 Red Hat Enterprise Linux 6.10 的示例。 其他 Linux 操作系统几乎相同。

   

  1. 从源码编译megaraid_sas驱动

  - 查找或安装与目标操作系统版本相同的Linux 操作系统。 它可以在虚拟机上。
  - 使用命令安装所需的编译工具：\
    yum groupinstall \"Development tools\"
  - 下载驱动程序并解压缩以获取源代码：
  - [https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=2xrm7&oscode=rhel8&productcode=poweredge-r740](https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=2xrm7&oscode=rhel8&productcode=poweredge-r740)

   

   

  [\[root@test610 kmod_srpm\]# rpm -ivh kmod-megaraid_sas-07.710.06.00-1.src.rpm\
     1:kmod-megaraid_sas      \########################################### \[100%\]]

   

  [\[root@test610 \~\]# cd rpmbuild/]

  [\[root@test610 rpmbuild\]# ls\
  SOURCES  SPECS]

  [\[root@test610 rpmbuild\]# cd SOURCES/]

   

  [\[root@test610 SOURCES\]# ls\
  megaraid_sas-07.710.06.00.tar.bz2  megaraid_sas.conf]

  [\[root@test610 rpmbuild\]# ls SPECS/\
  megaraid_sas.spec]

   

  [\[root@test610 rpmbuild\]# rpmbuild -bb SPECS/megaraid_sas.spec\
  Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.H89EAc\
  + umask 022\
  \...\...\
  Wrote: /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
  Wrote: /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-debuginfo-07.710.06.00-1.x86_64.rpm\
  Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.watJLx\
  + umask 022\
  + cd /root/rpmbuild/BUILD\
  + cd megaraid_sas-07.710.06.00\
  + rm -rf /root/rpmbuild/BUILDROOT/kmod-megaraid_sas-07.710.06.00-1.x86_64\
  + exit 0]

   

  [\[root@test610 rpmbuild\]# rpm -qlp /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
  /etc/depmod.d/megaraid_sas.conf\
  /lib/modules/2.6.32-754.el6.x86_64\
  /lib/modules/2.6.32-754.el6.x86_64/extra\
  /lib/modules/2.6.32-754.el6.x86_64/extra/megaraid_sas\
  /lib/modules/2.6.32-754.el6.x86_64/extra/megaraid_sas/megaraid_sas.ko]

   

  - 驱动程序 megaraid_sas 使用 rpm 为 RHEL 6.10 的 PERC11 构建

   

  2. 为 iDRAC 虚拟媒体创建映像"dd.img"以加载用于操作系统安装的驱动程序。

   

  [\[root@test610 \~\]# dd if=/dev/zero of=dd.img bs=1M count=10]\
  10+0 records in\
  10+0 records out\
  10485760 bytes (10 MB) copied, 0.00998085 s, 1.1 GB/s

   

  [\[root@test610 \~\]# mkdir /mnt/dd]

   

  [\[root@test610 \~\]# mkfs.ext3 -L OEMDRV dd.img]\
  mke2fs 1.41.12 (17-May-2010)\
  dd.img is not a block special device.\
  Proceed anyway? (y,n) y\
  Filesystem label=OEMDRV\
  OS type: Linux\
  Block size=1024 (log=0)\
  Fragment size=1024 (log=0)\
  Stride=0 blocks, Stripe width=0 blocks\
  2560 inodes, 10240 blocks\
  512 blocks (5.00%) reserved for the super user\
  First data block=1\
  Maximum filesystem blocks=10485760\
  2 block groups\
  8192 blocks per group, 8192 fragments per group\
  1280 inodes per group\
  Superblock backups stored on blocks:\
  [        8193]

  Writing inode tables: done\
  Creating journal (1024 blocks): done\
  Writing superblocks and filesystem accounting information: done

  This filesystem will be automatically checked every 22 mounts or\
  180 days, whichever comes first.[  Use tune2fs -c or -i to override.]

   

  [\[root@test610 \~\]# mount -o loop dd.img /mnt/dd]

   

  [\[root@test610 \~\]# mkdir -p /mnt/dd/rpms/x86_64/]

   

  [\[root@test610 \~\]# cp /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm /mnt/dd/rpms/x86_64/]

   

  [\[root@test610 \~\]# cd /mnt/dd/rpms/x86_64/]

   

  [\[root@test610 x86_64\]# yum install yum-utils createrepo]

   

  [\[root@test610 x86_64\]# createrepo -v .]\
  Spawning worker 0 with 1 pkgs\
  Worker 0: reading kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
  Workers Finished\
  Gathering worker results

  Saving Primary metadata\
  Saving file lists metadata\
  Saving other metadata\
  Generating sqlite DBs\
  Starting other db creation: Thu Jun 23 06:16:42 2022\
  Ending other db creation: Thu Jun 23 06:16:42 2022\
  Starting filelists db creation: Thu Jun 23 06:16:42 2022\
  Ending filelists db creation: Thu Jun 23 06:16:42 2022\
  Starting primary db creation: Thu Jun 23 06:16:42 2022\
  Ending primary db creation: Thu Jun 23 06:16:42 2022\
  Sqlite DBs complete

   

  [\[root@test610 x86_64\]# ls] 

  kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm[  repodata]

   

  [\[root@test610 x86_64\]# cd ../..]

   

  [\[root@test610 dd\]# echo \"Driver Update Disk version 3\" \> rhdd3]

   

  [\[root@test610 dd\]# tree .]\
  .\
  ├── lost+found\
  ├── rhdd3\
  └── rpms\
  [    └── x86_64\
          ├── kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
          └── repodata\
              ├── 27c0b78f5bb55b1d625effaeadb06a30fb971897787c13d7676b5847be6ce9b7-primary.sqlite.bz2\
              ├── 37e5fc322878d2296d78d41960292265b9d47b2f6d80b9bce86cf1552c3d70d0-filelists.xml.gz\
              ├── 4964b955e62e87e52e365711e646e90c7938d3676ef42b3d886777b39c743358-other.sqlite.bz2\
              ├── 5111d3dc1d7ecdc33b85896b0b46bb7030feacd4da202495bf0faf2c10f876f7-other.xml.gz\
              ├── 67365974d92a91932a1c5744d1676b79a2f6cfbc81e51512feff827b8dd99d34-filelists.sqlite.bz2\
              ├── e39f0ccff9670aac0f9c78c7a71f111e1a4fe1912518dbbdb4e80855988696b1-primary.xml.gz\
              └── repomd.xml]

  4 directories, 9 files

   

  [\[root@test610 dd\]# blkid \| grep OEMDRV]\
  /dev/loop0: LABEL=\"OEMDRV\" UUID=\"b0cddbf5-f705-40d9-b757-b929aaea9ab3\" TYPE=\"ext3\"

   

  [\[root@test610 \~\]# umount /mnt/dd]

  \
   

  3. Linux操作系统安装加载驱动

  - 在 iDRAC 可移动映像上安装 dd.img
  - 挂载安装 ISO 并从 ISO 引导
  - 指示安装程序加载附加磁盘驱动程序：
    - 对于 RHEL6，在显示 GRUB 菜单时按"TAB"，并在 linux 内核行末尾添加"dd"。
    - 对于RHEL7/8，按"e"进行编辑，移动到以linux开头的行，添加"inst.dd"
  - 按照屏幕上的说明在"dd.img"中加载驱动程序。
  - 最后，看到虚拟磁盘被检测到用于操作系统安装，如下例（RHEL6.10）

   

  ![[Technology_ALL_Linux 问题收集_089_Linux DD方式安装驱动_001.png]]

   

  备注：

  1. 请建议客户根据以下支持矩阵安装支持的操作系统。

  <https://linux.dell.com/files/supportmatrix/RHEL_Support_Matrix.pdf>

  2. 本知识库适用于坚持使用新服务器硬件安装旧版本Linux 的客户。 但我们不保证超出支持范围的旧版本 Linux 的稳定性和支持。

  3、本例附上编译好的RHEL6.10 PERC11驱动，供参考。

   

   

   

  ==================================================================

  英文版本：

   

  Describe the steps of how to accomplish the task.

   

  We can build the PERC11 driver megaraid_sas from driver source code for the targeted operating system.

  Here is an example for Red Hat Enterprise Linux 6.10. It would be almost the same for other Linux operating system.

   

  1\. Compile the megaraid_sas driver from source code

  - Find or install a Linux operating system with the same version of the targeted operating system. It can be on a virtual machine.
  - Install the required compilation tools with command:\
    yum groupinstall \"Development tools\"
  - Download the driver and unzip to get the source in:
  - [https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=2xrm7&oscode=rhel8&productcode=poweredge-r740](https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=2xrm7&oscode=rhel8&productcode=poweredge-r740)

   

  [\[root@test610 kmod_srpm\]# rpm -ivh kmod-megaraid_sas-07.710.06.00-1.src.rpm\
     1:kmod-megaraid_sas      \########################################### \[100%\]\
  \[root@test610 \~\]# cd rpmbuild/\
  \[root@test610 rpmbuild\]# ls\
  SOURCES  SPECS\
  \[root@test610 rpmbuild\]# cd SOURCES/\
  \[root@test610 SOURCES\]# ls\
  megaraid_sas-07.710.06.00.tar.bz2  megaraid_sas.conf\
  \[root@test610 rpmbuild\]# ls SPECS/\
  megaraid_sas.spec\
  \[root@test610 rpmbuild\]# rpmbuild -bb SPECS/megaraid_sas.spec\
  Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.H89EAc\
  + umask 022\
  \...\...\
  Wrote: /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
  Wrote: /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-debuginfo-07.710.06.00-1.x86_64.rpm\
  Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.watJLx\
  + umask 022\
  + cd /root/rpmbuild/BUILD\
  + cd megaraid_sas-07.710.06.00\
  + rm -rf /root/rpmbuild/BUILDROOT/kmod-megaraid_sas-07.710.06.00-1.x86_64\
  + exit 0\
  \[root@test610 rpmbuild\]# rpm -qlp /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
  /etc/depmod.d/megaraid_sas.conf\
  /lib/modules/2.6.32-754.el6.x86_64\
  /lib/modules/2.6.32-754.el6.x86_64/extra\
  /lib/modules/2.6.32-754.el6.x86_64/extra/megaraid_sas\
  /lib/modules/2.6.32-754.el6.x86_64/extra/megaraid_sas/megaraid_sas.ko]

  - The driver megaraid_sas built with rpm for PERC11 for RHEL 6.10

  2\. Create image \"dd.img\" for iDRAC virtual media to load the driver for operating system installation.

  [\[root@test610 \~\]# dd if=/dev/zero of=dd.img bs=1M count=10\
  10+0 records in\
  10+0 records out\
  10485760 bytes (10 MB) copied, 0.00998085 s, 1.1 GB/s\
  \[root@test610 \~\]# mkdir /mnt/dd\
  \[root@test610 \~\]# mkfs.ext3 -L OEMDRV dd.img\
  mke2fs 1.41.12 (17-May-2010)\
  dd.img is not a block special device.\
  Proceed anyway? (y,n) y\
  Filesystem label=OEMDRV\
  OS type: Linux\
  Block size=1024 (log=0)\
  Fragment size=1024 (log=0)\
  Stride=0 blocks, Stripe width=0 blocks\
  2560 inodes, 10240 blocks\
  512 blocks (5.00%) reserved for the super user\
  First data block=1\
  Maximum filesystem blocks=10485760\
  2 block groups\
  8192 blocks per group, 8192 fragments per group\
  1280 inodes per group\
  Superblock backups stored on blocks:\
          8193]

  Writing inode tables: done\
  Creating journal (1024 blocks): done\
  Writing superblocks and filesystem accounting information: done

  This filesystem will be automatically checked every 22 mounts or\
  180 days, whichever comes first.[  Use tune2fs -c or -i to override.\
  \[root@test610 \~\]# mount -o loop dd.img /mnt/dd\
  \[root@test610 \~\]# mkdir -p /mnt/dd/rpms/x86_64/\
  \[root@test610 \~\]# cp /root/rpmbuild/RPMS/x86_64/kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm /mnt/dd/rpms/x86_64/\
  \[root@test610 \~\]# cd /mnt/dd/rpms/x86_64/\
  \[root@test610 x86_64\]# yum install yum-utils createrepo\
  \[root@test610 x86_64\]# createrepo -v .\
  Spawning worker 0 with 1 pkgs\
  Worker 0: reading kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
  Workers Finished\
  Gathering worker results]

  [Saving Primary metadata\
  Saving file lists metadata\
  Saving other metadata\
  Generating sqlite DBs\
  Starting other db creation: Thu Jun 23 06:16:42 2022\
  Ending other db creation: Thu Jun 23 06:16:42 2022\
  Starting filelists db creation: Thu Jun 23 06:16:42 2022\
  Ending filelists db creation: Thu Jun 23 06:16:42 2022\
  Starting primary db creation: Thu Jun 23 06:16:42 2022\
  Ending primary db creation: Thu Jun 23 06:16:42 2022\
  Sqlite DBs complete\
  \[root@test610 x86_64\]# ls\
  kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm  repodata\
  \[root@test610 x86_64\]# cd ../..\
  \[root@test610 dd\]# echo \"Driver Update Disk version 3\" \> rhdd3\
  \[root@test610 dd\]# tree .\
  .\
  ├── lost+found\
  ├── rhdd3\
  └── rpms\
      └── x86_64\
          ├── kmod-megaraid_sas-07.710.06.00-1.x86_64.rpm\
          └── repodata\
              ├── 27c0b78f5bb55b1d625effaeadb06a30fb971897787c13d7676b5847be6ce9b7-primary.sqlite.bz2\
              ├── 37e5fc322878d2296d78d41960292265b9d47b2f6d80b9bce86cf1552c3d70d0-filelists.xml.gz\
              ├── 4964b955e62e87e52e365711e646e90c7938d3676ef42b3d886777b39c743358-other.sqlite.bz2\
              ├── 5111d3dc1d7ecdc33b85896b0b46bb7030feacd4da202495bf0faf2c10f876f7-other.xml.gz\
              ├── 67365974d92a91932a1c5744d1676b79a2f6cfbc81e51512feff827b8dd99d34-filelists.sqlite.bz2\
              ├── e39f0ccff9670aac0f9c78c7a71f111e1a4fe1912518dbbdb4e80855988696b1-primary.xml.gz\
              └── repomd.xml]

  [4 directories, 9 files\
  \[root@test610 dd\]# blkid \| grep OEMDRV\
  /dev/loop0: LABEL=\"OEMDRV\" UUID=\"b0cddbf5-f705-40d9-b757-b929aaea9ab3\" TYPE=\"ext3\"\
  \[root@test610 \~\]# umount /mnt/dd]

   

  3\. Load driver for Linux operating system installation

  - Mount the dd.img on iDRAC removable image
  - Mount the installation ISO and boot from ISO
  - Instruct installer to load add-on disk driver:
    - For RHEL6, press \"TAB\" when showing GRUB menu and add \"dd\" at the end of the linux kernel line.
    - For RHEL7/8, press \"e\" for edit and move to the line starting with linux and add \"inst.dd\"
  - Follow the on screen instruction to load the driver in \"dd.img\".
  - Finally, see the virtual disk was detected for OS installation as following example (RHEL6.10)

   

  ![[Technology_ALL_Linux 问题收集_089_Linux DD方式安装驱动_002.png]]

   

  Notes:

  1\. Please advise customer to install the supported operating system based on the following support matrix.

  <https://linux.dell.com/files/supportmatrix/RHEL_Support_Matrix.pdf>

  2\. This KB is for the customer who insist on installing old version of Linux with new server hardware. But we don\'t guarantee the stability and support in older version of Linux which is out of support scope.

  3\. Attached the compiled RHEL6.10 PERC11 driver in this example for reference.

   

 

已使用 OneNote 创建。
