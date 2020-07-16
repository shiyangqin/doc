# Python

+ 安装
  + [Centos7安装python3](Centos7安装python3.md)

+ 模块学习，Python官方文档：https://docs.python.org/zh-cn/
  + [queue](queue.md)
  + [threading](threading.md)
  + [multiprocessing](multiprocessing.md)
  + [concurrent](concurrent.md)
  + [asyncio](asyncio.md)

## 零散记录

+ GIL(global interpreter lock) -- 全局解释器锁

    python 的解释器，有很多种，但市场占有率99.9%的都是基于c语言编写的CPython.  在这个解释器里规定了GIL

    GIL使无论有多少个cpu，python在执行时在同一时刻只允许一个线程运行。（一个进程可以开多个线程，但线程只能同时运行一个）

+ python六个标准的数据类型

  1. number，包括：int、float、bool、complex
  2. string
  3. tuple
  4. set
  5. list
  6. dict
  
+ 可变对象和不可变对象

  可变对象与不可变对象的区别在于对象本身是否可变。不可变对象变化时，属于重新赋值，变量指向的地址改变，可变对象变化时，是修改对象本身，地址不变。可变对象作为参数传入时，在函数中对其本身进行修改，是会影响到全局中的这个变量值的，因为函数直接对该地址的值进行了修改。不可变对象作为参数时，在函数中修改不会改变全局对象值，重新指向一个新的地址

  不可变对象：number、string、tuple

  可变对象：set、list、dict
