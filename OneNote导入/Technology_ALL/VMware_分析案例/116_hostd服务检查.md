hostd服务检查

2020年7月1日

10:30

R740\|VMware ISSUE\|PSP\|ST:2YDHD43\|case#68606202\|

 

 

Host-167438是cnsu01vs001的runtime name，日志中可以看到每次host sync的时间耗费量都非常高，正常一般不超过10000ms

【vclog\|var/log/vmware/vpxd/vpxd-205.log】

2020-06-26T05:18:10.187Z warning vpxd\[28303\] \[Originator@6876 sub=VpxProfiler opID=QS-host-167438-29822c54\] DoHostSync:host-167438 \[GetChangesTime\] took 1621628 ms

2020-06-26T05:18:10.187Z warning vpxd\[28303\] \[Originator@6876 sub=VpxProfiler opID=QS-host-167438-29822c54\] DoHostSync:host-167438 \[DoHostSyncTime\] took 1621628 ms

2020-06-26T05:18:10.189Z warning vpxd\[28303\] \[Originator@6876 sub=MoHost opID=QS-host-167438-29822c54\] host \[vim.HostSystem:host-167438,cnsu01vs001.onsemi.com\] connection state changed to NO_RESPONSE

2020-06-26T05:18:10.197Z warning vpxd\[28303\] \[Originator@6876 sub=VpxProfiler opID=QS-host-167438-29822c54\] InvtHostSyncLRO::StartWork \[HostSyncTime\] took 1621638 ms

2020-06-26T05:18:10.197Z warning vpxd\[28303\] \[Originator@6876 sub=VpxProfiler opID=QS-host-167438-29822c54\] VpxLro::LroMain \[TotalTime\] took 1621638 ms

 

检查vpxa log则发现大量的operation timed out记录，

【没找到】

2020-06-26T05:18:33.401Z error vpxa\[2100448\] \[Originator@6876 sub=vpxaVmomi\] \[VpxaClientAdapter::InvokeCommon\] Got exception while invoking refresh on vim.Datastore:5ee09751-87f3c36e-bb34-bc97e150f532: \'N7Vmacore16TimeoutExceptionE(Operation timed out)

2020-06-26T05:18:33.404Z warning vpxa\[2100448\] \[Originator@6876 sub=VpxProfiler\] InvokeWithOpId \[TotalTime\] took 1800006 ms

2020-06-26T05:18:56.509Z info vpxa\[2100441\] \[Originator@6876 sub=vpxLro opID=PollQuickStatsLoop-68ecc8dd-7\] \[VpxLRO\] \-- BEGIN lro-21686 \-- vpxa \-- vpxapi.VpxaService.fetchQuickStats \-- 52d94cca-4081-7990-4b3f-e0eb48a855fb

2020-06-26T05:18:56.509Z info vpxa\[2100441\] \[Originator@6876 sub=vpxaMoService opID=PollQuickStatsLoop-68ecc8dd-7\] Fetched null quickStats

2020-06-26T05:18:56.509Z info vpxa\[2100441\] \[Originator@6876 sub=vpxLro opID=PollQuickStatsLoop-68ecc8dd-7\] \[VpxLRO\] \-- FINISH lro-21686

2020-06-26T05:18:58.330Z error vpxa\[2100450\] \[Originator@6876 sub=vpxaVmomi opID=QS-host-167438-29822c54-b1\] \[VpxaClientAdapter::InvokeCommon\] Got exception while invoking GetPerfCounter on vim.PerformanceManager:ha-perfmgr: \'N7Vmacore16TimeoutExceptionE(Operation timed out)

 

 

 

Hostd进程因此经常被判断为无响应状态

【ESXI\|/var/run/log/vmkwarning.log】

2020-06-29T01:10:00.381Z cpu50:2151680)ALERT: hostd detected to be non-responsive

2020-06-29T00:35:00.801Z cpu30:2151625)ALERT: hostd detected to be non-responsive

2020-06-29T00:00:01.200Z cpu27:2151523)ALERT: hostd detected to be non-responsive

2020-06-28T23:30:00.699Z cpu15:2151433)ALERT: hostd detected to be non-responsive

2020-06-28T22:25:00.589Z cpu16:2151295)ALERT: hostd detected to be non-responsive

2020-06-28T22:55:01.102Z cpu68:2151378)ALERT: hostd detected to be non-responsive

2020-06-28T21:50:01.004Z cpu53:2151236)ALERT: hostd detected to be non-responsive

2020-06-28T21:20:00.501Z cpu48:2151142)ALERT: hostd detected to be non-responsive

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

自己补充【vclog \|/commands/journalctl\_-b\--0.txt.FRAG-00184】

Jun 28 10:39:43 apvcenter.ad.onsemi.com vpxd\[55407\]: Event \[20514472\] \[1-1\] \[2020-06-28T10:36:24.415638Z\] \[vim.event.GeneralHostWarningEvent\] \[warning\] \[\] \[China\] \[20514472\] \[Issue detected on cnsu01vs001.onsemi.com in China: hostd detected to be non-responsive

Jun 28 10:39:43 apvcenter.ad.onsemi.com vpxd\[55407\]: Event \[20514473\] \[1-1\] \[2020-06-28T10:36:24.416764Z\] \[vim.event.GeneralHostWarningEvent\] \[warning\] \[\] \[China\] \[20514473\] \[Issue detected on cnsu01vs001.onsemi.com in China: hostd detected to be non-responsive

Jun 28 10:39:44 apvcenter.ad.onsemi.com vpxd\[55407\]: Event \[20514475\] \[1-1\] \[2020-06-28T10:39:44.746283Z\] \[vim.event.EventEx\] \[error\] \[\] \[China\] \[20514475\] \[Alarm \'Host error\' on cnsu01vs001.onsemi.com triggered by event 20514469 \'Issue detected on cnsu01vs001.onsemi.com in China: hostd detected to be non-responsive

Jun 28 10:39:44 apvcenter.ad.onsemi.com vpxd\[55407\]: Event \[20514477\] \[1-1\] \[2020-06-28T10:39:44.746414Z\] \[vim.event.EventEx\] \[error\] \[\] \[China\] \[20514477\] \Alarm \'Host error\' on cnsu01vs001.onsemi.com triggered by event 20514470 \'Issue detected on cnsu01vs001.onsemi.com in China: hostd detected to be non-responsive

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

[但是Hostd并没有真正死掉，是因为state updating的开销太高导致它超时，

【ESXI \| vm-support \| /var/run/log/hostd.log】

2020-06-21T18:26:45.219Z warning hostd\[2099995\] \[Originator@6876 sub=IoTracker\] In thread 2099951, access(\"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/cnsu01ls2005/vmx-cnsu01ls2005-3302048296-1.vswp\") took over 2850 sec.

2020-06-21T18:26:45.219Z warning hostd\[2099995\] \[Originator@6876 sub=IoTracker\] In thread 2194343, access(\"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/cnsu01ls2005/vmx-cnsu01ls2005-3302048296-1.vswp\") took over 2800 sec.

2020-06-21T18:26:45.219Z warning hostd\[2099995\] \[Originator@6876 sub=IoTracker\] In thread 2099952, access(\"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/CNSU01WS5003/CNSU01WS5003-b6b0e974.vswp\") took over 2950 sec.

2020-06-21T18:26:45.219Z warning hostd\[2099995\] \[Originator@6876 sub=IoTracker\] In thread 2100530, stat(\"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/CNSU01WS5004/CNSU01WS5004_1.vmdk\") took over 2900 sec.

2020-06-21T19:25:15.633Z warning hostd\[2099951\] \[Originator@6876 sub=IoTracker\] In thread 2100530, access(\"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/CNSU01WS5004/vmx-CNSU01WS5004-3065047413-1.vswp\") took over 310 sec.

2020-06-21T19:25:15.633Z warning hostd\[2099951\] \[Originator@6876 sub=IoTracker\] In thread 2099995, stat(\"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/cnsu01ls2006/cnsu01ls2006.vmx\") took over 410 sec.

 

2020-06-26T06:22:54.501Z info hostd\[2099996\] \[Originator@6876 sub=vm\] DictionaryLoad: Cannot open file \"/vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/cnsu01ls2007/cnsu01ls2007.vmx\": Connection timed out.

2020-06-26T06:22:54.504Z info hostd\[2099996\] \[Originator@6876 sub=vm\] VigorOffline_GenSecPolicy: retry reading /vmfs/volumes/5ee09751-87f3c36e-bb34-bc97e150f532/cnsu01ls2007/cnsu01ls2007.vmx

 

 

然后我们在日志中较多的如下错误记录， H:0x7 的意义为"device has been reset due to initiator error" 由于启动器错误，设备已被重置

【ESXI\|/var/run/log/vmkernel.log】

2020-06-29T00:34:55.357Z cpu5:2098330)ScsiDeviceIO: 3449: Cmd(0x459bc1c67e00) 0x28, CmdSN 0x4dc1a2 from world 0 to dev \"naa.600a09803831353173244d7069347a6a\" failed H:0x7 D:0x0 P:0x0 Invalid sense data: 0xfb 0x7f 0x41.

2020-06-29T00:34:55.783Z cpu5:2098330)ScsiDeviceIO: 3449: Cmd(0x459bc1c67e00) 0x28, CmdSN 0x4dc1a3 from world 0 to dev \"naa.600a09803831353173244d7069347a6a\" failed H:0x7 D:0x0 P:0x0 Invalid sense data: 0xcf 0x2 0x43.

 

 

同时还可以看到第二个FC HBA也就是vmhba4上一直存在异常如下，Mid-layer underflow的意思为FC transport & link上存在异常，我们无法确认这个问题来自于访问Netapp设备还是3Par设备，如果来自于访问Netapp设备，这个范围包括FC HBA Firmware/Driver & adapter/SFP, Cables, SwitchPort/SFP,Storage port/SFP.

【ESXI\|/var/run/log/vmkernel.log】

2020-06-29T00:35:46.307Z cpu3:2103179)qlnativefc: vmhba4(86:0.0): (6:137): Mid-layer underflow detected (8 of 512 bytes).

2020-06-29T00:35:46.312Z cpu3:2103179)qlnativefc: vmhba4(86:0.0): (6:137): Mid-layer underflow detected (8 of 512 bytes).

 

 

综合目前以上看到的情况，我们初步认为问题并不在ESXi上，建议先解决FC链路的异常和确保存储访问性能正常后，我们观察问题是否得以解决，对此个人的建议如下供参考，另外也建议跟存储厂商支持获取相关细节内容。

1.        需要检查并修复vmhba4上的链路异常问题，涉及必要固件驱动更新，以及必要的SFP,cable,switch,storage port的检查和替换测试等

2.        按照Netapp 存储的要求来调整FC HBA的固件和驱动，

3.        请考虑Netapp对于ESXi的best practice手册来进行存储的连接和部署，厂商一般都有自己存储对应的best practice guide，按照要求部署是保证稳定和性能必备的条件。

4.        请确认FC Switch上划分的是否为Single initiator zone，除非厂商存储特定要求，不然请务必不要把多个initiztor划分到同一个Zone里面。

5.        我们看到该host有识别到两个size是0的Lun id是256的3Par lun，不知道这个是不是对应meta lun，除此以外没有看到其他3Par lun，如果确定该主机不需要挂载3Par，建议移除对该主机的storage mapping来排除影响。

 

=====================

另外一个case ST: 5FK4PK2 \| SR: 70131679 \| 达飞轮船虚拟化环境问题

 

Vpxd.log

2020-07-28T09:44:25.516Z warning vpxd\[04154\] \[Originator@6876 sub=VpxProfiler opID=QS-host-24-77e0da59\] DoHostSync:host-24 \[GetChangesTime\] took 1800006 ms

2020-07-28T09:44:25.516Z warning vpxd\[04154\] \[Originator@6876 sub=VpxProfiler opID=QS-host-24-77e0da59\] DoHostSync:host-24 \[DoHostSyncTime\] took 1800006 ms

2020-07-28T09:44:25.516Z warning vpxd\[04154\] \[Originator@6876 sub=InvtHostCnx opID=QS-host-24-77e0da59\] Exception occurred during host sync; Host communication failed; \[vim.HostSystem:host-24,cnshgccw-svs003.sc.cma-cgm.com\], e: N5Vmomi5Fault17HostCommunication9ExceptionE(Fault cause: vmodl.fault.HostCommunication

 

=========èExplain: vpxd与host cnshgccw-svs003 同步状态信息失败,在记录中未见vmnic0/1 uplink异常，所以初步怀疑esxi agent遇到了问题。

 

Vpxa.0.log

2020-07-28T09:42:56.175Z error vpxa\[11148964\] \[Originator@6876 sub=vpxaVmomi\] \[VpxaClientAdapter::InvokeCommon\] Got exception while invoking queryStats on vim.PerformanceManager:ha-perfmgr: \'N7Vmacore16TimeoutExceptionE

(Operation timed out)

\--\> \[context\]zKq7AVICAgAAAEpT5wAVdnB4YQAALE42bGlidm1hY29yZS5zbwAAsL4bAL6dFwCcqR8B04YRbGlidm1vbWkuc28AAWeNEQIuQyd2cHhhAAKIRScBRbATgzg/CgFsaWJ2aW0tdHlwZXMuc28AAiFOMQJlUDECwFUxAtKWNQJ+lzUAB6kwALXlKACT6SgAq8Q2BDt9AGxpYnB0aHJlYWQuc28uMAAFfZ8O

bGliYy5zby42AA==\[/context\]\'

2020-07-28T09:42:56.177Z error vpxa\[11148964\] \[Originator@6876 sub=hostdstats\] Failed to fetch current stats. Fault: N5Vmomi5Fault17HostCommunication9ExceptionE(Fault cause: vmodl.fault.HostCommunication

\--\> )

\--\> \[context\]zKq7AVICAgAAAEpT5wATdnB4YQAALE42bGlidm1hY29yZS5zbwAAsL4bAJCcFwHBiCV2cHhhAAG2RCcBiEUnAkWwE2xpYnZtb21pLnNvAIM4PwoBbGlidmltLXR5cGVzLnNvAAEhTjEBZVAxAcBVMQHSljUBfpc1AAepMAC15SgAk+koAKvENgQ7fQBsaWJwdGhyZWFkLnNvLjAABX2fDmxpYmMuc2

8uNgA=\[/context\]

2020-07-28T09:42:56.179Z warning vpxa\[11148964\] \[Originator@6876 sub=VpxProfiler\] InvokeWithOpId \[TotalTime\] took 1800007 ms

2020-07-28T09:43:13.714Z error vpxa\[11148954\] \[Originator@6876 sub=vpxaVmomi opID=HB-SpecSync-host-24@1885851-31596745-f4\] \[VpxaClientAdapter::InvokeCommon\] Got exception while invoking queryView on vim.option.OptionManager:ha-adv-options: \'N7Vmacore16TimeoutExceptionE(Operation timed out)

\--\> \[context\]zKq7AVICAgAAAEpT5wAYdnB4YQAALE42bGlidm1hY29yZS5zbwAAsL4bAL6dFwCcqR8B04YRbGlidm1vbWkuc28AAWeNEQIuQyd2cHhhAAKIRScBRbATg/AnPQFsaWJ2aW0tdHlwZXMuc28AAvjAJQLPdiwCNHcsBOXDCWxpYnZweGFwaS10eXBlcy5zbwAC7t43AsE9JwL46DYCBvc2AqJMNwC

15SgAk+koAKvENgU7fQBsaWJwdGhyZWFkLnNvLjAABn2fDmxpYmMuc28uNgA=\[/context\]\'

 

=========èExplain: 从vpxa agent记录看，vpxa无法获取主机信息,这部分工作是跟hostd service协同完成，当前怀疑hostd状态异常

 

 

Hostd.log

2020-07-28T09:11:46.464Z info hostd\[11148975\] \[Originator@6876 sub=Default opID=sps-Main-843896-42-2c-c5-c86f user=vpxuser:VSPHERE.LOCAL\\vpxd-extension-5f673928-9ddf-465f-864a-abeebe289302\] AdapterServer caught exception: N3Vim5Fault8NotFound9ExceptionE(Fault cause: vim.fault.NotFound

\--\> )

\--\> \[context\]zKq7AVICAgAAAEpT5wAPaG9zdGQAACxONmxpYnZtYWNvcmUuc28AALC+GwCQnBcBgyJjaG9zdGQAAaB2agGuD8MBq5fIgh84SgFsaWJ2aW0tdHlwZXMuc28AAUXIwgE90cIAteUoAJPpKACrxDYDO30AbGlicHRocmVhZC5zby4wAAR9nw5saWJjLnNvLjYA\[/context\]

2020-07-28T09:11:46.467Z info hostd\[11148975\] \[Originator@6876 sub=Vimsvc.TaskManager opID=sps-Main-843896-42-2c-c5-c86f user=vpxuser:VSPHERE.LOCAL\\vpxd-extension-5f673928-9ddf-465f-864a-abeebe289302\] Task Completed : haTask\--vim.vslm.host.CatalogSyncManager.queryCatalogChange-945138899 Status error

2020-07-29T00:40:15.740Z - time the service was last started, Section for VMware ESX, pid=11927129, version=6.7.0, build=15160138, option=Release

 

=========èExplain: 检查hostd log发现hostd从UTC时间7/28 9:11也就是北京时间17:11开始没有任何日志记录，怀疑从该时间左右开始进入无响应状态 

 

Host probing failed:

2020-07-28T09:45:01.130Z warning hostd-probe\[11857730\] \[Originator@6876 sub=Default\] Timeout: N7Vmacore16TimeoutExceptionE(Operation timed out)

\--\> \[context\]zKq7AVICAgAAAEpT5wAMaG9zdGQtcHJvYmUAACxONmxpYnZtYWNvcmUuc28AALC+GwC+nRcAnKkfAdOGEWxpYnZtb21pLnNvAAFnjREBRbATgrY5DgFsaWJ2aW0tdHlwZXMuc28AA45PAGhvc3RkLXByb2JlAAPwQgAEfRkCbGliYy5zby42AANxRgA=\[/context\]

2020-07-28T09:45:01.133Z error hostd-probe\[11857730\] \[Originator@6876 sub=Default\] hostd detected to be non-responsive

 

=========èExplain: hostd probing routing task 也显示hostd确实无响应，也即是进入了hang up 状态 

 

【日志总结】

1、ESXi 上的vpxa 与hostd process协同工作向vCenter report heartbeat, 从而vCenter在UI上显示与主机的连接情况。从当前日志记录看vpxa未见明确异常，但是hostd曾经进入hang up状态，这应该是从vCenter中看到ESXi  host失去连接的主要原因。

2、目前未见当前ESXi版本有明确的公开已知KB与该问题相符合。

3、根据日志中的记录未能判断hostd hang-up的触发原因。在此情况下需要问题复现的时候，捕获hostd process dump来进行调试分析，该部分工作因为代码和调试工具/方式都不公开，只能由VMware原厂支持来进行。需要向原厂寻求支持。

 

 

 

已使用 OneNote 创建。
