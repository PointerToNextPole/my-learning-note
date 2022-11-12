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

##### Vue3 文档中的补充

在 Vue3 文档 中有类似的说法：

###### template

> 如果 `render` 选项也同时存在于该组件中，`template` 将被忽略。
>
> 如果应用的根组件不含任何 `template` 或 `render` 选项，Vue 将会尝试使用所挂载元素的 `innerHTML` 来作为模板。

###### render

> `render` 是字符串模板的一种替代，可以使你利用 JavaScript 的丰富表达力来完全编程式地声明组件最终的渲染输出。
>
> 预编译的模板，例如单文件组件中的模板，会在构建时被编译为 `render` 选项。如果一个组件中同时存在 `render` 和 `template`，则 `render` 将具有更高的优先级。

摘自：[Vue3 Doc - API - 渲染选项]()



## 响应式原理



##### Vue2 响应式原理示意图

摘自：[Vue2 Doc - 深入响应式原理](https://v2.cn.vuejs.org/v2/guide/reactivity.html)

<img src="https://s2.loli.net/2022/11/10/fmZebpxQkOAYzd5.png" alt="data" style="zoom:55%;" />

#### Vue2 render 函数

##### 《Vue 组件渲染更新原理》笔记

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

**根据 [[#Vue2 响应式原理示意图]] 中的内容：**

Vue 首先将模版 Template 编译成 `render` 函数，<font color=fuchsia>`render` 函数会返回一个 Vitural DOM</font>，从而进行渲染（ 调用 patch 函数，相应的代码： `patch(container, vnode)` ）。而 `_c` 即 `h` 函数，调用`h(p, 'msg')` （ 完整写法 `h(p, {}, 'msg')` ），会返回一个 vnode 。以上是初次渲染的步骤。

> 👀 h 函数即 `createElement` 。 h 函数的第一个参数，可以是原生的标签，也可以是自定义的组件。第二个参数是一个对象，其中包含各种 attribute。其中比较常见的是 class、style、attrs、props、<font color=fuchsia>**on**</font>（ 👀 包含当前 Element 需要监听的事件） 。  详见 [Vue2 Doc - 渲染函数 & JSX # 深入数据对象](https://v2.cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1)
>
> Vnode 是 Vue 中的一个类，实现代码见 https://github.com/vuejs/vue/blob/dev/src/core/vdom/vnode.js

同时，在初次渲染时，Vue 会将 模版 template 中使用的数据进行 “依赖收集”，从而侦听 使用的数据是否发生变化；如果发生变化，将会触发 `setter` （ 👀 详见 [[#getter / setter 文档摘抄]]），而 `setter` 会通知 ( Notify ) `watcher` （👀 详见 [[#watcher 文档摘抄]]），触发模版的重新渲染 ( Trigger re-render )， 也会再次调用 patch 函数 ( `patch(oldVnode, newVnode) `，⚠️ 注意与第一次调用 patch 写法的区别 )。

##### getter / setter 文档摘抄

> 当你把一个普通的 JavaScript 对象传入 Vue 实例作为 `data` 选项，Vue 将遍历此对象所有的 property，并<font color=red>使用 `Object.defineProperty` 把这些 property 全部转为 getter/setter</font>（👀 访问器属性）。
>
> 摘自：[Vue2 Doc - 深入响应式原理](https://v2.cn.vuejs.org/v2/guide/reactivity.html)

##### watcher 文档摘抄

> <font color=fuchsia>每个组件实例都对应一个 **watcher** 实例</font>，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后<font color=fuchsia>当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染</font>。
>
> 摘自：[Vue2 Doc - 深入响应式原理](https://v2.cn.vuejs.org/v2/guide/reactivity.html)

学习自：[哈默 - Vue组件渲染更新原理](https://www.bilibili.com/video/BV1mU4y1z7S5)

##### Render 函数总结

render 函数的返回值是 vnode，也就是我们要渲染的节点。

render 函数的参数是 createElement 函数，createElement 也有返回值，也是 vnode。

##### createElement 函数有三个参数

1. 一个 HTML 标签字符串，组件选项对象，或者解析上述任何一种的一个 async 异步函数。类型：`{String | Object | Function}`。必需
2. 一个包含模板相关属性的数据对象你可以在 template 中使用这些特性。类型：`{Object}`。可选
3. 子虚拟节点 ( VNodes)，由 createElement() 构建而成，也可以使用字符串来生成“文本虚拟节点”。类型：`{String | Array}`。可选

摘自：[终于搞懂了vue 的 render 函数（一） -_-|||](https://blog.csdn.net/sansan_7957/article/details/83014838)









##### WatchEffect

Vue 提供了 watchEffect 来创建响应式副作用，当依赖发生变化时自动执行（产生作用）。

总结自：[Vue3 Doc - 深入响应式系统](https://cn.vuejs.org/guide/extras/reactivity-in-depth.html)



#### Vue3 渲染机制

从高层面的视角看，Vue 组件挂载时会发生如下几件事：

1. **编译**：Vue 模板被编译为**渲染函数**：即用来返回虚拟 DOM 树的函数。<font color=red>这一步骤可以通过构建步骤提前完成</font>，<font color=red>也可以通过使用运行时编译器即时完成</font>。
2. **挂载**：<font color=lightSeaGreen>运行时渲染器调用渲染函数，遍历返回的虚拟 DOM 树，并基于它创建实际的 DOM 节点</font>。这一步会作为[响应式副作用](https://cn.vuejs.org/guide/extras/reactivity-in-depth.html)执行，因此<font color=red>它会追踪其中所用到的所有响应式依赖</font>。
3. **更新**：<font color=red>当一个依赖发生变化后，副作用会重新运行，这时候会创建一个更新后的虚拟 DOM 树</font>。运行时渲染器遍历这棵新树，将它与旧树进行比较，然后将必要的更新应用到真实 DOM 上去。

<img src="https://s2.loli.net/2022/11/12/WreKwE912tcPnzl.png" alt="render pipeline" style="zoom:60%;" />



#### Vue2 监听对象与数组

##### 对象

Vue 无法检测 property 的添加或移除。由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化（👀 通过 `Object.defineProperty` 设置访问器属性），所以 property 必须在 `data` 对象上存在才能让 Vue 将它转换为响应式的。

对于已经创建的实例，Vue 不允许动态添加根级别的响应式 property。但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式 property。还可以使用 `vm.$set` 实例方法，这也是全局 `Vue.set` 方法的别名。

摘自：[Vue2 Doc - 深入响应式原理 # 对于对象](https://v2.cn.vuejs.org/v2/guide/reactivity.html#对于对象)

##### 数组

<font color=dodgerBlue>**Vue 不能检测以下数组的变动：**</font>

1. 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

举个例子：

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

为了<font color=dodgerBlue>解决第一类问题</font>，<font color=LightSeaGreen>以下两种方式都可以实现和 `vm.items[indexOfItem] = newValue` 相同的效果，同时也将在响应式系统内触发状态更新</font>：

```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// 或者 vm.$set
vm.$set(vm.items, indexOfItem, newValue)
```

```js
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

为了<font color=dodgerBlue>解决第二类问题</font>，你可以使用 `splice` ：

```js
vm.items.splice(newLength)
```

摘自：[Vue2 Doc - 深入响应式原理 # 对于数组](https://v2.cn.vuejs.org/v2/guide/reactivity.html#对于数组)

###### Vue2 监听数组的原理

<font color=fuchsia>Vue 将被侦听的数组的变更方法进行了包裹</font>，所以<font color=fuchsia>它们也将会触发视图更新</font>。这些被包裹过的方法包括：`push()` 、`pop()` 、`shift()` 、`unshift()` 、`splice()` 、`sort()` 、 `reverse()`。

<font color=dodgerBlue>上面说的不够清楚，进一步补充：</font>

> vue 中的<font color=red>数组的监听不是通过 `Object.defineProperty` 来实现的</font>，是<font color=red>通过对 `'push', 'pop','shift','unshift','splice', 'sort','reverse'`这几个改变数组本身的方法执行后来通知监听达到的</font>，源码传送门：https://github.com/vuejs/vue/blob/16700c95e1cff5b28f838562d3ebff8699378998/src/core/observer/array.js#L14
>
> 摘自：[为什么直接修改数组长度或设置数组项的索引时，Vue不能检测到数组的变动？ - 陈小成的回答 - 知乎](https://www.zhihu.com/question/51520173/answer/399438278)

> 为了搞清楚这个问题，我用vue的源码测试了下，下面是vue对数据监测的源码：
>
> <img src="https://s2.loli.net/2022/11/12/9q5jAKLCZnwucGf.png" alt="Observer" style="zoom:95%;" />
>
> 可以看到：<font color=red>当数据是数组时，会停止对数据属性的监测</font>。
>
> <font color=dodgerBlue>GitHub 上提问了尤大</font>：[GitHub - Vue - issue - 为什么vue没有提供对数组属性的监听](https://github.com/vuejs/vue/issues/8562)
>
> <img src="https://s2.loli.net/2022/11/12/TUyQgteMR8nGXuD.png" alt="image-20221112155114144" style="zoom:45%;" />
>
> 摘自：[记一次思否问答的问题思考：Vue为什么不能检测数组变动](https://segmentfault.com/a/1190000015783546)

摘自：[Vue2 Doc - 列表渲染 # 变更方法](https://v2.cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E6%9B%B4%E6%96%B9%E6%B3%95)





#### Vue3 的渲染机制

Vue2 的 Diff 算法是全量对比，而 Vue3 不是。Vue3 采用了静态标记的方法，在编译阶段单独提取了动态节点出来，运行时只要对比动态节点。另外，静态标记是优化 render 时不需要重新生成 vnode，不是优化 diff。

学习自：codingstartup 交流群

> <font color=red>虚拟 DOM 在 React 和大多数其他实现中都是 **纯运行时** 的</font>：更新算法无法预知新的虚拟 DOM 树会是怎样，因此它<font color=fuchsia>总是需要遍历整棵树、比较每个 vnode 上 props 的区别来确保正确性</font>。另外，<font color=LightSeaGreen>即使一棵树的某个部分从未改变，还是会在每次重渲染时创建新的 vnode，带来了大量不必要的内存压力</font>。这也是虚拟 DOM 最受诟病的地方之一：这种有点暴力的更新过程通过牺牲效率来换取声明式的写法和最终的正确性。
>
> <font color=dodgerBlue>但实际上我们并不需要这样</font>。在 Vue 中，框架同时控制着编译器和运行时。这使得我们可以为紧密耦合的模板渲染器应用许多编译时优化。<font color=fuchsia>编译器可以静态分析模板并在生成的代码中留下标记，使得运行时尽可能地走捷径</font>。与此同时，我们仍旧保留了边界情况时用户想要使用底层渲染函数的能力。我们称这种混合解决方案为**带编译时信息的虚拟 DOM**。
>
> 其中包含如下的主要优化：静态提升、更新类型标记、树结构打平。👀 这里略，详见链接。
>
> 摘自：[Vue3 Doc - 渲染机制 # 带编译时信息的虚拟 DOM](https://cn.vuejs.org/guide/extras/rendering-mechanism.html#templates-vs-render-functions)



### 《Vue 3 Deep Dive with Evan You》笔记



##### Vue 的三个核心模块

响应式模块 reactivity module 、编译器模块 compiler module 、渲染模块render module

##### 编译器模块

将 HTML 模板编译成渲染函数 render function<font color=red size=4>**s**</font> ，这样浏览器就可以只接收渲染函数

##### 渲染模块

渲染模块包含在网页上渲染组件的三个不同阶段：渲染阶段 render phase 、挂载阶段 mount phase 、补丁阶段 patch phase

- 渲染阶段，render 函数将会返回一个虚拟 DOM 节点
- 挂载阶段，使用虚拟 DOM 节点，并调用 DOM API 来创建网页
- 补丁阶段，渲染器将旧的 vnode 和新的 vnode 进行比较，并更新需要改变的部分

##### 简单组件执行顺序

首先，模板编译器将 HTML 转换成渲染函数，响应式模块初始化响应对象；