---
title:      《编写整洁的Python代码》读书笔记（一）
description: 属性的 setter 和 getter、可迭代对象
date:       2025-09-02 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - 读书笔记
    - python
---

## 属性
### getter 和 setter

```python
class User:
  def __init__(self):
    self._age = None

  # getter
  @property
  def age(self):
    return self._age

  # setter
  @age.setter
  def age(self,newValue):
    if newValue<0 or newValue>120:
      raise ValueError('age out of range')
    self._age = newValue

Tom = User()
Tom.age = 20
print(Tom.age)

Tom.age = 150
```

## 可迭代对象
可迭代对象的两种形式

+ 包含 `__iter__` 和 `__next__` 方法的迭代，`StopIteration` 异常
+ 包含 `__len__` 和 `__getitem__` 方法的序列

### 创建可迭代对象
```python
import datetime

class DateRangeContainerIterable:
  def __init__(self,start_date,end_date):
    self.start_date = start_date
    self.end_date = end_date

  def __iter__(self):
    current_day = self.start_date
    while current_day<self.end_date:
      yield current_day
      current_day += datetime.timedelta(days=1)

dr = DateRangeContainerIterable(
  datetime.date(2025,2,15),
  datetime.date(2025,3,15)
)

for day in dr:
  print(day)
```

### 创建序列
```python
import datetime

class DateRangeContainerIterable:
  def __init__(self,start_date,end_date):
    self.start_date = start_date
    self.end_date = end_date
    self._range = self._create_range()

  def _create_range(self):
    days = []
    current_day = self.start_date
    while current_day<self.end_date:
      days.append(current_day)
      current_day += datetime.timedelta(days=1)
    return days

  def __getitem__(self,day_no):
    return self._range[day_no]

  def __len__(self):
    return len(self._range)

dr = DateRangeContainerIterable(
  datetime.date(2025,2,15),
  datetime.date(2025,3,15)
)

for day in dr:
  print(day)
```

## 容器对象

容器对象需实现 `__contains__` 方法，以支持 `in` 运算符

## 动态属性

实现魔术方法 `__getattr__`

```python
class DynamicAttributes:
  def __init__(self,attribute):
    self.attribute = attribute

  def __getattr__(self,attr):
    if attr.startswith('fallback_'):
      name = attr.replace('fallback_','')
      return f'[fallback resolved] {name}'
    raise AttributeError('f{self.__class__.__name__} has no attribute {attr}')

dyn = DynamicAttributes('value')
print(dyn.attribute) # value

print(dyn.fallback_test) # [fallback resolved] test

dyn.__dict__['fallback_new'] = 'new value'
print(dyn.fallback_new) # new value

print(getattr(dyn,'something','default')) # defalt
```
