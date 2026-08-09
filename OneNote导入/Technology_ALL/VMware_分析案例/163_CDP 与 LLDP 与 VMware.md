CDP 与 LLDP 与 VMware

2024年1月9日

16:58

Vswitch 有个 CDP 的东西，

![[Technology_ALL_VMware_分析案例_163_CDP 与 LLDP 与 VMware_001.png]]

 

他是 cisco 的发现协议

还有一个 LLDP[  ]的东西，它是开源的，和CDP是一样的功能

 

标准交换机是没有 LLDP，而分布式交换机才有。

![[Technology_ALL_VMware_分析案例_163_CDP 与 LLDP 与 VMware_002.png]]

 

 

 

LLDP，即链路层发现协议（Link Layer Discovery Protocol），是一种用于在网络中发现和交换设备信息的网络协议。LLDP 操作在 OSI 模型的链路层（第二层）。

LLDP 允许连接到网络的设备（如交换机、路由器、服务器等）发现彼此的存在，并交换有关自己的信息。这包括设备的标识、设备类型、设备的能力信息、连接的端口信息等。

 

ethtool \--set-priv-flags enoxxx disable-fw-lldp \<off/on\>

![[Technology_ALL_VMware_分析案例_163_CDP 与 LLDP 与 VMware_003.png]]

 

来自GPT：

 

LLDP（Link Layer Discovery Protocol）和 CDP（Cisco Discovery Protocol）是两种用于网络设备发现和信息交换的协议。它们在一些方面相似，但也有一些显著的区别。

 

相同点：

发现协议： LLDP 和 CDP 都是用于在网络中发现设备的协议。它们允许网络设备交换彼此的信息，包括设备的标识、能力、连接的端口等。

 

自动化： 这两个协议都支持自动化设备发现，允许设备在连接到网络时自动发现邻近的设备。

 

跨厂商兼容性： 尽管 CDP 是 Cisco 的专有协议，但 LLDP 是一个开放标准，被多个厂商支持。这使得 LLDP 在跨不同厂商的设备上更具通用性。

 

不同点：

厂商支持：

 

LLDP： 是一个开放标准，由 IEEE 制定。因此，它在多个厂商的设备上都有支持。

CDP： 是 Cisco 的专有协议，主要在 Cisco 设备上使用。

标准化程度：

 

LLDP： 是 IEEE 802.1AB 标准的一部分，是一个开放的标准协议。

CDP： 是 Cisco 自家的协议，不是一个开放的标准。

信息元素：

 

LLDP： 提供一种灵活的信息元素格式，可以携带多种类型的信息。

CDP： 需要按照其特定的格式传递信息。

协议开销：

 

LLDP： 通常具有较小的协议开销，并且在网络中的广播量相对较少。

CDP： 由于是 Cisco 的专有协议，可能在网络中产生更多的广播。

兼容性：

 

LLDP： 在支持 IEEE 802.1AB 的多个厂商的设备上工作，提供更好的跨厂商兼容性。

CDP： 主要在 Cisco 设备上使用，可能不够灵活以适应混合设备厂商的环境。

 

综合而言，如果网络环境中主要使用 Cisco 设备，那么 CDP 可能更为方便。如果存在多个厂商的设备，LLDP 可能更具有通用性。在跨厂商和混合设备环境中，LLDP 的开放标准性质可能使其更受欢迎。

 

 

已使用 OneNote 创建。
