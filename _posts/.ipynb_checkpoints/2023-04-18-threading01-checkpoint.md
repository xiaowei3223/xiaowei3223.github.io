---

layout: post
title: "Threading 详解"
date: 2023-04-18
description: "多线程或多进程"

---

## 一、常见的类

1. `Thread`：线程类，它是所有线程的基类，可以通过继承 `Thread` 类来创建自己的线程类。

   ```python
   '''
   Thread 类是 Python 中用于创建线程的基类，使用起来非常简单。下面是一个使用 Thread 类创建线程的示例：
   
   在这个示例中，我们定义了一个名为 MyThread 的线程类，这个类继承自 Thread 类。我们通过重写 run 方法来定义线程的任务。
   
   然后我们创建了两个线程实例 t1 和 t2，并分别启动它们。最后我们使用 join 方法等待线程执行结束，然后再输出一条消息。
   
   需要注意的是，线程的启动必须通过 start 方法来完成。线程执行的任务在 run 方法中定义，需要自行实现。
   
   在实际开发中，我们可以通过继承 Thread 类来创建自己的线程类，并在其中定义线程的任务。由于 Thread 类实现了 Python 中的协程，因此可以在一个程序中同时运行多个线程，以达到并行执行的效果。
   
   '''
   import threading
   import time
   
   # 定义一个线程类
   class MyThread(threading.Thread):
       def __init__(self, name):
           super().__init__(name=name)
   
       # 重写 run 方法
       def run(self):
           print("线程 %s 开始运行" % self.getName())
           # 执行线程任务
           for i in range(5):
               print("线程 %s 运行中，i=%d" % (self.getName(), i))
               time.sleep(1)
           print("线程 %s 运行结束" % self.getName())
   
   # 创建两个线程
   t1 = MyThread(name="Thread-1")
   t2 = MyThread(name="Thread-2")
   
   # 启动线程
   t1.start()
   t2.start()
   
   # 等待线程执行结束
   t1.join()
   t2.join()
   
   print("主线程结束")
   ```

   

2. `Lock`：锁类，可以用于在多个线程之间进行同步，避免出现数据竞争等问题。

   ```python
   # -*- coding: utf-8 -*-
   
   '''
   在多线程编程中，为了避免多个线程同时对同一个资源进行修改导致数据不一致的情况，我们需要使用锁机制来保证同一时刻只有一个线程可以访问该资源。threading 模块中提供了 Lock 类来实现这种机制。
   
   下面是一个使用 Lock 类实现线程同步的示例：
   
   在这个示例中，我们定义了一个名为 MyThread 的线程类，这个类继承自 Thread 类。我们通过重写 run 方法来定义线程的任务。
   
   在任务中，我们首先使用 acquire 方法获取锁，然后对全局变量 num 进行修改，最后使用 release 方法释放锁。这样可以保证同一时刻只有一个线程可以对 num 变量进行修改，避免了数据不一致的情况。
   
   需要注意的是，对于使用锁的代码，必须保证获取锁和释放锁的操作是成对出现的。如果获取锁的代码执行后出现了异常导致没有释放锁，那么其他线程将无法获取锁，程序可能会死锁。
   
   除了 Lock 类之外，threading 模块还提供了其他的锁类型，如 RLock、Semaphore、Condition 等，开发者可以根据自己的需要选择合适的锁类型。
   
   '''
   import threading
   import time
   
   # 定义一个全局变量
   num = 0
   
   # 创建一个锁对象
   lock = threading.Lock()
   
   # 定义一个线程类
   class MyThread(threading.Thread):
       def __init__(self, name):
           super().__init__(name=name)
   
       # 重写 run 方法
       def run(self):
           global num  # 声明 num 为全局变量
           # 获取锁
           lock.acquire()
           try:
               print("线程 %s 获取到锁" % self.getName())
               # 对 num 变量进行修改
               for i in range(5):
                   num += 1
                   print("线程 %s 修改 num，num=%d" % (self.getName(), num))
                   time.sleep(1)
           finally:
               # 释放锁
               lock.release()
   
   # 创建两个线程
   t1 = MyThread(name="Thread-1")
   t2 = MyThread(name="Thread-2")
   
   # 启动线程
   t1.start()
   t2.start()
   
   # 等待线程执行结束
   t1.join()
   t2.join()
   
   print("主线程结束，num=%d" % num)
   ```

   

3. `RLock`：可重入锁类，它是 `Lock` 类的改进版，可以允许同一个线程在未释放锁的情况下再次获取该锁。

   ```python
   # -*- coding: utf-8 -*-
   
   '''
   在多线程编程中，有时候我们需要在同一个线程中多次获取锁，这时就需要用到可重入锁（Reentrant Lock），也叫递归锁。
   
   在Python中，threading模块提供了RLock类来实现可重入锁。与普通锁不同的是，可重入锁可以被同一个线程多次获取，但是必须被同样次数的释放。这种锁机制通常用于线程递归调用的场景。
   
   下面是一个使用RLock类实现线程同步的示例：
   
   在这个示例中，我们使用 RLock 类来实现线程同步。在任务中，我们首先使用 acquire 方法获取锁，然后对全局变量 num 进行修改。接着，在循环中再次获取锁，这里需要注意的是，必须要在释放锁之后才能再次获取，否则会导致死锁。最后，在 finally 语句块中释放锁。
   
   需要注意的是，在使用可重入锁时，必须保证获取锁和释放锁的次数是相等的，否则可能会导致死锁。
   
   '''
   import threading
   
   # 定义一个全局变量
   num = 0
   
   # 创建一个可重入锁对象
   rlock = threading.RLock()
   
   # 定义一个线程类
   class MyThread(threading.Thread):
       def __init__(self, name):
           super().__init__(name=name)
   
       # 重写 run 方法
       def run(self):
           global num  # 声明 num 为全局变量
           # 获取锁
           rlock.acquire()
           try:
               print("线程 %s 获取到锁" % self.getName())
               # 对 num 变量进行修改
               for i in range(5):
                   num += 1
                   print("线程 %s 修改 num，num=%d" % (self.getName(), num))
                   # 再次获取锁
                   rlock.acquire()
                   print("线程 %s 再次获取锁" % self.getName())
                   # 释放锁
                   rlock.release()
                   print("线程 %s 释放锁" % self.getName())
           finally:
               # 释放锁
               rlock.release()
               print("线程 %s 释放锁" % self.getName())
   
   # 创建两个线程
   t1 = MyThread(name="Thread-1")
   t2 = MyThread(name="Thread-2")
   
   # 启动线程
   t1.start()
   t2.start()
   
   # 等待线程执行结束
   t1.join()
   t2.join()
   
   print("主线程结束，num=%d" % num)
   ```

   

4. `Semaphore`：信号量类，可以用于限制同时访问共享资源的线程数量。

   ```python
   # -*- coding: utf-8 -*-
   
   '''
   Semaphore 是 threading 模块中的一种同步原语，它允许在某一时刻只有固定数量的线程可以访问共享资源。当一个线程需要访问共享资源时，它必须先请求 Semaphore，如果此时 Semaphore 允许访问，则线程可以继续执行。如果此时 Semaphore 不允许访问，则线程必须等待，直到有足够的资源可用。
   Semaphore 类的构造函数接受一个整数参数，表示最多允许多少个线程同时访问共享资源。Semaphore 类有两个主要的方法：acquire() 和 release()，分别用于获取和释放资源。
   
   下面是一个使用 Semaphore 类的示例：
   
   在这个示例中，我们使用 Semaphore 类来限制同时访问共享资源的线程数量，当有线程需要访问共享资源时，必须先获取 Semaphore，如果此时 Semaphore 允许访问，则线程可以继续执行。否则，线程必须等待，直到有足够的资源可用。在任务中，我们首先使用 acquire() 方法获取 Semaphore，然后对全局变量 num 进行修改。最后，在 finally 语句块中释放 Semaphore。
   
   需要注意的是，Semaphore 并不会限制线程执行的顺序，它只是限制同时访问共享资源的线程数量。在这个示例中，我们设置 Semaphore 的值为 2，因此最多只能有 2 个线程同时访问共享资源。
   
   
   '''
   import threading
   
   # 定义一个全局变量
   num = 0
   
   # 创建一个 Semaphore 对象
   semaphore = threading.Semaphore(2)
   
   # 定义一个线程类
   class MyThread(threading.Thread):
       def __init__(self, name):
           super().__init__(name=name)
   
       # 重写 run 方法
       def run(self):
           global num  # 声明 num 为全局变量
           # 获取 Semaphore
           semaphore.acquire()
           try:
               print("线程 %s 获取到 Semaphore" % self.getName())
               # 对 num 变量进行修改
               for i in range(5):
                   num += 1
                   print("线程 %s 修改 num，num=%d" % (self.getName(), num))
               # 释放 Semaphore
               semaphore.release()
               print("线程 %s 释放 Semaphore" % self.getName())
           except Exception as e:
               print(e)
   
   # 创建四个线程
   t1 = MyThread(name="Thread-1")
   t2 = MyThread(name="Thread-2")
   t3 = MyThread(name="Thread-3")
   t4 = MyThread(name="Thread-4")
   
   # 启动线程
   t1.start()
   t2.start()
   t3.start()
   t4.start()
   
   # 等待线程执行结束
   t1.join()
   t2.join()
   t3.join()
   t4.join()
   
   print("主线程结束，num=%d" % num)
   ```

   

5. `Event`：事件类，可以用于在线程之间传递信号，使得某些线程可以等待某些事件的发生后再继续执行。

   ```python
   # -*- coding: utf-8 -*-
   
   '''
   threading.Event类是一个线程同步的工具，它用于线程之间的事件通知。Event对象维护一个内部标志，该标志可以被设置为“已设置”或“未设置”。线程可以等待（阻塞），直到事件被设置，也可以在事件被设置时唤醒。
   
   Event类有三个方法：
   
   set()：将Event的内部标志设置为True。所有在此Event上等待的线程将被唤醒。
   clear()：将Event的内部标志设置为False。所有调用wait()方法的线程将阻塞。
   wait(timeout=None)：阻塞线程直到Event的内部标志为True。如果在超时时间内未设置Event的内部标志，则线程将继续执行。
   下面是一个简单的例子，展示了如何使用Event类：
   
   '''
   import threading
   import time
   
   def wait_for_event(event):
       print('wait_for_event: starting')
       event.wait()
       print('wait_for_event: event set')
   
   def wait_for_event_timeout(event, timeout):
       print('wait_for_event_timeout: starting')
       event_is_set = event.wait(timeout)
       if event_is_set:
           print('wait_for_event_timeout: event set')
       else:
           print('wait_for_event_timeout: event not set')
   
   # 创建一个Event对象
   event = threading.Event()
   
   # 启动两个线程，一个线程调用wait_for_event方法，另一个线程调用wait_for_event_timeout方法
   t1 = threading.Thread(target=wait_for_event, args=(event,))
   t2 = threading.Thread(target=wait_for_event_timeout, args=(event, 2))
   
   t1.start()
   t2.start()
   
   # 在3秒后将Event的内部标志设置为True
   time.sleep(3)
   event.set()
   
   t1.join()
   t2.join()
   
   '''
   可以看到，线程t1一直在等待Event的内部标志被设置为True，而线程t2在等待2秒后返回，因为Event的内部标志未被设置为True。在3秒后，Event的内部标志被设置为True，两个线程都成功执行完成。
   '''
   
   ```

   

6. `Condition`：条件变量类，可以用于在多个线程之间进行同步，支持线程的等待和唤醒操作。

   ```python
   # -*- coding: utf-8 -*-
   
   '''
   Condition类是一个复杂的同步工具，它允许线程之间进行高级通信和协作。它通过在内部维护一个Lock对象来实现，可以允许一个线程在满足特定条件时暂停执行，直到另一个线程发出信号告知条件已经满足，然后该线程再继续执行。 Condition类提供了与Lock类相同的acquire()和release()方法，以及两个附加方法：wait()和notify()/notify_all()。
   
   Condition类的常用方法有：
   
   __init__(lock=None)：创建一个Condition对象。如果未指定锁，则使用新创建的RLock。
   acquire(*args)：获取底层锁。
   release()：释放底层锁。
   wait(timeout=None)：使当前线程等待，直到另一个线程调用notify()或notify_all()，或者超时时间到达。在调用wait()之前必须获得底层锁。
   notify(n=1)：唤醒wait()调用的一个或多个线程。默认唤醒一个线程。
   notify_all()：唤醒所有等待线程。
   下面是一个简单的例子，展示了如何使用Condition类：
   
   
   
   '''
   import threading
   import time
   
   # 创建一个Condition对象
   condition = threading.Condition()
   
   # 一个消费者和一个生产者
   items = []
   max_items = 5
   
   def consumer():
       global items
       while True:
           with condition:
               while not items:
                   print("consumer: waiting for items")
                   condition.wait()
               item = items.pop()
               print("consumer: popped %s from items" % item)
               condition.notify()
   
   def producer():
       global items
       for i in range(10):
           with condition:
               while len(items) == max_items:
                   print("producer: items are full, waiting for consumer")
                   condition.wait()
               items.append(i)
               print("producer: appended %s to items" % i)
               condition.notify()
   
   # 启动消费者线程和生产者线程
   c = threading.Thread(target=consumer)
   p = threading.Thread(target=producer)
   
   c.start()
   p.start()
   
   # 等待消费者和生产者线程完成
   c.join()
   p.join()
   
   
   '''
   这个例子中，生产者线程不断地向items列表中添加元素，当items已经满了的时候，会调用condition.wait()暂停线程，直到消费者线程取走一个元素并调用condition.notify()为止。消费者线程不断地从items列表中取出元素，当items为空的时候，会调用condition.wait()暂停线程，直到生产者线程添加一个元素并调用condition.notify()为止。这种方式可以有效地避免了生产者线程向已经满了的列表中添加元素和消费者线程从空列表中
   '''
   ```

   

7. `Timer`：定时器类，可以用于在指定时间后执行某个任务。

   `Timer`类是Python中`threading`模块提供的一种定时器。与`time.sleep()`方法不同，它可以在特定时间后执行一个函数，并且可以在执行之前取消定时器。以下是`Timer`类的一些基本使用方法：

   1）创建一个定时器对象：通过`Timer`类创建一个定时器对象，并指定定时器的延迟时间和要执行的函数。

   ```python
   from threading import Timer
   
   # 延迟2秒后执行print函数
   t = Timer(2.0, print, ["hello"])
   ```

   2）启动定时器：通过`start()`方法启动定时器，会在延迟时间结束后执行指定函数

   ```python
   t.start()
   ```

   3）取消定时器：通过`cancel()`方法可以在定时器未执行之前取消定时器。

   ```python
   t.cancel()
   ```

例子：

```python
from threading import Timer

def say_hello():
    print("Hello, Timer!")

# 延迟5秒后执行say_hello函数
t = Timer(5.0, say_hello)
t.start()

# 3秒后取消定时器
Timer(3.0, t.cancel).start()

```

