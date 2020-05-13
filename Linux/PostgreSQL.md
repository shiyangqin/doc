+ [使用docker部署PostgreSQL](#使用docker部署PostgreSQL)
+ [psql命令](#psql命令)

### 使用docker部署PostgreSQL

+ 下载官方镜像
```
docker pull postgres:12.2
```

+ 创建数据卷
```
docker volume create pg_data
```

+ 运行容器
```
docker run -itd -p 5432:5432 -v pg_data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres --name pg postgres:12.2

-p: 指定映射端口
-v: 绑定数据卷
-e: 设置密码
```

+ shell
```
tee ./pg_deploy.sh <<-'EOF'
#!/bin/bash
docker pull postgres:12.2
docker volume create pg_data
docker run -itd -p 5432:5432 -v pg_data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres --name pg postgres:12.2
EOF
sh ./pg_deploy.sh
rm -rf pg_deploy.sh
```

### psql命令

+ 创建数据库oa_data
```
psql -U postgres -c "CREATE DATABASE oa_data;"
```

+ 指定数据库执行sql文件
```
 psql -U postgres -d oa_data -c "\i /opt/pg.sql"
```
