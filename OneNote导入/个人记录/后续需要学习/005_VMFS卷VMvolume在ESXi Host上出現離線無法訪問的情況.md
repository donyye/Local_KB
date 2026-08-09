VMFS卷VMvolume在ESXi Host上出現離線無法訪問的情況

2015年1月27日

9:29

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       答复: 
    发件人     Ye, Dony
    收件人     Yin, Guoxun
    发送时间   2015年1月26日 14:55
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

   

  問題描述:

  VMFS卷VMvolume2 在1月20日在ESXi Host  10.1.1.195上出現離線無法訪問的情況。

   

   

  分析:

  VMvolume2卷的設備ID

  naa.60fff1ba2e13e9e6704715d3b5f8a855:1                           /vmfs/devices/disks/naa.60fff1ba2e13e9e6704715d3b5f8a855:1 5477126e-0cf0a088-d11e-c81f66ec342c  0  VMvolume2

   

  問題出現時間：

  UTC/ 10:43:58  20-10  2015

  2015-01-20T10:35:04.492Z cpu22:33670)WARNING: NMP: nmp_DeviceRequestFastDeviceProbe:237: NMP device \"naa.60fff1ba2e13e9e6704715d3b5f8a855\" state in doubt; requested fast path state update\...

   

  在此之前該卷曾在短時間出現lantency數值出現較大的變化

  2015-01-20T10:33:53.403Z cpu5:34435)WARNING: ScsiDeviceIO: 1223: Device naa.60fff1ba2e13e9e6704715d3b5f8a855 performance has deteriorated. I/O latency increased from average value of 3011 microseconds to 558476 microseconds.

  2015-01-20T10:33:53.403Z cpu7:32820)WARNING: ScsiDeviceIO: 1223: Device naa.60fff1ba2e13e9e6704715d3b5f8a855 performance has deteriorated. I/O latency increased from average value of 3011 microseconds to 1201245 microseconds.

  2015-01-20T10:34:21.023Z cpu8:33110)WARNING: ScsiDeviceIO: 1223: Device naa.60fff1ba2e13e9e6704715d3b5f8a855 performance has deteriorated. I/O latency increased from average value of 3012 microseconds to 2408671 microseconds.

  2015-01-20T10:34:29.363Z cpu14:782983)WARNING: ScsiDeviceIO: 1223: Device naa.60fff1ba2e13e9e6704715d3b5f8a855 performance has deteriorated. I/O latency increased from average value of 3013 microseconds to 968537 microseconds.

   

  Syslog顯示iscsi login 成功，但是session最終連接失敗

  2015-01-20T10:35:03Z iscsid: Kernel reported iSCSI connection 6:0 error (1011) state (3)

  2015-01-20T10:35:03Z iscsid: Kernel reported iSCSI connection 5:0 error (1011) state (3)

  2015-01-20T10:35:03Z iscsid: Kernel reported iSCSI connection 7:0 error (1011) state (3)

  2015-01-20T10:35:06Z iscsid: connection 8:0 (iqn.2001-05.com.equallogic:0-af1ff6-e6e9132eb-55a8f8b5d3154770-vmvolume2 if=iscsi_vmk@vmk2 addr=192.168.190.100:3260 (TPGT:1 ISID:0x2)  (T3 C1)) Nop-out timeout after 10 sec in state (3).

   

  檢查ISCSI network的uplink為vmnic4/7

  ======== esxcfg-vswitch -l  ========

  Switch Name      Num Ports   Used Ports  Configured Ports  MTU     Uplinks  

  vSwitch3         5632        7           128               9000    vmnic4,vmnic7

   

    PortGroup Name        VLAN ID  Used Ports  Uplinks  

    iSCSI_2               0        1           vmnic7   

    iSCSI_1               0        1           vmnic4  

   

  確認問題時間段內vmnic4和7未出現網路中斷，RX/TX/CRC不存在嚴重資料錯誤。

   

  ISCSI initiator 上DelayedAck功能未關閉

     \`node.conn\[0\].iscsi.DelayedAck\`=\'1\'

   

  Login Timeout未設置合理的超時值

    \`node.conn\[0\].timeo.login_timeout\`=\'5\'

   

   

  總結

  由於瞬間的鏈路延遲變動較大，導致初次I/O超時後鏈路一直處於抖動狀態無法恢復，檢查確認物理網路未出現中斷情況，埠連接未出現嚴重錯誤導致通訊故障。

  由於未提供具體的埠拓撲，我們只知道ISCSI Network工作在VLAN 190上，根據收集到的日誌顯示該ISCSI vlan網段的部分埠未開啟Port fast功能，未開啟Flow control功能。

  綜合所有日誌可以判斷非硬體故障，鏈路延時波動導致了path thrashing是本次lun訪問中斷的直接原因，請按照以下建議修改優化設置使之符合最佳設置，我們會就此問題提供後續支援和關注。

   

  1.  ISCSI Initiator 優化設置 。
      a.  關閉DelayedAck.   Command:    vmkiscsi-tool -W -a delayed_ack=0 -j vmhba39
      b.  修改ISCSI login timeout值到60s .  Command:  esxcli iscsi adapter param set -A vmhba39 -k LoginTimeout -v 60

   

  1.  在ISCSI network所涉及的埠上開啟流控和Portfast功能 。

   

  1.  提高MEM的debug日誌輸出級別，以備以後出現問題可以提供更多的日誌內容幫戶分析故障原因。
      a.  Ssh登錄到esxi主機，備份/etc/cim/dell/echmd.conf後使用Vi打開該檔，將其中的debuglevel欄位的數值設置為3後保存退出，重啟主機。

   

  1.  另外請注意儘量避免將所有disk I/O較多的 VM放置在同一個VMFS LUN上，將其平均分配到多個lun會有效的降低延遲和I/O衝突。

   

   

 

已使用 OneNote 创建。
