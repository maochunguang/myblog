title: mysql学习笔记
date: 2016-05-28 22:24:56
tags: mysql
categories: 数据库
---
** mysql学习笔记：** <Excerpt in index | 首页摘要>
	mysql学习，基础的增删改查，数据库优化，索引，分片，集群搭建等等。
<!-- more -->
<The rest of contents | 余下全文\>
 
### mysql的特点
1. 关系型数据库，免费使用，
2. 插入式存储引擎，
3. 性能高，

### 基础的增删改查
1. ddl语句，数据定义语句
	```
	create database test1;
	drop database test1;
	use test1;
	create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(2));
	drop table emp;
	alter table emp modify ename varchar(20);
	alter table emp add column age int(3);
	alter table emp drop column age;
	alter table emp change age age1 int(4);
	alter table emp add birth date after ename;
	alter table emp modify age int(3) first;
	alter table emp rename emp1;
	```
2. dml语句，数据操纵语句
	```
	insert into emp(ename,hiredate,sal,deptno) values('zzx1','2000-10-11',2000,1);
	insert into emp values('lisa','2004-05-09',3000,2);
	insert into dept values(5,'dept5'),(6,'dept6');
	update emp set sal=4000 where ename='lisa';
	update emp a,dept b set a.sal=a.sal*b.deptno,b.deptname=a.ename where a.deptno=b.deptno;
	delete from emp where ename='dony';
	delete a,b from emp a,dept b where a.deptno=b.deptno and a.deptno=3;
	select * from emp where ename='lisa';
	select distinct deptno from emp;
	select * from emp order by sal(desc);
	select * from emp order by sal limit 5;
	select * from emp order by sal limit 1,5;ss

	```
3. dcl语句，数据控制语句

### sql优化
1. 尽量使用 prepareStatement(java)，利用预处理功能。
2. 在进行多条记录的增加、修改、删除时，建议使用批处理功能，批处理的次数以整
个 SQL 语句不超过相应数据库的 SQL 语句大小的限制为准。
3. 建议每条 SQL 语句中 in 中的元素个数在 200 以下，如果个数超过时，应拆分为多
条 SQL 语句。禁止使用 xx in(‘’,’’….) or xx in(‘’,’’,’’)。 ★
4. 禁止使用 or 超过 200，如 xx =’123’ or xx=’456’。 ★
5. 尽量不使用外连接。
6. 禁止使用 not in 语句，建议用 not exist。 ★
7. 禁止使用 Union, 如果有业务需要，请拆分为两个查询。 ★
8. 禁止在一条 SQL 语句中使用 3 层以上的嵌套查询，如果有，请考虑使用临时表或
中间结果集。
9. 尽量避免在一条 SQL 语句中从>= 4 个表中同时取数， 对于仅是作为过滤条件关联，
但不涉及取数的表，不参与表个数计算
10. 查询条件里任何对列的操作都将导致表扫描，所以应尽量将数据库函数、计算表达
式写在逻辑操作符右边。
11. 在对 char 类型比较时,建议不要使用 rtrim()函数,应该在程序中将不足的长度补
齐。
12. 用多表连接代替 EXISTS 子句。
13. 如果有多表连接时， 应该有主从之分， 并尽量从一个表取数， 如 select a.col1, a.col2
from a join b on a.col3=b.col4 where b.col5 = ‘a’。
14. 在使用 Like 时，建议 Like 的一边是字符串，表列在一边出现。
15. 不允许将 where 子句的条件放到 having 中。
16. 将更新操作放到事务的最后执行。如
17. 一个事务需更新多个对象时，需保证更新的顺序一致以避免死锁的发生。如总是先
更新子表再更新主表，根据存货档案批量更新现存量时，对传入的存货档案 PK 进
行排序，再做更新处理等。
18. 禁止随意使用临时表，在临时数据不超过 200 行的情况下禁止使用临时表。
29. 禁止随意使用 distinct，避免造成不必要的排序。

### 索引优化
1. 创建索引，删除索引
```
create index cityname on city(city(10));
drop index cityname on city;
```
2. 搜索的索引列最好在where的字句或者连接子句
3. 使用唯一索引
4. 使用短索引，对于较长的字段，使用其前缀做索引
5. 不要过度使用索引，索引引起额外的性能开销和维护

### 高级优化措施

### 集群搭建
