title: redis后台启动详细配置
date: 2017-05-15 22:58:07
tags: redis
categories: 数据库
---
** {{ title }}：** <Excerpt in index | 首页摘要>
  redis启动的时候有多种模式，后台启动，集群启动等等。
<!-- more -->
<The rest of contents | 余下全文>

## 说明
在开发中一般都是在命令行中直接运行`redis-server`,但是这样命令行关闭，服务就停止了。
如果要在后台运行redis服务，需要制定配置文件。这里以**ubuntu14**为例子

## 准备配置文件
查看‘/etc/redis/redis.conf’,没有可以创建一个，或者下载一个，配置文件位置没有要求

## 修改配置文件
把daemonize设置为yes，
然后`redis-server /etc/redis/redis.conf`启动服务，

## 查看服务
`ps -ef|grep redis-server`查看是否有redis进程存在

## 更多配置，在conf文件有说明
```
# 是否以后台daemon方式运行，默认是 no，一般我们会改为 yes
daemonize no
pidfile /var/run/redis.pid
# 只允许本机访问
bind 127.0.0.1
# 端口设置
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 0
loglevel notice
# 日志文件
logfile ""
# 开启数据库的数量，Redis 是有数据库概念的，默认是 16 个，数字从 0 ~ 15
databases 16
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
dir ./
slave-serve-stale-data yes
slave-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-disable-tcp-nodelay no
# 密码设置，需要设置密码打开
requirepass 123455
slave-priority 100
appendonly no
appendfilename "appendonly.aof"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-entries 512
list-max-ziplist-value 64
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit slave 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
aof-rewrite-incremental-fsync yes
```





> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
