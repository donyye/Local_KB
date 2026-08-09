025-LKB-SUSE_kdump_issue

2025年3月19日

16:06

LKB-000301980 \| 提交时间 2025-03-31 \| Q1

\
\
Title:

PowerEdge: SUSE15sp6 error kdump start failed.

 

Summanry: 

This article gives the solution of PowerEdge: SUSE15sp6 error kdump start failed.

 

Symptoms:

The customer environment is vsphere8 Install SUSE15 sp6 vm.

kdump.service: Failed with result \'exit-code\'.

 

Cause:

Secure boot causes kdump to fail to start.

 

Resolution:

\
Customers found that kdump service has errors in the system and cannot start normally.

The figure below:

![[记录_信息_LKB_记录_027_025-LKB-SUSE_kdump_issue_001.png]]

\
Eventually checking revealed that this had something to do with the vm having secure boot turned on.

After shutting down the vm and disabling secure boot, the kdump service can start normally.

![[记录_信息_LKB_记录_027_025-LKB-SUSE_kdump_issue_002.png]]

 

 

Keywords: 

PowerEdge,SUSE15sp6,kdump start failed, fatal error

 

已使用 OneNote 创建。
