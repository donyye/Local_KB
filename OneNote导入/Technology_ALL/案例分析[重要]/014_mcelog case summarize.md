mcelog case summarize

Friday, August 01, 2014

10:49 AM

1，mcelog 常出现的错误信息

 

1)[  ]首先会再message里出现此报错信息"Hardware Error"

![Machine generated alternative text: JItn1809:17:31 dUle. Jun1809:17:31 JUn1809:17:31 JUn1809:17:31 ortmodule. JUn1809:17:32 JUn1809:17:32 JUn1809:17:32 JUn1809:17:32 JUn1809:17:32 JUn1809:17:33 JUn1809:17:33 Jun1809:17:3性 couldn\'tlocdte JUn1809:17:3怪 JUnl日日9:17:35 JItn1818:81:81 ementedget Jun1215:15：性7 JUn1815:15：怪7 JUn1818：唯性：51 JUn1818：唯性：51 Jun1828:IB:55 IDCdlhost\'ernel:RPC: Registered Registered Registered Reqistered ndmedUHIXsockettransportmo IDCdlh0St IDCdlh0St IDCdlh0St 君r劝el:RPC: \'ernel:RPC: kernel:RPC: udptrdnsportmodule. tcptrdnsportmodule. tcpHFSv4.1backchanne1transP locdlhostkdumP:kexec:loadedkdumpkernel localhostkdumP:stdrtedup localhostacpid:startingup localhostacpid:ruleloaded localhostacpid:\"aitingforeuents:eventlogging15off locdlhostdcpid:clientconnectedfrom298日［68:68\] localhostacpid:1clientruleloaded localhostdutomount\[2923\]:lookuP_redd_master:lookup(nisplus): n15+tableduto.master localhostmcelog:fdiledtoprefillDl日日ddtdbdsefromD日1ddta localhostabrtd:InitcomPlete,enteringmainloop locdlhost\"ernel:EBST:HVRA日EBSTLogAddressBange15notimPI IDCdlh0St IDCdlh0St IDCdlh0St IDCdlh0St IDCdlh0St \'ernel kernel 戈er协el lernel kernel :\[Hdrd\"dreErrorl:Hdchine :\[Hdrd\"dreError\]:Hdchine :rdte1imit:1cdllbdCkS :\[Hdrd\"areErrDr\]：日dchine :\[Hard\"areError\]:Hachine CheCkeUentS CheCkeUentS suPPressed CheCkeVentS CheCkeUentS 96性，6](attachments/Technology_ALL_案例分析%5B重要%5D_014_mcelog%20case%20summarize_001.jpg)

 

2) cat /var/log/mcelog 时能看到此信息"CPU 0 BANK 9"等错误

![[Technology_ALL_案例分析[重要]_014_mcelog case summarize_002.png]]

 

3) 有一些 mcelog 记录的信息在一天之内写30G\~50G之多。

 

2，此问题的解决方法

 

优先考虑CPU与MB，首先进行一些测试来定位问题。

 

Step1：交换CPU0 与 CPU1，查看报错信息是否有所改变。如果没有，排除CPU的可能性。

- 需要注意，有种情况是在没做交换CPU测试时mcelog记录非常快，文件大小也增长快，但是交换测试后mcelog还是会出现，但是明显减少，这时也需要注意CPU问题。

 

Step2：交换内存插槽测试，查看报错信息是否有所改变。如果没有，排除内存问题。

- 拿R910举例，内存比较多，交换测试时使用两组两组交换测试。

 

Step3：观察主板上的CPU1插口是否有针脚歪的情况出现。

- 一个M910 CASE mcelog报错发现的问题，这个比较难发现，需要仔细看。

![[Technology_ALL_案例分析[重要]_014_mcelog case summarize_003.png]]

 

三步之后都能确定问题，那为什么我们判断是CPU1而 不是CPU2上的问题了？主要是看报错的信息是"CPU 0 BANK 9"，只要查看一下 /proc/cpuinfo 就知道对应关系。

例子：

\# cat /proc/cpuinfo

processor        : 0[           \--\> Logic CPU]

vendor_id        : GenuineIntel

cpu family        : 6

model                : 23

model name        : Intel(R) Xeon(R) CPU           X3353  @ 2.66GHz

stepping        : 6

cpu MHz                : 2000.000

cache size        : 6144 KB

physical id        : 0[           ][ \--\> Physical CPU]

\...\...

 

processor        : 1[           \--\> Logic CPU]

vendor_id        : GenuineIntel

cpu family        : 6

model                : 23

model name        : Intel(R) Xeon(R) CPU           X3353  @ 2.66GHz

stepping        : 6

cpu MHz                : 2000.000

cache size        : 6144 KB

physical id        : 1[          ][ \--\> Physical CPU]

\...\...

 

现在应该知道CPU0是指那个物理CPU了吧。

BANK 9 指的是物理内存地址，但是这个目前为止没办法准确知道是那条内存。

 

3，其它出现在mcelog里的信息

 

meclog:processor 25 below trip temperature.throffing disable

meclog:processor 27 below trip temperature.throffing disable

meclog:processor 11 below trip temperature.throffing disable

 

log表示CPU温度没有到达阀值，没有触发throttling，这并不是一个报错信息。

 

解决方法: 可通过关闭 C1E、C States、MONITOR/MWAIT 或是升级kernel解决，建议使用前一种，关闭后此信息部再记录。

 

4, 在某些情况下linux会报下面的类似错误。提示硬件错误。

Error message

TIME 1392739789 Wed Feb 19 00:09:49 2014

MCG status:

MCi status:

Corrected error

MCi_MISC register valid

MCi_ADDR register valid

MCA: MEMORY CONTROLLER MS_CHANNEL0_ERR

Transaction: Memory scrubbing error

STATUS 8c000040000800c0 MCGSTATUS 0

MCGCAP 1000c14 APICID 20 SOCKETID 1 

CPUID Vendor Intel Family 6 Model 45

Hardware event. This is not a software error.

MCE 3

CPU 8 BANK 8 

MISC 90003830383008c ADDR f36114800 

TIME 1392739789 Wed Feb 19 00:09:49 2014

MCG status:

MCi status:

Corrected error

MCi_MISC register valid

MCi_ADDR register valid

MCA: MEMORY CONTROLLER MS_CHANNEL0_ERR

Transaction: Memory scrubbing error

STATUS 8c000040000800c0 MCGSTATUS 0

MCGCAP 1000c14 APICID 20 SOCKETID 1

CPUID Vendor Intel Family 6 Model 45

Hardware event. This is not a software error.

MCE 4

CPU 8 BANK 8 

我们并不能根据CPU 8 BANK 8算出到底哪条DIMM出问题。可以让DSP用LiveCD运行以下命令确定。

echo \"ADDR xxxxxxx\" \| mcelog \--dmi \--ascii

![[Technology_ALL_案例分析[重要]_014_mcelog case summarize_004.png]]

 

 

5，最后

 

在做交换测试时候如果想马上验证mcelog是否还会出现信息，可直接在终端敲 mcelog ，此命令可马上进行一次检查，如问题没解决还会出现信息，建议敲多几次。另外需要注意的是手动删除 mcelog 文件后需要手动重启一下此服务，否则就算有日志信息也不会被记录。

 

已使用 OneNote 创建。
