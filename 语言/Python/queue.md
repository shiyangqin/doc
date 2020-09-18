# queue模块

+ [Queue](#Queue)

## Queue

一个自带线程同步的队列,每一次put都会使unfinished_tasks+1,只有手动调用task_done才能使unfinished_tasks-1,调用get不影响unfinished_tasks,join会阻塞线程直到unfinished_tasks减为0

```python
# -*- coding:utf-8 -*-
import queue
import time
import threading


class QueueDemo(object):
    """队列使用样例"""

    def __init__(self):
        self._queue = queue.Queue(10)

    def run(self):
        threading.Thread(target=self.__process_get).start()
        threading.Thread(target=self.__process_put).start()
        time.sleep(0.5)
        print(self._queue.empty())
        print(self._queue.full())
        print(self._queue.qsize())
        self._queue.join()
        print(self._queue.empty())
        print(self._queue.full())
        print(self._queue.qsize())

    def __process_get(self):
        time.sleep(1)
        while self._queue.qsize():
            print(self._queue.get())
            time.sleep(0.5)
            self._queue.task_done()

    def __process_put(self):
        for i in range(10):
            self._queue.put(i)


if __name__ == '__main__':
    QueueDemo().run()

```
