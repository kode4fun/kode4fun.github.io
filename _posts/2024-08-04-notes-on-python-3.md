---
title:      Python 笔记（三）基本结构
description: python 中列表、元组、集合、字典结构
date:       2024-08-04 00:00:00+0800
author:     kode4fun
categories: [tech,python]
tags:
    - python
---


|  名称   | mutable | ordered | index/slice | Duplicatable | delimiter |
| :-----: | :-----: | :-----: | :---------: | :----------: | :-------: |
| `list`  |    √    |    √    |      √      |      √       |   `[ ]`   |
| `tuple` |    ×    |    √    |      √      |      √       |   `( )`   |
|  `set`  |    √    |    ×    |      ×      |      √       |   `{ }`   |
| `dict`  |    √    |    ×    |      ×      |      ×       |   `{ }`   |

## 列表 list

|                  方法                   | 作用                                           |
| :-------------------------------------: | :--------------------------------------------- |
|              `append(obj)`              | 在列表尾部添加数据                             |
|                `clear()`                | 清除所有列表元素                               |
|                `copy()`                 | 浅拷贝列表                                     |
|             `count(value)`              | 返回列表中指定值出现的次数                     |
|           `extend(iterable)`            | 扩展列表                                       |
| `index(index(value, start=0, stop=MAX)` | 返回列表中指定值第一次出现的下标               |
|          `insert(index,value)`          | 在指定位置插入指定值                           |
|             `pop(index=-1)`             | 移除并返回列表元素                             |
|             `remove(value)`             | 移除列表中第一个出现的指定值（不存在抛出异常） |
|               `reverse()`               | 就地翻转                                       |
|   `sort(*, key=None, reverse=False)`    | 排序                                           |

## 元组 tuple
元组初始化后，**不能增删成员，也不能修改成员的值**


|                  方法                   | 作用                             |
| :-------------------------------------: | :------------------------------- |
|             `count(value)`              | 返回列表中指定值出现的次数       |
| `index(index(value, start=0, stop=MAX)` | 返回列表中指定值第一次出现的下标 |

## 集合 set

集合成员是**无序**且**不重复**的，成员必须是 **hashable** 类型如 `string`、`int`，不能是列表或元组

|             方法              | 作用 |
| :---------------------------: | :--- |
|             `add`             |      |
|            `clear`            |      |
|            `copy`             |      |
|         `difference`          |      |
|      `difference_update`      |      |
|           `discard`           |      |
|        `intersection`         |      |
|     `intersection_update`     |      |
|         `isdisjoint`          |      |
|          `issubset`           |      |
|         `issuperset`          |      |
|             `pop`             |      |
|           `remove`            |      |
|    `symmetric_difference`     |      |
| `symmetric_difference_update` |      |
|            `union`            |      |
|           `update`            |      |

集合支持并（`|`）、交（`&`）、差（`-`）、补（`^`）运算

## 字典 dict

|           方法           | 作用                                          |
| :----------------------: | :-------------------------------------------- |
|        `clear()`         | 移除所有字典成员                              |
|          `copy`          |                                               |
|        `fromkeys`        |                                               |
| `get(key, default=None)` | 返回指定下标的元素值，不存在时默认返回 `None` |
|        `items()`         | 返回所有元素                                  |
|         `keys()`         | 返回所有元素的键                              |
|    `pop(key,default)`    | 移除并返回指定键的值                          |
|       `popitem()`        | 移除并返回最后一个元素                        |
|       `setdefault`       |                                               |
|    `update([E,]**F)`     |                                               |
|        `values()`        | 返回所有元素的值                              |

## 有序序列

有序序列包括 列表、元组、字符串，元组和字符串的成员不可修改。

###  `+` 和 `*` 运算

### 切片 slice
切片语法 `L[start:end:step]` **不包括** `end`

```python
L = list('hello')
print(L[:]) # hello

print(L[1:-1]) # ell

print(L[::-1]) # olleh
```
### 推导式 comprehension

```python
print([x for x in range(10)])
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

print(['A' if x%2==0 else 'B' for x in range(10)])
# ['A', 'B', 'A', 'B', 'A', 'B', 'A', 'B', 'A', 'B']

print([x for x in range(10) if x%2==0])
# [0, 2, 4, 6, 8]
```
