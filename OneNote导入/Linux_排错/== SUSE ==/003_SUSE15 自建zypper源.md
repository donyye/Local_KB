SUSE15 自建zypper源

2024年7月5日

11:51

最简单的方法，安装完SUSE 15后，在系统里会看到下面zypper信息。

![[__ SUSE ___003_SUSE15 自建zypper源_001.png]]

 

通过yast 命令把第一个激活。

![[__ SUSE ___003_SUSE15 自建zypper源_002.png]]

 

![[__ SUSE ___003_SUSE15 自建zypper源_003.png]]

 

![[__ SUSE ___003_SUSE15 自建zypper源_004.png]]

这样就可以开始安装软件

 

=======自建整个源========

1. 先挂载ISO到系统里，拷贝iso里的多有软件到一个目录里。

如把所有文件都拷贝到 /var/yum 目录里

 

2. 安装 createrepo 命令

\# zypper install createrepo\*

注意：此命令有很多依赖包关系，单独rpm方式安装会很yas麻烦。

 

3. 添加源

\# zypper addrepo /var/yum myzpp

![[__ SUSE ___003_SUSE15 自建zypper源_005.png]]

 

4 创建repodata

在使用命令之前需要删除之前的 repodata

\# createrepo -p -d -o /var/yum/ /var/yum/

![[__ SUSE ___003_SUSE15 自建zypper源_006.png]]

 

5. 清楚缓存和刷新一下

\# zypper clean

\# zypper refresh myzpp

 

测试：

\# zypper install libncurses5\*

 

搭建FTP share 源给到其它的SUSE系统使用

1. 安装vsftpd

如果需要安装vsftpd需要吧所有默认的源都激活。

\# zypper install vsftpd

 

2. 把iso的所有数据都拷贝或迁移到ftp的主目录下 "/srv/ftp"

如：

![[__ SUSE ___003_SUSE15 自建zypper源_007.png]]

ftp 不需要修改配置文件，默认就可以。另外开启 ftp 服务，systemctl start vsftpd

 

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

\# zypper clean

\# zypper refresh myzpp

![[__ SUSE ___003_SUSE15 自建zypper源_008.png]]

完成后就可以安装了。

 

==== 配置 zypper 通过 apache 共享 ====

 

localhost:\~ \# zypper install apache2

 

localhost:\~ \#  systemctl start apache2.service

localhost:\~ \#  systemctl enable apache2.service

 

/srv/www/htdocs/ 是apache 主目录

localhost:\~ \# zypper addrepo /srv/www/htdocs/SUSE15SP3 myzpper

Adding repository \'myzpper\' \...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\....\[done\]

Repository \'myzpper\' successfully added

 

URI         : dir:/srv/www/htdocs/SUSE15SP3

Enabled     : Yes

GPG Check   : Yes

Autorefresh : No

Priority    : 99 (default priority)

 

Repository priorities are without effect. All enabled repositories share the same priority.

 

localhost:\~ \# createrepo -p -d -o /srv/www/htdocs/SUSE15SP3 /srv/www/htdocs/SUSE15SP3

Directory walk started

Directory walk done - 12977 packages

Temporary output repo path: /srv/www/htdocs/SUSE15SP3/.repodata/

Preparing sqlite DBs

Pool started (with 5 workers)

Pool finished

 

localhost:\~ \# zypper clean 

All repositories have been cleaned up.

 

localhost:\~ \# zypper refresh myzpper

Repository \'myzpper\' is up to date.                                                                                                                                                                                    

Building repository \'myzpper\' cache \...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\...\.....\[done\]

Specified repositories have been refreshed.

 

localhost:\~ \# cat /etc/zypp/repos.d/myzpper.repo

\[myzpper\]

enabled=1

autorefresh=1

baseurl=http://10.10.41.21/SUSE15SP3

keeppackages=0

 

====完成====

 

 

 

已使用 OneNote 创建。
