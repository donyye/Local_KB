Share: 关闭ESXi 6.X的密码复杂性要求

Friday, September 02, 2016

5:04 PM

  -------------------------------------- ---------------------------------------------------------------------------------
  主题       Share: 关闭ESXi 6.X的密码复杂性要求
  发件人     Yin, Guoxun
  收件人     CN XMN TS ENT L2 SME
  发送时间   Friday, September 02, 2016 4:10 PM
  -------------------------------------- ---------------------------------------------------------------------------------

 

正好因为一个CASE给用户写了个简单的指导，顺便也share下给大家，

ESXi 5.x 6.x 只有在安装的时候允许设置诸如 password一样的简单密码，安装完成后默认再也不允许使用简单密码。

如果用户要修改密码但是又不希望密码太复杂太难记，那么按照以下步骤做完后就可以设置简单弱密码了。

 

 

Enable local shell in DCUI at ESXi host

![[Technology_ALL_VMware_分析案例_036_Share_ 关闭ESXi 6.X的密码复杂性要求_001.jpg]]

 

![[Technology_ALL_VMware_分析案例_036_Share_ 关闭ESXi 6.X的密码复杂性要求_002.jpg]]

 

Press Alt+F1 to login the command line user interface with root account at DCUI

 

![[Technology_ALL_VMware_分析案例_036_Share_ 关闭ESXi 6.X的密码复杂性要求_003.png]]

 

 

Bacnkup the /etc/pam.d/passwd file to passwd.bak

Command: cp /etc/pam.d/passwd  /etc/pam.d/passwd.bak

 

Change the related section in /etc/pam.d/passwd as the below picture

Command: vi /etc/pam.d/passwd

![[Technology_ALL_VMware_分析案例_036_Share_ 关闭ESXi 6.X的密码复杂性要求_004.png]]

 

Save the configuration and exit from the vi editor. Now the root password can be changed to nutnaix\\4u

 

 

 

 

BR.

Guoxun.

 

 

已使用 OneNote 创建。
