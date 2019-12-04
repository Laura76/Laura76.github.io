---
layout: post
title:  "FrontEnd  Notes"
date:   2019-11-19
categories: CSS Html5 JS
---

> 前端实践经验记录  

#### 第一条：canvas标签的长宽高不可以在css文件中定义     
原因我也TM不知道，反正只能在标签中直接写，示例代码是这样的：  
```
<canvas id="myCanvas" width="1200" height="700"></canvas>
```
---
> JS记录

#### 第一条：在函数中定义的成员和在prototype里定义的成员的区别  
关键点在于在prototype里面定义的成员是被类的所有对象共享的。  
具体见https://www.cnblogs.com/douyage/p/8630529.html  
#### 第二条 void 0
if(某参数 == void 0)意思是该参数未定义
#### 第三条 类型化数组
ES6采纳了类型化数组，共有以下八种类型：  
8 位有符号整数（int8）  
8 位无符号整数（uint8）  
16 位有符号整数（int16）  
16 位无符号整数（uint16）  
32 位有符号整数（int32）  
32 位无符号整数（uint32）  
32 位浮点数（float32）  
64 位浮点数（float64）
#### 第四条 Infinity  
正无穷，-Infinity表示负无穷  
