vsan 检查- cmmds

2023年10月18日

14:21

1. 从 vSAN 透视图的状态来看，ESxi 的维护模式与 vCenter 中的不同。若要检查 ESxi 是否已进入 vSAN 维护模式，或者是否卡住，请运行以下命令

 

cmmds-tool find -t NODE_DECOM_STATE -f json

输出：

{\
   \"uuid\": \"5c99d5dd-3f98-0ee0-6f75-e4434b398000\",\
   \"owner\": \"5c99d5dd-3f98-0ee0-6f75-e4434b398000\",\
   \"health\": \"Healthy\",\
   \"revision\": \"2546\",\
   \"type\": \"NODE_DECOM_STATE\",\
   \"flag\": \"2\",\
   \"minHostVersion\": \"0\",\
   \"md5sum\": \"5919c7a6c18472bea464f097b577e011\",\
   \"valueLen\": \"144\",\
   \"content\": ,\
   \"errorStr\": \"(null)\"\
 }]

在输出中，找到解除状态。下面是解除状态的已知状态及其含义:

 

decomState: 0: 这意味着节点当前不处于维护模式。

decomState: 4: 这意味着节点当前正在进入维护模式。

decomState: 6: 这意味着从 vSAN 的角度来看，节点当前处于维护模式。

 

如果由于任何原因该节点无法进入或退出维护模式，请尝试以下操作

 

如果来自 vCenter 的 ESxi 可以进入维护模式(右键单击 ESxi 时可以使用该选项) ，请尝试进入和退出(或退出和进入)维护模式。

 

如果不成功，请尝试使用以下命令退出 vSAN 维护模式：

esxcli vsan maintenancemode cancel

 

 

2. 查找特定node的信息

\[root@localhost:\~\] cmmds-tool whoami

617b9ac4-a309-4397-f6a5-0050568937c2

 

\[root@localhost:\~\] cmmds-tool find -t HOSTNAME -u \'617b9ac4-a309-4397-f6a5-0050568937c2\' -f json

{

 \"entries\":

\

[ {

   \"uuid\": \"617b9ac4-a309-4397-f6a5-0050568937c2\",

   \"owner\": \"617b9ac4-a309-4397-f6a5-0050568937c2\",

[   \"health\": \"Healthy\", ][ ][ \--\> ]正常，不正常会是Unhealthy

   \"revision\": \"0\",

   \"type\": \"HOSTNAME\",

   \"flag\": \"2\",

   \"minHostVersion\": \"0\",

   \"md5sum\": \"3b3f33b75721a7306357deaf93e9099a\",

   \"valueLen\": \"24\",

[   \"content\": ,][  \--\> ]不正常这里也会指向那个node

   \"errorStr\": \"(null)\"

 }

\]

}

 

 

3. 其它

<https://virtuallyvtrue.com/2020/10/26/how-to-troubleshoot-vsan-issues/>

 

检查组件(主机)的状态

cmmds-tool find -f python \| grep CONFIG_STATUS -B 4 -A 6 \| grep \'uuid\\\|content\' \| grep -o \'state\\\\\\\":\\ \[0-9\]\*\' \| sort \| uniq -c

显示代表的意思

772 state\\\": 7 \--healthy (健康正常的)

2 state\\\": 13 \--inaccessible objects (无法接近的物体)

6 state\\\": 15 \--absent/degraded (不存/退化)

2 state\\\": 45 \-- 孤立的对象组键

 

cmmds-tool find -f python \| grep CONFIG_STATUS -B 4 -A 6 \| grep \'uuid\\\|content\' \| grep \'state\\\\\\\": 13\' -B 1 \| grep uuid \| cut -d \"\\\"\" -f4

7328b659-9d15-cb40-68eb-a81e846157fb

e52bb359-d0e0-3ce2-7070-a81e846153d9

 

磁盘平衡测试(主机)

esxcli vsan health cluster get -t \"vSAN Disk Balance\"

平衡状态：

vSAN Disk Balance green

Overview

Metric Value

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Average Disk Usage 10 %

Maximum Disk Usage 12 %

Maximum Variance 4 %

LM Balance Index 2 %

 

Disk Balance

Host Device Rebalance State Data To Move

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

不平衡状态：

SAN Disk Balance yellow

 

Overview

Metric Value

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Average Disk Usage 27 %

Maximum Disk Usage 45 %

Maximum Variance 37 %

LM Balance Index 19 %

 

Disk Balance

Host Device Rebalance State Data To Move

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

172.16.11.246 Local TOSHIBA Disk (naa.50000398b821a7c5) Proactive rebalance is in progress 420.5244 GB

 

查找处于 PDL 状态的设备(主机)

\[root@esxi-1:\~\] vdq -iH

正常状态：

Mappings:

DiskMapping\[0\]:

SSD: naa.58ce38ee2016ffe5

MD: naa.5002538a4819e540

MD: naa.5002538a4819e510

MD: naa.5002538a4819e3e0

 

DiskMapping\[2\]:

SSD: naa.58ce38ee2016fe55

MD: naa.5002538a48199ca0

MD: naa.5002538a48199e20

MD: naa.5002538a48199e00

 

缺失状态：

Mappings:

DiskMapping\[0\]:

SSD: naa.58ce38ee2016ffe5

MD: naa.5002538a4819e3e0

 

DiskMapping\[2\]:

SSD: naa.58ce38ee2016fe55

MD: naa.5002538a48199ca0

MD: naa.5002538a48199e20

MD: naa.5002538a48199e00

 

\[root@esxi-04:\~\] vdq -qH

DiskResults:

 DiskResult\[0\]:

 Name: naa.600508b1001c4b820b4d80f9f8acfa95

 VSANUUID: 5294bbd8-67c4-c545-3952-7711e365f7fa

 State: In-use for VSAN

 ChecksumSupport: 0

 Reason: Non-local disk

 IsSSD?: 0

IsCapacityFlash?: 0

 IsPDL?: 0

 \<\<truncated\>\>

 DiskResult\[18\]:

 Name:

 VSANUUID: 5227c17e-ec64-de76-c10e-c272102beba7

 State: In-use for VSAN

 ChecksumSupport: 0

 Reason: None

 IsSSD?: 0

IsCapacityFlash?: 0

 IsPDL?: 1          = 设备处于 PDL 状态，1就是全路径丢失的状态，0正常。

 

已使用 OneNote 创建。
