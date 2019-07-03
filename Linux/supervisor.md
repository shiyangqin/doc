### supervisor安装
```
yum install python-setuptools
easy_install supervisor
```
### 配置Supervisor
```
创建文件夹：
mkdir /etc/supervisor

生成配置文件：
echo_supervisord_conf > /etc/supervisor/supervisord.conf

修改配置文件：
vim /etc/supervisor/supervisord.conf
修改最后两行，去掉前面的分号
[include]
files = ./*.ini

编写自己的配置文件：
cd /etc/supervisor
vim sap.ini

sap.ini示例：
[program:run]
directory=/opt/acd_response
command=/bin/python /opt/acd_response/run.py
autostart=true
stopwaitsecs=3600
```

### 启动和关闭
```
启动：
supervisord -c /etc/supervisor/supervisord.conf
关闭：
ps -ef | grep super，然后kill -9 杀进程
```

### 常用命令
```
supervisorctl status：查看所有进程的状态
supervisorctl stop es/all：停止es/停止所有进程
supervisorctl start es：启动es/启动所有进程
supervisorctl restart es: 重启es/重启所有进程
supervisorctl update ：配置文件修改后可以使用该命令加载新的配置
supervisorctl reload: 重新启动配置中的所有程序
```