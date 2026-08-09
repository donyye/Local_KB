Θ-LKB-无法克隆vm

2024年3月14日

10:09

ISSUE：

VCSA6.7 因为 /storage/log 磁盘空间满出现异常，无法克隆vm。

![[记录_信息_LKB_记录_035_Θ-LKB-无法克隆vm_001.png]]

 

 

Solution：

进入 /storage/log 目前查看各个文件夹大小。

\# cd /storage/log

\# du -hSx /storage/log \|sort -rh \|head -30

 

最后确认在 /storage/log/vmware/content-library 有15G大小

![[记录_信息_LKB_记录_035_Θ-LKB-无法克隆vm_002.png]]

 

 

手动清楚

\# cd /storage/log/vmware/content-library/

\# echo \> content-library-runtime.log.stdout  

 

将 /etc/vmware-content-library/log4j.properties 中的内容替换为KB附件的文件的内容：

<https://kb.vmware.com/s/article/89009>

\# cd /etc/vmware-content-library/

\# cp -R log4j.properties log4j.properties.old

\# cat /dev/null \> log4j.properties

\# vi log4j.properties - 插入本文件所附文件中的数据

 

![[记录_信息_LKB_记录_035_Θ-LKB-无法克隆vm_003.png]]

 

验证文件所有权/权限，并做出相应更改：

\# chown content-library:cis log4j.properties

\# chmod 640 log4j.properties

 

重新启动内容库服务：

\# service-control \--restart content-library

 

重启服务成功后会自动清除成功

![[记录_信息_LKB_记录_035_Θ-LKB-无法克隆vm_004.png]]

 

 

KB：https://kb.vmware.com/s/article/89009\
[https://knowledge.broadcom.com/external/article?legacyId=89009](https://knowledge.broadcom.com/external/article?legacyId=89009)

 

 

已使用 OneNote 创建。
