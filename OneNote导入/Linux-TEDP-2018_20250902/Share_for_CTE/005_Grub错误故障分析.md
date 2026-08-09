Grub错误故障分析

2022年4月29日

17:10

<https://blog.csdn.net/fanzhuozhuo/article/details/108503791>

模拟系统启动错误

\# cat test2.sh

echo \"fs.file-max=700\" \>\> /etc/sysctl.conf

 

这个设置是系统级别所有进程可以打开的文件描述符的数量限制。

 

 

 

 

1. 系统重启一直在转圈圈

![[Linux-TEDP-2018_2025_Share_for_CTE_005_Grub错误故障分析_001.png]]

 

2. 点击F4进入字符画面

发现一大堆服务启动时发生错误。

![[Linux-TEDP-2018_2025_Share_for_CTE_005_Grub错误故障分析_002.png]]

 

 

问题原因：

正常时打开的数量是 11136个，二系统默认限制是 375213个，所以如果限制到700个，那系统很多服务都无法启动，导致无法正常进入系统。

 

\[root@rh8a \~\]# cat /proc/sys/fs/file-nr

11136        0        375213

 

 

 

 

已使用 OneNote 创建。
