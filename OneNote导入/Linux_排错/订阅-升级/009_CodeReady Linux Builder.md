[ CodeReady Linux Builder]

2025年3月25日

9:27

[https://access.redhat.com/zh_CN/articles/7051887](https://access.redhat.com/zh_CN/articles/7051887)\
\
如何启用 CodeReady Linux Builder 并使用其中提供的内容 

已更新 January 15 2024 at 3:06 AM - 

[Chinese](https://access.redhat.com/zh_CN/articles/7051887)

内容目录：

- [概述](https://access.redhat.com/zh_CN/articles/7051887#overview)
- [启用软件仓库](https://access.redhat.com/zh_CN/articles/7051887#enable)
- [CodeReady Linux Builder 模块](https://access.redhat.com/zh_CN/articles/7051887#modules)
- [如何从门户下载 CodeReady 软件包](https://access.redhat.com/zh_CN/articles/7051887#download)

 

概述

在 Red Hat Enterprise Linux 8 中，除了两个主要的软件仓库外，还有一个新的 CodeReady Linux Builder (CRB) 软件仓库，用于提供不被支持的额外内容。

有关此内容的概述信息，请参阅以下内容：

[CodeReady Linux Builder 软件仓库简介 - 红帽客户门户网站：](https://access.redhat.com/solutions/4180391)

因为 CRB 所提供的内容不被支持，所以它们被默认禁用。安装该仓库中提供的 RPM 和模块需要额外的操作。

 

启用软件仓库

要启用软件仓库，可以使用以下 subscription-manager 命令：

[Raw](https://access.redhat.com/zh_CN/articles/7051887#)

\# subscription-manager repos \--enable \<repo-id\>      ##syntax##

\# subscription-manager repos \--enable codeready-builder-for-rhel-8-x86_64-rpms

这将允许使用大多数内容用于安装。具体来说，任何以非模块化 RPM 形式提供的内容都可以被安装。

 

CodeReady Linux Builder 模块

CRB 软件仓库还包括了多个模块。以下是编写本文时可用的模块列表：

[Raw](https://access.redhat.com/zh_CN/articles/7051887#)

\# yum module list \--disablerepo=\* \--enablerepo=codeready-builder-for-rhel-8-x86_64-rpms

Updating Subscription Management repositories.

Last metadata expiration check: 0:40:08 ago on Tue 13 Aug 2019 08:49:29 AM EDT.

Red Hat CodeReady Linux Builder for RHEL 8 x86_64 (RPMs)

Name                      Stream        Profiles        Summary                                          

javapackages-tools        201801        common          Tools and macros for Java packaging support      

virt-devel                rhel                          Virtualization module                            

 

Hint: \[d\]efault, \[e\]nabled, \[x\]disabled, \[i\]nstalled

在上面的输出中，javapackages-tools 模块包含了一个 common 配置集（profile），但它没有被定义为是默认的。virt-devel 模块没有配置集。对于这两个模块，它们没有配置集或其配置集没有被设置为是默认的，因此需要启用相关的模块流以安装它们的内容。

[Raw](https://access.redhat.com/zh_CN/articles/7051887#)

\# yum module info javapackages-tools \| grep xz-java

: xz-java-0:1.8-2.module+el8+2598+06babf2e.noarch

: xz-java-javadoc-0:1.8-2.module+el8+2598+06babf2e.noarch

 

\# yum install xz-java

Updating Subscription Management repositories.

Last metadata expiration check: 0:42:24 ago on Tue 13 Aug 2019 08:49:30 AM EDT.

No match for argument: xz-java

Error: Unable to find a match

上面的输出显示，在默认情况下不会安装模块化的 xz-java 软件包。要安装它，需要先启用该模块。

[Raw](https://access.redhat.com/zh_CN/articles/7051887#)

\# yum module enable javapackages-tools

Updating Subscription Management repositories.

Last metadata expiration check: 0:42:33 ago on Tue 13 Aug 2019 08:49:30 AM EDT.

Dependencies resolved.

=========================================================================================================

Package                 Arch                   Version                    Repository               Size

=========================================================================================================

Enabling module streams:

javapackages-tools                             201801  

 

Transaction Summary

=========================================================================================================

 

Is this ok \[y/N\]: y

Complete!

 

\[root@localhost \~\]# yum install xz-java

Updating Subscription Management repositories.

Last metadata expiration check: 0:42:40 ago on Tue 13 Aug 2019 08:49:30 AM EDT.

Dependencies resolved.

=========================================================================================================

Package          Arch   Version                          Repository                                Size

=========================================================================================================

Installing:

xz-java          noarch 1.8-2.module+el8+2598+06babf2e   codeready-builder-for-rhel-8-x86_64-rpms 106 k

Installing dependencies:

javapackages-filesystem

noarch 5.3.0-2.module+el8+2598+06babf2e codeready-builder-for-rhel-8-x86_64-rpms  30 k

copy-jdk-configs noarch 3.7-1.el8                        rhel-8-for-x86_64-appstream-rpms          27 k

libjpeg-turbo    x86_64 1.5.3-7.el8                      rhel-8-for-x86_64-appstream-rpms         155 k

lua              x86_64 5.3.4-10.el8                     rhel-8-for-x86_64-appstream-rpms         192 k

java-1.8.0-openjdk-headless

x86_64 1:1.8.0.222.b10-0.el8_0          rhel-8-for-x86_64-appstream-rpms          32 M

tzdata-java      noarch 2019b-1.el8                      rhel-8-for-x86_64-appstream-rpms         189 k

lksctp-tools     x86_64 1.0.18-3.el8                     rhel-8-for-x86_64-baseos-rpms            100 k

 

Transaction Summary

=========================================================================================================

Install  8 Packages

 

Total download size: 33 M

Installed size: 114 M

Is this ok \[y/N\]: y

\<snip\>

Installed:

xz-java-1.8-2.module+el8+2598+06babf2e.noarch

javapackages-filesystem-5.3.0-2.module+el8+2598+06babf2e.noarch

copy-jdk-configs-3.7-1.el8.noarch

libjpeg-turbo-1.5.3-7.el8.x86_64

lua-5.3.4-10.el8.x86_64

java-1.8.0-openjdk-headless-1:1.8.0.222.b10-0.el8_0.x86_64

tzdata-java-2019b-1.el8.noarch

lksctp-tools-1.0.18-3.el8.x86_64

 

Complete!

如何从门户下载 CodeReady 软件包

您可以从以下链接下载 CodeReady 软件包。

<https://access.redhat.com/downloads/content/491/ver=/rhel---8/8/x86_64/packages>

 

已使用 OneNote 创建。
