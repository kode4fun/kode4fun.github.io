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

### Vue3 中使用 Map 类型
使用 `ref` 或 `reactive` 包裹 _Map_ 类型是可行的。但是，不能使用 `Object.entries` 来遍历。  
JS 中遍历的方法如下：
```js
const students = reactive(new Map());
students.value.set('1233456',{...});
// 使用 for ... of 结构便利
for(let [stdId,student] of students.value) {

}
console.log(students.value.size); // map 的成员个数
```

模板中的遍历方式
```html
<div v-for="[index,value] in students">
<!-- index 是序号，value 为数组 -->
<!-- 数组第一项 value[0] 为 map 的 key, 第二项 value[1] 为 object -->
</div>
```

### 路由文件内使用 pinia 的问题
路由文件内，需要注意 store 的引入位置。进入路由前，`pinia` 未初始化，不可以使用。
```js
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'homeview',
      component: HomeView,
      children: [
        {
          beforeEnter: async (to, from, next) => {
            // 不能在外面使用 pinia，路由初始化时，pinia 未初始化
            const { myTestStore: MTStore } = useStore();
            const { isUserLogined } = storeToRefs(MTStore);
            if (!MTStore.isUserLogined())
              next({ path: from.path }); //  注意： next(false) 只是中断路由，并不返回
            else
              next();
          },
          path: '/main-content',
          name: 'main-content',
          component: () => import('@/components/MainContent.vue')
        },
      ],
    },
});
```

### Vue3 中 404 路由设置
Vue3 与 Vue2 在 404 路由语法上不兼容。
```js
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/:pathMatch(.*)*', // 404 页面
      redirect: '/404',
    },
  ]
});
```


### 懒加载模块
懒加载脚本，若提供了默认导出，可使用如下案例：

```js
let myFunc = null
async function lazyLoadScript() {
    if(!myFunc)
        myFunc = await import('./path-to-script/myscript.js'); // 提供默认导入
}

```
若脚本未提供默认导出，则按照下面的方式引入：

```js
let myFunc = null
async function lazyLoadScript() {
    if(!myFunc)
        await import('./path-to-script/myscript.js')
            .then(module=>myFunc = module.myFunc); // 未提供默认导入
}

```

### 样式穿透
语法是 `:deep(...)`，如 `:deep(.el-iput__content)`。
### 测试图片
<!-- <img src="/img/in-post/moon-in-the-sky.jpg" align="center" width="50%"> -->
<!-- ![moon in the sky](/img/in-post/moon-in-the-sky.jpg) -->