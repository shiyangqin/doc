+ 后台运行python，丢掉所有输出，这里丢掉所有输出因为有日志输出了
```
nohup python run.py >/dev/null &
```

+ 后台运行python，将输出信息写在out.log文件里
```
nohup python -u run.py > out.log 2>&1 &
```

+ 显示行数,切换到某一行
```
:set nu
：行数
```

+ 进程管理工具supervisor
```
yum install supervisor
修改配置文件
启动
```