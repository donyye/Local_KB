RHCS双机集群调测

Friday, November 15, 2013

9:45 AM

RHCS双机集群调测

 

RHCS简介

 

Red Hat Cluster Suite 是一款能够提供高性能、高可靠性、负载均衡、高可用性且经济廉价的集群工具集。一个集群通常有两个或两个以上的计算机（称为"节点"或"成员"）共同执行一个任务。

其中有四种常见集群：

a存储集群

b高可用性集群

c负载均衡

d高性能集群

 

存储集群

存储集群在一个集群中为服务提供一个一致的文件系统映像，允许服务同时去读写一个单一的共享文件系统。存储集群通过将数据放到一个共享文件系统中从而消除 了在应用程序间拷贝数据的麻烦，并提供一个单一的备份和故障恢复点。Red Hat Cluster Suite通过Red Hat GFS提供一个存储集群。

 

高可用性集群

高可用性集群通过消除单一故障点和节点故障转移功能（当一个集群节点失败后将服务转移到其他节点上）来提供高可用性。节点故障转移功能对客户端是透明的， 当节点失败后客户端并不会看到节点之间的服务转移。Red Hat Cluster Suite通过高可用性服务管理组件来提供一个高可用性集群。

 

负载均衡

负载均衡集群将服务请求调度到集群中的多个节点上。负载均衡是一个低成本、高可用性的集群，因为你可以根据负载情况灵活的添加和删除节点。在负载均衡集群 中当一个节点失败后，调度器会发现这个失败并停止向此节点发送请求。节点的失败对于客户端是透明的。Red Hat Cluster Suite 通过LVS（Linux Virtual Server）来提供负载均衡集群。

 

高性能集群

高性能集群在节点上执行并行计算。高性能集群允许应用程序并行计算从而提高应用程序性能。

 

RHCS主要有下面部分组成：

集群架构\--提供一个基本功能使节点作为集群工作在一起：配置文件管理，成员关系管理，锁管理和栅设备。

 

高可用性服务管理\--提供节点失败转移服务，当一个节点失败后将服务转移到另一个节点上。

 

集群管理工具\--通过配置和管理工具来配置和管理Red Hat集群。

 

Linux Virtual Server (LVS)\--LVS提供一个基于IP的负载均衡功能，通过LVS可以将客户请求均匀的分配到集群节点上。

 

你可以通过下面的组件补充Red Hat集群：

◆Red Hat GFS (Global File System)\--GFS为Red Hat Cluster Suite提供一个集群文件系统，CFS允许多个节点在块级别上共享存储。

◆Cluster Logical Volume Manager (CLVM)\--提供逻辑卷管理集群存储。

◆Global Network Block Device (GNBD)\--GFS的一个补充组件用于将存储在块级别导出到以太网上。

 

以上内容引自：<http://bkeep.blog.163.com/blog/static/123414290201043132231414/>

 

通过上面的介绍对RHCS有了一个简单的认识。现在知道它的功能强大了吧，最重要的的是RHCS是Redhat企业版自带的的一个集群套件，有了它就不用再去采购第三方的集群软件了，从经济上来说也更加划算。

 

注：之前的RedHat软件打包或者发行光盘的时候，分成好多种，比如RHEL AS高级服务器版，ES企业版, WS工作站版, Desktop桌面版, RHCS 集群套件, GFS集群文件系统, DS开发者版本等等一大堆，从 5开始采用新的打包发行方式，大大简化了之前的产品包分类:

 

现在只有2种可以购买到或下载到，现在统一叫AP版，目前最新版本是6.1

\* Red Hat Enterprise Linux 5 Client

\* Red Hat Enterprise Linux 5 Server

 

本次实施采用的是操作系统RHEL AP5.5，数据库是oracle10g，硬件是HP DL380 G7（接上篇）

 

本集群是高可用性集群，也可以简单的理解成为双机热备。更准备的说是双机互备。因为要做一个oracle与应用接口的双机，正常的情况下是qddb1做oracle的主节点，qddb2做oracle的备节点。qddb2做应用接口的主节点，而qddb1做应用接口的备节点。以下是IP地址规划情况：

 

IP地址规划

  ---------------------------------- ------------------------------------ ---------------------------------- ---------------------------------- ------------------------------------------------------------------------------------------ ---------------------------------- ---------------------------------- ------------------------------------
  名称   主机名   网卡   绑定   IP地址   掩码   网关   浮动ip
  数据库、应用接口服务器             qddb1                                eth0                               bond0                              172.16.0.1                                                                                 255.255.255.0                      172.16.0.254                       172.16.0.100
                                                                                                                                                                                                                                                                                                                 （数据库）
                                                                                                                                                                                                                                                                                                                 172.16.0.101
                                                                          eth1                                                                                                                                                                                                                                   （应用接口）
                                                                          eth2                               bond1                              192.168.0.1                                                                                255.255.255.0                                                          
                                                                          eth3                                                                                                                                                                                                                                    
                                                                          ILO                                                                   192.168.0.3                                                                                                                                                       
                                     qddb2                                eth0                               bond0                              172.16.0.2                                                                                 255.255.255.0                      172.16.0.254                       172.16.0.100
                                                                                                                                                                                                                                                                                                                 （数据库）172.16.0.101
                                                                          eth1                                                                                                                                                                                                                                   （应用接口）
                                                                          eth2                               bond1                              192.168.0.2                                                                                255.255.255.0                                                          
                                                                          eth3                                                                                                                                                                                                                                    
                                                                          ILO                                                                   192.168.0.4                                                                                                                                                       
  ---------------------------------- ------------------------------------ ---------------------------------- ---------------------------------- ------------------------------------------------------------------------------------------ ---------------------------------- ---------------------------------- ------------------------------------

 

每台HP服务器上有4个以太网口与一个ILO口。其中eth1与eth2做端口绑定bond0，eth3以与eth4做端口绑定bond1。将bond0用做应用服务的网络接口，分配的网段为172.16.0.0/24，将bond1做心跳，和ILO口放在一个网段192.168.0.0/24。接入交换机时将bond0与bond1的2个网口分别接入不同交换机。

 

补充一些关于fence设备的知识，后面要用到：

1为什么要用到fence设备？

因为纯软件并不能实现真正意义上的HA，为了解决脑列现象，所以需要硬件的支持。

2 RHCS fence设备的分类？

通常分为2类：

常见的内部fence设备有：

   IBM服务器提供的RSAII卡设备

   HP服务器提供的iLO卡

   DELL服务器提供的DRAC卡

   智能平台管理接口IPMI

常见的外部fence设备有：

   UPS

   SAN SWITCH，

   NETWORK SWITCH

 

安装前的准备工作

1\>分别在2个节点上编辑/etc/hosts文件，加入IP地址和主机对应表，修改后的hosts示范如下:

\[root@qddb1 /\]# cat /etc/hosts

\# Do not remove the following line, or various programs

\# that require network functionality will fail.

127.0.0.1       localhost

172.16.0.1       qddb1

172.16.0.2       qddb2

192.168.0.1     qddb1-priv

192.168.0.2     qddb2-priv

 

2\>打开2个节点上的xdmcp服务，方便使用xmanager工具进行图形化远程操作。

a 禁用selinux与iptables

b 修改vi /etc/gdm/custom.conf

\[xdmcp\]

Enable=1

c修改defaults.conf

chmod 700 /usr/share/gdm/defaults.conf

vi defaults.conf

AllowRoot=true

AllowRemoteroot=true

AllowRemoteAutoLogin=false

Enable=true

DisplaysPerHost=10

Port=177

d重启gdm服务

在root用户下执行gdm-restart

以上方式只是针对RHEL AP5.5，如果是其它版本的系统方法不尽相同，此外也可以安装VNC等其它图形化服务软件包，根据个人使用习惯进行选择。

 

3\>配置HP的ILO

ILO需要开始启动的时候进行配置。

1、开机自检时，按F8键进入iLO的设置界面：

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_001.jpg]]](http://img1.51cto.com/attachment/201110/160137700.jpg)

2、进入iLO的设置：这项是将所有的设置恢复为出厂值。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_002.jpg]]](http://img1.51cto.com/attachment/201110/160156572.jpg)

3、配置网络：

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_003.jpg]]](http://img1.51cto.com/attachment/201110/160219686.jpg)

分别设置IP 地址和DNS。IP应和服务器的其中一个网段的ip在同一个网段中，注意子网掩码的一致。(只有在DHCP被设为Disable时，才能设置IP address/Subnet Mask/Gateway IP address)

DNS的名字在服务器前面带的卡片上，还包括管理员的账号和密码。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_004.jpg]]](http://img1.51cto.com/attachment/201110/160302123.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_005.jpg]]](http://img1.51cto.com/attachment/201110/160322938.jpg)

IP必须是静态的，所以DHCP需设置为OFF。

 

4、在这里可以添加、删除、更改远程访问User的密码，权限等。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_006.jpg]]](http://img1.51cto.com/attachment/201110/160407976.jpg)

Add user

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_007.jpg]]](http://img1.51cto.com/attachment/201110/160457765.jpg)

Remove

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_008.jpg]]](http://img1.51cto.com/attachment/201110/161000420.jpg)

Edit

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_009.jpg]]](http://img1.51cto.com/attachment/201110/161039505.jpg)

 

5、Settings 的选项设置Keyboard的属性等，一般都为默认值。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_010.jpg]]](http://img1.51cto.com/attachment/201110/161125684.jpg)

6、About中为iLO的firmware version等一些信息。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_011.jpg]]](http://img1.51cto.com/attachment/201110/161151303.jpg)

 

RHCS安装过程

采用手动安装，首先将光盘挂载到/mnt目录下。然后进行如下操作。

a 建立文件/etc/yum.repos.d/mycdrom.repo，内容如下：

\[Base\] 

name=RHEL5 ISO Base 

baseurl=file:///mnt/Server 

enabled=1

gpgcheck=0

\[Cluster\] 

name=RHEL5 ISO Cluster 

baseurl=file:///mnt/Cluster 

enabled=1

gpgcheck=0

 

b 执行yum命令进行集群组件的安装：

yum install cman rgmanager system-config-cluster luci ricci

使用yum源安装方式主要是为了自动安装依赖性包。

 

RHCS配置过程

cluster配置（单节点运行）

1.1 开始配置cluster：出现配置界面时点击「Create New Configuration」

方法一：用命令启动图型配置界面

\# system-config-cluster

方法二：在图形界面下从应用程序－系统配置－服务器设置－cluster manegement：

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_012.jpg]]](http://img1.51cto.com/attachment/201110/162820349.jpg)

 

1.2 为cluster起一个名字，如lyapp_cluster；选择锁方式：默认即可；点击「OK」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_013.jpg]]](http://img1.51cto.com/attachment/201110/162915151.jpg)

1.3 添加cluster节点：点击「Cluster Nodes」，并点击「Add Cluster Node」，输入节点名，并点击「OK」即可，其他几个节点以次增加；

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_014.jpg]]](http://img1.51cto.com/attachment/201110/162951985.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_015.jpg]]](http://img1.51cto.com/attachment/201110/163046631.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_016.jpg]]](http://img1.51cto.com/attachment/201110/163105159.jpg)

1.4 添加Fence设备：选中Fances Devices，并点击「Add a Fance Device」；选中HP ILO Device，为ILO卡命名及输入登陆名、密码和ip地址，点击「Ok」；其他几个节点以次增加；

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_017.jpg]]](http://img1.51cto.com/attachment/201110/163147120.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_018.jpg]]](http://img1.51cto.com/attachment/201110/163248701.jpg)

1.5 添加Fence级别：选中节点，点击「Manager Fanceing For This Node」，之后点击「Add a New Fence Level」，点击「Close」；其他几个节点以次增加；

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_019.jpg]]](http://img1.51cto.com/attachment/201110/163322413.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_020.jpg]]](http://img1.51cto.com/attachment/201110/163348540.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_021.jpg]]](http://img1.51cto.com/attachment/201110/163403341.jpg)

1.6 添加节点级别上的Fence设备：选中节点下面的Fence-Level-1,点击「Add New Fence to this Level」，选择节点对应的Fence设备，最后点击「OK」；其他几个节点以次增加；

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_022.jpg]]](http://img1.51cto.com/attachment/201110/163550329.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_023.jpg]]](http://img1.51cto.com/attachment/201110/163550114.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_023.jpg]]](http://img1.51cto.com/attachment/201110/163550417.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_024.jpg]]](http://img1.51cto.com/attachment/201110/163550232.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_025.jpg]]](http://img1.51cto.com/attachment/201110/163631385.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_026.jpg]]](http://img1.51cto.com/attachment/201110/163631744.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_027.jpg]]](http://img1.51cto.com/attachment/201110/163631515.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_028.jpg]]](http://img1.51cto.com/attachment/201110/163631428.jpg)

1.7 配置Failover Domains：

 第一步：点击Failover Domains，点击「Create a Failover Domain」为其命名并点击「确定」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_029.jpg]]](http://img1.51cto.com/attachment/201110/163709149.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_030.jpg]]](http://img1.51cto.com/attachment/201110/163709882.jpg)

第二步：配制Failover Domains，添加相应成员节点，且选中两项规则；并点击「确定」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_031.jpg]]](http://img1.51cto.com/attachment/201110/163815592.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_032.jpg]]](http://img1.51cto.com/attachment/201110/163815169.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_033.jpg]]](http://img1.51cto.com/attachment/201110/163816748.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_034.jpg]]](http://img1.51cto.com/attachment/201110/163816636.jpg)

1.8 配置资源：

第一步：选中Resources，点击「Create a Resource」，选择IP Address并输入与其绑定的ip地址且选中右边的一条规则，点击「OK」。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_035.jpg]]](http://img1.51cto.com/attachment/201110/163904242.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_036.jpg]]](http://img1.51cto.com/attachment/201110/163904461.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_037.jpg]]](http://img1.51cto.com/attachment/201110/163904552.jpg)

第二步：选中Resources，点击「Create a Resource」，选择Script并输入脚本名及其路径名，并点击「OK」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_037.jpg]]](http://img1.51cto.com/attachment/201110/163952225.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_038.jpg]]](http://img1.51cto.com/attachment/201110/163952566.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_039.jpg]]](http://img1.51cto.com/attachment/201110/163952905.jpg)

1.9 配置服务：

第一步：选中Services，点击「Create a Service」为其命名并点击「OK」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_040.jpg]]](http://img1.51cto.com/attachment/201110/164030628.jpg)

第二步：点击「Add a Share Resource to this service」，选择里边的共享ip资源，并点击「确定」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_041.jpg]]](http://img1.51cto.com/attachment/201110/164107186.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_042.jpg]]](http://img1.51cto.com/attachment/201110/164107784.jpg)

 第三步：点击「Add a Share Resource to this service」，选择里边的jboss_script资源，并点击「OK」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_043.jpg]]](http://img1.51cto.com/attachment/201110/164132345.jpg)

第五步：在「Autostart This Service」前面打钩，选中Failover Domain里面的「lyapp_failover」，选择Relocate，并点击「Close」

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_044.jpg]]](http://img1.51cto.com/attachment/201110/164225630.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_045.jpg]]](http://img1.51cto.com/attachment/201110/164225298.jpg)

1.10 保存cluster配置，选择「File」,点击「Save」；再连续点击两次「OK」退出；最后退出。

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_046.jpg]]](http://img1.51cto.com/attachment/201110/164331150.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_047.jpg]]](http://img1.51cto.com/attachment/201110/164331749.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_048.jpg]]](http://img1.51cto.com/attachment/201110/164331935.jpg)

 

[![[Technology_ALL_未分类知识库_001_RHCS双机集群调测_049.jpg]]](http://img1.51cto.com/attachment/201110/164331130.jpg)

2. 同步cluster配置

将节点1上的cluster配置文件拷贝到其它节点：

\[root@lyapp1 /\]# scp /etc/cluster/cluster.conf lyapp2:/etc/cluster

 

3. 重启群集内的各个节点，cluster即可起来！

 

注：为了安全起见，没有采用现网的截图，但总的配置过程如图所示，维一不同的是现网有oracle与接口应用2个service，而且需要分别互为主备，为了实现这个需求，需要配置2个failover domains，并且在添加成员节点时，珍对不同的服务配置不同的节点优先级。其实为了更加保险，可以在添加了第一个共享IP资源后，可以采用附加资源的方式，依次附加文件系统，脚本等其它共享资源。这样做的目的是体现资源的层次及依赖关系。如果oracle是采用裸设备方式进行管理，则添加共享资源时不用添加文件系统。

 

RHCS集群管理

其实总的来说RHCS还是很方便管理的。

方法一：

使用图形化工具可以很直观的看到集群的状态

 

方法二：

由于条件所限，相对来说用命令进行管理使用提更加普遍。常用的命令如下：

1RHCS启动

分别在2个节点上执行

service cman start

分别在2个节点上执行

service rgmanager start

注：cman可以同步在2台服务器上执行，等2个节点上cman下的相关服务都启来后，然后在2个节点上分别启动rgmanager

2RHCS停止

分别在2个节点上执行

service rgmanager stop

分别在2个节点上执行

service cman stop

注：可以看出停止与启动是相反的操作，先停rgmanager，再停cman相关服务。

 

3Cluster服务启动后的正常状态如下：

\[root@qddp1 \~\]# clustat

Member Status: Quorate

 

 Member Name                              Status

 \-\-\-\-\-- \-\-\--                              \-\-\-\-\--

 qddb1                                 Online, Local, rgmanager

 qddb2                                 Online, rgmanager

 

 Service Name         Owner (Last)                   State        

 \-\-\-\-\-\-- \-\-\--         \-\-\-\-- \-\-\-\-\--                   \-\-\-\--        

 ios_server             qddb1                       started

  ora_server             qddb2                       started

 

4卸载掉文件系统（disable）

\[root@qddp1 \~\]# clusvcadm --d ios_server

 

5将ios_server服务在指定的节点上启动

\[root@qddp1 \~\]# clusvcadm --e ios_server --m qddb2

 

6切换服务

\[root@qddp1 \~\]# clusvcadm --r ios_server

 

注：如果需要对应用进行升级操作，可以手工卸载掉文件系统，然后再将文件系统挂载到一台服务器上，再执行应用升级操作，升级完成后再通过clusvcadm启动服务。

本文出自 "[清枫拂面](http://crazy123.blog.51cto.com/)" 博客，请务必保留此出处[http://crazy123.blog.51cto.com/1029610/684394](http://crazy123.blog.51cto.com/1029610/684394)

 

源文档 \<[http://crazy123.blog.51cto.com/1029610/684394](http://crazy123.blog.51cto.com/1029610/684394)\> 

 

 

已使用 OneNote 创建。
