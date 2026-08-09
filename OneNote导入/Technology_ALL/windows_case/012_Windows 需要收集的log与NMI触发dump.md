Windows 需要收集的log与NMI触发dump

Thursday, March 10, 2016

9:23 AM

1，unstable issue：

收集 mps report/Dset/Dump，thanks

MPS收集的时候将除了sql server/exchange以外的其他东西都选上。

Mps工具：[\\\\xmntsdb03\\EntTS_Log\\YGX\\Soft\\mpsreports_x64.zip](file://xmntsdb03/EntTS_Log/YGX/Soft/mpsreports_x64.zip)

 

Windwos 使用Portable_Diagnostic收集系统日志：

[\\\\xmntsdb03\\EntTS_Log\\@Dony\\Tool](file://xmntsdb03/EntTS_Log/@Dony/Tool)

 

 

BSOD了，拿下Dump和MPS report，

补充下务必拿到full memory dump，

Minidump仅供初步判断，无法分析到具体原因，

 

 

如何通过idrac收集Windows hang住日志

 

![Machine generated alternative text: Integrated Dell Remote Acce Controller 8 电 源 监 Enterprise 电 压 温 度 支 持 I Dell TechCenter I 关 于 \] ， 主 、 0 PowerEdge R730Xd DBE root Admin 順 虍 控 制 敵 幃 滁 iDRAC 设 电 源 源 控 制 源 状 念 ： 关 毛 控 制 作 @ O （ 〕 （ 〕 O 打 开 电 頂 关 以 統 毛 頂 NMI 〔 非 屏 蔽 中 断 〕 正 常 关 欞 重 设 〔 热 引 导 ） 統 关 再 开 〔 引 导 ， ](attachments/Technology_ALL_windows_case_012_Windows%20需要收集的log与NMI触发dump_001.jpg)

 

点击应用后就能保存dump，另外也需要Windows里有设置dump。

 

IDRAC 9 的触发

![[Technology_ALL_windows_case_012_Windows 需要收集的log与NMI触发dump_002.jpg]]

 

 

如果客户说没有dump生成，需要检查下面的值。如果值是0，说明不会有dump生成。

regedit

 HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\CrashControl

////// 0 means no memory dump will be generated.

![[Technology_ALL_windows_case_012_Windows 需要收集的log与NMI触发dump_003.png]]

 

 

Windows 中英文翻译：

 

[https://www.microsoft.com/zh-cn/language](https://www.microsoft.com/zh-cn/language)

 

or

 

[https://www.microsoft.com/zh-cn/language/Emt](https://www.microsoft.com/zh-cn/language/Emt)

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

== 录制方式收集数据 ==

·右键单击"开始"按钮，单击"运行"，输入"psr"，然后单击\[确定[\]]

·在"步骤记录器"窗口中，单击最右侧的箭头，然后单击\[设置[\]]。

·将"最近屏幕捕获的数量"从25更改为100，然后单击\[确定[\]]

·单击\[开始记录[\]]。

·重现问题。

·重现问题后，单击\[停止录制[\]]。

·单击\[保存[\]]图标。

·将日志保存到名为"Psr.zip"的zip文件中。

 

\-\-\-\-\--

 

・[Right click the Start button, click \"Run\", enter \"psr\" and then click \[OK\]]

・[In the \"Step Recorder\" window, click the arrow on the most right side and click \[Settings\].]

・[Change the \"Number of recent screen captures to store\] from 25 to 100 and click \[OK\]]

・[Click \[Start Record\].]

・Reproduce the problem.

・[After the issue has been reproduced, click \[Stop Record\].]

・[Click \[Save\] icon.]

・Save the logs to a zip file with name \"Psr.zip\".

 

===============================

OEM Windows 标准版，是可以激活两个VM，需要满足下面条件： 

1.使用跟宿主机一样的版本，一样的标准版的盘，不能是随便找的。

2.联网条件下，使用主机的COA标签上的KEY激活 。

 

================================

MPSReport log

Path:              [\\\\10.74.202.248\\Share\\Tools](file://10.74.202.248/Share/Tools)

[\\\\xmntsdb03\\EntTS_Log\\@Dony\\Mpsreport-tool\\Tools-1](file://xmntsdb03/EntTS_Log/@Dony/Mpsreport-tool/Tools-1)

Tool:               mpsreports_x64.exe

Options:

[        \✔[\] General]

[        \✔[\] Internet and Networking]

[        \✔[\] Business Networks]

[        \✔[\] Server Components]

[        \✔[\] Windows Update Services]

        \[ \] Exchange Servers

        \[ \] SQL and other Data Stores (MDAC))

 

 

 

已使用 OneNote 创建。
