023-ESXI磁盘出现Degraded状态

2024年2月29日

11:00

023-LKB-000223703_2024-0402

\
 

ESXI Warnings with Storage Degraded Status

\
ISSUE：

I see a \'Degraded\' issue with the mounted storage in the ESXi web UI, but there is no corresponding warning message in vCenter.

 

![[记录_信息_LKB_记录_024_023-ESXI磁盘出现Degraded状态_001.png]]

 

Solution：

The reason for this is due to the lack of configured multipath access to the storage.

Reference for multipath access configuration is as follows:

![[记录_信息_LKB_记录_024_023-ESXI磁盘出现Degraded状态_002.png]]

 

![[记录_信息_LKB_记录_024_023-ESXI磁盘出现Degraded状态_003.png]]

 

 

![[记录_信息_LKB_记录_024_023-ESXI磁盘出现Degraded状态_004.png]]

 

![[记录_信息_LKB_记录_024_023-ESXI磁盘出现Degraded状态_005.png]]

 

After completing the configuration, perform a rescan, and the warning message should no longer appear.

![[记录_信息_LKB_记录_024_023-ESXI磁盘出现Degraded状态_006.png]]

 

已使用 OneNote 创建。
