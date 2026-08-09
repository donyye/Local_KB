！！！！kdump

SST: 188845223

![[My-Case_2024-04_010_！！！！kdump_001.png]]

 

 

![[My-Case_2024-04_010_！！！！kdump_002.png]]

 

 

[    0.000000] Reserving 168MB of memory at 720MB for crashkernel (System RAM: 130524MB)

 

 

 

 

![[My-Case_2024-04_010_！！！！kdump_003.png]]

 

 

 

1.  ssh 进入 idrac ， 运行命令 console com2
2.  你稍后重启机器，在grub kernel下加入 console=ttyS0 来启动
3.  这时候第1步的console会有输出， 
4.  然后 echo c \> /proc/sysrq-trigger 触发crash

 

![[My-Case_2024-04_010_！！！！kdump_004.png]]

 

 

lsmod  | awk 'BEGIN  END'

![[My-Case_2024-04_010_！！！！kdump_005.png]]

 

![[My-Case_2024-04_010_！！！！kdump_006.png]]

 

 

![[My-Case_2024-04_010_！！！！kdump_007.png]]

 

 

 

console=ttyS0,115200 rd.driver.blacklist=qla2xxx,lpfc

![[My-Case_2024-04_010_！！！！kdump_008.png]]

 

 

![[My-Case_2024-04_010_！！！！kdump_009.png]]

 

 

 

 

 

 

[11:50] Huang, Antti

和HP的对比，Dell的server有对qla的驱动参数更改，导致qla会被kdump kernel加载。而HP没有下面这个文件，所以kdump kernel不会加载qla2xxx，也就能每次都成功，这应该就是root cause.

$ cat etc/modprobe.d/nxupmodules.conf

###UltraPath append options begin,don't change this###

options qla2xxx qlport_down_retry=5

options lpfc lpfc_nodev_tmo=5

###UltraPath append options end,don't change this###

 

[11:51] Huang, Antti

我在lab测试过，如果没有这个修改的话，默认kdump kernel是不会加载qla2xxx的，这样也就不会有OOM的问题

 

 

 

 

 

 

如昨天测试，以下是结论：

vmcore生产失败是由于qla2xxx驱动占用较多的内存而导致保留的内存不足而产生OOM

a. 禁用qla2xxx后能生成vmcore.

b. 增加保留内存到512MB也能生成vmcore.

 

实验室较少硬件，用VM带上Qlogic卡测试结果：

auto带上qla2xxx会小概率OOM, 此时的保留内存是162MB

降低12M（即150MB）带上qla2xxx会完全中OOM，可见此保留值接近所需值的边缘。

此VM模块总数是98个，占用内存5930006

[[root@rhel79perc755 ~]# ]lsmod | wc -l

98

[[root@rhel79perc755 ~]# ]lsmod | awk 'BEGIN  END'

5930006

[[root@rhel79perc755 ~]#]

 

 

对比HP的sosreport，占用内存和VM差不多

[[WSL20@ kernel]$] cat lsmod | wc -l

76

[[WSL20@ kernel]$] cat lsmod | awk 'BEGIN  END'

6505736

[[WSL20@ kernel]$]

 

再对比R940，此服务器是14G最高端服务器，带硬件接口和所需要的驱动较多，如下，所需要内存是上面两个的近两倍，此值仅是驱动本身的最小所需值，实际需要预留更多。

[[WSL20@ kernel]$] cat lsmod | wc -l

90

[[WSL20@ kernel]$] cat lsmod | awk 'BEGIN  END'

11744390

[[WSL20@ kernel]$]

 

所以建议，对生产服务器的kdump kernel禁用qla2xxx和lpfc，设置crashkernel=512MB或更高

如何禁用，请参考KB：

<https://access.redhat.com/articles/5332081>

在/etc/sysconfig/kdump下的 KDUMP_COMMANDLINE_APPEND后面加上 " rd.driver.blacklist=qla2xxx,lpfc"，并重启kdump服务。然后开始测试。谢谢！
