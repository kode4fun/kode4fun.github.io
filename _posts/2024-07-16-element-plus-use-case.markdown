---
layout:     post
title:      "ElementPlus 使用记录"
# subtitle:   "下次换SheetJS"
date:       2024-07-16 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Vue
    - ElementPlus
---

> ElementPlus 库的使用记录

### ElInputNumber
`ElInputNumber` 用于数字类型的输入。  
如果需要输入的是整数，把 `precision` 设置为 `0`。  
默认情况下，_change_ 事件在输入值改变时触发，用户输入过程中不触发。如果需要检测的用户输入过程，需要使用 `@input.native` 。


### XLSX 导出不带公式问题
XLSX 库默认安装下，存在无法将 `.f` 属性的公式导出。需要修改 _node_modules_ 下的 _xlsx.js_ 文件。

![xlsx export with formula](/img/in-post/hack-xlsx-export-with-formula.png)


### XLSX
### 测试图片
<!-- <img src="/img/in-post/moon-in-the-sky.jpg" align="center" width="50%"> -->
<!-- ![moon in the sky](/img/in-post/moon-in-the-sky.jpg) -->