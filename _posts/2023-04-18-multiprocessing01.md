---

layout: post
title: "multiprocessing 详解"
date: 2023-04-18
description: "多线程或多进程"

---

`multiprocessing`是Python标准库中用于实现多进程编程的模块，提供了一系列的类和函数来支持多进程编程。

下面是`multiprocessing`的一些常用类和函数：

1. `Process`类：用于创建进程的类，可以通过实例化`Process`类来创建一个新的进程。

   ```python
   from multiprocessing import Process
   import os
   
   def info(title):
       print(title)
       print('module name:', __name__)
       print('parent process:', os.getppid())
       print('process id:', os.getpid())
   
   def f(name):
       info('function f')
       print('hello', name)
   
   if __name__ == '__main__':
       info('main line')
       p = Process(target=f, args=('bob',))
       p.start()
       p.join()
   ```

2. `Queue`类：用于在多进程之间传递数据的类，它实现了进程安全的队列，可以安全地传递Python对象。

   ```python
   from multiprocessing import Process, Queue
   
   def worker(q):
       data = q.get()
       print('Got data:', data)
   
   q = Queue()
   p = Process(target=worker, args=(q,))
   p.start()
   
   q.put('Hello, Queue!')
   ```

3. `Pool`类：用于实现进程池的类，可以在多个进程之间分配和管理任务。

   ```python
   from multiprocessing import Pool
   
   def worker(n):
       return n ** 2
   
   pool = Pool(processes=4)
   results = pool.map(worker, range(10))
   print(results)
   ```

4. `Lock`类：用于在多个进程之间同步共享资源的类，它提供了一种互斥锁的机制，用于保护共享资源。

   ```python
   from multiprocessing import Process, Lock
   
   def worker(lock, n):
       lock.acquire()
       try:
           print('Got lock:', n)
       finally:
           lock.release()
   
   lock = Lock()
   for i in range(5):
       p = Process(target=worker, args=(lock, i))
       p.start()
   ```

以上是`multiprocessing`的一些常用类和函数，还有一些其他的类和函数，如`Manager`类、`Pipe`类等，都可以用于不同的多进程编程场景。需要注意的是，在多进程编程中，有些操作是不支持的，比如不能在多个进程之间共享文件句柄、不能在多进程之间共享全局变量等，需要使用特殊的机制来实现数据的共享和同步。

