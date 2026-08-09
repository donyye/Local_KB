SUSE ssh 登录问题

2024年9月5日

9:29

 

1\. /etc/ssh/sshd_config

 

文件中，将#PasswordAuthentication no的注释去掉，并将no修改为yes

 

将#PermitRootLogin yes的注释去掉

 

2.检查防火墙是否启动，是否有过滤ssh，

 

简单的方法是关闭防火墙

 

/etc/init.d/Susefirewall2_init stop

/etc/init.d/Susefirewall2_setup stop

\-\-\-\-\-\-\-\--

chkconfig SuSEfirewall2_init off

chkconfig SuSEfirewall2_setup off

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

SUSE 12 关闭方法：

systemctl stop SuSEfirewall2_init.service

systemctl stop SuSEfirewall2_setup.service

\-\-\-- 关闭自动启动 \-\-\--

 systemctl disable SuSEfirewall2_init.service 

 systemctl disable SuSEfirewall2_setup.service

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

SUSE15 关闭防火墙：\
\# systemctl stop firewalld.service

\# systemctl disable firewalld.service\
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

完成上述修改后，重启ssh

 

/etc/init.d/sshd restart

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

systemctl restart sshd

 

检查ssh状态

 

/etc/init.d/sshd status 为active即可

service SuSEfirewall2_setup start

service SuSEfirewall2_init  start

怎么让某些服务或者端口可以通过防火墙正常使用呢？

方法就是修改配置文件：/etc/sysconfig/SuSEfirewall2。

如下以TCP服务为例开放服务或者端口号，使用配置 FW_SERVICES_EXT_TCP，它的值可以等于：/etc/services中的服务名，端口号，或者端口号范围，或者混合，使用空格分隔。

示例1：开放ssh服务

FW_SERVICES_EXT_TCP=\"ssh\"

示例2：开放123 和 514端口

FW_SERVICES_EXT_TCP=\"123 514\"

示例3：开放3200到3299之间的100个端口

FW_SERVICES_EXT_TCP=\"3200:3299\"

示例4：开放ftp服务，22端口，telnet服务，512到514这三个端口

FW_SERVICES_EXT_TCP=\"ftp 22 telnet 512:514\"

=======================================

SSH key 登录配置

node1:

PasswordAuthentication yes

#支持密码验证

PermitRootLogin yes

#允许root登录

 

 # ssh-keygen -t rsa  \--》默认回车

# cat id_rsa.pub \>\> authorized_keys

 

 

\# ssh-copy-id -i root@node2

\#### 要验证

 

已使用 OneNote 创建。
