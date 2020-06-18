# pip安装psycopy2报错问题解决方案

错误信息：

```txt
pip3 install psycopy2 Error: pg_config executable not found.
```

解决方案1：

```txt
pip3 install psycopg2-binary
```

解决方案2：

```txt
yum install python-devel postgresql-devel
pip3 install psycopg2
```
