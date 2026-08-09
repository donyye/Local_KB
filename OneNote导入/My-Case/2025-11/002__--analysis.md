|--analysis

 

 

211       2  23  ffff964f54c98000  RU   0.0        0        0  [khungtaskd]

 

 

================

 

分析vmcore ，触发 panic 的是内核线程 khungtaskd，它是内核的 "hung task watchdog" 守护线程，它的任务是定期扫描所有进程，看有没有被卡在不可中断（D）状态而太久，所以当它发现有卡主太久的情况后触发了 panic。

 

从整个过程来看，很可能是有 CPU 状态进入了 intel_idle 模式而导致的问题。

 

建议在系统 kernel 选项添加下面设置后再观察一段时间，看是否有相同问题发生。

intel_idle.max_cstate=0 processor.max_cstate=0 idle=poll idle=halt

 

vmcore 分析如下：

crash\> sys

      KERNEL: /lib/modules/4.18.0-193.el8.x86_64/vmlinux

    DUMPFILE: 1108-2025-11-08-02:06:52/vmcore  [PARTIAL DUMP]

        CPUS: 32

        DATE: Sat Nov  8 02:06:34 CST 2025

      UPTIME: 2 days, 21:33:50

LOAD AVERAGE: 212.98, 136.15, 75.65

       TASKS: 1980

    NODENAME: OMCIMIMICAP01

     RELEASE: 4.18.0-193.el8.x86_64

     VERSION: #1 SMP Fri Mar 27 14:35:58 UTC 2020

     MACHINE: x86_64  (2100 Mhz)

      MEMORY: 31.6 GB

       PANIC: "Kernel panic - not syncing: hung_task: blocked tasks"

 

crash\> bt

PID: 211      TASK: ffff964f54c98000  CPU: 23   COMMAND: "khungtaskd"

 #0 [ffffb91e03d57d28] machine_kexec at ffffffffa0a5982e

 #1 [ffffb91e03d57d80] \_\_crash_kexec at ffffffffa0b58d8d

 #2 [ffffb91e03d57e48] panic at ffffffffa0ab1498

 #3 [ffffb91e03d57ec8] watchdog at ffffffffa0b8e0e7

 #4 [ffffb91e03d57f10] kthread at ffffffffa0ad4812

 #5 [ffffb91e03d57f50] ret_from_fork at ffffffffa140023f

......

 

 

PID: 0        TASK: ffff964d05658000  CPU: 4    COMMAND: "swapper/4"

 #0 [fffffe00000b3e50] crash_nmi_callback at ffffffffa0a4ce93

 #1 [fffffe00000b3e58] nmi_handle at ffffffffa0a22893

 #2 [fffffe00000b3eb0] default_do_nmi at ffffffffa0a22d4e

 #3 [fffffe00000b3ed0] do_nmi at ffffffffa0a22f28

 #4 [fffffe00000b3ef0] end_repeat_nmi at ffffffffa14016b4

    [exception RIP: intel_idle+133]

    RIP: ffffffffa12949c5  RSP: ffffb91e002d7e48  RFLAGS: 00000046

    RAX: 0000000000000001  RBX: 0000000000000001  RCX: 0000000000000001

    RDX: 0000000000000000  RSI: ffffffffa1d2a540  RDI: 0000000000000004

    RBP: ffff96506fcb3d68   R8: 0000000000000002   R9: 0000000000029480

    R10: 0001e5f2a1798e3e  R11: ffff96506fca8bc0  R12: 0000000000000002

    R13: ffffffffa1d2a618  R14: 0000000000000002  R15: 0000000000000000

    ORIG_RAX: ffffffffffffffff  CS: 0010  SS: 0018

--- \<NMI exception stack\> ---

 #5 [ffffb91e002d7e48] intel_idle at ffffffffa12949c5

 #6 [ffffb91e002d7e60] cpuidle_enter_state at ffffffffa10a6fd3

 #7 [ffffb91e002d7eb0] cpuidle_enter at ffffffffa10a73cc

 #8 [ffffb91e002d7ed0] do_idle at ffffffffa0ae67e6

 #9 [ffffb91e002d7f10] cpu_startup_entry at ffffffffa0ae6a0f

#10 [ffffb91e002d7f30] start_secondary at ffffffffa0a4ea17

#11 [ffffb91e002d7f50] secondary_startup_64 at ffffffffa0a000e7

......

 

多个进程已经是D状态:

crash\> ps -a

     1108       1   2  ffff96506b76bd80  UN   0.1   106416    25792  systemd-journal

     1529       2   4  ffff96546718bd80  UN   0.0        0        0  [xfsaild/dm-3]

     4033    3778  11  ffff964fd1209ec0  UN   0.5  4257692   164696  bdsecd

    13928    1665  26  ffff965444b20000  UN   2.4  3649892   870896  s1-agent

   116674       2   4  ffff964ff09b9ec0  UN   0.0        0        0  [kworker/u65:4]

   116687   79397   0  ffff964e32bce000  UN   5.2  3836200  1852176  default task-30

   116702   79397  16  ffff964dead52000  UN   5.2  3836200  1852176  default task-31

   116742   79397   6  ffff964f02cfc000  UN   5.2  3836200  1852176  default task-31

   116769   79397   8  ffff964ea4cb4000  UN   5.2  3836200  1852176  default task-31

   116784   79397  18  ffff964fea345c40  UN   5.2  3836200  1852176  default task-31

   116845   79397  14  ffff964faa150000  UN   5.2  3836200  1852176  default task-32

   116860   79397   6  ffff964e9dea3d80  UN   5.2  3836200  1852176  default task-32

   116898   79397  28  ffff964d5c72dc40  UN   5.2  3836200  1852176  default task-32

   116951   79397   6  ffff964f46679ec0  UN   5.2  3836200  1852176  default task-33

   117068    2301  15  ffff964f5728bd80  UN   2.2  6388848   792068  tp:unixsock

   117069    2301  22  ffff964f57288000  UN   2.2  6388848   792068  tp:unixsock

......

   117217   79397   8  ffff964eec219ec0  UN   5.2  3836200  1852176  default task-34

   117218   79397  28  ffff964e51515c40  UN   5.2  3836200  1852176  default task-34

   117219   79397  14  ffff964e9dea1ec0  UN   5.2  3836200  1852176  default task-34

   117220   79397  20  ffff964f58821ec0  UN   5.2  3836200  1852176  default task-34

   117223   79397  30  ffff964e9dea0000  UN   5.2  3836200  1852176  default task-34

   117224   79397  26  ffff964faa155c40  UN   5.2  3836200  1852176  default task-34

   117225   79397  20  ffff964faa151ec0  UN   5.2  3836200  1852176  default task-34
