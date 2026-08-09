Haswell架构Bug（Redhat bug）

2016年1月14日

14:08

目前在网上发现Redhat一个严重的Bug，会影响基于Haswell架构的服务器。

<http://www.infoq.com/cn/news/2015/06/redhat-futex>

 

1，故障形象：

用户进程会出现死锁或被挂起（在高负债的情况容易发生），任何的 futex 调用都会被堵塞，如果运气好可以在message或dmesg里发现soft lockup信息，但现实是不一定会出现此信息。

 

2，影响的OS版本：

RHEL 5（包括CentOS 5和Scientific Linux 5）：所有版本（包括5.11版）都没有问题。

RHEL 6（包括CentOS 6和Scientific Linux 6）：从6.0～6.5版都没问题。 但6.6版存在缺陷，而6.6.z版本没有问题。

RHEL 7（包括CentOS 7和Scientific Linux 7）：7.1是有缺陷的。

 

3，Redhat KB 描述：

<https://access.redhat.com/articles/1749293>

![[Technology_ALL_Linux 问题收集_016_Haswell架构Bug（Redhat bug）_001.jpg]]

4，问题的发现和解决

一些 OS unstable 问题，CPU是V3的，OS版本是RHEL6.6、RHEL7.0、RHEL7.1的请留意此bug。Redhat有对此bug的修复kernel，但如果是非OEM OS客户，不提供kernel的下载。

[https://access.redhat.com/solutions/1386323](https://access.redhat.com/solutions/1386323)

 

Upgrading to RHEL 6.6, 7.0 or 7.1 may result in an application, using futexes, appearing to stall in futex_wait() 

Red Hat Enterprise Linux 6

This issue was originally tracked in a private bugzilla by Red Hat and has subsequently been addressed. In order to fix this issue, an update to the following kernel version (or later) within 6.6 will be required: kernel-2.6.32-504.16.2.el6, released with [RHSA-2015-0864](https://rhn.redhat.com/errata/RHSA-2015-0864.html). RHEL6.7GA and later already include the fix for this issue.

Red Hat Enterprise Linux 7

This issue has been fixed in a RHEL7.1.z errata. Update the kernel to 3.10.0-229.7.2.el7 (released with [RHSA-2015-1137](https://rhn.redhat.com/errata/RHSA-2015-1137.html)) or later.

 

已使用 OneNote 创建。
