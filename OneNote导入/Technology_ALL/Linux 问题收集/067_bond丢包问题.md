bond丢包问题

2020年9月2日

10:32

- 问题: 四台R740XD机器配置BCM57416网卡会存下丢包情况，CentOS 7.6，网口有做绑定，有更换过交换机，网线问题依旧

   

  处理过程：

  1.  检查网卡硬件状态正常，速率和链路也正常

  ![[Technology_ALL_Linux 问题收集_067_bond丢包问题_001.jpg]]

  1.  升级NIC固件到最新，10G网口做绑定后会仍然有丢包情况
  2.  解除网口绑定，直连测试，没有丢包情况
  3.  客户在交换机端关闭开启LLDP控制报文学习MAC的机制，测试10G网口做绑定，还是有丢包情况
  4.  尝试装2个千兆的网口做BOND后测试，没有丢包的情况

   

  解决方案：

  F2\-\-\--Device setting\-\-\--选择对应的网口，禁用LLDP nearest bridge和LLDP nearest non-TPMR bridge后，问题解决。

  以下是网卡设置的参考截图。

  ![[Technology_ALL_Linux 问题收集_067_bond丢包问题_002.jpg]]

 

已使用 OneNote 创建。
