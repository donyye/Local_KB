Redhat-SCAP

2026年2月5日

16:13

关于redhat系统漏洞扫描SCAP

 

oscap oval eval 的结果 =「基于 Red Hat 官方规则，对你当前系统"理论上是否受某些 RHSA / CVE 影响"的判断清单」

 

\# yum instal openscap-scanner scap-security-guide scap-workbench openscap-engine-sce

 

- openscap-scanner \--\> 主体安装程序
- scap-workbench \--\> 提供图形工具
- scap-security-guide \--\> 系统提供安全策略的集合
- openscap-engine-sce \--\> 脚本检查引擎(SCE), 扩展在此软件包中提供

 

CVE \--\> 漏洞编号

CVSS \--\> 漏洞评分

 

执行漏洞扫描（OVAL）

\# oscap oval eval   \--report /root/oval-report.html   /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml

Definition oval:ssg-zipl_vsyscall_argument:def:1: false

Definition oval:ssg-zipl_systemd_debug-shell_argument_absent:def:1: true

Definition oval:ssg-zipl_slub_debug_argument:def:1: false

Definition oval:ssg-zipl_page_poison_argument:def:1: false

Definition oval:ssg-zipl_page_alloc_shuffle_argument:def:1: false

Definition oval:ssg-zipl_init_on_alloc_argument:def:1: false

Definition oval:ssg-zipl_bootmap_is_up_to_date:def:1: false

Definition oval:ssg-zipl_bls_entries_only:def:1: true

\.....

Definition oval:ssg-account_password_selinux_faillock_dir:def:1: true

Definition oval:ssg-account_password_pam_faillock_system_auth:def:1: false

Definition oval:ssg-account_password_pam_faillock_password_auth:def:1: false

Definition oval:ssg-account_disable_post_pw_expiration:def:1: false

Evaluation done.

 

执行完你才会看到：

CVE

RHSA

true \--\> 当前系统受影响

false \--\>        不受影响        

unknown \--\>        无法判断        

 

它的作用是告诉你哪些安全公告"真的命中"了你这台 RHEL系统。

 

 

列出可修复的安全补丁

\# dnf updateinfo list security

Updating Subscription Management repositories.

Last metadata expiration check: 0:41:02 ago on Thu 05 Feb 2026 09:07:54 AM CST.

RHSA-2024:9317  Low/Sec.       NetworkManager-1:1.48.10-2.el9_5.x86_64

RHSA-2025:0377  Moderate/Sec.  NetworkManager-1:1.48.10-5.el9_5.x86_64

RHSA-2024:9317  Low/Sec.       NetworkManager-adsl-1:1.48.10-2.el9_5.x86_64

RHSA-2025:0377  Moderate/Sec.  NetworkManager-adsl-1:1.48.10-5.el9_5.x86_64

RHSA-2024:9317  Low/Sec.       NetworkManager-bluetooth-1:1.48.10-2.el9_5.x86_64

RHSA-2025:0377  Moderate/Sec.  NetworkManager-bluetooth-1:1.48.10-5.el9_5.x86_64

RHSA-2024:9317  Low/Sec.       NetworkManager-config-server-1:1.48.10-2.el9_5.noarch

RHSA-2025:0377  Moderate/Sec.  NetworkManager-config-server-1:1.48.10-5.el9_5.noarch

RHSA-2024:9317  Low/Sec.       NetworkManager-libnm-1:1.48.10-2.el9_5.x86_64

RHSA-2025:0377  Moderate/Sec.  NetworkManager-libnm-1:1.48.10-5.el9_5.x86_64

RHSA-2024:9555  Important/Sec. NetworkManager-libreswan-1.2.22-4.el9_5.x86_64

RHSA-2024:9317  Low/Sec.       NetworkManager-team-1:1.48.10-2.el9_5.x86_64

\...\...

 

 

比如看到有关于 wget 命令的安全漏洞

1）先看看机器是否有安装此包

\[root@RH94-T1 \~\]# systemctl is-active wget

inactive[   #\--\> ]没安装或当前未运行，如果有运行显示是\"active\"。

 

 

2）查看具体 RHSA描述

\# dnf updateinfo info RHSA-2024:6192

Updating Subscription Management repositories.

Last metadata expiration check: 0:13:53 ago on Thu 05 Feb 2026 10:14:18 AM CST.

===============================================================================

  Moderate: wget security update

===============================================================================

  Update ID: RHSA-2024:6192

       Type: security

    Updated: 2024-09-04 01:44:34

       Bugs: 2292836 - CVE-2024-38428 wget: Misinterpretation of input may lead to improper behavior

       CVEs: CVE-2024-38428

Description: The wget packages provide the GNU Wget file retrieval utility for HTTP, HTTPS, and FTP protocols.

           :

           : Security Fix(es):

           :

           : \* wget: Misinterpretation of input may lead to improper behavior (CVE-2024-38428)

           :

           : For more details about the security issue(s), including the impact, a CVSS score, acknowledgments, and other related information, refer to the CVE page(s) listed in the References section.

[   Severity: Moderate  #\--\> ]中等风险

 

3）修复方法

1: 精准修复: 如只修复关于 openssl 

dnf update \--security openssl

2: 全升级: 系统会更新到目前最新

dnf update

3: 如果指定只是升级 security，或只升级 Critical的，那么有可能会有包的冲突等问题出现

dnf update \--security \--sec-severity=Critical

 

已使用 OneNote 创建。
