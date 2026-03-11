# Vue3-源码分析

> version：3.4.33

### 一. 源码下载⬇️、打包、调试

[vuejs/core: 🖖 Vue.js is a progressive, incrementally-adoptable JavaScript framework for building UI on the web.](https://github.com/vuejs/core)

```sh
# 安装依赖
pnpm install
pnpm run dev
# 输出内容
> built: packages\vue\dist\vue.global.js
```

其后在`packages/vue/example`文件夹中创建自己的调试文件夹进行调试

<img src="./images\vue调试文件夹位置.png" style="margin-left: 0; width: 300px" />

测试代码案例如下

```html
![vue3整体架构](D:\study_work\work\面试准备\vue\images\vue3整体架构.png)<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>test</title>
</head>
<body>
  <div id="app">
    <h1>计数器案例</h1>
    <p>当前计数: {{ count }}</p>
    <button @click="count++">增加</button>
    <button @click="count--">减少</button>
  </div>
</body>

<script src="../../dist/vue.global.js"></script>

<script>
  const { ref, createApp } = Vue
  const App = {
    setup() {
      const count = ref(0)
      function increment() {
        count.value++
      }
      function decrement() {
        count.value--
      }
      return {
        count,
        increment,
        decrement
      }
    }
  }
  createApp(App).mount('#app')
</script>

</html>
```

---

### 二. Vue3的整体架构

<img src="./images\vue3整体架构.png" style="width: 70%; margin-left: 0;" /><img src="D:\study_work\work\面试准备\vue\images\vue3整体架构2.png" style="width: 30%; height: 50%; margin-left: 0;" />

---

### 三. Vue3源码分析

#### 1. 😊 createVNode 的过程 (h函数的本质)

🐴小马口述总流程: 

- h函数 -> createVNode -> _createVNode -> createBaseVNode -> 创建Vnode对象

<img src='./images\h函数原理.png' style="margin-left: 0;"/>

---

#### 2. 😊 createApp(app).mount('#app') 的过程

🐴小马口述总流程: 

- 通过ensureRenderer()函数实例化渲染器，调用渲染器中的createApp(...args)方法获得 app 对象。并在其中拓展app中的mount方法，返回app；.mount('#app')调用拓展的mount()方法。

- ensureRenderer() 则会调用createRenderer()继而调用baseCreateRenderer()来创建渲染器

  <img src="./images\ensureRenderer.png" style="margin-left: 0;">

- createApp() 即渲染器baseCreateRenderer() 返回的createApp -> createAppAPI(render,hydrate)('#app')中返回app实例(柯里化)

  <img src="./images\createApp.png"/>

- 调用 .mount('#app') 挂载到目标渲染对象

  <img src='./images/app-mount重写.png' />

