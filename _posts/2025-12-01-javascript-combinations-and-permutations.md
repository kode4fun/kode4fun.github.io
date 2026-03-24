---
title:   javascript 递归函数实现排列组合
description: 递归函数案例
date:     2025-12-01 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

> javascript 递归函数实现排列、组合
{: .prompt-tip }

```javascript
{
    function combinations(x,m) {
        const elements = [...x];
        const n = x.length;
        if(m == 0)
            return [[]];
        else if (m > n)
            return [];
        else if(m==n)
            return [elements];
        const prefix = elements.shift(); // 第一个元素
        const with_first = []
        for(let item of combinations(elements,m-1))
            with_first.push([prefix].concat(item));
        const without_first = []
        for(let item of combinations(elements,m))
            without_first.push(item);
        return with_first.concat(without_first)
    }

    function permutations(x,m) {
        const elements = [...x];
        const n = x.length;
        if (m==0) 
            return [[]];
        else if (m>n)
            return [];
        const perms = []
        for(let i=0;i<x.length;i++) {
            const current = x[i];
            const rest = [...x]
            rest.splice(i,1) // 去除第 i 号元素
            for(let item of permutations(rest,m-1))
                perms.push([current].concat(item));
        }
        return perms;
    }
    const combs = combinations("ABCD",3);
    console.log('组合 ==> %o',combs)

    const perms = permutations("ABCD",3);
    console.log('排列 ==> %o',perms)
}
```
