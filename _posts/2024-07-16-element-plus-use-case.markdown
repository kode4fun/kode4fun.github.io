---
layout:     post
title:      "ElementPlus 使用记录"
# subtitle:   "下次换SheetJS"
date:       2024-07-16 00:00:00
author:     "kode4fun"
header-img: "img/post-bg-2015.jpg"
tags:
    - Vue
    - ElementPlus
---

> ElementPlus 库的使用记录

### ElInputNumber
`ElInputNumber` 控件用于数字值的输入，主要设置项有：
+ `controls` 决定是否显示按钮，默认为 `true`
+ `precision` 如果需要输入的是整数，如设置为 1 ，表示保留 1 位小数，若设置为 0 表示整数
+ `max` 和 `min` 用于设置最大、最小值范围
+ `step` 控制步长
+ `step-strictly` 表示严格按照步长值，默认为 `false`

用户在 `ElInputNumber` 中输入不合法值时，会自动按照设定格式调整。  
绑定值改变时会触发 `change` 事件，失去焦点会触发 `blur` 事件  
在用户输入过程中，检测用户输入需使用 `@input.native` 。
```html
<template v-for="(value,index) in [10, 20, 30]">
  序号：{{ index }}
  <el-input-number v-model="scores[index]" 
    :controls="false" 
    :step="1" :step-strictly="true" 
    :min="0" :max="100"
    :precision="0" 
    @input.native="onNativeInut($event,index)" 
    @blur="onBlur($event, index)"
    style="width:60px;margin:10px"
  />
</template>
```

```js
const scores = ref([...new Array(3)].fill(0));
function onNativeInut(newValue,index) {
  console.log(`第 ${index} 个输入的分数是 ${newValue}`);
}
function onBlur(event,index) {
  console.log(`事件：%o，序号${index}`,event);
}
```

在循环中使用 `ElInputNumber` 时，需要注意 `key` 。

### ElBadge
当给 `ElBadge` 绑定 `click` 事件时，需要检测点击的是否是 `ElBadged` 包裹的内部对象。
```html
<el-badge :value="msgNum" :offset="[0, 0]" @click="onBagdeClick($event)" style="cursor:pointer">
  <el-tag size="large">Hello</el-tag>
</el-badge>
```

```js
function onBagdeClick(event) {
  // 如果用户点的不是 badge 的字符区域则跳过
  if (!/is-fixed/g.test(event?.srcElement?.className))
    return;
  console.log(`You clicked %o`,event);
}
```


<!-- <img src="/img/in-post/moon-in-the-sky.jpg" align="center" width="50%"> -->
<!-- ![moon in the sky](/img/in-post/moon-in-the-sky.jpg) -->