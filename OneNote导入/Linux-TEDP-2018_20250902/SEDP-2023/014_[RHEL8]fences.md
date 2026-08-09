\[RHEL8\]fences

2024年2月19日

17:21

[\
\[root@ha01 \~\]#] pcs stonith create vmfence fence_vmware_rest pcmk_host_map=\"ha01:node1-vm;ha02:node2-vm\" ipaddr=\"10.10.40.250\" ssl=1 login=administrator@vsphere.local passwd=P@ssw0rd! ssl_insecure=1

 

 

 

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

 

 

 

 

已使用 OneNote 创建。
