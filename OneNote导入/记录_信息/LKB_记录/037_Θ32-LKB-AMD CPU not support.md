Θ32-LKB-AMD CPU not support

2025年7月21日

14:07

Title:

R7515, cpu AMD EPYC 7F72 24-Core Processor , RHEL8.10 error message \"AMD Processor family 23: mcelog does not support this processor\"

 

Summanry: 

This article gives the solution of RHEL8.10 error message :

[Jul 20 10:05:55 FSTrade02-ban mcelog\[1375\]: mcelog: ERROR: AMD Processor family 23: mcelog does not support this processor.  Please use the edac_mce_amd module instead.]

[Jul 20 10:05:55 FSTrade02-ban mcelog\[1375\]: CPU is unsupported]

Jul 20 10:05:55 FSTrade02-ban smartd\[1379\]: smartd 7.1 2019-12-30 r5022 \[x86_64-linux-4.18.0-553.el8_10.x86_64\] (local build)

......

 

Symptoms:\
The system shows that the CPU hardware and system are not supported.

\
Check the compatibility list. R7515 is compatible with RHEL8.10.

 

![[记录_信息_LKB_记录_037_Θ32-LKB-AMD CPU not support_001.png]]

 

Cause:

Newer AMD processors do not support the mcelog daemon. With the release of RHEL 6.3 and the update of mcelog to mcelog-1.0pre3_20110718-0.14.el6, mcelog now properly reports an error against these newer AMD processors.

 

Redhat KB:

<https://access.redhat.com/solutions/158503>

 

 

Resolution:

You can try to fix it by loading the module, according to Redhat KB.

 

![[记录_信息_LKB_记录_037_Θ32-LKB-AMD CPU not support_002.png]]

 

 

Keywords: 

R7515,RHEL8.10,error message,AMD Processor family 23: mcelog does not support this processor

 

 

已使用 OneNote 创建。
