====== 1 =====

[[R740xd] [IC: 192150001 ] [PC: 192137868 ] [ST: 347LTJ3 ]]
TS:Huang, Rae | IM:N/A |SAM:Geng, Xin

[OS: ] [Kernel: ] [OEM: Y/N]

--- 描述 ---

Internal;

Escalation Reason: SAM要求升级;

Product Type: Software;

Software Name: Linux(Redhat/SUSE/Ubuntu/SAP/HANA/HPC);

Order Type: 非DELL订单号，但有PSP服务;

TSR Log;

Detail Symptom Descriptions(故障现象): 用户设置服务器为performance模式,但cpu频率会发生变化,不能稳定在固定频率,请看截图,[相比而言,centos是稳定工作频率的[]; ]日志已上传thunder

Troubleshooting Steps(详细诊断步骤): OS: ubuntu 22.04，查看bios设置无误;

Current status(当前状态和要求): sam要求升级提供方案或者原因;

====== 2 =====

The customer has not had time to operate and has no plans to do. Cases are turned off for now and turned back on if needed.

----------------------------\
Confirmed with SAM that the customer is not yet operational and waiting for feedback from the customer.

----------------------------\
Further testing, adding "idle=poll idle=halt" to the kernel and changing the BIOS to custom can satisfy customer requirements.\
----------------------------\
From testing, ubuntu 22.04 kernel doesn't have the problem of CPU frequency dropping after disable idle is set, some of them will be higher than the original frequency.

----------------------------\
Analyzing:

Client system Ubutnu 22.04 was found to have a downgrade.

Solution:

Provide relevant methods to customers for testing.

Next step:

Waiting for update.

====== 3 =====

Resolution：

自行关闭 20240701

 

Root Cause: 

 

 

----Remark----

idle=poll idle=halt

 

在 /etc/default/grub 里 添加下面设置|

 processor.max_cstate=1 intel_idle.max_cstate=0 intel_pstate=disable

如图：

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_001.jpg]]

然后使用 grub-mkconfig -o /boot/grub/grub.cfg 命令跟新一下

重启系统生效

 

 

 

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_002.png]]

 

 

 

有降频

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_003.png]]

 

有降频

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_004.png]]

 

 

不降频，有些会睿频

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_005.png]]

Intel(R) Xeon(R) Gold 6348 CPU @ 2.60GHz\
cpu MHz                : 2600.000

cpu MHz                : 2600.000

cpu MHz                : 2600.000

cpu MHz                : 2600.000

cpu MHz                : 2784.818

cpu MHz                : 2600.000

cpu MHz                : 2600.000

......

 

 

会降频

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_006.png]]

 

不降频，有些会睿频

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_007.png]]

 

 

 

 

 

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_008.png]]

 

 

 

 

 

 

 

 

 

root@user1:~# cpupower frequency-info

 

root@user1:~# cpupower frequency-set -g performance

 

 

 

 

 

idle=poll idle=halt

![[My-Case_2024-06_007__7ubuntu-cpu-C1E_009.png]]

 

 

 

processor.max_cstate=1

intel_idle.max_cstate=0

intel_pstate=disable

idle=poll

idle=halt

Idle=nomwait
