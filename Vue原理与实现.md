# Vue 原理与实现



## Vue 实例生命周期



##### Vue2 实例生命周期示意图

摘自：[Vue2 Doc - Vue 实例](https://v2.cn.vuejs.org/v2/guide/instance.html)

<img src="https://v2.cn.vuejs.org/images/lifecycle.png" alt="Vue 实例生命周期" style="zoom:52%;" />

##### outerHTML、Template 和 render 函数 优先级

如下代码：

```vue
<div id="app">
  <p>{{ msg }}</p>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data() {
      return { msg: 'OuterHTML' };
    },
    template: `<div>Template</div>`,
    render(h) {
      return h('div', {}, 'Render')
    }
  });
</script>
```

会发现：“Render” 显示，去掉 render 函数后，“Template” 显示，去掉 template 后，“Outer HTML” 显示；即从优先级上： <font color=fuchsia>render 函数 > template  > outer HTML</font>。因为无论是 outer HTML 还是 template 最后都会编译成 render 函数；所以，如果提供 render 了，则不会再使用 outer HTML 和 template。

以上的优先级 对应着 [[#Vue2 实例生命周期示意图]] 图中 “created 生命周期” 下方 “是否有 el 选项” ( Has “el” option? ) 的判断：如果有的话，还会判断 “是否有 template 选项” ( Has “template” option?  ) 。如果有的话，会直接使用 template 中的内容 ( Compile template into render function )；没有则退而求其次，使用 el 选项对应的 outer HTML ( Compile el’s outer HTML as template )

学习自：[哈默 - Vue原来是这样使用我们的模板（template）的！](https://www.bilibili.com/video/BV1Df4y1Q7KK)





## 响应式原理



##### Vue2 响应式原理示意图

摘自：[Vue2 Doc - 深入响应式原理](https://v2.cn.vuejs.org/v2/guide/reactivity.html)

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

下面打印 `App.vue` 对应的 render 函数：

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

其中 `_c` 表示 `createElement`（ ⚠️ 不是 `document.createElement` ，注意区分） ， `_v` 表示 `createTextVNode`（ ⚠️ 不是 `document.createTextNode` ）， `_s` 表示 `toString` 。

根据 [[#Vue2 响应式原理示意图]] 中的内容：

Vue 首先将模版 Template 编译成 `render` 函数，<font color=fuchsia>`render` 函数会返回一个 Vitural DOM</font>，从而进行渲染（ 调用 patch 函数，相应的代码： `patch(container, VNode)` ）。而 `_c` 即 `h` 函数，即 `h(p, 'msg')` ，完整写法 `h(p, {}, 'msg')` ，会返回一个 VNode 。以上是初次渲染的步骤。

> h 函数即 `createElement` 。 h 函数的第一个参数，可以是原生的标签，也可以是自定义的组件。而第二个参数是一个对象，其中包含各种 attribute。其中比较常见的是 class、style、attrs、props、<font color=fuchsia>**on**</font>（ 👀 包含当前 Element 需要监听的事件） 。  详见 [Vue2 Doc - 渲染函数 & JSX # 深入数据对象](https://v2.cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1)

同时，在初次渲染时，Vue 会将 模版 template 中使用的数据进行 “依赖收集”，从而侦听 使用的数据是否发生变化；如果发生变化，将会触发 `setter` （ 👀 关于 getter / setter 见下面），而 `setter` 会通知 ( Notify ) `watcher` ，触发模版的重新渲染 ( Trigger re-render )， 也会再次调用 patch 函数 ( `patch(oldVnode, newVnode) `，⚠️ 注意与第一次调用 patch 写法的区别 )。

> 当你把一个普通的 JavaScript 对象传入 Vue 实例作为 `data` 选项，Vue 将遍历此对象所有的 property，并<font color=red>使用 `Object.defineProperty` 把这些 property 全部转为 getter/setter</font>。
>
> 摘自：[Vue2 Doc - 深入响应式原理](https://v2.cn.vuejs.org/v2/guide/reactivity.html)

学习自：[哈默 - Vue组件渲染更新原理](https://www.bilibili.com/video/BV1mU4y1z7S5)

