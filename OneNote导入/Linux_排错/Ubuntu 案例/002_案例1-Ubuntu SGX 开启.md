案例1-Ubuntu SGX 开启

客户需要在 ESXI 上的为 Ubuntu 20.04 VM 开启 SGX 功能

 

SR: 194592582  \| ST: 4Z1Y014 \| R960

\
1. 首先需要开启硬件SGX功能，相关开启方法请看下面KB\
LKB ：Enable Intel SGX on 15G Intel PowerEdge servers

![[Ubuntu 案例_002_案例1-Ubuntu SGX 开启_001.png]]

 

\# 硬件开启需要注意内存有一定的插法，如内存不够或是其它原因下面是灰色的。

![[Ubuntu 案例_002_案例1-Ubuntu SGX 开启_002.png]]

 

 

2. VM上的修改在安全设备那些，勾选启动。

如果硬件没有开启成功，这个也无法勾选。Ubuntu 20.04

![[Ubuntu 案例_002_案例1-Ubuntu SGX 开启_003.png]]

 

3. 从VM系统查看

[ ]使用 cpuid 命令检查。

![[Ubuntu 案例_002_案例1-Ubuntu SGX 开启_004.png]]

看到 SGX1 supported 是 false 的也是说明没开启 SGX。

\# 如果没有 cpuid 命令，需要安装。 apt-get install cpuid

 

 

如果前面都设置好了，使用此命令可以看到 SGX1 supported 是 true

![[Ubuntu 案例_002_案例1-Ubuntu SGX 开启_005.png]]

 

使用 dmesg 可以看到已经开启。

![[Ubuntu 案例_002_案例1-Ubuntu SGX 开启_006.png]]

 

 

 

 

 

已使用 OneNote 创建。
