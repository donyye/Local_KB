VC 关于vgpu

2023年5月22日

11:31

1. vgpu 现在已经是个vm，直接导入到ESXI使用。\
\
2. 查看VC是否有开启vgpu的支持

这是在VC上的全局配置，不需要对vm一个个设置。

![[VMware-排错_vCenter_排查_005_VC 关于vgpu_001.png]]

 

要勾选

![[VMware-排错_vCenter_排查_005_VC 关于vgpu_002.png]]

 

3. 要检测BIOS是否有开启

VT-D    \--enable

IOMMU    \--enable

Memory Mapped I/O above 4GB    \-\--enable

Memory Mapped I/O Base     \-\--512GB

SR-IOV  capable        \-\--enable

\# 另外更换了主板后也有可能没开启导致带vgpu的vm无法在这个host启动。 

 

已使用 OneNote 创建。
