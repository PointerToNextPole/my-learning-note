# Vue 原理与实现



## 响应式原理



##### Vue2 响应式原理示意图

摘自：[]

<img src="https://s2.loli.net/2022/11/10/fmZebpxQkOAYzd5.png" alt="data" style="zoom:60%;" />

#### render 函数

```vue
<!-- App.vue -->
<template>
  <div id="app">
    <p>{{msg}}</p>
    <button @click="onBtnClick">btn</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return { msg: 'msg' }
  },
  methods: {
    onBtnClick() { this.msg = 'msg is changed' }
  }
}
</script>
```

下面打印 `App.vue` 对应的 rendr 函数：

```js
var render = function render() {
  var _vm = this,
    _c = _vm._self._c
  return _c("div", { attrs: { id: "app" } }, [
    _c("p", [_vm._v(_vm._s(_vm.msg))]),
    _c("button", { on: { click: _vm.onBtnClick } }, [_vm._v("btn")]),
  ])
}
var staticRenderFns = []
render._withStripped = true

export { render, staticRenderFns }
```

其中 `_c` 表示 `createElement` ， `_v` 表示 `createTextVNode` ， `_s` 表示 `toString` 。

根据 [[#Vue2 响应式原理示意图]] 中的内容：

Vue 首先将模版 Template 编译成 `render` 函数，`render` 函数会返回一个 Vitural DOM，从而进行渲染（ 调用 patch 函数 ）。而 `_c` 实现效果类似于 `h` 函数，即 `h(p, 'msg')` ，会返回一个 VNode 。以上是初次渲染的步骤。

同时，在初次渲染时，Vue 会将 模版 template 中使用的数据进行 “依赖收集”，从而侦听 使用的数据是否发生变化；如果发生变化，将会触发 `setter` ，而 `setter` 会通知 ( Notify ) `watcher` ，触发模版的重新渲染 ( Trigger re-render )， 也会再次调用 patch 函数。

学习自：[【我是哈默】Vue组件渲染更新原理【Vue小知识】](https://www.bilibili.com/video/BV1mU4y1z7S5)

