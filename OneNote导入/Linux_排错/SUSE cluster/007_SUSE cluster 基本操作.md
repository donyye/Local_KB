SUSE cluster 基本操作

2024年4月2日

11:14

SUSE cluster 基本操作

<https://documentation.suse.com/sle-ha/15-SP1/html/SLE-HA-all/cha-ha-maintenance.html>

 

1.cluster得启动和停止

crm cluster start

Start cluster services on one node

crm cluster stop

Stop cluster services on one node

crm cluster restart

Restart cluster services on one node

crm cluster status

View status of the cluster stack on one node

 

 

2. cluster node 维护步骤

1.检查集群状态，首先确保集群是健康的。

\# crm status

 

2. 将节点置于standby状态。

\# crm node standby NODENAME

或是当前节点crm -w node standby

 

如果节点正在运行MASTER：

3. 它将SAP HANA 实例故障转移到另一个节点。

此为自动，只需crm status 查看状态

 

4. 关闭服务器并进行维护操作。

 

5、维护后启动机器。

 

6. 注册 SAP HANA 复制并确保数据同步。

此为SAP的范围，需要用SAP的命令hdbnsutil，或在SAP HANA studio操作，设置从对面节点同步复制数据到此节点

如链接中的范例

 

Manually register the old primary (on node 1) with the new primary after takeover (on node 2) as \<sid\>adm.

 

suse01:\~\> hdbnsutil -sr_register \--remoteHost=suse02 \--remoteInstance=10 \--replicationMode=sync \--operationMode=logreplay \--name=WDF

 

7. 退出待机模式，检查集群状态以确保其健康。

\# crm status

\# crm node online NODENAME

\# crm status

 

如有任何资源报错，可通过以下命令刷新对应资源所在的节点。（替换大写参数）

\# crm resource refresh RESOURCE_NAME NODE_NAME

 

 

8. 如果需要failback，同上failback。

重复1,2,3,6,7的步骤，在对面节点操作，就会切换SAP HANA回到现有节点，

 

如果节点正在运行SLAVE：

3.它将停止服务

此为自动，只需crm status 查看状态

 

4. 重启服务器并进行维护。

服务器在第2部时，已经在standby状态

 

5、维护后开机。

 

6. 检查集群状态是否为最佳并退出待机。

\# crm status

\# crm node online NODENAME

\# crm status

 

已使用 OneNote 创建。
