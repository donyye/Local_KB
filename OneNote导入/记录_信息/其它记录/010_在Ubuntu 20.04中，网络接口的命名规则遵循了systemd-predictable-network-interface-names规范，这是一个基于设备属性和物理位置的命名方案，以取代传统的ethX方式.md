2023年4月10日

14:06

在Ubuntu 20.04中，网络接口的命名规则遵循了systemd-predictable-network-interface-names规范，这是一个基于设备属性和物理位置的命名方案，以取代传统的ethX方式。

 

具体来说，Ubuntu 20.04的网络接口命名规则如下：

 

以en为前缀，表示Ethernet接口，例如enp0s3。

 

接着是一个标识符，标识设备的物理位置。标识符的格式是由一到三个字符的PCI域（bus、slot和function）号，用冒号分隔。例如，0000:02:00.0表示PCI总线2上的第一个设备（插槽0），功能0。

 

最后是一个可选的、由设备驱动程序定义的、描述接口类型的名称。例如，如果设备是万兆以太网接口，则可以在末尾添加一个"-10"表示10 GbE。如果设备是无线接口，则可以在末尾添加"-wifi"。

 

需要注意的是，这个命名规则并不适用于所有的网络接口，如USB网络适配器等设备可能不会遵循该规则。但大部分以太网接口都会使用这个规则来命名。

 

 

要修改Ubuntu系统中网络接口的命名方式，可以通过修改配置文件的方式来实现。以下是具体步骤：

 

打开/编辑 /etc/default/grub 文件。例如使用命令：sudo nano /etc/default/grub

 

找到 GRUB_CMDLINE_LINUX_DEFAULT=\"quiet splash\" 这一行，在其中添加一个 net.ifnames=0 参数。修改后的行应该类似于：GRUB_CMDLINE_LINUX_DEFAULT=\"quiet splash net.ifnames=0\"

 

保存文件并关闭编辑器。

 

更新grub配置，运行命令：sudo update-grub

 

重新启动系统，新的命名方式将会生效。

 

 

net.ifnames=0参数将禁用Systemd Predictable Network Interface Names，从而恢复传统的网络接口命名方式，即eth0, eth1等。使用这个参数时，可以在启动时将其添加到内核命令行中，或将其添加到GRUB配置文件中以进行持久化设置。

 

biosdevname=0参数将禁用BIOS提供的网络接口名。这个参数适用于使用服务器类硬件的系统，这些硬件通常具有预先定义的网络接口名称。使用这个参数时，可以在启动时将其添加到内核命令行中，或将其添加到GRUB配置文件中以进行持久化设置。

 

已使用 OneNote 创建。
