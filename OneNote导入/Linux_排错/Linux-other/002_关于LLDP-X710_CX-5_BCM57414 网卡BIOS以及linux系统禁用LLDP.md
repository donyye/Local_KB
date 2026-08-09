关于LLDP-X710/CX-5/BCM57414 网卡BIOS以及linux系统禁用LLDP

2025年7月17日

12:33

  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       关于X710/CX-5/BCM57414 网卡BIOS以及linux系统禁用LLDP
  发件人     Cao, Ting
  收件人     CN XMN TS ENT L2 SME
  抄送       Wang, Xing Fang
  发送时间   2025年7月17日 11:44
  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

Hi

Team

share with you

 

LKB:000345978

How to disabled network LLDP function from BIOS setting and linux system

 

Symptom:

PowerEdge Server 10G/25G NIC firmware LLDP function may cause network package lost randomly, especially when the nic work in teaming/boding mode.

If un-bonding or un-teaming the nic setting, this issue should be disappeared

If using 1G NIC in same network environment, there is no similar symptom

 

Resolution:

Need disable firmware LLDP from network side or switch side:

• It is not LLDP influence the network package lost, it is caused when switch use LLDP mechanism to learn mac address that could cause mac address flapping and then cause package lost.

Switch use 3 type protocol mechanism to learn mac address normally, include LLDP, ARP, LACP. So if switch use LLDP to do mac address learn, this issue maybe occur in dedicated situation.

 

Test system :sli8.8

X710: The LLDP feature for the network card can be disabled at the BIOS level.

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_001.png]]

 

 

Under the system, use the ethtool command to modify and disable the LLDP protocol feature. 

Note that this command will restore the default parameters of the network card after a server reboot, so a startup script needs to be added to make it effective automatically.

Example:

 

ethtool \--set-priv-flags enoxxx disable-fw-lldp \<off/on\>

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_002.png]]

 

 

2: [MT27800 Family \[ConnectX-5 Lx\]:] There is no LLDP setting for the network port under BIOS level, and LLDP is enabled by default at the hardware firmware level.

The Mellanox MFT tool can be used to disable LLDP under the system, but a reboot is required for the changes to take effect.

Example:

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_003.png]]

 

 

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_004.png]]

 

 

3:Broadcom 57414: The LLDP feature for the network card can be disabled at the BIOS level.

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_005.png]]

 

 

Under the system, the NICCLI Configuration Utility can be used to disable the LLDP feature for the Broadcom network card, which also requires a reboot to take effect.

Example:

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_006.png]]

 

 

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_007.png]]

 

 

 

![[Linux-other_002_关于LLDP-X710_CX-5_BCM57414 网卡BIOS以及linux系统禁用LLDP_008.png]]

 

 

Attached:

[Broadcom Niccli Utility - Linux](https://nam02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.broadcom.com%2Fdocs%2FLinux_Niccli-233.0.198.0&data=05%7C02%7CDony_Ye%40Dell.com%7Ca0efaaf203e64fd7676208ddc4e4306f%7C945c199a83a24e809f8c5a91be5752dd%7C0%7C0%7C638883206561529633%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=gEOhSreTb0JpmEgb5HH0XASKoekyEQbmFU3dKl8tOzU%3D&reserved=0)

[https://www.broadcom.com/support/download-search?pg=&pf=Ethernet+Network+Adapters&pn=&pa=&po=Dell&dk=&pl=&l=false](https://nam02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.broadcom.com%2Fsupport%2Fdownload-search%3Fpg%3D%26pf%3DEthernet%2BNetwork%2BAdapters%26pn%3D%26pa%3D%26po%3DDell%26dk%3D%26pl%3D%26l%3Dfalse&data=05%7C02%7CDony_Ye%40Dell.com%7Ca0efaaf203e64fd7676208ddc4e4306f%7C945c199a83a24e809f8c5a91be5752dd%7C0%7C0%7C638883206561559771%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=Gh6O3d%2Bze%2FVUc%2BPyfxKMSnHyB3u6gjG7dYac%2BMCsXD4%3D&reserved=0)

Mellanox Firmware Tool:

[https://network.nvidia.com/products/adapter-software/firmware-tools/](https://nam02.safelinks.protection.outlook.com/?url=https%3A%2F%2Fnetwork.nvidia.com%2Fproducts%2Fadapter-software%2Ffirmware-tools%2F&data=05%7C02%7CDony_Ye%40Dell.com%7Ca0efaaf203e64fd7676208ddc4e4306f%7C945c199a83a24e809f8c5a91be5752dd%7C0%7C0%7C638883206561575618%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=VVXNnBswHITrm5lQ0LtBHJQUEiQ5xBmhiyaaZjJBmlg%3D&reserved=0)

 

Internal Use - Confidential

 

已使用 OneNote 创建。
