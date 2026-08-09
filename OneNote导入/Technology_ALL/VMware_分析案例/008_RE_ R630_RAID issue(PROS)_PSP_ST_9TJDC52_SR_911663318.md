RE: R630\|RAID issue(PROS)\|PSP\|ST:9TJDC52\|SR:911663318

Monday, June 01, 2015

3:27 PM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------------
    主题       RE: R630\|RAID issue(PROS)\|PSP\|ST:9TJDC52\|SR:911663318
    发件人     Yin, Guoxun
    收件人     Lian, Wenxiang; Ding, Simon
    抄送       CN XMN TS ENT L2 SME
    发送时间   Monday, June 01, 2015 3:02 PM
    -------------------------------------- ---------------------------------------------------------------------------------------
  :::

   

  Simon

  该vSphere 产品并非OEM，不知道你是否做过检查并与相关sales确认，对此CASE我们只提供硬件范围内的支持。

   

  从收到的日志看，该ESXi存在一个名为test的RHEL64的VM，检查ESXi和该VM的日志发现该环境存在以下问题:

  1.  ESXi需要必须使用U2版本，目前为U1
  2.  该VM hardware 版本不正确，config.version = \"8\" ，至少设置为10

  [\[root@RHEL70 9TJDC52\]# grep -ir \'config.version\' .]

  ./vmfs/volumes/55688703-4a5ce41c-0438-44a8420c547a/test/test.vmx:config.version = \"8\"

  ./vmfs/volumes/55688703-4a5ce41c-0438-44a8420c547a/test/vmware-1.log:2015-05-29T16:15:27.363Z\| vmx\| I120: DICT[            config.version = 8]

  ./vmfs/volumes/55688703-4a5ce41c-0438-44a8420c547a/test/vmware.log:2015-05-29T16:52:33.269Z\| vmx\| I120: DICT[            config.version = 8]

  ./vmfs/volumes/55688703-4a5ce41c-0438-44a8420c547a/新建虚拟曟vmware.log:2015-06-01T09:16:14.157Z\| vmx\| I120: DICT[            config.version = 8]

  ./vmfs/volumes/55688703-4a5ce41c-0438-44a8420c547a/新建虚拟曟新建虚拟曟vmx:config.version = \"8\"

   

  1.  该VM挂载ISO的存放位置严重错误，该文件夹为系统保留作用，严禁在其中认为放置任何文件，建议不要使用中文文件和文件夹名字，尤其不要包含空格

  ide1:0.fileName = \"/vmfs/volumes/55688703-4a5ce41c-0438-44a8420c547a/.sdd.sf/新建 文件夹/rhel-server-6.4-x86_64-dvd.iso\"

   

  1.  该机器开启了CPUID Mask，请用户确认开启mask的是否有必要，如果在非异构成员群集中，建议关闭.

  "CPUID Mask"隐藏NX/XD标记将增加主机之间的vmotion兼容性，代价是机器或是OS应用程序而言禁用了某些CPU功能。

  ![[Technology_ALL_VMware_分析案例_008_RE_ R630_RAID issue(PROS)_PSP_ST_9TJDC52_001.png]]

  [http://xjsunjie.blog.51cto.com/999372/1602690](http://xjsunjie.blog.51cto.com/999372/1602690)[  详细解释]

  搜索关键字：grep -ir \'CPUIDMask\' .

   

  1.  该机未安装vmware-tools，该工具包必须安装

   

  以下为抓来的图片显示的linux VM的问题

  1.  文件系统再次挂载的时候发现时间有跳变，请用户注意检查时间和时区是否设置合理。 另外时间同步可以选择跟ESXi同步，也可以选择由linux系统同步其他时钟源，但是只能二选一。
  2.  该linux文件系统存在不一致的情况，请用户确认是否之前没有正常关机，我们推荐从系统中以"graceful"的方式关机，在没有安装vm-tools的情况下直接点红色按钮关机对linux VM是致命的。

   

   

  综合以上所有情况，建议:

  1.  收集TTY确认storage subsystem 工作正常。
  2.  在升级ESXi至U2后，以"typical"模式创建一个标准的hardware版本10的VM，安装6.4测试安装是否成功，之后安装VMware-tools后重启，确保Tool工作正常再部署Oracle database.

   

   

   

  From: Lian, Wenxiang

  Sent: 2015年6月1日 12:32

  To: Ding, Simon; Yin, Guoxun

  Cc: CN XMN TS ENT L2 SME

  Subject: RE: R630\|RAID issue(PROS)\|PSP\|ST:9TJDC52\|SR:911663318

   

  Dell - Internal Use - Confidential 

  Guoxun,

   

  Please help look into this case.

   

   

  Thanks & Regards,

   

  Wenxiang Lian

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  From: Ding, Simon

  Sent: Monday, June 01, 2015 12:11

  To: CN XMN TS Server Escalation

  Cc: Ding, Simon

  Subject: R630\|RAID issue(PROS)\|PSP\|ST:9TJDC52\|SR:911663318

   

  Dell - Internal Use - Confidential 

   

  Detail Symptom Descriptions

  1,安装的esxi 5.5 系统(DELL ISO版本),里面部署linux虚拟机,有些虚拟机无法安装成功,有些可以安装成功,安装RH6.4的时候会报错:unsupported hardware detectd.

  2,如果安装RH6.2虚拟机,安装的过程报错:

  /dev/mapper/vg_pdmqas1-1v_root:superblock last mount time is in the future.

  (bu less than a day,probably due to the hardware closck being incorrectly set \\)fixed

  /dev/mapper/vg_pdmqas1-1v_root contains a file system with errors,check forced

  /dev/mapper/vg_pdmqas1-1v_root:

  HTREE directory inode 925324 has an invalid root node

  /dev/mapper/vg_pdmqas1-1v_root:UNEXPECTED INCONSISTENCY;RUN fsckManually. failed

  an error occurred during the file system check

  dropping you to a ahell;the system will reboot

  disableing security enforcement for system recovery

  run setenforce 1 to reenable

  3,有些时候又可以安装成功,但安装成功的虚拟机里面安装Orcal数据库无法成功,报错:

  CREATE UNIQUE INDEX \"PDM7\".\"WTPARTUSAGELINK\$UNIQ1\" ON \"PDM7\".\"WTPARTUSAGELINK\" (\"IDA3A5\", NVL(\"COMPONENTID\",TO_CHAR(\"IDA2A2\"))) PCTFREE 10 INITRANS 2 MAXTRANS 255  STORAGE(INITIAL 1048576 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645 PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT) TABLESPACE \"INDX\" PARALLEL 1

  ORA-39083: Object type INDEX failed to create with error:

  ORA-01578: ORACLE data block corrupted (file \# 15, block \# 1798150)

  ORA-01110: data file 15: \'/oracle/ocu/oradata/wind/windusers03.dbf\'

  Failing sql is:

  CREATE INDEX \"PDM7\".\"WFPROCESS\$PTC1\" ON \"PDM7\".\"WFPROCESS\" (UPPER(\"NAME\")) PCTFREE 10 INITRANS 2 MAXTRANS 255  STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645 PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT) TABLESPACE

  Job \"SYSTEM\".\"SYS_IMPORT_SCHEMA_01\" completed with 161 error(s) at Tue May 26 20:25:17 2015 elapsed 0 02:18:25

  Troubleshooting Steps

  4,客户重新配置RAID,重新安装esxi5.5然后配置虚拟机,一样的情况.

  5,客户刚开始说有硬盘的block,所以有PSA测试过所有硬件,硬盘PASS.

  6,有三台R630服务器都是类似的情况,DSET LOG没有发现硬件直接的报错(没有收集到tty log)

  Current status

  6,客户表示项目比较着急,需要尽快处理,项目已经拖住了.sales需要帮忙尽快处理, 建议客户收集vm-support日志,升级L2协助处理.

  Must Collect Logs: dset log /vm-support log

  日志存放路径:

  [\\\\XMNTSDB03\\EntTS_Log\\storage\\Simon_ding\\9TJDC52](file://XMNTSDB03/EntTS_Log/storage/Simon_ding/9TJDC52)

 

已使用 OneNote 创建。
