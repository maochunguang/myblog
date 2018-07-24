title: Illegal mix of collations
date: 2017-06-12 11:00:14
tags: mysql
categories: 数据库
---
** mysql排序字符集问题：** <Excerpt in index | 首页摘要>
mysql表的每个字段都可以设置单独的排序字符集和文本字符集，如果你创建表的时候不注意，很可能会遇到Illegal mix of collations这个问题。
<!-- more -->
<The rest of contents | 余下全文>

## 问题描述
用mysql进行两个表的联合查询的时候，出现下面的错误。
```
Illegal mix of collations (utf8_unicode_ci,IMPLICIT) and (utf8_general_ci,IMPLICIT) for operation '='
```
## 排查过程
1. 通过google搜索找到原因，这个错误是mysql的排序字符集不一致导致的。
2. 把联合查询的表使用navicat查看字段的设置，发现了有一个关联字段排序字符集的问题，如图：
3. 这两个表中openid的排序规则不一致，导致出现问题。
![user表中opeid](http://o7kalf5h3.bkt.clouddn.com/openid01.png)
![user_tag表中opeid](http://o7kalf5h3.bkt.clouddn.com/openid02.png)

## 解决方法
将user表中的字符集和排序规则设置为默认，保持一致即可。



> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
