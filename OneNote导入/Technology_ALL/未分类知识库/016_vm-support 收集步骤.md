vm-support 收集步骤

Wednesday, September 24, 2014

4:54 PM

具体步骤，开启SSH使用命令方式收集log：

1.  进入管理界面，按F2进行系统设置，此时需要输入root帐号的密码
2.  选择Troubleshooting Options
3.  选择开启或者关闭ssh服务
4.  开启SSH服务后，通过客户端登录ESXI
5.  在终端敲入以下命令

\[user@esx2host\]\$ cd /tmp

\[user@esx2host\]\$ /usr/bin/vm-support

     6,        脚本产生的输出文件会存放在当前的目录中，被命名为esx-xxxx.tgz

 

已使用 OneNote 创建。
