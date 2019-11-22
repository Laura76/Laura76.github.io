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
- 创建表空间  
//新建数据表  
```
create table tbl_taxi_gps(
tbl_taxi_gps_ID varchar(40) primary key,
FSTR_ID varchar(40) ,
FSTR_TIME varchar(40),
FFLT_X number(10),
FFLT_Y number(10)
);
```
//默认值是sysdate  
```
create table tbl_taxi_gps_count(
fdt_sysdate date default sysdate,
fstr_id varchar(20),
fstr_time varchar(20),
fint_gps_count number
);
```
//删除表  
```
drop table tbl_taxi_gps;
```
//删除表中所有的数据  
```
truncate table tbl_taxi_gps;
```
//新建没有主键的表--成功了--注意小数位数  
```
create table tbl_taxi_gps(
FSTR_ID varchar(40) ,
FSTR_TIME varchar(40),
FFLT_X number(20,10),
FFLT_Y number(20,10)
);
```
//导入txt数据的步骤--速度贼慢  
1.右键数据表，打开导入向导  
2.一步步按照顺序执行（注意点是要手动选择目标字段）  
//创建视图1  
```
create or replace view vw_taxi_gps1
as
select a.FSTR_ID,a.FSTR_TIME,a.FFLT_X,a.FFLT_Y,b.fint_gps_count from(
(select FSTR_ID,FSTR_TIME,FFLT_X,FFLT_Y
from (select row_number()over(partition by FSTR_ID order by FSTR_TIME desc)rn, tbl_taxi_gps.*
			from tbl_taxi_gps
			)
where rn=1) a
left join
(select FSTR_ID,count(FSTR_ID)fint_gps_count
from tbl_taxi_gps
group by FSTR_ID) b

on a.FSTR_ID=b.FSTR_ID
)
with read only;
```
//创建视图2  
```
create or replace view vw_taxi_gps2
as
select  FSTR_ID,time fstr_hour,count(FSTR_ID)fint_count from
(select FSTR_ID,substr(FSTR_TIME,0,2)time
from tbl_taxi_gps)a
group by FSTR_ID,time
order by FSTR_ID,time
with read only;
```
//创建存储过程--就是navicat里面的函数  
```
create or replace procedure pro_taxi_gps as
begin
insert into tbl_taxi_gps_count
select sysdate,FSTR_ID,max(FSTR_TIME),count(FSTR_ID)
from tbl_taxi_gps
where FSTR_TIME<=(to_char(sysdate,'HH24:mi:ss'))
group by FSTR_ID;
commit;
end;
```
//查询当前用户的存储过程  
```
select  *  from  user_objects  where  object_type  =  'PROCEDURE';
```
//创建job  
```
declare
job number;
begin
dbms_job.submit(
					job=>job,
					what=>'pro_taxi_gps;',
					next_date=>sysdate,
					interval=>'trunc(sysdate,''mi'') + 5/ (24*60)'
					);
					commit;
end;
```
//查看job下次执行时间以及间隔时间  
```
select * from user_jobs;
select * from dba_jobs where job = '1';
//停用job
begin
dbms_job.broken(job=>1,broken=>true);--Pro_CMMPtoZZJInf_VehicleInfo;
end;
```
//启用job  
```
begin
dbms_job.broken(1,false,trunc(sysdate,'mi') + 5/ (24*60) );
end;
```
//创建触发器--new表示新插入的记录  
```
create or replace trigger mytrigger
before insert on tbl_taxi_gps_count
for each row
begin
delete from tbl_taxi_gps gps where :new.fstr_id=gps.fstr_id and :new.fstr_time=gps.fstr_time;
end;
```
