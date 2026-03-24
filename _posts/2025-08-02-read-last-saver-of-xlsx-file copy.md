---
title:   python 读取 xlsx 最后保存者信息
description: 用到 openpyxl 模块
date:     2025-08-02 00:00:00+0800
author:   kode4fun
categories: [tech,python]
tags:
  - snippets
  - python
---

> python 读取 xlsx 文件最后保存者信息
{: .prompt-tip }

```python
import os,re
import logging
from openpyxl import load_workbook

logger = logging.getLogger('openpyxl')
logger.setLevel(logging.ERROR)

base = r'PATH_TO_CHECK'
base = os.path.join(os.path.dirname(__file__),base)
print(f'根目录：{base}')
for root,dirs,files in os.walk(base):
  for f in files:
    if re.match(r'^\.xlsx$',os.path.splitext(f)[1],flags=re.I):
      try:
        file = os.path.join(root,f)
        wb = load_workbook(filename=file)
        author = wb.properties.lastModifiedBy
        relpath = os.path.relpath(file,base)
        print(relpath,author,sep='\t')
      except Exception as e:
        print(f'Fail to check file {file}')
```
