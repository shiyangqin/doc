# Redis

[Redis中文官方](http://www.redis.cn/)中的文档已经非常详细了，本文档只是用于熟悉python中的redis函数，用作个人快速查找函数，具体的函数说明请前往官网查看

+ [Connection(连接相关操作)](#Connection)
+ [Keys(键相关操作)](#Keys)
+ [Hashes(哈希相关操作)](#Hashes)

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

## Hashes

Redis hash 是一个 string 类型的 field 和 value 的映射表

|key field1 value1 field2 value2

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
