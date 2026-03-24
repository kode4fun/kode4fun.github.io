---
title:      《Python3学习笔记》读书笔记（三）
description: 类
date:       2025-12-09 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - 读书笔记
    - python
---

## 类定义
### 私有属性(P233)
+ 以两个下划线 `_` 开头的属性不会被重命名，继承类可以访问，只起到提示为私有属性作用。
+ 以两个下划线 `__` 开头的属性会被重命名，且继承类也无法访问。

```python
class Parent:
    def __init__(self):
        self._a = 1 # 私有属性
        self.__b = 1 # 私有属性，子类不可访问

class Son(Parent):
    def __init__(self):
        super().__init__()
        print(self._a) # OK
        # print(self.__b) # AttributeError

Tom = Son()
```

### 相关函数(P234)
+ `hasattr(obj, name)` 
+ `getattr(obj, name, default)`
+ `delattr(obj, name)` 

### getter 和 setter(P235)

```python
class X:
    def __init__(self,name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,value):
        self._name = value

    @name.deleter
    def name(self):
        raise AttributeError('Cannot delete attribute name')

o = X('dog')
print(o.name) # dog
o.name = 'cat'
print(o.name) # cat
del o.name # AttributeError
```

### 类型方法和静态方法(P240)
+ 类型方法使用 `@classmethod` 装饰器，第一个参数为 `cls`
+ 静态方法使用 `@staticmethod` 装饰器，不绑定 `cls` 和 `self`

```python
class Dog:
    num = 0 # 类属性
    def __new__(cls,*args):
        cls.num = cls.num + 1 # 注意：可使用 cls 作为前缀
        print(f'__new__ called:',*args)
        return super().__new__(cls)

    def __init__(self,name):
        self.name = name
        print(f'__init__ called:',name)
        # Dog.num = Dog.num + 1 # 注意：此处要用类名作为前缀

    @classmethod # 类方法
    def getDogNum(cls):
        return f'{cls.num} Dog(s)'

    @staticmethod # 静态方法
    def sayHello():
        return f"Hello！"

tom = Dog('Tom')
jack = Dog('Jack')

print(tom.getDogNum())
tom.sayHello()
```
## 继承
### 获取继承关系(P245)
+ 子类以 `__base__` 获取基类
+ 基类以 `__subclasses__` 获取基类

## 抽象类(P254)
+ 抽象类不能实例化
+ 抽象类必须继承 `ABC` ，或使用 `ABCMeta` 元素

```python
from abc import ABC,abstractmethod

class Animal(ABC):
    @abstractmethod
    def bark(self):...

class Dog(Animal):
    def bark(self):
        print('汪汪汪~~')

class Cat(Animal):
    def bark(self):
        print('喵喵喵~~')

tom = Dog()
kit = Cat()
tom.bark()
kit.bark()
```

## 运算符重载

### `__repr__`

### `__call__` 

### `__item__` 相关
+ `__getitem__`
+ `__setitem__`
+ `__delitem__`

### `__attr__` 相关
+ `__getattr__`
+ `__setattr__`
+ `__delattr__`
