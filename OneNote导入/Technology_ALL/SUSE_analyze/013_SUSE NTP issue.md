SUSE NTP issue

2018年1月18日

15:56

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       FW: 戴爾技術支援 \-- B6RP4J2 (SR:959391154 )
  发件人     Huang, Antti
  收件人     Ye, Dony
  发送时间   2018年1月18日 15:45
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Sorry, 忘了把你copy进去.  FYI.

 

Regards,

 

Antti Huang

Software & Solutions Master Engineer

APJ Solutions Support Team (SST)

RHCE7 \| RHCE Openstack \| SAP HANA Specialist \|

CCAH Hadoop \| MCSE \| VCP \| ITIL \| JNCIA

Dell EMC \| Global Support & Deployment

 

From: Huang, Antti

Sent: Thursday, January 18, 2018 15:45

To: \'ivan.chang@kellysgroup.com\' \<ivan.chang@kellysgroup.com\>

Cc: Lu, Eason \<Eason_Lu@DELL.com\>; Chen, Jessia \<Jessia_Chen@Dell.com\>; Shih, Matt \<Matt_Shih@Dell.com\>; Lin, Sybil \<Sybil_Lin@Dell.com\>

Subject: RE: 戴爾技術支援 \-- B6RP4J2 (SR:959391154 )

 

您好, 张先生,

 

从webex里, 我们换成其他的time source后, 时间同步就正常了. 所以可以确认是此suse连接您的windows time source之间有不兼容的问题.

我们曾经也遇见过类似的情况, 以下两个KB仅供参考. 谢谢.

 

[https://access.redhat.com/solutions/23365](https://access.redhat.com/solutions/23365)

[https://kb.netapp.com/support/s/article/how-to-determine-if-a-windows-domain-controller-is-a-suitable-ntp-server-for-data-ontap-8?language=en_US](https://kb.netapp.com/support/s/article/how-to-determine-if-a-windows-domain-controller-is-a-suitable-ntp-server-for-data-ontap-8?language=en_US)

 

现在我们已经帮您将两台hana同步到外面的时间, 两台server现在已为同步状态.

 

sap-hprd2:\~ \# ntpq -pn

     remote           refid      st t when poll reach   delay   offset  jitter

==============================================================================

192.168.100.19  .LOCL.           1 u    3   64  377    0.255  -302.79 256.992

sap-hprd2:\~ \# ntpq

ntpq\> associations

 

ind assid status  conf reach auth condition  last_event cnt

===========================================================

  1 63331  9024   yes   yes  none    reject   reachable  2

ntpq\> rv 63331

associd=63331 status=9024 conf, reach, sel_reject, 2 events, reachable,

srcadr=tphq-dc02.kelly.com.tw, srcport=123, dstadr=192.168.100.84,

dstport=123, leap=00, stratum=1, precision=-6, rootdelay=0.000,

rootdisp=10092.041, refid=LOCL,

reftime=de0aaca8.db645a1c  Thu, Jan 18 2018 13:18:32.857,

rec=de0ac674.08f0f482  Thu, Jan 18 2018 15:08:36.034, reach=377,

unreach=0, hmode=3, pmode=4, hpoll=6, ppoll=6, headway=30,

flash=400 peer_dist, keyid=0, offset=-302.799, delay=0.255,

dispersion=16.504, jitter=256.992, xleave=0.014,

filtdelay=     0.26    0.34    0.29    0.24    0.33    0.29    0.31    0.32,

filtoffset= -302.80 -302.84 -302.82 -302.80   37.16   37.18   37.17   37.17,

filtdisp=     15.63   16.59   17.59   18.57   19.53   20.50   21.51   22.48

ntpq\> exit

sap-hprd2:\~ \# ntpq -pn

     remote           refid      st t when poll reach   delay   offset  jitter

==============================================================================

192.168.100.19  .LOCL.           1 u   34   64  377    0.330  -240.33  17.696

sap-hprd2:\~ \# ntpdate 192.168.100.19

18 Jan 15:28:55 ntpdate\[16030\]: the NTP socket is in use, exiting

sap-hprd2:\~ \# systemctl stop ntpd

sap-hprd2:\~ \# ntpdate 192.168.100.19

18 Jan 15:29:24 ntpdate\[16036\]: adjust time server 192.168.100.19 offset -0.2383                                                                                        95 sec

sap-hprd2:\~ \# systemctl start ntpd

sap-hprd2:\~ \# vi /etc/ntp.conf

sap-hprd2:\~ \# cat /etc/ntp.conf

#server 192.168.100.19

server time.stdtime.gov.tw

\# key (6) for accessing server variables

sap-hprd2:\~ \# systemctl restart ntpd

sap-hprd2:\~ \# ntpq -pn

     remote           refid      st t when poll reach   delay   offset  jitter

==============================================================================

\*118.163.81.61   192.168.0.3      2 u    3   64    1    3.843  -82.988   0.000

sap-hprd2:\~ \#

 

 

Regards,

 

Antti Huang

Software & Solutions Master Engineer

APJ Solutions Support Team (SST)

RHCE7 \| RHCE Openstack \| SAP HANA Specialist \|

CCAH Hadoop \| MCSE \| VCP \| ITIL \| JNCIA

Dell EMC \| Global Support & Deployment

 

From: Huang, Antti

Sent: Thursday, January 18, 2018 14:29

To: \'ivan.chang@kellysgroup.com\' \<<ivan.chang@kellysgroup.com>\>

Cc: Lu, Eason \<<Eason_Lu@DELL.com>\>; Chen, Jessia \<<Jessia_Chen@Dell.com>\>; Shih, Matt \<<Matt_Shih@Dell.com>\>; Lin, Sybil \<<Sybil_Lin@Dell.com>\>

Subject: 戴爾技術支援 \-- B6RP4J2 (SR:959391154 )

 

您好,  张先生,

 

关于您的SAP HANA的ntp问题, 请您加入以下webex进上远程协助, 谢谢.

 

[https://dell.webex.com/dell/sc30/t.php?MTID=s32761eaa3afc275832cd9989f5bf3535](https://dell.webex.com/dell/sc30/t.php?MTID=s32761eaa3afc275832cd9989f5bf3535)

 

 

Regards,

 

Antti Huang

Software & Solutions Master Engineer

APJ Solutions Support Team (SST)

RHCE7 \| RHCE Openstack \| SAP HANA Specialist \|

CCAH Hadoop \| MCSE \| VCP \| ITIL \| JNCIA

Dell EMC \| Global Support & Deployment

 

 

已使用 OneNote 创建。
