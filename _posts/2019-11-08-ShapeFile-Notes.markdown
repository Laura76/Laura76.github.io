---
layout: post
title:  "ShapeFile Notes"
date:   2019-11-08
categories: ShapeFile

---

> 解析shapefile文件的轮子

---
- DBF文件格式  **模拟数据库的数据库文件**    
- 分为**文件头**、**字段描述**、**数据内容**  
- 文件头区：**32字节**  
03（这是16进制的一个字节）文件类型  
77（119年）文件最后修改日期的年份为1900+119=2019  
0A （10月份）  
19 （19号）  
02 00 00 00：小字节（倒着读），表示是有两条记录  
41 00：文件头长度（文件头和字段描述两部分，加上一字节的结束符0D回车)64+1  
0B 00：（11=10+1）,表示一条记录的长度为10，即一条记录中所有字段的长度之和  
00 00...00：（20个保留字节）  
- 字段描述区：**65-32=33**（65是上面的文件头长度）  
<u>**字段一，每个字段的描述长度是32个字节**</u>  
4F 42...00:（10个字节）字段名 OBJECTID  
00:(一个系统保留的字节)可以理解为字符串结束符  
4E:字段类型，78表示ASCII码是N，即数字类型的字段  
00 00 00 00：（系统保留的四个字节）  
0A:(字段的长度是10)  
00:(小数的位数是0)  
00 00...00:系统保留的  
<u>**所有的字段结束以后，以一个字节的0D结束**</u>
- 数据内容区  
<u>**记录一，记录长度为1+10，由文件头区得知**</u>  
20：占位符  
20 20...36:字段一的内容，长度为10，由字段描述区得知，值为692（注意要删掉20占位符，留下的数值所对应的ASCII码才是真实的值，例如这里就是32 39 36小字节即为36 39 32对应的ASCII码为692  
<u>**记录二**</u>    
20：占位符
20 20...35:字段二的内容，值为554  
<u>**所有的记录结束以后，可能以一个字节的1A结束**</u>

---

- shx文件格式  **每个数据项都是定长的** **方便在shp文件中找到任意几何体的位置**
- 分为文件头区、八字节定长记录区  
文件头：100字节，和shp文件相同的文件头  
八字节定长记录区：  
<u>记录一</u>  
0-3字节：记录位移  大端序
4-7字节：记录长度

---

- shp文件格式  
- 分为 文件头区、变长记录数据区  
 **文件头**：100字节  
0-3字节：文件编号，永远是0x0000270a，大端序  
4-23：五个没有被使用的整数（4个字节一个整数），大端序  
24-27：文件长度  
28-31：版本  
32-35：图形类型，小端序  
36-67：最小外接矩形，包含四个浮点数即xmin,xmax,ymin,ymax  
68-83:z坐标的范围  
84-99：m坐标的范围  
 **变长记录区：**             
<u>**记录一**</u>  
记录头：八个字节，前四个是记录编号，后四个是记录长度  
实际的记录内容：  
0-3个字节：图形类型（支持的类型有：点、折线、多边形等）  
4-：图形内容
---
- 注意每一个点的z坐标和权重m值是可选的。
- point类  
一个点的xyz坐标和权重M
- multipoint类  
内含有许多个点，该实体类包含每一个点的xyz坐标和每一个点的权重m。范围上面包含所有的点xyz的最大最小值和m的最大最小值。  
- polyline类  
有很多的部分part组成，每一个part由很多的点组成。  
- polygon类  
和polyline类似
---
