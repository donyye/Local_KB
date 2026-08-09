[ LKB-]关于vsan磁盘定位和更换

2023年3月29日

11:07

 标题：如何确定被vSAN判定为故障硬盘的插槽位置

LKB：[000190933](https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Knowledge/%E5%A6%82%E4%BD%95%E7%A1%AE%E5%AE%9A%E8%A2%ABvSAN%E5%88%A4%E5%AE%9A%E4%B8%BA%E6%95%85%E9%9A%9C%E7%A1%AC%E7%9B%98%E7%9A%84%E6%8F%92%E6%A7%BD%E4%BD%8D%E7%BD%AE?language=zh_CN)

关键字：vSAN, HD，NAA ID

往期好文请访问：[000211644](https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Knowledge/%E5%A5%BD%E6%96%87%E6%8E%A8%E8%8D%90%E6%96%87%E7%AB%A0%E6%B1%87%E6%80%BB?language=zh_CN)

注意：因LKB文章会不定时更新，需要查看时请进到LKB库获取最新版本的文章

如何判断VSAN环境下故障盘的真实槽位，这个在我们处理中比较经常会遇到，但还是有很多同学不会，记得可以通过LKB查询！

常见的问题，LKB都有收录，文章有用，别忘记attach & solve哦！❤

需要登录LKB搜索文章解锁详细的步骤和高清截图

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_001.jpg]]

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_002.jpg]]

 

 

方法一：

通过磁盘LED定位符 确认

VMware 官方推荐的做法是使用[定位符LED](https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.virtualsan.doc/GUID-E3FDC631-FF56-4E5A-B33B-F40EB85F2290.html) ,但是若定位符LED 无法使用的情况下，请参考方法二和方法三

 

 

方法二：

通过故障磁盘的NAA ID 确认

仅适用于：有故障磁盘的NAA ID 

 

第1步，收集服务器硬件的TSR日志

[Export a SupportAssist collection via iDRAC9](https://www.dell.com/support/kbdoc/000126308/export-a-supportassist-collection-via-idrac9)

[Export a SupportAssist Collection via iDRAC7 and iDRAC8](https://www.dell.com/support/kbdoc/000126803/export-a-supportassist-collection-via-idrac7-and-idrac8)

 

第2步，使用vSphere Client登录到vCenter，选择主机和群集 -\>选择vSAN群集-\> 监控（Monitor） -\>  vSAN -\>物理磁盘（Physical disks） ，获取到故障磁盘的NAA ID

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_003.gif]]

第3步，使用SSH的方式登录ESXi主机执行如下命令esxcfg-mpath -bd 获取故障磁盘的SAS地址

如果是SATA盘比较麻烦，SATA没SAS地址

命令1：

[\[root@ESXi:\~\] ]esxcfg-mpath -bdnaa.5002538f0256392c

naa.5002538f0256392c : Local ATA Disk (naa.5002538f0256392c)

   vmhba3:C0:T9:L0 LUN:0 state:active sas Adapter: 52cea7f0c7760800  Target: 500056b3c0a7f8c7

命令2：

[\[root@ESXi:\~\]] esxcli storage core device physical get  -dnaa.5002538f0256392c

   Physical location：enclosure 1, slot 7

 

注：

(1) naa.5002538f0256392c 需要换成客户实际故障磁盘的NAA ID

(2) 输出的结果中，Target就是故障磁盘对应的SAS地址

(3) 若客户无法执行命令，那么也可以收集ESXi日志，具体参考：（ [收集vCenter Server 6.5和ESXi 6.5主机日志的操作方法](https://www.dell.com/support/kbdoc/000138790/%E6%94%B6%E9%9B%86vcenter-server-6-5%E5%92%8Cesxi-6-5%E4%B8%BB%E6%9C%BA%E6%97%A5%E5%BF%97%E7%9A%84%E6%93%8D%E4%BD%9C%E6%96%B9%E6%B3%95)） ，在ESXi日志的command 文件夹下找到esxcfg-mpath\_-b.txt也能找到故障磁盘的SAS地址

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_004.gif]]

 

第4步，根据获取到的故障磁盘的SAS地址，在TSR日志的attribute 里面确定故障磁盘的槽位

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_005.gif]]

 

 

第5步，根据故障磁盘槽位信息，我们进一步可以获取到故障磁盘的PPID

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_006.gif]]

 

第6步，确认故障磁盘是缓存盘还是容量盘后，参考如下步骤进行更换

更换闪存盘步骤：

（1）  [https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-95E5DAF9-FE36-497B-90B4-DB1CA05FE935.html](https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-95E5DAF9-FE36-497B-90B4-DB1CA05FE935.html)  

（2）[How to replace vSAN capacity disk](https://www.dell.com/support/kbdoc/000122008/how-to-replace-vsan-capacity-disk)

更换容量盘步骤：

（1）  [https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-4E3390C1-6C50-49E5-AEB6-C9BC037979A1.html](https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-4E3390C1-6C50-49E5-AEB6-C9BC037979A1.html)  

（2）[How to replace vSAN cache disk via vSphere Web Client](https://www.dell.com/support/kbdoc/000120252/how-to-replace-vsan-cache-disk-via-vsphere-web-client)

 

 

方法三：

通过正常磁盘的NAA ID 反推故障磁盘槽位

 

第1步，收集服务器硬件的TSR日志

[Export a SupportAssist collection via iDRAC9](https://www.dell.com/support/kbdoc/000126308/export-a-supportassist-collection-via-idrac9)

[Export a SupportAssist Collection via iDRAC7 and iDRAC8](https://www.dell.com/support/kbdoc/000126803/export-a-supportassist-collection-via-idrac7-and-idrac8)

 

第2步，使用vSphere Client登录到vCenter，选择主机和群集 -\>选择vSAN群集-\> 监控（Monitor） -\>  vSAN -\>物理磁盘（Physical disks） ，获取到所有正常磁盘的NAA ID 

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_007.gif]]

 

第3步，使用SSH的方式登录ESXi主机执行命令esxcfg-mpath -bd 获取所有磁盘的SAS地址

[\[root@ESXi:\~\] ]esxcfg-mpath -bd

t10.ATA\_\_\_\_\_DELLBOSS_VD\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_f63e2782d31f001000000000 : Local ATA Disk (t10.ATA\_\_\_\_\_DELLBOSS_VD\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_f63e2782d31f001000000000)

   vmhba2:C0:T0:L0 LUN:0 state:active Local HBA vmhba2 channel 0 target 0

 

naa.500056b3b6377efd : Local DP Enclosure Svc Dev (naa.500056b3b6377efd)

   vmhba3:C0:T11:L0 LUN:0 state:dead sas Adapter: Unavailable Target: Unavailable

 

naa.58ce38ee205e1be9 : Local TOSHIBA Disk (naa.58ce38ee205e1be9)

   vmhba3:C0:T1:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 58ce38ee205e1bea

 

naa.5000c500bcad6443 : Local SEAGATE Disk (naa.5000c500bcad6443)

   vmhba3:C0:T4:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bcad6441

 

naa.58ce38ee205e1bed : Local TOSHIBA Disk (naa.58ce38ee205e1bed)

   vmhba3:C0:T2:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 58ce38ee205e1bee

 

naa.5000c500bcb2b247 : Local SEAGATE Disk (naa.5000c500bcb2b247)

   vmhba3:C0:T3:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bcb2b245

 

naa.5000c500bca2b757 : Local SEAGATE Disk (naa.5000c500bca2b757)

   vmhba3:C0:T8:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bca2b755

 

naa.5000c500bc8c8867 : Local SEAGATE Disk (naa.5000c500bc8c8867)

   vmhba3:C0:T5:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bc8c8865

 

naa.5000c500bca17a1b : Local SEAGATE Disk (naa.5000c500bca17a1b)

   vmhba3:C0:T6:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bca17a19

 

naa.5000c500bcad5d5f : Local SEAGATE Disk (naa.5000c500bcad5d5f)

   vmhba3:C0:T7:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bcad5d5d

 

naa.5000c500bc8cf0bf : Local SEAGATE Disk (naa.5000c500bc8cf0bf)

   vmhba3:C0:T9:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bc8cf0bd

 

eui.0050430000000000 : Local Marvell Processor (eui.0050430000000000)

   vmhba2:C0:T2:L0 LUN:0 state:active Local HBA vmhba2 channel 0 target 2

 

naa.5000c500bca2cba7 : Local SEAGATE Disk (naa.5000c500bca2cba7)

   vmhba3:C0:T10:L0 LUN:0 state:active sas Adapter: 54cd98f05461ff00  Target: 5000c500bca2cba5

 

注：

(1) 输出的结果中，Target就是物理磁盘对应的SAS地址

(2) 若客户无法执行命令，那么也可以收集ESXi日志，具体参考：（ [收集vCenter Server 6.5和ESXi 6.5主机日志的操作方法](https://www.dell.com/support/kbdoc/000138790/%E6%94%B6%E9%9B%86vcenter-server-6-5%E5%92%8Cesxi-6-5%E4%B8%BB%E6%9C%BA%E6%97%A5%E5%BF%97%E7%9A%84%E6%93%8D%E4%BD%9C%E6%96%B9%E6%B3%95)） ，在ESXi日志的command 文件夹下找到esxcfg-mpath\_-b.txt也能找到故障磁盘的SAS地址

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_008.gif]]

 

 

第4步，在TSR日志的attribute 里面找到所有物理磁盘的SAS地址

第5步，TSR日志SAS地址/槽位信息，跟命令esxcfg-mpath -bd中的获取到正常SAS地址进行整合，我们就可以找到所有正常物理磁盘NAA所对应的物理槽位槽位，剩下那个没有匹配关系的就是故障磁盘了

 

![[VMware-排错_VSAN_排错_012_LKB-关于vsan磁盘定位和更换_009.jpg]]

 

 

第6步，确认故障磁盘是缓存盘还是容量盘后，参考如下步骤进行更换

更换闪存盘步骤：

（1）  [https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-95E5DAF9-FE36-497B-90B4-DB1CA05FE935.html](https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-95E5DAF9-FE36-497B-90B4-DB1CA05FE935.html)  

（2）[How to replace vSAN capacity disk](https://www.dell.com/support/kbdoc/000122008/how-to-replace-vsan-capacity-disk)

更换容量盘步骤：

（1）  [https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-4E3390C1-6C50-49E5-AEB6-C9BC037979A1.html](https://docs.vmware.com/cn/VMware-vSphere/6.7/com.vmware.vsphere.vsan-monitoring.doc/GUID-4E3390C1-6C50-49E5-AEB6-C9BC037979A1.html)  

（2）[How to replace vSAN cache disk via vSphere Web Client](https://www.dell.com/support/kbdoc/000120252/how-to-replace-vsan-cache-disk-via-vsphere-web-client)

 

 

已使用 OneNote 创建。
