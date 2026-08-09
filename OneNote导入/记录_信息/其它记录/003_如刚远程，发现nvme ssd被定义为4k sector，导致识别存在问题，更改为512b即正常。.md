2023年4月10日

13:59

如刚远程，发现nvme ssd被定义为4k sector，导致识别存在问题，更改为512b即正常。

 

 

通过命令查看当前nvme device：

\[root@localhost:/dev/disks\] esxcli nvme device list

HBA Name  Status  Signature

\-\-\-\-\-\-\--  \-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

vmhba0    Online  nvmeMgmt-nvme001060000

vmhba3    Online  nvmeMgmt-nvme001070000

vmhba4    Online  nvmeMgmt-nvme001080000

vmhba5    Online  nvmeMgmt-nvme001870000

vmhba6    Online  nvmeMgmt-nvme001880000

vmhba7    Online  nvmeMgmt-nvme001890000

 

 

通过命令可查看对应的vmhba的LBA format情况

 

 

\[root@localhost:/dev/disks\] esxcli nvme device namespace get -A vmhba0 -n 1

Namespace Identify Info:

   Namespace Size: 0x3a3817d6 Logical Blocks

   Namespace Capacity: 0x3a3817d6 Logical Blocks

   Namespace Utilization: 0x3a3817d6 Logical Blocks

   Thin Provisioning Support: false

   Namespace Atomic Support: false

   Deallocated or Unwritten Logical Block Error Support: false

   Number of LBA Formats: 1

   LBA Format: 1

   Extended Metadata: false

   Metadata as Seperate Buffer Support: false

   Metadata as Extended Buffer Support: false

   PI Type 1 Support: false

   PI Type 2 Support: false

   PI Type 3 Support: false

   PI in First Eight Bytes of Metadata Support: false

   PI in Last Eight Bytes of Metadata Support: false

   PI Enabled Type: 0

   MetaData Location: PI Disabled

   Namespace Shared by Multiple Controllers: false

   Persist Through Power Loss Support: false

   Write Exclusive Reservation Type Support: false

   Exclusive Access Reservation Type Support: false

   Write Exclusive Registrants Only Reservation Type Support: false

   Exclusive Access Registrants Only Reservation Type Support: false

   Write Exclusive All Registrants Reservation Type Support: false

   Exclusive Access All Registrants Reservation Type Support: false

   Format Progress Indicator Support: false

   Percentage Remains to Be Formatted: 0 %

   Namespace Atomic Write Unit Normal: 0

   Namespace Atomic Write Unit Power Fail: 0

   Namespace Atomic Compare and Write Unit: 0

   Namespace Atomic Boundary Size Normal: 0

   Namespace Atomic Boundary Offset: 0

   Namespace Atomic Boundary Size Power Fail: 0

   NVM Capacity: 0x3a3817d6000

   Namespace Globally Unique Identifier: 01000000010000005cd2e44d08115351

   IEEE Extended Unique Identifier: 5cd2e44d08110100

   LBA Format Support:

         Format ID: 0

         LBAData Size: 512

         Metadata Size: 0

         Relative Performance: Good performance

      

         Format ID: 1

         LBAData Size: 4096

         Metadata Size: 0

         Relative Performance: Best performance

 

通过如下命令更改对应vmhba的LBA format为512即可：

 

\[root@localhost:/dev/disks\] esxcli nvme device namespace format -A vmhba0 -f 0 -n 1 -m 0 -p 0 -l 0 -s 0

Format successfully!

 

 

==============

1.关于去除nobarries以下KB有说法

<https://www.suse.com/c/xfs-nobarrier-option-is-now-more-than-deprecated/>

 

2. 关于firmware bug errors信息，Dell已经给出了解释的信息，但是不一定所有OS对此都有需要解释的说法。

 

3. 关于kernel.shmmni的值应该如何设置的问题

SST 可能需要帮忙补充一下。

 

已使用 OneNote 创建。
