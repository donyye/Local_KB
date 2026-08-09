\[RHEL9\]fences-vm

2023年12月26日

17:16

 

[\[root@r61 \~\]#] pcs stonith create vmfence fence_vmware_rest pcmk_host_map=\"r61-ha:node1-vm;r62-ha:node2-vm\" ipaddr=10.10.40.250 ssl=1 login=administrator@vsphere.local passwd=Pxxxxxxx! ssl_insecure=1\
 

\[root@r61 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o list \|egrep \"(RHEL9_1_A\|RHEL9_1_B)\"

RHEL9_1_A,

RHEL9_1_B,

\
 

\[root@r61 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o status -n RHEL9_1_B

Status: ON

\
\
\[root@r62 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o status -n RHEL9_1_A

Status: ON

 

pcs property set stonith-enabled=true

 

测试reboot

\[root@r61 \~\]# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o reboot -n RHEL9_1_B

Success: Rebooted

 

 

 

已使用 OneNote 创建。
