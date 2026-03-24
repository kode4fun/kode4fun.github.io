---
title:  three.js 学习笔记（三）自定义形状、材质
description: three.js 学习记录
date:     2025-08-12 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
  - three.js
---

## 自定义形状

```js
const geometry = new THREE.BufferGeometry()
const vertices = new Float32Array([
    -1, -1, 0,
    1, -1, 0,
    0, 1, 0
])
geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3))
```

## 材质贴图
### 普通材质

```js
const matStar = new THREE.TextureLoader().load('./textures/star.jpg')
matStar.repeat.set(2, 2) // 重复
matStar.offset.set(0.2, 0.3) // 偏移
matStar.wrapS = THREE.MirroredRepeatWrapping // 水平镜像重复
matStar.wrapT = THREE.RepeatWrapping // 垂直重复
matStar.rotation = Math.PI / 4 // 旋转45度
matStar.center.set(0.5, 0.5) // 设置旋转中心点
// matStar.magFilter = THREE.NearestFilter // 放大采样方式
// matStar.minFilter = THREE.LinearFilter // 缩小采样方式 
// matStar.generateMipmaps = false // 关闭 mipmap 生成
// matStar.anisotropy = 16 // 各向异性过滤

const boxMaterial = new THREE.MeshBasicMaterial({
    color: 0xffff00,
    map: materialStar
})
```

### 透明材质

```js
const matAlpha = new THREE.TextureLoader().load('./textures/star.jpg')
const boxMaterial = new THREE.MeshBasicMaterial({
  map:matStar,
  alphaMap:matAlpha,
  transparent:true,
  opacity:0.3,
  side:THREE.DoubleSide,
}) 
```

### 遮挡贴图


## 参考资料

+ [Three.js企业3D可视化系统项目实战](https://study.163.com/course/introduction/1212491801.htm)
