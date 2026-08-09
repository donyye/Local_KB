Windows 内存占用日志收集

2021年10月15日

15:01

使用PoolMon3VBSPerf 收集windwos 内存日志

 

\[16:03\] Lu2, Yang

是的。可以知道Paged Pool和Non-paged Pool的总量和各使用量排名前十的20个驱动的Pool Tag

 

\[16:05\] Lu2, Yang

如果某个驱动的内存使用量异常大的话，就可以通过Pool Tag来找出驱动名。

 

 

===收集方法===

・解压 PoolMon3VBSPerf.zip 并将"PoolMon3VBSPerf"文件夹复制到有问题机器上的本地文件夹（例如：C:\\Tools）。

・右键单击"\_LogPool-as-a-service.cmd"并选择"以管理员身份运行"。

・在"用户帐户控制"窗口中，单击\[是[\]]继续。

・如果显示以下消息，您可以关闭窗口，因为日志收集已经在后台启动。

 

Poolmon data collection is running as service \"Poolmon3vbs\". This window may

be closed if you do not want to see the most recent poolmon data displayed.

如图：

![Machine generated alternative text: ag onT PC i IE P 00\] ver M31 tff ITRe Mfr ok E tfF i Cf tFE exe Pref 33263161 849413 28637700 31295234 13 24229 Tools Poo Imo n -OUTPU Log Poo I -as -a -se Nice Remove Pool mon3 service 155164 84R537 10 3087 Pool Mon3VBSPerf 2021/10/15 14:51 2009/9/29 0: 1 5 2009/9/29 0:15 Bytes Wi ndows Windows Type Nonp Nonp Nonp -Itip Nonp Nonp -Itip Nonp Nonp Paged Fpsed Paged Fpsed Paged Fpsed Allocs 31 8100 7684 56 876 10 54 38 9233 1907 2991 7479 24 5682 18912 18R79 135045 44218 oolmon data e closed if collection is -running as servicp Poolmon3vbs . you dc wane va see the mose xecent poolmon This window may data displayed. ](attachments/Technology_ALL_windows_case_075_Windows%20内存占用日志收集_001.png)

 

・等待30分钟以上。

・右键单击"\_RemovePoolmon3service.cmd"并选择"以管理员身份运行"。

・在"用户帐户控制"窗口中，单击\[是[\]]继续。

・如果显示以下信息，请输入任意键关闭窗口

 

The service was successfully deleted!

Press any key to continue \...

 

・压缩文件夹"C:\\Tools\\PoolMon3VBSPerf\\Poolmon-OUTPUT"并将其发送给戴尔。

 

===英文版本===

・Extract the PoolMon3VBSPerf.zip and copy the \'PoolMon3VBSPerf\' folder to the local folder of (example: C:\\Tools) on problematic machine.

・Right click the \'\_LogPool-as-a-service.cmd\' and select \'Run as administrator\'.

・[In \'User Account Control\' window, click \[Yes\] to continue.]

・If the following message is displayed, you can close the window because the log collection has been already started in background.

=====================================================================

Poolmon data collection is running as service \"Poolmon3vbs\". This window may

be closed if you do not want to see the most recent poolmon data displayed.

=====================================================================

・Keep waiting more than 30 minutes.

・Right click the \'\_RemovePoolmon3service.cmd\' and select \'Run as administrator\'.

・[In \'User Account Control\' window, click \[Yes\] to continue.]

・If the following message is displayed, enter any key to close the window.

==================================

The service was successfully deleted!

Press any key to continue \...

==================================

・Compress the folder \'C:\\Tools\\PoolMon3VBSPerf\\Poolmon-OUTPUT\' and send it to Dell.

 

 

 

已使用 OneNote 创建。
