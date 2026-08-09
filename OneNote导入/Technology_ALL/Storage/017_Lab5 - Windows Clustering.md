Lab5 - Windows Clustering

Friday, November 22, 2013

 

1.两台服务器，四张网卡：

一张网卡Cluster之间的Private network(192.168.62.148 & 192.168.62.146)

两张网卡连接iscsi ， 四路径 

192.168.60.148[  -\> 192.168.60.232 & 192.168.60.233                  192.168.60.146   -\> 192.168.60.232 & 192.168.60.233]

192.168.61.148[  -\> 192.168.61.232 & 192.168.61.233                 192.168.61.146   -\> 192.168.61.232 & 192.168.61.233]

一张网卡连接域(192.168.200.148) domain DC(192.168.200.1)[  domain name:msapp.local]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_001.png]]

 

2.两台服务器配好iscsi initiator

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_002.png]]

\*防火墙要关闭掉，不然可能会PING不通

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_003.png]]

 

3\.

\*Student1 安装MDSM & Host，MDSM用来管理MD3260i

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_004.png]]

\*Student2只需要安装多路径软件即可(Host)

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_005.png]]

 

4.创建Host Group

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_006.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_007.png]]

 

5.为Host Group分配两个LUN

STU14Quorum 2G \--Quorum分区

STU14CLUSTERFILE \-- File Services分区

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_008.png]]

\*确认设备是否有被识别，通常都需要扫描下硬件

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_009.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_010.png]]

\*\*盘符务必要一致

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_011.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_012.png]]

 

6.添加Cluster

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_013.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_014.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_015.png]]

 

7.创建Cluster

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_016.jpg]]

\*\*此处务必使用主机Hostname+域名

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_017.png]]

\*选择公网的网卡，并且为Cluster Server分配个IP地址（不能冲突）

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_018.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_019.png]]

\*\*一开始看到文件共享盘是没有盘符的，为其分配个盘符

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_020.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_021.png]]

 

8.添加文件服务器角色

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_022.png]]

 

\*为两台服务器都装上File Services

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_023.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_024.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_025.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_026.png]]

 

9.创建一个文件服务器

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_027.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_028.png]]

\*为文件服务器指定一个服务器名和IP

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_029.png]]

\*为文件服务器分配磁盘

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_030.png]]

\*创建好如下图

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_031.png]]

\*目前文件服务是在STUDENT14A上

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_032.png]]

 

查看Cluster的实现效果:

将文件服务器转移到STUDENT14B上，文件服务器转移到了B上。

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_033.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_034.png]]

\*将cluster磁盘设为共享

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_035.png]]

用IP为192.168.200.149 ， 主机名为STUDENT14的主机访问这个共享目录：\\\\stu14clusterfs，答案是可以的，也可以正常写入。

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_036.png]]

 

如何添加磁盘:

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_037.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_038.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_039.png]]

![[Technology_ALL_Storage_017_Lab5 - Windows Clustering_040.png]]

 

 

 

步骤参考：

4.在MDSM识别到两台服务器，并且添加 Host Group，分配LUN(一块2GB Quorum Disk；一块文件共享10GB)

\*会自动挑最小的磁盘作为Quorum Disk

5.到服务器上online好，初始化好硬盘（注意写下卷标，以便于将两台服务器上的磁盘盘符一致）

6.两台机器安装群集软件 Features -\> Add Features -\> Failover Cluster Manager

7.创建群集（Cluster Name不能冲突,Cluster IP: 192.168.200.140）

\*注意添加两台主机的时候，最好用完全的主机名(student14a.msapp.local)

8.两台机器安装Roles -\>Add Roles -\>File Server

9.在cluster机器上配置File Server(192.168.200.141)

Services and application

10.将共享磁盘设置共享权限

10.尝试自动切换,看是否能成功

 

已使用 OneNote 创建。
