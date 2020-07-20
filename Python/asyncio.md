# asyncio模块

## 协程

通常在Python中我们进行并发编程一般都是使用多线程或者多进程来实现的，对于CPU计算密集型任务由于GIL的存在通常使用多进程来实现，而对于IO密集型任务可以通过线程调度来让线程在执行IO任务时让出GIL，从而实现表面上的并发。

其实对于IO密集型任务我们还有一种选择就是协程。协程，又称微线程，英文名Coroutine，是运行在单线程中的“并发”，协程相比多线程的一大优势就是省去了多线程之间的切换开销，获得了更高的运行效率。Python中的异步IO模块asyncio就是基本的协程模块。

协程的切换不同于线程切换，是由程序自身控制的，没有切换的开销。协程不需要多线程的锁机制，因为都是在同一个线程中运行，所以没有同时访问数据的问题，执行效率比多线程高很多。

因为协程是单线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

如果你还无法理解协程的概念，那么可以这么简单的理解：

进程/线程：操作系统提供的一种并发处理任务的能力。

协程：程序员通过高超的代码能力，在代码执行流程中人为的实现多任务并发，是单个线程内的任务调度技巧。

## 协程使用

协程函数通过async关键子创建

在协程函数中，通过await执行其他协程函数并等待对方结束

协程函数直接调用不会执行，需要调用asyncio中的相关功能才能执行

一个简单的协程程序：

```python
# -*- coding:utf-8 -*-
import asyncio
import time
import requests


async def work(i, j):
    print('work-{}-{} start'.format(i, j))
    end_time = time.time() + 1
    while time.time() < end_time:
        print('work-{}-{}: '.format(i, j) + requests.get("http://10.255.175.224/test").text)
    print('work-{}-{} end'.format(i, j))


async def test_1(n):
    print('test-{} start'.format(n))
    await asyncio.gather(work(n, 1), work(n, 2))
    print('test-{} end'.format(n))
    return 0


async def test_2(n):
    print('test-{} start'.format(n))
    f = asyncio.gather(work(n, 1), work(n, 2))
    print('test-{} end'.format(n))
    return 0


if __name__ == '__main__':
    asyncio.run(test_1(0))
    print("-----------------------------------")
    asyncio.run(test_2(1))

```
