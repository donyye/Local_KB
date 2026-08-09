SUSE 集群配置环境搭建

2024年4月2日

11:09

yum 原设置

ISO ：SLE-15-SP2-Full-x86_64-GM-Media1.iso

\# zypper install createrepo\*

\# zypper in vsftpd

\# vim /etc/vsftpd.conf

\# mount /dev/sr0 /mnt/

\# cp -ivr /mnt/ /srv/ftp/yum

\# zypper addrepo /srv/ftp/yum myzpp

\# cd /srv/ftp/yum/

\# rm -rf repodata/

\# createrepo -p -d -o /srv/ftp/yum/ /srv/ftp/yum/

\# zypper clean

\# zypper refresh myzpp

 

Node1

![[SUSE cluster_002_SUSE 集群配置环境搭建_001.png]]

 

Node2

![[SUSE cluster_002_SUSE 集群配置环境搭建_002.png]]

 

 

已使用 OneNote 创建。
