[\[RHEL8\]fences]-vm

2024年2月19日

17:21

\# yum install fence-agents-all

\# pcs stonith list \--\>[  ]列出所支持的所有fence

\# pcs stonith describe fence_idrac \--\> 查看fence资源使用的参数

\
 

新命令：

[\[root@ha01 \~\]#] pcs stonith create vmfence fence_vmware_rest pcmk_host_map=\"ha01:node1-vm;ha02:node2-vm\" ip=\"10.10.40.250\" ssl=1 username=administrator@vsphere.local password=P@ssw0rd! ssl_insecure=1

 

以前旧命令

[\[root@ha01 \~\]#] pcs stonith create vmfence fence_vmware_rest pcmk_host_map=\"ha01:node1-vm;ha02:node2-vm\" ipaddr=\"10.10.40.250\" ssl=1 login=administrator@vsphere.local passwd=P@ssw0rd! ssl_insecure=1

 

\[root@ha01 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"P@ssw0rd!\" \--ssl-insecure -z -o list \|egrep \"(RHEL89-A\|RHEL89-B)\"

RHEL89-A,

RHEL89-B,

 

 

\[root@ha01 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"P@ssw0rd!\" \--ssl-insecure -z -o status -n RHEL89-A

Status: ON

 

\[root@ha01 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"P@ssw0rd!\" \--ssl-insecure -z -o status -n RHEL89-B

Status: ON

 

\[root@ha01 \~\]# pcs property set stonith-enabled=true

 

 

\[root@ha01 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"P@ssw0rd!\" \--ssl-insecure -z -o reboot -n RHEL89-B

Success: Rebooted

 

 

清除fence记录

\[root@rh88 \~\]# pcs stonith history cleanup

cleaning up fencing-history for node \*

 

 

已使用 OneNote 创建。
