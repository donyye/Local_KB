Lab1 - Brocade & Zoning

Monday, November 18, 2013

Brocade Web Tools

Summary:

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_001.png]]

Switch Admin:

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_002.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_003.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_004.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_005.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_006.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_007.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_008.png]]

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_009.png]]

 

Name Server:

 

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_010.png]]

 

Performance Monitor:

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_011.png]]

 

Admin Domain

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_012.png]]

 

Port Administration:

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_013.png]]

 

Zone Administration:

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_014.png]]

 

How to create Zoning:

1.为设备Port创建别名

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_015.png]]

 

2.为别名指定对应的设备Port

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_016.png]]

 

3.创建Zone名为Zone_MKT

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_017.png]]

 

4.为Zone_MKT指定刚刚创建的别名端口

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_018.png]]

 

5.创建config配置文件MKT_ENG

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_019.png]]

 

6.将创建好的Zone_MKT和Zone_ENG放到配置文件MTK_ENG里

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_020.png]]

 

7.点击Save Config

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_021.png]]

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_022.png]]

 

8.点击Enable Config (会造成交换机短暂性2-3s的中断)

![[Technology_ALL_Storage_008_Lab1 - Brocade & Zoning_023.png]]

 

已使用 OneNote 创建。
