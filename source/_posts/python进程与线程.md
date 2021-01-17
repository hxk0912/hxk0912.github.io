---
title: python进程与线程
date: 2021-01-16 14:42:32
categories: python
tags:
---

## python中的多进程

### 创建一个进程

``` python
from multiprocessing import Process
from os import getpid
from random import randint
from time import time, sleep


def download_task(filename):
    print('启动下载进程，进程号[%d].' % getpid())
    print('开始下载%s...' % filename)
    time_to_download = randint(5, 10)
    sleep(time_to_download)
    print('%s下载完成! 耗费了%d秒' % (filename, time_to_download))


def main():
    start = time()
    p1 = Process(target=download_task, args=('Python从入门到住院.pdf', ))
    p1.start()
    p2 = Process(target=download_task, args=('Peking Hot.avi', ))
    p2.start()
    p1.join()
    p2.join()
    end = time()
    print('总共耗费了%.2f秒.' % (end - start))


if __name__ == '__main__':
    main()

```

在上面的代码中，我们通过`Process`类创建了进程对象，通过`target`参数我们传入一个函数来表示进程启动后要执行的代码，后面的`args`是一个元组，它代表了传递给函数的参数。`Process`对象的`start`方法用来启动进程，而`join`方法表示等待进程执行结束。运行上面的代码可以明显发现两个下载任务“同时”启动了，而且程序的执行时间将大大缩短，不再是两个任务的时间总和。

## python中多线程

``` python
from random import randint
from threading import Thread
from time import time, sleep


def download(filename):
    print('开始下载%s...' % filename)
    time_to_download = randint(5, 10)
    sleep(time_to_download)
    print('%s下载完成! 耗费了%d秒' % (filename, time_to_download))


def main():
    start = time()
    t1 = Thread(target=download, args=('Python从入门到住院.pdf',))
    t1.start()
    t2 = Thread(target=download, args=('Peking Hot.avi',))
    t2.start()
    t1.join()
    t2.join()
    end = time()
    print('总共耗费了%.3f秒' % (end - start))


if __name__ == '__main__':
    main()
```

基本与进程一致

## 进程与线程编程时要注意的地方

**进程在创建时，子进程复制了父进程及其所有的数据结构，每个子进程都有自己的独立的内存空间，所以不可以通过设置全局变量的方式进行通信**

**但是多个线程可以共享进程的内存空间，因此可以通过设置全局变量的方式来进行，但是如果多个线程共享同一个变量时，多个线程竞争可能会使程序失效。如果一个资源被多个线程竞争使用，那么我们通常称之为"临界资源"，访问的时候需要加上保护。**

所以在执行比较耗时间的任务时将任务放到线程中执行比较好。
