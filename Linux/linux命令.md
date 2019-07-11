+ 后台运行python，丢掉所有输出，这里丢掉所有输出因为有日志输出了
```
nohup python run.py >/dev/null &
```

+ 显示行数,切换到某一行
```
:set nu
:行数
```

+ 动态查看日志信息

```
tail -f filename
```