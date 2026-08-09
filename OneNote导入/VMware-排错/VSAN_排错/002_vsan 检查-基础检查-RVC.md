vsan 检查-基础检查-RVC

2023年11月17日

16:10

=====RVC ==ssh VC

\# rvc administrator@vsphere.local@localhost

\> cd 1

/localhost\> ls

0 Data_CCD (datacenter)

/localhost\> cd 0

/localhost/Data_CCD\> ls

0 storage/

1 computers \[host\]/

2 networks \[network\]/

3 datastores \[datastore\]/

4 vms \[vm\]/

/localhost/Data_CCD\> cd 1

/localhost/Data_CCD/computers\> ls

0 HA_HOST (cluster): cpu 37 GHz, memory 17 GB

/localhost/Data_CCD/computers\> cd 0

/localhost/Data_CCD/computers/HA_HOST\> vsan.support_information ./[  \--\> ]在线查看

\..... 有大量信息输出

 

检查虚拟机和 vSAN 对象是否有效且不可访问组件检查。

 

/localhost/Data_CCD/computers/HA_HOST\> vsan.check_state  ./

\# check_state 可以执行，但是如果加了-r参数 ,修不了就直接给清掉VM注册，一定要提前把那些异常的VM的名字留下来。

/localhost/Data_CCD/computers/HA_HOST\> vsan.check_state -r ./

 

vsan.resync_dashboard ./[  \--\> ]看是否有同步

vsan.check_state ./[  \--\> ]看是否有组件错误

vsan.disks_stats ./[  \--\> ]查看disk状态

 

 

 

已使用 OneNote 创建。
