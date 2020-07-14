# YAML语法

参考文档：https://www.runoob.com/w3cnote/yaml-intro.html

## 基础语法

+ 大小写敏感
+ 使用缩进表示层级关系
+ 缩进不允许使用tab，只允许空格
+ 缩进的空格数不重要，只要相同层级的元素左对齐即可
+ '#'表示注释

## 数据类型

YAML 支持以下几种数据类型：

1. 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
2. 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
3. 纯量（scalars）：单个的、不可再分的值

### 对象

对象键值对使用冒号结构表示 key: value，冒号后面要加一个空格。

也可以使用 key:{key1: value1, key2: value2, ...}。

还可以使用缩进表示层级关系；

```yml
key1: value1

key2:
    child-key1: value1
    child-key2: value2

key3: {child-key1: value1, child-key2: value2}
```

### 数组

一组连词线("-")开头的行，构成一个数组。

```yml
key1:
    - value1
    - value2
    - value3

key2: [value1, value2, value3]
```

### 纯量

纯量是最基本的，不可再分的值，包括：

+ 字符串
+ 布尔值
+ 整数
+ 浮点数
+ Null
+ 时间
+ 日期

```yml
boolean:
    - TRUE  #true,True都可以
    - FALSE  #false，False都可以
float:
    - 3.14
    - 6.8523015e+5  #可以使用科学计数法
int:
    - 123
    - 0b1010_0111_0100_1010_1110    #二进制表示
null:
    nodeName: null #可以写成null
    parent: ~  #使用~表示null
string:
    - 哈哈
    - 'Hello world'  #可以使用双引号或者单引号包裹特殊字符
    - newline
      newline2    #字符串可以拆成多行，每一行会被转化成一个空格
date:
    - 2018-02-17    #日期必须使用ISO 8601格式，即yyyy-MM-dd
datetime:
    -  2018-02-17T15:02:31+08:00    #时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
```

## 引用

& 锚点和 * 别名，可以用来引用:

&用来建立锚点（defaults），<<表示合并到当前数据，*用来引用锚点。

```yml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```

相当于：

```yml
defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost

test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```
