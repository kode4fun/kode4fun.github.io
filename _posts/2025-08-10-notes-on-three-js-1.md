---
title:  three.js 学习笔记（一）搭建开发环境
description: three.js 学习记录
date:     2025-08-10 00:00:00+0800
author:   kode4fun
categories: [tech,javascript]
tags:
  - snippets
  - javascript
  - three.js
---

> three.js 开发环境搭建和程序框架结构。
{: .prompt-tip }

## 搭建开发环境
### 建立本地开发文档

```bash
git clone --depth=1 --branch=dev https://github.com/mrdoob/three.js.git
cd three.js
npm install
npm run start
```
### 初始化工程
初始化工程，安装 `three.js` `parcel` 等模块

```bash
npm init
npm install --save three
npm install --save-dev parcel
```

在项目 `package.json` 中加入启动命令

```json
{
  "scripts":
    {
      "dev":"parcel src/index.html",
      "build":"rd /s /q dist && parcel build src/index.html"
    }
}
```

### 编写代码
项目内添加 `src` 目录，在 `src` 下添加 `index.html` 和 `main/index.js` 文件

```js
import * as THREE from "three"
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"

// 创建场景
const scene = new THREE.Scene()
scene.background = new THREE.Color(0xeeeeee)
// 创建相机
const camera = new THREE.PerspectiveCamera(
    75, // file of vertical
    window.innerWidth / window.innerHeight, // aspect
    0.1, // near
    1000 // far
)
// 设置相机位置
camera.position.set(0, 0, 10)
// 将相机添加到场景
scene.add(camera)

// 辅助坐标轴
const axes = new THREE.AxesHelper(2)
scene.add(axes)

// 建立几何体
const boxGeometry = new THREE.BoxGeometry(1, 1, 1)
// 建立材质
const boxMaterial = new THREE.MeshBasicMaterial({
    color: 0xffff00
})
// 建立物体
const box = new THREE.Mesh(boxGeometry, boxMaterial)
// 将物体添加到场景
scene.add(box)

const renderer = new THREE.WebGLRenderer()
// 设置渲染尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight)

document.body.appendChild(renderer.domElement)
// 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene,camera)

// 建立控制器
const controls = new OrbitControls(camera, renderer.domElement)
// 允许控制器阻尼
controls.enableDamping = true

function render() {
    controls.update()
    renderer.render(scene, camera);
    requestAnimationFrame(render)
};

render();
```

### 编译并运行

```bash
npm run build
npm run dev
```

## 参考资料

+ [Three.js企业3D可视化系统项目实战](https://study.163.com/course/introduction/1212491801.htm)
