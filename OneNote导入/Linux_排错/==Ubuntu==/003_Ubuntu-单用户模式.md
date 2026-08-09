Ubuntu-单用户模式

2024年7月24日

15:34

1.重启系统，快速按住"Shift"进入grub，选择"Advanced options for Ubuntu"按回车;

 

![[__Ubuntu___003_Ubuntu-单用户模式_001.png]]

 

2.选择"Ubuntu，with Linux 5.15.0-107-generic(recovery mode)"（选择高版本选项），按"e"

![[__Ubuntu___003_Ubuntu-单用户模式_002.png]]

 

3.修改配置文件，进入编辑页面，使用键盘的方向键，移动光标向下至linux命令开头的一行，并在本行中将 ro 至末尾的内容删除并替换为 rw init=/bin/bash，按下键盘的ctrl+x组合键，系统会直接进入单用户模式

![[__Ubuntu___003_Ubuntu-单用户模式_003.png]]

 

改成如下：

![[__Ubuntu___003_Ubuntu-单用户模式_004.png]]

 

 

4.系统加载完毕，按回车键，执行 mount \| grep -w / 命令确认根目录是否已被挂载为rw可读写权限。输入 passwd修改root密码。

出现 password updated successfully 说明修改过成功。

输入 exec /sbin/init 重启系统。

![[__Ubuntu___003_Ubuntu-单用户模式_005.png]]

 

 

 

已使用 OneNote 创建。
