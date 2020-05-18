# Redis

[Redis中文官方](Redis中文官方)中的文档已经非常详细了，本文档只是用于熟悉python中的redis函数，用作记录

+ [Connection](#Connection)
+ [Hashes](#Hashes)

___

## Connection

数据库连接相关操作

命令|格式|描述|例
-|-|-|-
AUTH|AUTH password|验证服务器密码|AUTH password
ECHO|ECHO message|打印字符串|ECHO "Hello World"
PING|PING|ping 服务器，验证当前连接是否还可用|PING
QUIT|QUIT|关闭连接，退出|QUIT
SELECT|SELECT index|选择新的数据库|SELECT 1
SWAPDB|SWAPDB index index|交换同一Redis服务器上的两个DATABASE|SWAPDB 0 1

## Hashes

Redis hash 是一个 string 类型的 field 和 value 的映射表

例：key field1 value1 field2 value2
