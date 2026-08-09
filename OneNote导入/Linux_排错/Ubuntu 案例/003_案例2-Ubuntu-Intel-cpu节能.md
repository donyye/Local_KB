案例2-Ubuntu-Intel-cpu节能

2024年8月2日

12:07

\
ST: 17YQZ24 \
\
目前通过我们的测试来看，与客户遇到的问题一致。

在 ubuntu 20.04 kernel 5.4.0-182 里，current_driver 是没有启动如何的 cpu idle driver。 而在kernel 5.15.0-107 里默认使用了 acpi_idle 用来控制CPU的功效。

 

另外我对比了一下其它的系统，比如 RHEL7 current_driver 也是没有启动如何 idle。而到了 RHEL8 RHEL9 也是默认使用了 acpi_idle 

下面KB是 Redhat 如何完全禁用 CPUidle

<https://access.redhat.com/solutions/6969204>

ubuntu 20.04 kernel 5.4.0-182

 

![[Ubuntu 案例_003_案例2-Ubuntu-Intel-cpu节能_001.jpg]]

 

kernel 5.15.0-107 

 

![[Ubuntu 案例_003_案例2-Ubuntu-Intel-cpu节能_002.jpg]]

 

总结是：

kernel 版本不同启用了不同的模块，但是在禁用了 acpi_idel后pstate有接管，所以要吧 intel_pstate 也关闭了才解决问题。

 

 

回看： 347LTJ3 

 

另外如果发现没办法锁定最高平率的时候，需要试试下面红色的部分的其中一个。

processor.max_cstate=1

intel_idle.max_cstate=0

intel_pstate=disable

idle=poll

idle=halt

Idle=nomwait

 

 

 

已使用 OneNote 创建。
