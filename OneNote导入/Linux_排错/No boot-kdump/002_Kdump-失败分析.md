Kdump-失败分析

2024年4月18日

8:46

 

vmcore 保持失败分析

 

设置 console 输出，可以查看当模拟crash时的一个具体输出情况。

1.  ssh 进入 idrac ， 运行命令 console com2。保存ssh的输出。

\# idrac系统密码是和root web 登录密码一样。

1.  重启机器，在grub kernel下加入 console=ttyS0,115200[  来启动]系统。

\# 115200 是使输出信息更快。

1.  成功后能看到整个开机输出过程。
2.  然后 echo c \> /proc/sysrq-trigger 触发crash

 

开机时编辑kernel添加 "console=ttyS0,115200" ，如下图。可以把 rhgb quiet 去掉看整个过程。

![[No boot-kdump_002_Kdump-失败分析_001.png]]

 

 

这个命令是查看到机器上有两张卡。

![[No boot-kdump_002_Kdump-失败分析_002.png]]

 

查看总共启动加载的内存是多少

lsmod  \| awk \'BEGIN  END\'

![[No boot-kdump_002_Kdump-失败分析_003.png]]

12568904 是占用了 12G的内存

 

运行lsscsi命令会显示每个SCSI设备的相关信息，包括设备路径、设备类型、厂商、型号等。

![[No boot-kdump_002_Kdump-失败分析_004.png]]

从输出看只有两个 PERC卡，没有链接存储设置。

如果有链接比如是这样：这个是SUSE15的输出

![[No boot-kdump_002_Kdump-失败分析_005.png]]

 

从关键字 Reserving 可以看到系统为 kdump 保留的内存大小。

![[No boot-kdump_002_Kdump-失败分析_006.png]]

 

 

从console输出可以发现OOM有出现qla2xxx,lpfc的驱动信息，所以开机启动时禁止这两个驱动的启动。

 

console=ttyS0,115200 rd.driver.blacklist=qla2xxx,lpfc

![[No boot-kdump_002_Kdump-失败分析_007.png]]

 

 

禁止这个两个驱动后发现crash就dump就没问题了，最后尝试先禁止其中要给比如 qla2xxx，然后再试 lpfc，看那个会出问题。

最后发现是 qla2xxx 驱动被加载，auto系统自己计算保留内存使用有问题。

 

总的来说 crash kdump 是独立的一个系统，有自己的内存，它的唯一的目的就是把内存dunp到硬盘里，加载 qla2xx 是完全没意义，因为kdump不用写到存储里去，如果它要写到存储里那就有意义，所以可以禁掉不要 kdump 去加载这个驱动。

 

=========================

后续是对qla2xxx的一些进一步的分析：

 

vmcore生产失败是由于qla2xxx驱动占用较多的内存而导致保留的内存不足而产生OOM

a. 禁用qla2xxx后能生成vmcore.

b. 增加保留内存到512MB也能生成vmcore.

 

实验室较少硬件，用VM带上Qlogic卡测试结果：

auto带上qla2xxx会小概率OOM, 此时的保留内存是162MB

降低12M（即150MB）带上qla2xxx会完全中OOM，可见此保留值接近所需值的边缘。

此VM模块总数是98个，占用内存5930006

[\[root@rhel79perc755 \~\]# ]lsmod \| wc -l

98

[\[root@rhel79perc755 \~\]# ]lsmod \| awk \'BEGIN  END\'

5930006

[\[root@rhel79perc755 \~\]#]

 

 

对比HP的sosreport，占用内存和VM差不多

[\[WSL20@ kernel\]\$] cat lsmod \| wc -l

76

[\[WSL20@ kernel\]\$] cat lsmod \| awk \'BEGIN  END\'

6505736

[\[WSL20@ kernel\]\$]

 

再对比R940，此服务器是14G最高端服务器，带硬件接口和所需要的驱动较多，如下，所需要内存是上面两个的近两倍，此值仅是驱动本身的最小所需值，实际需要预留更多。

[\[WSL20@ kernel\]\$] cat lsmod \| wc -l

90

[\[WSL20@ kernel\]\$] cat lsmod \| awk \'BEGIN  END\'

11744390

[\[WSL20@ kernel\]\$]

 

所以建议，对生产服务器的kdump kernel禁用qla2xxx和lpfc，设置crashkernel=512MB或更高

如何禁用，请参考KB：

<https://access.redhat.com/articles/5332081>

在/etc/sysconfig/kdump下的 KDUMP_COMMANDLINE_APPEND后面加上 " rd.driver.blacklist=qla2xxx,lpfc"，并重启kdump服务。然后开始测试。谢谢！

 

 

已使用 OneNote 创建。
