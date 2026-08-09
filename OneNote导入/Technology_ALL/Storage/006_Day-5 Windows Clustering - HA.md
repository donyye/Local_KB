Day-5 Windows Clustering - HA

Friday, November 22, 2013

微软自己的群集系统

MSCS属于HA 群集(高可用性)

功能简单描述：当一台机器宕机，会自动切换到另外一台机器上继续跑应用

2003 : 8 nodes

2012 : 32 nodes   (N+1)

重点:

1.生成一个Cluster Hostname / Cluster IP

2.Cluster都会有个共享的磁盘

3.Cluster通常会有两个网络:①群集内部Private网络 ②公共网络

 

群集的角色:

Nodes :

节点，几台服务器

Cluster Service

Resource:  

群集所有的资源设备

States: Offline, Offline Pending , Online , Online Pending , and Failed

Shared Disks - Quorum Disk:

①记录了谁将来控制物理磁盘的信息

②提供哪些node能访问哪些物理磁盘

③用NTFS的文件系统

④一般情况下2G的大小足以

\*如果这个文件坏掉了，整个Cluster都会坏掉

Private network:

专门用来跑群集之间的数据

 

共享盘符的条件：

- All shared disks, including the quorum disk, are physically attached to a shared bus or buses. 
- Disks attached to the shared bus can be seen from all of the nodes. You can verify that you can see all of the nodes at the host adapter setup level
- Shared disks are configured as basic, not dynamic. 
- All disks must be formatted NTFS. 
- Drive letters should match between nodes

 

 

 

已使用 OneNote 创建。
