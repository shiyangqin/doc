# PostgreSQL

+ [使用docker部署PostgreSQL](#使用docker部署PostgreSQL)
+ [psql命令](#psql命令)

___

## 使用docker部署PostgreSQL

+ Dodkerfile

```Dodkerfile
FROM postgres:12.2

ENV POSTGRES_USER postgres
ENV POSTGRES_PASSWORD postgres
ENV POSTGRES_DB oa_data

ADD ./pg.sql /docker-entrypoint-initdb.d/

EXPOSE 5432
```

+ 创建数据卷

```shell
docker volume create pg_data
```

+ 构建镜像

```shell
docker build -t pg:test .
```

+ 运行容器

```shell
docker run -itd -p 5432:5432 -v pg_data:/var/lib/postgresql/data --name pg pg:test
```

-p: 指定映射端口;
-v: 绑定数据卷;
-e: 设置密码

### psql命令

+ 创建数据库oa_data

```shell
psql -U postgres -c "CREATE DATABASE oa_data;"
```

+ 指定数据库执行sql文件

```shell
 psql -U postgres -d oa_data -c "\i /opt/pg.sql"
```
