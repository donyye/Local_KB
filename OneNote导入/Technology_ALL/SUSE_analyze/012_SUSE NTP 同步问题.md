SUSE NTP 同步问题

2018年1月16日

15:47

HANA 默认的配置是没有 \" iburst\" 参数, 所以ntpd 启动后, 如果server时间和ntp server时间相差大于1000s, 是不是同步的. 具体请参考man ntpd,  客户当前相差1万多秒

另外, 客户做了cron通过ntpdate来同步, 这是无效的, 因为当ntpd在运行的情况下,  ntpdate是不可能同步时间的.

 

\# /etc/ntp.conf

server 192.168.100.19

server time.stdtime.gov.tw

\# key (6) for accessing server variables

 

\# /usr/sbin/ntpq -p

     remote           refid      st t when poll reach   delay   offset  jitter

==============================================================================

tphq-dc02.kelly .LOCL.           1 u   22   64  377    0.269  10329.1  15.476

 

 

所以, 现在要做的是停止ntpd服务. 手工通过ntpdate同步, 确认时间正确后, 再启动ntpd. 并移除对应的ntpdate cron job.

 

具体如下:

hana131:\~ \# systemctl stop ntpd.service 

hana131:\~ \# ntpdate 192.168.31.5

16 Jan 15:37:30 ntpdate\[31013\]: adjust time server 192.168.31.5 offset 0.004022 sec

hana131:\~ \# systemctl start ntpd

hana131:\~ \# ntpq -pn

     remote           refid      st t when poll reach   delay   offset  jitter

==============================================================================

\*192.168.31.5    172.16.22.252    4 u    2   64    1    0.617   -2.069   0.001

hana131:\~ \#

 

已使用 OneNote 创建。
