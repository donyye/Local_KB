关于激活windows VM问题

2019年3月28日

16:02

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: Error Code-DZWKT92
  From      Huang, Trevon
  To        ZHANG, XIANCHAO
  Cc        zhang, hui; Ye, Dony; Lee, Choon Seh
  Sent      2019年3月28日 15:58
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

最后客户说已经激活，看来是key不对，应该按照机箱上key来激活。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

您好：

 

出厂的Windows的license，有贴在主机上，您可以检查一下机箱。

 

1.Windows 标签介绍

Windows Server License是安装和使用服务器软件的权利，出厂预装OEM操作系统的主机箱体上将贴有Windows Serve正版标签(COA)

Windows Server 2012的COA标签上面只有一串product key, Dell OEM的物理机安装Server 2012时不需要激活，而物理机上创建的VM可用标签上的Product key来激活。

![[Technology_ALL_windows_case_051_关于激活windows VM问题_001.png]]

Windows Server 2008的COA上面有两个key :Product key和Virtual Key。Dell OEM的物理机安装Sever 2008时不需要激活，而物理机上创建的VM需要用Virtual key来激活。

![[Technology_ALL_windows_case_051_关于激活windows VM问题_002.png]]

CAL: 即Client Assess License，是提供用户和设备访问服务器的权利。

Windows Server 2012以前的版本, CAL是与系统捆绑销售。从Windows Server 2012开始被分开销售。

 

使用电话激活选项

1.  在"Windows 激活"中选择\[显示其他激活方法[\]]
2.  在"Windows 激活"中选择\[使用自动电话系统[\]]
3.  在"Windows 激活"中选择\[中国[\]]

可以拨打800 830 1832(免费) 或 800 820 3800(免费) 或 400 820 3800(免费)，或者拨打+86 21-96081368(收费)。拨通激活中心，根据语音提示转入人工服务。

800 830 1832 转1再转5  转入人工后，说明降级需求，激活中心同事将会帮助您激活。以下提供参考，最终以激活中心工作人员要求为准。

 

 

 

 

From: ZHANG, XIANCHAO \<XIANCHAO.ZHANG@rexel.com.cn\>

Sent: 2019年3月28日 15:34

To: Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

Hi All,

 

   刚刚和微软通了电话，和他们电话确认了，激活策略没有变更，是可以激活的。要用主机的激活码激活。

 

微软协助我找到了后5位激活码，但是和我收到的纸质的激活产品上的不一样。你们那边有什么办法可以协助我找到吗？

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_003.png]]

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: ZHANG, XIANCHAO

Sent: 2019年3月25日 14:15

To: \'Trevon.Huang@Dell.com\' \<<Trevon.Huang@Dell.com>\>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

Hi Trevon,

 

  我需要再跟微软确认一下。

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Trevon.Huang@Dell.com> \[[mailto:Trevon.Huang@Dell.com](mailto:Trevon.Huang@Dell.com)\]

Sent: 2019年3月25日 14:13

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

尊敬的戴尔客户：

 

 服务器报修/咨询的问题解决了吗？ 有什么需要帮助？

 

 

 

 

 

 

 

Trevon Huang 

Tech Support Analyst, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[Trevon_Huang@dell.com](mailto:Trevon_Huang@dell.com)

 

How am I doing? Please contact my manager [Sukie_Wu@Dell.com](mailto:Sukie_Wu@Dell.com)

 

 

 

From: Ye, Dony

Sent: 2019年3月22日 12:56

To: ZHANG, XIANCHAO; Huang, Trevon; Lee, Choon Seh

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

Dear,

 

我们可以看到文章下面的描述，而这个文章是2013年的，微软可能会不时更改它。

![[Technology_ALL_windows_case_051_关于激活windows VM问题_004.jpg]]

关于新的描述更新与2018年，下面的两个链接，需要Windows 2012 DC for AVMA.

 

[https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)](https://eur02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fprevious-versions%2Fwindows%2Fit-pro%2Fwindows-server-2012-R2-and-2012%2Fdn303421(v%3Dws.11)&data=02%7C01%7CXIANCHAO.ZHANG%40rexel.com.cn%7Ccc955f25c0734eaf267708d6b0e9044d%7Cf91cd4eb0e4b4bcc982e32c194cfcefa%7C0%7C0%7C636890912212681734&sdata=hCsbKXE4MHOyEUik2xHDjqotbta6ncRajlBZgobwa4k%3D&reserved=0) 

 

[https://docs.microsoft.com/en-us/windows-server/get-started-19/vm-activation-19](https://eur02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows-server%2Fget-started-19%2Fvm-activation-19&data=02%7C01%7CXIANCHAO.ZHANG%40rexel.com.cn%7Ccc955f25c0734eaf267708d6b0e9044d%7Cf91cd4eb0e4b4bcc982e32c194cfcefa%7C0%7C0%7C636890912212691747&sdata=q8mPKQTlLMzzkQUn%2BpYBpLFBvL7SS%2BTg7Ss8Hp6rxTc%3D&reserved=0) 

 

 

 

 

B R

Dony

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月22日 10:28

To: Ye, Dony; Huang, Trevon; Lee, Choon Seh

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

我不认同这一点，因为之前我在Windows 2012 R2 Standard 宿主机用Hyper-V 可以激活

 

 

[http://download.microsoft.com/download/F/3/9/F39124F7-0177-463C-8A08-582463F96C9D/Windows_Server_2012_R2_Licensing_Datasheet.pdf](https://eur02.safelinks.protection.outlook.com/?url=http%3A%2F%2Fdownload.microsoft.com%2Fdownload%2FF%2F3%2F9%2FF39124F7-0177-463C-8A08-582463F96C9D%2FWindows_Server_2012_R2_Licensing_Datasheet.pdf&data=02%7C01%7CXIANCHAO.ZHANG%40rexel.com.cn%7Ccc955f25c0734eaf267708d6b0e9044d%7Cf91cd4eb0e4b4bcc982e32c194cfcefa%7C0%7C0%7C636890912212701755&sdata=ZTl%2FzB8M5ordNhjWUUpfTLW2TI81f0ELtHv0YpWSbL4%3D&reserved=0)

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_005.png]]

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Dony.Ye@dell.com> \[[mailto:Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)\]

Sent: 2019年3月22日 10:15

To: <Trevon.Huang@Dell.com>; <Choon.Seh.Lee@dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>; ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

Hi, All

 

通过一些KB我们发现只有Datacenter版本的windows 2012R2才能再hyper-v下激活VM，而Standard版本的是不行的。

 

如KB的描述：

[https://docs.microsoft.com/en-us/windows-server/get-started-19/vm-activation-19](https://eur02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows-server%2Fget-started-19%2Fvm-activation-19&data=02%7C01%7CXIANCHAO.ZHANG%40rexel.com.cn%7Ccc955f25c0734eaf267708d6b0e9044d%7Cf91cd4eb0e4b4bcc982e32c194cfcefa%7C0%7C0%7C636890912212701755&sdata=EbrnZS2Be3GGGyKzkZ2TOBQFZh73k6Tg2lAnQ%2BdTF5U%3D&reserved=0)

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_006.png]]

 

 

B R

Dony

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月20日 15:44

To: Ye, Dony; Lee, Choon Seh; Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

Hi Dony,

 

  我们是随机光盘，采购了三台服务器，所以激活信息可能混淆了。但是这三组激活码当时我们都测试了。照片如下：

 

 

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Dony.Ye@dell.com> \[[mailto:Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)\]

Sent: 2019年3月20日 15:39

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>; <Choon.Seh.Lee@dell.com>; <Trevon.Huang@Dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

Dear，

 

请问能对机箱上贴的windows key拍一张清晰的照片给我们看看吗？

 

B R

Dony

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月20日 15:23

To: Ye, Dony; Lee, Choon Seh; Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

Dony,

 

  按你所描述的，下载了镜像并安装，用命令去激活时，出现了之前的错误。

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_007.png]]

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Dony.Ye@dell.com> \[[mailto:Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)\]

Sent: 2019年3月20日 14:41

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>; <Choon.Seh.Lee@dell.com>; <Trevon.Huang@Dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

Dear,

 

经过刚才的远程，key还是不被成功加入到VM，请通过下面链接下载新的windows2012R2镜像，然后安装VM，然后我们再测试激活。谢谢！

 

[https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2012-r2](https://eur02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.microsoft.com%2Fen-us%2Fevalcenter%2Fevaluate-windows-server-2012-r2&data=02%7C01%7CXIANCHAO.ZHANG%40rexel.com.cn%7Ccc955f25c0734eaf267708d6b0e9044d%7Cf91cd4eb0e4b4bcc982e32c194cfcefa%7C0%7C0%7C636890912212711763&sdata=f5JHYSACH6i1gh4Yfbz6MkAHErKlgIz8SSZjn4Y1w7g%3D&reserved=0)

 

B R

Dony

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月20日 13:36

To: Lee, Choon Seh; Ye, Dony; Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

可能你们还要再等一下。

 

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_008.png]]

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Choon.Seh.Lee@dell.com> \[[mailto:Choon.Seh.Lee@dell.com](mailto:Choon.Seh.Lee@dell.com)\]

Sent: 2019年3月20日 13:16

To: <Dony.Ye@dell.com>; ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>; <Trevon.Huang@Dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

HI All,

Please join [https://dell.webex.com/dell/e.php?MTID=m01602fdbe1fca7e9c70cd10c5b254565](https://eur02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdell.webex.com%2Fdell%2Fe.php%3FMTID%3Dm01602fdbe1fca7e9c70cd10c5b254565&data=02%7C01%7CXIANCHAO.ZHANG%40rexel.com.cn%7Ccc955f25c0734eaf267708d6b0e9044d%7Cf91cd4eb0e4b4bcc982e32c194cfcefa%7C0%7C0%7C636890912212721772&sdata=eC%2B8EqfVS5b0arqJ7ECYx2QMn2HKx7nQDWbysyz1HTo%3D&reserved=0)

 

Regards,

Lee Choon Seh (李春昇)

Principal Engineer, Infrastructure & Client Solutions Support

APJ Solutions Support Team

Dell EMC \| Support and Deployment Services

[Choon.Seh.Lee@Dell.com](mailto:Choon.Seh.Lee@Dell.com)  

Working Hours: Monday ‒ Friday \| 8:00 ‒ 17:00 (MYSG Time)

 

From: Ye, Dony

Sent: Wednesday, March 20, 2019 1:14 PM

To: ZHANG, XIANCHAO; Huang, Trevon

Cc: zhang, hui; Lee, Choon Seh

Subject: RE: Error Code-DZWKT92

 

Dear,

 

我们通过WebEx 的工具，不需要安装，我们会给你一个远程的连接。远程到您笔记本上，然后您笔记本能访问服务器就行。

 

B R

Dony

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月20日 13:11

To: Ye, Dony; Huang, Trevon

Cc: zhang, hui; Lee, Choon Seh

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

Hi Dony,

 

  要远程我这台虚拟机吗？你们要用什么远程工具？

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Dony.Ye@dell.com> \[[mailto:Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)\]

Sent: 2019年3月20日 13:10

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>; <Trevon.Huang@Dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>; <Choon.Seh.Lee@dell.com>

Subject: RE: Error Code-DZWKT92

 

Dear,

 

请问您那边有时间可以远程吗？

 

B R

Dony

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月20日 13:00

To: Ye, Dony; Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

Hi Dony,

 

执行过程，请参考如下报错。我安装的是标准版。

 

 

+-------------------------------------------------------------------------------------+
| Microsoft Windows \[Version 6.3.9600\]                                              |
|                                                                                     |
| \(c\) 2013 Microsoft Corporation. All rights reserved.                              |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| C:\\Windows\\system32\>dism /online /get-currentedition                             |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Deployment Image Servicing and Management tool                                      |
|                                                                                     |
| Version: 6.3.9600.17031                                                             |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Image Version: 6.3.9600.17031                                                       |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Current edition is:                                                                 |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Current Edition : ServerStandard                                                    |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| The operation completed successfully.                                               |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| C:\\Windows\\system32\>dism /online /get-targeteditions                             |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Deployment Image Servicing and Management tool                                      |
|                                                                                     |
| Version: 6.3.9600.17031                                                             |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Image Version: 6.3.9600.17031                                                       |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Editions that can be upgraded to:                                                   |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Target Edition : ServerDatacenter                                                   |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| The operation completed successfully.                                               |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| C:\\Windows\\system32\>slmgr /ipk DBGBW-NPF86-BJVTX-K3WKJ-MTB6V                     |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| C:\\Windows\\system32\>DISM /online /Set-Edition:ServerStandard /ProductKey:DBGBW-N |
|                                                                                     |
| PF86-BJVTX-K3WKJ-MTB6V /AcceptEula                                                  |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Deployment Image Servicing and Management tool                                      |
|                                                                                     |
| Version: 6.3.9600.17031                                                             |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Image Version: 6.3.9600.17031                                                       |
|                                                                                     |
|                                                                                     |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| Error: 50                                                                           |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| This Windows image cannot upgrade to the edition of Windows that was specified.     |
|                                                                                     |
| The upgrade cannot proceed.                                                         |
|                                                                                     |
| Run the /Get-TargetEditions option to see what edition of Windows you can upgrad    |
|                                                                                     |
| e to.                                                                               |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| The DISM log file can be found at C:\\Windows\\Logs\\DISM\\dism.log                 |
|                                                                                     |
|                                                                                     |
|                                                                                     |
| C:\\Windows\\system32\>                                                             |
+-------------------------------------------------------------------------------------+

 

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Dony.Ye@dell.com> \[[mailto:Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)\]

Sent: 2019年3月20日 12:52

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>; <Trevon.Huang@Dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

Dear,

 

请在VM里，使用管理员允许CMD输入下面命令：

1.dism /online /get-currentedition

2.dism /online /get-targeteditions

3.slmgr /ipk DBGBW-NPF86-BJVTX-K3WKJ-MTB6V (may require reboot VM)

4.DISM /online /Set-Edition:ServerStandard /ProductKey: DBGBW-NPF86-BJVTX-K3WKJ-MTB6V /AcceptEula (may require reboot VM)

 

 

 

B R

Dony

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月19日 17:19

To: Ye, Dony; Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

Dony,

 

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_009.png]]

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Dony.Ye@dell.com> \[[mailto:Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)\]

Sent: 2019年3月19日 17:16

To: <Trevon.Huang@Dell.com>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>; ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

Hi, Trevon

 

建议客户安装下面的步骤看看能否激活VM：

 

VM连接inter

1. 卸载旧KEY：

slmgr /upk

 

2. 安装新KEY:

slmgr /ipk  \[新[KEY\]]

 

3.激活 

slmgr /ato

 

 

 

B R

Dony

 

From: Huang, Trevon

Sent: 2019年3月19日 16:35

To: Ye, Dony

Cc: zhang, hui; ZHANG, XIANCHAO

Subject: RE: Error Code-DZWKT92

 

Hi  Dony

 

麻烦核实一下，谢谢

 

 

 

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月19日 16:33

To: Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code-DZWKT92

 

\[EXTERNAL EMAIL\]

![[Technology_ALL_windows_case_051_关于激活windows VM问题_010.jpg]]

 

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Trevon.Huang@Dell.com> \[[mailto:Trevon.Huang@Dell.com](mailto:Trevon.Huang@Dell.com)\]

Sent: 2019年3月19日 16:31

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code-DZWKT92

 

请把光盘拍个照片过来，谢谢

 

 

 

 

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月19日 16:29

To: Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code

 

\[EXTERNAL EMAIL\]

这个是Dell给我们的随机光盘

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Trevon.Huang@Dell.com> \[[mailto:Trevon.Huang@Dell.com](mailto:Trevon.Huang@Dell.com)\]

Sent: 2019年3月19日 16:27

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code

 

您好：

 

您使用hyper-v安装的2012R2，镜像是出厂定制的ISO吗？还是在其他地方下载？

 

 

 

 

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月19日 16:07

To: Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code

 

\[EXTERNAL EMAIL\]

Hi Trevon,

 

  这是第一个 2012 

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Trevon.Huang@Dell.com> \[[mailto:Trevon.Huang@Dell.com](mailto:Trevon.Huang@Dell.com)\]

Sent: 2019年3月19日 16:06

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code

 

您好：

 

现在hyper-v安装的2012R2是第几个？

 

 

 

 

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月19日 15:56

To: Huang, Trevon

Cc: zhang, hui

Subject: RE: Error Code

 

\[EXTERNAL EMAIL\]

Hi Trevon,

 

   Thanks.

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

From: <Trevon.Huang@Dell.com> \[[mailto:Trevon.Huang@Dell.com](mailto:Trevon.Huang@Dell.com)\]

Sent: 2019年3月19日 15:54

To: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Cc: zhang, hui \<<hui.zhang@rexel.com.cn>\>

Subject: RE: Error Code

 

收到，我这边查询一下，您也联系一下微软，请您理解，谢谢！

 

 

 

 

 

 

From: ZHANG, XIANCHAO \<<XIANCHAO.ZHANG@rexel.com.cn>\>

Sent: 2019年3月19日 15:49

To: Huang, Trevon

Cc: zhang, hui

Subject: Error Code

 

\[EXTERNAL EMAIL\]

Error:

 

![[Technology_ALL_windows_case_051_关于激活windows VM问题_011.png]]

 

Best Regards,

Bruce ZHANG

 

Tel : +86 (531) 5575 9120

Mobile: +86 131 7669 3800

 

 

已使用 OneNote 创建。
