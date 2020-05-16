# RESTful API

REST全称是Representational State Transfer，直接翻译的意思是"表现层状态转化"。。它是一种互联网应用程序的API设计理念：URL定位资源，用HTTP动词（GET，POST，PUT，PATCH，DELETE）描述操作。

核心：

1. 资源定位
2. 资源操作

## URI

统一资源标识符，服务器上每一种资源，比如文档、图像、视频片段、程序 都由一个通用资源标识符（Uniform Resource Identifier, 简称"URI"）进行定位。

规范：

1. 不用大写
2. 用中杠-不用下杠_
3. 参数列表要encode
4. URI中的名词表示资源集合，使用复数形式

## HTTP动词

常用的HTTP动词有下面五个：

1. GET：获取资源（一项或多项）
2. POST：新建一个资源
3. PUT：更新资源（客户端提供改变后的完整资源）
4. PATCH：更新资源（客户端提供改变的属性）
5. DELETE：删除资源

简单来说就是url地址中只包含名词表示资源，使用http动词表示动作进行操作资源

如果某些动作是HTTP动词表示不了的，你就应该把动作做成一种资源。比如网上汇款，从账户1向账户2汇款500元，正确的写法是把动词transfer改成名词transaction，资源不能是动词，但是可以是一种服务

## 安全性和幂等性

安全性：不会改变资源状态，可以理解为只读的；

幂等性：执行1次和执行N次，对资源状态改变的效果是等价的。

HTTP|安全性|幂等性
-|-|-
GET|√|√
POST|×|×
PUT|×|√
PATCH|×|√
DELETE|×|√

## 例

我没有具体设计过

```txt
GET     /users          # 获取全部用户
GET     /users/id       # 获取指定id的用户
POST    /users          # 新建用户
PUT     /users/id       # 更新指定id的用户信息（参数为账户的全部信息）
PATCH   /users/id       # 更新指定id的用户信息（参数为账户的部分信息）
DELETE  /users/id       # 删除指定id的用户信息
```

GitHub的[API v3](https://developer.github.com/v3/)是RESTful API的完整实现，可以看一看。
