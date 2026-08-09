Tcpdump 抓包

2019年5月29日

17:00

- 1.  收集日志

  Show clock

  Show tech

  Show trace

  Show trace hardware

  Show inventory media

   

  1.  准备工作
      1.  时间同步：因为后续需要要结合抓的数据包里的时间戳和对端设备的事件的时间戳匹配起来看，所以在操作前请先分别确认所有相关设备包括：交换机、抓包服务器、问题服务器，logging服务器都需要确保时间相同，建议使用相同的NTP时间源；若没有NTP服务器，请务必调整时间一致。

  备注：通过交换机日志已看到配置有NTP连接10.0.30.202、10.0.30.204

  1.  交换机log和debug时间戳设置

  Dell(conf)#service timestamps log datetime localtime show-timezone

  Dell(conf)#service timestamps debug datetime localtime show-timezone

  1.  准备两台抓包服务器(建议万兆服务器，并带有网卡)，或者一台服务器两个网卡，其装有wireshark/tcpdump等抓包工具。
  2.  准备一台logging 服务器，装有有logging 软件，并可以ping通两台交换机。建议此台服务器和前面抓包服务器一台，这样时间可以同步。

  备注：通过交换机日志已看到配置有logging server(10.0.21.221)

  1.  针对Port-channel 16抓取有用包 
      1.  在Z9100找到空闲端口，建议使用最后的SFP+万兆端口，否则根据需要调整
      2.  在两台Z9100上配置抓包命令，注意：相关命令可能按照实际情况调整，//为相关解释

  //检查session，看是否已经有重复的session

  Show monitor session

  //x/y是抓包服务器的连接端口

  Monitor session 1

    Source interface po/13 destination x/y direction both

  Show monitor session

  1.  服务器抓包，注意，需要选择相应的物理端口，以及开启抓包过滤模式。
      1.  Windows+ Wireshark：

  Capture filter为ether proto 0x8809，请注意，是抓包模式过滤。

  ![[Technology_ALL_未分类知识库_065_Tcpdump 抓包_001.jpg]]

  1.  Linux+tcpdump

  \$tcpdump -i p1p1 ether proto 0x8809 -C 50M -w LACP_p1p1.pcap

  \$tcpdump -i p1p2 ether proto 0x8809 -C 50M -w LACP_p1p2.pcap

   

  注意：以上指令是仅抓取LACP协议包，每50M会生成一个.pcap包，请确保有足够硬盘空间。另外，抓包可能会持续超过24小时，需要确保运行tcpdump的终端窗口不会被意外终止（如关闭ssh窗口）。如果有那个可能的话，那就最好用screen或者类似的工具。

  1.  反复调整，直至可以看到正确的LACP被捕获。

  <!-- -->

  1.  交换机debug

  备注：交换机的debug会导致CPU利用率上升，请观察前后CPU的1分钟或5分钟利用率(show process cpu) 目前CPU的利用率都在个位数，比较低。如果有异常(CPU的1分钟或者5分钟利用率上升超过50%等)，直接关闭debug。

  1.  SNMP性能监控设置：从配置来看，交换机已经配置有SNMP服务器 (community 属性为ready Only), SNMP server 是10.0.21.234，请确定是否可以收到正确的trap。如果需要针对LACP发出SNMP告警，配置命令: snmp-server enable traps lacp。DELL已经提供MIB文件。建议在开启Debug前跟踪性能趋势，并配置好相应的警告阈值。
  2.  如果是telnet或者SSH到交换机，需要在开启debug之前做好logging session后，开启terminal monitor
  3.  两台交换机开启相关logging server配置

  Logging \<server IP address\>

  Logging monitor debugging

  Logging trap debugging

  Logging on

   

  1.  在交换机上开debug

  Debug lacp events

  Debug lacp pdu interface hu100

   

  1.  关闭debug

  No debug lacp events

  No debug lacp pdu interface hu100

 

已使用 OneNote 创建。
