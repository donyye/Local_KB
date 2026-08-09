ESXI 硬件监控 IPMI

2021年4月23日

10:54

 

客户发现ESXI在监控硬件运行情况上到有些硬件不能被监控。

 

![Machine generated alternative text: esxi_d_ddoonny RHEL76_A RHEL76_a Test-2016 Win-2016 Physical_machine Testlll t\] 10.10.4051 EQ-A PXE_TEST RH75-Core RHEL6g-A Win2003 Win2016_AD Cent0S7_7 CPUinfo ESXI-AC ESXI_OI ESXl_02 ESXl_03 ESXl_04 ESXI_AA ESXI_AB ESXI_AD Bios . BIOS PowerEdge R740. GYHDPR2. the: 4c4c4544-oosg-4810-8044-c7c04t505232, Asset Tag: o 145 SEL CPU Skyline 0.7.1.117 0.7.1.62 0.7.1.63 0.7.1.64 0.7.1.65 0.7.1.66 0.7.1.67 0.10 1 133 0.11.1_74 0.11.2_88 0.11.2_136 System Board 1 System Board 1 System Board 1 System Board 1 System Board 1 System Board 1 System Board 1 Power Optimized O Fanl Status O Fan2 Status O Fans Status O Fan4 Status O Fans status O Fan6 Status O Power Supply 1 Status O Add-in Card 1 Presence O Add-in Card 2 Presence O Add -in card 2 ROMB Battery O SystemBoard SystemBoard SystemBoard SystemBoard SystemBoard SystemBoard SystemBoard Power Other Other Battery 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 2021/04/12 ](attachments/Technology_ALL_VMware_分析案例_122_ESXI%20硬件监控%20IPMI_001.png)

 

这些信息是硬件厂商自定义的，在ESXI上是看不到的。

运行命令：

 esxcli hardware ipmi sdr list

 

![[Technology_ALL_VMware_分析案例_122_ESXI 硬件监控 IPMI_002.png]]

 

因为ESXI是使用 IPMI的命令来获取硬件信息，而这些OEM reserved是不能通过IPMI命令来获取，所以在硬件监控里是不能被看到。

 

FYI:

[https://kb.vmware.com/s/article/57171?lang=en_us](https://kb.vmware.com/s/article/57171?lang=en_us)

[https://kb.vmware.com/s/article/1010716](https://kb.vmware.com/s/article/1010716)

![[Technology_ALL_VMware_分析案例_122_ESXI 硬件监控 IPMI_003.png]]

 

 

已使用 OneNote 创建。
