---
title:   在 Webworker 中绘制 canvas
description: transferControlToOffscreen 实现 Webworker 绘制
date:     2025-08-09 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
---

> 使用 transferControlToOffscreen 将离屏 canvas 副本传给 Webworker，在 Webworker 中实现绘制。
{: .prompt-tip }

## 页面代码

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas.transferControlToOffscreen</title>
  <style>
    .caption {
      font-weight:bold;
      text-align: center;
      margin-bottom:1rem;
    }
    #myCanvas {
      border:1px solid gray;
      padding:1rem;
      border-radius: 5px;
      display: block;
      margin:0 auto;
    }
  </style>
</head>
<body>
  <div class="caption">
    Canvas transferControlToOffscreen demo
  </div>
  <canvas id="myCanvas" width=800 height=600>
    canvas demo
  </canvas>
  <script>
    function drawSomething() {
      console.time('绘制用时')
      const canvas = document.getElementById("myCanvas");
      const offScreenCanvas = canvas.transferControlToOffscreen();
      const worker = new Worker(`./worker.js?ts=${new Date().getTime()}`);
      // 如果是内嵌 worker 代码，需要使用如下方式使用：
      // const blob = new Blob([document.querySelector('#worker').textContent]);
      // const url = window.URL.createObjectURL(blob);
      // const worker = new Worker(url);

      worker.onmessage = msg => {
        console.log(msg.data);
        console.timeEnd('绘制用时');
      }

      // postMessage 传递 Transferable Objects 需要用到第二个参数
      worker.postMessage({canvas:offScreenCanvas},[offScreenCanvas]);
    }

    window.onload = ()=> drawSomething();
  </script>
</body>
</html>
```

## Webworker 代码

```javascript
const delay = (n=50) => new Promise(resolve=>setTimeout(resolve,n));
const randomColor = ()=> '#' + Math.floor(Math.random()*16777215).toString(16).padStart(6, '0');
const rndFromRange = (a,b) => Number.parseInt(a+Math.random()*(b-a))

const requestAnimationFramePromise = cb =>{
  return new Promise((resolve,reject)=>{
    const cbType = Object.prototype.toString.call(cb).slice(8,-1);
    // 参数 cb 不是函数或异步函数
    if(!['Function','AsyncFunction'].includes(cbType))
      reject();
    requestAnimationFrame(async ()=>{
      let rt = null;
      try {
        rt = await cb();
        await delay();
      } catch(e) {
        reject(e)
      }
      resolve(rt);
    })
  });
}

// 需将 this 绑定到 ctx
const drawCircle = async function (_cx=0,_cy=0,_r=10,color='#00000') {
  this.save()
  this.beginPath();
  this.arc(_cx,_cy,_r,0,2*Math.PI);
  this.fillStyle = this.strokeStyle = color;
  this.stroke();
  this.fill();
  this.restore()
  await delay();
}

onmessage = async(message) =>{
  const promises = [];
  const canvas = message.data.canvas;
  const ctx = canvas.getContext("2d");
  console.log(`canvas.width=${canvas.width} canvas.height=${canvas.height} `);

  for(let n=0;n<100;n++) {
    const r = Number.parseInt(Math.random()*Math.min(canvas.width,canvas.height)/8)
    const cx = rndFromRange(r,Number.parseInt(canvas.width-r))
    const cy = rndFromRange(r,Number.parseInt(canvas.height-r))
    const color = randomColor()
    console.log(`[${n}] (${cx},${cy}),r=${r},color=${color}`)
    const p = requestAnimationFramePromise(drawCircle.bind(ctx,cx,cy,r,color));
    promises.push(p);
  }

  Promise.all(promises).then(()=>{
    postMessage('绘图全部完成！');
  }).catch(e =>console.log(e))
}
```

## 参考资料

+ [我把Canvas放 Webworker中绘制，性能提升200%](https://www.51cto.com/article/816930.html)
+ [Javascript 标准教程](https://www.kancloud.cn/kancloud/javascript-standards-reference/46548)
+ [canvas换图时候会闪烁_OffscreenCanvas-离屏canvas使用说明](https://blog.csdn.net/weixin_39564831/article/details/110597134)
