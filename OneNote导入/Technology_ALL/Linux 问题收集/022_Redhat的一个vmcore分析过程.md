Redhat的一个vmcore分析过程

Thursday, September 01, 2016

5:15 PM

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       (WoC) (SEV 3) 问题单 #01692732 (系统日志中报出大量的硬件异常日志，需要协助判定问题；) ref:\_00DA0HxWH.\_500A0V7Ol4:ref
  发件人     [Red Hat Support](mailto:support@redhat.com)
  收件人     王懿; 虞晟玺（Sage Yu）; 周炯梁
  发送时间   Thursday, September 01, 2016 11:17 AM
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\| 问题单信息 \|

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

<https://access.redhat.com/support/cases/#/case/01692732>

问题单标题 : 系统日志中报出大量的硬件异常日志，需要协助判定问题；

问题单号 : 01692732

问题单创建日期 : 2016-08-29 16:20:15

严重性 : 3 (Normal)

 

最新评论: 在 2016-09-01 11:17:45, Kwon, Daniel 评论:

\"Hi,

 

This is Daniel Kwon from Kernel team. I\'m working with the case owner to support your case.

 

From the vmcore, it shows that the system was crashed as a process was running on a CPU for more than 60 seconds.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

KERNEL: /cores/retrace/repos/kernel/x86_64/usr/lib/debug/lib/modules/2.6.32-431.el6.x86_64/vmlinux

DUMPFILE: /cores/retrace/tasks/439793897/crash/vmcore \[PARTIAL DUMP\]

CPUS: 48

DATE: Mon Aug 29 05:51:39 2016

UPTIME: 48 days, 21:31:28

LOAD AVERAGE: 29.04, 30.07, 30.27

TASKS: 1156

NODENAME: h-lp-ps-325

RELEASE: 2.6.32-431.el6.x86_64

VERSION: #1 SMP Sun Nov 10 22:19:54 EST 2013

MACHINE: x86_64 (2600 Mhz)

MEMORY: 255.9 GB

PANIC: \"Kernel panic - not syncing: Watchdog detected hard LOCKUP on cpu 30\"

PID: 139801

COMMAND: \"oracle\"

TASK: ffff883bd3cee040 \[THREAD_INFO: ffff8821463b0000\]

CPU: 30

STATE: TASK_RUNNING (PANIC)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

From the log, it shows that the process 139801 took longer than 60 seconds to finish it\'s job.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

oracle: page allocation failure. order:1, mode:0x20

Pid: 89930, comm: oracle Not tainted 2.6.32-431.el6.x86_64 #1

Call Trace:

\<IRQ\> \[\<ffffffff8112f9e7\>\] ? \_\_alloc_pages_nodemask+0x757/0x8d0

\[\<ffffffff8116e482\>\] ? kmem_getpages+0x62/0x170

\[\<ffffffff8116f09a\>\] ? fallback_alloc+0x1ba/0x270

\[\<ffffffff8116eaef\>\] ? cache_grow+0x2cf/0x320

\[\<ffffffff8116ee19\>\] ? \_\_\_\_cache_alloc_node+0x99/0x160

\[\<ffffffff8116fd9b\>\] ? kmem_cache_alloc+0x11b/0x190

\[\<ffffffff8144c1b8\>\] ? sk_prot_alloc+0x48/0x1c0

\[\<ffffffff8144d3c2\>\] ? sk_clone+0x22/0x2e0

\[\<ffffffff8149ebf6\>\] ? inet_csk_clone+0x16/0xd0

\[\<ffffffff814b84c3\>\] ? tcp_create_openreq_child+0x23/0x470

\[\<ffffffff814b5c7d\>\] ? tcp_v4_syn_recv_sock+0x4d/0x310

\[\<ffffffff814b8266\>\] ? tcp_check_req+0x226/0x460

\[\<ffffffff814ad7f6\>\] ? tcp_rcv_state_process+0x126/0xa30

\[\<ffffffff814b56bb\>\] ? tcp_v4_do_rcv+0x35b/0x490

\[\<ffffffff81086274\>\] ? mod_timer+0x144/0x220

\[\<ffffffff814b6f4a\>\] ? tcp_v4_rcv+0x51a/0x900

\[\<ffffffff8105a625\>\] ? select_idle_sibling+0x95/0x150

\[\<ffffffff8149427d\>\] ? ip_local_deliver_finish+0xdd/0x2d0

\[\<ffffffff81494508\>\] ? ip_local_deliver+0x98/0xa0

\[\<ffffffff814939cd\>\] ? ip_rcv_finish+0x12d/0x440

\[\<ffffffff81493f55\>\] ? ip_rcv+0x275/0x350

\[\<ffffffff8145b54b\>\] ? \_\_netif_receive_skb+0x4ab/0x750

\[\<ffffffff8145f1b8\>\] ? netif_receive_skb+0x58/0x60

\[\<ffffffff8145f2c0\>\] ? napi_skb_finish+0x50/0x70

\[\<ffffffff81460a29\>\] ? napi_gro_receive+0x39/0x50

\[\<ffffffffa01351d1\>\] ? igb_poll+0x9a1/0x1020 \[igb\]

\[\<ffffffff81460b43\>\] ? net_rx_action+0x103/0x2f0

\[\<ffffffff8107a8e1\>\] ? \_\_do_softirq+0xc1/0x1e0

\[\<ffffffff810e6ec0\>\] ? handle_IRQ_event+0x60/0x170

\[\<ffffffff8107a93f\>\] ? \_\_do_softirq+0x11f/0x1e0

\[\<ffffffff8100c30c\>\] ? call_softirq+0x1c/0x30

\[\<ffffffff8100fa75\>\] ? do_softirq+0x65/0xa0

\[\<ffffffff8107a795\>\] ? irq_exit+0x85/0x90

\[\<ffffffff81530fe5\>\] ? do_IRQ+0x75/0xf0

\[\<ffffffff8100b9d3\>\] ? ret_from_intr+0x0/0x11

\<EOI\>

Kernel panic - not syncing: Watchdog detected hard LOCKUP on cpu 30

Pid: 139801, comm: oracle Not tainted 2.6.32-431.el6.x86_64 #1

Call Trace:

\<NMI\> \[\<ffffffff815271fa\>\] ? panic+0xa7/0x16f

\[\<ffffffff810153a3\>\] ? native_sched_clock+0x13/0x80

\[\<ffffffff810e697d\>\] ? watchdog_overflow_callback+0xcd/0xd0

\[\<ffffffff8111c857\>\] ? \_\_perf_event_overflow+0xa7/0x240

\[\<ffffffff8101d93d\>\] ? x86_perf_event_set_period+0xdd/0x170

\[\<ffffffff8111ce24\>\] ? perf_event_overflow+0x14/0x20

\[\<ffffffff81022d87\>\] ? intel_pmu_handle_irq+0x187/0x2f0

\[\<ffffffff8152cee6\>\] ? kprobe_exceptions_notify+0x16/0x430

\[\<ffffffff8152ba59\>\] ? perf_event_nmi_handler+0x39/0xb0

\[\<ffffffff8152d515\>\] ? notifier_call_chain+0x55/0x80

\[\<ffffffff8152d57a\>\] ? atomic_notifier_call_chain+0x1a/0x20

\[\<ffffffff810a154e\>\] ? notify_die+0x2e/0x30

\[\<ffffffff8152b1db\>\] ? do_nmi+0x1bb/0x340

\[\<ffffffff8152aaa0\>\] ? nmi+0x20/0x30

\[\<ffffffff8152a202\>\] ? \_spin_lock_irqsave+0x32/0x40

\<\<EOE\>\> \<IRQ\> \[\<ffffffff81058d32\>\] ? \_\_wake_up+0x32/0x70

\[\<ffffffff811386de\>\] ? wakeup_kswapd+0xce/0x130

\[\<ffffffff8112f92f\>\] ? \_\_alloc_pages_nodemask+0x69f/0x8d0

\[\<ffffffff8116e482\>\] ? kmem_getpages+0x62/0x170

\[\<ffffffff8116f09a\>\] ? fallback_alloc+0x1ba/0x270

\[\<ffffffff8116eaef\>\] ? cache_grow+0x2cf/0x320

\[\<ffffffff8116ee19\>\] ? \_\_\_\_cache_alloc_node+0x99/0x160

\[\<ffffffff814551ae\>\] ? sk_stream_kill_queues+0x4e/0x120

\[\<ffffffff8116fd9b\>\] ? kmem_cache_alloc+0x11b/0x190

\[\<ffffffff8144c1b8\>\] ? sk_prot_alloc+0x48/0x1c0

\[\<ffffffff8144d3c2\>\] ? sk_clone+0x22/0x2e0

\[\<ffffffff8149ebf6\>\] ? inet_csk_clone+0x16/0xd0

\[\<ffffffff814b84c3\>\] ? tcp_create_openreq_child+0x23/0x470

\[\<ffffffff814b5c7d\>\] ? tcp_v4_syn_recv_sock+0x4d/0x310

\[\<ffffffff814b8266\>\] ? tcp_check_req+0x226/0x460

\[\<ffffffff814ad7f6\>\] ? tcp_rcv_state_process+0x126/0xa30

\[\<ffffffff814b56bb\>\] ? tcp_v4_do_rcv+0x35b/0x490

\[\<ffffffff8146074f\>\] ? dev_queue_xmit+0x11f/0x320

\[\<ffffffff814b6f4a\>\] ? tcp_v4_rcv+0x51a/0x900

\[\<ffffffff8105a625\>\] ? select_idle_sibling+0x95/0x150

\[\<ffffffff8149427d\>\] ? ip_local_deliver_finish+0xdd/0x2d0

\[\<ffffffff81494508\>\] ? ip_local_deliver+0x98/0xa0

\[\<ffffffff814939cd\>\] ? ip_rcv_finish+0x12d/0x440

\[\<ffffffff81493f55\>\] ? ip_rcv+0x275/0x350

\[\<ffffffff8145b54b\>\] ? \_\_netif_receive_skb+0x4ab/0x750

\[\<ffffffff8145f1b8\>\] ? netif_receive_skb+0x58/0x60

\[\<ffffffff8145f2c0\>\] ? napi_skb_finish+0x50/0x70

\[\<ffffffff81460a29\>\] ? napi_gro_receive+0x39/0x50

\[\<ffffffffa01351d1\>\] ? igb_poll+0x9a1/0x1020 \[igb\]

\[\<ffffffff81460b43\>\] ? net_rx_action+0x103/0x2f0

\[\<ffffffff8107a8e1\>\] ? \_\_do_softirq+0xc1/0x1e0

\[\<ffffffff810e6ec0\>\] ? handle_IRQ_event+0x60/0x170

\[\<ffffffff8107a93f\>\] ? \_\_do_softirq+0x11f/0x1e0

\[\<ffffffff8100c30c\>\] ? call_softirq+0x1c/0x30

\[\<ffffffff8100fa75\>\] ? do_softirq+0x65/0xa0

\[\<ffffffff8107a795\>\] ? irq_exit+0x85/0x90

\[\<ffffffff81530fe5\>\] ? do_IRQ+0x75/0xf0

\[\<ffffffff8100b9d3\>\] ? ret_from_intr+0x0/0x11

\<EOI\> \[\<ffffffff8152a311\>\] ? \_spin_lock+0x21/0x30

\[\<ffffffff81154e42\>\] ? page_referenced+0xa2/0x360

\[\<ffffffff811397c7\>\] ? isolate_lru_pages.clone.0+0xd7/0x170

\[\<ffffffff81139a41\>\] ? shrink_active_list+0x1e1/0x370

\[\<ffffffff8113a7f5\>\] ? shrink_mem_cgroup_zone+0x3f5/0x610

\[\<ffffffff81179ffd\>\] ? mem_cgroup_iter+0xfd/0x280

\[\<ffffffff8113aa73\>\] ? shrink_zone+0x63/0xb0

\[\<ffffffff8113b661\>\] ? zone_reclaim+0x331/0x650

\[\<ffffffff8112d83c\>\] ? get_page_from_freelist+0x6ac/0x870

\[\<ffffffff8111f73e\>\] ? find_get_page+0x1e/0xa0

\[\<ffffffff8113ed80\>\] ? shmem_getpage_gfp+0xc0/0x610

\[\<ffffffff8112f3a3\>\] ? \_\_alloc_pages_nodemask+0x113/0x8d0

\[\<ffffffff8111fa47\>\] ? unlock_page+0x27/0x30

\[\<ffffffff8114a499\>\] ? \_\_do_fault+0x469/0x530

\[\<ffffffff8114a657\>\] ? handle_pte_fault+0xf7/0xb00

\[\<ffffffff81167a9a\>\] ? alloc_pages_current+0xaa/0x110

\[\<ffffffff8104ee9b\>\] ? pte_alloc_one+0x1b/0x50

\[\<ffffffff81146412\>\] ? \_\_pte_alloc+0x32/0x160

\[\<ffffffff8114b220\>\] ? handle_mm_fault+0x1c0/0x300

\[\<ffffffff8104a8d8\>\] ? \_\_do_page_fault+0x138/0x480

\[\<ffffffff8105dfbd\>\] ? thread_group_times+0x3d/0x120

\[\<ffffffff8106f1de\>\] ? mmput+0x1e/0x120

\[\<ffffffff810912b8\>\] ? getrusage+0x158/0x340

\[\<ffffffff8152d45e\>\] ? do_page_fault+0x3e/0xa0

\[\<ffffffff8152a815\>\] ? page_fault+0x25/0x30

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Above is showing that this process had busy moment as it had to allocate page for the process and while trying to find a page, it got a packet over the network which is handled by module \'igb\'. The problem is it also had to allocate page for the packet.

 

There were huge number of tasks awaiting to take a spinlock for pages and runqueue. The only processes that don\'t blocked in spinlock were below two processes which was in \'flush_tlb_others_ipi()\'.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

PID: 3943 TASK: ffff88204cdceaa0 CPU: 5 COMMAND: \"zabbix_agentd\"

#0 \[ffff8820f0c47e90\] crash_nmi_callback at ffffffff8102fee6

#1 \[ffff8820f0c47ea0\] notifier_call_chain at ffffffff8152d515

#2 \[ffff8820f0c47ee0\] atomic_notifier_call_chain at ffffffff8152d57a

#3 \[ffff8820f0c47ef0\] notify_die at ffffffff810a154e

#4 \[ffff8820f0c47f20\] do_nmi at ffffffff8152b1db

#5 \[ffff8820f0c47f50\] nmi at ffffffff8152aaa0

\[exception RIP: flush_tlb_others_ipi+288\]

RIP: ffffffff8104fa60 RSP: ffff882050663de8 RFLAGS: 00000246

RAX: 0000000000000000 RBX: 0000000000000b40 RCX: 0000000000000030

RDX: 0000000000000000 RSI: 0000000000000090 RDI: ffffffff81e274d8

RBP: ffff882050663e28 R8: 0000000000000002 R9: 0000000000000080

R10: 0000000000000000 R11: 0000000000000206 R12: ffffffff81e274d8

R13: 0000000000000005 R14: ffffffff81e274d0 R15: ffff88204cf1c8c8

ORIG_RAX: ffffffffffffffff CS: 0010 SS: 0018

\-\-- \<NMI exception stack\> \-\--

#6 \[ffff882050663de8\] flush_tlb_others_ipi at ffffffff8104fa60

#7 \[ffff882050663e30\] native_flush_tlb_others at ffffffff8104fae6

#8 \[ffff882050663e60\] flush_tlb_mm at ffffffff8104fcbc

#9 \[ffff882050663e80\] unmap_region at ffffffff8115022f

#10 \[ffff882050663ef0\] do_munmap at ffffffff81150966

#11 \[ffff882050663f50\] sys_munmap at ffffffff81150ab6

#12 \[ffff882050663f80\] system_call_fastpath at ffffffff8100b072

RIP: 00007f4fe1134347 RSP: 00007fffc2d64258 RFLAGS: 00000212

RAX: 000000000000000b RBX: ffffffff8100b072 RCX: 000000000000001b

RDX: 0000000000000000 RSI: 0000000000001000 RDI: 00007f4fe2559000

RBP: 0000000000000000 R8: 00007f4fe253c7c0 R9: 0000000000000000

R10: 3438333120363436 R11: 0000000000000206 R12: 000000000043952a

R13: 00007fffc2d64f50 R14: 0000000000000000 R15: 000000000087e2b0

ORIG_RAX: 000000000000000b CS: 0033 SS: 002b

 

PID: 140316 TASK: ffff882101dbeaa0 CPU: 20 COMMAND: \"oracle\"

#0 \[ffff88011fd47e90\] crash_nmi_callback at ffffffff8102fee6

#1 \[ffff88011fd47ea0\] notifier_call_chain at ffffffff8152d515

#2 \[ffff88011fd47ee0\] atomic_notifier_call_chain at ffffffff8152d57a

#3 \[ffff88011fd47ef0\] notify_die at ffffffff810a154e

#4 \[ffff88011fd47f20\] do_nmi at ffffffff8152b1db

#5 \[ffff88011fd47f50\] nmi at ffffffff8152aaa0

\[exception RIP: flush_tlb_others_ipi+288\]

RIP: ffffffff8104fa60 RSP: ffff883ad675b3b8 RFLAGS: 00000246

RAX: 0000000000000000 RBX: 0000000000000900 RCX: 0000000000000030

RDX: 0000000000000000 RSI: 0000000000000090 RDI: ffffffff81e27298

RBP: ffff883ad675b3f8 R8: 0000000000000002 R9: 0000000000000080

R10: 0000000000000000 R11: 0000000000000000 R12: ffffffff81e27298

R13: 0000000000000004 R14: ffffffff81e27290 R15: ffff88014b5ec348

ORIG_RAX: ffffffffffffffff CS: 0010 SS: 0018

\-\-- \<NMI exception stack\> \-\--

#6 \[ffff883ad675b3b8\] flush_tlb_others_ipi at ffffffff8104fa60

#7 \[ffff883ad675b400\] native_flush_tlb_others at ffffffff8104fae6

#8 \[ffff883ad675b430\] flush_tlb_page at ffffffff8104fc0e

#9 \[ffff883ad675b460\] ptep_clear_flush_young at ffffffff8104eb50

#10 \[ffff883ad675b490\] page_referenced_one at ffffffff811544e4

#11 \[ffff883ad675b4f0\] page_referenced at ffffffff81154ee8

#12 \[ffff883ad675b5a0\] shrink_active_list at ffffffff81139a41

#13 \[ffff883ad675b660\] shrink_mem_cgroup_zone at ffffffff8113a74a

#14 \[ffff883ad675b730\] shrink_zone at ffffffff8113aa73

#15 \[ffff883ad675b7a0\] zone_reclaim at ffffffff8113b661

#16 \[ffff883ad675b8c0\] get_page_from_freelist at ffffffff8112d83c

#17 \[ffff883ad675b9f0\] \_\_alloc_pages_nodemask at ffffffff8112f3a3

#18 \[ffff883ad675bb30\] alloc_pages_current at ffffffff81167a9a

#19 \[ffff883ad675bb60\] \_\_page_cache_alloc at ffffffff8111fd57

#20 \[ffff883ad675bb90\] \_\_do_page_cache_readahead at ffffffff811354cb

#21 \[ffff883ad675bc20\] ra_submit at ffffffff81135621

#22 \[ffff883ad675bc30\] ondemand_readahead at ffffffff81135995

#23 \[ffff883ad675bc90\] page_cache_async_readahead at ffffffff81135b50

#24 \[ffff883ad675bce0\] generic_file_aio_read at ffffffff81121613

#25 \[ffff883ad675bdc0\] do_sync_read at ffffffff81188dba

#26 \[ffff883ad675bef0\] vfs_read at ffffffff811896a5

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

From the task 3943, I\'ve checked which CPUs it was awaiting to be cleared.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

crash\> dis -lr ffffffff8104fae6 \| tail

0xffffffff8104fad0 \<native_flush_tlb_others+96\>: leaveq

0xffffffff8104fad1 \<native_flush_tlb_others+97\>: retq

0xffffffff8104fad2 \<native_flush_tlb_others+98\>: nopw 0x0(%rax,%rax,1)

/usr/src/debug/kernel-2.6.32-431.el6/linux-2.6.32-431.el6.x86_64/arch/x86/mm/tlb.c: 224

0xffffffff8104fad8 \<native_flush_tlb_others+104\>: mov %r12,%rdx

0xffffffff8104fadb \<native_flush_tlb_others+107\>: mov %rbx,%rsi

0xffffffff8104fade \<native_flush_tlb_others+110\>: mov %r13,%rdi

0xffffffff8104fae1 \<native_flush_tlb_others+113\>: callq 0xffffffff8104f940 \<flush_tlb_others_ipi\>

 

crash\> dis -lr ffffffff8104fa60 \| head

/usr/src/debug/kernel-2.6.32-431.el6/linux-2.6.32-431.el6.x86_64/arch/x86/mm/tlb.c: 177

0xffffffff8104f940 \<flush_tlb_others_ipi\>: push %rbp

0xffffffff8104f941 \<flush_tlb_others_ipi+1\>: mov %rsp,%rbp

0xffffffff8104f944 \<flush_tlb_others_ipi+4\>: sub \$0x40,%rsp

0xffffffff8104f948 \<flush_tlb_others_ipi+8\>: mov %rbx,-0x28(%rbp)

0xffffffff8104f94c \<flush_tlb_others_ipi+12\>: mov %r12,-0x20(%rbp)

0xffffffff8104f950 \<flush_tlb_others_ipi+16\>: mov %r13,-0x18(%rbp)

0xffffffff8104f954 \<flush_tlb_others_ipi+20\>: mov %r14,-0x10(%rbp)

0xffffffff8104f958 \<flush_tlb_others_ipi+24\>: mov %r15,-0x8(%rbp)

0xffffffff8104f95c \<flush_tlb_others_ipi+28\>: nopl 0x0(%rax,%rax,1)

 

#6 \[ffff882050663de8\] flush_tlb_others_ipi at ffffffff8104fa60

ffff882050663df0: ffffffff81e274c0 00000000ffffffff

ffff882050663e00: ffff88204cf1c600 ffffffffffffffff

ffff882050663e10: ffff88204cf1c8c8 ffff88204cf1c600

ffff882050663e20: 00007f4fe2559000 ffff882050663e58

ffff882050663e30: ffffffff8104fae6

#7 \[ffff882050663e30\] native_flush_tlb_others at ffffffff8104fae6

 

crash\> px 0xffff882050663e30-0x8

\$4 = 0xffff882050663e28

crash\> px 0xffff882050663e28-0x18

\$5 = 0xffff882050663e10

crash\> rd 0xffff882050663e10

ffff882050663e10: ffff88204cf1c8c8 \...L \...

crash\> cpumask ffff88204cf1c8c8

struct cpumask {

bits = 

}

crash\> eval -b 8796093022240

hexadecimal: 80000000020

decimal: 8796093022240

octal: 200000000000040

binary: 0000000000000000000010000000000000000000000000000000000000100000

bits set: 43 5

 

crash\> pd 43%8

\$6 = 3

crash\> p flush_state\[3\].flush_cpumask

\$7 =

 

crash\> p flush_state\[5\].flush_cpumask

\$8 =

crash\> eval -b 8796093022208

hexadecimal: 80000000000 (8192GB)

decimal: 8796093022208

octal: 200000000000000

binary: 0000000000000000000010000000000000000000000000000000000000000000

bits set: 43

 

crash\> bt 175

PID: 175 TASK: ffff88205333c040 CPU: 43 COMMAND: \"migration/43\"

#0 \[ffff8820f0ea7e90\] crash_nmi_callback at ffffffff8102fee6

#1 \[ffff8820f0ea7ea0\] notifier_call_chain at ffffffff8152d515

#2 \[ffff8820f0ea7ee0\] atomic_notifier_call_chain at ffffffff8152d57a

#3 \[ffff8820f0ea7ef0\] notify_die at ffffffff810a154e

#4 \[ffff8820f0ea7f20\] do_nmi at ffffffff8152b1db

#5 \[ffff8820f0ea7f50\] nmi at ffffffff8152aaa0

\[exception RIP: \_spin_lock+30\]

RIP: ffffffff8152a30e RSP: ffff882053347d30 RFLAGS: 00000097

RAX: 000000000000139f RBX: ffff8820f0eb6840 RCX: 0000000000000000

RDX: 0000000000001394 RSI: ffff88011fd76840 RDI: ffff88011fd76840

RBP: ffff882053347d30 R8: ffff88205338a400 R9: 0000000000000000

R10: 0000000000000000 R11: 00000000000e7ef0 R12: ffff88011fd76840

R13: 000000000000002b R14: 0000000000000016 R15: ffff88011fd76840

ORIG_RAX: ffffffffffffffff CS: 0010 SS: 0018

\-\-- \<NMI exception stack\> \-\--

#6 \[ffff882053347d30\] \_spin_lock at ffffffff8152a30e

#7 \[ffff882053347d38\] double_lock_balance at ffffffff81058e7a

#8 \[ffff882053347d58\] pull_rt_task at ffffffff81067c04

#9 \[ffff882053347da8\] pre_schedule_rt at ffffffff81067db5

#10 \[ffff882053347db8\] schedule at ffffffff81527642

#11 \[ffff882053347e78\] migration_thread at ffffffff81068635

#12 \[ffff882053347ee8\] kthread at ffffffff8109aef6

#13 \[ffff882053347f48\] kernel_thread at ffffffff8100c20a

 

crash\> runq \| grep ffff88011fd76840

CPU 22 RUNQUEUE: ffff88011fd76840

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

So, it was awaiting CPU 43 to be cleared which was trying to get a lock for runqueue 22. It was same on the second process. So, all ended up on CPU 22.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

PID: 140316 TASK: ffff882101dbeaa0 CPU: 20 COMMAND: \"oracle\"

#0 \[ffff88011fd47e90\] crash_nmi_callback at ffffffff8102fee6

#1 \[ffff88011fd47ea0\] notifier_call_chain at ffffffff8152d515

#2 \[ffff88011fd47ee0\] atomic_notifier_call_chain at ffffffff8152d57a

#3 \[ffff88011fd47ef0\] notify_die at ffffffff810a154e

#4 \[ffff88011fd47f20\] do_nmi at ffffffff8152b1db

#5 \[ffff88011fd47f50\] nmi at ffffffff8152aaa0

\[exception RIP: flush_tlb_others_ipi+288\]

RIP: ffffffff8104fa60 RSP: ffff883ad675b3b8 RFLAGS: 00000246

RAX: 0000000000000000 RBX: 0000000000000900 RCX: 0000000000000030

RDX: 0000000000000000 RSI: 0000000000000090 RDI: ffffffff81e27298

RBP: ffff883ad675b3f8 R8: 0000000000000002 R9: 0000000000000080

R10: 0000000000000000 R11: 0000000000000000 R12: ffffffff81e27298

R13: 0000000000000004 R14: ffffffff81e27290 R15: ffff88014b5ec348

ORIG_RAX: ffffffffffffffff CS: 0010 SS: 0018

\-\-- \<NMI exception stack\> \-\--

#6 \[ffff883ad675b3b8\] flush_tlb_others_ipi at ffffffff8104fa60

#7 \[ffff883ad675b400\] native_flush_tlb_others at ffffffff8104fae6

#8 \[ffff883ad675b430\] flush_tlb_page at ffffffff8104fc0e

#9 \[ffff883ad675b460\] ptep_clear_flush_young at ffffffff8104eb50

#10 \[ffff883ad675b490\] page_referenced_one at ffffffff811544e4

#11 \[ffff883ad675b4f0\] page_referenced at ffffffff81154ee8

#12 \[ffff883ad675b5a0\] shrink_active_list at ffffffff81139a41

#13 \[ffff883ad675b660\] shrink_mem_cgroup_zone at ffffffff8113a74a

#14 \[ffff883ad675b730\] shrink_zone at ffffffff8113aa73

#15 \[ffff883ad675b7a0\] zone_reclaim at ffffffff8113b661

#16 \[ffff883ad675b8c0\] get_page_from_freelist at ffffffff8112d83c

#17 \[ffff883ad675b9f0\] \_\_alloc_pages_nodemask at ffffffff8112f3a3

#18 \[ffff883ad675bb30\] alloc_pages_current at ffffffff81167a9a

#19 \[ffff883ad675bb60\] \_\_page_cache_alloc at ffffffff8111fd57

#20 \[ffff883ad675bb90\] \_\_do_page_cache_readahead at ffffffff811354cb

#21 \[ffff883ad675bc20\] ra_submit at ffffffff81135621

#22 \[ffff883ad675bc30\] ondemand_readahead at ffffffff81135995

#23 \[ffff883ad675bc90\] page_cache_async_readahead at ffffffff81135b50

#24 \[ffff883ad675bce0\] generic_file_aio_read at ffffffff81121613

#25 \[ffff883ad675bdc0\] do_sync_read at ffffffff81188dba

#26 \[ffff883ad675bef0\] vfs_read at ffffffff811896a5

 

 

crash\> px 0xffff883ad675b400-0x8

\$9 = 0xffff883ad675b3f8

crash\> px 0xffff883ad675b3f8-0x18

\$10 = 0xffff883ad675b3e0

crash\> rd 0xffff883ad675b3e0

ffff883ad675b3e0: ffff88014b5ec348 H.\^K\....

crash\> cpumask ffff88014b5ec348

struct cpumask {

bits = 

}

crash\> eval -b 128

hexadecimal: 80

decimal: 128

octal: 200

binary: 0000000000000000000000000000000000000000000000000000000010000000

bits set: 7

 

crash\> p flush_state\[7\].flush_cpumask

\$11 =

 

crash\> runq -c 7

CPU 7 RUNQUEUE: ffff8820f0c76840

CURRENT: PID: 79152 TASK: ffff8801a727eae0 COMMAND: \"oracle\"

RT PRIO_ARRAY: ffff8820f0c769c8

\[no tasks queued\]

CFS RB_ROOT: ffff8820f0c768d8

\[no tasks queued\]

 

crash\> bt 79152

PID: 79152 TASK: ffff8801a727eae0 CPU: 7 COMMAND: \"oracle\"

#0 \[ffff8820f0c67e90\] crash_nmi_callback at ffffffff8102fee6

#1 \[ffff8820f0c67ea0\] notifier_call_chain at ffffffff8152d515

#2 \[ffff8820f0c67ee0\] atomic_notifier_call_chain at ffffffff8152d57a

#3 \[ffff8820f0c67ef0\] notify_die at ffffffff810a154e

#4 \[ffff8820f0c67f20\] do_nmi at ffffffff8152b1db

#5 \[ffff8820f0c67f50\] nmi at ffffffff8152aaa0

\[exception RIP: \_spin_lock+33\]

RIP: ffffffff8152a311 RSP: ffff8801d84cfb08 RFLAGS: 00000093

RAX: 0000000000001399 RBX: ffff8820f0c76840 RCX: 0000000000000090

RDX: 0000000000001394 RSI: ffff88011fd76840 RDI: ffff88011fd76840

RBP: ffff8801d84cfb08 R8: ffff884052d27420 R9: 0000000000000080

R10: 0000000000000001 R11: 0000000000000000 R12: ffff88011fd76840

R13: ffff8820f0c70ba0 R14: 0000000000000007 R15: ffff88207fe82000

ORIG_RAX: ffffffffffffffff CS: 0010 SS: 0018

\-\-- \<NMI exception stack\> \-\--

#6 \[ffff8801d84cfb08\] \_spin_lock at ffffffff8152a311

#7 \[ffff8801d84cfb10\] double_lock_balance at ffffffff81058e7a

#8 \[ffff8801d84cfb30\] thread_return at ffffffff81527cb8

#9 \[ffff8801d84cfbf0\] io_schedule at ffffffff815280a3

#10 \[ffff8801d84cfc10\] sync_page at ffffffff8111f96d

#11 \[ffff8801d84cfc20\] sync_page_killable at ffffffff8111f98e

#12 \[ffff8801d84cfc30\] \_\_wait_on_bit_lock at ffffffff8152893a

#13 \[ffff8801d84cfc80\] \_\_lock_page_killable at ffffffff8111f897

#14 \[ffff8801d84cfce0\] generic_file_aio_read at ffffffff811215c4

#15 \[ffff8801d84cfdc0\] do_sync_read at ffffffff81188dba

#16 \[ffff8801d84cfef0\] vfs_read at ffffffff811896a5

#17 \[ffff8801d84cff30\] sys_pread64 at ffffffff811899d2

#18 \[ffff8801d84cff80\] system_call_fastpath at ffffffff8100b072

RIP: 00000035f720f113 RSP: 00007fff6250c320 RFLAGS: 00000246

RAX: 0000000000000011 RBX: ffffffff8100b072 RCX: 00007fb686f12650

RDX: 0000000000100000 RSI: 00007fb686a28000 RDI: 0000000000000161

RBP: 00007fff6250ae50 R8: 0000000000100000 R9: 00007fb686a28000

R10: 0000000099500000 R11: 0000000000000246 R12: 00053b32cf583590

R13: 0000000000002000 R14: 00007fb685f0b7a0 R15: 0000000000000001

ORIG_RAX: 0000000000000011 CS: 0033 SS: 002b

 

 

#7 \[ffff8801d84cfb10\] double_lock_balance at ffffffff81058e7a

ffff8801d84cfb18: ffff88011fd76840 ffff8820f0c76840

ffff8801d84cfb28: ffff8801d84cfbe8 ffffffff81527cb8

#8 \[ffff8801d84cfb30\] thread_return at ffffffff81527cb8

 

crash\> runq \| grep ffff88011fd76840

CPU 22 RUNQUEUE: ffff88011fd76840

crash\> runq \| grep ffff8820f0c76840

CPU 7 RUNQUEUE: ffff8820f0c76840

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

The process \'migration/22\' on CPU 22 was trying to get the lock for runqueue 22.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

crash\> runq -c 22

CPU 22 RUNQUEUE: ffff88011fd76840

CURRENT: PID: 91 TASK: ffff8820530e6ae0 COMMAND: \"migration/22\"

RT PRIO_ARRAY: ffff88011fd769c8

\[ 0\] PID: 91 TASK: ffff8820530e6ae0 COMMAND: \"migration/22\"

\[ 98\] PID: 17660 TASK: ffff881f06f54040 COMMAND: \"oracle\"

CFS RB_ROOT: ffff88011fd768d8

\[120\] PID: 85496 TASK: ffff882d1e526aa0 COMMAND: \"oracle\"

crash\> bt 91

PID: 91 TASK: ffff8820530e6ae0 CPU: 22 COMMAND: \"migration/22\"

#0 \[ffff88011fd67e90\] crash_nmi_callback at ffffffff8102fee6

#1 \[ffff88011fd67ea0\] notifier_call_chain at ffffffff8152d515

#2 \[ffff88011fd67ee0\] atomic_notifier_call_chain at ffffffff8152d57a

#3 \[ffff88011fd67ef0\] notify_die at ffffffff810a154e

#4 \[ffff88011fd67f20\] do_nmi at ffffffff8152b1db

#5 \[ffff88011fd67f50\] nmi at ffffffff8152aaa0

\[exception RIP: \_spin_lock+33\]

RIP: ffffffff8152a311 RSP: ffff8820530eb870 RFLAGS: 00000097

RAX: 0000000000001395 RBX: 0000000000016840 RCX: 0000000000000000

RDX: 0000000000001394 RSI: ffff8820530eb8d8 RDI: ffff88011fd76840

RBP: ffff8820530eb870 R8: 0000000000000000 R9: 00000000000032ec

R10: 0000000000000000 R11: 0000000000000001 R12: ffff88204f828aa0

R13: ffff8820530eb8d8 R14: ffff88011fd76840 R15: 0000000000000001

ORIG_RAX: ffffffffffffffff CS: 0010 SS: 0018

\-\-- \<NMI exception stack\> \-\--

#6 \[ffff8820530eb870\] \_spin_lock at ffffffff8152a311

#7 \[ffff8820530eb878\] task_rq_lock at ffffffff81058fed

#8 \[ffff8820530eb8a8\] try_to_wake_up at ffffffff81065a4c

#9 \[ffff8820530eb918\] default_wake_function at ffffffff81065e02

#10 \[ffff8820530eb928\] autoremove_wake_function at ffffffff8109b2b6

#11 \[ffff8820530eb948\] \_\_wake_up_common at ffffffff81054839

#12 \[ffff8820530eb998\] \_\_wake_up at ffffffff81058d48

#13 \[ffff8820530eb9d8\] wakeup_kswapd at ffffffff811386de

#14 \[ffff8820530eba18\] \_\_alloc_pages_nodemask at ffffffff8112f92f

#15 \[ffff8820530ebb58\] kmem_getpages at ffffffff8116e482

#16 \[ffff8820530ebb88\] fallback_alloc at ffffffff8116f09a

#17 \[ffff8820530ebc08\] \_\_\_\_cache_alloc_node at ffffffff8116ee19

#18 \[ffff8820530ebc68\] kmem_cache_alloc_node_trace at ffffffff8116ffe0

#19 \[ffff8820530ebcc8\] alloc_cpumask_var_node at ffffffff8128248a

#20 \[ffff8820530ebce8\] alloc_cpumask_var at ffffffff812824f1

#21 \[ffff8820530ebcf8\] find_lowest_rq at ffffffff8106cdd8

#22 \[ffff8820530ebd48\] push_rt_task at ffffffff8106cfba

#23 \[ffff8820530ebd98\] post_schedule_rt at ffffffff8106d290

#24 \[ffff8820530ebdb8\] thread_return at ffffffff81527a24

#25 \[ffff8820530ebe78\] migration_thread at ffffffff81068635

#26 \[ffff8820530ebee8\] kthread at ffffffff8109aef6

#27 \[ffff8820530ebf48\] kernel_thread at ffffffff8100c20a

 

 

/usr/src/debug/kernel-2.6.32-431.el6/linux-2.6.32-431.el6.x86_64/kernel/sched.c: 1193

0xffffffff81058fd2 \<task_rq_lock+66\>: mov 0x8(%r12),%rax

0xffffffff81058fd7 \<task_rq_lock+71\>: mov %rbx,%r14

0xffffffff81058fda \<task_rq_lock+74\>: mov 0x18(%rax),%eax

0xffffffff81058fdd \<task_rq_lock+77\>: add -0x7e4030e0(,%rax,8),%r14

/usr/src/debug/kernel-2.6.32-431.el6/linux-2.6.32-431.el6.x86_64/kernel/sched.c: 1194

0xffffffff81058fe5 \<task_rq_lock+85\>: mov %r14,%rdi

0xffffffff81058fe8 \<task_rq_lock+88\>: callq 0xffffffff8152a2f0 \<\_spin_lock\>

 

1186 static struct rq \*task_rq_lock(struct task_struct \*p, unsigned long \*flags )

1187 \_\_acquires(rq-\>lock)

1188 {

1189 struct rq \*rq;

1190

1191 for (;;) {

1192 local_irq_save(\*flags);

1193 rq = task_rq(p);

 

 

crash\> bt ffff88204f828aa0 \| grep CPU

PID: 499 TASK: ffff88204f828aa0 CPU: 22 COMMAND: \"kswapd0\"

 

crash\> runq -c 22

CPU 22 RUNQUEUE: ffff88011fd76840

CURRENT: PID: 91 TASK: ffff8820530e6ae0 COMMAND: \"migration/22\"

RT PRIO_ARRAY: ffff88011fd769c8

\[ 0\] PID: 91 TASK: ffff8820530e6ae0 COMMAND: \"migration/22\"

\[ 98\] PID: 17660 TASK: ffff881f06f54040 COMMAND: \"oracle\"

CFS RB_ROOT: ffff88011fd768d8

\[120\] PID: 85496 TASK: ffff882d1e526aa0 COMMAND: \"oracle\"

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

There\'s a known issue regarding this which lots of processes are awaiting in double_lock_balance() as it took it\'s own lock and awaiting the other\'s lock which causes of deadlock situation.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

crash\> bt -a \| grep double_lock

#7 \[ffff8820536b3d38\] double_lock_balance at ffffffff81058e7a

#7 \[ffff884050769d58\] double_lock_balance at ffffffff81058e7a

#7 \[ffff8801d84cfb10\] double_lock_balance at ffffffff81058e7a

#7 \[ffff882053791d68\] double_lock_balance at ffffffff81058e7a

#7 \[ffff88213a7039b0\] double_lock_balance at ffffffff81058e5e

#7 \[ffff88218379fb10\] double_lock_balance at ffffffff81058e7a

#7 \[ffff88205303dd68\] double_lock_balance at ffffffff81058e7a

#7 \[ffff882053097d38\] double_lock_balance at ffffffff81058e7a

#7 \[ffff8820530e9d68\] double_lock_balance at ffffffff81058e7a

#7 \[ffff882adc6bdb10\] double_lock_balance at ffffffff81058e7a

#7 \[ffff88245fc0bb10\] double_lock_balance at ffffffff81058e7a

#7 \[ffff883c3a8619b0\] double_lock_balance at ffffffff81058e7a

#7 \[ffff8820532bdd68\] double_lock_balance at ffffffff81058e7a

#7 \[ffff882053347d38\] double_lock_balance at ffffffff81058e7a

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

It has been fixed in kernel-2.6.32-504.el6. Please consider to upgrade your kernel to the fixed version. You can find some details about this in the below article.

 

Watchdog detected hard LOCKUP on cpu X due to runqueue spinlock lock inversion

<https://access.redhat.com/solutions/763373>

 

Please let us know if you have any questions. Thanks.

 

Best regards,

Daniel Kwon

Software Maintenance Engineer, GSS, Red Hat Asia Pacific\"

 

<https://access.redhat.com/support/cases/#/case/01692732?commentId=a0aA000000HlkrPIAR>

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

一条更新内容被添加到了此问题单。

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

为了确保最佳支持体验，请注意以下事项：

 

\* 回复此邮件会将您的评论添加到该问题单中。我们建议您在邮件出问题的情况下通过客户门户网站直接将评论添加到问题单中。

\* 回复此邮件时不要更改主题。

\* 检查确定您使用作为问题单联系人的电子邮件地址回复问题单邮件。

\* 附件不能通过电子邮件的方式添加到问题单上。附件必须直接在问题单上上传。

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Supporting success. Exceeding expectations.

 

Red Hat 支持服务在社交媒体中的公共主页：https://access.redhat.com/social/

Red Hat 客户门户网站技术讨论：https://access.redhat.com/discussions/

Red Hat Access Labs: <https://access.redhat.com/labs/>

 

如果您需要紧急支持，请参考 [https://access.redhat.com/support/contact/technicalSupport/](https://access.redhat.com/support/contact/technicalSupport/)。

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ref:\_00DA0HxWH.\_500A0V7Ol4:ref

 

已使用 OneNote 创建。
