2023年4月10日

14:01

\[root@localhost:\~\] esxcli network vswitch dvs vmware list

DSwitch

   Name: DSwitch

   VDS ID: 50 17 d4 16 3d ac 54 90-c8 d0 0a 7a 4f 3c bd 0e

   Class: cswitch

   Num Ports: 2290

   Used Ports: 6

   Configured Ports: 512

   MTU: 1500

   CDP Status: listen

   Beacon Timeout: -1

   Uplinks: vmnic4

   VMware Branded: true

   DVPort:

         Client: vmnic4

         DVPortgroup ID: dvportgroup-11014

         In Use: true

         Port ID: 16

      

         Client:

         DVPortgroup ID: dvportgroup-11014

         In Use: false

         Port ID: 17

      

         Client:

         DVPortgroup ID: dvportgroup-11014

         In Use: false

         Port ID: 18

      

         Client:

         DVPortgroup ID: dvportgroup-11014

         In Use: false

         Port ID: 19

      

         Client:

         DVPortgroup ID: dvportgroup-11014

         In Use: false

         Port ID: 27

      

         Client:

         DVPortgroup ID: dvportgroup-11014

         In Use: false

         Port ID: 26

      

         Client: vcsa7.eth0

         DVPortgroup ID: dvportgroup-11015

         In Use: true

         Port ID: 0

\[root@localhost:\~\] \^C

 

 

\[root@localhost:\~\] esxcli network vswitch standard list

vSwitch0

   Name: vSwitch0

   Class: cswitch

   Num Ports: 2290

   Used Ports: 6

   Configured Ports: 128

   MTU: 1500

   CDP Status: listen

   Beacon Enabled: false

   Beacon Interval: 1

   Beacon Threshold: 3

   Beacon Required By:

   Uplinks: vmnic3, vmnic0

   Portgroups: VM Network, Management Network

 

vSwitch1

   Name: vSwitch1

   Class: cswitch

   Num Ports: 2290

   Used Ports: 4

   Configured Ports: 128

   MTU: 1500

   CDP Status: listen

   Beacon Enabled: false

   Beacon Interval: 1

   Beacon Threshold: 3

   Beacon Required By:

   Uplinks: vmnic1

   Portgroups: VMkernel-vsan

 

vSwitch2

   Name: vSwitch2

   Class: cswitch

   Num Ports: 2290

   Used Ports: 4

   Configured Ports: 128

   MTU: 1500

   CDP Status: listen

   Beacon Enabled: false

   Beacon Interval: 1

   Beacon Threshold: 3

   Beacon Required By:

   Uplinks: vmnic2

   Portgroups: VMkernel-vMotion

   

\# esxcli network ip interface list

 

 

 

\[root@localhost:\~\] esxcli network nic list

Name    PCI Device    Driver    Admin Status  Link Status  Speed  Duplex  MAC Address         MTU  Description

\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\--  \-\-\-\--  \-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\--  \-\-\-\-\-\-\-\-\-\--

vmnic0  0000:0b:00.0  nvmxnet3  Up            Up           10000  Full    00:50:56:89:a2:3b  1500  VMware Inc. vmxnet3 Virtual Ethernet Controller

vmnic1  0000:13:00.0  nvmxnet3  Up            Up           10000  Full    00:50:56:89:7e:a3  1500  VMware Inc. vmxnet3 Virtual Ethernet Controller

vmnic2  0000:1b:00.0  nvmxnet3  Up            Up           10000  Full    00:50:56:89:2b:d3  1500  VMware Inc. vmxnet3 Virtual Ethernet Controller

vmnic3  0000:04:00.0  nvmxnet3  Up            Up           10000  Full    00:50:56:89:a6:f5  1500  VMware Inc. vmxnet3 Virtual Ethernet Controller

vmnic4  0000:0c:00.0  nvmxnet3  Up            Up           10000  Full    00:50:56:a0:98:93  1500  VMware Inc. vmxnet3 Virtual Ethernet Controller

 

 

\[root@localhost:\~\] esxcfg-vswitch -l

Switch Name      Num Ports   Used Ports  Configured Ports  MTU     Uplinks   

vSwitch0         2290        6           128               1500    vmnic0,vmnic3

 

  PortGroup Name                            VLAN ID  Used Ports  Uplinks   

  VM Network                                0        0           vmnic0,vmnic3

  Management Network                        0        1           vmnic0,vmnic3

 

Switch Name      Num Ports   Used Ports  Configured Ports  MTU     Uplinks   

vSwitch1         2290        4           128               1500    vmnic1    

 

  PortGroup Name                            VLAN ID  Used Ports  Uplinks   

  VMkernel-vsan                             0        1           vmnic1    

 

Switch Name      Num Ports   Used Ports  Configured Ports  MTU     Uplinks   

vSwitch2         2290        4           128               1500    vmnic2    

 

  PortGroup Name                            VLAN ID  Used Ports  Uplinks   

  VMkernel-vMotion                          0        1           vmnic2    

 

DVS Name         Num Ports   Used Ports  Configured Ports  MTU     Uplinks   

DSwitch          2290        4           512               1500              

 

  DVPort ID                               In Use      Client      

  16                                      0           

  17                                      0           

  18                                      0           

  19                                      0           

  27                                      0           

  26                                      0           

  0                                       1           vcsa7.eth0

 

 

删除DVS上的上行网卡：

\[root@localhost:\~\] esxcfg-vswitch -Q vmnic4 -V 16 DSwitch

\# \[root@localhost:\~\] esxcfg-vswitch -Q vmnicX -V \<Port ID\> \<dvSwitch\>

 

 

 

已使用 OneNote 创建。
