Linux 下GPU温度获取

2018年12月12日

9:38

- 1.  GPU卡温度抓取
      - 在待排查问题主机上运行 "nvidia-smi daemon --s pucv --p /home --j monitor"。如下图，在/home目录下生成一个名字包含"monitor"的记录文件

  ![[Technology_ALL_Linux 问题收集_044_Linux 下GPU温度获取_001.jpg]]

  - 由用户加载GPU应用负载
  - 持续一段时间后，运行"nvidia-smi daemon --t"结束温度监控。
  - 请用户提供/home目录下的记录文件

  1.  系统风扇转速

  在运行用户GPU应用负载时，查看 iDRAC-\>硬件-\>风扇 页面的风扇转速，尤其是风扇3和风扇6的转速。

  1.  正常散热情况

  运行"nvidia-smi replay --f \<GPU温度记录文件\>"，如下图所示，正常情况下，GPU最高温度应不超过85摄氏度，同时系统风扇3和6的转速应该在7000转左右。

  ![[Technology_ALL_Linux 问题收集_044_Linux 下GPU温度获取_002.jpg]]

   

  ![[Technology_ALL_Linux 问题收集_044_Linux 下GPU温度获取_003.jpg]]

 

已使用 OneNote 创建。
