RHEL8 VNC 配置

2021年12月29日

12:22

参考一些网络上的文档，测试了一下，供你参考。您的那个问题是其它用户没有权限去运行，其实只要root 用户能运行，开启端口，其它用户的可以登录。

 

1. 系统环境 RHEL8.0

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_001.jpg]]

 

2. 取消注释 waylandEnable=false 

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_002.jpg]]

 

3. yum安装 VNC Server

yum install -y tigervnc-server xorg-x11-fonts-Type1

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_003.jpg]]

 

4. 配置VNC Server，在 /etc/systemd/system/ 中创建配置文件，以便将VNC服务作为系统服务运行。

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_004.jpg]]

 

5. 设置用户的登录密码

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_005.jpg]]

 

6. 启动服务

systemctl daemon-reload

systemctl start <vncserver@:2.service>

systemctl enable [vncserver@\\:2.service](mailto:vncserver@\:2.service)

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_006.jpg]]

 

7. 检查端口与服务是否已经开启

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_007.jpg]]

 

8. 使用VNC软件进行登录测试

![[Technology_ALL_Linux 问题收集_075_RHEL8 VNC 配置_008.gif]]

 

 

 

已使用 OneNote 创建。
