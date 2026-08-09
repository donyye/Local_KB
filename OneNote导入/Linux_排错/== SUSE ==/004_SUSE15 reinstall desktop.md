SUSE15 reinstall desktop

2024年5月24日

9:43

SUSE15 reinstall desktop

 

SUSE15 reinstall desktop

\# zypper pattern-info gnome-basic x11 \|grep package \|grep -v Reading \|awk \'\' \|xargs zypper remove

![[__ SUSE ___004_SUSE15 reinstall desktop_001.png]]

 

确认配置好了本地的原（挂载的iso: SLE-15-SP2-Full-x86_64-GM-Media1.iso）

开始安装

\# zypper -n install -t pattern x11 gnome_basic

![[__ SUSE ___004_SUSE15 reinstall desktop_002.png]]

 

控制界面命令启动图像界面：

\# sudo systemctl isolate graphical.target

经过测试，是可以的。

 

默认图像界面启动：

<https://www.server-world.info/en/note?os=SUSE_Linux_Enterprise_15&p=desktop&f=1>

 vi /etc/sysconfig/windowmanager

\# line 22: change

DEFAULT_WM=\"gnome\"

\# change to graphical login

\# ln -fs /usr/lib/systemd/system/graphical.target /etc/systemd/system/default.target

\# reboot

 

 

已使用 OneNote 创建。
