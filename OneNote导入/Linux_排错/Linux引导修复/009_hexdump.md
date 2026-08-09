hexdump

2023年4月10日

18:02

 

使用 hexdump 命令查看 sda 都是0，没有任何的数据，但是其它的sdb是有的。

![[Linux引导修复_009_hexdump_001.png]]

 

使用 hexdump -C -n 409600 -s \$((409600\*2)) /dev/sda 查看sda后半段是有数据的

![[Linux引导修复_009_hexdump_002.png]]

 

![[Linux引导修复_009_hexdump_003.png]]

 

 

 

![[Linux引导修复_009_hexdump_004.png]]

 

![[Linux引导修复_009_hexdump_005.png]]

 

00100400

1045K

 

![[Linux引导修复_009_hexdump_006.png]]

 

![[Linux引导修复_009_hexdump_007.png]]

 

 

 

 

已使用 OneNote 创建。
