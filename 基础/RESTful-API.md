# RESTful API

REST全称是Representational State Transfer，直接翻译的意思是"表现层状态转化"。。它是一种互联网应用程序的API设计理念：URL定位资源，用HTTP动词（GET，POST，PUT，PATCH，DELETE）描述操作。

## URI

即统一资源标识符，服务器上每一种资源，比如文档、图像、视频片段、程序 都由一个通用资源标识符（Uniform Resource Identifier, 简称"URI"）进行定位。

## HTTP动词

常用的HTTP动词有下面五个：

1. GET：获取资源（一项或多项）
2. POST：新建一个资源
3. PUT：更新资源（客户端提供改变后的完整资源）
4. PATCH：更新资源（客户端提供改变的属性）
5. DELETE：删除资源

简单来说就是url地址中只包含名词表示资源，使用http动词表示动作进行操作资源
