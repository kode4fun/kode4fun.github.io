---
title:   javascript 生成可重复随机数
description: 使用相同种子，输出相同的随机数
date:     2025-08-04 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

> javascript 生成可重复随机数
{: .prompt-tip }

```javascript
{
  function* mulberry32(seed) {
    let t = seed += 0x6D2B79F5;
    // Generate numbers indefinitely
    while(true) {
      t = Math.imul(t ^ t >>> 15, t | 1);
      t ^= t + Math.imul(t ^ t >>> 7, t | 61);
      yield ((t ^ t >>> 14) >>> 0) / 4294967296;
    }
  }
  const delay = (n) => new Promise(resolve=>setTimeout(()=>resolve(),n));

  const run = async() => {
      const g = mulberry32(100);
      for(let n=0;n<10;n++) {
        await delay(1000).then(msg=>msg && console.log(msg));
        console.log(g.next().value);
      }
      console.log('Done!');
    };
  run();
}
```
