---
layout:     post
title:      "XLSX库使用中的问题"
subtitle:   "下次换SheetJS"
date:       2024-07-15 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - XLSX
    - xlsx-style-vite
---

> XLSX 历史遗留坑

### 安装 xlsx-style 问题
Vue3+Vite 安装 `xlsx-style` 后，出现 `Can't resolve './cptable' in 'xxx\node_modules_xlsx'` 信息。  
某度出来的要么改源代码，要么改 `vite.config.json`，都行不通，应直接使用 `xlsx-style-vite` 包代替。  
测试环境 vue 3.4.29 + vite 5.3.1。

### XLSX 导出不带公式问题
XLSX 库默认安装下，存在无法将 `.f` 属性的公式导出。需要修改 _node_modules_ 下的 _xlsx.js_ 文件。

![xlsx export with formula](/img/in-post/hack-xlsx-export-with-formula.png)


### 行高问题
当使用 `XLSX-style-vite` 时，通过 `!row` 设置行高失效，需要魔改。

魔改后的 `XLSX-style-vite` 文件点击<a href="/attachments/xlsx-with-formula-and-row.js" download>这里</a>下载。


<!-- ### 测试图片 -->
<!-- <img src="/img/in-post/moon-in-the-sky.jpg" align="center" width="50%"> -->
<!-- ![moon in the sky](/img/in-post/moon-in-the-sky.jpg) -->