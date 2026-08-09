echo c 测试

2024年4月30日

14:31

通过 echo c \> /proc/sysrq-trigger 来测试集群

在node2上运行 echo c 后日志记录。

 

 

Node1:

May[  6 16:56:23 localhost root\[5537\]: CCCCC\-\-\-\-\-\--0506\-\--16:56][    ]《\--手动插入日志记录

May  6 16:56:38 localhost corosync\[4875\]:  \KNET[  \] link: host: 2 link: 0 is down

May  6 16:56:38 localhost corosync\[4875\]:  \KNET[  \] host: host: 2 (passive) best link: 0 (pri: 1)

May  6 16:56:38 localhost corosync\[4875\]:  \KNET[  \] host: host: 2 has no active links

May  6 16:56:39 localhost corosync\[4875\]:  \[TOTEM \] Token has not been received in 2250 ms

May  6 16:56:40 localhost corosync\[4875\]:  \[TOTEM \] A processor failed, forming new configuration: token timed out (3000ms), waiting 3600ms for consensus.

May  6 16:56:44 localhost corosync\[4875\]:  \[QUORUM\] Sync members\[1\]: 1

May  6 16:56:44 localhost corosync\[4875\]:  \[QUORUM\] Sync left\[1\]: 2

May  6 16:56:44 localhost corosync\[4875\]:  \[TOTEM \] A new membership (1.e0) was formed. Members left: 2

May  6 16:56:44 localhost corosync\[4875\]:  \[TOTEM \] Failed to receive the leave message. failed: 2

May  6 16:56:44 localhost pacemaker-attrd\[4891\]: notice: Lost attribute writer node2.abc.com

May  6 16:56:44 localhost pacemaker-fenced\[4889\]: notice: Node node2.abc.com state is now lost

May  6 16:56:44 localhost pacemaker-based\[4888\]: notice: Node node2.abc.com state is now lost

May  6 16:56:44 localhost pacemaker-controld\[4893\]: notice: Our peer on the DC (node2.abc.com) is dead

May  6 16:56:44 localhost corosync\[4875\]:  \[QUORUM\] Members\[1\]: 1

May  6 16:56:44 localhost corosync\[4875\]:  \MAIN[  \] Completed service synchronization, ready to provide service.

May  6 16:56:44 localhost pacemaker-fenced\[4889\]: notice: Purged 1 peer with id=2 and/or uname=node2.abc.com from the membership cache

May  6 16:56:44 localhost pacemaker-based\[4888\]: notice: Purged 1 peer with id=2 and/or uname=node2.abc.com from the membership cache

May  6 16:56:44 localhost pacemaker-attrd\[4891\]: notice: Node node2.abc.com state is now lost

May  6 16:56:44 localhost pacemaker-attrd\[4891\]: notice: Removing all node2.abc.com attributes for peer loss

May  6 16:56:44 localhost pacemaker-controld\[4893\]: notice: State transition S_NOT_DC -\> S_ELECTION

May  6 16:56:44 localhost pacemaker-attrd\[4891\]: notice: Purged 1 peer with id=2 and/or uname=node2.abc.com from the membership cache

May  6 16:56:44 localhost pacemaker-controld\[4893\]: notice: Node node2.abc.com state is now lost

May  6 16:56:44 localhost pacemaker-attrd\[4891\]: notice: Recorded local node as attribute writer (was unset)

May  6 16:56:44 localhost pacemaker-controld\[4893\]: notice: State transition S_ELECTION -\> S_INTEGRATION

May  6 16:56:44 localhost pacemaker-controld\[4893\]: notice: Cluster does not have watchdog fencing device

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: notice: On loss of quorum: Ignore

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: Cluster node node2.abc.com will be fenced: peer is no longer part of the cluster

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: Node node2.abc.com is unclean

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: fence_node1_stop_0 on node2.abc.com is unrunnable (node is offline)

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: fence_node2_stop_0 on node2.abc.com is unrunnable (node is offline)

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: nfsshare_stop_0 on node2.abc.com is unrunnable (node is offline)

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: nfs_vip_stop_0 on node2.abc.com is unrunnable (node is offline)

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: nfs_root_stop_0 on node2.abc.com is unrunnable (node is offline)

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: nfs_share02_stop_0 on node2.abc.com is unrunnable (node is offline)

May  6 16:56:44 localhost pacemaker-schedulerd\[4892\]: warning: nfsd_stop_0 on node2.abc.com is unrunnable (node is offline)

......

May  6 16:56:44 localhost pacemaker-controld\[4893\]: notice: Result of monitor operation for fence_node2 on n

ode1.abc.com: ok

May  6 16:57:33 localhost pacemaker-fenced\[4889\]: notice: Operation \'reboot\' \[5553\] targeting node2.abc.com

using fence_node2 returned 0

May  6 16:57:33 localhost pacemaker-fenced\[4889\]: notice: Operation \'reboot\' targeting node2.abc.com by node

1.abc.com for pacemaker-controld.4893@node1.abc.com: OK (complete)

May  6 16:57:33 localhost pacemaker-controld\[4893\]: notice: Fence operation 4 for node2.abc.com passed

May  6 16:57:33 localhost pacemaker-controld\[4893\]: notice: Peer node2.abc.com was terminated (reboot) by no

de1.abc.com on behalf of pacemaker-controld.4893@node1.abc.com: OK

 

 

 

Node 2 :

May[  6 16:56:20 localhost root\[227913\]: CCCCC\-\-\-\-\-\--0506\-\--16:56][   ]《\--手动插入日志记录

运行 cho c 后没有提示，直接系统重启了

May  6 17:00:54 localhost kernel: The list of certified hardware and cloud instances for Red Hat Enterprise Linux 9 can be viewed at the Red Hat Ecosystem Catalog, <https://catalog.redhat.com>.

May  6 17:00:54 localhost kernel: Command line: BOOT_IMAGE=(hd0,gpt2)/vmlinuz-5.14.0-162.6.1.el9_1.x86_64 root=/dev/mapper/rhel-root ro crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M resume=/dev/mapper/rhel-swap rd.lvm.lv=rhel/root rd.lvm.lv=rhel/swap rhgb quiet

May  6 17:00:54 localhost kernel: x86/split lock detection: #AC: crashing the kernel on kernel split_locks and warning on user-space split_locks

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x001: \'x87 floating point registers\'

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x002: \'SSE registers\'

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x004: \'AVX registers\'

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x020: \'AVX-512 opmask\'

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x040: \'AVX-512 Hi256\'

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x080: \'AVX-512 ZMM_Hi256\'

May  6 17:00:54 localhost kernel: x86/fpu: Supporting XSAVE feature 0x200: \'Protection Keys User registers\'

May  6 17:00:54 localhost kernel: x86/fpu: xstate_offset\[2\]:  576, xstate_sizes\[2\]:  256

May  6 17:00:54 localhost kernel: x86/fpu: xstate_offset\[5\]:  832, xstate_sizes\[5\]:   64

May  6 17:00:54 localhost kernel: x86/fpu: xstate_offset\[6\]:  896, xstate_sizes\[6\]:  512

May  6 17:00:54 localhost kernel: x86/fpu: xstate_offset\[7\]: 1408, xstate_sizes\[7\]: 1024

May  6 17:00:54 localhost kernel: x86/fpu: xstate_offset\[9\]: 2432, xstate_sizes\[9\]:    8

May  6 17:00:54 localhost kernel: x86/fpu: Enabled xstate features 0x2e7, context size is 2440 bytes, using \'compacted\' format.

May  6 17:00:54 localhost kernel: signal: max sigframe size: 3632

May  6 17:00:54 localhost kernel: BIOS-provided physical RAM map:

May  6 17:00:54 localhost kernel: BIOS-e820: \[mem 0x0000000000000000-0x000000000008efff\] usable

May  6 17:00:54 localhost kernel: BIOS-e820: \[mem 0x000000000008f000-0x000000000008ffff\] reserved

 

 

已使用 OneNote 创建。
