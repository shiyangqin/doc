# Redis

[Redis中文官方](Redis中文官方)中的文档已经非常详细了，本文档只是用于熟悉python中的redis函数，用作记录

+ [Connection](#Connection)
+ [Hashes](#Hashes)

___

## Connection

数据库连接相关操作

命令|格式|描述|示例
-|-|-|-
AUTH|AUTH password|验证服务器密码|AUTH password
ECHO|ECHO message|打印字符串|ECHO "Hello World"
PING|PING|ping 服务器，验证当前连接是否还可用|PING
QUIT|QUIT|关闭连接，退出|QUIT
SELECT|SELECT index|选择新的数据库|SELECT 1
SWAPDB|SWAPDB index index|交换同一Redis服务器上的两个DATABASE|SWAPDB 0 1

```txt
root@6946b28aad4c:/data# redis-cli
127.0.0.1:6379> auth redis
OK
127.0.0.1:6379> echo 'hello world'
"hello world"
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> swapdb 0 1
OK
127.0.0.1:6379[1]> quit
root@6946b28aad4c:/data#
```

## Hashes

Redis hash 是一个 string 类型的 field 和 value 的映射表

例：key field1 value1 field2 value2

命令|格式|描述|例
-|-|-|-
HSET|HSET key field value|设置hash里一个字段的值|HSET k f 'v'
HMSET|HMSET key field value [field value ...]|设置hash字段值|HMSET k f1 'v1' f2 'v2'
HSETNX|HSETNX key field value|当field不存在时，设置hash里一个字段的值|HSETNX k f 'v'f
HGET|HGET key field|获取hash中一个field的值|HGET k f
HMGET|HMGET key field [field ...]|获取hash中field的值|HMGET k f1 f2 f3
HKEYS|HKEYS key|获取hash中所有字段|HKEYS k
HVALS|HVALS key|获取hash中所有值|HVALS k
HGETALL|HGETALL key|获取hash中所有字段和值|HGETALL k
HLEN|HLEN key|获取hash中field的数量|HLEN k
HSTRLEN|HSTRLEN key field|获取hash中指定field的value的字符串长度|HSTRLEN k f
HDEL|HDEL key field [field ...]|删除一个或多个hash的field|HDEL k f
HEXISTS|HEXISTS key field|判断field是否存在于hash中|HEXISTS k f
HINCRBY|HINCRBY key field increment|将hash中指定field的值增加给定数字|HINCRBY k f -5
HINCRBYFLOAT|HINCRBYFLOAT key field increment|将hash中指定field的值增加给定浮点数|HINCRBYFLOAT k f -3.5
HSCAN|HSCAN key cursor [MATCH pattern] [COUNT count]|参考 SCAN命令， HSCAN与之类似|略
