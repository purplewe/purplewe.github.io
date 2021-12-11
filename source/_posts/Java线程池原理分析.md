---
title: Java线程池运行分析
date: 2020-8-12 17:09:33
tags:
---
```java
ExecutorService executorService = Executors.newCachedThreadPool();
ExecutorService executorService1 = Executors.newFixedThreadPool(10);


ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(10,
                20,
                10,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(10));
```
创建一个线程池，它的构造函数有四个非常重要的参数

corePoolSize 核心线程数
 
maximumPoolSize 最大线程数

keepAliveTime 线程空闲时间

BlockingQueue workQueue 阻塞队列

就拿我上面创建的那个线程池来说，当有10个以内的任务需要执行时，会在线程池中创建相应的线程执行，当任务数大于10的时候，任务就会进入阻塞队列中，等待空闲的任务执行，当任务数大于20的时候，阻塞队列装不下了，也没有空闲的线程，但是这个时候的线程数是小于最大线程数20的，所以这个时候还会创建线程来执行20后面的任务（不会先执行阻塞队列的任务哦），如果这个时候任务数大于30且线程数已经最大，阻塞队列页塞不下，那么就会抛出RejectedExecutionException异常。这个时候如果有线程空闲就会去执行阻塞队列中的任务，当线程没有任务可以执行并且超过线程空闲时间，那么线程就会被销毁。

![](1.png)
<center>图片来自B站up主free-coder</center>
线程池中创建的线程通过代理的方式执行原来的任务

![](2.png)