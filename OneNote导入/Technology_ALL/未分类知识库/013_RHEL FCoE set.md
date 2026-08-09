RHEL FCoE set

Tuesday, July 29, 2014

12:15 PM

Setting-up FCoE for RHEL 6.2

Installing / verifying FCoE support

If FCoE support was not installed in RHEL 6.2, install it. To verify if FCoE support was installed in RHEL 6.2, perform the following steps:

 

1. Run the following command: rpm -qa \| grep fcoe-utils. If no information is returned, as in Figure 24, then the fcoe-utils package is not installed; it must be installed at this time.

 

Figure 24. No FCoE support is installed. 

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_001.png]]

 

2. To install the appropriate FCoE support package (for example: install the fcoe-utils package), insert Disc 1 of the Red Hat Enterprise Linux 6.2 installation media into your CD/DVD drive, and then mount the disc to a directory of your choice; in this example, use /media, as in Figure 25.

 

Figure 25. Mounting the installation media.

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_002.png]]

 

3. Next, change directory to /media/Packages, and install the appropriate packages; for example, if you installed your copy of RHEL 6.2 as a Basic Server, you need the following packages: libhbaapi; libconfig; lldpad; libpciaccess; libhbalinux; device-mapper-multipath; device-mapper-multipath-libs; fcoe-utils. See Figure 26 for details.

 

Figure 26. Installing FCoE support.

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_003.png]]

 

 

4. After package installation is complete, you can then change back to the home directory and unmount your installation media by running the cd \~ and umount /media command, as in Figure 27.

 

Figure 27.[  Unmounting the installation media.]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_004.png]]

 

 

Configuring the FCoE client VLAN

 

To configure the FCoE client VLAN, perform the following steps:

 

1. Change directory to /etc/fcoe, and copy the /etc/fcoe/cfg-ethx file to /e4tc/fcoe/\<interface\>, where \<interface\> is the name of the specific network interface over which FCoE traffic flows. In this example, we will use the interface name p3p1. See Figure 28 for details.

 

Figure 28.[  Creating the FCoE Adapter Configuration file.]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_005.png]]

 

2. Then edit the /etc/fcoe/cfg-p3p1 file and set DCB_REQUIRED variable to \"no\", as in Figure 29.

 

Figure 29.[  Setting the DCB_REQUIRED variable to \"no\".]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_006.png]]

 

3. Next, check the /etc/fcoe/config file, and verify that the SUPPORTED_DRIVERS variable is set to \"fcoe bnx2fc\", as in Figure 30.

 

Figure 30.[  Checking the SUPPORTED_DRIVERS variable.]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_007.png]]

 

4. Then, start the lldpad and fcoe services running the service lldpad start and service fcoe start commands. Then running the fcoeadm -i command to verify that the FCoE VLAN is properly configured, as in Figure 31.

 

 

Figure 31.[  Verifying proper configuration of FCoE VLAN.]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_008.png]]

 

5. Finally, verify LUNs device name by listing all available partitions using the cat /proc/partitions command, as in Figure 32.

 

Figure 32.[  Verifying LUNs availability.]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_009.png]]

 

Partition and mount your FCoE LUNs as any other hard disk device.

 

Configure the FCoE client to start at boot

 

If you want the FCoE client to start and the FCoE LUNs to automatically be available after every reboot, configure the appropriate services using chkconfig. To do this, perform the following steps:

 

1. To enable FCoE at boot time, run the following commands: chkconfig lldpad on and chkconfig fcoe on, as in Figure 33.

 

Figure 33.[  Configuring FCoE to start at system boot.]

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_010.png]]

 

2. Verify that the services are set to start at boot by running chkconfig \--list \| grep lldpad and chkconfig \--list \| grep fcoe, as in Figure 34.

 

Figure 34. Verifying that FCoE services are configured to start at boot time

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_011.png]]

 

Troubleshooting

 

It is possible for other various conditions to exist on the network that can interfere with FIPS snooping, particularly on congested networks. If the FCoE client is unable to see the FCoE VLAN after booting, it is recommend to restart the lldpad and fcoe daemons as a troubleshooting step.

 

1. To restart the lldpad and fcoe daemons, run the following commands:

service fcoe stop; service lldpad stop; service lldpad start; service fcoe start

Then check the FCoE VLAN status by running fcoeadm -i. See Figure 35 for details.

 

Figure 35. Restarting the lldpad and fcoe daemons.

![[Technology_ALL_未分类知识库_013_RHEL FCoE set_012.png]]

 

Conclusion

While it is not possible to cover every conceivable combination of FCoE hardware in a single document, most modern FCoE implementations are likely be fairly similar on the various available FCoE compliant devices. Use this document as a general guide to aid you in configuring FCoE in most situations. If you do use this guide with hardware other than that listed in the Described Configuration section above, verify that it is FCoE compliant and that all appropriate firmware and drivers are updated per vendor specifications prior to attempting to configure FCoE.

 

已使用 OneNote 创建。
