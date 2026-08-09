Day-3 Backup Exec

Wednesday, November 20, 2013

Netbackup (Solaris)

主要针对Unix , Windows , Enterprise marketing

 

Backup Exec (Windows)

1.备份软件分服务器端和客户端:

BE服务器端只支持Windows , Linux只支持客户端.

2.针对Midrange marketing

 

\*Server free: 服务器提交一个备份任务的时候,磁盘柜直接备份到磁带库;而不经过服务器.

使服务器的资源得到更有效的利用.

 

3.SAN share[   - ]共享使用备份设备

 

BE installing:

Installation Program Components:

-SQL Server 2000 core components

-MDAC v2.62

-ODBC 3.0

 

Tapeinst.exe - Device Driver Install Utility[  \<\-\-\--]要用BE自己的设备驱动,不要用厂商的设备驱动

 

NAS - NDMP

 

BE客户端远程端口默认为10000

注意:

1.BE默认会有60天的试用期,试用期结束后会停止需要License的服务【如:带机机械臂管理】

2.如果客户本身有SQL Server,可以不装软件自带的Express;需要选择自定义安装里面会有现有SQL Server的选项.

3.安装过程中会有Windows窗口弹出警告,提示是否要安装驱动(磁带机),原则上都要安装Symantec自身的驱动

4.装了备份软件之后，需要更换磁带的时候，不要再到机器旁或磁带管理界面去弹出来了。

可以使用备份软件里面自带的Export；放磁带也是一样，用备份软件自带的Import。

 

为什么连不上客户端?

1.检查服务是否有开启（RAWS）

2.检查端口是否有开启（netstat -a ; 10000）

 

编目录(Catalog Media):

磁带从不同带机之间的转移,需要读取的时候

Label Media:

类似快速的格式化磁带

定义介质集(Media Set Properties):

定义特定用途的磁带组【例如某一台服务器只允许备份到介质集里的磁带】

 

两种日志方法：

1.  获取日志

C:\\Program Files\\Sysmantec\\Backup Exec\>bediag /all /o:bediag.txt[      \-\--\>]将所有的日志放到bediag.txt(默认原目录下)。

1.  C:\\Program Files\\Sysmantec\\Backup Exec\\SGMON

 

1.建议先加入域之后再安装BackupExec

2.如果要备份远程的数据，需要在远程客户端装Remote Agent。

3.如果要备份SQL/Exchange，需要安装SQL/Exchange option。

 

iSCSI

1.RDP  to 10.206.210.30   -\>  <https://10.206.210.43/cloud/org/glndccc/>  (FireFox)

2.Create two VC:

Student14A

NIC1:192.168.200.148  =\> DC

NIC2:192.168.60.148    =\> TBU iSCSI

Student14B

NIC1:

NIC2:

 

遇到问题

1.没有找到Drive,Robotic[  -]可以尝试将服务重启下-\>Backup Exec Device & Media Service

 

 

![[Technology_ALL_Storage_004_Day-3 Backup Exec_001.png]]

 

![[Technology_ALL_Storage_004_Day-3 Backup Exec_002.png]]

 

![[Technology_ALL_Storage_004_Day-3 Backup Exec_003.png]]

 

已使用 OneNote 创建。
