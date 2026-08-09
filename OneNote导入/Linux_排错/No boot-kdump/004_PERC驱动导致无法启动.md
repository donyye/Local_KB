PERC驱动导致无法启动

2024年3月27日

17:29

 

CentOS 8.7 开机后无法正常进入系统，不停在下面界面循环

![[No boot-kdump_004_PERC驱动导致无法启动_001.png]]

 

得知之前有做个PERC驱动的更新，报错也显示有PERC卡启动的错误（mpi3mr）。

 

\# lspci -vvv

Subsystem: Dell PERC H965i Adapter

        Control: I/O+ Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+

        Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast \>TAbort- \<TAbort- \<MAbort- \>SERR- \<PERR- INTx-

        Latency: 0

        NUMA node: 0

        Region 0: Memory at b4000000 (64-bit, prefetchable) \[size=16K\]

        Expansion ROM at \<ignored\> \[disabled\]

....

 

进入系统自带的recuse模式，查看目前所匹配的驱动，已经是跟新，但是 rhelversion 显示版本是8.8,但是系统是 8.7，应该是驱动安装错误。

![[No boot-kdump_004_PERC驱动导致无法启动_002.png]]

 

![[No boot-kdump_004_PERC驱动导致无法启动_003.png]]

 

检查之前 history 命令，看到之前所安装的rpm包信息

 rpm -Uvh signed_rhel8/rpms-1/kmod-mpi3mr-8.4.4.0.0_el8.8-1.x86_64.rpm

 

通过删除之前安装的驱动，会自动匹配回原来的inbox驱动。

\# rpm -e kmod-mpi3mr

![[No boot-kdump_004_PERC驱动导致无法启动_004.png]]

 

 

再重启系统，能成功进入

 

 

 

已使用 OneNote 创建。
