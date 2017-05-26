title: redis学习笔记
date: 2016-05-23 08:25:57
tags: redis
categories: 数据库
---
** redis学习笔记：** <Excerpt in index | 首页摘要>
    redis数据库的基本操作，增删改查
<!-- more -->
<The rest of contents | 余下全文>

## keys
redis本质上是一个key-value数据库
1. 设置：set key value
2. 获取：get key
3. 判断存在：exists key
4. 删除：del key		del  test:fan:age
5. 重命名：rename  oldkey newkey		
6. 数量：dbsize  返回数据
7. 获取所有key（通配符）：`Keys test:*:age`
`Keys test:?:age`
8. 清空：flushdb	flushall
9. 设置有效时间：expire test:fan:age 30
10. 查询有效时间：ttl test:fan:age

## String类型
1. 设置：
	set key value
	setnx ky value(nx是not exist)
	mset key1 value1 keyN valueN
	msetnx key1 value1 keyN valueN
2. 获取：
	get			不存在返回nil
	getset		设置key的值，并返回key的旧值，不存在返回nil
	mget		
3. 自增减：
	incr key   对key的值进行++操作，返回新的值
	decr key
	incrby key integer		对key加上一个数值
	decrby key integer
4. 截取：
	substr key indexStart indexEnd 			下标从0开始
5. 追加：
	append key value

## list类型
redis的list其实就是一个每个元素都是string 的双向链表，所以push和pop的时间复杂度都是O（1）
1. 添加
	lpush key string 		在头部添加
	rpush key string		在尾部添加
2. 修改
	lset key index value  修改指定下标的key的值
3. 删除
	lpop key 	从头部返回删除
	rpop key  从尾部
	lrem key count value  删除count个相同的value，count为0删除全部
	blpop key ...keyN timeout
	brpop 从尾部删除
4. 获取
	lrange key indexStart indexEnd
5. 数量
	llen key		返回key对应的list长度
6. 截取
	ltrim key start end
7. 转移
	rpoplpush key1 key2	从key1尾部移到key2头部

## set集合
redis的set就是String的无序集合，通过hashtable实现
1. 添加
	sadd key member
2. 删除
	srem key member		移除指定的元素
	spop key 					删除并返回一个随机的
3. 获取
	smembers key			返回所有
	srandmember			随机取一个不删除
4. 判断存在
	sismember key member
5. 数量
	scard key 					返回元素个数
6. 转移
	smove srckey dstkey member
7. 取交集
	sinter key1 key2 keyN
	sinterstore dstkey key1 keyN		将交集存在dstkey
8. 取并集
	sunion key1 key2 keyN
	sunionstore dstkey key1 keyN	将并集存在dstkey
9. 取差集
	sdiff key1 key2 keyN
	sdiffstore dstkey key1 keyN		将差集存在dstkey

## 有序set类型
和set一样，不同的是每个元素关联一个double类型的score，根据score排序，sorted set的实现由skip list和hashtable
1. 添加
	zadd key score member
2. 删除
	zrem key member
	zremrangebyrank key min max
	zremrangebyscore key min max 	删除集合score在给定区间的元素
3. 获取
	zrange key start end
	zrevrange	key start end			按score的逆序
	zrangebyscore key min max		
4. 判断存在
	zrank key member		返回下标
	zrerank key member		返回逆序的下标
5. 数量
	zcard key						总数
	zcount key min max 		区间的数量
6. 修改
	zincrby key incr member	增加member的score值并排序

## hash类型
redis的hash是一个string类型的field和value的映射表，hash特别适合存储对象，
1. 设置：
	hset key field value
	hmset key field1 value1 field2 value2
2. 获取：
	hget key field
	hmget key field1 field2
3. 判断存在
	hexists key field
4. 删除
	hdel key field
5. 查找
	hkeys key			返回所有 field
	hvals key			返回所有的value
	hgetall key		返回所有field和value
6. 数量
	hlen key
7. 值加减
	hincrby key field integer	将指定的hash field加上定值
