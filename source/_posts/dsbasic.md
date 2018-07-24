title: 分布式系统理论基础
date: 2018-02-26 22:31:40
tags: protocol
categories: 分布式架构
---
** {{ title }}：** <Excerpt in index | 首页摘要>
分布式系统不是万能，不能解决所有痛点。在高可用，一致性，分区容错性必须有所权衡。
<!-- more -->
<The rest of contents | 余下全文>

## CAP理论
定理：任何分布式架构都只能同时满足两点，无法三者兼顾。
* Consistency（一致性），数据一致更新，所有的数据变动都是同步的。
* Availability（可用性），好的响应性能。
* Partition tolerance（分区容忍性）可靠性，机器宕机是否影响使用。

关系数据库的ACID模型拥有 高一致性 + 可用性 很难进行分区：
1. Atomicity原子性：一个事务中所有操作都必须全部完成，要么全部不完成。
2. Consistency一致性. 在事务开始或结束时，数据库应该在一致状态。
3. Isolation隔离性. 事务将假定只有它自己在操作数据库，彼此不知晓。
4. Durability持久性 一旦事务完成，就不能返回。
跨数据库两段提交事务：2PC (two-phase commit)， 2PC is the anti-scalability pattern (Pat Helland)
是反可伸缩模式的，JavaEE中的JTA事务可以支持2PC。因为2PC是反模式，尽量不要使用2PC，使用BASE来回避。

## BASE理论
* Basically Available 基本可用，支持分区失败
* Soft state 软状态，允许状态某个时间短不同步，或者异步
* Eventually consistent 最终一致性，要求数据最终结果一致，而不是时刻高度一致。

## paxos协议
Paxos算法的目的是为了解决分布式环境下一致性的问题。多个节点并发操纵数据，如何保证在读写过程中数据的一致性，并且解决方案要能适应分布式环境下的不可靠性（系统如何就一个值达到统一）。
### Paxos的两个组件:
* Proposer：提议发起者，处理客户端请求，将客户端的请求发送到集群中，以便决定这个值是否可以被批准。
* Acceptor:提议批准者，负责处理接收到的提议，他们的回复就是一次投票。会存储一些状态来决定是否接收一个值

### Paxos有两个原则
1. 安全原则---保证不能做错的事
    * a） 针对某个实例的表决只能有一个值被批准，不能出现一个被批准的值被另一个值覆盖的情况；(假设有一个值被多数Acceptor批准了，那么这个值就只能被学习)
    * b） 每个节点只能学习到已经被批准的值，不能学习没有被批准的值。
2. 存活原则---只要有多数服务器存活并且彼此间可以通信，最终都要做到的下列事情：
    * a）最终会批准某个被提议的值；
    * b）一个值被批准了，其他服务器最终会学习到这个值。

## zab协议(ZooKeeper Atomic broadcast protocol)
ZAB协议是为分布式协调服务 ZooKeeper 专门设计的一种支持崩溃恢复的原子广播协议。在 ZooKeeper 中，主要依赖 ZAB 协议来实现分布式数据一致性，基于该协议，ZooKeeper 实现了一种主备模式的系统架构来保持集群中各个副本之间的数据一致性。

### Phase 0: Leader election（选举阶段）
节点在一开始都处于选举阶段，只要有一个节点得到超半数节点的票数，它就可以当选准 leader。只有到达 Phase 3 准 leader 才会成为真正的 leader。这一阶段的目的是就是为了选出一个准 leader，然后进入下一个阶段。

### Phase 1: Discovery（发现阶段）
在这个阶段，followers 跟准 leader 进行通信，同步 followers 最近接收的事务提议。这个一阶段的主要目的是发现当前大多数节点接收的最新提议，并且准 leader 生成新的 epoch，让 followers 接受，更新它们的 acceptedEpoch。
一个 follower 只会连接一个 leader，如果有一个节点 f 认为另一个 follower p 是 leader，f 在尝试连接 p 时会被拒绝，f 被拒绝之后，就会进入 Phase 0。

### Phase 2: Synchronization（同步阶段）
同步阶段主要是利用 leader 前一阶段获得的最新提议历史，同步集群中所有的副本。只有当 quorum 都同步完成，准 leader 才会成为真正的 leader。follower 只会接收 zxid 比自己的 lastZxid 大的提议。

### Phase 3: Broadcast（广播阶段）
到了这个阶段，Zookeeper 集群才能正式对外提供事务服务，并且 leader 可以进行消息广播。同时如果有新的节点加入，还需要对新节点进行同步。

## raft协议
在Raft中，每个结点会处于下面三种状态中的一种：
### follower
所有结点都以follower的状态开始。如果没收到leader消息则会变成candidate状态。
### candidate
会向其他结点“拉选票”，如果得到大部分的票则成为leader。这个过程就叫做Leader选举(Leader Election)
### leader
所有对系统的修改都会先经过leader。每个修改都会写一条日志(log entry)。leader收到修改请求后的过程如下，这个过程叫做日志复制(Log Replication)：

    1. 复制日志到所有follower结点(replicate entry)
    2. 大部分结点响应时才提交日志
    3. 通知所有follower结点日志已提交
    4. 所有follower也提交日志
    5. 现在整个系统处于一致的状态





> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
