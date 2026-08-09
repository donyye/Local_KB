Share: blocked FC remote port time out: removing rport

2018年2月12日

9:44

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Share: blocked FC remote port time out: removing rport
  发件人     Han, Ruyang
  收件人     CN XMN TS ENT L2 SME
  抄送       Wang, Xing Fang; Dang, Hongtao
  发送时间   2018年2月10日 22:03
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

 

 

Hi Team

 

开脑洞解决了一个Case分享一下：

 

故障现象：

Linux /var/log/message下报如下FC HBA相关错误：

            Feb  7 13:50:08 localhost kernel: rport-2:0-2: blocked FC remote port time out: removing rport

            Feb  7 13:50:08 localhost kernel: rport-1:0-2: blocked FC remote port time out: removing rport

            Feb  7 13:53:39 localhost kernel: rport-2:0-3: blocked FC remote port time out: removing rport

            Feb  7 13:53:39 localhost kernel: rport-1:0-3: blocked FC remote port time out: removing rport

            Feb  7 13:58:50 localhost kernel: rport-2:0-4: blocked FC remote port time out: removing rport

            Feb  7 13:58:50 localhost kernel: rport-1:0-4: blocked FC remote port time out: removing rport

 

 

Redhat官方KB解释开机时产生的是正常现象可以忽略，其它情况产生的则不正常：

[https://access.redhat.com/solutions/65596](https://access.redhat.com/solutions/65596)

 

![[Technology_ALL_Linux 问题收集_035_Share_ blocked FC remote port time out_ r_001.jpg]]

 

此KB没有提到的一种现象是（HBA: Emulex LPE1205-M）：

当服务器开关机时，会影响网络中的其它服务器使其产生同样的报错。如果光纤交换机没有做Zone，或者做的不是Single Zone，那么服务器之间没有隔离，就会互相影响产生这个报错。

 

说服客户在光纤交换机上配置了Single Zone后问题解决。

 

 

Best Regards

Ruyang Han

 

 

已使用 OneNote 创建。
