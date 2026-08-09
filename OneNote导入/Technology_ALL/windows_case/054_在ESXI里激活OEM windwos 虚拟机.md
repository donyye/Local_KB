在ESXI里激活OEM windwos 虚拟机

2019年10月17日

16:06

如果需要在ESXI里激活OEM windwos VM，需要在ESXI来做修改，然后重启ESXI生效。

SMBIOS.reflectHost = \"TRUE\"

SMBIOS.noOEMStrings = \"TRUE\"

smbios.addHostVendor = \"TRUE\"

 

这些参数到下面添加：

 

![[Technology_ALL_windows_case_054_在ESXI里激活OEM windwos 虚拟机_001.jpg]]

 

 

 

已使用 OneNote 创建。
