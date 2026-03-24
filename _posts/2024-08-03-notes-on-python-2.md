---
title:      Python 笔记（二）内置对象和魔术方法
description: python 的内置对象、魔术方法
date:       2024-08-02 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - python
---

> 可使用 `print(dir(__builtins__))` 枚举所有的内置函数和对象
{: .prompt-tip}

> 执行 python 命令时，默认会导入 _site.py_，该文件定义有 `_sitebuiltins._Helper` 等类。  
> 可使用 `-S` 禁止导入 _site.py_。
{: .prompt-tip}

## help
`help` 是 `_sitebuiltins._Helper` 类的实例，用于提取对象的帮助信息。

## 数学运算
`abs` `round` `sum` `min` `max` `divmod` `pow`

## 类型
`int` `float` `complex` `bool` `str` `list` `tuple` `set` `dict` `True` `False` `None`

## 进制转换
`bin` `oct` `hex`

### 属性操作

|           name            | application                                                          |
| :-----------------------: | :------------------------------------------------------------------- |
|      `delattr(obj)`       | Deletes the named attribute from the given object.                   |
|    `getattr(obj,attr)`    | Get a named attribute from an object.                                |
| `setattr(obj,attr,value)` | Sets the named attribute on the given object to the specified value. |
|    `hasattr(obj,attr)`    | Return whether the object has an attribute with the given name.      |


```python
def foo():
    '''
    this is the help info
    '''
    pass

help(foo)
```

## exit / quit
`exit` / `quit` 是 `_sitebuiltins.Quitter` 类的实例，用于在程序任意终止执行。

## len
`len` 获取对象的长度

## id
Return the identity of an object 

## dir
`dir(...)` Show attributes of an object

## type
`type` 可以获取变量的类型，也可以新建类型。

```python
 type(object) -> the object's type
 type(name, bases, dict, **kwds) -> a new type
```

```python
MyClass = type('MyClass', (object,), 
  {
    'attr': 123, 
    'method': lambda self: print('hello')
  }
)
instance = MyClass()
print(instance.attr)
instance.method()
```

## chr / ord / ascii

## any / all

```python
L = [1,2,3,0,2]
print(any(L)) # True
print(all(L)) # False
```

## globals / locals / vars

## isinstance / issubclass

+ `isinstance(isinstance(obj, class_or_tuple)` 
  +  Return whether an object is an instance of a class or of a subclass thereof.
+ `issubclass(cls, class_or_tuple)`       
  + Return whether 'cls' is derived from another class or is the same class.

## bytes / bytearray

## sorted / reversed

+ `sorted(iterable, /, *, key=None, reverse=False)`
  +  Return a new list containing all items from the iterable in ascending order.
+ `reversed(sequence)`
  +  Return a reverse iterator over the values of the given sequence.

## filter

```python
filter(function or None, iterable) --> filter object
```

## map
`map` 完成序列映射

```python
map(func, *iterables) --> map object
```

## repr / str

```python
import datetime
now = datetime.datetime.now()
print(repr(now))
# datetime.datetime(2025, 8, 6, 11, 26, 22, 111043)
print(str(now))
# 2025-08-06 11:26:22.111043
```

## range / enumerate / zip

```python
for index,ch in enumerate('hello'):
  print(index,ch)

for ch in zip(range(5),'hello'):
  print(ch)
```

## iter / next / StopIteration

```python
L = ['apple','banana','peach']

iterator = iter(L)
while True:
  try:
    print(next(iterator))
  except StopIteration:
    break
```

## aiter / anext / StopAsyncIteration

```python
import asyncio

async def g():
  i = 0
  while (i:= i+1)<5:
    yield i
    await asyncio.sleep(1)

async def main():
  generator = aiter(g()) # 与 generator = g() 等价
  while True:
    try:
      v = await anext(generator)
      print(v)
    except StopAsyncIteration:
      break
    except:
      print('Unkown Exception')
      break

asyncio.run(main())
```

## property
## classmethod
## staticmethod
## compile
## eval
## exec
## frozenset
`frozenset` 用于建立一个成员不可变的集合。因为集合的成员必须是不可变类型，普通的集合是不能作为另一个集合的成员的。

```python
S1 = {'a','b'}
S2 = {'A','B'}

S1.add('c')
print(S1) # {'a', 'b', 'c'}

# S1.add(S2) # unhashable type: 'set'
S1.add(frozenset(S2))
print(S1)
# {'a','b','c', frozenset({'A', 'B'})}
```

## 其它

|    method    | application |
| :----------: | :---------- |
| `breakpoint` |             |
|  `callable`  |             |
|   `format`   |             |
|    `hash`    |             |
| `memoryview` |             |
|   `slice`    |             |
|    `hash`    |             |


## magic methods
### 构造与析构
+ `__new__(cls, ...)` ： 用于创建对象实例，在 `__init__` 之前调用，cls是类本身。
+ `__init__(self, ...)` ： 对象的初始化方法，在 `__new__` 之后调用，用于设置对象属性。

### 一元操作符
+ `__neg__(self)` ： `-` 运算符(取反)
+ `__pos__(self)` ： `+` 运算符(正值)
+ `__abs__(self)` ： `abs()` 函数(绝对值)
+ `__invert__(self)` ： `~` 运算符(按位取反)
+ `__complex__(self)` ： `complex()` 函数(转换为复数)
+ `__int__(self)` ： `int()` 函数(转换为整数)
+ `__float__(self)` ： `float()` 函数(转换为浮点数)
+ `__bool__(self)` ： `bool()` 函数(转换为布尔型)
+ `__round__(self, n)` ： `round()` 函数(四舍五入)

### 二元操作符
+ `__add__(self, other)` ： `+` 运算符
+ `__sub__(self, other)` ： `-` 运算符
+ `__mul__(self, other)` ： `*` 运算符
+ `__truediv__(self, other)` ： `/` 运算符(真除法)
+ `__floordiv__(self, other)` ： `//` 运算符(整除)
+ `__mod__(self, other)` ： `%` 运算符(取模)
+ `__pow__(self, other)` ： `**` 运算符(幂运算)
+ `__and__(self, other)` ： `&` 运算符(按位与)
+ `__or__(self, other)` ： `|` 运算符(按位或)
+ `__xor__(self, other)` ： `^` 运算符(按位异或)
+ `__lshift__(self, other)` ： `<<` 运算符(左移位)
+ `__rshift__(self, other)` ： `>>` 运算符(右移位)

### 比较操作符
+ `__lt__(self, other)` ： `<` 运算符(小于)
+ `__le__(self, other)` ： `<=` 运算符(小于等于)
+ `__eq__(self, other)` ： `==` 运算符(等于)
+ `__ne__(self, other)` ： `!=` 运算符(不等于)
+ `__gt__(self, other)` ： `>` 运算符(大于)
+ `__ge__(self, other)` ： `>=` 运算符(大于等于)

### 属性访问
+ `__getattr__(self, name)` ： 当访问不存在的属性时调用。
+ `__delattr__(self, name)` ： 删除属性。
+ `__property__(self)` ： 定义属性的 getter，setter，deleter。

### 容器类相关
+ `__len__(self)` ： len() 函数(返回容器长度)
+ `__getitem__(self, key)` ： `[ ]` 运算符(获取元素)
+ `__setitem__(self, key, value)` ： `[ ]` 运算符(设置元素)
+ `__delitem__(self, key)` ： del 关键字(删除元素)
+ `__iter__(self)` ： iter() 函数(返回迭代器)
+ `__next__(self)` ： next() 函数(迭代器的下一个元素)
+ `__contains__(self, item)` ： `in` 运算符(成员测试)

### 表示与转换
+ `__str__(self)` ： `str()` 函数(可读性字符串表示)
+ `__repr__(self)` ： `repr()` 函数(解释器字符串表示)


### 其它

|    magic method     | apply to |    magic method    | apply to |
| :-----------------: | :------: | :----------------: | :------: |
|      `__dir__`      |   dir    |                    |          |
|     `__hash__`      |   hash   |     `__doc__`      |   help   |
|     `__class__`     |          | `__subclasshook__` |          |
|    `__format__`     |          |                    |          |
|   `__getstate__`    |          |                    |          |
| `__init_subclass__` |          |                    |          |
|    `__reduce__`     |          |  `__reduce_ex__`   |          |
|    `__sizeof__`     |          |                    |          |

