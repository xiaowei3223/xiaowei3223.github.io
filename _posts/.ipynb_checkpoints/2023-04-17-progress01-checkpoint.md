---
layout: post
title: "threading和multiprocessing的区别"
date: 2023-04-17
description: "多线程或多进程"
tags: 并行运行
--- 

threading和multiprocessing都是Python中的模块，用于实现多线程和多进程编程。

主要区别在于：

threading实现的是多线程编程，多个线程共享同一个进程的内存空间，因此线程间的通信和数据共享比较容易。但是由于全局解释器锁（GIL）的存在，所以多线程并不能完全发挥多核CPU的优势。

multiprocessing实现的是多进程编程，每个进程都有自己独立的内存空间，因此进程间的通信和数据共享需要通过特定的方式来实现。但是由于每个进程都有独立的解释器和全局解释器锁，所以可以完全发挥多核CPU的优势。

总的来说，如果需要执行I/O密集型任务，使用threading可以获得比较好的性能表现，而如果需要执行CPU密集型任务，使用multiprocessing会更加高效。



