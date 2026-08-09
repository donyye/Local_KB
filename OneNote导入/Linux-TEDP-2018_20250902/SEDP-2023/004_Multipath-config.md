Multipath-config

2023年12月26日

9:11

 

 

![[Linux-TEDP-2018_2025_SEDP-2023_004_Multipath-config_001.png]]

 

 

\# yum install device-mapper-\* -y

\# mpathconf \--enable

\# systemctl start multipathd

 

[\[root@localhost \~\]# multipath -ll]

mpatha (36001405cd0be2ba4f084531a7942eb79) dm-0 LIO-ORG,disk0

size=30G features=\'0\' hwhandler=\'1 alua\' wp=rw

\|-+- policy=\'service-time 0\' prio=50 status=active

\| \`- 33:0:0:0 sdb 8:16 active ready running

\`-+- policy=\'service-time 0\' prio=50 status=enabled

  \`- 34:0:0:0 sdc 8:32 active ready running

 

policy=\'service-time 0\': 负载均衡策略，通过选择服务时间最短的路径，也就是那个路径响应快就走那条。配置选项 path_selector \"xxxx\"

prio=50 这个是路径的优先级，0\~100 ， 100优先级最高，而配置是在配置文件里的 prio 参数

 

 

重新加载配置

\# multipath -r 

 

 

<https://access.redhat.com/solutions/137073>

 

defaults ]

 

polling_interval: 这是轮询间隔的时间，单位是秒。它定义了多路径软件定期检查路径状态的频率。在这里，路径每 5 秒进行一次状态检查。

fast_io_fail_tmo: 这是 I/O 失败的超时时间，单位是秒。当 I/O 操作失败时，multipath 会在此超时时间内快速切换到另一路径。在这里，设置为 5 秒。

dev_loss_tmo: 这是设备丢失超时时间，单位是秒。它指定了在检测到设备丢失后等待多长时间之后进行切换。在这里，设置为 10 秒。默认是 30-35s

checker_timeout: 发出具有显式超时的 SCSI 命令的路径检查器的用户超时时间（以秒为单位）;缺省值取自 /sys/block/sd\<x\>/device/timeout 。

 

 

设置blacklist 有两种方法如下：

blacklist 

 

 

 

获取本地 sdc 盘 wwid

\[root@ha01 \~\]# /lib/udev/scsi_id -g -u /dev/sdc

360014056a2143ca881246fca7ee032d2

 

 

 

 

已使用 OneNote 创建。
