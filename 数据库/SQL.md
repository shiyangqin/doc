### 概述

个人现在所使用的关系型数据库为：PostgreSQL

+ [增](#增)

+ [删](#删)

+ [改](#改)

+ [查](#查)

### 增

+ 插入明确的值
```
insert into table_name(
    column_name,[,..]
) 
values(
    { expression | DEFAULT } [, ...]
)

例：insert into test(id, name, number) values(1, '测试', '6')
```

+ 插入查询的值
```
insert into table_name(
    column_name,[,..]
)
{query}

例：insert into test_1(id, name, number) select id, name, number from test where id=1
```

+ 直接拿现有表数据创建一个新表并填充
```
select <新建表列名> into <新建表名> from <源表名>
 
例：select id,name into test_2 from test
```

+ copy

在一个文件和一个表之间复制数据，个人暂时用不到，留个链接：http://www.postgres.cn/docs/11/sql-copy.html

### 删

+ delete：删除单表中数据
```
delete from table_name [WHERE condition]

例：delete from test_1 where id=1 and name='测试'
```

+ delete：删除多表中数据
```
PostgreSQL不支持一条delete语句删除多表中的数据，只能多次删，如果有外键关联，删的时候注意先删除底层数据表数据
```

+ drop：删除表
```
drop table table_name

例：drop table test_1
```

+ truncate：快速删除表中海量数据，保留表结构
```
truncate table table_name

例：truncate table test
```

### 改

+ update：
```
update table_name set { 
    column_name = { expression | DEFAULT } |
    ( column_name [, ...] ) = ( { expression | DEFAULT } [, ...] ) |
    ( column_name [, ...] ) = ( sub-SELECT )
}
[WHERE condition]

例：update test_1 set id=11,name='测试11',number='11' where id=1
例：update test_1 set (id,name,number) = (111, '测试111', '111') where id=11
例：update test_1 set (id,name,number) = (select id,name,number from test_2 where id=2) where id=11
```

### 查

查询是开发人员应用最多的，也是最重要的

distinct：去重
```
select distinct name from test
```

coalesce：当查询的字段为空时，给默认值
```
select coalesce(name, '默认名称') from test
```

as：别名
```
select a.id,a.name from test as a
```

order by：排序
```
select a.id,a.name from test as a order by a.id asc
select a.id,a.name from test as a order by a.id desc
select a.id,a.name from test as a order by a.id nulls first
select a.id,a.name from test as a order by a.id desc nulls last 

asc：正序，可省略
desc：倒序
nulls first：空值放在前
nulls last：空值放在后
```

group by：分组
```
select name from test group by name

查询test中所有的name，相同值合并。带有group by的查询，查询字段必须为group by的字段
```

offset，limit：分页
```
select a.id,a.name from test as a offset 10 limit 10

offset：忽略几行
limit：查询几行
offset 10 limit 10：忽略前10条记录，从第11条开始，向后查询10条记录
```

between and：取在两个参数中间的值
```
select id,name from test where id between 0 and 10
查询id为0~10的记录，包含0和10，不包含的情况请使用“<”、“>”
```

in：取在参数集合中的值
```
select id,name from test where id in (1,2)
查询id为1或2的记录
```

like：模糊匹配
```
select id,name from test where name like '%测%'
查询name值中包含“测”字的记录
```

inner join：内连接
```
inner join创建一个新的结果表，通过连接谓词，找到所有对满足连接谓词的行。

select * from test_1 as a inner join test_2 as b on a.id=b.id
```

left join：左外连接
```
left join先进行内联接,然后，如果表1中有一行并不满足连接条件的表2中的任何行，则连接以后保留表1中的行，该行对应的表2的列值用null填充。

select * from test_1 as a left join test_2 as b on a.id=b.id
```

union | union all：合并查询结果
```
union：合并SELECT语句的结果，不返回任何重复的行
union all：合并SELECT语句的结果，包括重复行

select * from test_1 where id=1
union
select * from test_1 where id=2
```

intersect | intersect all:查询两个结果集的交集
```
intersect：同时存在于结果集1和结果集2的结果中的行，所有重复的行会被消除
intersect all：同时存在于结果集1和结果集2的结果中的行，包括重复行

select * from test_1 where id=1
intersect
select * from test_1 where id=2
```

except | except all：查询在结果集1的结果中但是不在结果集2的结果中的行
```
except：返回所有在query1的结果中但是不在query2的结果中的行，所有重复行都被消除
except all：返回所有在query1的结果中但是不在query2的结果中的行，包括重复行
```

test表中有id、pid两个字段，pid指父id，test中的数据根据父级关系会组成一个树，则递归查询某个id=x下的所有子id
```
with recursive cte as (
    (
        select
            id,
            pid
        from
            test
        where
            id = x
    )
    union all
    (
        select
            k.id,
            k.pid
        from
            test as k
        inner join cte as c on c.id = k.pid
    )
) select id from cte
```

conflict：表中存在主键或具有唯一性字段的情况下，插入数据时，若主键不存在则插入，存在则进行其他操作
```
主键存在时更新其他信息：
insert into test(
    id, 
    name, 
    r_time
) values (
    1,
    '测试',
    '2020-04-15 14:22:46'
) on conflict(id) do update set 
    name='测试',
    r_time='2020-04-15 14:22:46'

主键存在时不做其他处理：
insert into test(
    id, 
    name, 
    r_time
) values (
    1,
    '测试',
    '2020-04-15 14:22:46'
) on conflict(id) do nothing
```
