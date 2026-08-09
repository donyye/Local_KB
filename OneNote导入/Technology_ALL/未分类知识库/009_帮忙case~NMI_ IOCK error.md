帮忙case\~NMI: IOCK error

Thursday, February 20, 2014

4:16 PM

 

个人理解这个错误应该是不可中断的IO出现了中断情况，使用下文提示的方式开启IO不可中断设置：

 

\[root@localhost \~\]# vim /etc/sysctl.conf

 

kernel.panic_on_io_nmi = 1   à 加入这行

 

\[root@localhost \~\]# sysctl -p

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Hi dony

我找到Feb 18 12:35:21 CDP4 kernel: You probably have a hardware problem with your RAM chips

Feb 18 12:35:21 CDP4 kernel: NMI: IOCK error (debug interrupt?)

报错，下面文档有提到，这个能否帮忙确认一下

 

 

<https://access.redhat.com/site/solutions/42261>

 

\-\-\-\-\-\-\-\-\-\-\-\-\--

请1.Detail Symptom Descriptions-详细的故障现象描述：

用户机器死机黑屏无响应，可以ping通重启正常

2.Troubleshooting Setups-排错步骤：

收集dset没有硬件问题，用户redhat系统提示有错误

Feb 18 12:35:21 CDP4 kernel: You probably have a hardware problem with your RAM chips

Feb 18 12:35:21 CDP4 kernel: NMI: IOCK error (debug interrupt?)

3.Current status-当前状态:

用户重启后正常使用，要求更换所有内存上门测试,请L2帮忙确认是否需要做其他测试

4.Must Collect Logs-必须收集的日志 ;

dset

 

 

 

已使用 OneNote 创建。
