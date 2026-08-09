【Case Share】X520网卡PXE boot issue,PXE-E51:No DHCP or proxyDHCP offers were received

2020年1月19日

14:46

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   【Case Share】X520网卡PXE boot issue,PXE-E51:No DHCP or proxyDHCP offers were received
  From      Luo, Jeason
  To        CN XMN TS ENT L2 SME; CN XMN TS ENT L2 Coach; GC ENT RM
  Cc        Wang, Xing Fang; Lin, Yongliang
  Sent      2020年1月19日 14:35
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

问题：X520网卡，PXE引导安装系统失败，能获取到MAC地址，但无法获取到DHCP分配的IP地址，如下报错

PXE-E51:No DHCP or proxyDHCP offers were received.

PXE-M0F:Exiting Intel Boot Agent.

![[Technology_ALL_Linux 问题收集_059_【Case Share】X520网卡PXE boot issue,PXE-E51_001.jpg]]

 

处理过程:

1.使用X520网卡port1和port2网口从PXE引导，情况一样

2.换了其它能正常PXE引导的线过来测试，还是无法获取到DHCP分配的IP地址

3.从TSR log查看X520网卡port1和port2网口的MAC地址已有正获取到，网卡没有问题，再往后要获取DHCP分配IP时超时，客户PXE的DHCP服务器正常

4.安排了MB+NDC上门测试，情况一样

5.降级NIC FW 19.0.12到18.8.9，PXE-E51:No DHCP or proxyDHCP offers were received.这条信息消失，但依旧在获取DHCP分配IP时超时

6.客户反馈PXE环境中有对MAC做绑定，重新对新换网卡MAC绑定后，问题依旧

7.和其它正常机器的网卡配置做对比，发现网卡VLAN配置为：0，当前无法获取DHCP分配IP机器的网卡VLAN配置为：1，重启按F2\-\--Device settings将对应网口的VLAN设置为：0后，PXE引导可以正常通过并安装系统

![[Technology_ALL_Linux 问题收集_059_【Case Share】X520网卡PXE boot issue,PXE-E51_002.jpg]]

 

![[Technology_ALL_Linux 问题收集_059_【Case Share】X520网卡PXE boot issue,PXE-E51_003.jpg]]

 

解决方案：

将网卡VLAN ID改成和客户网络环境相匹配的VLAN ID后解决

 

总结：

PXE引导失败的原因有很多，需要根据反馈的情况灵活处理，以下几点可供后续检查做参考

1.  检查服务器网口的PXE启动设置是否正确
2.  检查网线、SFP模块是否兼容，确保物理连接正常
3.  检查PXE失败的具体报错，根据报错定位排查方向
4.  装正常和非正常的机的机器做对比，查看是否有设置不同的地方
5.  客户PXE环境也很重要，需要客户尽可能提供一些准确的信息并让其先排除掉一些基特殊的网络配置问题，如VLAN、MAC绑定等

 

 

Jeason Luo

Senior Engineer, Server Support

Dell EMC \| Technical Support

Dell line: 8880771

Phone: +86 592 8180771

[Jeason_luo@dell.com](mailto:Jeason_luo@dell.com)

 

 

已使用 OneNote 创建。
