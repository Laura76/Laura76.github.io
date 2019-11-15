---
layout: post
title:  "Oracle Notes"
date:   2019-11-15
categories: Oracle

---

> Oracle的学习记录

---
- sql语句,默认表的名字是suwan  
truncate table suwan;(删除表中的所有数据，并不是删除表)  
drop table suwan; (删除表的结构)  
create table suwan(
    name varchar(20)
);(创建表)  
- 配置  
在Navicat软件中，新建Oracle连接：连接名随意定义如我定义了一个suwan,连接类型是basic表示是我本地的Oracle。（考试的时候应该是远程的Oracle，也是这样的建立Oracle连接）  
主机是localhost,端口是1521，服务名是我在本地中建的suwandb,用户名是system，密码是#definefalse123。（这些是我在win10中新建的，不知道为什么orcl服务的用户名密码都不成了无奈）  
在新建的Oracle连接suwan中，完成所有的考试题目，至此所有繁琐的Oracle数据库配置就完成了。
