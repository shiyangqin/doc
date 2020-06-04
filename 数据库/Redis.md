# Redis

[Redis中文官方](http://www.redis.cn/)中的文档已经非常详细了，本文档只是用于熟悉redis函数，用作个人快速查找函数，具体的函数说明请前往官网查看

+ [Connection(连接)](#Connection)
+ [Keys(键)](#Keys)
+ [Strings(字符串)](#Strings)
+ [Hashes(哈希)](#Hashes)
+ [Lists(列表)](#Lists)
+ [Set(集合)](#Set)
+ [Sorted Set(有序集合)](#SortedSet)
+ [HyperLogLog](#HyperLogLog)
+ [Pub/Sub(发布订阅)](#PubSub)

___

## Connection

数据库连接相关操作

序号|命令|示例
-|-|-
1|AUTH password<br>验证服务器密码|AUTH password
2|ECHO message<br>打印字符串|ECHO "Hello World"
3|PING<br>ping 服务器，验证当前连接是否还可用|PING
4|QUIT<br>关闭连接，退出|QUIT
5|SELECT index<br>选择新的数据库|SELECT 1
6|SWAPDB index index<br>交换同一 Redis 服务器上的两个DATABASE|SWAPDB 0 1

## Keys

对key进行操作

序号|命令|示例
-|-|-
1|DEL key [key ...]<br>删除指定的一批 keys|DEL k1 k2 k3
2|DUMP key<br>序列化给定 key，并返回被序列化的值|DUMP k
3|EXISTS key [key ...]<br>返回key是否存在|EXISTS k1 k2
4|EXPIRE key seconds<br>设置key的过期秒数|EXPIRE k1 60
5|EXPIREAT key timestamp<br>给key设置一个时间戳为过期时间|EXPIREAT k1 1293840000
6|KEYS pattern<br>查找所有符合正则表达式的 key|KEYS t??
7|MIGRATE host port key destination-db timeout [COPY] [REPLACE]<br>将key从一个实例移到另一个实例|MIGRATE 127.0.0.1 7777 greeting 0 1000
8|MOVE key db<br>移动一个key到另一个库|MOVE k 1
9|OBJECT subcommand [arguments [arguments ...]]<br>检查内部的再分配对象|object refcount k<br>object encoding k<br>object idletime k
10|PERSIST key<br>移除给定key的生存时间|PERSIST k
11|PEXPIRE key milliseconds<br>以毫秒为单位设置 key 的生存时间|PEXPIRE k 10000
12|PEXPIREAT key milliseconds-timestamp<br>以毫秒为单位设置 key 的过期 unix 时间戳|PEXPIREAT k 1555555555005
13|PTTL key<br>以毫秒为单位返回 key 的剩余生存时间|PTTL k
14|RANDOMKEY<br>从当前数据库返回一个随机的key|RANDOMKEY
15|RENAME key newkey<br>将key重命名为newkey|RENAME k nk
16|RENAMENX key newkey<br>当且仅当 newkey 不存在时，将 key 改名为 newkey|RENAME k nk
17|RESTORE key ttl serialized-value [REPLACE]<br>反序列化给定的序列化值，并将它和给定的 key 关联|RESTORE k 0 ""
18|key [BY pattern] [LIMIT offset count] [GET pattern] [ASC\|DESC] [ALPHA] destination<br>对list、 set 或sorted set进行排序|SORT mylist LIMIT 0 5 ALPHA DESC
19|TTL key<br>返回key剩余的过期时间，单位：秒|TTL k
20|TYPE key<br>返回key所存储的value的数据结构类型|TYPE k
21|WAIT numslaves timeout<br>阻塞当前客户端|WAIT 2 1000
22|SCAN cursor [MATCH pattern] [COUNT count]<br>迭代元素|scan 0

## Strings

Redis Strings 数据类型的相关命令用于管理 redis 字符串值

序号|命令|示例
-|-|-
1|APPEND key value<br>在key的值上追加一个值|APPEND k v2
2|BITCOUNT key [start end]<br>统计指定起始和结束位置的字节数|BITCOUNT k 0 0
3|BITFIELD<br>操作字节|略
4|BITOP<br>操作字节|略
5|BITPOS<br>操作字节|略
6|DECR key<br>对key对应的数字做减1操作|DECR k
7|DECRBY key decrement<br>将key对应的数字减decrement|DECRBY k 3
8|GET key<br>返回key的值|GET k
9|GETBIT key offset<br>返回key对应的string在offset处的bit值|GETBIT k 0
10|GETRANGE key start end<br>返回value的子串（start和end都在string内）|GETRANGE k 0 3
11|GETSET key value<br>设置新值，返回旧值|GETSET k 3
12|INCR key<br>对存储在指定key的数值执行原子的加1操作|INCR k
13|INCRBY key increment<br>将key对应的数字加decrement|INCRBY k 3
14|INCRBYFLOAT key increment<br>将key对应的浮点数加decrement|INCRBYFLOAT k 0.3
15|MGET key [key ...]<br>MGET key [key ...]|MGET k1 k2 k3
16|MSET key value [key value ...]<br>设置多个key value|MSET k1 v1 k2 v2 k3 v3
17|MSETNX key value [key value ...]<br>当key不存在时，设置多个key value|MSETNX key1 "Hello" key2 "there"
18|PSETEX key milliseconds value<br>设置key value，并且设置key在指定毫秒之后超时过期|PSETEX k 1000
19|SET key value [EX seconds] [PX milliseconds] [NX\|XX]<br>将键key设定为指定的“字符串”值|SET k "Hello"
20|SETBIT key offset value<br>设置或者清空key的value(字符串)在offset处的bit值|SETBIT mykey 7 0
21|SETEX key seconds value<br>设置key value，并且设置key在指定秒数之后超时过期|SETEX k 60 v
22|SETNX key value<br>当key不存在，设置key value|SETNX k v
23|SETRANGE key offset value<br>覆盖key对应的string的一部分|SETRANGE k 6 "Redis"
24|STRLEN key<br>获取字符串长度|STRLEN k

## Hashes

Redis hash 是一个 string 类型的 field 和 value 的映射表

序号|命令|示例
-|-|-
1|HSET key field value<br>设置 hash 里一个字段的值|HSET k f 'v'
2|HMSET key field value [field value ...]<br>设置 hash 字段值|HMSET k f1 'v1' f2 'v2'
3|HSETNX key field value<br>当 field 不存在时，设置 hash 里一个字段的值|HSETNX k f 'v'f
4|HGET key field<br>获取 hash 中一个 field 的值|HGET k f
5|HMGET key field [field ...]<br>获取 hash 中 field 的值|HMGET k f1 f2 f3
6|HKEYS key<br>获取 hash 中所有字段|HKEYS k
7|HVALS key<br>获取 hash 中所有值|HVALS k
8|HGETALL key<br>获取 hash 中所有字段和值|HGETALL k
9|HLEN key<br>获取 hash 中 field 的数量|HLEN k
10|HSTRLEN key field<br>获取 hash 中指定 field 的 value 的字符串长度|HSTRLEN k f
11|HDEL key field [field ...]<br>删除一个或多个 hash 的 field|HDEL k f
12|HEXISTS key field<br>判断 field 是否存在于 hash 中|HEXISTS k f
13|HINCRBY key field increment<br>将 hash 中指定 field 的值增加给定数字|HINCRBY k f -5
14|HINCRBYFLOAT key field increment<br>将 hash 中指定 field 的值增加给定浮点数|HINCRBYFLOAT k f -3.5
15|HSCAN key cursor [MATCH pattern] [COUNT count]<br>参考 SCAN 命令， HSCAN与之类似|略

## Lists

Lists 是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部或者尾部

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

序号|命令|示例
-|-|-
1|BLPOP key [key ...] timeout<br>删除并获取列表第一个元素，或阻塞到有一个可用|BLPOP L1 L2 L3 0
2|BRPOP key [key ...] timeout<br>删除并获取列表最后一个元素，或阻塞到有一个可用|BRPOP L1 L2 L3 0
3|RPOPLPUSH source destination<br>弹出 source 的尾部元素，放入 destination 列表头部|RPOPLPUSH L1 L2
4|BRPOPLPUSH source destination timeout<br>RPOPLPUSH的阻塞版本|BRPOPLPUSH L1 L2 0
5|LINDEX key index<br>获取列表指定位置元素|LINDEX L 0
6|LINSERT key BEFORE\|AFTER pivot value<br>把 value 插入到列表中在 pivot 的前面或后面|LINSERT L BEFORE "World" "Hello"
7|LLEN key<br>获取列表长度|LLEN L
8|LPOP key<br>移除并返回列表第一个元素|LPOP L
9|LPUSH key value [value ...]<br>在列表头部插入一个或多个值|LPUSH L "hello"
10|LPUSHX key value<br>当列表存在时，在列表头部插入值|LPUSHX L "Hello"
11|LRANGE key start end<br>从列表钟获取指定返回的元素|LRANGE L 0 0
12|LREM key count value<br>从列表钟移除第count次出现的元素|LREM L -2 "hello"
13|LSET key index value<br>设置指定位置元素|LSET L 0 "one"
14|LTRIM key start stop<br>修剪列表|LTRIM mylist 1 -1
15|RPOP key<br>移除并返回存于 key 的 list 的最后一个元素|RPOP L
16|RPUSH key value [value ...]<br>从列表尾部插入元素|RPUSH key value [value ...]
17|RPUSHX key value<br>当 key 存在并且是一个列表时，将值 value 插入到列表 key 的表尾|RPUSHX mylist "World"

## Set

Set 是 String 类型的无序集合，且不允许重复的元素

Set 是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)

集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

序号|命令|示例
-|-|-
1|SADD key member [member ...]<br>添加一个或多个元素到set里|SADD s "Hello"
2|SCARD key<br>返回集合元素个数|SCARD s
3|SDIFF key [key ...]<br>返回一个集合与给定集合的差集的元素|SDIFF s1 s2
4|SDIFFSTORE destination key [key ...]<br>将一个集合与给定集合的差集的元素放在destination里|SDIFFSTORE s s1 s2
5|SINTER key [key ...]<br>返回指定所有的集合的成员的交集|SINTER s1 s2
6|SINTERSTORE destination key [key ...]<br>将指定所有的集合的成员的交集放在destination里|SINTERSTORE s s1 s2
7|SISMEMBER key member<br>返回 member 是否在集合里|SISMEMBER s "one"
8|SMEMBERS key<br>返回集合所有元素|SMEMBERS s
9|SMOVE source destination member<br>将member从source集合移动到destination集合中|SMOVE s1 s2 "two"
10|SPOP key [count]<br>从存储在key的集合中移除并返回一个或多个随机元素|SPOP myset 3
11|SRANDMEMBER key [count]<br>返回一个或多个随机元素|SRANDMEMBER myset 2
12|SREM key member [member ...]<br>在key集合中移除指定的元素|SREM s "one"
13|SSCAN key cursor [MATCH pattern] [COUNT count]|参考SCAN
14|SUNION key [key ...]<br>返回多个集合的并集|SUNION s1 s2 s3
15|SUNIONSTORE destination key [key ...]<br>将多个set的并集存在destination中|SUNIONSTORE s s1 s2

## SortedSet

Sorted Set 是string类型元素的集合,且不允许重复的元素

每个元素都会关联一个double类型的分数。redis通过分数来为集合中的成员进行从小到大的排序

有序集合的成员是唯一的,但分数(score)却可以重复。

集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)

序号|命令|示例
-|-|-
1|ZADD key score member [score member ...]<br>向有序集合添加一个或多个成员，或者更新已存在成员的分数|ZADD zs 1 1
2|ZCARD key<br>返回key的有序集元素个数|ZCARD zs
3|ZCOUNT key min max<br>返回有序集key中，score值在min和max之间(默认包括score值等于min或max)|ZCOUNT myzset 1 3
4|ZINCRBY key increment member<br>将set中member的score增加increment|ZINCRBY zs -1 '1'
5|ZINTERSTORE destination numkeys key [key ...]<br>计算给定的numkeys个有序集合的交集，并且把结果放到destination中|ZINTERSTORE zs 2 zs1 zs2
6|ZLEXCOUNT key min max<br>ZLEXCOUNT 命令用于计算有序集合中指定成员之间的成员数量|ZLEXCOUNT myzset "start" "end"
7|ZPOPMAX key [count]<br>删除并返回有序集合key中的最多count个具有最高得分的成员|ZPOPMAX myzset 3
8|ZPOPMIN key [count]<br>删除并返回有序集合key中的最多count个具有最低得分的成员|ZPOPMIN myzset 3
9|ZRANGE key start stop [WITHSCORES]<br>返回存储在有序集合key中的指定范围的元素|ZRANGE zs 0 -1 WITHSCORES
10|ZRANGEBYLEX key min max [LIMIT offset count]<br>返回指定成员区间内的成员|ZRANGEBYLEX zs - +
11|ZRANK key member<br>返回有序集合中member的排名|ZRANK zs "one"
12|ZREM key member [member ...]<br>移除有序集合中的元素|ZREM zs "one"
13|ZREMRANGEBYLEX key min max<br>删除名称按字典由低到高排序成员之间所有成员|ZREMRANGEBYLEX zs - +
14|ZREMRANGEBYRANK key start stop<br>移除有序集中，指定排名区间内的所有成员|ZREMRANGEBYRANK zs 0 1
15|ZREMRANGEBYSCORE key min max<br>移除有序集中所有score值介于min和max之间(包括等于min或max)的成员|ZREMRANGEBYSCORE zs 0 3
16|ZREVRANGE key start stop [WITHSCORES]<br>返回有序集中，指定区间内的成员且按score值递减排列|ZREVRANGE zs 0 -1 WITHSCORES
17|ZREVRANGEBYLEX key max min [LIMIT offset count]<br>返回指定成员区间内的成员，按成员字典倒序排序, 分数必须相同|ZREVRANGEBYLEX zs + -
18|ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]<br>返回有序集合中指定分数区间内的成员，分数由高到低排序|ZREVRANGEBYSCORE myzset +inf -inf
19|ZREVRANK key member<br>返回有序集合中member的排名|ZREVRANK zs "one"
20|ZSCAN key cursor [MATCH pattern] [COUNT count]|参考SCAN
21|ZSCORE key member<br>返回有序集key中，成员member的score值|ZSCORE zs "one"
22|ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight] [SUM\|MIN\|MAX]<br>计算给定的numkeys个有序集合的并集，并且把结果放到destination中|ZUNIONSTORE out 2 zset1 zset2
23|BZPOPMAX key [key ...] timeout<br>返回第一个非空key中分数最大的成员和对应的分数|BZPOPMAX zset1 zset2 0
24|BZPOPMIN key [key ...] timeout<br>返回第一个非空key中分数最小的成员和对应的分数|BZPOPMIN zset1 zset2 0

## HyperLogLog

HyperLogLog 用来做基数统计的算法

序号|命令|示例
-|-|-
1|PFADD key element [element ...]<br>向HyperLogLog中添加基数|PFADD hll a b c d e f g
2|PFCOUNT key [key ...]<br>返回给定 HyperLogLog 的基数估算值|PFCOUNT hll
3|PFMERGE destkey sourcekey [sourcekey ...]<br>将多个 HyperLogLog 合并为一个 HyperLogLog|PFMERGE hll hll1 hll2

## PubSub
