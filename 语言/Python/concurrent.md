# concurrent模块

+ [ThreadPoolExecutor和ProcessPoolExecutor](#ThreadPoolExecutor和ProcessPoolExecutor)

## ThreadPoolExecutor和ProcessPoolExecutor

concurrent.futures.thread.ThreadPoolExecutor

concurrent.futures.process.ProcessPoolExecutor

线程池和进程池，用法都是一样的，构造池对象时传入最大线程（进程）数，使用submit或map提交任务，任务提交完成后，调用shutdown等待任务完成后释放资源

submit返回一个Future对象

map返回一个generator对象，通过__next__函数依次获取返回值

```python
# -*- coding:utf-8 -*-
import time
import datetime
from concurrent.futures.thread import ThreadPoolExecutor
from concurrent.futures.process import ProcessPoolExecutor


def run(n):
    time.sleep(3)
    return str(datetime.datetime.now()) + ": " + str(n)


def thread_pool_executor_test():
    print(str(datetime.datetime.now()) + ": start")
    with ThreadPoolExecutor(2) as executor:
        f1 = executor.submit(run, 0)
        print(str(datetime.datetime.now()) + ": submit")
        f2 = executor.map(run, (1, 2, 3))
        print(str(datetime.datetime.now()) + ": map")
    print(str(datetime.datetime.now()) + ": print")
    print(type(f1))
    print(f1.result())

    print(type(f2))
    print(f2.__next__())
    print(f2.__next__())
    print(f2.__next__())


def process_pool_executor_test():
    print(str(datetime.datetime.now()) + ": start")
    ppe = ProcessPoolExecutor(2)
    f1 = ppe.submit(run, 0)
    print(str(datetime.datetime.now()) + ": submit")
    f2 = ppe.map(run, (1, 2, 3))
    print(str(datetime.datetime.now()) + ": map")
    print(str(datetime.datetime.now()) + ": print")
    print(type(f1))
    print(f1.result())

    print(type(f2))
    print(f2.__next__())
    print(f2.__next__())
    print(f2.__next__())
    ppe.shutdown()


if __name__ == '__main__':
    thread_pool_executor_test()
    # process_pool_executor_test()

```

使用with写法，代码会在with代码块执行完后调用shutdown，导致阻塞到池中任务全部结束
