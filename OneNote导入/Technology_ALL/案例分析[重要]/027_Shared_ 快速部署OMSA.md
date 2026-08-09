Shared: 快速部署OMSA

2015年3月19日

8:35

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Shared: 快速部署OMSA
  发件人     Wang, Sean
  收件人     CCC XMN Enterprise ProSupport 2
  抄送       Zeng, Richa; Ye, Dony; Zheng, Benjamin
  发送时间   2015年3月18日 19:11
  附件       \<\<sa.sh\>\>
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

从下面的实验可以看到，安装OMSA时需要用到OS上的一些library。

......

......

Installing for dependencies:

 cim-schema[                              noarch           2.22.0-2.1.el6                rhel65           1.0 M]

 libcmpiCppImpl0                         x86_64           2.0.1-5.el6                   rhel65            71 k

 libsmbios                               x86_64           2.2.27-4.4.1.el6              OMSA             2.0 M

 libwsman1                               x86_64           2.2.3-8.el6                   rhel65           112 k

 openwsman-client                        x86_64           2.2.3-8.el6                   rhel65            28 k

 openwsman-server                        x86_64           2.2.3-8.el6                   rhel65            87 k

 sblim-sfcb                              x86_64           1.3.11-2.el6                  rhel65           443 k

 sblim-sfcc                              x86_64           2.2.2-2.el6                   rhel65            88 k

 smbios-utils-bin                        x86_64           2.2.27-4.4.1.el6              OMSA             119 k

 

另外"libcmpiCppImpl0[  ]"与"tog-pegasus"有相同的文件名，Redhat 已知issue。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- 

谢谢Dony的支持。

 

Shared:

 

 

 

1.  客户有超过100+的服务器（R720、R720XD、R920）需要部署OMSA，以便第三放监控软件通过OMSA抓取服务器信息；客户不同意使用OME
2.  客户起初打算使用Dell OpenManage Linux repository(http://linux.dell.com/repo/hardware/)，但是由于该服务器在美国以及客户网路环境限制，无法下载。我在公司经过测试，可以下载，但是文件超过100GB。
3.  于是如下方法是在本地创建一个OMSA YUM 仓库，客户端从服务端下载一个脚本运行安装omsa
4.  环境；R420 ，RHEL6.3 X64测试通过
5.  OMSA7.4.1下载：[http://downloads.dell.com/FOLDER02645210M/1/OM-SrvAdmin-Dell-Web-LX-7.4.1-1341.RHEL6.x86_64_A00.tar.gz](http://downloads.dell.com/FOLDER02645210M/1/OM-SrvAdmin-Dell-Web-LX-7.4.1-1341.RHEL6.x86_64_A00.tar.gz)
6.  另外需要准备RHEL6.3 X63光盘，附件为客户端使用的脚本
7.  OMSA服务端配置：
    1.  临时关闭防火墙：service iptables stop

![Machine generated alternative text: Hll92.l6B.B8.l5O:22 jj XsheU 5 (Build 0497) Copyright (c) 2002-2014 NetSarang Computer, Inc. All rights reserved. Type 慼elp? to learn how to use Xshell prompt. \[c:\\-\]\$ ssh 192.168.88.150 Connecting to 192.168.88.150:22\... Connection established. To escape to local shell, press 慍trl+Alt+\]? \[root@localhost 梋#\[service iptables stop iptables: Flushing firewall rules: \[ 0K \] iptables: Setting chains to policy ACCEPT: filter \[ 0K \] iptables: Unloading modules: \[ 0K \] \[root@localhost 梋# ?](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_001.jpg)

1.  df -h查看光驱挂载情况， 切换到光驱挂载目录（如这里是/dvd），切换到系统光盘的目录Packages安装相关RPM包，cd /dvd/Packages/
    1.  ![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_002.png]]
    2.  安装FTP客户端：rpm -ivh ftp-0.17-51.1.el6.x86_64.rpm 

![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_003.jpg]]

1.  安装FTP VSFTPD服务端：rpm -ivh  vsftpd-2.2.2-11.el6.x86_64.rpm 

![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_004.jpg]]

1.  安装createrepo的依赖包：rpm -ivh  deltarpm-3.5-0.5.20090913git.el6.x86_64.rpm 
    1.  ![Machine generated alternative text: \[root@localhost Packages\]# warning: deltarpm-3.5-0.5.20090J13git.el6.\_\_b4.rpm: Header V3 RSA/S..6 Signature, key ID fd431d51: NOKEY P rep a rin g\... \########################################### \[100%\] 1: del t a r p m \########################################### \[10 0%\]](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_005.jpg)
    2.  安装另一个依赖包：rpm -ivh  python-deltarpm-3.5-0.5.20090913git.el6.x86_64.rpm 

![Machine generated alternative text: \[root@local.host Packages\]# warning: python-deltarpm-3.5-O.5.20090913git.el..6.x86_64. rpm: Header V3 RSA/SHA256 Signature, key ID fd4 31d51: NOKEY P rep a rin g\... \########################################### \[100%\] 1: p y t h on - de 1 t a r p m \########################################### \[100%\]](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_006.jpg)

1.  安装createrepo：rpm -ivh createrepo-0.9.8-5.el6.noarch.rpm 

![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_007.jpg]]

1.  启动VSFTPD服务并设置开机启动：chkconfig vsftpd on和service vsftpd start

![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_008.jpg]]

1.  设置操作系统文件和OMSA存放位置
    1.  创建操作系统目录：mkdir -p  /var/ftp/pub/os
    2.  创建OMSA目录：mkdir -p /var/ftp/pub/omsa
    3.  解压OMSA文件：tar xvf OM-SrvAdmin-Dell-Web-LX-7.4.0-866.RHEL6.x86_64_A00.tar.gz -C /var/ftp/pub/omsa

![Machine generated alternative text: \[root@iocaihost 梋# cd /mnt/hgts/C/36Odowntoad/ \[root@iocaihost 36Odownioad\]# mkdir -p /var/ftp/pub/omsa \[root@iocaihost 36Odownioad\]# is OM-SrvAdmin-Deii-Web-LX-7.4.O-866.RHEL6.x86_64_AOO.tar.gz OM-SrvAdmin -Dell-Web- LX-7 .4.0-866. RHEL6 . x86_64_A00 . tar. gz \[root@iocaihost 36Odownioad\]# doc s? iinux/ iinux/RPMS/ iinux/RPMS/suppo rtRPMS/ iinux/RPMS/suppo rtRPMS/open sou rce - component si iinux/RPMS/suppo rtRPMS/open sou rce - component s/RHEL6i](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_009.jpg)

1.  复制系统光盘中的所有文件到目录：（如下图/dvd表示系统光盘的挂载点）cp  -R  /dvd/\*   /var/ftp/pub/os

![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_010.jpg]]

1.  配置YUM
    1.  切换到目录：cd /etc/yum.repos.d，接着新建一个sa.repo文件或修改现有文件为sa.repo，如下为修改：mv   rhel-source.repo   sa.repo。注意：该目录底下不能同时存在2个\*.repo文件，否则可能出错

![Machine generated alternative text: \[root@local.host 36Odownload\]# I \[root@1.ocal.host yum.repos.d\]# l.s rhel.-source. repo \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ \[root@local.host yum. repos.d\]#Lmv rnei-source.repo sa. repo \[root@1.ocal..host yum.repos.d\]# Is sa. repo \[root@Iocal.host yum.repos.d\]# ?](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_011.jpg)

1.  修改sa.repo文件，然后保存退出：
    1.  vim  sa.repo
    2.  具体内容如下：

![Machine generated alternative text: \[OS\] name=RHEL6.3 x64 baseu r1=f ile :///var/ftp/pub/os/Serve rl enabL.ed=1 gpgchec k=O \[OMSA\] name=OMSA baseu rl..=f ile :1//va r/ftp/publomsa/linuxl enabled=1 gpgchec k=O](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_012.jpg)

1.  创建OMSA依赖关系
    1.  命令：createrepo -v  /var/ftp/pub/omsa/linux/

![Machine generated alternative text: \[root@localhost yum. repos.d\]# 1/127 - custom/RHEL6/Server-Instrumentation/x86_64/srvadmin-deng-snmp-7.4.O-4.141.el.6.x86_64. rpm 2/127 - custom/RHEL6/Server-Instrumentation/x86_64/srvadmin-omcommon-7.4O-497.1.e16.x86_64. rpm 3/127 - custom/RHEL6/Server-Instrumentation/x86_64/smbios-utfls-bin-2.2.27-4.12.1.e16.x86_64. rpm 4/127 - custom/RHEL6/Server-Instrumentation/x86_64/srvadmin-oslog-7.4.O-4.100.1.e16.x86_64. rpm 5/127 - custom/RHEL6/Server-Instrumentation/x86_64/libsmbios-2.2.27-4.12.1.et6.x86_64. rpm 6/127 - custom/RHEL6/Server-Instrumentation/x86_64/srvadmin-smcommon-7.4.O-4.152.2.et6.x86_64. rpm](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_013.jpg)

1.  安装服务端的OMSA:
    1.  命令: yum -y install \*srvadmin\*

![Machine generated alternative text: \[root@localhost yum. repos.d\]#\[ \_\_\_\_\_\_\_\_ Loaded plugins: product-id, reti h-packag. Updating certificate-based repositories Unable to read consumer identity OS OS/primary_db Setting up InstaU Process Resolving Dependencies - -\> Running transaction check Kit, security, subscription-manager I 4.0 kB 00:00 I 3.1 MB 00:00 \-\--\> Package srvadmin-all.x86_64 0:7.4.0-4.l.l.el6 will be installed \-\--\> Package srvadmin-argtable2.x86_64 0:7.4.0-4.2.1.el6 will be installed - \--\> Package srvadmin-base.x86_64 0:7.4.0-4.1.1.el6 will be installed \-\--\> Package srvadmin-cm.x86_64 0:7.4.0-4.1.115.el6 will be installed \--\> Processing Dependency: smbios-utils-bin for package: srvadmin-cm-7.4.0-4.1.115.el6.x86_64 - - -\> Package srvadmin-deng.x86_64 0:7.4.0-4.14.1.e16 will be installed - - -\> Package srvadmin-deng-snmp.x86_64 will be installed - - -\> Package srvadmin-hapi.x86_64 0:7.4.0-4.28.2.e16 will be installed](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_014.jpg)

1.  配置客户端脚本sa.sh
    1.  切换目录到/var/ftp，命令：cd   /var/ftp，编辑客户端脚本：vim sa.sh

![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_015.jpg]]

1.  在文件sa.sh中输入如下内容，保存退出。（注意：这里的192.168.88.150为服务端的IP）

![Machine generated alternative text: #!Ibinlsh service iptables stop mv Ietclyum.repos.dI\* echo 揕OI. ? 籌etclyum.repos.dls鈘epo echo ? ama- 籌etc/yum.repos.dlsa.repo echo u r ? 籌etclyum.repos.dlsa..repo echo 籌etc/yum.repos.dlsa.repo echo ? eJ ? ?etc/yum.repos.d/sa.repo echo 摀 ?etc/yum. repos . d/sa. repo echo ? ? 籌etclyum.repos.dlsa.repo echo ? ? ?etc/yum.repos.d/sa.repo echo ? jr ? ?etc!yum. repos.d/sa. repo echo ? ?etc/yum.repos.d/sa.repo echo ? ?etc/yum.repos.d/sa.repo I yum install ftp yum install \*srvadmin\* /opt/dell/s rvadmin/sbin/s rvadmin-services .sh enable yum remove ftp echo ? 1 minute \] \\O33\[39;L . ? s hut down](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_016.jpg)

1.  在客户端上安装OMSA：
    1.  暂停防火墙：service  iptables stop
    2.  从服务端下载安装脚本：wget    [ftp://192.168.88.150/sa.sh](ftp://192.168.88.150/sa.sh)     （注意：这里的192.168.88.150为服务端的IP）

![Machine generated alternative text: ? 2J?192.168.88.149:22 ? \[root@tocalhost /\]# service 靝tabl..es stop \[root@tocalhost /\]# \--2015-03-17 07:34:29\-- ftp://192.168.88.150/sa.sh =\> ? sa.sh? Connecting to 192.168.88.150:21\... connected. Logging in as anonymous \... Logged in! ==\> SYST . .. done. ==\> PWD . .. done. ==\> TYPE I \... done. ==\> CWD not needed. ==\> SIZE sa.sh \... 721 ==\> PASV \... done. ==\> RETR sa.sh \... done. Length: 721 (unauthoritative) 100% \[=============================================================\>\] 72 1 . - in Os 2015-03-17 07:34:29 (85.6 MB/s) - sa.sh? saved \[721\]](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_017.jpg)

1.  运行脚本进行安装：sh  sa.sh，

![Machine generated alternative text: \[root@localhost /\]# mv: cannot stat 慖etclyum.repos.dI\*? No such file or directory Loaded plugins: product-id, refresh-packagekit, security, subscription-manager Updating certificate-based repositories. Unable to read consumer identity om 1.3 kB 00:00 os I 4.0 kB 00:00 Setting up Install Process Resolving Dependencies - -\> Running transaction check \-\--\> Package ftp.x86_64 0:0.17-51.1.e16 will be installed - -\> Finished Dependency Resolution Dependencies Resolved](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_018.jpg)

1.  客户端安装了OMSA后会自动卸载ftp客户端并在1分钟后自动重启。

![Machine generated alternative text: Downloading Packages: Running rpm_check_debug Running Transaction Test Transaction Test Succeeded Running Transaction Erasing : ftp-0.17-5l.l.e16.x86_64 Installed products updated. Verifying : ftp-0.17-51. 1.e16.x86_64 Removed: ftp.x86_64 0:0.17-51.1.e16 Complete! \[The server is going down for reboot in 1 minute\....\] Broadcast message from root@localhost.localdomain (/dev/pts/1) at 3:45 \... The system is going down for reboot in 1 minute! ?](attachments/Technology_ALL_案例分析%5B重要%5D_027_Shared_%20快速部署OMSA_019.png)

 

 

Sean_wang

Enterprise Product Engineer

Dell \| Enterprise Support Services

How am I doing? Email my manager (<Richa_Zeng@dell.com>) with any feedback.

[Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \|一款软件插件，可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣零部件

回复邮件获取详细资料或点击 SupportAssist超链接了解更多信息！！

戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_020.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_021.gif]]](http://www.weibo.com/techsupportdell)   [![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_022.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[Technology_ALL_案例分析[重要]_027_Shared_ 快速部署OMSA_023.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

已使用 OneNote 创建。
