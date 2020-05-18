# Redis

[Redis中文官方](Redis中文官方)中的文档已经非常详细了，本文档只是用于熟悉python中的redis函数，用作个人快速查找函数，具体的函数说明请前往官网查看

+ [Connection(连接相关操作)](#Connection)
+ [Keys(键相关操作)](#Keys)
+ [Hashes(哈希相关操作)](#Hashes)

___

## Connection

数据库连接相关操作

序号|命令|示例
-|-|-
1|AUTH password<br>验证服务器密码<br>|AUTH password
2|ECHO message<br>打印字符串<br>|ECHO "Hello World"
3|PING<br>ping 服务器，验证当前连接是否还可用<br>|PING
4|QUIT<br>关闭连接，退出<br>|QUIT
5|SELECT index<br>选择新的数据库<br>|SELECT 1
6|SWAPDB index index<br>交换同一 Redis 服务器上的两个DATABASE<br>|SWAPDB 0 1

## Keys

对key进行操作

序号|命令|示例
-|-|-
1|DEL key [key ...]<br>删除指定的一批 keys<br>|DEL k1 k2 k3
2|DUMP key<br>序列化给定 key，并返回被序列化的值<br>|DUMP k

## Hashes

Redis hash 是一个 string 类型的 field 和 value 的映射表

|key field1 value1 field2 value2

序号|命令|示例
-|-|-
1|HSET key field value<br>设置 hash 里一个字段的值<br>|HSET k f 'v'
2|HMSET key field value [field value ...]<br>设置 hash 字段值<br>|HMSET k f1 'v1' f2 'v2'
3|HSETNX key field value<br>当 field 不存在时，设置 hash 里一个字段的值<br>|HSETNX k f 'v'f
4|HGET key field<br>获取 hash 中一个 field 的值<br>|HGET k f
5|HMGET key field [field ...]<br>获取 hash 中 field 的值<br>|HMGET k f1 f2 f3
6|HKEYS key<br>获取 hash 中所有字段<br>|HKEYS k
7|HVALS key<br>获取 hash 中所有值<br>|HVALS k
8|HGETALL key<br>获取 hash 中所有字段和值<br>|HGETALL k
9|HLEN key<br>获取 hash 中 field 的数量<br>|HLEN k
10|HSTRLEN key field<br>获取 hash 中指定 field 的 value 的字符串长度<br>|HSTRLEN k f
11|HDEL key field [field ...]<br>删除一个或多个 hash 的 field<br>|HDEL k f
12|HEXISTS key field<br>判断 field 是否存在于 hash 中<br>|HEXISTS k f
13|HINCRBY key field increment<br>将 hash 中指定 field 的值增加给定数字<br>|HINCRBY k f -5
14|HINCRBYFLOAT key field increment<br>将 hash 中指定 field 的值增加给定浮点数<br>|HINCRBYFLOAT k f -3.5
15|HSCAN key cursor [MATCH pattern] [COUNT count]<br>参考 SCAN 命令， HSCAN与之类似|略
