## 集群情况下如何保证quartz定时任务不重复执行？

### 1 数据量评估

定时任务一般的使用场景是定时查询出一批数据，对这一批数据进行业务操作。

根据数据量的大小决定是否使用分布式任务，如果数据量不大或者实时性要求不高，单机任务就够了，也可以减少相应复杂度。

如果数据量大就需要部署分布式任务。分布式集群中的节点对数据进行分片处理，即每个节点拿一部分数据进行业务处理。

### 2 、仅解决并发问题方案

(1) Quartz + 分布式锁

假设定时任务部署了3台机器，在任务启动时3台机器竞争分布式锁，谁竞争到谁就执行，剩下2台不执行。分布式锁可以使用Redis或者Zookeeper

(2) 开关方案

方案一：配置文件中设置开关是否开启，执行任务前读取该开关，开启则执行。这种方式实现比较简单，但是如果需要换另外一台机器执行，必须修改配置项并发布项目，维护成本较高

方案二：建一个数据库配置表，配置表中配置可执行任务的机器标识，每台机器执行前读取这个配置，看看是否是本机。如果是则执行，否则不执行

方案三：Zookeeper做一个全局配置，配置项内容是可执行任务的机器标识，执行原理同方案一

### 3 、真正分布式任务方案

(1) Quartz官方分布式方案

我认为这种方式比较重，需要根据官方文档新建数据表，个人并不推荐

(2) Elastic Job

当当网开源的一个分布式调度解决方案，在业界比较通用

(3) 自研分布式任务平台

有一定技术实力的公司，可以选择自研分布式任务平台

（全文完）