Θ33-LKB-配置bond会异常重启

2025年9月16日

14:58

Title:

PowerEdge: RHEL9.4 error message \"Hardware error from APEI Generic Hardware Error Source: 4\"

 

Summanry: 

This article gives the solution of RHEL9.4 error message \"Hardware error from APEI Generic Hardware Error Source: 4\".

 

Symptoms:

The following error occurs in the TSR System Event Log:

A fatal error was detected on a component at bus 2 device 0 function 0.

 

dmesg has the following error:

\[Hardware Error\]: Hardware error from APEI Generic Hardware Error Source: 4

 

Cause:

N/A

 

Resolution:

When the server is running the Red Hat Enterprise Linux 9.3 kernel (5.14.0-362.8.1.el9_3) or later, iDRAC reports the following error:

A fatal error was detected on a component at bus 2 device 0 function 0\
......\
IPS case：PSE-37872

 

Keywords: 

PowerEdge,RHEL9.4,error message,Hardware error from APEI Generic Hardware Error Source: 4, fatal error

 

 

已使用 OneNote 创建。
