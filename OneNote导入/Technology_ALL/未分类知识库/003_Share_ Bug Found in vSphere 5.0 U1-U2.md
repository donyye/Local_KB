Share: Bug Found in vSphere 5.0 U1-U2

Thursday, November 21, 2013

11:21 AM

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Share: Bug Found in vSphere 5.0 U1-U2
  发件人     Yin, guoxun
  收件人     CCC XMN Enterprise ProSupport 1 ; CCC XMN Enterprise ProSupport 2; CCC XMN Enterprise ProSupport 3; CCC XMN Enterprise ProSupport Storage; CN XMN TS Server TW;    CN XMN EEC HK; CN XMN TS Server Coach; CN XMN TS Networking; CN XMN GSD TS Enterprise
  发送时间   Thursday, November 21, 2013 11:02 AM
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi All

目前在vSphere 5.0 U1/U2上遇到个问题，已经确认是vSphere的bug, 详细情况如下，遇到请注意按照以下情况分析确认，不要盲目更换硬件。

 

Hardware：PE M910

OS：vSphere 5.0 U1/U2

故障现象：vCenter里面会反复报告ESXi主机网络冗余链路丢失，间隔几分钟至几十分钟就会出现一次，而且与正常时间不匹配，会与当前时间延迟半个小时左右。报错信息中有时会指定相应的物理网卡，有时候不会指定相应的物理网卡。

分析：目前发现两例类似情况，检查vmkernel log和vobd.log中的日志条目发现跟vCenter报告的频率和条目不匹配，通过以下命令监视网络通断情况1个小时，未发现任何网络中断产生，期间网络中断的消息一直在vCenter中报出。

VMware已经确认是vCenter的bug，目前没有FIX。 

临时解决方案：将受影响的ESXi主机上的VM迁走，将主机置于维护模式后从vCenter中移除，过几分钟后再重新加入至vCenter，此问题会暂时消失，重现时间不确认。

 

 

 

1：监视ESXi主机端所有网卡的状态

Ssh登陆到esxi主机，执行以下命令，该命令每隔3秒自动刷新一次，显示网卡的up/down状态，观察一小时，按q键可退出监视画面

watch -n 3 "esxcfg-nics -l" 

 

 

 

2：通过logging debug功能观察交换机

通过ssh登陆到cmc,然后输入connect switch-\#（\#为要登陆的switch编号），然后输入enable和密码，执行以下命令，打开logging debug功能，如果当前交换机有端口状态发生变化，

会在console上产生相应消息，用户可截取图片发送给我们检查。

config

logging console debug

line console

exec-timeout 0

end

每一行为一条命令，观察屏幕一小时，看是否会跳出信息，如果跳出异常信息请及时截图发送给我们。

观察完毕后输入no logging console关闭debug功能。

 

 

Best Regards

 

Yin Guo Xun

Dell \| Enterprise Support Services

Mail Address:[guoxun_yin@dell.com](http://guoxun_yin@dell.com)

Certifications: VCP3/4/5 ,VCI, CCA, HPUX_CSA

 

How am I doing? Email my manager ([Wang, XingFang](mailto:Xing_Fang_Wang@Dell.com)) with any feedback.

 

 

已使用 OneNote 创建。
