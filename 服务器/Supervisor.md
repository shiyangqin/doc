# supervisor

+ [supervisor安装](#supervisor安装)
+ [配置supervisor](#配置supervisor)
+ [常用命令](#常用命令)

___

## supervisor安装

```shell
yum install -y epel-release
yum install -y supervisor
systemctl enable supervisord
```

## 配置supervisor

配置文件路径：/etc/supervisord.conf

/etc/supervisord.conf最后引入了/etc/supervisord.d/*.ini;我们只需要在/etc/supervisord.d/下新建自己的ini文件并进行配置，就可以直接使用了

自己的配置文件:/etc/supervisord.d/sap.ini

```ini
[program:run]  # 进程名称
directory=/opt/app  # 工作目录
command=python3 app.py  # 启动命令
autostart=true  # 随supervisor启动而启动
startsecs=1  # 启动10秒后没有异常退出，就表示进程正常启动了，默认为1秒
autorestart=true  # 程序退出后自动重启
startretries=3  # 启动失败自动重试次数，默认是3
user=root  # 用哪个用户启动进程
priority=999  # 进程优先级，优先级低的program，将会先启动后关闭
redirect_stderr=false  # 把stderr重定向到stdout，默认false
stdout_logfile=/opt/app/app.supervisor.log  # supervisord的标准输出日志文件路径
stdout_logfile_maxbytes=20MB  # 日志文件大小，默认50MB
stdout_logfile_backups=20  # 日志文件备份数，默认是10
stderr_logfile=/opt/app/app.log # 进程标准输出日志文件路径
stderr_logfile_maxbytes=10MB  # 日志文件大小
stderr_logfile_backups=10  # 日志文件备份数
```

启动：

```shell
supervisord -c /etc/supervisord.conf
```

## 常用命令

```txt
supervisorctl status：查看所有进程的状态
supervisorctl start 进程名：启动进程
supervisorctl stop 进程名：停止进程
supervisorctl restart 进程名：重启进程
supervisorctl update ：重新载入配置文件
supervisorctl shutdown 关闭supervisord
```
