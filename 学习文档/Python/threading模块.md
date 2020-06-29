# threading模块

+ [Lock](#Lock)
+ [RLock](#RLock)
+ [Condition](#Condition)
+ [Event](#Event)
+ [Semaphore和BoundedSemaphore](#Semaphore和BoundedSemaphore)
+ [Barrier](#Barrier)
+ [Thread](#Thread)
+ [Timer](#Timer)
+ [杂项](#杂项)

## Lock

当多个线程操作同一资源时，保证前一个线程操作完成，下一个才可以开始操作

```python
# -*- coding:utf-8 -*-
import threading


class ProblemDemo(object):
    """问题样例"""

    def __init__(self):
        self._item_list = ['item']

    def run(self):
        for i in range(100):
            threading.Thread(target=self.__process).start()

    def __process(self):
        for _ in range(10):
            print('remove item')
            self._item_list.remove('item')
            print('add item')
            self._item_list.append('item')


if __name__ == '__main__':
    ProblemDemo().run()

```

代码执行报错步骤：

1. 初始ProblemDemo._item_list = ['item']
2. 线程1：ProblemDemo._item_list.remove('item')
3. cpu暂停执行线程1，开始执行线程2代码
4. 线程2：ProblemDemo._item_list.remove('item')
5. 当线程2执行remove时，_item_list中已经没有 item 了，抛出 ValueError: list.remove(x): x not in list

<img src="img/threading1.jpg" width=500 />

解决方法：给代码块加锁

```python
# -*- coding:utf-8 -*-
import threading


class LockDemo(object):
    """加锁样例"""

    def __init__(self):
        self._item_list = ['item']
        self._item_list_lock = threading.Lock()

    def run(self):
        for i in range(100):
            threading.Thread(target=self.__process).start()

    def __process(self):
        for _ in range(10):
            with self._item_list_lock:
                print('remove item')
                self._item_list.remove('item')
                print('add item')
                self._item_list.append('item')


if __name__ == '__main__':
    LockDemo().run()

```

代码执行步骤：

1. 线程1获取锁，失去cpu
2. 线程2尝试获取锁，失败，线程阻塞
3. 线程1执行移除和添加操作，代码块执行完毕，释放锁
4. 线程2尝试获取锁，成功，线程2继续执行

## RLock

当获取一个资源需要先获取另一个资源时，保证一个线程获取其中一个资源的锁时，其他线程无法获取另外的锁，直到当前线程释放所有锁

```python
# -*- coding:utf-8 -*-
import threading


class ProblemDemo(object):
    """死锁样例"""

    def __init__(self):
        self._item_1 = 0
        self._item_2 = 0
        self._item_1_lock = threading.Lock()
        self._item_2_lock = threading.Lock()

    def run(self):
        threading.Thread(target=self.__process_1).start()
        threading.Thread(target=self.__process_2).start()

    def __process_1(self):
        for _ in range(10):
            with self._item_1_lock:
                print("item_1 + 1")
                self._item_1 += 1
                with self._item_2_lock:
                    print("item_2 + 1")
                    self._item_2 += 1

    def __process_2(self):
        for _ in range(10):
            with self._item_2_lock:
                print("item_2 + 1")
                self._item_2 += 1
                with self._item_1_lock:
                    print("item_1 + 1")
                    self._item_1 += 1


if __name__ == '__main__':
    ProblemDemo().run()

```

死锁步骤：

1. 线程1拿到_item_1_lock，失去cpu
2. 线程2拿到_item_2_lock，执行代码，尝试获取_item_1_lock
3. 线程1执行代码，尝试获取_item_2_lock
4. 2个线程各拿一个锁，互相等对方释放锁，达成死锁，代码执行停滞

```python
# -*- coding:utf-8 -*-
import threading


class RLockDemo(object):
    """递归锁样例"""

    def __init__(self):
        self._item_1 = 0
        self._item_2 = 0
        self._item_1_lock = self._item_2_lock = threading.RLock()

    def run(self):
        threading.Thread(target=self.__process_1).start()
        threading.Thread(target=self.__process_2).start()

    def __process_1(self):
        for _ in range(10):
            with self._item_1_lock:
                print("item_1 + 1")
                self._item_1 += 1
                with self._item_2_lock:
                    print("item_2 + 1")
                    self._item_2 += 1

    def __process_2(self):
        for _ in range(10):
            with self._item_2_lock:
                print("item_2 + 1")
                self._item_2 += 1
                with self._item_1_lock:
                    print("item_1 + 1")
                    self._item_1 += 1


if __name__ == '__main__':
    RLockDemo().run()

```

## Condition

条件变量，一个操作需要满足一个条件才能执行，python中的Queue是一个很好的例子，可以看一看

```python
# -*- coding:utf-8 -*-
import threading
from collections import deque


class ConditionDemo(object):
    """信号样例(参考Queue)"""

    def __init__(self, maxsize=0):
        """初始化队列"""
        self.maxsize = maxsize
        self._queue = deque()
        self.mutex = threading.Lock()
        self.not_empty = threading.Condition(self.mutex)
        self.not_full = threading.Condition(self.mutex)

    def size(self):
        """队列大小"""
        return len(self._queue)

    def is_full(self):
        """队列是否已满"""
        with self.mutex:
            return 0 < self.maxsize <= self.size()

    def is_empty(self):
        """队列是否已空"""
        with self.mutex:
            return not self.size()

    def put(self, item):
        """向队列中添加元素，队列满时阻塞，调用get函数时唤醒"""
        print("put item")
        with self.not_full:
            if self.maxsize > 0:
                while self.size() >= self.maxsize:
                    print("put item wait")
                    self.not_full.wait()
            self._queue.append(item)
            self.not_empty.notify()

    def get(self):
        """从队列中获取元素，队列空时阻塞，调用put函数时唤醒"""
        print("get item")
        with self.not_empty:
            while not self.size():
                print("get item wait")
                self.not_empty.wait()
            item = self._queue.popleft()
            self.not_full.notify()
            return item


if __name__ == '__main__':
    demo = ConditionDemo(1)
    print("size:" + str(demo.size()))
    print("is_empty:" + str(demo.is_empty()))
    print("is_full:" + str(demo.is_full()))

    threading.Timer(1, function=demo.put, args=(1,)).start()
    print(demo.get())

    threading.Timer(2, function=demo.get).start()
    demo.put(2)
    demo.put(3)

    print("size:" + str(demo.size()))
    print("is_empty:" + str(demo.is_empty()))
    print("is_full:" + str(demo.is_full()))

```

调用get函数使队列元素减少时，唤醒正在等待的not_full，调用put函数添加元素时，唤醒正在等待的not_empty

## Event

事件对象，用于线程间通讯，event对象中有一个flag，默认值为false，当调用set时，置为true，调用clear时置为false，调用wait时，如果flag为false则阻塞，否则通过

```python
# -*- coding:utf-8 -*-
import datetime
import threading


class EventDemo(object):
    """event样例"""

    def __init__(self):
        self._event_obj = threading.Event() 

    def run(self):
        threading.Thread(target=self.__process_1).start()
        threading.Thread(target=self.__process_2).start()
        threading.Timer(2, function=self._event_obj.set).start()
        threading.Timer(4, function=self._event_obj.set).start()

    def __process_1(self):
        while not self._event_obj.is_set():
            print("process_1 wait")
            self._event_obj.wait()
        else:
            print("process_1:" + str(datetime.datetime.now()))
            self._event_obj.clear()

    def __process_2(self):
        while not self._event_obj.is_set():
            print("process_2 wait")
            self._event_obj.wait()
        else:
            self._event_obj.clear()
            print("process_2:" + str(datetime.datetime.now()))


if __name__ == '__main__':
    EventDemo().run()

```

执行结果有2种可能

<img src="img/threading4.jpg" />

第一种可能：当event第一次被置为True时，2条线程都在对方调用self._event_obj.clear()之前已经完成判定，表现为同时输出时间

<img src="img/threading5.jpg" />

第二种可能，一条线程先执行self._event_obj.clear()，第二条线程判定失败，继续等待

## Semaphore和BoundedSemaphore

Semaphore：信号量，用于控制获取资源的线程数量，创建对象时初始化value，当调用acquire时value-1，当调用release时value+1，当value=0时，调用acquire将阻塞线程，当value达到初始值时，再次调用release仍会使value+1，不会报错

BoundedSemaphore：Semaphore的子类，当value为初始值时，再次调用release抛出ValueError("Semaphore released too many times")异常

```python
# -*- coding:utf-8 -*-
import datetime
import threading
import time


class SemaphoreDemo(object):
    """Semaphore样例"""

    def __init__(self):
        self._sleep_time = 2
        self._available = threading.Semaphore(2)

    def run(self):
        for i in range(6):
            threading.Thread(target=self.__process).start()

    def __process(self):
        with self._available:
            print(datetime.datetime.now())
            time.sleep(self._sleep_time)
            self._available.release()


if __name__ == '__main__':
    SemaphoreDemo().run()

```

__process函数在最后再次执行了release函数，所以第一波拿到Semaphore的2个线程执行完后，Semaphore的value已经变成了4，所以执行结果是第一波两条线程打印时间，第二波剩下的4条线程打印时间

<img src="img/threading2.jpg" />

```python
# -*- coding:utf-8 -*-
import datetime
import threading
import time


class BoundedSemaphoreDemo(object):
    """BoundedSemaphore样例"""

    def __init__(self):
        self._sleep_time = 2
        self._available = threading.BoundedSemaphore(2)

    def run(self):
        for i in range(6):
            threading.Thread(target=self.__process).start()

    def __process(self):
        with self._available:
            print(datetime.datetime.now())
            time.sleep(self._sleep_time)


if __name__ == '__main__':
    BoundedSemaphoreDemo().run()

```

BoundedSemaphore额外执行release会报错，所以这次每一波都是2条线程，分3次打印

<img src="img/threading3.jpg" />

## Barrier

栅栏对象，当调用wait对象时，线程阻塞，并记录已阻塞线程的数量，当阻塞线程线程数量达到指定值时才继续向下执行，或者阻塞线程等待时间超过等待时间参数，抛出threading.BrokenBarrierError错误

```python
# -*- coding:utf-8 -*-
import datetime
import threading
import time


class BarrierDemo(object):
    """barrier样例"""

    def __init__(self):
        self._barrier_obj = threading.Barrier(2)

    def run(self):
        print(datetime.datetime.now())
        for i in range(5):
            threading.Thread(target=self.__process, args=(i,)).start()

    def __process(self, sleep_time):
        time.sleep(sleep_time)
        try:
            self._barrier_obj.wait(2)
        except RuntimeError:
            print('error')
        print(datetime.datetime.now())


if __name__ == '__main__':
    BarrierDemo().run()

```

<img src="img/threading6.jpg" />

## Thread

线程对象

主要参数说明：

+ target: 是函数名字，需要调用的函数。

+ name: 设置线程名字。

+ args: 函数需要的参数，元组形式

+ kwargs: 函数需要的参数，字典形式

+ daemon: 是否是守护线程

Thread 对象主要方法说明:

+ run(): 用以表示线程活动的方法。

+ start(): 启动线程活动。

+ join(): 等待至线程中止。

### 函数创建多线程

```python
# -*- coding:utf-8 -*-
import threading
import time


def run(t_name, sleep_time):
    print(t_name + " run")
    time.sleep(sleep_time)


def thread_demo_1():
    """线程创建示例1"""
    for i in range(5):
        t = threading.Thread(name="Thread-"+str(i), target=run, args=("Thread-"+str(i), i))
        print(t.getName() + " start")
        t.start()


if __name__ == '__main__':
    thread_demo_1()

```

<img src="img/threading7.jpg" />

可call的类对象也可以使用

```python
# -*- coding:utf-8 -*-
import threading
import time


class CallDemo(object):

    def __init__(self, t_name, sleep_time):
        self._t_name = t_name
        self._sleep_time = sleep_time

    def __call__(self):
        self.run()

    def run(self):
        print(self._t_name + " run")
        time.sleep(self._sleep_time)


def thread_demo_3():
    """线程创建示例3"""
    for i in range(5):
        t = threading.Thread(name="Thread-"+str(i), target=CallDemo("Thread-"+str(i), i))
        print(t.getName() + " start")
        t.start()


if __name__ == '__main__':
    thread_demo_3()

```

<img src="img/threading10.jpg" />

### 类创建多线程

```python
# -*- coding:utf-8 -*-
import threading
import time


class ThreadDemo2(threading.Thread):
    """Thread子类"""

    def run(self) -> None:
        print(self.getName() + " run")
        time.sleep(self._args[0])


def thread_demo_2():
    """线程创建示例2"""
    for i in range(5):
        t = ThreadDemo2(name="Thread-"+str(i), args=(i,))
        print(t.getName() + " start")
        t.start()


if __name__ == '__main__':
    thread_demo_2()

```

<img src="img/threading8.jpg" />

### 守护进程

如果当前python线程是守护线程，那么意味着这个线程是“不重要”的，“不重要”意味着如果他的主进程结束了但该守护线程没有运行完，守护进程就会被强制结束。如果线程是非守护线程，那么父进程只有等到守护线程运行完毕后才能结束。

```python
# -*- coding:utf-8 -*-
import threading
import time

class DaemonDemo(threading.Thread):
    """守护线程样例"""

    def run(self) -> None:
        print(self.getName() + " run")
        time.sleep(self._args[0])
        print(self.getName() + " end")


def daemon_demo():
    """守护线程样例"""
    t = DaemonDemo(name="Thread-daemon", args=(2,), daemon=True)
    print(t.getName() + " start")
    t.start()


if __name__ == '__main__':
    daemon_demo()

```

<img src="img/threading11.jpg" />

主函数daemon_demo运行完成后，守护线程处于还在sleep，这时由于主线程结束，守护线程也被强制结束，所以没有打印出"Thread-daemon end"

## Timer

定时器，Thread的子类，构造参数：

+ interval: 指定的时间
+ function: 要执行的方法（同Thread一样，可call的类对象也可以）
+ args/kwargs: 方法的参数

```python
# -*- coding:utf-8 -*-
import datetime
import threading


def run(t_name):
    print(datetime.datetime.now(), " " + t_name + " run")


def timer_demo1():
    """定时器创建示例1"""
    for i in range(5):
        t = threading.Timer(interval=i, function=run, args=("timer-" + str(i),))
        t.start()


if __name__ == '__main__':
    timer_demo1()

```

<img src="img/threading9.jpg" />

```python
# -*- coding:utf-8 -*-
import datetime
import threading


class CallDemo(object):

    def __init__(self, t_name):
        self._t_name = t_name

    def __call__(self):
        self.run()

    def run(self):
        print(self._t_name + " run")


def timer_demo2():
    """定时器创建示例2"""
    for i in range(5):
        t = threading.Timer(interval=i, function=CallDemo("timer-" + str(i)))
        t.start()


if __name__ == '__main__':
    timer_demo2()

```

输出结果是一样的

## 杂项

+ active_count：返回当前存活的线程类 Thread 对象。返回的计数等于 enumerate() 返回的列表长度。

+ current_thread：返回当前对应调用者的控制线程的 Thread 对象。如果调用者的控制线程不是利用 threading 创建，会返回一个功能受限的虚拟线程对象。

+ enumerate：以列表形式返回当前所有存活的 Thread 对象。 该列表包含守护线程，current_thread() 创建的虚拟线程对象和主线程。它不包含已终结的线程和尚未开始的线程。

+ get_ident：返回当前线程的 “线程标识符”。它是一个非零的整数。它的值没有直接含义，主要是用作 magic cookie，比如作为含有线程相关数据的字典的索引。线程标识符可能会在线程退出，新线程创建时被复用。

+ local：一个代表线程本地数据的类。

+ main_thread：返回主 Thread 对象。一般情况下，主线程是Python解释器开始时创建的线程。

+ TIMEOUT_MAX：阻塞函数（ Lock.acquire(), RLock.acquire(), Condition.wait(), ...）中形参 timeout 允许的最大值。传入超过这个值的 timeout 会抛出 OverflowError 异常。

+ setprofile：为所有 threading 模块开始的线程设置性能测试函数。在每个线程的 run() 方法被调用前，func 会被传递给 sys.setprofile() 。

+ settrace：为所有 threading 模块开始的线程设置追踪函数。在每个线程的 run() 方法被调用前，func 会被传递给 sys.settrace() 。

+ stack_size：返回创建线程时用的堆栈大小。可选参数 size 指定之后新建的线程的堆栈大小，而且一定要是0（根据平台或者默认配置）或者最小是32,768(32KiB)的一个正整数。如果 size 没有指定，默认是0。如果不支持改变线程堆栈大小，会抛出 RuntimeError 错误。如果指定的堆栈大小不合法，会抛出 ValueError 错误并且不会修改堆栈大小。32KiB是当前最小的能保证解释器有足够堆栈空间的堆栈大小。需要注意的是部分平台对于堆栈大小会有特定的限制，例如要求大于32KiB的堆栈大小或者需要根据系统内存页面的整数倍进行分配 - 应当查阅平台文档有关详细信息（4KiB页面比较普遍，在没有更具体信息的情况下，建议的方法是使用4096的倍数作为堆栈大小）。适用于: Windows，具有 POSIX 线程的系统。
