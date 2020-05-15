# Doc

+ 基础
   + [算法](基础/算法/home.md)
   + [数据结构](基础/数据结构/home.md)
   + [设计模式](基础/设计模式/home.md)
   + [Git/Github](基础/Git.md)

+ python
   + [flask代码实践](https://github.com/shiyangqin/Qinsy/tree/master/flask_test)

+ 数据库
   + [SQL](数据库/SQL.md)
   + [事务](数据库/事务.md)
   + [使用er图设计数据库表结构](数据库/使用er图设计创建数据库.md)

+ 编程思想
   + [代码大全](编程思想/代码大全/代码大全.md)

+ Linux
   + [Python](Linux/Python.md)
   + [docker](Linux/docker.md)
   + [nginx](Linux/nginx.md)
   + [supervisor](Linux/supervisor.md)
   + [gunicorn](Linux/gunicorn.md)
   + [redis](Linux/redis.md)
   + [PostgreSQL](Linux/PostgreSQL.md)
   + [使用docker+supervisor+nginx+flask+gunicorn部署web项目](Linux/使用docker+supervisor+nginx+flask+gunicorn部署web项目.md)

### 个人日常所遇问题记录

+ GitHub图片无法显示问题解决

此方法有效，如果失效，自行百度新的地址。2020.3.25

```
打开C:\Windows\System32\drivers\etc\hosts
在最后添加：

# GitHub Start 
192.30.253.112    github.com 
192.30.253.119    gist.github.com
151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
151.101.184.133    gist.githubusercontent.com
151.101.184.133    cloud.githubusercontent.com
151.101.184.133    camo.githubusercontent.com
151.101.184.133    avatars0.githubusercontent.com
151.101.184.133    avatars1.githubusercontent.com
151.101.184.133    avatars2.githubusercontent.com
151.101.184.133    avatars3.githubusercontent.com
151.101.184.133    avatars4.githubusercontent.com
151.101.184.133    avatars5.githubusercontent.com
151.101.184.133    avatars6.githubusercontent.com
151.101.184.133    avatars7.githubusercontent.com
151.101.184.133    avatars8.githubusercontent.com
# GitHub End
```

+ pip安装psycopy2报错问题解决方案
```
pip3 install psycopy2 Error: pg_config executable not found.

解决方案1：
pip3 install psycopg2-binary

解决方案2：
yum install python-devel postgresql-devel
pip3 install psycopg2
```

+ 在Windows上编写的shell文件上传到linux下执行报错
```
Windows上文件格式是dos，linux上是unix，需要手动修改

使用vi/vim可以修改脚本的格式

1、vim 打开文件

2、使用 shift + ：进入命令行模式 

3、输入 set ff 会发现 输出的 fileformat = dos 

4、命令行模式下使用 set ff = unix 即可修复问题

也可以使用命令直接在linux上创建文件然后执行:
tee ./shell.sh <<-'EOF'
shell脚本内容
EOF
sh ./shell.sh
```

+ python3.7调用kafka包报错
```
python3.7新增关键字：async、await
kafka-python 用到了关键字async，产生了冲突

解决方案1：
将kafka-python中的async改名

解决方案2：
更换python版本
```