Θ33-LKB-disk ordering problem

2025年8月25日

9:45

Title:

PowerEdge: RHEL8.x RHEL9.x RHEL10.x disk ordering problem.

 

Summanry: 

This article gives the solution of RHEL8.x RHEL9.x RHEL10.x disk ordering problem.

 

Symptoms:

The name of the boot disk for installing the system is often not sda. This phenomenon is more obvious in RAID and non-RAID environments.

 

Cause:

This issue is related to which device the kernel recognizes first after scanning the devices.

 

Resolution:

Therefore, in RHEL8, RHEL9, and RHEL10, we need to make some configurations or modifications to ensure that the system installation boot disk is fixed as sda.\
\
Environment R750 + H755F

 

Two methods of setting:

 

1) Firmware Device Order \--\> enabled

<https://infohub.delltechnologies.com/zh-cn/p/firmware-device-order-for-perc-h750-h755-h350-and-h355-storage-controllers-linux-only/>

 

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_001.png]]

\
 

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_002.png]]

 

 

2) sd_mod.probe=\"sync\"

<https://access.redhat.com/solutions/44389#scsi_sync>

 

 

Different configuration methods for RHEL8, RHEL9, and RHEL10.

\
 

=== RHEL8.X ===\
 

Firmware Device Order \--\> enabled

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_003.png]]

 

Firmware Device Order \--\> disable

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_004.png]]

 

 

=== RHEL9.X ===

 

RHEL9.X requires the following additions: sd_mod.probe=\"sync\"

Firmware Device Order does not need to be modified; both disable and enabled are acceptable.

 

Firmware Device Order enabled but sd_mod.probe="sync" has not been added.

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_005.png]]

 

Add sd_mod.probe="sync"

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_006.png]]

 

Modify method:

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_007.png]]

\
Then, save the changes with \# grub2-mkconfig -o /boot/grub2/grub.cfg \--update-bls-cmdline.

 

\
==== RHEL10.X[  ====]

 

Just change \"firmware device order\" to \"enable\".

![[记录_信息_LKB_记录_039_Θ33-LKB-disk ordering problem_008.png]]

 

 

Therefore, based on testing, different system versions will have different settings, but all can ensure that the boot disk position of the installed system is sda.

 

Red Hat recommends using UUIDs or aliases to mount and use disks.\
<https://docs.redhat.com/zh-cn/documentation/red_hat_enterprise_linux/8/html-single/managing_storage_devices/index#con_disadvantages-of-non-persistent-naming-attributes_assembly_overview-of-persistent-naming-attributes>

 

 

Keywords: 

PowerEdge,RHEL8,RHEL9,RHEL10,H755F

 

已使用 OneNote 创建。
