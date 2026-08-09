Secure Boot

2025年2月17日

11:15

Secure Boot，安全启动，这个是BIOS上的一个选项，它默认是关闭的。\
它有一个表，里面是一些厂商的证书，这些硬件就能在开启 Secure Boot 后也能正常使用。

 

再idrac里可以查看和关闭与开启。

登陆idrac:

维护 \--\> System security \--\> Secure Boot

![[No boot-kdump_010_Secure Boot_001.png]]

 

这里看到灰色无法选择的原因是我使用的 BIOS 安装模式，不是使用的 UEFI 模式，所以无法选择关闭和开启，因为它只是支持UEFI。

 

目前有些案例是添加了新的网卡，但是系统无法认定此网卡，无法使用。

1）此网卡需要Dell原厂网卡，因为一般Dell网卡都会在 Secure Boot 里有list。

2）可能是网卡FW的版本有这证书方面的缺失。

3）此硬件没有在Secure Boot里有list。

是否需要导入证书到 Secure Boot里？这个需要到BIOS里做，听说非常麻烦。

 

影响系统启动

比如系统是开启 Secure Boot 安装的，安装时会提示说有个证书导出，当出现MB更换，需要重新导入证书后系统才能启动，否则启动会失败。

 

 

 

已使用 OneNote 创建。
