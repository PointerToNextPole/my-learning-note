# **Redis学习笔记**
> 慕课网《一站式学习Redis 从入门到高可用分布式实践》



### 其他

#### redis主从、哨兵、集群的区别

- **主从**

  由于数据是存储在一台服务器上的，如果这台服务器出现硬盘故障等问题，也会导致数据丢失。为了避免单点故障，通常的做法是将数据库复制多个副本以部署在不同的服务器上，这样即使有一台服务器出现故障，其他服务器依然可以继续提供服务。为此， Redis 提供了复制（replication）功能，可以实现当一台数据库中的数据更新后，自动将更新的数据同步到其他数据库上。

  在复制的概念中，数据库分为两类，一类是主数据库（master），另一类是从数据库（slave）。主数据库可以进行读写操作，当写操作导致数据变化时会自动将数据同步给从数据库。而从数据库一般是只读的，并接受主数据库同步过来的数据。一个主数据库可以拥有多个从数据库，而一个从数据库只能拥有一个主数据库。

  **主从数据库的配置**

  - master不用配置，从redis的conf文件加入`slaveof ip port`就可以了

  - 或者从redis启动时 `redis-server --port 6380 --slaveof 127.0.0.1 6379`

    从数据库一般是只读，可以改为可写，但写入的数据很容易被主同步没，所以还是只读就可以。

- **哨兵**

  当主数据库遇到异常中断服务后，开发者可以通过手动的方式选择一个从数据库来升格为主数据库，以使得系统能够继续提供服务。然而整个过程相对麻烦且需要人工介入，难以实现自动化。 为此，Redis 2.8中提供了哨兵工具来实现自动化的系统监控和故障恢复功能。**哨兵的作用**就是监控redis主、从数据库是否正常运行，主出现故障自动将从数据库转换为主数据库。

  顾名思义，哨兵的作用就是监控Redis系统的运行状况。它的功能包括以下两个。

  - 监控主数据库和从数据库是否正常运行。
- 主数据库出现故障时自动将从数据库转换为主数据库。
  
- **群集**

  即使使用哨兵，redis每个实例也是全量存储，每个redis存储的内容都是完整的数据，浪费内存且有木桶效应。为了最大化利用内存，可以采用集群，就是分布式存储。

摘自：[redis主从、哨兵、集群的区别](https://blog.csdn.net/u014761412/article/details/90408173)

***

### From 1.1

Redis是高性能Key-Value服务器

### From 1.2

Redis是一个基于<font color=FF0000>键值对</font>的存储服务系统（即数据库），支持五种数据结构：字符串（Strings / bitmaps位图 / Blobs 补充：HyperLogLog、GEO）、哈希表、链表、集合、有序集合。

### From 1.4

**Redis特性：**

- 速度快
- 持久化
- 支持多种数据结构
- 支持多种编程语言
- 功能丰富
- 简单
- 主从复制
- 高可用、分布式

### From 1.5

- Redis之所以快是因为Redis将数据存在内存中
- Redis使用C语言编写
- Redis线程模型是单线程

### From 1.6

Redis将所有数据都保存在<font color=FF0000>**内存**</font>中，对于数据的更新将异步的保存到磁盘上<font color=FF0000>(即为持久化)</font>

### From 1.8
Redis支持包括Java、Python、PHP、Ruby、Lua、Node.js等语言

### From 1.9

**Redis功能：**

- 支持发布订阅
- 支持Lua脚本
- 支持事务
- 支持pipeline

### From 1.10

Redis不依赖于外部库

### From 1.11

Redis具有主从复制功能，即：<font color=FF0000>从服务器</font>可以将数据<font color=FF0000>复制</font>到<font color=FF0000>主服务器</font>中

### From 1.13
**Redis的典型应用场景：**

- 缓存系统（用Redis实现Cache的功能）
- 计数器（比如微博中的转发数、评论数）
- 消息队列系统（用于中间件）
- 排行榜（通过有序集合实现）
- 社交网络（如实现微博中粉丝数、共同好友数）
- 实时系统（比如实现垃圾邮件过滤）


### From 1.14
**Redis的安装命令（Linux）：**

```sh
wget http://download.redis.io/releases/redis-x.x.x.tar.gz   #下载
tar -xzf redis-x.x.x.tar.gz 																#进行解压缩
ln -s redis-x.x.x redis 																		#建立软链接，有利于后期升级
cd redis 																										#进入目录
make && make install 																				#编译和安装
```

**Redis可执行文件说明：**
- redis-server              ==> 使用它可以启动Redis服务器
- redis-cli                     ==> Redis命令行客户端，用来**<font color=FF0000>连接Redis服务端</font>**
- redis-benchmark    ==> Redis性能测试工具，用于对Redis进行性能测试
- redis-check-aof       ==> AOF文件修复工具，对持久化方法AOF的文件修复工具（比如断电的情况下，文件会损坏）
- redis-check-dump  ==> RDB文件修复工具，与上面类似，RDB文件的修复工具
- redis-sentinel          ==> redis-sentinel是高可用版本，用于启用sentinel服务器

**Redis三种启动方法：**
- 最简启动（默认配置启动）：直接使用redis-server

  ```sh
  #验证方法：
  ps -ef | grep redis #查看进程方式进行验证
  netstat -antpl | grep redis #查看端口是否处于listen的状态
  redis-cli -h ip -p port ping
  ```
- 动态参数启动（指定一些动态参数）
  Redis默认使用6379作为默认端口，若要使用其他端口即可使用如下命令

  ```sh
  redis-server --port 6380
  ```
- 配置文件启动（推荐使用）
  ```sh
  reids-server configPath  #将所有的配置都写在配置文件中，其中configPath为配置文件路径
  ```

**三种启动方式比较**

- 推荐使用“<font color=FF0000>生产环境</font>选择<font color=FF0000>配置启动</font>”
- <font color=FF0000>单机多实例</font>配置文件可以<font color=FF0000>用端口区分</font>开

**Redis客户端连接**

```sh
#使用Redis连接10.10.79.150上的6384端口的Redis
redis-cli -h 10.10.79.150 -p 6384

#连接成功之后
10.10.79.150:6384> ping #使用客户端向Redis服务器发送一个ping，如果服务器运作正常的话，将会返回一个PONG。通常用于测试与服务器的连接是否依然生效，或者测量延迟值
PONG
10.10.79.150:6384> set hello world #设置一个键值对，hello为键，world为值
OK
10.10.79.150:6384> get hello #获得hello为键的值
"world"
10.10.79.150:6384> del hello #删除key-value
(integer) 1
10.10.79.150:6384> quit #推出连接
```

**Redis客户端返回值**
- 状态回复
```sh
10.10.79.150:6384> ping
PONG
```
- 错误回复
```sh
10.10.79.150:6384> hget hello field #其中hget是一个hash的命令，这里错在用在了字符串的键值对上
(error) WRONGTYPE Operation against
```
- 整数回复
```sh
# 对键值做一个计数
10.10.79.150:6384> incr hello
(integer) 1
```
- 字符串回复
```sh
10.10.79.150:6384> get hello
"world"
```
- 多行字符串回复
```sh
#使用批量命令
10.10.79.150:6384> mget hello foo
1) "world"
2) "bar"
```



### From 1.15
**Redis常用配置**

- daemonize    ==> 是否以<font color=FF0000>守护进程</font>(no | yes)的方式<font color=FF0000>对Redis进行启动</font>，<font color=FF0000>默认配置为no</font>；但建议使用yes：当使用yes时启动的日志将会打印到设置的文件的配置当中
- port                ==> Redis对外端口号，使用哪一个端口进行使用，<font color=FF0000>**默认为6379**</font>
- logfile            ==> Redis系统日志文件名称，其中记录Redis相关表现和异常
- dir                  ==> Redis工作目录，日志文件持久化文件是存在哪个目录的

### From 1.16（自己补充）

**macOS的Redis安装在`/usr/local/bin`中，其中包含：**

- redis-benchmark
- redis-check-aof
- redis-check-rdb
- redis-cli
- redis-sentinel
- redis-server

**macOS的Redis配置在`/usr/local/etc`中，其中包含：**

- redis-sentinel.conf
- redis.conf
- redis.conf.default

### From 2.2 - 2.4
- **通用命令**

  - keys   ==>  计算Redis数据库中所有的键值，如命令

    ```sh
    keys * 				#表示完全通配
    keys he*      #*表示多位
    keys he[h-l]* #表示部分通配（类似正则表达式）
    keys he?      #?表示一位
    ```

    不建议在生产环境中使用keys，原因：

    - 是O(n)的命令，当数据很多时，占用过多资源
    - 在生产环境中keys命令没有意义

    如何使用keys：热备从节点  

  - dbsize   ==>  计算Redis数据库的大小，O(1)命令（因为Redis内置计数器）

  - exists key   ==>  判断key是否存在，存在则返回1，不存在则返回0

  - del key [key...]   ==>  删除key，后面可跟多个key。存在且成功删除则返回1，不存在删除失败 则返回0

  - expire key seconds   ==>  为key设定删除时间，即在seconds时间后key将被删除（过期）（除了seconds外，还有在一个时间戳时间内过期，在毫秒时间内过期）

    - ttl key   ==> 查看key<font color=FF0000>剩余的过期时间</font>
    - persist key ==> 去掉key过期的时间

  - type key   ==>  判断key的数据类型（返回：string、hash、list、set、zset（有序集合）、none（不存在））

  |  命令  | 时间复杂度 |
  | :----: | :--------: |
  |  keys  |    O(n)    |
  | dbsize |    O(1)    |
  |  del   |    O(1)    |
  | exists |    O(1)    |
  | expire |    O(1)    |
  |  type  |    O(1)    |
- **处理结构和内部编码**

  <img src="https://i.loli.net/2020/08/14/HvxnzKUpRVcFQjM.png" style="zoom: 33%;" />

<img src="https://i.loli.net/2020/08/14/NXv1l9Q5ZgOtayx.png" style="zoom: 18%;" />

- **单线程架构**

  为什么单线程这么快

  - 纯内存（主要原因）
  - 非阻塞IO
  - 避免线程切换和竞态消耗

  对于单线程的特性要注意什么：

  - 一次只执行一条命令
  - 拒绝长（慢）命令（类似于OS中的）
  - Redis并不完全是单线程（fsync file descriptor、close file descriptor）

### From 2.5

 **字符串**

key都是字符串，而<font color=FF0000>value可以是字符串、数字、位（bits）、json</font>，另外字符串的value不能大于512MB

字符串使用场景：缓存、计数器、分布式锁、等等

一些命令：

- set key value
  - setnx key value：key不存在才设置
  - set key value xx：key存在才设置
- mset key1 value1 key2 value2 key3 value3 .. ：批量设置key-value
  - 1次mget = 1次网络时间 + n次命令时间
- get key
- mget ke y1 key2 key3 ... ：批量获取key，原子操作，时间复杂度为O(n)
- del key
- incr key：key自增一，若key不存在，自增后get(key)=1
- decr key：key自减一，若key不存在，自减后get(key)=-1
- incrby key k：key自增k，若key不存在，自增后get(key)=k
- decrby key k：key自减k，若key不存在，自减后get(key)=-k
- getset key newValue：set key newValue并返回旧的value
- append key value：将value追加到旧的value
- strlen key：返回字符串的长度（注意中文，中文长度用的不是一个字节） 
- incrbyfloat key floatNum：增加key对应的<font color=FF0000>浮点数</font>floatNum
- getrange key startIndex endIndex：获取字符串制定下标范围内（ [startIndex, endIndex] ）所有的值
- setrange key index value：设置指定下标index所有对应的值value（这里的值可以是字符串），示例：
  - `setrange greeting 6 "redis"`

### From 2.6 - 2.7

哈希键值结构：<font color=FF0000>哈希value分为两个结构：field 和 value</font>（类似于mapmap？）

另外，需要补充的是：<font color=FF0000>field不能相同，value可以相同</font>

<img src="https://i.loli.net/2020/08/14/Hxaf7OtXoylBZCT.png" style="zoom: 33%;" />

命令：h表示hash

- hget key field：获取hash key对应的field的value
- hgetall key：获取hash key所对应的所有field和value，时间复杂度O(n)
- hvals key：获取hash key所对应的所有field和value，时间复杂度O(n)
- hkeys key：返回hash key 对应的所有field，时间复杂度O(n)
- hset key field value：设置hash key对应的field的value
  - hsetnx key field value
- hdel key field：删除hash key对应的field的value
- hincrby key field intCounter：hash key对应的field的value自增intCounter
- hintbyfloat key field floatCounter：hincrby浮点数版
- hexists key field：判断hash key是否有field
- hlen key：获取hash key field的数量
- hmget key field1 field2 ... fieldN，时间复杂度O(n)
- hmset key field1 value1 field2 value2 ... fieldN valueN，时间复杂度O(n)

三种方案比较

|   命令    |             优点             |                   缺点                   |
| :-------: | :--------------------------: | :--------------------------------------: |
| string v1 |    编程简单、可能节约内存    | 1.序列化开销<br>2.设置属性要操作整个结构 |
| string v2 |      直观、可以部分更新      |    1.内存占用比较大2.key比较分散    |
|   Hash    | 直观、节约空间、可以部分更新 |    1. 编程稍微复杂 2.ttl不好控制    |

### From 2.8 - 2.9

 列表的特点

- 有序
- 可以重复
- 左右两边可以插入弹出

命令：

- lpush key value1 value2 ... valueN：从列表左侧插入值
- rpush key value1 value2 ... valueN：从列表右侧插入值
- linsert key before | after value newValue：从list指定的值前｜后插入newValue，时间复杂度O(n)
- lpop key：从列表左边弹出一个item
- rpop key：从列表右边弹出一个命令
- lrem key count value：根据count值，从列表中删除所有value相等的项，时间复杂度O(n)
  - count > 0，<font color=FF0000>从左到右</font>，删除最多count个value相等的项
  - count < 0 ，<font color=FF0000>从右到左</font>，删除最多Math.abs(count)个value相等的项
  - count=0，删除所有value相等的项
- ltrim key startIndex endIndex：按照索引表范围修剪列表，保留[startIndex, endIndex]
- lrange key startIndex endIndex：获取列表指定索引范围所有的item，示例如下：a - b - c - d - e -f
  - lrange listkey 0 2 ：a - b - c
  - lrange listkey 1 -1 : b - c - d - e -f
- lindex key index：获取列表指定索引的item
- llen key：获取列表的长度，，时间复杂度O(1)
- lset key index newValue：设置列表指定索引值为newValue
- blpop key timeout：lpop的阻塞版本，timeout是阻塞超时时间，timeout=0表示永不阻塞
- brpop key timeout：rpop的阻塞版本，timeout是阻塞超时时间，timeout=0表示永不阻塞

**TIPS**

- LPUSH + LPOP = Stack
- LPUSH + RPOP = Queue
- LPUSH + LTRIM = Capped Collection
- LPUSH + BRPOP = Message Queue

### From 2.10

**集合特点：**

- 无序
- 无重复
- 集合间操作

 **命令：**

- sadd key element1 [element2 ...]：向集合key添加element（若element已存在，添加失败）
- srem key element1 [element2 ..]：将集合key中的element移除掉
- scard key：返回集合 key的基数(集合中元素的数量)，当 key不存在时，返回 0 。
- sismember：判断 member 元素是否集合 key 的成员。
- srandmember key [count]：只提供 key 参数时，返回一个元素；如果集合为空，返回 nil 。 如果提供了 count 参数，那么返回一个数组；如果集合为空，返回空数组。只提供 key 参数时为 O(1) 。如果提供了 count 参数，那么为 O(n) ，n 为返回数组的元素个数。
- smembers key：返回集合 key 中的所有成员。
- spop key：<font color=FF0000>移除</font>并返回集合中的一个<font color=FF0000>随机元素</font>。注意和srandmember key的区别
- sdiff key [key ...]：差集
- sinter key [key ...]：交集（intersection）。时间复杂度: O(N * M)， N 为给定集合当中个数最小的集合， M 为给定集合的个数。
- sunion key [key …]：并集。返回一个集合的全部成员，该集合是所有给定集合的并集。时间复杂度: O(N)， `N` 是所有给定集合的成员数量之和。
- sdiff | sinter | sunion + store destKey：将差集、交集、并集结果保存在destKey中

### From 2.11

**有序集合结构**

<img src="https://i.loli.net/2020/08/14/pvGZlVmtWiP2q48.png" style="zoom:20%;" />

**集合和有序集合的对比**

|    集合    |    有序集合     |
| :--------: | :-------------: |
| 无重复元素 |   无重复元素    |
|    无序    |      有序       |
|  element   | element + score |

**列表和有序集合的对比**

|    集合    |    有序集合     |
| :--------: | :-------------: |
| 有重复元素 |   无重复元素    |
|    有序    |      有序       |
|  element   | element + score |

**命令：**

- zadd key score member [ [score member] [score member] …]：添加score和element，时间复杂度O(logn)
- zrem key member [member …]：删除元素，时间复杂度O(1)
- zscore key element：返回元素的分数
- zincrby key increScore element：为有序集 key 的成员 element 的 score 值加上增量 increScore
- zcard key：返回元素key的总个数
- zrank key member：返回有序集 key 中成员 member 的排名。其中有序集成员按 score <font color=FF0000>值递增(从小到大)顺序排列</font>
- zrevrank key member：返回有序集 key 中成员 member 的排名。其中有序集成员按 score 值<font color=FF0000>递减(从大到小)排序</font>。时间复杂度: O(log(N))
- zrange key start stop [WITHSCORES]：返回有序集 key 中，指定区间内的成员。其中成员的位置按 score 值<font color=FF0000>递增(从小到大)来排序</font>。具有相同 score 值的成员按字典序(lexicographical order )来排列。时间复杂度: O(log(N)+M)， N 为有序集的个数，而 M 为结果集的个数。
- zrevrange key start stop [WITHSCORES]：返回有序集 key 中，指定区间内的成员。其中成员的位置按 score 值递减(从大到小)来排列。 具有相同 score 值的成员按字典序的逆序(reverse lexicographical order)排列。时间复杂度: O(log(N)+M)， `N` 为有序集的基数，而 `M` 为结果集的基数。
- zrangebyscore key min max ：返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。时间复杂度: O(log(N)+M)， N 为有序集的基数， M 为被结果集的基数
- zrevrangebyscore ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]：返回有序集 key 中， score 值介于 max 和 min 之间(默认包括等于 max 或 min )的所有的成员。有序集成员按 score 值递减(从大到小)的次序排列。时间复杂度: O(log(N)+M)， N 为有序集的基数， M 为结果集的基数。
- zcount key min max：返回有序集 key 中， score 值在 min 和 max 之间(默认包括 score 值等于 min 或 max )的成员的数量。时间复杂度: O(log(N))， N 为有序集的基数。
- zremrangebyrank key startIndex endIndex：移除有序集 key 中，指定排名(rank)区间内的所有成员。时间复杂度: O(log(N)+M)， N 为有序集的基数，而 M 为被移除成员的数量。
- zinterstore destination numkeys key [key …] [WEIGHTS weight [weight …]] [AGGREGATE SUM|MIN|MAX]：计算给定的一个或多个有序集的交集，其中给定 key 的数量必须以 numkeys 参数指定，并将该交集(结果集)储存到 destination 。默认情况下，结果集中某个成员的 score 值是所有给定集下该成员 score 值之和。关于 WEIGHTS 和 AGGREGATE 选项的描述，参见 ZUNIONSTORE destination numkeys key [key …] [WEIGHTS weight [weight …]] [AGGREGATE SUM|MIN|MAX] 命令。

### From 3.2

Java Redis客户端：Jedis

**Jedis的maven依赖**

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>x.x.x</version>
</dependency>
```

**使用Jedis直连**

```java
//1.生成一个Jedis对象，这个对象负责和指定Redis节点进行通信
Jedis jedis = new Jedis（" 127.0.0.1"， 6379）；
//2. jedistiTsetj
jedis.set（"hello"， "world"）；
//3. jedistiJget/'F， value "world"
String value = jedis.get（"hello"）；
```

**Jedis对象构造方法**

```java
/**
 * host: Redis节点的所在机器的IP
 * port: Redis节点的端口
 * connection Timeout：客户端连接超时
 * soTimeout:客户端读写超时
 */
Jedis(String host, int port, int connectionTimeout, int soTimeout)
```

**简单实用数据结构**

- String

  ```java
  //输出结果: OK
  jedis.set("hello"， "world")
  //输出结果: world
  jedis.get("hello");
  //输出结果:1
  jedis.incr(" counter");
  ```

- hash

  ```java
  jedis.hset("myhash", "f1", "v1");
  jedis.hset("myhash", "f2", "v2");
  //输出结果: {f1=v1, f2=v2}
  jedis.hgetAll(" myhash");
  ```

- list

  ```java
  jedis.rpush(" mylist", "1");
  jedis.rpush(" mylist", "2");
  jedis.rpush(" mylist", "3");
  //输出结果:[1, 2, 3]
  jedis.lrange(" mylist", о, -1);
  ```

- set

  ```java
  jedis.sadd("myset", "a");
  jedis.sadd("myset", "b");
  jedis.sadd("myset", "a");
  //输出结果: [b, a]
  jedis.smembers("myset");
  ```

- zset

  ```java
  jedis.zadd(" myzset", 99, "tom");
  jedis.zadd(" myzset", 66, "peter");
  jedis.zadd(" myzset", 33, "james");
  //输出结果 : [["james"],33.0], [[' peter"],66.0], [["tom"],99.0]]
  jedis.zrangeWithScores("myzset", 0, -1);
  ```

**Jedis直连示意图**

<img src="https://i.loli.net/2020/08/14/W35NMukgEZ8tIOY.png" style="zoom:30%;" />

**Jedis连接池示意图**

<img src="https://i.loli.net/2020/08/14/6AuHVZ9nafOpktP.png" style="zoom:30%;" />

**方案对比**

|        |                             优点                             |                             缺点                             |
| :----: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  直连  |            简单方便适用于少量长期连接的场景             | 存在每次新建/关闭TCP开销<br> 资源无法控制,存在连接泄露的可能<br> Jedis对象线程不安全 |
| 连接池 | Jedis预先生成，降低开销使用连接池的形式保护和控制资源的使用 | 相对于直连,使用相对麻烦，尤其在资源的管理上需要很多参数来保证，一旦规划不合理也会出现问题。 |

**Jedis连接池的简单使用**

```java
//初始化Jedis连接池， 通常来讲JedisPool是单例的。
GenericObjectPoolConfig = poolConfig = new GenericObjectPoolConfig();
JedisPool jedisPool = new JedisPool(poolConfig, "127.0.0.1", 6379);
```

### From 3.3 - 3.4

暂时没有使用redis for py or for golang的可能，所以略过

### From 3.5

- commons-pool配置(1)-资源数控制

  |  参数名   |            含义             | 默认值 | 使用建议 |
  | :-------: | :-------------------------: | :----: | :------: |
  | MaxTotal  |      资源池最大连接数       |   8    |          |
  |  maxIdle  |  资源池允许对打空闲连接数   |   8    |          |
  |  minIdle  |  资源池确保最少空闲连接数   |   0    |          |
  | jmxEnable | 是否开启jmx监控，可用于监控 |  true  | 建议开启 |

- commons-pool配置(2)-借还参数

  |       参数名       |                             含义                             |      默认值      |     使用建议     |
  | :----------------: | :----------------------------------------------------------: | :--------------: | :--------------: |
  | blockWhenExhausted | 当资源池用尽后，调用者是否要等待。只有当为true时，下面的maxWaitMillis才会生效 |       true       |  建议使用默认值  |
  |   maxWaitMillis    |     当资源池连接用尽后,调用者的最大等待时间(单位为毫秒)      | -1：表示永不过时 | 不建议使用默认值 |
  |    testOnBorrow    | 向资源池借用连接时是否做连接有效性检测(ping) ,无效连接会被移除 |      false       |    建议false     |
  |    testOnReturn    | 向资源池归还连接时是否做连接有效性检测(ping) ,无效连接会被移除 |      false       |    建议false     |

**确定适合的maxTotal**

- 业务希望Redis并发量
- 客户端执行命令时间
- Redis资源:例如nodes(例如应用个数) * maxTotal是不能超过redis的最大连接数。(config get maxclients)
- 资源开销:例如虽然希望控制空闲连接，但是不希望因为连接池的频繁释放创建连接造成不必靠开销。

**适合的maxIdle和minIdle**

- 建议maxIdle = max Total
  - 减少创建新连接的开销。

- 建议预热minIdle
  - 减少第一次启动后的新连接开销。

### From 4.2

慢查询： //todo

**慢查询的生命周期视图**

<img src="https://i.loli.net/2020/08/14/NoemlvfYRU4xQip.png" style="zoom: 28%;" />

两点说明：

- 慢查询发生在第3阶段
- 客户端超时不一定慢查询，但慢查询是客户端超时的一个可能因素