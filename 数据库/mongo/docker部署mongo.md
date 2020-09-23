# docker部署mongo

+ 拉取镜像

```shell
docker pull mongo:4.2.9
```

+ 创建容器

```shell
docker run -p 27017:27017 -v $PWD/db:/data/db -d --name mongo mongo:4.2.9 --auth
```

+ 创建admin用户

```shell
docker exec -it  mongo  mongo admin
db.createUser({ user: 'admin', pwd: '123456', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
exit
```

输出：

```shell
[root@10 mongo]# docker exec -it  mongo  mongo admin
MongoDB shell version v4.2.9
connecting to: mongodb://127.0.0.1:27017/admin?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("3c6bd906-a9d0-4bae-b033-1461aa3edd57") }
MongoDB server version: 4.2.9
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
        https://community.mongodb.com
> db.createUser({ user: 'admin', pwd: '123456', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
Successfully added user: {
        "user" : "admin",
        "roles" : [
                {
                        "role" : "userAdminAnyDatabase",
                        "db" : "admin"
                }
        ]
}
> exit
bye
[root@10 mongo]#
```

+ 创建普通用户

```shell
docker exec -it  mongo  mongo admin
db.auth("admin","123456");
db.createUser({ user: 'app', pwd: '123456', roles: [ { role: "readWrite", db: "app" } ] });
exit
```

+ 使用app用户登录app数据库

```shell
docker exec -it  mongo  mongo admin
db.auth("app","123456");
use app
```
