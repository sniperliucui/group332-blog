---
title: 分布式计算引擎Ray
categories:
  - 分布式计算
tags:
  - Ray
toc: true
copyright: true
reward: false
top: false
mathjax: true
date: 2023-11-22 23:00:00
description:
---

Ray框架介绍及简单应用[Ray: A Distributed Framework for Emerging AI Applications](https://arxiv.org/pdf/1712.05889.pdf)
<!--more-->

Ray是一个并行和分布式Python的开源库。从高层次上看，Ray生态系统由三部分组成：核心Ray系统、用于机器学习的可扩展库（包括原生库和第三方库），以及用于在任何集群或云提供商启动集群的工具。

Ray是基于Python的分布式计算框架，采用动态图计算模型，提供简单、通用的API来创建分布式应用。使用起来很方便，你可以通过装饰器的方式，仅需要修改极少的代码，让原本运行在单机的Python代码轻松实现分布式计算，目前多用于机器学习。
- 提供用于构建和运行分布式应用程序的简单语言
- 便于用户能够并行化单机代码，代码量修改极少
- Ray Core包括一个由应用程序、库和工具组成的大型生态系统，以支持复杂的应用程序。比如Tune、RLlib、RaySGD、Serve、Datasets、Workflows。


# Python分布式+并行化

一个装饰器就能完成分布式计算。

```python
import ray
import time

ray.init()
N = 100


@ray.remote
def f(x):
    time.sleep(0.1)
    return x * x


def f1(x):
    time.sleep(0.1)
    return x * x


def run_ray():
    start_time = time.time()
    futures = [f.remote(i) for i in range(N)]
    res = ray.get(futures)
    end_time = time.time()
    cost_time = end_time - start_time
    # print(f"res: {res}")
    print(f"run with ray cost time: {cost_time}")
    return cost_time


def run():
    start_time = time.time()
    res = [f1(i) for i in range(N)]
    end_time = time.time()
    cost_time = end_time - start_time
    # print(f"res: {res}")
    print(f"run cost time: {cost_time}")
    return cost_time


if __name__ == '__main__':
    run_ray()
    run()
```

结果如下：1s vs 10s，优势显而易见。
```
2023-12-05 23:48:39,826	INFO worker.py:1673 -- Started a local Ray instance.
run with ray cost time: 1.3737518787384033
run cost time: 10.364514827728271
```

`@ray.remote`既能装饰函数也能装饰一个类：

```python
import ray

ray.init()


@ray.remote
class Counter(object):
    def __init__(self):
        self.n = 0

    def increment(self):
        self.n += 1

    def read(self):
        return self.n


counters = [Counter.remote() for i in range(4)]  # 创建4个远程计数器对象列表
tmp1 = [c.increment.remote() for c in counters]  
tmp2 = [c.increment.remote() for c in counters]
tmp3 = [c.increment.remote() for c in counters]
futures = [c.read.remote() for c in counters]  # 三次操作是异步的
# 等待并获取所有读取操作的结果，由于前面的增加操作是异步的，为了确保它们已经完成
# 使用ray.get来等待所有的读取操作完成并返回结果列表
# 打印读取操作的结果，即四个计数器的当前值。由于每个计数器都被增加了三次，因此结果是[3, 3, 3, 3]
print(ray.get(futures))  # [3, 3, 3, 3]  
```

python并行化，平替版
```python
import multiprocessing
import time

N = 100


def f(x, result_queue):
    time.sleep(0.1)
    result_queue.put(x * x)


def run_multiprocess():
    start_time = time.time()
    result_queue = multiprocessing.Queue()
    processes = []

    for i in range(N):
        process = multiprocessing.Process(target=f, args=(i, result_queue))
        processes.append(process)
        process.start()

    # 等待所有进程结束
    for process in processes:
        process.join()

    # 从队列中获取结果
    results = []
    while not result_queue.empty():
        results.append(result_queue.get())

    end_time = time.time()
    cost_time = end_time - start_time
    # print(f"res: {results}")
    print(f"run with multiprocessing cost time: {cost_time}")
    return cost_time


if __name__ == '__main__':
    run_multiprocess()
```


ray和multiprocess的区别是：
1. 功能和设计目标:
   - multiprocessing：这是 Python 标准库的一部分，提供了基本的进程间通信和并行计算功能。它适用于在单个机器上使用多个进程进行并行计算，但在分布式计算方面支持有限。
   - ray： 这是一个由 RISELab 开发的分布式计算框架，专注于使分布式计算更加容易实现。ray 不仅支持在单个机器上的并行计算，还提供了用于分布式计算的功能，如分布式任务调度、分布式对象存储等。
2. 任务调度和并行模型:
   - multiprocessing：使用基于进程的并行模型，每个进程都有自己的 Python 解释器和内存空间，通信通过进程间通信（IPC）完成。
   - ray： 采用一种更灵活的任务调度模型，其中任务是以函数的形式定义的，而不仅仅是进程。ray 支持任务的动态调度，对象存储和分布式数据流。
3. 对象存储:
   - multiprocessing：进程间通信通常通过序列化和反序列化来完成，但它没有专门的对象存储。
   - ray： 提供了分布式对象存储，可以在不同的任务之间共享大量数据，而无需显式地序列化和传递数据。
4. 分布式计算:
   - multiprocessing：主要用于单机多进程，并不是为了简化分布式计算。
   - ray：提供了更强大的分布式计算能力，可以轻松地在多台机器上分发任务和数据。

# 什么是分布式计算
分布式计算（Distributed computing）是一种科学的计算方法，能把需要进行大量计算的工程数据分割成小块，由多台计算机分别计算，然后再把所有的结果进行合并，得出统一的结果。分布式计算能节约整体计算时间，提高效率。

分布式计算的工作原理是计算机在分布式系统架构中相互传递消息，不同的模块和架构之间相互依赖。这种相互依赖称为耦合，耦合主要有两种类型。
- 松耦合：在松耦合中，组件之间的联系较弱，因此对一个组件的更改不会影响另一个组件。  例如，客户端和服务器计算机可以按时间松散耦合。来自客户端的消息被添加到服务器队列中，客户端可以继续执行其他功能，直到服务器对其消息做出响应。
- 紧密耦合：高性能分布式系统通常使用紧密耦合。快速局域网通常连接多台计算机，从而创建一个集群。在集群计算中，每台计算机都被设置为执行相同的任务。中央控制系统（称为集群中间件）控制和调度任务并协调不同计算机之间的通信。

# 什么是并行计算
并行计算是一种计算类型，其中，网络中的一台计算机或者多台计算机同时执行许多计算或处理。

## 并行计算与分布式计算
并行计算是分布式计算的一种特别紧密耦合的形式。在并行处理中，所有处理器都可以访问共享内存以在它们之间交换信息。另一方面，在分布式处理中，每个处理器都有私有内存（分布式内存）。处理器使用消息传递来交换信息。





