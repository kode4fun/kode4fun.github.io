---
layout:     post
title:      "Python 的一些小细节"
subtitle:   "容易错过的、容易遗忘的"
date:       2024-08-03 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Python
---

> Python 的一些小细节

### 枚举所有关键字
```python
import keyword
print(keyword.kwlist)
```

### 枚举所有内置对象和函数
```python
print(dir(__builtins__))
```

### 内置数据结构比较

| 名称 | mutable | ordered | index/slice | Duplicatable |
| :---: | :---: | :---: | :---: | :---: |
| list | √ | √ | √ | √ |
| tuple | × | √ | √ | √ |
| set | √ | × | × | √ |
| dict | √ | × | × | × |

### 集合 set
+ 成员必须是不可变类型，如 `string`、`int` 等

### break 语句
