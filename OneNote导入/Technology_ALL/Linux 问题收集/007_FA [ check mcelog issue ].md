FA \[ check mcelog issue \]

Wednesday, June 11, 2014

9:02 AM

 

 

![[Technology_ALL_Linux 问题收集_007_FA [ check mcelog issue ]_001.jpg]]

 

![[Technology_ALL_Linux 问题收集_007_FA [ check mcelog issue ]_002.png]]

 

1. 查找mcelog生成前信息

1）[ automount\[3791\]: lookup_read_master: lookup(nisplus): couldn\'t locate nis+ table auto.master]

- As per the default settings in /etc/nsswitch.conf, the automount process tries to access an NIS+ server. If its not configured, the said messages will get logged and will subsequently fail. 
- [https://access.redhat.com/site/solutions/109583](https://access.redhat.com/site/solutions/109583)

与mcelog无关

 

2） failed to prefill DIMM database from DMI data\" in messages file

-  
- [https://access.redhat.com/site/solutions/68080](https://access.redhat.com/site/solutions/68080)

 

已使用 OneNote 创建。
