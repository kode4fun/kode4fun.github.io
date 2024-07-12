---
layout:     post
title:      "Vue3+Vite+Pinia 填坑"
subtitle:   "不是不会，是不完全会"
date:       2024-07-12 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Vue
    - Vite
    - Pinia
---

> Vue3+Vite+Pinia 黄金搭档里的超级大坑。

### 持久化问题
在使用 **setup** 语法写 Pinia 时，`ref` 和 `reactive` 的变量相当于 `state`， `computed` 相当于 `getter`，而 `function` 相当于 `action`，但是需要 `export` 出来才有用，如果光定义而不导出，是没有效果的。  
当配合 _pinia-plugin-persistedstate_ 进行持久化，如果不注意可能导致无法持久化。

### xlsx-style 问题
Vue3+Vite 安装 xlsx-style 后，出现 `Can't resolve './cptable' in 'xxx\node_modules_xlsx'`。  
某度出来的要么改源代码，要么改 _vite.config.json_，都行不通，应直接使用 `xlsx-style` 包代替。  
测试环境 vue 3.4.29 + vite 5.3.1。


### 测试图片
<!-- <img src="/img/in-post/moon-in-the-sky.jpg" align="center" width="50%"> -->
<!-- ![moon in the sky](/img/in-post/moon-in-the-sky.jpg) -->