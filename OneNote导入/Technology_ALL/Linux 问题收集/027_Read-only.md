Read-only

Monday, July 17, 2017

3:16 PM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------
    主题       RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648
    发件人     G, Zhenzhen
    收件人     Zhang, Jianguo; Wang, Andy King; Samuel, Su; Li, Jim
    抄送       Dong, Peter; CN XMN TS ENT L2 SME; Ma1, Mark
    发送时间   Monday, July 17, 2017 2:59 PM
    -------------------------------------- -----------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

   

  Hi jianguo

  客户输出信息如下：

   

   

   

   

  1.  2．运行\# for i in ; do ls -F /sys/class/block/sd\$i/device/enclosure\*; done，将显示输出存档。

   

  ::: 
  +--------------------------------------------------------------------------------------+
  | # for i in ; do ls -F /sys/class/block/sd\$i/device/enclosure\*; done          |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sda/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | /sys/class/block/sdb/device/enclosure_device:ArrayDevice0B@                          |
  |                                                                                      |
  | /sys/class/block/sdc/device/enclosure_device:ArrayDevice0A@                          |
  |                                                                                      |
  | /sys/class/block/sdd/device/enclosure_device:ArrayDevice09@                          |
  |                                                                                      |
  | /sys/class/block/sde/device/enclosure_device:ArrayDevice08@                          |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdf/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | /sys/class/block/sdg/device/enclosure_device:ArrayDevice06@                          |
  |                                                                                      |
  | /sys/class/block/sdh/device/enclosure_device:ArrayDevice05@                          |
  |                                                                                      |
  | /sys/class/block/sdi/device/enclosure_device:ArrayDevice04@                          |
  |                                                                                      |
  | /sys/class/block/sdj/device/enclosure_device:ArrayDevice03@                          |
  |                                                                                      |
  | /sys/class/block/sdk/device/enclosure_device:ArrayDevice02@                          |
  |                                                                                      |
  | /sys/class/block/sdl/device/enclosure_device:ArrayDevice01@                          |
  |                                                                                      |
  | /sys/class/block/sdm/device/enclosure_device:ArrayDevice00@                          |
  |                                                                                      |
  | /sys/class/block/sdn/device/enclosure_device:ArrayDevice07@                          |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdo/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdp/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdq/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdr/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sds/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdt/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdu/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdv/device/enclosure\*: No such file or directory |
  |                                                                                      |
  | ls: cannot access /sys/class/block/sdw/device/enclosure\*: No such file or directory |
  +--------------------------------------------------------------------------------------+
  :::

   

   3.  运行命令\# sas2ircu 0 display, 将显示输出存档。

  ::: 
  +--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  | # df -h                                                                                                                                                                  |
  |                                                                                                                                                                          |
  | Filesystem                               Size  Used Avail Use% Mounted on                                                                                                |
  |                                                                                                                                                                          |
  | /dev/mapper/v_root-lv_root                99G  4.4G   90G   5% /                                                                                                         |
  |                                                                                                                                                                          |
  | tmpfs                                     63G     0   63G   0% /dev/shm                                                                                                  |
  |                                                                                                                                                                          |
  | /dev/sda1                               1008M   63M  936M   7% /boot                                                                                                     |
  |                                                                                                                                                                          |
  | /dev/sdb1                                2.8T  469G  2.3T  17% /data01                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdc1                                2.8T  468G  2.3T  17% /data02                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdd1                                2.8T  462G  2.3T  17% /data03                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sde1                                2.8T  468G  2.3T  17% /data04                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdg1                                2.8T  466G  2.3T  17% /data06                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdh1                                2.8T  473G  2.3T  17% /data07                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdi1                                2.8T  462G  2.3T  17% /data08                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdj1                                2.8T  464G  2.3T  17% /data09                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdk1                                2.8T  468G  2.3T  17% /data10                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdl1                                2.8T  466G  2.3T  17% /data11                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/sdm1                                2.8T  465G  2.3T  17% /data12                                                                                                   |
  |                                                                                                                                                                          |
  | /dev/mapper/v_root-lv_home                63G  180M   60G   1% /home                                                                                                     |
  |                                                                                                                                                                          |
  | /dev/mapper/v_root-lv_software_cloudera   99G  2.2G   92G   3% /software/cloudera                                                                                        |
  |                                                                                                                                                                          |
  | /dev/mapper/v_root-lv_software_lib        99G  193M   94G   1% /software/lib                                                                                             |
  |                                                                                                                                                                          |
  | /dev/mapper/v_root-lv_software_log        99G  1.6G   92G   2% /software/log                                                                                             |
  |                                                                                                                                                                          |
  | cm_processes                              63G   27M   63G   1% /var/run/cloudera-scm-agent/process                                                                       |
  |                                                                                                                                                                          |
  | /dev/sdf1                                2.8T  109G  2.7T   4% /data05                                       |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  | # for i in \$(df -h\|grep \'/data\'\|awk \'\'); do touch \$i/test.txt; done                                                                                   |
  |                                                                                                                                                                          |
  | touch: cannot touch \`/data05/test.txt\': Input/output error                           |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  | # ll /dev/disk/by-id\|grep sdf                                                                                                                                           |
  |                                                                                                                                                                          |
  | lrwxrwxrwx 1 root root  9 Jun 28 15:40 ata-TOSHIBA_MG03ACA300\_Y4KEK1YBF -\> ../../sdf |
  |                                                                                                                                                                          |
  | lrwxrwxrwx 1 root root 10 May 26 10:56 ata-TOSHIBA_MG03ACA300_Y4KEK1YBF-part1 -\> ../../sdf1                                                                             |
  |                                                                                                                                                                          |
  | lrwxrwxrwx 1 root root  9 Jun 28 15:40 scsi-SATA_TOSHIBA_MG03ACA_Y4KEK1YBF -\> ../../sdf                                                                                 |
  |                                                                                                                                                                          |
  | lrwxrwxrwx 1 root root 10 May 26 10:56 scsi-SATA_TOSHIBA_MG03ACA_Y4KEK1YBF-part1 -\> ../../sdf1                                                                          |
  |                                                                                                                                                                          |
  | lrwxrwxrwx 1 root root  9 Jun 28 15:40 wwn-0x50000395ebd82af6 -\> ../../sdf                                                                                              |
  |                                                                                                                                                                          |
  | lrwxrwxrwx 1 root root 10 May 26 10:56 wwn-0x50000395ebd82af6-part1 -\> ../../sdf1                                                                                       |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  | \# ./sas2ircu_new 0 display                                                                                                                                              |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  | Device is a Hard disk                                                                                                                                                    |
  |                                                                                                                                                                          |
  |   Enclosure #                             : 2                                                                                                                            |
  |                                                                                                                                                                          |
  |   Slot #                                             : 7                                                                                                                                                           |
  |                                                                                                                                                                          |
  |   SAS Address                             : 500262d-0-0bd0-5924                                                                                                          |
  |                                                                                                                                                                          |
  |   State                                   : Ready (RDY)                                                                                                                  |
  |                                                                                                                                                                          |
  |   Size (in MB)/(in sectors)               : 2861588/5860533167                                                                                                           |
  |                                                                                                                                                                          |
  |   Manufacturer                            : ATA                                                                                                                          |
  |                                                                                                                                                                          |
  |   Model Number                            : TOSHIBA MG03ACA3                                                                                                             |
  |                                                                                                                                                                          |
  |   Firmware Revision                       : FL1D                                                                                                                         |
  |                                                                                                                                                                          |
  |   Serial No                               : Y4KEK1YBF                                                                                                                    |
  |                                                                                                                                                                          |
  |   GUID                                    : 50000395ebd82af6                                                                                                             |
  |                                                                                                                                                                          |
  |   Protocol                                : SATA                                                                                                                         |
  |                                                                                                                                                                          |
  |   Drive Type                              : SATA_HDD                                                                                                                     |
  |                                                                                                                                                                          |
  |                                                                                                                                                                          |
  +--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  :::

   

  4.运行 \# for i in ; do echo sd\$i; cat /sys/class/block/sd\$i/device/sas_address; done，将显示输出存档。

         

  ::: 
  +-------------------------------------------------------------------------+
  | sda                                                                     |
  |                                                                         |
  | 0x01e891e2dd7bae5c                                                      |
  |                                                                         |
  | sdb                                                                     |
  |                                                                         |
  | 0x500262d00bd05920                                                      |
  |                                                                         |
  | sdc                                                                     |
  |                                                                         |
  | 0x500262d00bd05921                                                      |
  |                                                                         |
  | sdd                                                                     |
  |                                                                         |
  | 0x500262d00bd05922                                                      |
  |                                                                         |
  | sde                                                                     |
  |                                                                         |
  | 0x500262d00bd05923                                                      |
  |                                                                         |
  | sdf                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdf/device/sas_address: No such file or directory |
  |                                                                         |
  | sdg                                                                     |
  |                                                                         |
  | 0x500262d00bd05925                                                      |
  |                                                                         |
  | sdh                                                                     |
  |                                                                         |
  | 0x500262d00bd05926                                                      |
  |                                                                         |
  | sdi                                                                     |
  |                                                                         |
  | 0x500262d00bd05927                                                      |
  |                                                                         |
  | sdj                                                                     |
  |                                                                         |
  | 0x500262d00bd05928                                                      |
  |                                                                         |
  | sdk                                                                     |
  |                                                                         |
  | 0x500262d00bd05929                                                      |
  |                                                                         |
  | sdl                                                                     |
  |                                                                         |
  | 0x500262d00bd0592a                                                      |
  |                                                                         |
  | sdm                                                                     |
  |                                                                         |
  | 0x500262d00bd0592b                                                      |
  |                                                                         |
  | sdn                                                                     |
  |                                                                         |
  | 0x500262d00bd05924                                                      |
  |                                                                         |
  | sdo                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdo/device/sas_address: No such file or directory |
  |                                                                         |
  | sdp                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdp/device/sas_address: No such file or directory |
  |                                                                         |
  | sdq                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdq/device/sas_address: No such file or directory |
  |                                                                         |
  | sdr                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdr/device/sas_address: No such file or directory |
  |                                                                         |
  | sds                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sds/device/sas_address: No such file or directory |
  |                                                                         |
  | sdt                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdt/device/sas_address: No such file or directory |
  |                                                                         |
  | sdu                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdu/device/sas_address: No such file or directory |
  |                                                                         |
  | sdv                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdv/device/sas_address: No such file or directory |
  |                                                                         |
  | sdw                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdw/device/sas_address: No such file or directory |
  |                                                                         |
  | sdx                                                                     |
  |                                                                         |
  | cat: /sys/class/block/sdx/device/sas_address: No such file or directory |
  +-------------------------------------------------------------------------+
  :::

   

   

   

   

  Guo Zhenzhen

  Enterprise Engineer ,Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  [Zhenzhen_G@dell.com](mailto:Zhenzhen_G@dell.com)

   

  From: Zhang, Jianguo

  Sent: Monday, July 17, 2017 2:33 PM

  To: G, Zhenzhen \<Zhenzhen_G@Dell.com\>; Wang, Andy King \<Andy_King_Wang@dell.com\>; Samuel, Su \<Su_Samuel@Dell.com\>; Li, Jim \<Jim_Li@DELL.com\>

  Cc: Dong, Peter \<Peter_Dong@dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>; Ma1, Mark \<Mark_Ma1@Dell.com\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Zhenzhen, 为确保信息准确和在线创建单盘RAID 0参数正确，请用户先按照如下操作提供输出信息给我们。附件软件可拷贝到Linux系统下chmod +x后直接运行。

   

  1.  如已经确认只读故障硬盘槽位，请忽略此步骤。如未确定槽位，请使用"df以及 touch"命令确认问题硬盘的设备名，例如下图中可从Touch输出确定data07为只读，通过df输出确认对应data07的硬盘block设备名为 /dev/sdh

  ![[Technology_ALL_Linux 问题收集_027_Read-only_001.png]]

  1.  运行\# for i in ; do ls -F /sys/class/block/sd\$i/device/enclosure\*; done，将显示输出存档。
  2.  运行命令\# sas2ircu 0 display, 将显示输出存档。
  3.  运行 \# for i in ; do echo sd\$i; cat /sys/class/block/sd\$i/device/sas_address; done，将显示输出存档。

   

  Thanks and best regards!

  Jianguo Zhang / 张建国

  Systems Principle Engineer, ESI Engineering

  Dell EMC\|Extreme Scale Infrastructure (ESI)

  Desktop: +86-10-58261442

  Mobile: +86-18611610290

  [Jianguo.zhang@dell.com](mailto:Jianguo.zhang@dell.com)

   

  From: G, Zhenzhen

  Sent: Friday, July 14, 2017 3:25 PM

  To: Zhang, Jianguo \<<Jianguo_Zhang@Dell.com>\>; Wang, Andy King \<<Andy_King_Wang@dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>; Li, Jim \<<Jim_Li@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Ma1, Mark \<<Mark_Ma1@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Hi

  客户表示一定要做raid0 之后再挂载，客户提供的存储节点机器ST: 28RK242.麻烦帮忙确认一下，谢谢！

   

   

  Guo Zhenzhen

  Enterprise Engineer ,Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  [Zhenzhen_G@dell.com](mailto:Zhenzhen_G@dell.com)

   

  From: Zhang, Jianguo

  Sent: Wednesday, July 12, 2017 5:22 PM

  To: Wang, Andy King \<<Andy_King_Wang@dell.com>\>; G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>; Li, Jim \<<Jim_Li@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Ma1, Mark \<<Mark_Ma1@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Andy, 这个ST对应的是存储节点，需要提供连接到这个存储节点的计算节点的ST确认RAID controller配置。不过根据H3QK242存储节点的配置，电信出货应该是用于配合采用9R4NW RAID控制器的计算节点。稳妥起见，建议核查并核对所连计算节点的ST。

   

  如果是9R4NW RAID控制器的计算节点，新装3TB硬盘无需配置RAID 0，即可直接使用，而且按照电信云呼和浩特数据中心的配置标准，3TB硬盘也是不配置RAID 0的。

   

  Thanks and best regards!

  Jianguo Zhang / 张建国

  Systems Principle Engineer, ESI Engineering

  Dell EMC\|Extreme Scale Infrastructure (ESI)

  Desktop: +86-10-58261442

  Mobile: +86-18611610290

  [Jianguo.zhang@dell.com](mailto:Jianguo.zhang@dell.com)

   

  From: Wang, Andy King

  Sent: Wednesday, July 12, 2017 4:45 PM

  To: Zhang, Jianguo \<<Jianguo_Zhang@Dell.com>\>; G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>; Li, Jim \<<Jim_Li@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Ma1, Mark \<<Mark_Ma1@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Jianguo,

   

  能否通过ST H3QK242 查询到出厂配置？

   

  Andy

   

  From: Zhang, Jianguo

  Sent: 2017年7月12日 16:41

  To: Wang, Andy King \<<Andy_King_Wang@dell.com>\>; G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>; Li, Jim \<<Jim_Li@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Ma1, Mark \<<Mark_Ma1@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  lspci \|grep -i lsi

  把输出给我看一下

  用户现在是什么硬盘配置？硬盘容量，各是几块？

  不同RAID控制器，工具软件和命令不同。如果是DPN : 9R4NW的控制器，不需要配置单盘RAID 0。

   

  Thanks and best regards!

  Jianguo Zhang / 张建国

  Systems Principle Engineer, ESI Engineering

  Dell EMC\|Extreme Scale Infrastructure (ESI)

  Desktop: +86-10-58261442

  Mobile: +86-18611610290

  [Jianguo.zhang@dell.com](mailto:Jianguo.zhang@dell.com)

   

  From: Wang, Andy King

  Sent: Wednesday, July 12, 2017 4:18 PM

  To: G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Zhang, Jianguo \<<Jianguo_Zhang@Dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>; Li, Jim \<<Jim_Li@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Ma1, Mark \<<Mark_Ma1@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Loop jim

  From: Wang, Andy King

  Sent: 2017年7月12日 16:15

  To: G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Zhang, Jianguo \<<Jianguo_Zhang@Dell.com>\>; Samuel, Su \<<Su_Samuel@Dell.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Ma1, Mark \<<Mark_Ma1@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Jianguo，

   

  Could you help check does any way to configure raid through command or other way?

   

  B.R

  Andy

   

  From: Lin, Yongliang

  Sent: 2017年7月12日 14:36

  To: G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Wang, Andy King \<<Andy_King_Wang@dell.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; TS_APJ_Storage_CHK_Directs \<<TS_APJ_Storage_CHK_Directs@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Andy:

   

  Help follow up for DCS .

   

   

  Yongliang, Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

   

  From: Lin, Yongliang

  Sent: Wednesday, July 12, 2017 2:31 PM

  To: G, Zhenzhen \<<Zhenzhen_G@Dell.com>\>; Lai, Tony \<<Tony_Lai@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; TS_APJ_Storage_CHK_Directs \<<TS_APJ_Storage_CHK_Directs@Dell.com>\>

  Subject: RE: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

  Tony:

   

  Help on it

   

   

  Yongliang, Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

   

  From: G, Zhenzhen

  Sent: Wednesday, July 12, 2017 2:26 PM

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Dong, Peter \<<Peter_Dong@dell.com>\>

  Subject: 机型DCS8000\|Summary\|HD issue(PROS)\|SR:950183648

   

  Dell - Internal Use - Confidential 

   a.     Detail Symptom Descriptions

  详细的故障现象描述: 客户保修硬盘故障，更换硬盘之后，客户表示机器不能停机，需要在linux下创建单盘raid0

  故障的时间点 :

  是否可以复现故障 :

  如何复现故障 :

   

  b.    Troubleshooting Steps

  详细的诊断步骤:硬盘故障已解决，客户表示机器不能停机，需要在linux下创建单盘raid0

  维修记录: 

  Bios/Driver/FW及存储控制器相关FW版本:

   

  c.     Current status

  客户公司名称: 中国电信

   

  d.     Must Collect Logs

  已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

  常见日志类型参考(根据实际情况获取相应日志)：

  服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

  存储(参考)：MD/EQL/NAS/CML/DR/DL log;

   

   

   

  Guo Zhenzhen

  Enterprise Engineer ,Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  [Zhenzhen_G@dell.com](mailto:Zhenzhen_G@dell.com)

   

 

已使用 OneNote 创建。
