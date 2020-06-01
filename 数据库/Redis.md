# Redis

[Redis中文官方](http://www.redis.cn/)中的文档已经非常详细了，本文档只是用于熟悉python中的redis函数，用作个人快速查找函数，具体的函数说明请前往官网查看

+ [Connection(连接相关操作)](#Connection)
+ [Keys(键相关操作)](#Keys)
+ [Strings(字符串相关操作)](#Strings)
+ [Hashes(哈希相关操作)](#Hashes)
+ [Lists(列表相关操作)](#Lists)

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

数据格式：key value

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

数据格式：key field1 value1 field2 value2

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

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

数据格式：["1", "2"]

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
11|LRANGE key start stop<br>