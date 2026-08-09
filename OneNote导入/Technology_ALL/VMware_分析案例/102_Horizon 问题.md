Horizon 问题

2019年6月3日

9:30

ST:G1KZKF2 \|SR:991491227 （客户自己解决的）

客户的vCenter有问题后重新部署，导致view有问题，vcenter server是红色状态，具体报错是证书无法被识别到。导致桌面池无法正常工作。

![[Technology_ALL_VMware_分析案例_102_Horizon 问题_001.jpg]]

 

 

应该是由两个问题导致的：

1，vcenter重装过后，系统默认采用了CA自签证书，并不在服务器的根证书信任列表中，所以我将这个证书放到了connection server和composer server的受信任的根证书颁发机构中

![[Technology_ALL_VMware_分析案例_102_Horizon 问题_002.jpg]]

 

![[Technology_ALL_VMware_分析案例_102_Horizon 问题_003.jpg]]

 

2，原来的VCURL实际上是[https://cnszn02a34:443/](https://cnszn02a34:443/)

我于上周五尝试用ADSI Editor在view connection server上将这个值(采用FQDN)已改为[https://cnszn02a34.ad005.onehc.net:443/](https://cnszn02a34.ad005.onehc.net:443/)

![[Technology_ALL_VMware_分析案例_102_Horizon 问题_004.jpg]]

 

服务器在重启之后已解决。

![[Technology_ALL_VMware_分析案例_102_Horizon 问题_005.jpg]]

 

 

 

已使用 OneNote 创建。
