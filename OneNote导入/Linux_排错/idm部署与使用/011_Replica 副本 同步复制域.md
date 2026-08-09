Replica 副本 同步复制域

2023年6月15日

11:35

<https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html-single/planning_identity_management/index#planning-the-replica-topology_planning-identity-management>

![[idm部署与使用_011_Replica 副本 同步复制域_001.png]]

 

\# rpm -qf /usr/sbin/ipa-replica-install

ipa-server-4.10.0-6.el9.x86_64

 

 

 

 

 

 

[\[root@secondary1 \~\]# ipa-replica-install \--principal admin \--admin-password P@ssw0rd!]

Configuring client side components

This program will set up IPA client.

Version 4.10.0

 

Discovery was successful!

Client hostname: secondary1.idm.domain.com

Realm: IDM.DOMAIN.COM

DNS Domain: idm.domain.com

IPA Server: master.idm.domain.com

BaseDN: dc=idm,dc=domain,dc=com

 

Synchronizing time

No SRV records of NTP servers found and no NTP server or pool address was provided.

Using default chrony configuration.

Attempting to sync time with chronyc.

Process chronyc waitsync failed to sync time!

Unable to sync time with chrony server, assuming the time is in sync. Please check that 123 UDP port is opened, and any time server is on network.

Successfully retrieved CA cert

    Subject:     CN=Certificate Authority,O=IDM.DOMAIN.COM

    Issuer:      CN=Certificate Authority,O=IDM.DOMAIN.COM

    Valid From:  2023-05-08 11:24:37

    Valid Until: 2043-05-08 11:24:37

 

Enrolled in IPA realm IDM.DOMAIN.COM

Created /etc/ipa/default.conf

Configured /etc/sssd/sssd.conf

Configured /etc/krb5.conf for IPA realm IDM.DOMAIN.COM

Systemwide CA database updated.

Hostname (secondary1.idm.domain.com) does not have A/AAAA record.

Missing reverse record(s) for address(es): 10.10.40.12, 10.10.250.32.

Adding SSH public key from /etc/ssh/ssh_host_ecdsa_key.pub

Adding SSH public key from /etc/ssh/ssh_host_ed25519_key.pub

Adding SSH public key from /etc/ssh/ssh_host_rsa_key.pub

SSSD enabled

Configured /etc/openldap/ldap.conf

Configured /etc/ssh/ssh_config

Configured /etc/ssh/sshd_config.d/04-ipa.conf

Configuring idm.domain.com as NIS domain.

Client configuration complete.

The ipa-client-install command was successful

 

Removing client side components

Unenrolling client from IPA server

Removing Kerberos service principals from /etc/krb5.keytab

Disabling client Kerberos and LDAP configurations

Redundant SSSD configuration file /etc/sssd/sssd.conf was moved to /etc/sssd/sssd.conf.deleted

Restoring client configuration files

Unconfiguring the NIS domain.

nscd daemon is not installed, skip configuration

nslcd daemon is not installed, skip configuration

Systemwide CA database updated.

Client uninstall complete.

The ipa-client-install command was successful

 

Your system may be partly configured.

Run /usr/sbin/ipa-server-install \--uninstall to clean up.

 

The host name master.idm.domain.com does not match the primary host name \_gateway. Please check /etc/hosts or DNS name resolution

The ipa-replica-install command failed. See /var/log/ipareplica-install.log for more information

\[root@secondary1 \~\]#

 

 

 

 

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

<https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html-single/installing_identity_management/index#preparing-the-system-for-ipa-replica-installation_installing-identity-management>

20.3. 授权在 IdM 客户端上安装副本

 

 

\[root@master \~\]# kinit admin

 

\[root@master \~\]# ipa hostgroup-add-member ipaservers \--hosts secon1.idm.domain.com

  Host-group: ipaservers

  Description: IPA server hosts

  Member hosts: master.idm.domain.com, secon1.idm.domain.com

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Number of members added 1

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

已使用 OneNote 创建。
