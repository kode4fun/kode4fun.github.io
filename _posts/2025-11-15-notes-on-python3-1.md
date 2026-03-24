---
title:      《Python3学习笔记》读书笔记（一）
description: 变量内存大小、引用计数、弱引用
date:       2025-11-15 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - 读书笔记
    - python
---

## 变量
### 内存大小
Python 使用边长存储数据，可使用 `sys.getsizeof()` 方法获取内存大小（单位字节）。

```python
import sys

a = 1
print(sys.getsizeof(a)) # 28 bytes

b = 1<<1000
print(sys.getsizeof(b)) # 160 bytes

c = True
print(sys.getsizeof(c)) # 28 bytes
```

### 引用计数(P17)
使用 `sys.getrefcount()` 方法获取引用计数。

```python
import sys
class X:pass

a = X()
print(sys.getrefcount(a)) # 2

b = a
print(sys.getrefcount(b)) # 3
```

## 弱引用(P20)
+ 弱引用（weak reference）在保留引用前提下，不增加计数，也不阻止目标被回收，常用于Fianalizer、Watcher 等场景。
+ _int_、_tuple_ 等不支持弱引用
+ 建立弱引用
  + 使用 `ref = weakref.ref(varname,callback)` 建立弱引用时，必须以 `ref().attr` 调用
  + 使用 `ref = weakref.proxy(varname,callback)` 建立弱引用时，可直接 `proxy.attr` 调用
+ 获取弱引用信息
  + 使用 `weakref.getrefcount()` 和 `weakref.getweakrefs()` 获取弱引用信息

```python
import sys,weakref
class X:
    def __init__(self,name):
        self.name = name
    def __del__(self):
        print(f'{self.name} is deleted!')

def callback(target):
    try:
        print('weakref callback is called:',target)
    except Exception as e:
        print('weakref callback exception',str(e))

a = X('Tom')
b = a # b 直接引用 a
print(a.name,b.name) # Tom Tom
print(sys.getrefcount(a)) # 直接引用 3

w = weakref.ref(a,callback) # w 是 a 的弱引用
p = weakref.proxy(a,callback) # p 也是 a 的弱引用
print(w().name) # Tom
print(p.name) # Tom
print(weakref.getweakrefcount(a)) # 弱引用 2

del a,b
```

## 循环引用和垃圾回收
`gc` 模块的以下方法

+ gc.disable()
+ gc.enable()
+ gc.collect()

## 编译
### 反编译代码

```python
import dis
def add(x,y):
    return x+y

print(add.__code__)
print(dis.dis(add))
print(dis.show_code(add))
```

### 手工编译
使用 `compile` 内置函数可将字符串编译为代码，然后使用 `exec` 执行。

### 发布 pyc 文件
使用 `py_compile` 或 `compileall` 模块编译为 *.pyc* 文件，然后使用 `runpy.run_modlue` 运行。（P29）

```bash
python -m compileall .
```

## 内存操作
### 字节顺序(P38)
+ `sys.byteorder` 返回当前系统的字节顺序
+ `x.bit_length()` 返回整数所需的‌最少二进制位数‌（不包含符号位和前导零）
+ `x.to_bytes(n,byteorder)` 返回指定字节长度的数据
+ `int.from_bytes(bytes)` 将字节转为整数

```python
import sys
x = 0b0101_0101
print(bin(x)) # 二进制显示 0b1010101
print(hex(x)) # 十六进制显示 0x55
print(sys.getsizeof(x)) # 28 bytes
print(x.bit_length()) # 7 bits

_bytes = x.to_bytes(2,byteorder = sys.byteorder)
print(_bytes.hex()) # 十六进制显示 5500
print(int.from_bytes(_bytes)) # 21760
print(bin(int.from_bytes(_bytes))) # 0b1010101 00000000
```

### 字节数组(62)
+ _bytes_ 与 _bytearray_ 对比
  + _bytes_ 是不可变序列类型， _bytearray_ 可修改
  + _bytes_ 一次性分配内存，_bytearray_ 长度可变

```python
b = bytes('测试',encoding='utf8')
print(b) # b'\xe6\xb5\x8b\xe8\xaf\x95'
print(b.hex()) # e6b58be8af95
print(b.decode('utf-8')) # 测试

# b = bytearray(b'测试') # b 修饰符只适用于 ASCII 字符
b = '测试'.encode('utf-8')
# b = bytearray([0xe6,0xb5,0x8b,0xe8,0xaf,0x95])
print(b) # bytearray(b'\xe6\xb5\x8b\xe8\xaf\x95')
print(b.hex()) # e6b58be8af95
print(b.decode('utf-8')) # 测试
```

### 内存视图（P63）
+ 内存视图 `memoryview` 可直接引用目标内存，执行读写操作
+ _bytes_、_bytearray_、_array.array_ 等支持内存视图

```python
a = bytearray(b'hello-world')
print(a) # bytearray(b'hello-world')
print(a.decode()) # hello-world

a[:5] = b'HELLO' # 直接通过切片修改内存数组
print(a) # bytearray(b'HELLO-world')
print(a.decode()) # HELLO-world

v = memoryview(a)
print(v.shape) # (11,)
print(v.tobytes()) # b'HELLO-world'
print(v[-5:].tobytes()) # 切片方式查看 b'world'
print(v.tolist()) # [72, 69, 76, 76, 79, 45, 119, 111, 114, 108, 100]

v[-6] = b'+'[0] # 通过内存视图索引修改单个元素
print(v.tobytes()) # b'HELLO+world'
v[-5:] = b'abcde' # 通过内存视图切片修改
print(v.tobytes()) # b'HELLO+abcde'
```

## 类型
### 枚举(P40)
+ Python 枚举通过 _enum_ 模块实现
+ 枚举名不允许重复
+ 如果枚举值重复，总是返回第一个（以 _enum.unique_ 装饰器避免）

```python
import enum
# Fruits = enum.Enum("Fruits","Orange Banana Watermelon") # OK
Fruits = enum.Enum("Fruits",{"Orange":'hello',"Banana":3,"Watermelon":5})

print(Fruits) # <enum 'Fruits'>
print(list(Fruits)) # [<Fruits.Orange: 'hello'>, <Fruits.Banana: 3>, <Fruits.Watermelon: 5>]
my_fruit = Fruits.Orange
print(my_fruit) # Fruits.Orange
print(my_fruit.value) # hello
print(my_fruit == 'hello') # False
print(my_fruit.value == 'hello') # True
```

### 列表(P69)
+ 使用复合赋值运算符时，是 **inplace** 操作
  
```python
a = [1,2,3]
b = a
a = a + [4,5]
print(a is b) # False

c = [1,2,3]
d = c
c += [4,5] # inplace add
print(c is d) # True
```

### 元组(P74)
+ 使用 _collections.namedtuple_ 替代 tuple

```python
import collections,enum
Suit = enum.Enum('Suit','heart spade diamond club')
Card = collections.namedtuple('Card','number suit')
myCard = Card('A',Suit.heart)
print(myCard)
print(myCard.number) # A
print(myCard.suit) # Suit.heart
```

### 数组(P74)
+ 数组以 _array_ 模块实现
+ 数组内成员数据类型必须一致，复合类型必须用 _struct_、_marshal_、_pickle_ 等转换为二进制数组后再存储

```python
import array

buff = array.array('i',[1,2,3])
print(buff) # array('i', [1, 2, 3])
for item in buff:
    print(item) # 1 2 3
buff.extend(range(4,101)) # 延伸数组

v = memoryview(buff) # 通过视图查看数组
for item in v[10:20]:
    print(item)

print(buff.buffer_info()) # 返回内存地址、长度
```
### 字典(P78)
+ _dict.setdefault(key,default)_ 用于当键位设置初始值时，设置并返回初始值
+ 字典默认返回视图(P80)

```python
d1 = {'a':1,'b':2}
t = d1.setdefault('c',3)
print(t) # 3
print(d1) # {'a': 1, 'b': 2, 'c': 3}

d2 = {'b':4,'e':5,'f':6}

ks = d1.keys() & d2.keys() # {'b'}

d1.update({k:d2[k] for k in ks})
print(d1)  # {'a': 1, 'b': 4, 'c': 3}
```
