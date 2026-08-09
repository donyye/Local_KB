ESXI 批量注册vm

2023年9月22日

13:49

 

ssh ESXI,批量注册vm

find /vmfs/volumes -name \*.vmx -exec vim-cmd solo/registervm  \\;

 

volumes 后添加vm所在的 datastorage

 

![[Technology_ALL_VMware_分析案例_154_ESXI 批量注册vm_001.png]]

 

已使用 OneNote 创建。
