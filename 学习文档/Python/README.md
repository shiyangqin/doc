# Python

Python官方文档：https://docs.python.org/zh-cn/3.8/

+ GIL(global interpreter lock) -- 全局解释器锁

python 的解释器，有很多种，但市场占有率99.9%的都是基于c语言编写的CPython.  在这个解释器里规定了GIL

GIL使无论有多少个cpu，python在执行时在同一时刻只允许一个线程运行。（一个进程可以开多个线程，但线程只能同时运行一个）

+ [queue模块](queue模块.md)

+ [threading模块](threading模块.md)

+ [multiprocessing模块](multiprocessing模块.md)
