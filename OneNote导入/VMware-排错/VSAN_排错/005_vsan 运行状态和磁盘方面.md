vsan 运行状态和磁盘方面

2022年9月19日

16:01

》esxcli vsan cluster list

\[root@localhost:\~\] esxcli vsan cluster list

Cluster Information of 52557adc-dba7-0136-901e-6d2b9c417ea6

   Enabled: true

   Current Local Time: 2023-04-18T07:10:57Z

   Local Node UUID: 617b9736-87f7-c584-5bcb-00505689a23b

   Local Node Type: NORMAL

[   Local Node State: MASTER][  \--\> ]这个host是master

   Local Node Health State: HEALTHY

   Sub-Cluster Master UUID: 617b9736-87f7-c584-5bcb-00505689a23b

   Sub-Cluster Backup UUID: 617b9ac4-a309-4397-f6a5-0050568937c2

   Sub-Cluster UUID: 52557adc-dba7-0136-901e-6d2b9c417ea6

   Sub-Cluster Membership Entry Revision: 4

[  ][ Sub-Cluster Member Count: 3][  \--\> ]一共有三个node

   Sub-Cluster Member UUIDs: 617b9736-87f7-c584-5bcb-00505689a23b, 617b9ac4-a309-4397-f6a5-0050568937c2, 617b9e14-8bb4-102c-37a6-00505689d43c

   Sub-Cluster Member HostNames: localhost, localhost, localhost

   Sub-Cluster Membership UUID: cb912264-3a62-836b-15fc-00505689a6f5

   Unicast Mode Enabled: true

[ ][  Maintenance Mode State: OFF][  \--\> ]没有在维护模式，on是在

   Config Generation: 776d7cce-c2a7-41c9-80b6-df59eec58ec9 9 2023-03-29T02:33:35.612

 

》命令列出所有 vSAN 对象的健康状况摘要。

\[root@localhost:\~\] esxcli vsan debug object health summary get

 

Object Health Status        Description

[     5                          Healthy                ]健康的

[     6                          Absent                ]缺席的

     9                          Degrade                

[     10                        Reconfiguring        ][             ]重新配置

 

 

》对vSAN存储进行检查         

\[root@localhost:\~\] localcli vsan storage list \|grep CMMDS

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

 

 

》对磁盘与磁盘组删除\
esxcli vsan storage list 

esxcli vsan storage remove \--disk mpx.vmhba0:C0:T4:L0[  \-- ]》删除容量盘

esxcli vsan storage remove \--ssd mpx.vmhba0:C0:T2:L0[   \--]》删除缓存盘

esxcli vsan storage remove -u 5229a8bb-7c36-4664-6c3f-50aea3ffb28a[  \--]》 删除磁盘组

 

已使用 OneNote 创建。
