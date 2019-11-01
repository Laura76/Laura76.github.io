---
layout: post
title:  "CTest Notes"
date:   2019-11-01
categories: Java SpringBoot

---

### 注意：请在TMS项目中查询文中提到的代码
> 1.导入Excel，查询数据，显示数据 

- 导入Excel：  

---

首先 **数据库连接**方面的参数在**application.properties**文件中设置,如下所示：  
```
spring.datasource.url =jdbc:oracle:thin:@IP:PORT:SERVERNAME  
spring.datasource.username = USERNAME
spring.datasource.password = USERPASSWORD
```
其次**创建数据库中的表格**，请在**navicat**（oracle可视化软件）中打开查询窗口，通过书写的sql语句来创建表格。如下所示：——应准备好常用的sql语句。
```
create table TEST_TAB_*
 (
  mid    VARCHAR2(32) default sys_guid(),
  bmname VARCHAR2(30),
  mname  VARCHAR2(30),
  mxb    NUMBER,
  msr    DATE,
  jg     VARCHAR2(30)
)
```
至此，数据库的准备工作结束了。开始进入正题......  

---

下面从**前端界面**开始，采用**layui**的前端框架。请从**templates**文件夹中的众多文件中寻找合适的轮子。

 
 
