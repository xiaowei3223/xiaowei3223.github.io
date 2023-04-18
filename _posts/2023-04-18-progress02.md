---
layout: post
title: "多线程时如何保证全局变量的访问顺序和安全性"
date: 2023-04-18
description: "多线程或多进程"
tags: 并行运行
--- 

在异步编程中使用concurrent模块（如ThreadPoolExecutor或ProcessPoolExecutor）时，多个线程或进程可能同时修改同一个全局变量，从而导致线程安全问题和程序错误。为了避免这种情况，可以使用锁机制来保证全局变量的访问顺序和安全性。

以下是一个使用锁机制保证全局变量安全的示例代码：
```
import concurrent.futures
import threading

token_lock = threading.Lock()  # 定义锁对象
token = "init_token"  # 全局变量

def update_token(new_token):
    global token
    with token_lock:
        token = new_token

def do_task():
    # 执行任务，需要使用全局变量token
    with token_lock:
        # 使用token执行任务
        pass

# 使用线程池执行任务
with concurrent.futures.ThreadPoolExecutor() as executor:
    # 提交任务到线程池
    for i in range(10):
        executor.submit(do_task)

    # 更新token
    new_token = "new_token"
    executor.submit(update_token, new_token)

```

在这个例子中，我们定义了一个锁对象token_lock，并在访问全局变量token时使用了with token_lock:来获取锁，以确保同一时刻只有一个线程可以访问和修改token。当我们需要更新token时，使用线程池提交一个update_token函数，并将新的token值作为参数传递给它。

需要注意的是，使用锁机制可能会影响程序的性能，因为它会引入额外的开销和竞争。如果能够避免使用全局变量，或者确保全局变量的访问顺序和安全性，可以考虑减少锁机制的使用。