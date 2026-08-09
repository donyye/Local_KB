Ubuntu-dbgsym-分析

2024年6月13日

9:42

 

\# 设置 repo 仓库

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

codename=\$(lsb_release -c \| awk  \'\')

sudo tee /etc/apt/sources.list.d/ddebs.list \<\< EOF

deb <http://ddebs.ubuntu.com/> \$      main restricted universe multiverse

deb <http://ddebs.ubuntu.com/> \$-security main restricted universe multiverse

deb <http://ddebs.ubuntu.com/> \$-updates  main restricted universe multiverse

deb <http://ddebs.ubuntu.com/> \$-proposed main restricted universe multiverse

EOF

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

root@user1:/etc/apt/sources.list.d# cat ddebs.list

deb <http://ddebs.ubuntu.com> focal main restricted universe multiverse

deb <http://ddebs.ubuntu.com> focal-updates main restricted universe multiverse

deb <http://ddebs.ubuntu.com> focal-proposed main restricted universe multiverse

-或者 lsb_release -cs 命令查看

 

root@user1:\~# apt install ubuntu-dbgsym-keyring

 

root@user1:\~# wget -O - [http://ddebs.ubuntu.com/dbgsym-release-key.asc](http://ddebs.ubuntu.com/dbgsym-release-key.asc) \| sudo apt-key add -

 

root@user1:\~# apt-get update

Get:1 <http://ddebs.ubuntu.com> focal InRelease \[41.3 kB\]                   

Hit:2 <http://us.archive.ubuntu.com/ubuntu> focal InRelease

Hit:3 <http://us.archive.ubuntu.com/ubuntu> focal-updates InRelease

Hit:4 <http://us.archive.ubuntu.com/ubuntu> focal-backports InRelease

Get:5 <http://ddebs.ubuntu.com> focal-updates InRelease \[41.3 kB\]

Hit:6 <http://us.archive.ubuntu.com/ubuntu> focal-security InRelease

Get:7 <http://ddebs.ubuntu.com> focal-proposed InRelease \[41.4 kB\]

Get:8 <http://ddebs.ubuntu.com> focal/main amd64 Packages \[514 kB\]

Get:9 <http://ddebs.ubuntu.com> focal/universe amd64 Packages \[4,376 kB\]

Get:10 <http://ddebs.ubuntu.com> focal/multiverse amd64 Packages \[67.6 kB\]                                                                                                                                                       

Get:11 <http://ddebs.ubuntu.com> focal-updates/main amd64 Packages \[322 kB\]                                                                                                                                                      

Get:12 <http://ddebs.ubuntu.com> focal-updates/restricted amd64 Packages \[664 B\]                                                                                                                                                 

Get:13 <http://ddebs.ubuntu.com> focal-updates/universe amd64 Packages \[364 kB\]                                                                                                                                                  

Get:14 <http://ddebs.ubuntu.com> focal-proposed/main amd64 Packages \[61.3 kB\]                                                                                                                                                    

Get:15 <http://ddebs.ubuntu.com> focal-proposed/restricted amd64 Packages \[668 B\]                                                                                                                                                

Get:16 <http://ddebs.ubuntu.com> focal-proposed/universe amd64 Packages \[25.4 kB\]                                                                                                                                                

Get:17 <http://ddebs.ubuntu.com> focal-proposed/multiverse amd64 Packages \[668 B\]                                                                                                                                                

Fetched 5,857 kB in 10s (599 kB/s)                                                                                                                                                                                             

Reading package lists\... Done

 

 

开始安装：

\# apt install linux-image-\$(uname -r)-dbgsym -y

![[__Ubuntu___006_Ubuntu-dbgsym-分析_001.png]]

 

-在 ubuntu-dbgsym-keyring 包安装成功后，我们可以看到下面目录里多vmlinux-x包

\# ll /usr/lib/debug/boot/

-rw-r\--r\-- 1 root root 803665040 Apr 26 12:01 vmlinux-5.4.0-186-generic

 

 

root@user1:/var/crash# ll -h

total 60K

drwxrwxrwt  3 root root 4.0K Jun 13 06:50 ./

drwxr-xr-x 13 root root 4.0K Mar 14  2023 ../

drwxr-xr-x  2 root root 4.0K Jun 13 06:45 202406130645/

-rw-r\--r\--  1 root root    0 Jun 13 06:48 kdump_lock

-rw-r\--r\--  1 root root  249 Jun 13 06:50 kexec_cmd

-rw-r\-\-\-\--  1 root root  44K Jun 13 06:50 linux-image-5.4.0-186-generic-202406130645.crash

 

使用 crash 命令进行分析：

root@user1:\~# crash /usr/lib/debug/boot/vmlinux-5.4.0-186-generic /var/crash/202406130645/dump.202406130645\
 

![[__Ubuntu___006_Ubuntu-dbgsym-分析_002.png]]

 

 

分析不同 kdump的方法

 

1. 检查是否有合适的 dbgsym

\# apt list \|grep \"linux-image-\*.\*.\*-generic-dbgsym\"

 

2. 安装所需要的包

\# apt install linux-image-unsigned-5.4.0-26-generic-dbgsym -y

 

3. 可以看到多一个

root@user1:\~# ll /usr/lib/debug/boot/

total 1544420

drwxr-xr-x 2 root root      4096 Jun 13 07:26 ./

drwxr-xr-x 4 root root      4096 Jun 13 06:42 ../

-rw-r\--r\-- 1 root root 803665040 Apr 26 12:01 vmlinux-5.4.0-186-generic

-rw-r\--r\-- 1 root root 777798448 Apr 20[  2020 vmlinux-5.4.0-26-generic][  ]\<\-- 新增加的

 

跟多的 kdump 分析参考下面：

FYI：https://www.ebpf.top/post/ubuntu_kdump_crash/

 

已使用 OneNote 创建。
