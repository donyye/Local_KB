Day-6,7 EQL

Monday, November 25, 2013

EqualLogic - 纯iSCSI

- 只有iSCSI的配置
- 适用于中低端市场
- 配置方法相对于MD更简化
- EQL是Scale -out横向按需求扩展

每个EQL都有控制器，扩展会增加iscsi接口

MD是Scale-up纵向扩展，盘柜间的扩展

- 数据分层：将数据分层存放到不同的介质（SSD/SAS/SATA,LSAS）上去

将经常访问的数据存放在SSD。

将一般访问的数据存放在SAS。

将不怎么访问的数据存放在SATA，LSAS。

- 第三位1为万兆，0为千兆。
- EQL上永远只有一个控制器在工作，另一个控制器处于Standby的状态。
- 不需要license。
- 硬盘配置只有半配/满配；M4110需要满配。
- X-SAS 10K[   ;    V-SAS 15K    ;    S-SSD    ;    E-Near line SAS]
- Thin provisioning：瘦分配

使用自动精简配置，所有的用户容量都以虚拟存储的形式分配，而实际的物理磁盘空间将根据实际使用情况进行分配。所有物理磁盘被视为一个磁盘池进行管理，按照写入虚拟卷的数据量完成分配。如此一来，未使用的物理磁盘容量显著降低，进而实现更高效的存储作业。另外，需要添加额外物理磁盘时，预定义阈值将发出警告，以避免容量短缺。

例如，某位用户向服务器管理员请求分配10TB的资源。

虽然可能确实需要10TB的物理存储容量，但根据当前使用情况，分配2TB就已足够。因此，系统管理员准备2TB的物理存储，并给服务器分配10TB的虚拟卷。服务器即可基于仅占虚拟卷容量1/5的现有物理磁盘池开始运行。这样的"始于小"方案能够实现更高效地利用存储容量。

当虚拟卷需要更多的物理容量时（如下图所示），会占用现有的物理卷容量。为避免出现容量短缺，可使用预定义的使用阈值来监控物理磁盘池。例如，将阈值定义为整个磁盘池容量的80%，则在达到阈值时，系统管理员就会收到扩展物理磁盘数量的警报。因而，您无需停止系统，即可添加新的驱动器，同时确保系统运行的连续性。

Thin clones：瘦克隆

- Volume = VD
- 一个Group为一套EQL，Member为Group的成员，Pool为硬盘池子
- EQL的一些型号里，管理口和数据口是共享的。（管理网口是10Mbps/100Mbps）

 

Init EQL

- 1.Serial port

恢复出厂默认值: reset -\> DeleteAllMyDataNow

stuXm -\> member ip

stuXg[  -\> group ip]

2.HIT(Host Integration Tool)

只能允许安装一个多路径管理（EQL有EQL自己的多路径【Hit会装】，不能和MD的多路径混在一起）

必须配置VSS CHAP用户名/密码

- Near line SAS/SATA 只能做成RAID6 or RAID10，不然会经常出现掉线情况。 -对于MD/COMPLE/EQL来说都是如此
- RAID6不允许转成其他的RAID级别，因为EQL认为RAID6是最安全的级别。
- 登陆盘柜之后，最好第一时间设置好盘柜的日期/时间。

 

连接到EQL iSCSI

iSCSI initiator 要连接的是group ip 

如果服务器同时连接MD和EQL，多路径不能装MD或者是EQL的多路径软件，而是用微软自带的MPIO。

 

做实验的时候:

用户名grpadmin 密码grpadmin

机器:4376

 

EQL Hardware

- CLB(Capacity Load Balance) 容量负载均衡
- NLB(Network Load Balance)网络负载均衡
- Preemptive Drive Mirroring 提供Hot spare的性能(不必要所有数据都从rebuild校验得出)
- M4100/M4110上控制器的FW在控制器里面的SD卡上

 

EQL Management

- CLI - Serial port/Telnet
- SAN HQ[  - ]更详细检查性能/监控更多
- 三种Volume分配方式: 1.IP[   2.IQN    3.CHAP account/pswd]
- Update Firmware: 1.查看Release Notes看合适的升级版本[  2. ]停机
- 收集日志:
  - 命令行底下 \>diag
  - 会在ftp://grpadmin:grpadmin@192.168.60.243 里生成6个dgo文件
  - 将6个dgo文件copy下来,打包成一个压缩包
  - 登陆到http://10.24.27.217 (用NT帐号申请),可以查看日志

 

EQL Snapshot[  - ]采用redirect I/O Snapshot快照

- Read Only / Read Write 
- 很对Volume上的snapshot（如果Double Fault了，snapshot无法作为backup还原）
- 通过ASM（Auto Snapshot Manager）去做【会将内存中的缓存马上写到硬盘里】

EQL Clone - 克隆

- 同一Group里的不同Pool的某个时间点的备份
- 通过ASM来做

EQL Sync Replication[  - ]同步复制

- 同一Group里的不同Pool的备份（镜像，随时的同步复制备份）
- 通过ASM来做

EQL Async Replication - 异步复制

- 不同Group之间的Volume远程复制（容灾，并不是随时同步的，会有些时间上的偏差）
- 通过ASM来做

 

已使用 OneNote 创建。
