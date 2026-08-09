SUSE-订阅

2025年3月21日

15:04

登录到网站查看订阅信息：\
\
[https://scc.suse.com](https://scc.suse.com/)  

![[__ SUSE ___002_SUSE-订阅_001.png]]

\
\
测试机器 SUSE15sp6 VM\
suse15sp6:\~ \# SUSEConnect -r DFC39785F25383FA

![[__ SUSE ___002_SUSE-订阅_002.png]]

 

 

suse15sp6:\~ \# SUSEConnect -s

\[,,,,,\]

 

\# 可以看到这个订阅是 1 年，一个普通的订阅，其中 Registered 说明已经是有订阅的模块，而 Not Registered 没订阅的模块。如果想订阅如下，有些订阅模块都是需要额外付费买的，如 SUSE订阅中的Live Patching和HA订阅。

SUSE的Live Patching服务是一种允许在运行中实时修补内核的技术，旨在提升系统安全性和稳定性，而无需重新启动操作系统。这种方法特别适用于任务关键型系统，因为它避免了因为应用更新而导致的停机时间。

 

单独添加一个订阅模块：

suse15sp6:\~ \# SUSEConnect -p sle-module-desktop-applications/15.6/x86_64

Registering system to SUSE Customer Center

 

Updating system details on <https://scc.suse.com> \...

 

Activating sle-module-desktop-applications 15.6 x86_64 \...

-\> Adding service to system \...

-\> Installing release package \...

 

Successfully registered system

 

suse15sp6:\~ \# SUSEConnect -s

\,,[,,,\]

\# 可以看到"sle-module-desktop-applications"添加后是 Registered了。

 

可以看到这个vm已经添加到订阅里：

![[__ SUSE ___002_SUSE-订阅_003.png]]

\
\
\
\
另外添加完成订阅后 zypper 库也会被自动添加原

![[__ SUSE ___002_SUSE-订阅_004.png]]

\
\
SUSE官方关于订阅的KB：

<https://scc.suse.com/docs/userguide#UG-Managing-Registered-Systems>

 

============================

取消订阅 SUSEConnect -d

![[__ SUSE ___002_SUSE-订阅_005.png]]

如果是取消订阅后源也会被清楚

![[__ SUSE ___002_SUSE-订阅_006.png]]

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

》测试升级CVE补丁，在运行中。

\# zypper patch \--cve=CVE#cve-2015-2808

![[__ SUSE ___002_SUSE-订阅_007.png]]

》如果输入的订阅与系统不匹配，会如提示。

![[__ SUSE ___002_SUSE-订阅_008.png]]

错误:注册服务器返回\'提供注册码的订阅不包括所请求的产品\'SUSE Linux企业服务器12 SP5 x86_64\' (422)

 

图形化更新与开启自动在线更新

\# yast2 online_update

![[__ SUSE ___002_SUSE-订阅_009.png]]

 

![[__ SUSE ___002_SUSE-订阅_010.png]]

\
Show Patch Category ：

Needed Patches (default view) ：应用于系统上安装的包的未安装补丁。

Unneeded Patches ：应用于未安装在系统上的包的补丁，或者满足需求的补丁(因为相关的包已经从其他来源更新了)。

All Patches ：所有补丁可用于SUSE Linux企业服务器。

点击Accept

![[__ SUSE ___002_SUSE-订阅_011.png]]

 

![[__ SUSE ___002_SUSE-订阅_012.png]]

=====

一个错误：

这个错误的出现是我使用 SUSEConnect -d 删除订阅后，快照回到订阅状态，然后再看注册状态就这样了。

这个是因为在系统订阅信息与网络上的那个已经不一样导致的。

Error: Invalid system credentials, probably because the registered system was deleted in SUSE Customer Center. Check

<https://scc.suse.com> whether your system appears there. If it does not, please call SUSEConnect \--cleanup and re-register this system.

![[__ SUSE ___002_SUSE-订阅_013.png]]

错误：无效的系统凭据，可能是因为在SUSE Customer Center中删除了已注册的系统。 检查https://scc.suse.com，您的系统是否在那里出现。 如果不是，请致电SUSEConnect \--cleanup并重新注册该系统。

 

已使用 OneNote 创建。
