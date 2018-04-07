title: 分布式锁的几种实现方式
date: 2018-03-02 22:18:29
tags: java
categories: 分布式架构
---
** {{ title }}：** <Excerpt in index | 首页摘要>
在分布式架构中，由于多线程和多台服务器，何难保证顺序性。如果需要对某一个资源进行限制，比如票务，比如请求幂等性控制等，这个时候分布式锁就排上用处。
<!-- more -->
<The rest of contents | 余下全文>

## 什么是分布式锁
分布式锁是控制分布式系统或不同系统之间共同访问共享资源的一种锁实现，如果不同的系统或同一个系统的不同主机之间共享了某个资源时，往往需要互斥来防止彼此干扰来保证一致性。

## 分布式锁需要解决的问题
1. 互斥性：任意时刻，只能有一个客户端获取锁，不能同时有两个客户端获取到锁。
2. 安全性：锁只能被持有该锁的客户端删除，不能由其它客户端删除。
3. 死锁：获取锁的客户端因为某些原因（如down机等）而未能释放锁，其它客户端再也无法获取到该锁。
4. 容错：当部分节点（redis节点等）down机时，客户端仍然能够获取锁和释放锁。

## 分布式锁的实现方式
1. 数据库实现

2. 缓存实现，比如redis

3. zookeeper实现

## 未完待续




> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc