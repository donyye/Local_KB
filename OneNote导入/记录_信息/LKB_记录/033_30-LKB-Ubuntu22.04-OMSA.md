30-LKB-Ubuntu22.04-OMSA

2025年5月9日

14:33

LKB-000350902\| 提交时间 2025-07-29 \| Q3 \
 

Title:

PowerEdge: ubuntu 20.04 install OMSA v11.1.0.0 and snmp.

 

Summanry: 

ubuntu 22.04 can link successfully using snmp after installing OMSA.

 

Symptoms:

OMSA is installed in ubuntu 22.04 with a dependency package error, it is suggested to install it through the following KB recommended way.

 

<https://linux.dell.com/repo/community/openmanage/>

 

Cause:

N/A

 

Resolution:

1. OMSA specific installation steps:

 

\# sudo echo \'deb [http://linux.dell.com/repo/community/openmanage/11100/jammy](http://linux.dell.com/repo/community/openmanage/11100/jammy) jammy main\' \| sudo tee -a /etc/apt/sources.list.d/linux.dell.com.sources.list

 

\# sudo apt-key add 0x1285491434D8786F.asc 

 

\# sudo apt-get update

 

\# sudo apt-get install srvadmin-all

 

\# service dsm_om_connsvc start

 

OMSA services:

dsm_sa_datamgrd

dsm_om_connsvc

dsm_sa_snmpd

dsm_sa_eventmgrd

 

2. SNMP install:

\# sudo apt-get install snmp snmpd snmp-mibs-downloader

 

Configuration file changes:

/etc/init.d/snmpd.conf

![[记录_信息_LKB_记录_033_30-LKB-Ubuntu22.04-OMSA_001.png]]

 

\# sudo service snmpd restart

 

Use another ubuntu system to test this.

\# snmpwalk -v 2c -c public localhost .1.3.6.1.4.1.674.10892.1

![[记录_信息_LKB_记录_033_30-LKB-Ubuntu22.04-OMSA_002.png]]

 

Keywords: 

PowerEdge,ubuntu 22.04, OMSA, snmp

 

已使用 OneNote 创建。
