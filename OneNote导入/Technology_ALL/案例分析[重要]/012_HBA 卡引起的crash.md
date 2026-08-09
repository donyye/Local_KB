HBA 卡引起的crash

Friday, July 25, 2014

2:49 PM

 

 

message log 里 Call Trace 信息：

Jul 21 17:47:13 localhost kernel: Call Trace:

Jul 21 17:47:13 localhost kernel: \[\<ffffffff880b673c\>\] :megaraid_sas:megasas_fire_cmd_gen2+0x25/0x48

Jul 21 17:47:13 localhost kernel: \[\<ffffffff8006f1f5\>\] do_gettimeofday+0x40/0x90

Jul 21 17:47:13 localhost kernel: \[\<ffffffff80028adc\>\] sync_page+0x0/0x43

Jul 21 17:47:13 localhost kernel: \[\<ffffffff800647ea\>\] io_schedule+0x3f/0x67

Jul 21 17:47:13 localhost kernel: \[\<ffffffff80028b1a\>\] sync_page+0x3e/0x43

Jul 21 17:47:13 localhost kernel: \[\<ffffffff8006492e\>\] \_\_wait_on_bit_lock+0x36/0x66

Jul 21 17:47:13 localhost kernel: \[\<ffffffff8003ff92\>\] \_\_lock_page+0x5e/0x64

......

......

 

查看这个模块的信息：

#cat sos_commands/kernel/modinfo_vfat_fat_usb_storage_ipmi_devintf_ipmi_si_ipmi_msghandle

\...\...

filename:       /lib/modules/2.6.18-194.el5/kernel/drivers/scsi/megaraid/megaraid_sas.ko

description:    LSI MegaRAID SAS Driver

\...\...

 

 

 

通过第二次crash时间来分析Call Trace 发现 megaraid_sas 模块操作请求sync page时超过120秒，此模块属于HBA卡，之后机器出现了crash，而从客户提供的图片显示同一时间也出现了"Kernel：journal commit I/O error"，所以此次crash怀疑和HBA卡关系比较大。

 

已使用 OneNote 创建。
