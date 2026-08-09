RHEL10-Kickstart

2025年7月23日

10:10

 

 

<https://access.redhat.com/labs/kickstartconfig>

 

Kickstart 生成器站点会呈现对话框供用户输入信息，并根据用户的选择生成一个 Kickstart 配置文件。每个对话框对应于 Anaconda 安装程序中的可配置项。

 

 

![[No boot-kdump_016_RHEL10-Kickstart_001.png]]

 

 

 

 

\# version=RHEL10

\# Use text install mode\
text[  ]\--\> 文本安装

\# Keyboard layouts\
keyboard \--vckeymap=us \--xlayouts=\'us\'

\# System language\
lang en_US.UTF-8

\# Use network installation\
url \--url=\"http://content.example.com/rhel10.0/x86_64/dvd/\"\
repo \--name=\"AppStream\" \--baseurl=\"http://content.example.com/rhel10.0/x86_64/dvd/AppStream\"

%packages\
@\^minimal-environment\
\@guest-agents\
vim-enhanced\
%end

\# Don\'t run the Setup Agent on first boot\
firstboot \--disable

\# Storage commands\
\# Partition clearing information\
zerombr\
clearpart \--all \--initlabel\
autopart\
bootloader \--location=mbr

\# System timezone\
timezone America/Chicago \--utc

\# Root password\
rootpw \--lock\
user \--groups=wheel \--name=student \--password=student \--plaintext \--gecos=\"student\"

%addon com_redhat_kdump \--enable \--reserve-mb=\'auto\'\
%end

%post\
mandb\
echo \"Kickstarted on \$(date +%F)\" \>\> /etc/issue\
%end

 

安装 pykickstart 包

\$ sudo dnf install pykickstart

 

 

使用 ksvalidator 命令确保你的 Kickstart 文件语法正确

\$ ksvalidator \~/RH304/labs/installing-kickstart/kickstart.cfg

Checking kickstart file /home/student/RH304/labs/installing-kickstart/kickstart.cfg

 

没问题后拷贝文件到 web 目录

\$ sudo cp \~/RH304/labs/installing-kickstart/kickstart.cfg [ ]/var/www/html/

 

 

启动使用PXE引导，然后在该行末尾添加 inst.ks=http://servera.lab.example.com/kickstart.cfg 启动选项。

![[No boot-kdump_016_RHEL10-Kickstart_002.png]]

 

![[No boot-kdump_016_RHEL10-Kickstart_003.png]]

安装完成后按 enter 

 

 

 

已使用 OneNote 创建。
