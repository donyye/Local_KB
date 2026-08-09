Mellanox 网卡分析（infiniband）

Monday, November 10, 2014

3:18 PM

1. 使用命令

 

  ./ibdiagnet_skip_hca_cable_info --pc --P all=1 --pm_pause_time 1200

生成文件：/var/tmp/ibdiagnet2下面的所有文件

 

执行后log里的结果：

网络里面有一个链路是2.5G

-E- Link: S0002c90300688830/N0002c90300688830/P20\<\--\>S0002c90300165e63/N0002c90300165e60/P1 - Unexpected actual link speed 2.5 (enable_speed1=\"2.5 or 5 or 10\", enable_speed2=\"2.5 or 5 or 10\" therefore final speed should be 10)

 

可以看出来8830的交换机的一个Port20端口，连接到C0104[  P1]卡的端口（c0104），现在传输速度只有2.5G，正常速度是10G的，明显有问题，从经验来说有可能是物理线路的问题。

 

后续可以使用此命令继续检查：

smpquery nd  -D 0,1,4

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

0002c90300165e60 在ibdiagnet2.lst log里可以找到对应的主机名字。

列:

{ CA Ports:02 SystemGUID:0002c90300165e63 NodeGUID:0002c90300165e60 PortGUID:0002c90300165e61 VenID:000002c9 DevID:1003 Rev:00000000  LID:0005 PN:01 } { SW Ports:24 SystemGUID:0002c90300688830 NodeGUID:0002c90300688830 PortGUID:0002c90300688830 VenID:000002c9 DevID:c738 Rev:000000a1  LID:0002 PN:14 } PHY=4x LOG=ACT SPD=2.5

 

已使用 OneNote 创建。
