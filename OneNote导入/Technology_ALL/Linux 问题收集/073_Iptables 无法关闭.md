Iptables 无法关闭

2021年9月17日

13:25

issue：

客户环境是RHEL6.10。发现Iptables 关闭后，并禁止开机启动但是系统重启后iptables还是会启动起来。

![[Technology_ALL_Linux 问题收集_073_Iptables 无法关闭_001.png]]

 

Troubleshooting：

一、基本的检查\
1. 使用下面命令清空与禁止 iptables 启动。

chkconfig iptables off

iptables -X

iptables -F

iptables -Z

iptables -t net -F

service iptables save

service iptables stop

2. 检查 rc.local 与 /etc/init.d/iptables[  里是有什么启动项。]（没发现）

3. 尝试重新iptables服务，没有发现iptables里的策略有出现。

4.重启系统

系统重启后检查还是与之前的一样， iptables  有策略，而且服务是启动的。

 

二、继续检查

通过检查message日志，追踪这个iptables上的ip的作用。

1.发现客户有安装OME。

2\. iptables里的策略iptables地址是idrac的ip地址。

3. 发现停止OME的服务模块"dcismeng"后问题能解决。重启系统后iptables服务不会再启动。

所以这里确定此问题与OME有关系，但是客户需要这个服务来获取机器的状态，所以不同意关闭此服务。

 

三、继续深入检查

1\. 客户有一台好的机器。使用两个机器做对比。

2.发现有一个Enable-iDRACAccessHostRoute的服务，在没问题的机器上是没有开启的。

#cd /opt/dell/srvadmin/iSM/bin/

#./Enable-iDRACAccessHostRoute get-status

#./Enable-iDRACAccessHostRoute 0

![[Technology_ALL_Linux 问题收集_073_Iptables 无法关闭_002.png]]

通过这个方式可以关闭这个服务。

3.关闭这个服务问题解决。

 

总结：

被关闭的这个服务是iSM 中的 iSM 端口 1265 当前启用了通过主机操作系统功能进行的 iDRAC 访问。

这个iptables其实就是一个让主机能访问idrac 的重定向，这是一个功能。

它被关闭后就不会再往开启iptables和往里面写规则。

 

已使用 OneNote 创建。
