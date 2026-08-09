027-LKB-VM_GPU

2025年3月31日

16:11

LKB-000319060\| 提交时间 2025-05-09 \| Q2 \
\
Title:

PowerEdge: RHEL8.X VM in use cannot use passthrough GPU.

 

Summanry: 

This article gives the solution of RHEL8.X VM in use cannot use passthrough GPU.

 

Symptoms:

The virtual machine fails to recognize the passthrough GPU after startup.

 

Cause:

N/A

 

Resolution:

Checked syslog sosreport no problem found. Checked vm-support log, MMIO setup parameter in vm_name.xmx is correct, no problem with setup.

pciPassthru1.present = \"TRUE\"

pciPassthru1.pciSlotNumber = \"256\"

 

This issue has nothing to do with adding multiple passthru GPUs, the problem is in adding the wrong MMIO profile.

By editing vm, advanced options, see add parameter

pciPassthru.use64bitMMIO = \"TRUE\"

pciPassthru.use64bitMMIOSizeGB = \"256\"

you can find that pciPassthru.use64bitMMIOSizeGB is wrongly written with an extra use The correct one is

pciPassthru.use64bitMMIO = \"TRUE\"

pciPassthru.64bitMMIOSizeGB = \"256\"

 

Then test it again, add all GPUs, and get the information successfully.

![[记录_信息_LKB_记录_029_027-LKB-VM_GPU_001.png]]

 

 

Keywords: 

PowerEdge,RHEL8.x,GPU,Nvidia

 

已使用 OneNote 创建。
