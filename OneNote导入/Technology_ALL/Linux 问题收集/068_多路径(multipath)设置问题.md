多路径(multipath)设置问题

2020年10月22日

8:21

  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: IC 82890476 \| PC 82888804 \| Unstable FC link to storage[    \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]]
  From      Lin, Cong
  To        Huang, Antti; Technical_Support; Ye, Dony; Wang, Andy King
  Sent      2020年10月19日 15:09
  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Internal Use - Confidential

 

HI Antti:

                谢谢您的回复。

 

From: Huang, Antti \<Antti_Huang@Dell.com\>

Sent: 2020年10月16日 上午 11:44

To: Lin, Cong; Technical_Support; Ye, Dony; Wang, Andy King

Subject: RE: IC 82890476 \| PC 82888804 \| Unstable FC link to storage \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]

 

Internal Use - Confidential

 

Hi Cong,

 

下面的powermt我和EMC的support确认过, 这个并不是光纤链路级别的latency, 这个是对应的sdx上的path上的IO的latency和IO的error

powerpath的修改实际上最终也是修改到KB所提到的几个参数. 都是sdx级别的监控. 并非链路上的监控.

这些io的latency是可以通过iostat查看到. 如我下面的实验环境.

 

[\[root@test1 \~\]#] multipath -ll

mpathc (36000d310006300000000000000000003) dm-10 COMPELNT,Compellent Vol 

size=10G features=\'1 queue_if_no_path\' hwhandler=\'0\' wp=rw

\`-+- policy=\'service-time 0\' prio=1 status=active

  \|- 34:0:0:1 sdc 8:32 active ready running

  \`- 36:0:0:1 sdd 8:48 active ready running

 

[\[root@test1 \~\]#] iostat -xzmd 1

Linux 3.10.0-327.el7.x86_64 (test1.sst.lab)     10/16/2020 \_x86_64\_      (20 CPU)

 

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util

sdc               0.00     0.00  518.00    0.00   259.00     0.00  1024.00     0.71    1.38    1.38    0.00   1.38  71.50

sdd               0.00     0.00  517.00    0.00   258.50     0.00  1024.00     0.68    1.31    1.31    0.00   1.31  67.70

dm-10             0.00     0.00 1035.00    0.00   517.50     0.00  1024.00     1.56    1.51    1.51    0.00   0.92  95.10

 

 

Regards,

 

Antti Huang

Senior Principal Engineer \| Solutions Support Team (SST), APJC

Dell Technologies \| APJC ISG Support Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

From: Lin, Cong \<<Cong_Lin1@Dell.com>\>

Sent: Friday, October 16, 2020 09:36

To: Huang, Antti; Technical_Support; Ye, Dony; Wang, Andy King

Subject: RE: IC 82890476 \| PC 82888804 \| Unstable FC link to storage \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]

 

Internal Use - Confidential

 

Antti：

                您好，就像我之前有提到过的powerpath有办法监控path的error以及latency，不知道multipath有没有办法可以实现此功能？

 

 

 

           

![[Technology_ALL_Linux 问题收集_068_多路径(multipath)设置问题_001.png]]

 

 

![[Technology_ALL_Linux 问题收集_068_多路径(multipath)设置问题_002.png]]

 

 

From: Huang, Antti \<<Antti_Huang@Dell.com>\>

Sent: 2020年10月16日 上午 09:15

To: Lin, Cong; Technical_Support; Ye, Dony; Wang, Andy King

Subject: RE: IC 82890476 \| PC 82888804 \| Unstable FC link to storage \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]

 

Internal Use - Confidential

 

Hi Cong,

 

这就涉及到灵敏度的问题了. 太过灵敏, 一两次的抖动就切也未必是好事. 要知道切路径的时间成本比重试更高, 所以很多软件的第一个尝试都是重试. 但重试过多或timeout过长导致应用等不了就是灵敏度过低. 都不好, 这就是一个对现有环境调优的过程. 

高可用多路径通常解决全死的问题, 对这种半死的状态是非常难处理的. 这涉及到根据现有环境调优了. 世界上没有一个万能的best practice能满足所有环境. 不同的参数会适应不同的应用环境. 

多路径软件对链路不稳定的冗余度本身是较弱的. 一旦发现不稳定链路, 就要用根本上解决链路问题(换线或口之类的), 而不是依赖多路径软件, 过度依赖多路径软件来处理不稳定路径是错误的. 它只能一定程度上避免应用受到影响. 在检测和切换过程中, 应用卡顿是肯定的. 应用受影响也是肯定的, 只是影响的多少而已(如果不从根本上解决链路问题). (需要调优来降低影响.) 所以是要从根本上解决链路问题.

如果一定要调, 我们只能建议用户按KB里所给的参数进行设置, 再进一步观察看是否能满足当前应用环境. 谢谢.

 

Regards,

 

Antti Huang

Senior Principal Engineer \| Solutions Support Team (SST), APJC

Dell Technologies \| APJC ISG Support Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

From: Lin, Cong \<<Cong_Lin1@Dell.com>\>

Sent: Thursday, October 15, 2020 17:19

To: Huang, Antti; Technical_Support; Ye, Dony; Wang, Andy King

Subject: RE: IC 82890476 \| PC 82888804 \| Unstable FC link to storage \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]

 

Internal Use - Confidential

 

HI Antti:

                您好，我看了一下KB说主要是以下的参数影响切换时间，我理解前面的2个参数主要是当rport或者说路径完全断开后的等待时间，第三个参数是当IO无法成功后反馈为timeout的等待时间，但是我们遇到的case是存储侧端口并未完全断开，只是说处于不稳定或者说丢包的一个状态，那是建议使用什么参数进行修改呢？

 

 

 

 

 

The dev_loss_tmo (rport) affects extended link timeout, in-flight I/O is held after a link-down event for seconds before the driver gives up waiting for the port to come back. Default is 30-35s, so in-flight I/O can be held seconds before being killed off. After timeout expiration, rport is put in offline (down) state.

The fast_io_fail_tmo (rport) affects how long io is queued and held while rport is in blocked state.

The checker_timeout specify the timeout to user for path checkers that issue SCSI commands with an explicit timeout, in seconds; default is taken from /sys/block/sd\<x\>/device/timeout.

The polling_interval is the interval between path checks in seconds.

 

 

From: Huang, Antti \<<Antti_Huang@Dell.com>\>

Sent: 2020年10月15日 下午 04:00

To: Technical_Support; Ye, Dony; Lin, Cong

Subject: RE: IC 82890476 \| PC 82888804 \| Unstable FC link to storage \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]

 

Internal Use - Confidential

 

Hi Cong/Dony,

 

Reviewed the log. Current multipath setting for the storage.

       device 

 

Current multipath

mpathmgmt (36000d3100583a2000000000000000009) dm-3 COMPELNT,Compellent Vol

size=600G features=\'1 queue_if_no_path\' hwhandler=\'0\' wp=rw

\`-+- policy=\'round-robin 0\' prio=1 status=active

  \|- 15:0:0:2 sdb 8:16  active ready running

  \|- 17:0:3:2 sdl 8:176 active ready running

  \|- 15:0:3:2 sdd 8:48  active ready running

  \|- 17:0:4:2 sdn 8:208 active ready running

  \|- 15:0:6:2 sdf 8:80  active ready running

  \|- 17:0:5:2 sdp 8:240 active ready running

  \|- 15:0:7:2 sdh 8:112 active ready running

  \`- 17:0:7:2 sdr 65:16 active ready running

mpathoradata (36000d31005838a00000000000000000f) dm-2 COMPELNT,Compellent Vol

size=40T features=\'1 queue_if_no_path\' hwhandler=\'0\' wp=rw

\`-+- policy=\'round-robin 0\' prio=1 status=active

  \|- 15:0:0:1 sda 8:0   active ready running

  \|- 17:0:3:1 sdk 8:160 active ready running

  \|- 15:0:3:1 sdc 8:32  active ready running

  \|- 17:0:4:1 sdm 8:192 active ready running

  \|- 15:0:6:1 sde 8:64  active ready running

  \|- 17:0:5:1 sdo 8:224 active ready running

  \|- 15:0:7:1 sdg 8:96  active ready running

  \`- 17:0:7:1 sdq 65:0  active ready running

 

From the error reported, only the 2 paths were impacted. means no issue no HBA host 15. Just the issue on the storage target 0. Need to check the FC link to the target 0 to fix the issue.

[Sep 27 11:48:00 mpdvdb1 kernel: \[1721000.551235\] sd 15:0:0:1: \[sda\] tag#0 FAILED Result: hostbyte=DID_ERROR driverbyte=DRIVER_OK]

[Sep 27 11:48:00 mpdvdb1 kernel: \[1721000.551254\] sd 15:0:0:1: \[sda\] tag#0 CDB: Read(16) 88 00 00 00 00 0a 15 04 0a d0 00 00 01 f0 00 00]

[Sep 27 11:48:00 mpdvdb1 kernel: \[1721000.551260\] blk_update_request: I/O error, dev sda, sector 43302259408]

[Sep 27 11:48:17 mpdvdb1 kernel: \[1721017.769833\] sd 15:0:0:2: \[sdb\] tag#0 FAILED Result: hostbyte=DID_ERROR driverbyte=DRIVER_OK]

[Sep 27 11:48:17 mpdvdb1 kernel: \[1721017.769849\] sd 15:0:0:2: \[sdb\] tag#0 CDB: Read(10) 28 00 0b 2e 20 20 00 04 00 00]

 

For improving the sensitive of multipath failover (Usually it\'s not required for many situation), but if you want for specific environment, you may refer to the Red Hat KB to set the OS. Thanks.

[https://access.redhat.com/solutions/137073](https://access.redhat.com/solutions/137073)

 

 

Regards,

 

Antti Huang

Senior Principal Engineer \| Solutions Support Team (SST), APJC

Dell Technologies \| APJC ISG Support Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

From: Technical_Support \<<Technical_Support@Dell.com>\>

Sent: Thursday, October 15, 2020 11:13

To: Ye, Dony; Huang, Antti; Lin, Cong

Subject: IC 82890476 \| PC 82888804 \| Unstable FC link to storage \[ ref:\_00D0bGaMp.\_5002R1D3dUe:ref \]

 

\[EXTERNAL EMAIL\]

Hi Dony,

 

I\'ve picked up the case. Pls. help to capture the required logs per conversation. Thanks.

 

Antti Huang

 

Senior Principal Engineer, Solutions Support Team (SST), APJC

Dell EMC \| Global Networking & Compute Solutions

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

ref:\_00D0bGaMp.\_5002R1D3dUe:ref

 

已使用 OneNote 创建。
