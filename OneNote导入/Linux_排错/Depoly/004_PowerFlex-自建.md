PowerFlex-自建

2024年2月28日

11:34

 

安装包下载：\
[https://www.dell.com/support/home/en-ie/product-support/product/scaleio-ready-node\--poweredge-14g/drivers](https://www.dell.com/support/home/en-ie/product-support/product/scaleio-ready-node--poweredge-14g/drivers)\
 

\[root@localhost \~\]# yum groups install \"Development Tools\"

 

需要 Java 包的支持

\[root@localhost \~\]# yum install jre

 

\[root@localhost \~\]# GATEWAY_ADMIN_PASSWORD=P@ssw0rd! rpm -i EMC-ScaleIO-gateway-3.6-2000.117.x86_64.rpm

validating java version

/bin/java

found the Java executable in PATH

version 8

java platform that was found : OpenJDK 64-Bit Server VM (build 25.402-b06, mixed mode)

Running pre installation verifications

/bin/keytool

found the keytool executable in PATH

found suffice RAM memory : 3880512

Running post install operations (rpm,install)

Opening the following TCP/UDP ports (Red Hat 7):

    TCP 443

    TCP 80

firewalld daemon is not running, ports will not be updated.

/bin/java

copy lockbox file to java (  ,/bin)

the target lockbox directory /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.402.b06-1.el7_9.x86_64/jre/bin .

copy lockbox jars to existing java lib/ext

/bin/keytool

certificate subject: CN=localhost.localdomain, OU=ASD, O=EMC, L=Hopkinton, ST=Massachusetts, C=US

 

Warning:

The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using \"keytool -importkeystore -srckeystore /opt/emc/scaleio/gateway/conf/certificates/.keystore -destkeystore /opt/emc/scaleio/gateway/conf/certificates/.keystore -deststoretype pkcs12\".

Certificate stored in file \</opt/emc/scaleio/gateway/temp/certificates/ScaleIO.cer\>

 

Warning:

The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using \"keytool -importkeystore -srckeystore /opt/emc/scaleio/gateway/conf/certificates/.keystore -destkeystore /opt/emc/scaleio/gateway/conf/certificates/.keystore -deststoretype pkcs12\".

truststore

Installing the web server service/process to run as root(default).

To run as a different user, define the variable GATEWAY_RUN_USER.

 

upgrading gateway - adjusting catalina.properties accordingly

Running post-run install operations

Opening the following TCP/UDP ports (Red Hat 7):

    TCP 443

    TCP 80

firewalld daemon is not running, ports will not be updated.

managing service via systemd

Creating service file /etc/systemd/system/scaleio-gateway.service

Creating service file /etc/init.d/scaleio-gateway

updating service via chkconfig

Created symlink from /etc/systemd/system/multi-user.target.wants/scaleio-gateway.service to /etc/systemd/system/scaleio-gateway.service.

The EMC ScaleIO Gateway is running. PID=16795.

\[root@localhost \~\]#

 

\[root@localhost \~\]# rpm -qa \|grep -i emc

EMC-ScaleIO-gateway-3.6-2000.117.x86_64

 

 

![[Depoly_004_PowerFlex-自建_001.png]]

 

 

![[Depoly_004_PowerFlex-自建_002.png]]

 

导入配置文件，点击upload installation CSV

![[Depoly_004_PowerFlex-自建_003.png]]

 

文件内容：

![[Depoly_004_PowerFlex-自建_004.png]]

 

 

 

![[Depoly_004_PowerFlex-自建_005.png]]

 

![[Depoly_004_PowerFlex-自建_006.png]]

 

 

![[Depoly_004_PowerFlex-自建_007.png]]

 

 

![[Depoly_004_PowerFlex-自建_008.png]]

 

 

![[Depoly_004_PowerFlex-自建_009.png]]

 

![[Depoly_004_PowerFlex-自建_010.png]]

 

 

![[Depoly_004_PowerFlex-自建_011.png]]

 

 

 

\[root@localhost \~\]# scli \--login \--username admin \--password P@ssw0rd!

Logged in. User role is SuperUser. System ID is 60dda64f1574400f

\[root@localhost \~\]#

\[root@localhost \~\]#

\[root@localhost \~\]#

\[root@localhost \~\]# scli \--query_cluster

Cluster:

    Mode: 3_node, State: Normal, Active: 3/3, Replicas: 2/2

    SDC refresh with MDM IP addresses: disabled

    Virtual IP Addresses: N/A

Master MDM:

    ID: 0x7cbb59a95d6dcb00

        IP Addresses: 10.10.40.111, Management IP Addresses: 10.10.40.111, Port: 9011, Virtual IP interfaces: N/A

        Status: Normal, Version: 3.6.2000

Slave MDMs:

    ID: 0x5da2231a3f997601

        IP Addresses: 10.10.40.112, Management IP Addresses: 10.10.40.112, Port: 9011, Virtual IP interfaces: N/A

        Status: Normal, Version: 3.6.2000

Tie-Breakers:

    ID: 0x77902c567cd74302

        IP Addresses: 10.10.40.113, Port: 9011

        Status: Normal, Version: 3.6.2000

 

 

 

Node2:

安装前需要先安装 java - 11 ，否则安装完成后 mgmt-server 服务会自动stop。

\[root@localhost \~\]# yum install java-11-openjdk -y

\[root@localhost \~\]# rpm -ivh EMC-ScaleIO-mgmt-server-3.6-2000.104.noarch.rpm

![[Depoly_004_PowerFlex-自建_012.png]]

 

服务进程是[  mgmt-server.service ]，端口是8443

 

![[Depoly_004_PowerFlex-自建_013.png]]

 

[https://10.10.40.112:8443](https://10.10.40.112:8443)

登录后需要输入 MDM的机器

10.10.40.111

![[Depoly_004_PowerFlex-自建_014.png]]

 

测试版本 ，使用85天

![[Depoly_004_PowerFlex-自建_015.png]]

 

 

 

 

 

已使用 OneNote 创建。
