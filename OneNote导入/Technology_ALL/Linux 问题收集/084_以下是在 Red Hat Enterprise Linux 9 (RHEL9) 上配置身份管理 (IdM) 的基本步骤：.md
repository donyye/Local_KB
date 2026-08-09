2023年2月15日

14:51

以下是在 Red Hat Enterprise Linux 9 (RHEL9) 上配置身份管理 (IdM) 的基本步骤：

 

安装 IdM 服务器：

 

运行以下命令作为 root 用户以安装 IdM 服务器软件包：

 

\# dnf install ipa-server

安装完成后，运行以下命令以设置 IdM 服务器：

 

\# ipa-server-install

此命令将启动 IdM 服务器的设置向导，您需要根据提示输入相关信息。

 

安装 IdM 客户端：

 

运行以下命令作为 root 用户以安装 IdM 客户端软件包：

\# dnf install ipa-client

安装完成后，运行以下命令以配置 IdM 客户端：

\# ipa-client-install

此命令将启动 IdM 客户端的设置向导，您需要根据提示输入相关信息。

 

配置防火墙：

 

如果您的系统使用防火墙，请确保打开必要的端口以允许 IdM 流量通过。以下是一些默认端口：

 

Copy code

tcp/80, tcp/443, tcp/389, tcp/636, udp/88, udp/464, udp/123

您可以通过运行以下命令打开上述端口：

\# firewall-cmd \--add-service=freeipa \--permanent

\# firewall-cmd \--reload

配置 DNS：

 

确保您的系统使用正确的 DNS 设置以允许 IdM 解析。以下是一些 DNS 设置：

domain example.com

nameserver 192.168.1.1

您可以通过编辑"/etc/resolv.conf"文件来配置 DNS 设置。

 

管理用户和主机：

 

通过 IdM 控制台或命令行工具，您可以添加、删除和管理用户和主机。

测试配置：

 

运行以下命令以测试 IdM 配置：

\$ kinit admin

\$ klist

如果您看到了有效的 Kerberos 凭据，这意味着您已成功配置了 IdM。

 

如果您在配置过程中遇到问题，请查看 IdM 日志以获取有关错误的更多信息。

 

已使用 OneNote 创建。
