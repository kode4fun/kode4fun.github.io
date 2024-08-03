---
layout:     post
title:      "Vue3 快速上手"
subtitle:   "从入门到入坑"
date:       2024-07-07 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Vue
    - Vite
    - Pinia
---

### 编辑器
编辑器首选 `VSCode`，必装插件 `Volar`，选装 `Stylus` 

### 项目初始化
```bash
npm init vue@latest
cd <your-project-name>
cd your-project-name
npm install
npm run dev
```

### EsLint 设置
+ 安装 `npm i -D eslint`
+ 初始化配置 EsLint `npx eslint --init`

### 获取 DOM 节点
在 `template` 的节点内附着 `ref` 属性，  
脚本中使用 `ref` 获取，注意变量名为指定的 `ref` 属性名。
```js
const refAttr = ref(null);
```

### 路由守卫
+ 全局前置守卫 `router.beforeEach`
+ 全局后置守卫 `router.afterEach`
+ 全局解析守卫 `router.beforeResolve`
+ 路由独享守卫 `beforeEnter`
+ 组件内守卫 `beforeRouterEnter`
+ 组件内守卫 `beforeRouterUpdate`
+ 组件内守卫 `beforeRouterLeave`

在每次导航时都会触发，但是确保在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被正确调用

### 文档和资源
+ [vue3 官方文档](https://cn.vuejs.org)
+ [Element-Plus 官网](https://element-plus.org/)
+ [ECharts项目](https://echarts.apache.org/zh/index.html)
+ [Vue3 语法参考](https://juejin.cn/post/7091562373147787277)
+ [EsLint](https://www.jianshu.com/p/4b94540dd998)
+ [在 router 中使用 pinia](https://www.cnblogs.com/xsj1989/p/16712066.html)
+ [@vueuse/core中api记录](https://www.cnblogs.com/musi03/p/15776306.html)
+ [Vue Router 10 条高级技巧](https://blog.csdn.net/qq_41581588/article/details/128686648)
+ [一文搞懂Vue3中watch和watchEffect区别和用法！](https://zhuanlan.zhihu.com/p/528715632)