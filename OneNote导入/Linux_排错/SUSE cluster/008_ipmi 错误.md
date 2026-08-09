ipmi 错误

2023年8月24日

10:43

SUSE hana cluster 出现下面状态\
 

![[SUSE cluster_008_ipmi 错误_001.png]]

 

需要使用此命令测试 iDRAC 连接状态

#ipmitool -I lanplus -H idracip -U username -a sel info 

 

下面是输出的示例。

\# ipmitool -I lanplus -H 192.168.0.140 -U root -a sel info

Password:

SEL Information

Version          : 1.5 (v1.5, v2 compliant)

Entries          : 212

Free Space       : 12992 bytes

Percent Used     : 20%

Last Add Time    : 03/24/2023 17:05:18

Last Del Time    : 01/18/2019 03:30:46

Overflow         : false

Supported Cmds   : \'Reserve\'

 

如果 sel info 没有得到结果或超时，我们可能需要对 iDRAC 进行软重置

需要确保向 iDRAC 发送 ipmitool 命令会得到响应

 

如果没问题，那就清除错误的资源信息

#crm resource cleanup rsc_erp-hana-prd01_stonith_ipmi

 

它的状态会变回 start

 

 

日志记录=====

来自 /var/log/message

![[SUSE cluster_008_ipmi 错误_002.png]]

 

![[SUSE cluster_008_ipmi 错误_003.png]]

 

已使用 OneNote 创建。
