single-bit ECC 记录

2025年5月26日

17:12

 

 

==1==========

CentOS7.9

issue：出现了多次此A4内存错误的相同记录，而且都有修复。

operator：更换此内存。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]: Hardware error from APEI Generic Hardware Error Source: 4

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]: It has been corrected by h/w and requires no further action

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]: event severity: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  Error 0, type: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  fru_text: A4

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   section_type: memory error

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   error_status: 0x0000000000000400

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   physical_address: 0x0000000e9d227900

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   node: 1 card: 0 module: 0 rank: 0 bank: 3 device: 12 row: 58152 column: 232

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   error_type: 2, single-bit ECC

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   DIMM location: not present. DMI handle: 0x0000

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]: Hardware error from APEI Generic Hardware Error Source: 65534

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]: It has been corrected by h/w and requires no further action

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]: event severity: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  Error 0, type: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   section type: unknown, 330f1140-72a5-11df-9690-0002a5d5c51b

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  Error 1, type: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   section type: unknown, 330f1140-72a5-11df-9690-0002a5d5c51b

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  Error 2, type: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   section type: unknown, 330f1140-72a5-11df-9690-0002a5d5c51b

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  Error 3, type: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   section type: unknown, 330f1140-72a5-11df-9690-0002a5d5c51b

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:  Error 4, type: corrected

May 18 23:04:17 pd1emdb01b_64191 kernel: \[Hardware Error\]:   section type: unknown, 330f1140-72a5-11df-9690-0002a5d5c51b

May 18 23:04:17 pd1emdb01b_64191 kernel: EDAC MC1: 0 CE memory read error on CPU_SrcID#0_MC#1_Chan#0_DIMM#0 (channel:0 slot:0 page:0xe9d227 offset:0x900 grain:32 syndrome:0x0 -  err_code:0x0000:0x009f socket:0 imc:1 rank:0 bg:0 ba:1 row:0xe3a1 col:0x1e0)

May 18 23:04:17 pd1emdb01b_64191 kernel: soft_offline: 0xe9d227: invalidated

 

==2==========

 

已使用 OneNote 创建。
