ethtool看到网卡驱动是

2015年12月3日

8:33

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Why ethtool output showing irregular firmware version for ixgbe drivers in RHEL 6.5?
  发件人     Mai, Shuang
  收件人     Ye, Dony
  发送时间   2015年12月2日 18:13
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

Environment

- Red Hat Enterprise Linux (RHEL) 6.3, 6.4, 6.5
- Driver: ixgbe

Issue

- ethtool -i showing inappropriate firmware version.

[Raw](https://access.redhat.com/solutions/742343#)

\# ethtool -i eth0\
driver: ixgbe\
version: 3.15.1-k-Custom\
firmware-version: 0x8000057b          \<\<========\
bus-info: 0000:05:00.0

Resolution

- Behaviour is expected by design.
- Source code is changed to display 32 bit value starting at offset 0x2d for ixgbe drivers.

Root Cause

- Use 32bit value starting at offset 0x2d for displaying the firmware version in ethtool. This should work for all current ixgbe Hardware.
- Related Source code :

 

来自 \<[https://access.redhat.com/solutions/742343](https://access.redhat.com/solutions/742343)\> 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Hello Dony

 

帮我贴下这个文档信息

[https://access.redhat.com/solutions/742343](https://access.redhat.com/solutions/742343)

Why ethtool output showing irregular firmware version for ixgbe drivers in RHEL 6.5?

 

用户发现输出不一样

 

![[Technology_ALL_Linux 问题收集_011_ethtool看到网卡驱动是_001.png]]

 

firmware-version: 0x80000596, 16.5.20

 

0x8000596和   16.5.20分别代表什么意思

 

![[Technology_ALL_Linux 问题收集_011_ethtool看到网卡驱动是_002.jpg]]

 

 

 

Mai Shuang 麦爽

Technical Account Manager

Dell \| Global Support Services 

office + 86 10 58261783, mobile +86 13701323458, fax + 86 10 5826 1000

How am I doing? Email my manager at <Xin_Zheng@dell.com>

 

已使用 OneNote 创建。
