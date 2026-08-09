SUSE-ZYPPER

2024年5月24日

9:40

SUSE15 自建zypper源

最简单的方法，安装完SUSE 15后，在系统里会看到下面zypper信息。

![[SUSE cluster_001_SUSE-ZYPPER_001.png]]

 

通过yast 命令把第一个激活。

![[SUSE cluster_001_SUSE-ZYPPER_002.png]]

 

![[SUSE cluster_001_SUSE-ZYPPER_003.png]]

 

![[SUSE cluster_001_SUSE-ZYPPER_004.png]]

这样就可以开始安装软件

 

=======自建整个源========

1. 先挂载ISO到系统里，拷贝iso里的多有软件到一个目录里。

如把所有文件都拷贝到 /var/yum 目录里

2. 安装 createrepo 命令

\# zypper install createrepo\*

注意：此命令有很多依赖包关系，单独rpm方式安装会很麻烦。

3. 添加源

\# zypper addrepo /var/yum myzpp

![[SUSE cluster_001_SUSE-ZYPPER_005.png]]

 

4 创建repodata

在使用命令之前需要删除之前的 repodata

\# createrepo -p -d -o /var/yum/ /var/yum/

![[SUSE cluster_001_SUSE-ZYPPER_006.png]]

 

5. 清楚缓存和刷新一下

\# zypper clean

\# zypper refresh myzpp

测试：

\# zypper install libncurses5\*

搭建FTP share 源给到其它的SUSE系统使用

1. 安装vsftpd

如果需要安装vsftpd需要吧所有默认的源都激活。

\# zypper install vsftpd

 

2. 把iso的所有数据都拷贝或迁移到ftp的主目录下 "/srv/ftp"

如：

![[SUSE cluster_001_SUSE-ZYPPER_007.png]]

ftp 不需要修改配置文件，默认就可以。另外开启 ftp 服务，systemctl start vsftpd

 

3. 然后把repo的配置文件拷贝到需要源的其它SUSE系统上

如下面是其它SUSE系统：

s2:\~ \# cat /etc/zypp/repos.d/myzpp.repo 

\[myzpp\]

name=myzpp

enabled=1

autorefresh=1

baseurl=ftp://10.10.40.61/yum    \--》只要修改这里就好

type=rpm-md

keeppackages=0

 

4. 清理一下zpper的缓存

\# zypper clean

\# zypper refresh myzpp

![[SUSE cluster_001_SUSE-ZYPPER_008.png]]

完成后就可以安装了。

 

已使用 OneNote 创建。
