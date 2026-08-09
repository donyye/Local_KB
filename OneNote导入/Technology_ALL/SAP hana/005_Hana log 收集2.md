Hana log 收集2

2018年1月12日

9:05

1. SUSE log收集，如果客户是有做HA，那两台机器的supportconfig日志都需要。

Collect SUSE OS log

hana33:\~ \# supportconfig -l

=============================================================================

                     Support Utilities - Supportconfig

                          Script Version: 3.0-70

                          Script Date: 2015 10 29

 

Detailed system information and logs are collected and organized in a

manner that helps reduce service request resolution times. Private system

information can be disclosed when using this tool. If this is a concern,

please prune private data from the log files. Several startup options

are available to exclude more sensitive information. Supportconfig data is

used only for diagnostic purposes and is considered confidential information.

See <http://www.novell.com/company/legal/privacy/>

=============================================================================

 

Gathering system information

  Data Directory:    /var/log/nts_hana33_170606_1158

 

  Basic Server Health Check\...                 Done

  RPM Database\...                              Done

  Basic Environment\...                         Done

\...\....

  System Logs\...                               Done

 

Creating Tar Ball

 

==\[ DONE \]===================================================================

  Log file tar ball: /var/log/nts_hana33_170606_1158.tbz

  Log file size:     14M

  Log file md5sum:   fe4da53d145ad3d734e1d5321ae65831

=============================================================================

 

 

 

2. hana log收集，具体方法如下：

 

Collect hana log in CMD

\[root@hana23 \~\]# su - hdbadm

hana23:/usr/sap/HDB/HDB00\> python exe/python_support/fullSystemInfoDump.py -h

Usage: fullSystemInfoDump.py \[options\]

 

Collects diagnosis information from the SAP HANA database

 

Options:

  \--version             show program\'s version number and exit

  -h, \--help            show this help message and exit

  -n, \--nosql           Excludes collection of system views

  -f FILE, \--file=FILE  Zips the specified file in the trace directory

  -d DAYS, \--days=DAYS  Collects trace files from the specified past number of

                        days

  -F FROMDATE, \--fromDate=FROMDATE

                        Collects trace files starting from the specified date

                        (format: YYYY-MM-DD)

  -T TODATE, \--toDate=TODATE

                        Collects trace files up to the specified date (format:

                        YYYY-MM-DD)

  -e EXPORTPATH, \--exportPath=EXPORTPATH

                        Path to exported tables/views

 

  Collects runtime environment (RTE) dump files:

    -r, \--rtedump       Collects RTE dump files

    \--indexservers=INDEXSERVERS

                        Specifies the index server(s) (comma separated) from

                        which RTE dump files are to be collected

    \--interval=INTERVAL

                        Specifies the interval (in minutes) at which RTE dump

                        files are to be collected; Possible values are

                        1,5,10,15,30.

    \--sets=SETS         Specifies the number of RTE dump file sets to be

                        collected; possible values are 1, 2, 3, 4, 5

\[OK\]

hana23:/usr/sap/HDB/HDB00\>

hana23:/usr/sap/HDB/HDB00\> python exe/python_support/fullSystemInfoDump.py -F 2017-05-15 -T 2017-05-24 

System Info Dump created 2017-01-20 06:02:44 (UTC) with script version 1.17

Called with command line options:

Writing to file /usr/sap/HDB/SYS/global/sapcontrol/snapshots/fullsysteminfodump_hana23_HDB_2017_01_20_06_02_44.zip

 

\-\-- Exporting trace files

exporting file /usr/sap/HDB/HDB00/../HDB00/hana23/trace/nsutil.crashdump.004344.trc \...  done.

exporting file /usr/sap/HDB/HDB00/../HDB00/hana23/trace/nsutil.crashdump.004366.trc \...  done.

exporting file /usr/sap/HDB/HDB00/../HDB00/hana23/trace/nsutil.crashdump.004217.trc \...  done.

exporting file /usr/sap/HDB/HDB00/../HDB00/hana23/trace/nameserver.crashdump.20170120-115359.029654.trc \...  done.

exporting file /usr/sap/HDB/HDB00/../HDB00/hana23/trace/nsutil.crashdump.004359.trc \...  done.

\...\...

\-\-- Exporting kerberos files

exporting file /etc/krb5.conf \... done.

\-\-- Exporting kerberos files done

 

System information written to file /usr/sap/HDB/SYS/global/sapcontrol/snapshots/fullsysteminfodump_hana23_HDB_2017_01_20_06_02_44.zip

Full System Dump done.

\[OK\]

 

已使用 OneNote 创建。
