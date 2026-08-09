RHEL8-HA-fences

2023年12月27日

12:48

====VMware_fence====

 

》查看支持fence种类

[\[root@rh8b \~\]# pcs stonith list \|grep fence_vmware]

fence_vmware_rest - Fence agent for VMware REST API

fence_vmware_soap - Fence agent for VMWare over SOAP API

 

[\[root@rh8a \~\]# pcs cluster cib stonith_c]

 

使用 fence_vmware_rest 创建虚拟fences

[\[root@rh8a \~\]# pcs stonith create vmfence fence_vmware_rest pcmk_host_map=\"rh8a.example.com:node1-vm;rh8b.example.com:node2-vm\" ipaddr=10.10.40.250 ssl=1 login=administrator@vsphere.local passwd=P]xxxxxx! ssl_insecure=1

Warning: stonith option \'ipaddr\' is deprecated and should not be used, use \'ip\' instead

Warning: stonith option \'login\' is deprecated and should not be used, use \'username\' instead

Warning: stonith option \'passwd\' is deprecated and should not be used, use \'password\' instead

# pcmk_host_map vm主机名，ipaddr \--\> vCenter IP ,  login \--\> vCenter user , passwd \--\> vCenter password

#  上面的 Warning 是提示你有些产生发生了改变，建议使用新的写法。

\# 删除资源命令 pcs stonith delete \<资源名字\>

 

[\[root@rh8a \~\]# pcs stonith config vmfence]

 Resource: vmfence (class=stonith type=fence_vmware_rest)

  Attributes: ipaddr=10.10.40.250 login=administrator@vsphere.local passwd=P@ssw0rd! pcmk_host_map=rh8a.example.com:node1-vm;rh8b.example.com:node2-vm ssl=1 ssl_insecure=1

  Operations: monitor interval=60s (vmfence-monitor-interval-60s)

 

[\[root@rh8a \~\]# pcs stonith status]

  \* vmfence    (stonith:fence_vmware_rest):     Started rh8b.example.com

 

测试：

例子： \# fence_vmware_rest -a \<vCenter IP address\> -l \<vcenter_username\> -p \<vcenter_password\> \--ssl-insecure -z -o list \| egrep \"(node1-vm\|node2-vm)\"\
 

\# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o list \|egrep \"(RHEL8_A\|RHEL8_B)\"

RHEL8_A,

RHEL8_B,

 

\# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o status -n RHEL8_A

Status: ON[   ]\<\-- 能获取到状态

 

[ \#] fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o status -n RHEL8_B

Status: ON[   \<\-- ]能获取到状态

 

使用 fence 来让 RHEL8_B 重启

\# fence_vmware_rest -a 10.10.40.250 -l \"administrator@vsphere.local\" -p \"Pxxxxxxx!\" \--ssl-insecure -z -o reboot -n RHEL8_B

Success: Rebooted[   \<\-- ]重启对方成功

 

\# pcs stonith config vmfence

Resource: vmfence (class=stonith type=fence_vmware_rest)

  Attributes: vmfence-instance_attributes

[    ipaddr=10.10.40.250][    ]\<\-- VC IP

[    login=administrator@vsphere.local][    ]\<\-- VC username

[    passwd=P]xxxxxxx![         ]\<\-- VC passwd

    pcmk_host_map=rh8a.example.comnode1-vm;rh8b.example.com:node2-vm

    ssl=1

    ssl_insecure=1

  Operations:

    monitor: vmfence-monitor-interval-60s

      interval=60s

 

\# pcs property set stonith-enabled=true

 

通过echo测试，fence 能成功fence对方

echo c \> /proc/sysrq-trigger

 

 

====fence_SBD====

\# dnf[  install sbd]

 

所有node开启watchdog：

\# echo softdog \> /etc/modules-load.d/watchdog.conf

\# systemctl restart systemd-modules-load

\# lsmod \| grep dog

softdog                16384  0

 

修改 sbd 配置：

\# vim /etc/sysconfig/sbd

SBD_DEVICE=\"/dev/mapper/mpathX\"  \--\> share disk

Notice: 共享盘不能是lvm 和 RAID

\# sbd -d /dev/mapper/mpatha create

\# sbd -d /dev/mapper/mpatha list

\# pcs stonith create sbd_fencing fence_sbd devices=/dev/mapper/mpathX

 

\# systemctl enable sbd

 

![[Linux-TEDP-2018_2025_SEDP-2023_012_RHEL8-HA-fences_001.png]]

 

 

[\[root@localhost \~\]# pcs stonith sbd status \--full]

SBD STATUS

\<node name\>: \<installed\> \| \<enabled\> \| \<running\>

rh89.ha.com: YES \| YES \| YES

rh88.ha.com: YES \| YES \| YES

 

Messages list on device \'/dev/mapper/mpatha\':

0        localhost.localdomain        clear        

 

 

SBD header on device \'/dev/mapper/mpatha\':

==Dumping header on disk /dev/mapper/mpatha

Header version     : 2.1

UUID               : 263c3000-9793-402d-8944-6e796beab67e

Number of slots    : 255

Sector size        : 512

Timeout (watchdog) : 5

Timeout (allocate) : 2

Timeout (loop)     : 1

Timeout (msgwait)  : 10

==Header on disk /dev/mapper/mpatha is dumped

 

 

没有添加watchdog时启动 pacemaker 时的错误：

[\[root@localhost \~\]# journalctl -u sbd -f]

\-- Logs begin at Wed 2023-12-27 13:24:48 CST. \--

Dec 27 13:34:49 localhost.localdomain sbd\[3108\]: /dev/mapper/mpatha:   notice: servant_md: Monitoring slot 0 on disk /dev/mapper/mpatha

Dec 27 13:34:49 localhost.localdomain sbd\[3110\]:    cluster:   notice: servant_cluster: Monitoring corosync cluster health

Dec 27 13:34:49 localhost.localdomain sbd\[3110\]:    cluster:   notice: verify_against_cmap_config: Corosync is in 2Node-mode

Dec 27 13:34:49 localhost.localdomain sbd\[3105\]:   notice: inquisitor_child: Servant cluster is healthy (age: 0)

Dec 27 13:34:50 localhost.localdomain sbd\[3105\]:   notice: watchdog_init: Using watchdog device \'/dev/watchdog\'

Dec 27 13:34:50 localhost.localdomain systemd\[1\]: Started Shared-storage based fencing daemon.

Dec 27 13:43:05 localhost.localdomain sbd\[3110\]:    cluster:  warning: set_servant_health: Connected to corosync but requires both nodes present

Dec 27 13:43:05 localhost.localdomain sbd\[3105\]:  warning: inquisitor_child: cluster health check: UNHEALTHY

Dec 27 13:43:05 localhost.localdomain sbd\[3105\]:  warning: inquisitor_child: Servant cluster is outdated (age: 1096)

Dec 27 13:44:00 localhost.localdomain sbd\[3105\]:   notice: inquisitor_child: Servant cluster is healthy (age: 0)

 

已使用 OneNote 创建。
