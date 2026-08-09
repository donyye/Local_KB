RHEL10-command-line assistant

2025年7月23日

11:19

 

 

\# dnf install command-line-assistant

 

\# journalctl -xb \|grep \'ERR\' \| c chat \"Please check if there are any errors\"

![[No boot-kdump_017_RHEL10-command-line assistant_001.png]]

 

 

 

\# ps aux \--sort=-%mem \| head -n 20 \| c chat \"Is there a possible memory leak in one of these processes?\"

![[No boot-kdump_017_RHEL10-command-line assistant_002.png]]

 

 

\# cat /var/log/messages \| c chat \"Look for critical errors in this log file.\"

![[No boot-kdump_017_RHEL10-command-line assistant_003.png]]

 

 

\# c chat -a /var/log/boot.log \"why did the last boot take so long\"

 

 

 

 

已使用 OneNote 创建。
