====== 1 =====

[[R640] \IC: 183987721[  ] [PC: 183476213 ] [ST: 3L1T3L3 ]]
TS:| IM:N/A |SAM:N/A

[OS: ] [Kernel: ] [OEM: N]

--- 描述 ---

Internal;

Escalation Reason: CTE建议升级;

Product Type: Software;

Software Name: VMware;

Order Type: DELL订单号（过保），但无PSP服务;

Product Model: R640;

VMware ESXi Log;

日志上传位置: SFDC Related;

Detail Symptom Descriptions(故障现象): 其中一台虚机传输数据会阻塞，导致数据传输中断;

Troubleshooting Steps(详细诊断步骤):

1】两台R640[  3L1T3L3  ]做主备服务器，FC HBA后端链接C332ZL3 4024存储，

主机下4台虚机 运行不同应用程序,其中的某一台虚机业务将产线上数据上传到后端存储，

最近发现会出现数据传输会中断，导致生产线会中断。

客户提供VMware 订单 484159143[  D4N40Q2 ]，查询已经OOW

2】 存储case：183916073 分析存储日志确定没有问题

3】服务器TSR 日志查看硬件没有报错

Current status(当前状态和要求): 协助分析系统日志，找到问题所在

5428Q-4JJ81-18VGW-0U3R2-10HL4

====== 2 =====

\
----------------------------\
Analyzing:

Feedback ESXI link storage is unstable.

Solution:

Suggest provide more details information and OS log.

Next step:

Waiting for update.

====== 3 =====

Resolution：

 

Root Cause: 

 

 

----Remark----
