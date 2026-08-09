RHCS dump 二

2018年5月9日

9:40

How to configure fence_kdump in RHEL 6 or 7 pacemaker cluster?

 SOLUTION VERIFIED - Updated October 16 2017 at 10:35 AM - 

[English ](https://access.redhat.com/solutions/2876971)

Environment

- Red Hat Enterprise Linux 6 and 7 with High Availability or Resilient Storage Add-on using pacemaker

Issue

How to configure fence_kdump in RHEL 6 or 7 pacemaker cluster?

Resolution

1.  Ensure that kdump service is properly configured on all nodes and is started. You can use [Kdump Helper](https://access.redhat.com/labs/kdumphelper/) application to assist you in configuring kdump.\
    RHEL6\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# service kdump status\
    Kdump is operational\
    \
    RHEL7\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# systemctl is-active kdump\
    active
2.  Create fence_kdump STONITH device in cluster.\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# pcs stonith create kdump fence_kdump pcmk_reboot_action=\"off\" pcmk_host_list=\"node-1 node-2\"\
    \
    Note: In some older versions the additional parameters may be needed if the STONITH device fails to start. For more information check solution [A fence_kdump STONITH device fails to start and fencing fails in RHEL 6 or 7 pacemaker clusters](https://access.redhat.com/solutions/875883).
3.  Configure STONITH levels in a way that fence_kdump is the primary fencing device, while your previous fencing device becomes secondary.\
    More information on STONITH levels can be found in solution [How to configure/manage STONITH \'levels\' in RHEL cluster with pacemaker ?](https://access.redhat.com/solutions/891323).\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# pcs stonith level add 1 node-1 kdump\
    \# pcs stonith level add 1 node-2 kdump\
    \# pcs stonith level add 2 node-1 fence-node-1\
    \# pcs stonith level add 2 node-2 fence-node-2\
    \
    An example on how the configuration will look like is below. Note that fence_xvm is used just as example.\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# pcs config\
    \...\
    Stonith Devices:\
    [ Resource: fence-node-1 (class=stonith type=fence_xvm)\
      Operations: monitor interval=30s (fence-node-1-monitor-interval-30s)\
     Resource: fence-node-2 (class=stonith type=fence_xvm)\
      Attributes: delay=10\
      Operations: monitor interval=30s (fence-node-2-monitor-interval-30s)\
     Resource: kdump (class=stonith type=fence_kdump)\
      Attributes: pcmk_reboot_action=off pcmk_host_list=\"node-1 node-2\"\
      Operations: monitor interval=60s (kdump-monitor-interval-60s)\
    Fencing Levels:\
     Node: node-1\
      Level 1 - kdump\
      Level 2 - fence-node-1\
     Node: node-2\
      Level 1 - kdump\
      Level 2 - fence-node-2\
    \...]
4.  On all nodes allow the port 7410/udp on the firewall to allow incoming notification from kdump when the vmcore is being collected.\
    RHEL6\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# iptables -I INPUT -p udp \--dport 7410 -j ACCEPT\
    \# service iptables save ; service iptables restart\
    \
    RHEL7\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# firewall-cmd \--add-port=7410/udp\
    \# firewall-cmd \--add-port=7410/udp \--permanent
5.  Test out configuration by crashing one of the nodes. You can crash a node using commands below.\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    \# echo c \> /proc/sysrq-trigger\
    \
    On the crashed node you may see on graphical or serial console how the kdump progresses and captures the vmcore. On the cluster node where the fencing was triggered you should see in the logs that fence kdump will wait for a message from that node.\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    [fencing node-1\
    fence_kdump\[XXXX\]: waiting for message from \'1.1.1.1\'\
    \
    ]Once the kdump on the crashed node is started, it will send messages every 10 seconds and the fencing node should then report that such message is received and confirm the fencing as successful.\
    [Raw](https://access.redhat.com/solutions/2876971#)\
    [fence_kdump\[XXXX\]: received valid message from \'1.1.1.1\'\
    fence node-1 success]

If you experience timeout to receive message in RHEL 7 and you use package kexec-tools prior to version 2.0.14-17.el7 then check the article [fence_kdump fails with \"timeout after X seconds\" in a RHEL 6 or 7 High Availability cluster ](https://access.redhat.com/solutions/2388711)for further information.

 

来自 \<[https://access.redhat.com/solutions/2876971](https://access.redhat.com/solutions/2876971)\> 

 

 

已使用 OneNote 创建。
