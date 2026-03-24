---
title:  three.js 学习笔记（二）基本动画、clock、gsap
description: three.js 学习记录
date:     2025-08-11 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
  - three.js
---

## 基本动画

```js
// 位置
box.position.set(x,y,z) 

// 旋转以 **弧度** 为单位
box.rotation.set(x=0,y=0,z=0,order='XYZ') // 默认 order=XYZ

// 缩放
box.scale.set(x=0,y=0,z=0)
```

## clock

## GSAP

`GSAP` 是一个用于创建补间动画的库。

### 安装

```bash
npm install --save gsap
```

### gsap.to

```js
import { gsap } from 'gsap'

// gsap.to 返回动画对象
const animation = gsap.to(box.position,{
  x : 5, // 属性及值
  duration : 10, // 动画时长
  ease : 'power1.inOut', // 缓动类型
  onStart:cb, // 动画启动时回调函数
  onComplete : cb, // 动画结束时的回调函数
  repeat : 5, // 重复次数，无限次为 -1
  yoyo :  true, // 往返运动
  delay : 2, // 延迟时间 2s
})

gsap.to(box.rotation,{x:Math.PI,duration:5})
```
### 动画对象

## 画面控制
### 画布自适应窗口
当浏览器窗口改变尺寸时，让画布自适应窗口大小。

```js

window.addEventListener("resize",()=>{
  // 更新摄像头
  camera.aspect = window.innerWidth / window.innerHeight;
  // 更新投影矩阵
  camera.updateProjectionMatrix()
  // 更新渲染器
  renderer.setSize(window.innerWidth,window.innerHeight)
  // 设置渲染器像素比
  renderer.setPixelRatio(window.devicePixelRatio)
})
```

### 全屏

```js
if(document.fullscreenElement)
  renderer.domElement.requestFullscreen(true)
else
  document.exitFullscreen(true)
```

## dat.gui
### 安装

```bash
npm install --save dat.gui
```

### gui.add
```js
import * as dat from 'dat.gui'

const gui = new dat.GUI()
// 改变 box.position 中的 x 属性
gui.add(box.position,"x")
  .min(0) // 最小 0
  .max(10) // 最大 10
  .step(0.1) // 步长 0.1
  .name('box_position_x')
  .onChange(cb) // 值修改时的回调函数
  .onFinishChange(cb)
```

### gui.addColor

```js
const params = {
  color:0xff0000
}

//  改变 obj 的 color 属性
gui.addColor(params,'color')
  .name('box_material_color')
  .onChange(newColor => box.material.color.set(newColor))
```

### 回调函数作为参数

```js
const params = {
  fn = ()=>{}
}

gui.add(params,'fn');
```



### gui.addFolder

``` js
const gui = new dat.GUI()
const guiApperance = gui.addFoler('外观')
guiApperance.add(....)

guiApperance.open() // 展开文件夹
```

## 参考资料

+ [Three.js企业3D可视化系统项目实战](https://study.163.com/course/introduction/1212491801.htm)
+ [GSAP 官方文档](https://gsap.com/docs/v3/)
+ [dat.GUI 官方文档](https://github.com/dataarts/dat.gui/blob/master/API.md)
