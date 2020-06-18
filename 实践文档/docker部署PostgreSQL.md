# docker部署PostgreSQL

+ Dodkerfile

POSTGRES_USER：用户名
POSTGRES_PASSWORD：密码
POSTGRES_DB：数据库名

初始化sql文件：./pg.sql

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
docker build -t pg:12.2 .
```

+ 运行容器

```shell
docker run -itd -p 5432:5432 -v pg_data:/var/lib/postgresql/data --name pg pg:12.2
```
