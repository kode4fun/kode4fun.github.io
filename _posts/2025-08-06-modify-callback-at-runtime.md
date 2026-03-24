---
title:   javascript 运行时修改回调函数
description: 页面加载后，通过控制台代码修改回调函数
date:     2025-08-06 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

## 页面代码

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>运行时修改回调函数</title>
</head>
<body>
  <input type="button" id="doCat" name="doCat" value="doCat">
  <script type="text/javascript">
    // 运行时通过控制台改变函数内部变量和函数
    function cat() {
      let inner = 0;
      console.log(arguments.callee.name,inner);
      // console.log(arguments.callee.toString());
    }

    function onload() {
      const btn = document.querySelector('#doCat');
      btn.onclick = cat;
      console.log(`原回调函数\n`,btn.onclick.toString());
    }
    window.addEventListener("load",onload);
  </script>
</body>
</html>
```
## 控制台代码

```javascript
(function cat2dog()
{
  const reg  = new RegExp(`(?<=let inner = )(?<value>\\d+)`,"g");
  let str_dog = cat.toString().replace(reg,100);
  const dog = eval(`(()=>${str_dog})();`);// 注意语法
  //console.dir(dog);
  cat = dog; // 不能替换掉 cat;因为之前赋值的地址未更新
  const btn = document.querySelector('#doCat');
  btn.onclick = cat; // 重新绑定到新地址
  return 'dog to cat OK';
})();
```

