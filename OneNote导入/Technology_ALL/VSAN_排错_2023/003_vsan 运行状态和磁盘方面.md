vsan 运行状态和磁盘方面

2022年9月19日

16:01

》命令列出所有 vSAN 对象的健康状况摘要。

\[root@localhost:\~\] esxcli vsan debug object health summary get

 

Object Health Status        Description

[     5                          Healthy                ]健康的

[     6                          Absent                ]缺席的

[     9                          Degrade                ]降级

[     10                        Reconfiguring        ][             ]重新配置

 

 

》对vSAN存储进行检查         

\[root@localhost:\~\] localcli vsan storage list \|grep CMMDS

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

 

 

 

已使用 OneNote 创建。
