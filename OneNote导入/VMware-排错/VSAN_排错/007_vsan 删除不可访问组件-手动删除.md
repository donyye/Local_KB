vsan 删除不可访问组件-手动删除

2023年10月23日

10:19

vsan有运行对象无法清除

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_001.png]]

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

1\. ssh vcsa login RVC

2. ssh ESXI and find the wrong component and clear it.

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

1. 尝试SSH登录到VCSA上通过RVC来清除

[root@vc7 \[ \~ \]# rvc administrator@vsphere.local@localhost]

\[DEPRECATION\] This gem has been renamed to optimist and will no longer be supported. Please switch to optimist as soon as possible.

Install the \"ffi\" gem for better tab completion.

password: 

0 /

1 localhost/

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

0 HA_HOST (cluster): cpu 36 GHz, memory 41 GB

/localhost/Data_CCD/computers\> cd 0

 

通过下面命令找到错误的组件信息

/localhost/Data_CCD/computers/HA_HOST\> vsan.check_state \~/computers/HA_HOST/

2023-10-23 10:41:10 +0800: Step 1: Check for inaccessible vSAN objects

Detected 2 objects to be inaccessible

Detected f8f64964-7cf8-0c28-b23a-005056895df1 on 10.10.40.93 to be inaccessible

Detected f6f64964-4ebb-1e6f-efdf-005056895df1 on 10.10.40.93 to be inaccessible

 

2023-10-23 10:41:10 +0800: Step 2: Check for invalid/inaccessible VMs

 

2023-10-23 10:41:10 +0800: Step 3: Check for VMs for which VC/hostd/vmx are out of sync

Did not find VMs for which VC/hostd/vmx are out of sync

 

通过下面的命令进行清除

/localhost/Data_CCD/computers/HA_HOST\> vsan.purge_inaccessible_vswp_objects \~/computers/HA_HOST/

2023-10-23 10:45:33 +0800: Collecting all inaccessible vSAN objects\...

2023-10-23 10:45:33 +0800: Found 0 inaccessbile objects.

 

通过上面方式还是无法清除，还是存在，这些对象有可能是 vmdk 组件，所以无法通过上述方式清除。

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_002.png]]

 

2. 在ESXI 上检查

需要在每个node上试，看是那个node上的组件。找对了会看到下面信息

[\[root@localhost:\~\] /usr/lib/vmware/osfs/bin/objtool getAttr \--bypassDom -u f6f64964-4ebb-1e6f-efdf-005056895df1 ]

Object Attributes \--

 

UUID:f6f64964-4ebb-1e6f-efdf-005056895df1

 

Object type:vsan

 

Object size:273804165120

 

Allocation type:Thin

 

Policy:((\\\"stripeWidth\\\" i1) (\\\"cacheReservation\\\" i0) (\\\"proportionalCapacity\\\" (i0 i100)) (\\\"hostFailuresToTolerate\\\" i1) (\\\"forceProvisioning\\\" i0) (\\\"spbmProfileId\\\" \\\"aa6d5a82-1c88-45da-85d3-3d74b91a5bad\\\") (\\\"spbmProfileGenerationNumber\\\" l+0) (\\\"spbmProfileName\\\" \\\"vSAN Default Storage Policy\\\"))

 

Object class:vmnamespace

 

Object capabilities:NONE

 

Object path:/vmfs/volumes/vsan:52557adcdba70136-901e6d2b9c417ea6/AD_Win2019_254

 

Group uuid:00000000-0000-0000-0000-000000000000

 

Container uuid:00000000-0000-0000-0000-000000000000

 

IsSparse:0

 

User friendly name:AD_Win2019_254

 

HA metadata:(null)

 

Filesystem type: vmfs5d

 

[\[root@localhost:\~\] /usr/lib/vmware/osfs/bin/objtool delete -u f8f64964-7cf8-0c28-b23a-005056895df1 -f]

成功后没有任何输出

再看vsan已经少了一个

 

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_003.png]]

 

[\[root@localhost:\~\] /usr/lib/vmware/osfs/bin/objtool delete -u f6f64964-4ebb-1e6f-efdf-005056895df1 -f]

再删除另外一个，此时再重新检查vsan健康状态就没有了。

 

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_004.png]]

 

 

FYI：

<https://www.johnborhek.com/vmware/vsan/identifying-and-removing-inaccessible-vsan-objects/>

 

 

 

 

=========================================

测试环境里再次出现的组件错误

 

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_005.png]]

 

 

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_006.png]]

 

 

\[root@localhost:\~\] /usr/lib/vmware/osfs/bin/objtool delete -u 938e1462-24f1-2607-1561-005056897607 -f

\[root@localhost:\~\] /usr/lib/vmware/osfs/bin/objtool delete -u 4e692365-5670-942e-2129-00505689a6f5 -f

 

 

删除所有组件后，删除不可访问的vm后再重新注册，开机提示"找不到文件 系统找不到指定的文件 无法枚举所有磁盘"。vm无法正常启动。

![[VMware-排错_VSAN_排错_007_vsan 删除不可访问组件-手动删除_007.png]]

 

无法找到的文件就是被删除的错误组件

[\[root@localhost:/vmfs/volumes/vsan:52557adcdba70136-901e6d2b9c417ea6/4c692365-e846-c046-1f98-00505689a6f5\] grep -ir \'failed to open\' vmware.log ]

2023-10-23T03:54:27.731Z In(05) vmx - MKSXlib: Failed to open libXi.so.6

2023-10-23T03:54:27.994Z In(05) vmx - AIOGNRC: Failed to open \'/vmfs/devices/vsan/4e692365-5670-942e-2129-00505689a6f5\' : Could not find the file (60003) (0x2013).

2023-10-23T04:14:27.356Z In(05) vcpu-0 lnx6ehce-42910-auto-x40-h5:70004075-b2-a9-003a AIOGNRC: Failed to open \'/vmfs/devices/vsan/4e692365-5670-942e-2129-00505689a6f5\' : Could not find the file (60003) (0x2013).

2023-10-23T04:53:35.688Z In(05) vcpu-0 - AIOGNRC: Failed to open \'/vmfs/devices/vsan/4e692365-5670-942e-2129-00505689a6f5\' : Could not find the file (60003) (0x2013).

2023-10-23T05:47:25.055Z In(05) vcpu-0 - AIOGNRC: Failed to open \'/vmfs/devices/vsan/4e692365-5670-942e-2129-00505689a6f5\' : Could not find the file (60003) (0x2013).

 

 

 

已使用 OneNote 创建。
