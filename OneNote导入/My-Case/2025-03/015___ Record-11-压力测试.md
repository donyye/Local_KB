|\_ Record-11-压力测试

 

使用SLI 的Stressapptest 对CPU/DIMM做简单的压力测试

 

SLI下载链接:

SLI 8.8： [https://dyson.okc.eerclab.dell.com/Share/Software/Dell/SLI/DEV/sli-8.8-rc12.iso](https://dyson.okc.eerclab.dell.com/Share/Software/Dell/SLI/DEV/sli-8.8-rc12.iso)

 

Stressapptest 测试方法如下:

1, 打开SLI命令行终端,切换root用户,(更改为 root 用户,然后输入密码dell)

 

![[My-Case_2025-03_015___ Record-11-压力测试_001.gif]]

2, 执行stressapptest 相关命令测试,例如执行如下命令测试30分钟

     stressapptest -s 1800 -M 240000 -m 10 -C 40 -W   

命令执行参考如下图

 

![[My-Case_2025-03_015___ Record-11-压力测试_002.gif]]

注意:

-M 根据客户服务器内存配置调整,不要超过服务器内存配置

-C 根据客户服务器配置的CPU线程数设置,不要超过总线程

 命令相关解释如下,可以根据现场实际情况增加测试时间和线程等.

 

![[My-Case_2025-03_015___ Record-11-压力测试_003.gif]]

 

开启 System Monitor

![[My-Case_2025-03_015___ Record-11-压力测试_004.png]]

 

3, 测试完成之后, 收集TSR日志查看是否有DIMM/CPU相关报错, 并且收集dmesg log确认是否有如下类似DIMM报错.

 

![[My-Case_2025-03_015___ Record-11-压力测试_005.gif]]

 \
循环测试 

for i in ; do stressapptest -s 600 -W; sleep 500; done

<https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P0000011s1tQAA/view>

# 跑100次 stressapptest ，然后每次 10 分钟 ，每次结束后停大概8分钟，然后再循环

 ================

 

 

1,F10下运行ePSA(advance)

[PowerEdge : How to Run Hardware Diagnostics on your Server | Dell India](https://www.dell.com/support/kbdoc/en-in/000132726/how-to-run-hardware-diagnostics-on-your-poweredge-server#PowerEdge)

2, SLI运行memtest86

3,多次运行CPU压测观察是否出现卡死或其他异常,LKB

3.1 Intel  DcDiag +业务环境

3.2 Intel DcDiag+ 压力模拟测试  (iperf+FIO) ,FIO测试注意数据先备份

3.3 Intel Dcdiag + stressapptest

 

附:

000228738

如何在sli-8.8上运行IPDT 诊断CPU

来自\<  [https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge\_\_kav/ka06P0000011UQrQAM/view](https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P0000011UQrQAM/view)\>

 

 

000139427

PowerEdge: How to test network bandwidth using 'iperf'

来自 \<  [https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge\_\_kav/ka06P0000011bWeQAI/view](https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P0000011bWeQAI/view)\>

 

000255501

Use SLI's Stressapptest to perform a simple stress test on the CPU/DIMM

来自 \<  [https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge\_\_kav/ka06P0000011ejaQAA/view](https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P0000011ejaQAA/view)\>

for i in ; do stressapptest -s 600 -W; sleep 500; done 

 

 

[如何在 PowerEdge 伺服器上執行硬體診斷 | Dell 台灣](https://www.dell.com/support/kbdoc/zh-tw/000132726/%E5%A6%82%E4%BD%95-%E5%9C%A8-poweredge-%E4%BC%BA%E6%9C%8D%E5%99%A8-%E4%B8%8A-%E5%9F%B7%E8%A1%8C-%E7%A1%AC%E9%AB%94-%E8%A8%BA%E6%96%B7?lwp=rt)

 

CPU stress-test using ePSA for Dell PowerEdge Server

来自 \< [https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge\_\_kav/ka06P000000YlmlQAC/view](https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P000000YlmlQAC/view)\>
