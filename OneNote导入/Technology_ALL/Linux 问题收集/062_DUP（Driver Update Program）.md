DUP（Driver Update Program）

2020年4月27日

13:39

DUP概述[  ]From \<[https://access.redhat.com/articles/64322](https://access.redhat.com/articles/64322)\> 

 

驱动程序更新程序（DUP）是Red Hat用于在正常操作系统更新周期之外向Red Hat Enterprise Linux（RHEL）添加更新的驱动程序的一种机制。它允许将正在开发的RHEL更新版本中的较新驱动程序反向移植到基于ISO映像的" DUP磁盘"中，该DUP磁盘可在安装时在当前发行的RHEL版本中加载。该计划的目的是允许供应商认证新硬件，而不必等待RHEL的下一个更新版本。

 

这是该程序如何工作的示例。网络/存储控制器供应商Acme正在交付一个新的聚合网络适配器（CNA），该适配器在RHEL 8.0发布之后但在RHEL 8.1发布之前可用。一旦新卡的驱动程序已被RHEL 8.1接受，就可以使用该驱动程序并将其反向移植到DUP磁盘上，该磁盘将允许该卡在RHEL 8.0中工作。这样一来，结合了Acme新的CNA的系统供应商就可以在RHEL 8.0上认证他们的系统，而不必等到8.1。

有关驱动程序更新程序磁盘的支持策略的更多信息，请参阅Red Hat Knowledgebase文章，标题[为Red Hat驱动程序更新程序的支持策略](https://access.redhat.com/articles/2743651)。

为Red Hat Enterprise Linux 8下载DUP磁盘

通过Red Hat客户门户网站[https://access.redhat.com](https://access.redhat.com/)将DUP磁盘提供给用户。由于身份验证信息嵌入RPM / ISO文件下载URL中的方式，我们无法提供到这些文件的直接链接。我们所提供的是指向列出DUP软件包的页面的链接。从下面的适当链接开始，以查找RHEL 8所有更新版本的DUP磁盘：

x86_64架构

[https://access.redhat.com/downloads/content/479/ver=/rhel\-\--8/8.0/x86_64/product-software](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.0/x86_64/product-software)

如果您需要DUP磁盘用于其他更新版本，请使用版本下拉列表框选择合适的一个。

s390x架构

[https://access.redhat.com/downloads/content/72/ver=/rhel\-\--8/8.0/s390x/product-software](https://access.redhat.com/downloads/content/72/ver=/rhel---8/8.0/s390x/product-software)

如果您需要DUP磁盘用于其他更新版本，请使用版本下拉列表框选择合适的一个。

ppc64le架构

[https://access.redhat.com/downloads/content/279/ver=/rhel\-\--8/8.0/ppc64le/product-software](https://access.redhat.com/downloads/content/279/ver=/rhel---8/8.0/ppc64le/product-software)

如果您需要DUP磁盘用于其他更新版本，请使用版本下拉列表框选择合适的一个。

aarch64体系结构

[https://access.redhat.com/downloads/content/419/ver=/rhel\-\--8/8.0/aarch64/product-software](https://access.redhat.com/downloads/content/419/ver=/rhel---8/8.0/aarch64/product-software)

如果您需要DUP磁盘用于其他更新版本，请使用版本下拉菜单框选择合适的一个。

下载适用于Red Hat Enterprise Linux 7和更早版本的DUP磁盘

通过Red Hat客户门户网站[https://access.redhat.com](https://access.redhat.com/)将DUP磁盘提供给用户。由于身份验证信息嵌入RPM / ISO文件下载URL中的方式，我们无法提供到这些文件的直接链接。我们所提供的是指向列出DUP软件包的页面的链接。从下面的适当链接开始，以查找所有RHEL 7和更早版本的DUP磁盘：

x86_64和x86体系结构

[https://access.redhat.com/downloads/content/69/ver=/rhel\-\--7/7.0/x86_64/product-downloads](https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.0/x86_64/product-downloads)

如果您需要DUP磁盘用于其他更新版本或其他体系结构，请使用版本下拉框，然后使用体系结构下拉框选择适当的版本（例如RHEL 6.4，x86）。

ppc64体系结构

[https://access.redhat.com/downloads/content/74/ver=/rhel\-\--7/7.0/ppc64/product-downloads](https://access.redhat.com/downloads/content/74/ver=/rhel---7/7.0/ppc64/product-downloads)

如果您需要DUP磁盘用于其他版本，请使用版本下拉框选择适当的一个（例如RHEL 6.7）。

ppc64le架构

[https://access.redhat.com/downloads/content/279/ver=/rhel\-\--7/7.1/ppc64le/product-software](https://access.redhat.com/downloads/content/279/ver=/rhel---7/7.1/ppc64le/product-software)

如果您需要DUP磁盘用于其他发行版，请使用版本下拉框选择适当的一个（例如RHEL 7.3）。

使用DUP磁盘

当安装需要DUP磁盘才能启用硬件的系统时：

1.  将DUP磁盘ISO刻录到BD / DVD / CD，将其写入USB闪存驱动器，或者通过虚拟磁盘将其提供给系统。
2.  引导安装程序介质，并为要安装的RHEL版本使用适当的引导时间选项：
    - 对于RHEL 6和5，请使用选项" dd"。" dd"选项代表"驱动程序磁盘"。
    - 对于RHEL 8和7，请使用选项" inst.dd"。
3.  出现提示时，使DUP磁盘对系统可用。
4.  选择您需要完成安装的驱动程序。
5.  继续正常安装。

 

已使用 OneNote 创建。
