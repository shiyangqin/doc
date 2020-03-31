+ [supervisor安装](#supervisor安装)
+ [配置supervisor](#配置supervisor)
+ [常用命令](#常用命令)


### supervisor安装
```
yum install -y epel-release
yum install -y supervisor
systemctl enable supervisord 设置开机启动
```

### 配置supervisor
```
配置文件：/etc/supervisord.conf

/etc/supervisord.conf最后引入了/etc/supervisord.d/*.ini
我们只需要在/etc/supervisord.d/下新建自己的ini文件并进行配置，就可以直接使用了

编写自己的配置文件：
vim /etc/supervisord.d/sap.ini

sap.ini示例：
[program:run]
directory=/opt/acd_response
command=/bin/python /opt/acd_response/run.py
autostart=true
stopwaitsecs=3600

启动：
supervisord -c /etc/supervisord.conf
```

### 常用命令
```
supervisorctl status：查看所有进程的状态
supervisorctl start 进程名：启动进程
supervisorctl stop 进程名：停止进程
supervisorctl restart 进程名：重启进程
supervisorctl update ：重新载入配置文件
supervisorctl shutdown 关闭supervisord
```
