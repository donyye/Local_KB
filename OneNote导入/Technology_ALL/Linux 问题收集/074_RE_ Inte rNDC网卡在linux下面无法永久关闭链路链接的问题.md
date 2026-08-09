RE: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

2021年9月18日

13:40

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题
  From      Ruan, Garuda
  To        Lin, XianXi; Wang, Xing Fang; Qin, Bill; Chu, Zunli; Ye, Dony
  Cc        Chi, Nathan; Chen, Bob; Chen, Salina; Lin, Yongliang
  Sent      2021年9月18日 13:10
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

All,

 

补充下第三点

 

3. Intel X710网卡固件 20.0.17，这个固件本身有点异常，可能会导致网卡出现端口down的情况。我们后续会有新版本来Fix。

 

不过和Permit Total Port Shutdown相关的issue目前只发现在XXV710上。

 

可以根据客户的实际情况考虑。

 

B.R

Garuda

 

Principal Engineer\| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 6430 EXT. 8886430

[Garuda_Ruan@Dell.com](mailto:Garuda_Ruan@Dell.com)

![[Technology_ALL_Linux 问题收集_074_RE_ Inte rNDC网卡在linux下面无法永久关闭链路链接的问题_001.jpg]]

How am I doing? Please contact my manager <Xing_Fang_Wang@DELL.com> to provide feedback. Thanks!

 

 

Internal Use - Confidential

From: Lin, XianXi \<XianXi_Lin@DELL.com\>

Sent: Saturday, September 18, 2021 13:05

To: Ruan, Garuda; Wang, Xing Fang; Qin, Bill; Chu, Zunli; Ye, Dony

Cc: Chi, Nathan; Chen, Bob; Chen, Salina; Lin, Yongliang

Subject: 回复: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Dear all

总结如下:

尝试途径1-网卡固件版本以及设置

刷到最新固件, 看网卡固件界面里是不是有一个permit total port shutdown的选项, 如果有, 可以设置成enable(默认是disable)

![[Technology_ALL_Linux 问题收集_074_RE_ Inte rNDC网卡在linux下面无法永久关闭链路链接的问题_002.jpg]]

 

![[Technology_ALL_Linux 问题收集_074_RE_ Inte rNDC网卡在linux下面无法永久关闭链路链接的问题_003.jpg]]

以下是主要10g/25G网卡的固件

1. Mellanox ConnectX-4 Lx Ethernet Adapter Firmware 固件升级到 14.31.10.14

[https://dl.dell.com/FOLDER07515967M/1/Network_Firmware_900N0_WN64_14.31.10.14.EXE](https://dl.dell.com/FOLDER07515967M/1/Network_Firmware_900N0_WN64_14.31.10.14.EXE)

看看新固件里是不是增加了一个开关

 

2\. Broadcom NetXtreme-E 21.85.21.91

[https://dl.dell.com/FOLDER07498796M/1/Network_Firmware_YPXWJ_WN64_21.85.21.91.EXE](https://dl.dell.com/FOLDER07498796M/1/Network_Firmware_YPXWJ_WN64_21.85.21.91.EXE)

 

3. Intel X710网卡固件 20.0.17(该版本固件有hotissue)

[https://dl.dell.com/FOLDER07187524M/1/Network_Firmware_DK4G2_WN64_20.0.17_A00.EXE](https://dl.dell.com/FOLDER07187524M/1/Network_Firmware_DK4G2_WN64_20.0.17_A00.EXE)

!!!据2线拿到的信息, 该版本的permit total port shutdown有hot issue, 启用后会导致网口down掉后不能up, 需要重启整机才行. fix这个issue的新版本固件大概在2022年3月发布. 建议继续使用ethtool方式:

ethtool \--set-priv-flags em1 link-down-on-close on

ethtool \--set-priv-flags em2 link-down-on-close on

 

 

尝试途径2-网卡/网口操作系统层面设置

 

如果刷新到最新网卡固件后没有目前没有里没有"Permit Total Port Shutdown"选项, 需要使用网卡厂商自有工具或者方式解决:

1.  Mellanox ConnectX-4

mlxconfig工具修改固件内参数, 安装工具包会有依赖关系

[https://www.mellanox.com/downloads/MFT/mft-4.17.0-106-x86_64-rpm.tgz](https://www.mellanox.com/downloads/MFT/mft-4.17.0-106-x86_64-rpm.tgz)

 

1.  Broadcom NetXtreme-E 21.85.21.91

暂无工具提供, 请zunli联系获取

 

1.  Intel X710

ethtool设置, 针对集成网卡的2个万兆网口命令如下:

![[Technology_ALL_Linux 问题收集_074_RE_ Inte rNDC网卡在linux下面无法永久关闭链路链接的问题_004.png]]

 

 

 

 

LIN XianXi / 林宪玺

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Senior Service Account Manager

Dell Technologies \| Global Account Management Services

Mobile [199-199-10398](tel:+44-123-4567890) / [198-0102-2046](tel:+44-123-4567890) 

eMail  <XianXi_Lin@dell.com>

My manager is <David.Shen@dell.com> Thanks!

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

 

 

Internal Use - Confidential

发件人: Ruan, Garuda \<[Garuda_Ruan@DELL.com](mailto:Garuda_Ruan@DELL.com)\> 

发送时间: 2021年9月18日 12:02

收件人: Lin, XianXi; Wang, Xing Fang; Qin, Bill; Chu, Zunli; Ye, Dony

抄送: Chi, Nathan; Chen, Bob; Chen, Salina; Lin, Yongliang

主题: RE: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Hi Xianxi,

 

As we talk, Dell didn't provide official tool to disable NCSI to full shutdown NIC port in OS.

 

But in some new NIC with new FW, we provided  'permit total port shutdown' feature. You can enable this in NIC BIOS to let Linux  full shutdown the NIC port when using ifdown command.

 

Suggest upgraded the NIC FW to newest and check.

 

An example from Intel NIC:

 

 

 

B.R

Garuda

 

Principal Engineer\| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 6430 EXT. 8886430

[Garuda_Ruan@Dell.com](mailto:Garuda_Ruan@Dell.com)

![[Technology_ALL_Linux 问题收集_074_RE_ Inte rNDC网卡在linux下面无法永久关闭链路链接的问题_001.jpg]]

How am I doing? Please contact my manager <Xing_Fang_Wang@DELL.com> to provide feedback. Thanks!

 

 

Internal Use - Confidential

From: Lin, XianXi \<<XianXi_Lin@DELL.com>\>

Sent: Saturday, September 18, 2021 11:37

To: Wang, Xing Fang; Qin, Bill; Chu, Zunli; Ye, Dony

Cc: Chi, Nathan; Chen, Bob; Chen, Salina; Lin, Yongliang; Ruan, Garuda

Subject: 回复: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

 

 

EU's requirement: Link must be in physically down status after manually run port down command, such as "ifdown em1" or "ip link set em1 down"

 

\@Chu, Zunli  I think you can provide a PN list of NICs used by China Telecom recently and in coming downloads.

 

 

 

LIN XianXi / 林宪玺

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Senior Service Account Manager

Dell Technologies \| Global Account Management Services

Mobile [199-199-10398](tel:+44-123-4567890) / [198-0102-2046](tel:+44-123-4567890) 

eMail  <XianXi_Lin@dell.com>

My manager is <David.Shen@dell.com> Thanks!

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

 

 

 

Internal Use - Confidential

发件人: Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\> 

发送时间: 2021年9月18日 11:23

收件人: Qin, Bill; Chu, Zunli; Ye, Dony

抄送: Chi, Nathan; Lin, XianXi; Chen, Bob; Chen, Salina; Lin, Yongliang; Ruan, Garuda

主题: RE: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Dony

 

Help look at the issue to update your suggestion.

 

Xing Fang Wang

Manager \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 5846 EXT. 8885846

Mobile phone +86 18030233742

[Xing_Fang_Wang@Dell.com](mailto:Xing_Fang_Wang@Dell.com)

![[Technology_ALL_Linux 问题收集_074_RE_ Inte rNDC网卡在linux下面无法永久关闭链路链接的问题_005.jpg]]

How am I doing? Please contact my manager John_O\'hare@Dell.com to provide feedback. Thanks!

 

 

 

Internal Use - Confidential

From: Qin, Bill \<<Bill_Qin@Dell.com>\>

Sent: Saturday, September 18, 2021 11:09 AM

To: Chu, Zunli; Wang, Xing Fang

Cc: Chi, Nathan; Lin, XianXi; Chen, Bob; Chen, Salina

Subject: RE: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Xing Fang ,

 

Could your team take a look this case and share your expertise?

 

Zunli,

 

Please share service tag

 

Thanks and best regards

Bill

 

 

 

Internal Use - Confidential

From: Chu, Zunli \<<Zunli_Chu@Dell.com>\>

Sent: Saturday, September 18, 2021 11:04 AM

To: Qin, Bill; Wang, Xing Fang

Cc: Chi, Nathan; Lin, XianXi; Chen, Bob; Chen, Salina

Subject: 回复: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Hi Bill，

 

TS team表示已经无能为力

 

褚遵利 18911801605

 

 

Internal Use - Confidential

发件人: Qin, Bill \<[Bill_Qin@Dell.com](mailto:Bill_Qin@Dell.com)\> 

发送时间: 2021年9月18日 11:00

收件人: Chu, Zunli; Wang, Xing Fang

抄送: Chi, Nathan; Lin, XianXi; Chen, Bob; Chen, Salina

主题: RE: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Zunli,

 

     What's TS team comments?

 

\+ Xing Fang

 

Thanks and best regards

Bill

 

 

 

Internal Use - Confidential

From: Chu, Zunli \<<Zunli_Chu@Dell.com>\>

Sent: Saturday, September 18, 2021 10:47 AM

To: Qin, Bill

Cc: Chi, Nathan; Lin, XianXi; Chen, Bob; Chen, Salina

Subject: 回复: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Urgent，loop Bill because Salina is on annual leave today

 

 

Internal Use - Confidential

发件人: Chu, Zunli 

发送时间: 2021年9月18日 10:45

收件人: Chen, Salina

抄送: Chi, Nathan; Lin, XianXi; Chen, Bob

主题: Inte rNDC网卡在linux下面无法永久关闭链路链接的问题

 

Hi Salina&Nathan，

 

问题：电信采购的R740xd里的rNDC网卡是Intel X710芯片，在linux下面用ifdown命令关闭网口后，网口上的LED等依然亮。客户觉得不可以接受。

 

1、不能永久解决问题的方案：找一个命令，可以在linux下面执行后，再用ifdown 命令去关系网口，可以关闭LED灯。可以临时解决这个问题，但是机器重启后作用失效，客户觉得这种方法不能接受。

[How to disable X710 LAN LED after key-in \"ifconfig down\"(TX disable) -- Advantech Cloud IoT Service Portal (zendesk.com)](https://advantech-ncg.zendesk.com/hc/en-us/articles/360020060592-How-to-disable-X710-LAN-LED-after-key-in-ifconfig-down-TX-disable-)

\# ethtool \--set-priv-flags b17p0 link-down-on-close on

 

2、请问我们能否向PG升级，有什么方法可以永久解决？售后服务team已经无能为力。

 

目前看其它网卡品牌是有方法设置的，mellanox是通过在linux中执行一个程序关闭firmware里的某个参数即可永久解决，Broadcom的25G网卡可以在启动时检测到网卡后进入设置解决设置某个参数解决。

 

Thanks & Regards.

 

Zunli Chu  褚遵利

DCCS System Engineer, ENT & GA North

Dell Technologies \| Greater China DCCS

Mobile +86 189 1180 1605  <zunli.chu@dell.com>

Unit 507.5 / F. Tower A, Full Link Plaza. No.18 Chaoyangmenwai Avenue, Beijing, China

 

 

Internal Use - Confidential

 

已使用 OneNote 创建。
