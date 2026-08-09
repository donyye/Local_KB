RHEL8\_Cockpit

2022年4月28日

14:55

Cockpit 是RHEL8的一个新功能，它可以通过Web对系统进行管理。而RHEL7.6与之后的版本也可以安装。

 

yum install cockpit cockpit-pcp cockpit-packagekit cockpit-storaged.noarch cockpit-dashboard tlog cockpit-session-recording lorax lorax-composer composer-cli cockpit-composer

 

\# systemctl restart cockpit.socket

\# systemctl enable cockpit.socket

 

 

Web 访问： [https://10.10.40.182:9090/](https://10.10.40.182:9090/)

 

Image Builder 方法：

<https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html-single/composing_a_customized_rhel_system_image/index#creating-system-images-with-composer-command-line-interface_composing-a-customized-rhel-system-image>

 

 

 

 

已使用 OneNote 创建。
