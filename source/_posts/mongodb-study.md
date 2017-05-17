title: mongodb从入门到精通
date: 2015-12-19 23:21:23
tags: mongodb
categories: 数据库
---
** mongodb从入门到精通** <Excerpt in index | 首页摘要>
    mongodb日常使用的一些知识，增删改查，索引，分片。
 <!-- more -->
<The rest of contents | 余下全文>

### mongodb学习
## 1.mongodb特性
    1）mongo是一个面向文档的数据库，它集合了nosql和sql数据库两方面的特性。
    2）所有实体都是在首次使用时创建。
    3）没有严格的事务特性，但是它保证任何一次数据变更都是原子性的。
    4）也没有固定的数据模型
    5）mongo以javascript作为命令行执行引擎，所以利用shell进行复杂的计算和查询时会相当的慢。
    6）mongo本身支持集群和数据分片
    7）mongo是c++实现的，支持windows mac linux等主流操作系统
    8）性能优越，速度快
## 2.mongo常用操作
    1.增删操作
       db.user.insert({name:'aaaa',age:30});
       db.user.save({name:'aaaa',age:30});
       db.collection.insertOne({});(3.2新特性)
       db.collection.deleteOne(<filter>,{});(3.2新特性)
       db.collection.remove({name:'aaa'});
       db.collection.remove();(删除全部)

    2.更新操作
      db.users.update ({   " name"   :   "joe"   },   joe );
	  db.users.update ({   " name"   :   "joe"   },   joe,  true );------upsert模式
	  db.users.update ({   " name"   :   "joe"   },   joe,  true ，true);------MULTI模式


> update是对文档替换，而不是局部修改默认情况update更新匹配的第一条文档，multi模式更新所有匹配的  

    3.查询操作
      -- 普通查询
      db.user.find();
      db.user.find({name:'aaa'});
      db.user.findOne({name:'aaa'});

      -- 模糊查询
      db.UserInfo.find({userName :/A/}) （名称%A%）
      db.UserInfo.find({userName :/^A/}) (名称A%)

    4.操作符
        1.$lt, $lte,$gt, $gte(<, <=, >, >= ) 	
		2.$all	数组中的元素是否完全匹配  db.things.find( { a: { $all: [ 2, 3 ] } } );
		3.$exists  可选：true，false  db.things.find( { a : { $exists : true } } );
		4.$mod  取模：a % 10 == 1  db.things.find( { a : { $mod : [ 10 , 1 ] } } );
		5.$ne 取反：即not equals  db.things.find( { x : { $ne : 3 } } );
		6.$in 类似于SQL的IN操作  db.things.find({j:{$in: [2,4,6]}});
		7.$nin $in的反操作，即SQL的  NOT IN  db.things.find({j:{$nin: [2,4,6]}});
		8.$nor $or的反操作，即不匹配(a或b)  db.things.find( { name : "bob", $nor : [ { a : 1 },{ b : 2 }]})
		9.$or Or子句，注意$or不能嵌套使用  db.things.find( { name : "bob" , $or : [ { a : 1 },{ b : 2 }]})
		10.$size  匹配数组长度  db.things.find( { a : { $size: 1 } } );
		11.$type  匹配子键的数据类型，详情请看  db.things.find( { a : { $type : 2 } } );

    5.数组查询
        $size 用来匹配数组长度（即最大下标）  
		// 返回comments包含5个元素的文档   
		db.posts.find({}, {comments:{‘$size’: 5}});  
		// 使用冗余字段来实现  
		db.posts.find({}, {‘commentCount’: { ‘$gt’: 5 }});   
		$slice 操作符类似于子键筛选，只不过它筛选的是数组中的项  
		// 仅返回数组中的前5项  
		db.posts.find({}, {comments:{‘$slice’: 5}});  
		// 仅返回数组中的最后5项  
		db.posts.find({}, {comments:{‘$slice’: -5}});  
		// 跳过数组中的前20项，返回接下来的10项  
		db.posts.find({}, {comments:{‘$slice’: [20, 10]}});  
		// 跳过数组中的最后20项，返回接下来的10项  
		db.posts.find({}, {comments:{‘$slice’: [-20, 10]}});  
		MongoDB 允许在查询中指定数组的下标，以实现更加精确的匹配  
		// 返回comments中第1项的by子键为Abe的所有文档  
		db.posts.find( { "comments.0.by" : "Abe" } );   
## 3.索引的使用
    1.创建索引
		db.things.ensureIndex ({'j': 1});
		创建子文档 索引
		db.things.ensureIndex ({'user.Name' : - 1});
		创建 复合 索引
		db.things.ensureIndex ({
		'j' : 1 ,   //  升序
		'x' : - 1   //  降序
		});
		如果 您的 find 操作只用到了一个键，那么索引方向是无关紧要的  
        当创建复合索引的时候，一定要谨慎斟酌每个键的排序方向

	2.修改索引
		修改索引，只需要重新 运行索引 命令即可  
		如果索引已经存在则会 重建， 不存在的索引会被 添加  
		db . things . ensureIndex ({
			--- 原来的索引会 重建
			'user.Name ' :   - 1 ,
			--- 新增一个升序 索引
			'user.Name ' :   1 ,
			---  为 Age 新建降序 索引
			'user.Age ' :   - 1
		},
		打开后台执行
		{	‘background' :   true}
		);
		重建索引
		db. things .reIndex();
	3.删除索引
		删除集合中的所有 索引
		db . things . dropIndexes ();  
		删除指定键的索引  
		db.things.dropIndex ({
			x :   1 ,
			y :   - 1
		});  
		使用 command 删除指定键的 索引
		db.runCommand ({
			dropIndexes : 'foo ' ,
			index  :   {   y :   1   }
		});  
		使用 command 删除所有 索引
		db . runCommand ({dropIndexes : 'foo ' ,index  :   '*‘})
		如果是删除集合中所有的文档（remove）则不会影响索引，当有新文档插入时，索引就会重建。
	4.唯一索引
	    创建唯一索引，同时这也是一个符合唯一索引  
		db.things.ensureIndex (
		{
			'firstName ' :   1 ,
			'lastName ' :   1
		},   {
		指定为唯一索引
		'unique ' :   true ,
		删除重复 记录
		'dropDups ' :   true
		});

	5、强制使用索引
	  强制使用索引 a 和 b
		db.collection.find ({
			'a ' :   4 ,
			'b ' :   5 ,
			'c ' :   6
		}). hint ({
			'a ' :   1 ,
			'b ' :   1
		});
		强制不使用任何 索引
		db.collection.find ().hint ({
			'$ natural' :   1
		});
----------
索引总结:
		索引可以加速查询；
		单个索引无需在意其索引方向；
		多键索引需要慎重考虑每个索引的方向；
		做海量数据更新时应当先卸载所有索引，待数据更新完成后再重建索引；
		不要试图为每个键都创建索引，应考虑实际需要，并不是索引越多越好；
		唯一索引可以用来消除重复记录；
		地理空间索引是没有单位的，其内部实现是基本的勾股定理算法


## 4.mongo数据库管理
        - 安全与认证
		1、 默认为无认证，启动用登录 shell ；
		2、 添加账号；
		3、 关闭 shell 、关闭 MongoDB ；
		4、 为 MongoDB 增加 — auth 参数；
		5、 重 启 MongoDB ；
		6、 登录 shell ，此时就需要认证了

		- 冷备份
		1、关闭MongoDB引擎
		2、拷贝数据库文件夹及文件
		3、恢复时反向操作即可		
			-- 优点：可以完全保证数据完整性；
			-- 缺点：需要数据库引擎离线 	
		- 热备份
		1、 保持MongoDB为运行状态
		2、使用mongodump备份数据
		3、使用mongorestore恢复数据
			--	优点：数据库引擎无须离线
			--缺点：不能保证数据完整性，操作时会降低MongoDB性能

		- 主从复制备份
		1、创建主从复制机制
		2、配置完成后数据会自动同步
		3、恢复途径很多
			-- 优点：可以保持MongoDB处于联机状态，不影响性能
			-- 缺点：在数据写入密集的情况下可能无法保证数据完整性

		- 修复
		db.repairDatabase();
		  修复数据库还可以起到压缩数据的作用；
		  修复数据库的操作相当耗时，万不得已请不要使用；
		  建议经常做数据备份；
## 5.mongo复制(集群)
    1、主从复制
		选项  	说明
		--only  作用是限定仅复制指定的某个数据库
		--slavedelay  为复制设置操作延迟，单位为秒
		--fastsync  以主节点的数据快照为基础启动从节点。
		--autoresync  当主从节点数据不一致时，是否自动重新同步
		--oplogSize  设定主节点中的oplog的容量，单位是MB

	2、副本集
		与普通主从复制集群相比，具有自动检测机制
		需要使用—replSet 选项指定副本同伴
		任何时候，副本集当中最多只允许有1个活跃节点

	3、读写分离
		将密集的读取操作分流到从节点上，降低主节点的负载
		默认情况下，从节点是不允许处理
		客户端请求的，需要使用—slaveOkay打开
		不适用于实时性要求非常高的应用

	4、工作原理—— OPLOG
		oplog保存在local数据库中，oplog就在其中的
		oplog.$main集合内保存。该集合的每个文档都记录了主节点上执行的一个操作，其键定义如下：
		 ts：操作时间戳，占用4字节
		 op：操作类型，占用1字节
		 ns：操作对象的命名空间（或理解为集合全名）
		 o：进一步指定所执行的操作，例如插入

	5、工作原理—— 同步
		 从节点首次启动时，做完整同步
		 主节点数据发生变化时，做增量同步
		 从节点与主节点数据严重不一致时，做完整同步

	6、复制管理—— 诊断
		db.printReplicationInfo()
		在主节点上使用
		 返回信息是oplog的大小以及各种操作的耗时、空间占用等数据
		在从节点上使用
		db.printSlaveReplicationInfo()
		 返回信息是从节点的数据源列表、同步延迟时间等

	7、复制管理—— 变更OPLOG 容量
		在主节点上使用
		  设定—oplogSize参数
		  重启MongoDB

	8、复制管理—— 复制认证
		主从节点皆须配置
		 存储在local.system.users
		 优先尝试repl用户
		 主从节点的用户配置必须保持一致
## 6.MONGODB分片
    - 1、分片与自动分片
		  分片是指将数据拆分，分散到不同的实例上进行负载分流的做法。我们常说的“分表”、“分
			库”、“分区”等概念都属于分片的实际体现。
		  传统分片做法是手工分表、分库。自动分片技术是根据指定的“片键”自动拆分数据并维护数据
		请求路由的过程。

		递增片键--连续 不均匀 写入集中 分流较差
		随机片键--不连续 均匀 写入分散 分流较好
		三个组成部分
		--片
		  保存子集数据的容器
		--mongos
		  MongoDB的路由器进程
		--配置服务器
		  分片集群的配置信息
	- 2、创建分片
		--启动配置服务器
		  可以创建一个或多个
		--添加片
		  每个片都应该是副本集
		--物理服务器
		  性能、安全和稳定性
	- 3、管理分片
		--查询分片
		  db.shards.find();
		--数据库
		  db.databases.find();
		--块
		  db.chunks.find();
		--分片状态
		  db.printShardingStatus();
		--删除片
		  db.runCommand({ “removeshard” : “ip:port” });





----------
