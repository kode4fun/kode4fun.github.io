---
title:      《Python3学习笔记》读书笔记（二）
description: 闭包、迭代器、生成器、模式
date:       2025-12-06 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - 读书笔记
    - python
---

## 闭包
### 闭包共享自由变量（P160）
+ 返回的多个闭包函数可以共享同一自由变量。

```python
def queue():
    data = []
    push = lambda x:data.append(x)
    pop = lambda :data.pop(0) if data else None
    return push,pop

push,pop = queue()
print(push.__closure__ is push.__closure__) # True

for i in range(3):
    push(i)
    x = pop()
    print(f'{x = }')
```

### 延迟绑定(P162)
+ 闭包只是绑定自由变量，并不会立即计算引用内容。只有当闭包函数执行时，才能访问所引用的目标对象。

```python
# late binding
def make(n):
    x = []
    for i in range(n):
        x.append(lambda:print(i))
    return x

a, b, c = make(3)
a() # 2
b() # 2
c() # 2
```

> 修改策略是将 `lamba:print(i)` 修改为 `lambda o=i:print(o)`。
{: .prompt-tip }

## 迭代器
### 自定义迭代(P175)
+ 可迭代对象要实现 `__iter__` 方法，该方法返回迭代器。

```python
class Data:
    def __init__(self,n):
        self.data = list(range(n))

    def __iter__(self):
        return DataIter(self.data) # 返回迭代器实例

class DataIter:
    def __init__(self,data):
        self.data = data
        self.index = 0

    def __next__(self):
        if not self.data or self.index>= len(self.data):
            raise StopIteration
        d = self.data[self.index]
        self.index += 1
        return d

    def __iter__(self):
      return self

x = Data(2).__iter__()

print(x.__next__()) # 0
print(x.__next__()) # 1
```

### iter 函数(P177)
+ `iter` 函数可以为实现 `__getitem__` 的序列对象自动创建迭代器包装。

```python
class Data:
    def __init__(self,n):
        self.n = n

    def __getitem__(self,index):
        if index<0 or index>=self.n :
            raise IndexError
        return index + 100

d = Data(2)
for x in iter(d): # 可省略 iter
    print(x) # 100 101
```

+ `iter` 甚至可以对函数、方法等 callable 进行包装。

```python
x = lambda :input("n :")

for i in iter(x,'end'): # 输入 end 结束
    print(f'输入的是',i)
```

## 生成器
### yield from(P180)
+ 若数据源本身是可迭代对象，可以使用 `yield from` 直接从子迭代语句返回数据。

```python
def test():
    yield from 'abc'
    yield from range(5)

for i in test():
    print(i)
```

### 生成器双向通信（P183）
+ 使用 `iterator.send()` 方法向迭代器发送数据。
+ 使用 `iterator.throw()` 方法向迭代器发送异常以结束迭代。

```python
class ExitException(Exception):pass
class ResetException(Exception):pass

def test():
    while True:
        try:
            v = yield
            print(f'recv:{v = }')
        except ResetException:
            print('reset...')
        except ExitException:
            print('exit...')
            return

x = test()
x.send(None) # 启动生成器
x.send(1)
x.send(2)
x.throw(ResetException)
x.send(3)
x.send(4)
x.throw(ExitException)
```

## 模式
### 生产者消费者(P186)
+ 首先启动消费者，消费者调用 `yield` 将执行权限让给生产者
+ 生产者生产数据后，通过 `send` 发送给消费者
+ 生产者可以 `throw` 异常给消费者通知数据已生产完毕

```bash
def consumer():
    while True:
        v = yield # 将执行权限交给生产者
        print(f'comsume: {v}')

def producer(c):
    for i in range(10,13):
        c.send(i) # 向消费者发送数据使其消费

c = consumer() # 创建消费者
c.send(None) # 启动消费者

producer(c)
c.close() # 关闭消费者

```

### 消除回调(P187)

```python
import time,threading

def wrapper(worker):
    try:
        s = time.time()
        g = worker()
        result = g.send(None) # 启动生成器任务
        print(f'{result = }') # result 是 worker 执行结果

        time.sleep(2)
        g.send(f'done: {time.time() - s}') # 此次 send 导致 StopIteration
    except StopIteration:
        print('StopIteration')

def service(worker):
    thread = threading.Thread(target = wrapper,args = (worker,))
    thread.start()

# worker 函数必须是生成器函数
def worker():
    print('start...')
    x = yield('in job')
    print(x)

service(worker)
```

### 协程(P189)

```python
from functools import partial
import time,random

def sched(*tasks):
    tasks = list(map(lambda t:t(),tasks))
    while tasks:
        try:
            task = tasks.pop(0) # 从列表头部弹出任务
            task.send(None) # 启动任务
            tasks.append(task) # 任务未结束，放在列表末尾
        except StopIteration:
            pass

def task(id,n,m):
    for i in range(n,m):
        print(f"{id}:{i}")
        yield # 主动调度
        time.sleep(random.random()) # 休眠依旧占据CPU?

t1 = partial(task,1,10,13)
t2 = partial(task,2,30,33)

sched(t1,t2)
```
## 无源码部署(P203)
```shell
# 将当前文件夹下所有文件编译为字节码
python -m compileall -b -o 2 .

# 运行
python main.pyc
```

## 动态导入
### 方法一(P208)
+ 使用 `exec` 动态执行，随后从 `sys.modules` 中获取模块实例

```python
import sys

def LoadModule(name):
    exec(f'import {name}')
    return sys.modules[name]

math = LoadModule('math')
print(math.pi)
```

### 方法二(P209)
+ 使用 `importlib` 库

```python
import importlib

def LoadModule(name):
    return importlib.import_module(name)

math = LoadModule('math')
print(math.pi)
```

