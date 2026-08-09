\|\_6-2 Red Hat Insights

2023年10月31日

16:50

\
Red Hat Insights 是一个 Software-as-a-Service (SaaS)产品，它帮助管理员报告适用的勘误表和已知的配置问题，并主动识别安全问题。洞察使您能够在潜在的影响服务的问题发生之前就意识到它们，让您能够在可能影响生产的问题出现之前计划如何解决它们。每个 Red Hat 企业 Linux (RHEL)订阅都包含了对 Red Hat Insights 的访问，所以没有什么额外的东西可以购买。

<https://www.redhat.com/sysadmin/how-red-hat-insights>\
 

RHEL8 测试：

 

RHEL8 和 RHEL9 都有预安装客户端 insights-client

 

[\[root@localhost \~\]# cat /etc/redhat-release ]

Red Hat Enterprise Linux release 8.7 (Ootpa)

 

[\[root@localhost \~\]# uname -a]

Linux localhost.localdomain 4.18.0-425.3.1.el8.x86_64 #1 SMP Fri Sep 30 11:45:06 EDT 2022 x86_64 x86_64 x86_64 GNU/Linux

 

系统添加订阅：\
[\[root@localhost \~\]# subscription-manager register \--username dony_1220 \--password \']xxxxxxx\' \--auto-attach

Registering to: subscription.rhsm.redhat.com:443/subscription

The system has been registered with ID: e6b6b04c-4d71-448c-8b5d-29242e7bd7d8

The registered system name is: localhost.localdomain

Ignoring request to auto-attach. It is disabled for org \"6342435\" because of the content access mode setting.

 

手动注册系统:

[\[root@localhost \~\]# insights-client \--register]

Successfully registered host localhost.localdomain

Automatic scheduling for Insights has been enabled.

Starting to collect Insights data for localhost.localdomain

Uploading Insights data.

Successfully uploaded report from localhost.localdomain to account 5382957.

View the Red Hat Insights console at <https://console.redhat.com/insights/>

 

[\[root@localhost \~\]# insights-client \--status]

System is registered locally via .registered file. Registered at 2023-10-31T16:38:04.483925

Insights API confirms registration.

 

WEB 访问 [https://console.redhat.com/insights/](https://console.redhat.com/insights/) 如下：

![[RHEL案例_012___6-2 Red Hat Insights_001.png]]

 

 

 

 

已使用 OneNote 创建。
