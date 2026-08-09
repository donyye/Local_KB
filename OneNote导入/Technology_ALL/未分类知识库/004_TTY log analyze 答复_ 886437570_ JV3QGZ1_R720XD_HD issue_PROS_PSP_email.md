TTY log analyze 答复: 886437570\| JV3QGZ1\|R720XD\|HD issue\|PROS\|PSP\|email

Wednesday, December 11, 2013

8:37 AM

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------
  主题       答复: 886437570\| JV3QGZ1\|R720XD\|HD issue\|PROS\|PSP\|email
  发件人     Lian, Wenxiang
  收件人     Zhang, Eileen; Chen, Roy
  抄送       CN XMN TS Server Coach; Zhang, QiYun
  发送时间   Thursday, December 05, 2013 9:56 PM
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Eileen,

 

根据当前提供的TTY确认如下：

 

当前设备状态

Virtual Drives    : 3

  Degraded        : 0

  Offline         : 0

Physical Devices  : 14

  Disks           : 12

  Critical Disks  : 0

  Failed Disks    : 0

 

内存状态，未发现错误

Memory Correctable Errors   : 0

Memory Uncorrectable Errors : 0

 

硬件配置信息，未发现问题

SAS Address      : 5b8ca3a0f76f9000

BBU              : Present

Alarm            : Absent

NVRAM            : Present

Serial Debugger  : Present

Memory           : Present

Flash            : Present

Memory Size      : 1024MB

TPM              : Absent

On board Expander: Absent

Upgrade Key      : Absent

Temperature sensor for ROC    : Present

Temperature sensor for controller    : Present

 

ROC temperature : 66  degree Celcius

Controller temperature : 66  degree Celcius

 

当前硬件版本信息，确认都是官网最新版本

Product Name    : PERC H710P Mini

Serial No       : 38O03FP

FW Package Build: 21.2.0-0007

ChipRevision    : D1

 

WD9001BKHG: DS16（硬盘固件最新）

 

机器的TTY收集是有30号的硬件信息，显示11号盘出现介质错误，从阵列卡巡检情况看，到机器12月5日关机前未发现其他动作信息。

11/30/13  6:35:00: empty i2c int 00000000

 

11/30/13  6:35:01: EVT#12228-11/30/13  6:35:01: 113=Unexpected sense: PD 0b(e0x20/s11) Path 50000c0f02db458e, CDB: 2f 00 49 8d de 9e 00 10 00 00, Sense: 3/11/00

 

11/30/13  6:35:01: Raw Sense for PD b: f0 00 03 49 8d de 9e 16 00 00 00 00 11 00 00 80 01 15 80 0a 16 0d 00 00 00 00 00 00 00 00

 

12/05/13 15:43:07: EVT#13057-12/05/13 15:43:07:  42=Shutdown command received from host

 

这边注意：PERC8的阵列卡收集日志一定不能关机，关机后日志会不全，但是可以重启，参考KCS ID: 603861  

 

12月5日硬件更换记录没有，因为关机了，更换硬盘后开机，一切正常，目前机器配置了RAID10，总共10块盘，3个VD。

[T40: LD  0: L=1  SS=128  Size=e1000000  NL=14132  Status=3  DT=6218  BT=9877, Encr=0, prop=\[ID=00,dcp=0d,ccp=0d,ap=0,dc=0,dbgi=0,S=0\|0,dps=00,cps=fe\] ]（有3条信息）

 

综合现有日志判断，该case属于独立硬盘故障case，如果还继续出现问题，请提供DSET及TTY log。

 

 

 

 

 

 

Thanks & Regards,

 

Wenxiang Lian

Technical Support for CHK LE & PUB

Dell \| Global Customer Support Services

 

发件人: Zhang, Eileen 

发送时间: Thursday, December 05, 2013 17:22

收件人: Chen, Roy; Lian, Wenxiang

抄送: CN XMN TS Server Coach; Zhang, QiYun

主题: RE: 886437570\| JV3QGZ1\|R720XD\|HD issue\|PROS\|PSP\|email

 

Dell - Internal Use - Confidential 

Wenxiang:

 

谢谢！

帮忙看一下TTY。

 

 

Eileen Zhang

Enterprise Engineer

Dell \| Enterprise Support Services

![[Technology_ALL_未分类知识库_004_TTY log analyze 答复_ 886437570_ JV3QGZ1_R7_001.png]]

 

[![[Technology_ALL_未分类知识库_004_TTY log analyze 答复_ 886437570_ JV3QGZ1_R7_002.png]]](http://zh.community.dell.com/support_forums/enterprise-solutions/f/260/t/8990.aspx)

 

[![[Technology_ALL_未分类知识库_004_TTY log analyze 答复_ 886437570_ JV3QGZ1_R7_003.png]]](http://support.dell.com.cn/)

 

[![[Technology_ALL_未分类知识库_004_TTY log analyze 答复_ 886437570_ JV3QGZ1_R7_004.png]]](http://bbs.dell.com.cn/)

 

How am I doing? Email my manager [jungle_wu@dell.com](mailto:jungle_wu@dell.com) with any feedback. 

 

From: Chen, Roy

Sent: 2013年12月5日 17:21

To: Lian, Wenxiang

Cc: Zhang, Eileen; CN XMN TS Server Coach

Subject: 答复: 886437570\| JV3QGZ1\|R720XD\|HD issue\|PROS\|PSP\|email

 

Dell - Internal Use - Confidential 

wenxiang

       help on this case, thanks.

L1 希望能现在帮他确认一下。

 

Eileen

    麻烦提供log 给wenxiang ,另外晚班的紧急CASE，可以打值班的L2 电话。

 

Roy_Chen

Enterprise Product Engineer

Dell \| Enterprise Support Services

How am I doing? Email my manager (<yuxuan_xie@dell.com>) with any feedback

 

发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\] ]

发送时间: 2013年12月5日 17:16

收件人: CN XMN TS Server Escalation

抄送: Zhang, Eileen

主题: 886437570\| JV3QGZ1\|R720XD\|HD issue\|PROS\|PSP\|email

 

TAM 希望能够由L2确认一下TTY日志，帮忙确认一下除了硬盘问题外，是否还有其它的问题。

 

已使用 OneNote 创建。
