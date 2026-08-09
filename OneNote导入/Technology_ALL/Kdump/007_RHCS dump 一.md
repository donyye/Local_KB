RHCS dump 一

2018年5月9日

9:39

How do I configure kdump for use with the RHEL 6, 7 High Availability Add-On?

Updated June 22 2017 at 5:20 PM - 

[English ](https://access.redhat.com/articles/67570)

Issue

A cluster node that encounters a kernel panic will be fenced immediately. If the cluster is configured to use power fencing, the failed node will be rebooted. This presents a problem when using the kdump crash recovery service since power fencing will reboot the failed node before kdump core collection can complete.

Environment

- Red Hat Enterprise Linux (RHEL) 6, 7 with the High Availability Add On

Resolution

There are three possible solutions:

1.  [Using post_fail_delay parameter](https://access.redhat.com/articles/67570#post_fail_delay) : Set [post_fail_delay](https://access.redhat.com/site/solutions/366343) parameter such that fencing is delayed long enough for kdump core collection to complete.
2.  [Using SAN based fencing](https://access.redhat.com/articles/67570#san_fencing) : Use an I/O based fencing mechanism that performs fencing without rebooting a failed node.
3.  [Using the fence_kdump agent (RHEL6.2 and later)](https://access.redhat.com/articles/67570#fence_kdump) : On RHEL 6.2 there is the option of using the fence agent [fence_kdump](https://access.redhat.com/site/solutions/171793) as a means to recognize when a cluster node has entered the kdump crash recovery service.
4.  [Using the fence_kdump agent (RHEL6.2 and later) on a cman cluster](https://access.redhat.com/articles/67570#cman)
5.  [Using the fence_kdump agent (RHEL6.5 and later) on a pacemaker cluster](https://access.redhat.com/articles/67570#pacemaker)

Using post_fail_delay parameter

The post_fail_delay parameter is used to delay fencing. This parameter defines the number of seconds to wait before the fence daemon (fenced) will fence a failed node. The value (in seconds) should be set to a sufficiently long period of time that kdump can complete its core collection prior to the node being fenced. This post_fail_delay parameter is set in the /etc/cluster/cluster.conf file.

[Raw](https://access.redhat.com/articles/67570#)

\<fence_daemon post_fail_delay=\"300\" post_join_delay=\"3\"/\>

In this example, the cluster will delay fencing 5 minutes. The kdump crash recovery service must be able to complete its core collection within this time to avoid being preempted by power fencing.

The time required for the kdump service to complete will vary based on a number of factors. These include the total amount of memory, where the resulting core file will be stored (local disk or remote), and various kdump options. If the core file will be copied to a remote location, the method (nfs or ssh) and network speed must also be considered.

Note that all cluster resources will remain blocked until fencing has completed. Since kdump can take a significant amount of time to complete, especially on systems that have a large amount of memory, it may not be feasible to use post_fail\_\_delay to delay fencing while kdump core collection occurs.

Using SAN based fencing

Unlike traditional power fencing, SAN based fencing agents do not reboot a failed node. Instead, SAN based fencing works by preventing the failed node from accessing shared storage. Since SAN based fencing does not reboot a failed node, the kdump crash recovery service will not be interrupted.

A complete list of supported fence agents can be found in the article [\"Fence Device and Agent Information for Red Hat Enterprise Linux\"](https://access.redhat.com/site/articles/28603). Acceptable fence agents for this workaround include agents marked as \"Fibre Channel Port\" or \"LUN access\" under the \"Fence Type\" heading.

Using the fence_kdump agent (RHEL6.2 and later)

In Red Hat Enterprise Linux 6.2 and later, the fence_kdump agent can be used to detect that a failed cluster node has entered the kdump crash recovery service and mark the node as fenced.

Using the fence_kdump agent will result is significantly shorter recovery time as compared to using post_fail_delay. When using post_fail_delay, recovery will not complete until kdump core collection has completed. The fence_kdump agent reduces recovery time since fencing will complete as soon as the cluster is notified that a failed node has entered the kdump crash recovery service, allowing the cluster to recovery prior to the completion of kdump core collection.

The fence_kdump agent must be used in conjunction with another fence agent. It must not be used by itself. The fence_kdump agent is only capable of detecting that a cluster node has entered the kdump kernel. Other events that required fencing (eg. network outage) must be handled by other fencing methods.

The fence_kdump fence agent has two components:

- fence_kdump: The fencing agent. When fencing occurs, this agent will listen for a message from the node that is being fenced. If the agent does not receive a message from the failed node within a certain amount of time, the agent returns failure and other fencing methods should be attempted. If the agent does receive a message from the failed node, the agent returns success and the node is considered to be fenced.
- fence_kdump_send: The utility that sends the message. This is normally run from within the kdump kernel while the kdump crash recovery service is performing core collection. Messages will be sent continuously at a regular interval to all nodes in the cluster.

Using the fence_kdump agent (RHEL6.2 and later) on a cman cluster

Below is procedure on how to add fence_kdump into existing configuration of cluster. Example below expects that cluster already contains functional fencing, in examples here it is represented by fence_apc (use the fencing device specific for your environment, not necesarrily fence_apc).

1.  Add fence_kdump into /etc/cluster/cluster.conf configuration file as the first fencing device for each node. In example below the fence_kdump agent will listen for a message from the failed node for 120 seconds. If no message is received from the failed node in that time frame, fence_kdump will return failure and the next fencing method (fence_apc in example below) will be attempted.\
    Cluster configuration before adding fence_kdump:\
    [Raw](https://access.redhat.com/articles/67570#)\
    \...\
    \<clusternode name=\"node-01\" votes=\"1\" nodeid=\"1\"\>\
    [  \<fence\>\
        \<method name=\"apc\"\>\
          \<device name=\"apc\"/\>\
        \</method\>\
      \</fence\>\
    \</clusternode\>\
    \...\
    \<fencedevices\>\
      \<fencedevice name=\"apc\" agent=\"fence_apc\"/\>\
    \</fencedevices\>\
    \...\
    \
    ]Cluster configuration after adding fence_kdump:\
    [Raw](https://access.redhat.com/articles/67570#)\
    \...\
    \<clusternode name=\"node-01\" votes=\"1\" nodeid=\"1\"\>\
    \<fence\>\
    [  \<method name=\"1\"\>\
        \<device name=\"kdump\"/\>\
      \</method\>\
      \<method name=\"2\"\>\
        \<device name=\"apc\"/\>\
      \</method\>\
    \</fence\>\
    \</clusternode\>\
    \...\
    \<fencedevices\>\
      \<fencedevice name=\"kdump\" agent=\"fence_kdump\" timeout=\"120\"/\>\
      \<fencedevice name=\"apc\" agent=\"fence_apc\"/\>\
    \</fencedevices\>\
    \...\
    \
    ]It is important to note that if a node fails for any reason other than a kernel panic, the total recovery time will be delayed by the time that fence_kdump waits for a message. In the example above, if \"node-01\" fails for any reason other than a kernel panic, the next fencing agent (fence_apc in above example) will not attempt to fence the node until fence_kdump has returned failure after 120 seconds.
2.  After making changes to /etc/cluster/cluster.conf [propagate the changes to all cluster nodes](https://access.redhat.com/solutions/5949#rhel6).
3.  Once the /etc/cluster/cluster.conf file has been modified to use fence_kdump, restart the kdump service. This step is required so that the kdump service detects that it should send messages to the cluster nodes. The kdump service will detect that the /etc/cluster/cluster.conffile has changed and rebuild the kexec initrd image. When this occurs, kdump will extract a list of cluster nodes that should received notification messages when the node enters the kdump crash recovery service. For the reason, the kdump service should be restarted whenever the /etc/cluster/cluster.conf is modified.\
    [Raw](https://access.redhat.com/articles/67570#)\
    \# service kdump restart
4.  Test out that fence_kdump works properly by forcing a node to panic. For this example, assume that the node being forced to panic is \"node-01\" with an IP address of 192.168.1.4.\
    [Raw](https://access.redhat.com/articles/67570#)\
    \# echo c \> /proc/sysrq-trigger\
    \
    Once the node has entered the kdump kernel, fence_kdump_send will begin sending messages to all cluster nodes. Of the remaining cluster nodes, the node with the lowest node ID will be responsible for fencing the failed node \"node-01\". Inspecting /var/log/messages on the node performing the fence operation should show the following:\`\
    [Raw](https://access.redhat.com/articles/67570#)\
    [fenced\[4789\]: fencing node node-01\
    fence_kdump\[6093\]: waiting for message from \'192.168.1.4\"\
    \
    ]When fence_kdump has received a valid message from \"node-01\", the following messages should be logged. If the received valid message is not logged then that would indicate that for some reason the kdump kernel never sent the message or that kdump kernel was not running:\
    [Raw](https://access.redhat.com/articles/67570#)\
    [fence_kdump\[6093\]: received valid message from \'192.168.1.4\'\
    fenced\[4789\]: fence node-01 success\
    \
    ]At this point \"node-01\" is considered to be fenced. It can continue with core collection without interruption. Once core collection has completed, the node will reboot.

 

By default, the fence_kdump agent will listen for UDP messages on port 7410. The default timeout is 60 seconds. These values can be modified by setting parameters for the fence_kdump fence device:

[Raw](https://access.redhat.com/articles/67570#)

\<fencedevice name=\"kdump\" agent=\"fence_kdump\" timeout=\"180\" ipport=\"8452\"/\>

In this example, the fence_kdump agent will listen for UDP messages on port 8452 and will timeout if no message is received after 180 seconds.

If the fence_kdump agent is configured to listen on a port other than the default, the fence_kdump_send utility must be configured to send to that same port number. The behavior of fence_kdump_send can be modified by setting options in the /etc/sysconfig/fence_kdump file:

[Raw](https://access.redhat.com/articles/67570#)

FENCE_KDUMP_OPTS=\"-i 5 -p 8452\"

In this example, fence_kdump_send would send a message to port 8452 every 5 seconds. For a complete list of fence_kdump_send options, please refer to the fence_kdump_send man page. Important: Any changes to /etc/sysconfig/fence_kdump requires that the kdump service be restarted (step 3).

Using the fence_kdump agent (RHEL6.5 and later) on a pacemaker cluster

Check the solution [How to configure fence_kdump in RHEL 6 or 7 pacemaker cluster](https://access.redhat.com/solutions/2876971).

 

来自 \<[https://access.redhat.com/articles/67570](https://access.redhat.com/articles/67570)\> 

 

 

已使用 OneNote 创建。
