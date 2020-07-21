# Python

+ 安装
  + [Centos7安装python3](Centos7安装python3.md)

+ [Python垃圾回收机制](Python垃圾回收机制.md)

+ 模块学习，Python官方文档：https://docs.python.org/zh-cn/
  + [queue](queue.md)
  + [threading](threading.md)
  + [multiprocessing](multiprocessing.md)
  + [concurrent](concurrent.md)
  + [asyncio](asyncio.md)

## 零散记录

### GIL(global interpreter lock) -- 全局解释器锁

python 的解释器，有很多种，但市场占有率99.9%的都是基于c语言编写的CPython.  在这个解释器里规定了GIL

GIL使无论有多少个cpu，python在执行时在同一时刻只允许一个线程运行。（一个进程可以开多个线程，但线程只能同时运行一个）

### python六个标准的数据类型

1. number，包括：int、float、bool、complex
2. string
3. tuple
4. set
5. list
6. dict
  
### 可变对象和不可变对象

可变对象与不可变对象的区别在于对象本身是否可变。不可变对象变化时，属于重新赋值，变量指向的地址改变，可变对象变化时，是修改对象本身，地址不变。可变对象作为参数传入时，在函数中对其本身进行修改，是会影响到全局中的这个变量值的，因为函数直接对该地址的值进行了修改。不可变对象作为参数时，在函数中修改不会改变全局对象值，重新指向一个新的地址

不可变对象：number、string、tuple

可变对象：set、list、dict

### 生成器和迭代器

+ 迭代器

  迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

  迭代器有两个基本的方法：iter() 和 next()，通过StopIteration异常标识结束

  字符串，列表或元组对象都可用于创建迭代器

  ```python
  # -*- coding:utf-8 -*-


  if __name__ == '__main__':
      it = iter([1, 2, 3, 4])
      try:
          while True:
              print(it.__next__())
      except StopIteration:
          pass

  ```

+ 生成器

  在 Python 中，使用了 yield 的函数被称为生成器（generator）。

  跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

  在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

  调用一个生成器函数，返回的是一个迭代器对象。

  ```python
  # -*- coding:utf-8 -*-


  def fibonacci(n):
      """生成器函数 - 斐波那契"""
      a, b, counter = 0, 1, 0
      while True:
          if n < counter:
              return
          yield a
          a, b = b, a + b
          counter += 1


  if __name__ == '__main__':
      f = fibonacci(10)
      try:
          while True:
              print(f.__next__())
      except StopIteration:
          pass
  ```

### 匿名函数（lambda表达式）

这种函数可以用在任何普通函数可以使用的地方，但在定义时被严格限定为单一表达式。

lambda 表达式：lambda argument1,argument2,...argumentN: expression using arguments

### @staticmethod和@classmethod

静态函数和类函数的区别在于：

1. 访问类属性以及调用类函数不同
2. 继承的不同

```python
# -*- coding:utf-8 -*-


class SupClass(object):
    X = 1
    Y = 2

    @staticmethod
    def average(*mixes):
        return sum(mixes) / len(mixes)

    @staticmethod
    def static_method():
        # 通过类名调用，如果类名修改，此处需要修改不太方便
        return SupClass.average(SupClass.X, SupClass.Y)

    @classmethod
    def class_method(cls):
        # 通过cls调用，如果类名修改，此处不需要修改
        return cls.average(cls.X, cls.Y)


class Subclass(SupClass):
    X = 3
    Y = 5

    @staticmethod
    def average(*mixes):
        return sum(mixes) / 3


if __name__ == '__main__':
    func = Subclass()
    print(func.static_method())  # Function.average(Function.X, Function.Y) = 1.5
    print(func.class_method())  # Subclass.average(Subclass.X, Subclass.Y) = 2.66666666666

```

### list set dict 的底层实现

+ list

python的列表内部实现是数组，因此就有组数的特点。超过容量会增加更多的容量，通过角标查找是O(1)，插入删除是O(n)

+ dict

python字典是通过哈希表实现的，因此key必须是可hash的，值没有要求，插入删除查找都是O(1)，最坏情况下是O(n)

+ set

Python中集合和字典非常相似，都是用哈希表实现的，其元素是唯一的、可哈希的对象，插入删除查找的平均时间复杂度都是O(1)，最坏情况下是O(n)
