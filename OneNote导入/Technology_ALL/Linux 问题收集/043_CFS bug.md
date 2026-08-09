CFS bug

2018年12月10日

17:31

R730 不稳定问题

 

1. 客户这批R730使用的是CentOS 7.2，kernel的版本是3.10.0-327.28.3.el7.x86_64。此集群上跑的是新业务。

 

2. 通过对5台机器的crash dumps log分析，所有的问题都指向了CFS的调度问题。

 

1）以其中个dump log为例，通过获取当时出问题时的mem数据，发现在调用"update_cfs_shares"与一些其它的CFS调度函数时会出现异常的清楚。具体如下：

#0 \[ffff88103e9059c8\] machine_kexec at ffffffff81051e9b

#1 \[ffff88103e905a28\] crash_kexec at ffffffff810f27c2

#2 \[ffff88103e905af8\] panic at ffffffff8162fcee

#3 \[ffff88103e905b78\] watchdog_overflow_callback at ffffffff8111b9b2

#4 \[ffff88103e905b88\] \_\_perf_event_overflow at ffffffff8115f201

#5 \[ffff88103e905c00\] perf_event_overflow at ffffffff8115fcd4

#6 \[ffff88103e905c10\] intel_pmu_handle_irq at ffffffff810325d8

#7 \[ffff88103e905e60\] perf_event_nmi_handler at ffffffff8163fe8b

#8 \[ffff88103e905e80\] nmi_handle at ffffffff8163f5d9

#9 \[ffff88103e905ec8\] do_nmi at ffffffff8163f6f0

#10 \[ffff88103e905ef0\] end_repeat_nmi at ffffffff8163ea13

    \[exception RIP: update_cfs_shares+101\]

    RIP: ffffffff810c18d5  RSP: ffff88103e903db8  RFLAGS: 00000002

    RAX: 00000000000000bc  RBX: ffff881021fb0480  RCX: 00000000000038b3

    RDX: 0000000000000002  RSI: 00000000000009e9  RDI: ffff881023adb400

    RBP: ffff88103e903dd0   R8: 0000000000000000   R9: 0000000000000001

    R10: 00000000000001dc  R11: 0000000000000001  R12: ffff881022149600

    R13: ffff881016eed600  R14: 000000000000000e  R15: 0000000000000000

    ORIG_RAX: ffffffffffffffff  CS: 0010  SS: 0018

\-\-- \<NMI exception stack\> \-\--

#11 \[ffff88103e903db8\] update_cfs_shares at ffffffff810c18d5

#12 \[ffff88103e903dd8\] enqueue_entity at ffffffff810c3650

#13 \[ffff88103e903e20\] unthrottle_cfs_rq at ffffffff810c4414

#14 \[ffff88103e903e58\] distribute_cfs_runtime at ffffffff810c4662

#15 \[ffff88103e903ea0\] sched_cfs_period_timer at ffffffff810c4847

#16 \[ffff88103e903ed8\] \_\_hrtimer_run_queues at ffffffff810a9d82

#17 \[ffff88103e903f30\] hrtimer_interrupt at ffffffff810aa320

#18 \[ffff88103e903f80\] local_apic_timer_interrupt at ffffffff810495c7

#19 \[ffff88103e903f98\] smp_apic_timer_interrupt at ffffffff816490cf

#20 \[ffff88103e903fb0\] apic_timer_interrupt at ffffffff8164779d

\-\-- \<IRQ stack\> \-\--

#21 \[ffff88102718fda8\] apic_timer_interrupt at ffffffff8164779d

    \[exception RIP: cpuidle_enter_state+82\]

    RIP: ffffffff814d4f92  RSP: ffff88102718fe50  RFLAGS: 00000202

    RAX: 004d6b9c020a0179  RBX: 000000000000f920  RCX: 0000000000000018

    RDX: 0000000225c17d03  RSI: ffff88102718ffd8  RDI: 004d6b9c020a0179

    RBP: ffff88102718fe78   R8: 0000000000000103   R9: 0000000000000018

    R10: 00000000000001dc  R11: 0000000000000001  R12: 0000000000000000

    R13: 004d6b9c27c4ce40  R14: ffffffff810a9aa7  R15: ffff88102718fe20

    ORIG_RAX: ffffffffffffff10  CS: 0010  SS: 0018

#22 \[ffff88102718fe80\] cpuidle_idle_call at ffffffff814d50d9

#23 \[ffff88102718fec0\] arch_cpu_idle at ffffffff8101e4ee

#24 \[ffff88102718fed0\] cpu_startup_entry at ffffffff810d6485

#25 \[ffff88102718ff28\] start_secondary at ffffffff8104768a

 

2）通过这些函数的追踪，基本指向了"/kernel/sched/fair.c"。

/usr/src/debug/kernel-3.10.0-327.28.3.el7/linux-3.10.0-327.28.3.el7.x86_64/kernel/sched/fair.c: 2037

0xffffffff810c18d5 \<update_cfs_shares+101\>:     cmovae %rax,%rdx

 

3）目前我们并不知道客户的什么程序在使用这些函数时总会出现问题，而我们也通过一些Redhat KB做过这方面的查询，但是没发现相同的问题描述。

而我们在一起其它的开源论坛上看到了有相似的问题描述，通过patch的方式去解决。这些信息之前在IPS的邮件也提过。

[https://lkml.org/lkml/2018/7/27/1127](https://lkml.org/lkml/2018/7/27/1127)

 

3. 总结与建议

目前我们能通过多台系统crash dumps日志了解到问题都指向CFS问题，一些开源社区里也有人遇到过相同问题，但是那种patch的方法并不是一种可操作性很好的方法，所以建议客户可以通过更新kernel来达到对CFS更新的效果，看能否改善或是解决这个问题。

 

从一些开源的网站可以看到fair.c其它关键调度函数都一直有更新。

[https://github.com/torvalds/linux/tree/master/kernel/sched](https://github.com/torvalds/linux/tree/master/kernel/sched)

 

下面是对这些代码最近的一些改善，所以可尝试使用kernel更新的方法，建议升级到目前CentOS7最新版本的kernel。

![[Technology_ALL_Linux 问题收集_043_CFS bug_001.jpg]]

 

有其它问题可以继续讨论。

 

已使用 OneNote 创建。
