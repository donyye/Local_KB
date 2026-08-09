VCSA 扩展lun的正确操作

2023年8月10日

11:19

Storage 端的LUN扩容

                在共享该LUN的所有主机Storage adapter下依次执行rescan (一次只在一台主机上执行rescan)，确认所有主机已经识别到了该LUN扩容后的容量。

                等待10到15分钟，再次依次在所有主机Storage adapter下依次执行rescan (一次只在一台主机上执行rescan)，确认所有主机已经识别到了该LUN扩容后的容量且没有异常。

                在一台ESXi主机上执行VMFS扩容动作。

                在其他ESXi主机上执行VMFS rescan动作。

 

补充：请注意同一个LUN  mapping共享给所有的ESXi主机，需要保证所有的主机看到的该LUN的ID是同一个。

 

 

另外补充一个KB

通过 vCenter Server 扩展或增加数据存储失败 (1011754)

<https://kb.vmware.com/s/article/1011754?lang=zh_cn>

 

 

 

 

已使用 OneNote 创建。
