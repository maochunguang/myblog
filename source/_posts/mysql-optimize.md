title: mysql优化的常用方法
date: 2016-06-11 07:25:13
tags: mysql
categories: 数据库
---
** mysql优化：** <Excerpt in index | 首页摘要>
    mysql的优化措施，从sql优化做起
<!-- more -->
<The rest of contents | 余下全文>

## 优化sql的一般步骤
1. 通过show status了解各种sql的执行频率
2. 定位执行效率低的sql语句
3. 通过explain分析效率低的sql
4. 通过show profile分析sql
5. 通过trace分析优化器如何选择执行计划
6. 确定问题，采取措施优化

## 索引优化措施
1. mysql中使用索引的典型场景
    1. 匹配全值，条件所有列都在索引中而且是等值匹配
    2. 匹配值的范围查找，字段必须在索引中
    3. 匹配最左前缀，复合索引只会根据最左列进行查找
    4. 仅仅对索引进行查询，即查询的所有字段都在索引上
    5. 匹配列前缀，比如like 'ABC%',如果是like '%aaa'就不可以
    6. 如果列名是索引，使用column is null会使用索引

2. 存在索引但不会使用索引的典型场景
    1. 以%开头的like查询不能使用b树索引
    2. 数据类型出现隐式转换不能使用索引
    3. 复合索引，查询条件不符合最左列原则
    4. 用or分割的条件，如果前面的条件有索引，而后面的条件没有索引

3. 查看索引使用的情况
```
show status like 'Handler_read%';
```
如果Handler_read_rnd_next的值比较高，说明索引不正确或者查询没有使用到索引

## 简单实用的优化方法
1. 定期检查表和分析表
分析表语法：
```
analyze table 表名；
```
检查表语法：
```
check table 表名；
```
2. 定期优化表
    - 对于字节大小不固定的字段，数据更新和删除会造成磁盘空间不释放，这时候就行优化表，可以整理磁盘碎片，提高性能
语法如下：
```
optimize table user(表名)；
```









> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
