---
title:      Python 笔记（一）语法要点
description: python 语言的赋值、循环等语法要点
date:       2024-08-02 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - python
---

## 语言特性
### 获取相关信息
```python
import sys,keyword
print(sys.version) # 获取 python 版本
print(sys.version_info) # 获取 python 版本
print(keyword.kwlist) # 所有关键字
print(dir(__builtins__)) # 枚举所有内置对象和函数
```

### 解包

解包时，可使用 `*` 指代不定项目的成员

```python
a,*b,c = 'hello'
print(b) # ['e', 'l', 'l']
```

## 流程控制
### for/while...else 结构
只有 **循环正常结束** 时，其后面的 `else` 才会被执行，否则不会执行

```python
for i in range(5):
  print(i,end='    ')
  if i==3: break
else:
  print('in else')
# 0    1    2    3
```

### break 语句
只能跳出当前层循环
