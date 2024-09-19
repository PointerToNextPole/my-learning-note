# Vue3 笔记



## Coderwhy Vue3 + TS



#### Vue3带来的变化

##### 源码

###### 源码通过monorepo的形式来管理源代码

Mono：单个；Repo : repository：仓库。主要是将许多项目的代码存储在同一个repository中，这样做的目的是多个包本身相互独立，可以有自己的功能逻辑、单元测试等,同时又在同一个仓库下方便管理；而且模块划分的更加清晰，可维护性、可扩展性更强

###### 源码使用TypeScript来进行重写

- 在 Vue2.x 的时候，Vue使用 Flow 来进行类型检测;
- 在 Vue3.x 的时候，Vue的源码全部使用 TypeScript 来进行重构，并且 Vue 本身对 TypeScript 支持也更好了;

##### 性能

###### 用Proxy进行数据劫持

- 在Vue2.x 的时候，Vue2 是使用 Object.defineProperty 来劫持数据的 getter和 setter 方法的
- 这种方式一致存在一个缺陷就是当给对象添加或者删除属性时，是无法劫持和监听的
- 所以在 Vue2.x 的时候，不得不提供一些特殊的 API，比如 `$set` 或 `$delete`，事实上都是一些 hack 方法，也增加了开发者学习新的 API 的成本
- 而在 Vue3.x 开始，Vue 使用 Proxy 来实现数据的劫持，这个 API 的用法和相关的原理我也会在后续讲到;

###### 删除了一些不必要的API

- 移除了实例上的 \$on、\$off 和 \$once
- 移除了一些特性：如filter、内联模板等

###### 包括编译方面的优化

- 生成 Block Tree、Slot 编译优化、diff 算法优化

##### 新的 API

###### 由 Options API到Composition API

- 在 Vue2.x 的时候,我们会通过 Options API 来描述组件对象;

- Options API 包括 data、props、methods、computed、生命周期等等这些选项;

- 存在比较大的问题是多个逻辑可能是在不同的地方:
  - 比如created中会使用某一个 method 来修改 data 的数据,代码的内聚性非常差

- Composition API 可以将相关联的代码放到同一处进行处理,而不需要在多个Options之间寻找

###### Hooks 函数增加代码的复用性

- 在 Vue2.x 的时候，我们通常通过 mixins 在多个组件之间共享逻辑
- 但是有一个很大的缺陷就是 mixins 也是由一大堆的Options组成的，并且多个 mixins 会存在命名冲突的问题
- 在 Vue3.x 中，我们可以通过 Hook 函数，来将一部分独立的逻辑抽取出去，并且它们还可以做到是响应式的
- 具体的好处,会在后续的课程中演练和讲解（包括原理）

##### Vue3 的 data和 Vue2 的 data 是不同的

Vue3 的 data 属性必须是一个函数，<font color=FF0000>**否则会报错**</font>。而在Vue2 中不是这样。

```js
// Vue3
data() {
	return {...}
}
```



#### template 属性

##### template属性：表示的是Vue需要帮助我们渲染的模板信息

- 目前我们看到它里面有很多的HTML标签，这些标签会替换掉我们挂载到的元素（比如id为app的div）的 innerHTML
- 模板中有一些奇怪的语法：比如 {{}}、比如@click，这些都是模板特有的语法,我们会在后面讲到;

但是这个模板的写法有点过于别扭了,并且IDE很有可能没有任何提示,阻碍我们编程的效率.

##### Vue提供了两种方式

- **使用script标签，并且标记它的类型为 x-template，并设置id**

  ```html
  <script type="text/x-template" id="foo">
  	<div>
    	<h2>{{foo}}</h2>
    	<button @click="onClick">button</button>
    </div>
  </script>
  
  <script>
  	Vue.createApp({
      template: '#foo',
      ...
    })
  </script>
  ```

  这个时候，在createApp的对象中,我们需要传入的 template 以 `#  `开头：
  如果字符串是以 `#` 开始，那么它将被用作 querySelector，并且使用匹配元素的innerHTML作为模板字符串

- **使用任意标签（通常使用 template 标签，因为template标签 不会被浏览器渲染），设置id**。

  另外，虽然可以用任意标签，但是如果用 div 标签的话，会被渲染两次（浏览器解析一次，Vue再解析挂载一次）

  template元素是一种用于保存客户端内容的机制，该内容再加载页面时不会被呈现，但随后可以在运行时使用 JavaScript实例化

  ```html
  <template id="foo">
  	<div>
    	<h2>{{foo}}</h2>
    	<button @click="onClick">button</button>
    </div>
  <template>
  
  <script>
  	Vue.createApp({
      template: '#foo',
      ...
    })
  </script>
  ```

  

#### 阅读 Vue3 源码

如果想要学习 Vue 的源码，比如看createApp的实现过程，应该怎么办？步骤如下：

1. 在 GitHub 上搜索 vue core，下载源代码。这里推荐通过 git clone 的方式下载

2. 安装 Vue 源码项目相关的依赖。执行 `yarn install`

3. 对项目执行打包操作。执行 `yarn dev`。

   另外，执行前修改脚本，加上 `--sourcemap` ，否则看到的是一个完整的打包后文件，而不是开发时，模块层级的代码。

   ```js
   "scripts": {
   	"dev": "node srcript/dev.js --sroucemap"
   }
   ```

4. 通过 packages/vue/dist/vue.global.js 调试代码

**另外，在开始 研究源码 时，再看一遍视频（Lesson1 最后）**



<font color=fuchsia>**在 Vue 的 methods 中，函数定义不可以使用 箭头函数 `foo: () => { ... }`，如果使用，那么其中的 this 就是 window 对象**</font>。

这里涉及到箭头函数使用 this 的查找规则：箭头函数中是不绑定 this 的，如果使用 this，它会在自己的上层作用域中来查找this；其上层的 methods的大括号，以及再上层的 Vue.create() 中的大括号，都是写的对象，不构成作用域。所以，<font color=FF0000>最终刚好找到的是 script 作用域中的 this，所以就是 window</font>。



#### v-pre 和 v-cloak

##### v-pre

跳过元素和它的子元素的编译过程，显示原始的 Mustache 标签。

即：让当前元素中的差值表达式失效，即：`<p>{{foo}}</p>` 显示都内容还是 `{{foo}}`

> 👀 注：看了下MDN 的 [ \<pre>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre) 文档，感觉 v-pre 应该就是借鉴的 \<pre> 标签；感觉可以辅助记忆。

##### v-cloak

这个指令保持在元素上直到关联组件实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到组件实例准备完毕。

```html
<div v-cloak>{{ message }}</div>

<style>
[v-cloak] {
  display: none;
}
</style>
```

即：在渲染时可能会出现，差值表达式没有来得及转换成“真值”，这时候就会显示 {{foo}}；添加这个属性，可让差值表达式所在的元素，在转换成“真值”前不显示，直到转换成功再显示。




#### v-bind

##### Vue 中 v-bind 绑定 class

数组中是可以添加三元运算符，当然更好的方法是 数组中嵌套对象。

```html
<div :class="['foo', 'bar', isBaz? 'baz': '']"></div>
<!-- 数组中嵌套对象 是更好的方法 -->
<div :class="['foo', 'bar', {baz: isBaz, qux: isQuz}]"></div>
```

##### Vue中 v-bind 绑定 style

style中可以写驼峰也可以写短横线分隔 (kebab-case)，但是短横线分隔必须要加上引号：

```html
<div :style="{color: myColor, fontSize: '30px'}">aaa</div>
<div :style="{color: myColor, 'font-size': '30px'}">aaa</div>
```

另外，style 对象中的键值对，值是引用类型或者是一个字符串。如果写如下代码且data中没有定义，将会报错：`:style="color: red"`，因为 `red` 是一个变量，且没有定义。应该写成 `:style="color: 'red'"`

##### v-bind 直接绑定对象

```html
<div v-bind="foo">foo</div>
<script>
	const App = {
    data() { 
      return { 
        foo: { bar: 'bar', baz: 'baz', qux: 'qux' } 
      } 
    }
  }
</script>
```

等价于：

```html
<div bar="bar" baz="baz" qux="qux">foo</div>
```

另外，上面的 `v-bind` 也是可以省略的：

```html
<div :="foo">foo</div>
```

一般用于高级组件开发中传递配置，或者是父组件向子组件中传递多个值，而用对象直接封装（见下面的 "**父子组件通信方式**" ）。

##### Vue3 新特性：`v-bind()` 可以用于 style 标签中

`v-bind()` 可以用于 `<style>` 标签，如下示例：

```vue
<template>
  <div class="text">hello</div>
</template>

<script>
export default {
  data() {
    return { color: 'red' }
  }
}
</script>

<style>
.text { color: v-bind(color); } // 👀
</style>
```

同样，这个语法 也适用于 `<script setup>`

如上摘自：[Vue3 成为默认版本后新文档 - SFC CSS Features - v-bind() in CSS](https://vuejs.org/api/sfc-css-features.html#v-bind-in-css)，另外，可以参考：[Vue3 \<style>状态驱动 CSS 变量](https://www.qiyuandi.com/zhanzhang/zonghe/17302.html)

> 💡 在实际项目中，出现了 `<style>` 中 `v-bind` 中需要根据条件选择不同的值的情况，在 codeium 的帮助下写了出来；效果如下：
>
> ```css
> min-width: v-bind("props.isChannel ? '250px' : '420px' ")
> ```



#### 模板语法：Vue3 新文档的补充

( 👀 v-bind ) 如果绑定的值是 `null` 或者 `undefined`，那么该 attribute 将会从渲染的元素上移除。

> 👀 注：这点是之前不知道的

##### 受限的全局访问

模板中的表达式将被沙盒化，仅能够访问到 [有限的全局对象列表](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsWhitelist.ts#L3)。该列表中会暴露常用的内置全局对象，比如 `Math` 和 `Date`。

> 👀 注：上面  [有限的全局对象列表](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsWhitelist.ts#L3) 的链接这里做一下摘抄和思考：
>
> > ```js
> > const GLOBALS_WHITE_LISTED =
> >   'Infinity,undefined,NaN,isFinite,isNaN,parseFloat,parseInt,decodeURI,' +
> >   'decodeURIComponent,encodeURI,encodeURIComponent,Math,Number,Date,Array,' +
> >   'Object,Boolean,String,RegExp,Map,Set,JSON,Intl,BigInt'
> > ```
> >
> > 可以发现：这里的 `GLOBALS_WHITE_LISTED` 是没有 `window` / `global` 的，这也说明了：为什么在 template 里面使用 `console.log()` 会报错说找不到，因为 `console.log()` 属于 DOM，属于 `window` / `global` 的，所以会报错。

没有显式包含在列表中的全局对象将不能在模板内表达式中访问，例如用户附加在 `window` 上的属性。然而，你也可以自行在 `app.config.globalPropezrties` 上显式地添加它们，供所有的 Vue 表达式使用。

##### 参数 Arguments

某些指令会需要一个“参数”，在指令名后通过一个冒号隔开做标识。例如用 `v-bind` 指令来响应式地更新一个 HTML attribute：

```vue
<a v-bind:href="url"> ... </a>

<!-- 简写 -->
<a :href="url"> ... </a>
```

这里 `href` 就是一个参数，它告诉 `v-bind` 指令将表达式 `url` 的值绑定到元素的 `href` attribute 上。在简写中，参数前的一切 (例如 `v-bind:` ) 都会被缩略为一个 `:` 字符。

另一个例子是 `v-on` 指令，它将监听 DOM 事件：

```vue
<a v-on:click="doSomething"> ... </a>

<!-- 简写 -->
<a @click="doSomething"> ... </a>
```

> 👀 注：这里 `v-bind:attrName` 和 `v-on:eventName` 就是 `v-directive` 中 `binding.arg` 的一种；这是之前完全没有想到的... 🥬
>
> 同样的，事件修饰符也是。

摘自：[Vue3 新文档 - 模板语法](https://cn.vuejs.org/guide/essentials/template-syntax.html)



#### v-on

##### v-on 修饰符

- .stop - 调用event.stopPropagation()。
- .prevent - 调用event.preventDefault()。
- .capture - 添加事件侦听器时使用capture 模式。
- .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- .{keyAlias} - 仅当事件是从特定键触发时才触发回调。
- .once - 只触发一次回调。
- .left - 只当点击鼠标左键时触发。
- .right - 只当点击鼠标右键时触发。
- .middle - 只当点击鼠标中键时触发。
- .passive - { passive: true } 模式添加侦听器

##### v-on 绑定对象用于绑定多个事件

```html
<div v-on="{click: onClick, mousemove: onMouseMove}">div</div>
```

这里的 v-on 也可以使用简写

##### v-on传递参数，尤其是事件对象

- 如果没有函数带参数，会默认的、隐式的传递一个 event 对象。

  ```html
  <button @click='onClick'>button</button>
  
  <script>
    const app = {
      methods: {
        onclick() { console.log(event) } 
      }
    }
  </script>
  ```

- 带参数，则需要显式的传递event对象，且必须要用$event：

  ```html
  <button @click='onClick($event, 'param')'>button</button>
  
  <script>
    const app = {
      methods: {onclick(evnet, param) { console.log(event) } }
    }
  </script>



#### v-if & v-show

##### v-if 的渲染原理

v-if 是惰性的

- 当条件为false时，其判断的内容完全不会被渲染或者会被销毁掉

- 当条件为true时，才会真正渲染条件块中的内容

##### v-show 和 v-if 的区别

- **首先，在用法上的区别：**v-show 是不支持 template

- **其次，本质的区别：**
  - v-show 元素无论是否需要显示到浏览器上，它的 DOM 实际都是有渲染的，只是通过 CSS 的 display 属性来进行切换；
  - v-if 当条件为 false 时，其对应的原生压根不会被渲染到 DOM 中；

- **开发中如何进行选择：**

  - 如果我们的原生需要在显示和隐藏之间频繁的切换，那么使用 v-show；

  - 如果不会频繁的发生切换，那么使用 v-if



#### key & diff

##### key 的作用

- key属性主要用在Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes；

- 如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法

- 而使用key时，它会基于key的变化重新排列元素顺序，并且会移除 / 销毁 key 不存在的元素

##### diff 算法

Vue中对于列表的更新究竟是如何操作的？Vue<font color=FF0000>会对于有key和没有key会调用两个不同的方法；</font>

- **有key：**使用 **patchKeyedChildren** 方法

  1. 第一步的操作是从头开始进行遍历、比较：

     - a和b是一致的会继续进行比较

     - c和f因为key不一致，所以就会break跳出循环

     ![image-20211111170725995](https://i.loli.net/2021/11/11/3UMV4JaHsxBiX2Y.png)

  2. 第二步的操作是从尾部开始进行遍历、比较：

     ![image-20211111170822169](https://i.loli.net/2021/11/11/mQPA2o739BeVwba.png)

  3. 第三步是如果旧节点遍历完毕，但是依然有新的节点，那么就新增节点：

     ![image-20211111171103821](https://i.loli.net/2021/11/11/phkJWbngCjfOQDa.png)

  4. 第四步是如果新的节点遍历完毕，但是依然有旧的节点，那么就移除旧节点：

     ![image-20211111171122906](https://i.loli.net/2021/11/11/8pX1PJFDycwV4Bq.png)

  5. 第五步是最特色的情况，中间还有很多未知的或者乱序的节点：

     ![image-20211111171322649](https://i.loli.net/2021/11/11/ozys67AfvJ4TgND.png)

- **没有key：**使用 **patchUnkeyedChildren** 方法

另外，这里可以重新看下Lesson3的最后，又对diff的一些讲解



#### computed

##### 计算方法可以写成函数，也可以写成对象

- 如果写成函数，则默认是getter方法（可以说：这种方法是下面写成对象方式的语法躺）
- 如果写成对象，则既可以在里面写getter方法，也可以在里面写setter方法

```js
computed: {
  fullName() {
    return firstName + ' ' + lastName
  }
  
  otherFullName: {
    get: function() {
      return firstName + ' ' + lastName
    },
    set: function(newValue) {
      this.firstName = newValue
    }
  }
}
```

Vue3不支持过滤器，不过：推荐两种做法：使用计算属性，或者 使用全局的方法

全局方法是指：在 app.config.globalProperties 对象中定义的方法



#### 侦听器 watch

##### 用法

选项：watch

类型：`{ [ key: string ] : string | Function | Object | Array }`

- **函数式 ( Function ) ：**最常用

  ```js
  info(newValue, oldValue) {
    console.log('newValue', newValue, 'oldValue', oldValue)
  }
  ```

- <font color=FF0000>**对象形式（Object）：**</font>如果想要加上deep和immedate配置还是需要使用Object形式。可以说：函数式的watch是对象形式watch的一种语法糖

  ```js
  info: {
    handler: function(newValue, oldValue) {
      console.log('newValue', newValue, 'oldValue', oldValue)
    },
    deep: true, // 深度监听
    immediate: true // 一般监听器初始时，因为数据没有变化，所以不会执行（触发）。这个属性让其，在初始化时立即执行（触发）
  }
  ```

  <font color=FF0000>**另外，需要注意：**</font>当变更（ 不是替换 ）对象或数组并使用 deep 选项时，旧值将与新值相同，因为它们的引用指向同一个对象/数组。Vue 不会保留变更之前值的副本。

- **string 类型：**是：`watchedValue: 'someMethod'` ，其中 watchValue 是被侦听的数据，someMethod 在methods 中被定义。

- <font color=FF0000>**Array 类型：**</font>一个属性可以被多个函数侦听，这些函数会按照执行顺序，依次执行。

**另外，如果想要深度侦听对象中的某一个成员，不一定使用 deep配置，也可以使用如下方法：**

```js
watch() {
	"info.aMember": function(newValue, oldValue) {
    console.log('newValue', newValue, 'oldValue', oldValue)
  }
}
```

Vue 会自动解析 字符串中的内容。

##### \$watch 

```js
// 还有一个返回值 unwatch
const unwatch = this.$watch("info", function(newValue, oldValue) {
    console.log('newValue', newValue, 'oldValue', oldValue)
}, {
  deep: true,
  immediate: true
})

// 一旦调用unwatch 就会停止监听
unwatch()
```

关于 `$watch` 详见：https://v3.cn.vuejs.org/api/instance-methods.html#watch，其中值得注意的有：

```js
// 用于监视单个嵌套property 的函数
this.$watch(
  () => this.c.d,
  (newVal, oldVal) => {
    // 做点什么
  }
)

// 用于监视复杂表达式的函数
this.$watch(
  // 表达式 `this.a + this.b` 每次得出一个不同的结果时处理函数都会被调用。这就像监听一个未被定义的计算属性
  () => this.a + this.b,
  (newVal, oldVal) => {
    // 做点什么
  }
)
```

关于watch，更多的可以参考官方文档中的：https://v3.cn.vuejs.org/api/options-data.html#watch，这里有更多示例

```js
const app = createApp({
  data() {
    return {
      a: 1,
      b: 2,
      c: {
        d: 4
      },
      e: 5,
      f: 6
    }
  },
  watch: {
    // 侦听顶级 property
    a(val, oldVal) {
      console.log(`new: ${val}, old: ${oldVal}`)
    },
    // 字符串方法名。这里的someMethods定义在 methods中
    b: 'someMethod',
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler(val, oldVal) {
        console.log('c changed')
      },
      deep: true
    },
    // 侦听单个嵌套 property
    'c.d': function (val, oldVal) {
      // do something
    },
    // 该回调将会在侦听开始之后被立即调用
    e: {
      handler(val, oldVal) {
        console.log('e changed')
      },
      immediate: true
    },
    // 你可以传入回调数组，它们会被逐一调用
    f: [
      'handle1',
      function handle2(val, oldVal) {
        console.log('handle2 triggered')
      },
      {
        handler: function handle3(val, oldVal) {
          console.log('handle3 triggered')
        }
        /* ... */
      }
    ]
  },
  methods: {
    someMethod() {
      console.log('b changed')
    },
    handle1() {
      console.log('handle 1 triggered')
    }
  }
})

const vm = app.mount('#app')

vm.a = 3 // => new: 3, old: 1
```



#### v-model

**v-model 实际上是一个语法糖。v-model 的原理其实是背后有两个操作：**

- v-bind 绑定 value属性的值

- v-on 绑定 input 事件监听到函数中，函数会获取最新的值赋值到绑定的属性中

```html
<input v-model="searchText" />
<!-- 等价于 -->
<input :value="searchText" @input="searchText = $event.target.value" />
```

##### lazy 修饰符的作用

- **默认情况下**：<font color=FF0000>v-model 在进行双向绑定时，绑定的是 input 事件</font>，那么会在每次内容输入后就将最新的值和绑定的属性进行同步

- 如果我们<font color=FF0000>在 v-model 后加上 lazy 修饰符，那么会将绑定的事件切换为 change 事件</font>，<font color=FF0000>只有在提交时（比如回车）才会触发</font>

> 💡 这里涉及到一个优化点：如果 v-model 的输入与动画相关，显然这会导致动画卡顿，因为默认的 input 事件，导致每一次输入都会导致 v-model 修改，从而导致渲染发生。
>
> 如果说给 v-model 加上 lazy 修饰符，每一次输入都不会导致 v-model 修改，则不会出现类似的问题。不过，也有小的弊病：如果不失焦，虽然 input 了，但是 v-model 始终不变，这也会导致潜在的问题（虽然，很少遇到类似的场景）

学习自：[Vue常用指令V-model如何优化？【渡一教育】](https://www.bilibili.com/video/BV1284y127pn)



#### SFC 顶层 template 下的顶层标签

Vue3中，之所以 \<template> 标签下，可以添加多个标签，而不再需要一个根标签；是因为：Vue 把 tempate 里面的内容，当作代码片段 fragment 来处理；也就是说：Vue 会自动加上 fragment

> 💡 Fragment 是 Vue3 的一个新组件，主要用来解决上面 Vue2 的痛点。



#### webpack

webpack 是一个静态的模块化打包工具，为现代的 JavaScript 应用程序：

- **打包bundler：**webpack可以将帮助我们进行打包，所以它是一个打包工具
- **静态的static：**这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器）
- 模块化module：webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等
- 现代的modern：我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了webpack的出现和发展

webpack-cli 的作用是：在命令行中使用webpack，比如 `webpack --entry ./src/index.js` 需要对命令进行读取，所以需要webpack-cli。

##### webpack、webpack-cli之间的关系

- 执行webpack命令，会执行node_modules下的.bin目录下的webpack

- webpack在执行时是依赖webpack-cli的，如果没有安装就会报错

- 而webpack-cli中代码执行时，才是真正利用webpack进行编译和打包的过程

- 所以在安装webpack时，我们需要同时安装webpack-cli（第三方的脚手架事实上是没有使用webpack-cli的，而是类似于自己的vue-service-cli的东西）

##### webpack.config.js

webpack.config.js 中的 output 下 path 对应的值必须是绝对路径，所以需要使用 path.resolve() 做拼接，来获取绝对路径

webpack.config.js 是webpack打包时默认读取的名字，如果修改了这个名字（比如 webpack.conf.js ），需要修改 npm scripts：

```json
"scripts": {
  "build": "webpack --config webpack.conf.js"
}
```

npm scripts 中的命令，比如`"serve": "webpack serve"` ，webpack会到node_modules 下面的 .bin文件下去找 webpack（和npx 一样），而后执行。

##### webpack是如何对项目进行打包的

- 事实上webpack在处理应用程序时，它会根据命令或者配置文件找到入口文件

- <font color=FF0000>**从入口开始，会生成一个依赖关系图，这个依赖关系图会包含应用程序中所需的所有模块**</font>（比如.js文件、css文件、图片、字体等）

- 然后遍历图结构，打包一个个模块（根据文件的不同使用不同的loader来解析）

webpack 默认（没有配置webpack.config.js 的情况下），只能对js代码进行解析（新的ES，似乎还是要借助 babel），对于css不能进行解析（会报错）。所以，需要使用 css-loader 对 css 模块进行解析。

但是，<font color=FF0000 size=4>**仅仅使用 css-loader 对css 文件进行加载是不足以让css样式 显示到页面上的，因为css-loader 只是做了解析，不会将解析之后的页面插入到页面中，如果希望将css 插入到 style中，还需要 style-loader**</font>

**如何使用loader来加载文件呢？有三种方式（这里不一定是css文件，也可以是其他格式文件）：**

- **内联方式：**内联方式使用较少，因为不方便管理。在引入的样式前加上使用的loader，并且使用!分割；

  ```js
  // 之前
  import "./css/style.css"
  
  // 使用内联方式：
  import "css-loader./css/style.css"
  ```

- **CLI方式：**webpack5中不再使用
  - 在webpack5的文档中已经没有 --module-bind 了
  - 实际应用中也比较少使用，因为不方便管理

- **配置方式：**最常用。

##### loader 配置方式

配置方式表示的意思是在 webpack.config.js 文件中写明配置信息：

- module.rules 中允许我们配置多个loader（因为我们也会继续使用其他的loader，来完成其他文件的加载）

- 这种方式可以更好的表示loader的配置，也方便后期的维护，同时也让你对各个Loader有一个全局的概览

##### module的 rules 的配置

rules属性对应的值是一个数组：[Rule]。数组中存放的是一个个的Rule，Rule是一个对象，对象中可以设置多个属性：

- **test属性：**用于对resource（资源）进行匹配的，通常会设置成正则表达式

- **use属性：**对应的值时一个数组：[UseEntry]

  - UseEntry是一个对象，可以通过对象的属性来设置一些其他属性

    - loader：必须有一个loader属性，对应的值是一个字符串
    - options：可选的属性，值是一个字符串或者对象，值会被传入到loader中；

    - query：目前已经使用options来替代

  - 传递字符串（如：use: [ 'style-loader' ]）是loader 属性的简写方式（如：use: [ { loader: 'style-loader'} ]）；

- **loader属性：**Rule.use: [ { loader } ] 的简写

##### PostCSS 工具

###### 什么是 PostCSS？

PostCSS是一个通过 JavaScript 来转换样式的工具。<font color=FF0000>这个工具可以帮助我们进行一些CSS的转换和适配，比如自动添加浏览器前缀、css样式的重置；但是<font size=4>**实现这些功能，我们需要借助于PostCSS对应的插件**</font>。</font>

###### 如何使用 PostCSS ？主要就是两个步骤：

1. 查找PostCSS在构建工具中的扩展，比如 webpack 中的 postcss-loader

2. 选择可以添加你需要的 PostCSS 相关的插件

**也可以在 终端中 使用 PostCSS：**需要 安装工具：[postcss-cli](https://github.com/postcss/postcss-cli)。

如果要使用 PostCSS 的 autoprefixer 插件，需要使用有两种方法：

- **使用命令行（相当麻烦）：**

  ```sh
  npx postcss --use autoprefixer -o end.css './src/scc/style.css'
  ```

- **使用 postcss-loader，以及配置 webpack.config.js：**

  ```js
  use: [
    "style-loader",
    "css-loader",
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [
            require('autoprefixer')
          ]
        }
      }
    }
  ]
  ```

  另外，由于postcss-loader 中的配置文件内容是相当多的，所以可以抽取出来，放入 postcss.config.js 中：

  ```js
  // postcss.config.js
  module.exports = {
    plugins: [
      require('autoprefixer')
    ]
  }
  ```

  这时候，webpack.config.js 中只需要像 css-loader 一样 ( `"css-loader"` )，写成 `"postcss-loader"` 即可。

**事实上，在配置 postcss-loader 时，我们配置插件并不需要使用autoprefixer，我们可以使用另外一个插件：postcss-preset-env。**

postcss-preset-env 也是一个 postcss 的插件，它可以帮助我们将一些现代的CSS特性，转成大多数浏览器认识的CSS，并且会根据目标浏览器或者运行时环境添加所需的polyfill；也包括会自动帮助我们添加autoprefixer（所以相当于已经内置了autoprefixer）；

> 💡 在 [[前端工程化笔记#PostCSS]] 中，也有一些笔记。



#### file-loader

file-loader的作用就是：<font color=FF0000>帮助我们处理 import() / require()方式引入的一个文件资源，并且会将它放到我们输出的文件夹中</font>。

如下代码在打包（比如使用 file-loader 进行打包）时，会出现问题：

```js
const imgEl = document.createElement('img')
imgEl.src = '../static/image/foo.png'
document.body.appendChild(imgEl)
```

由于这里的路径使用的是相对路径（相对于index.html ），所以，最后打包的时候，这里面的路径也是相对路径；而打包后的文件已经被放到 dist / build 文件夹了，会导致资源找不到的情况。原因就是这里 `img.src` 被赋的值是一个字符串，那么在打包后， `img.src`  的值仍将是这个字符串。<font color=FF0000>为了解决这种情况，需要使用 模块引入，将图片以模块的形式引入</font>：

```js
import fooImage from '../static/image/foo.png'

const imgEl = document.createElement('img')
imgEl.src = fooImage
document.body.appendChild(imgEl)
```

##### file-loader 打包后的文件命名

可以使用PlaceHolders来完成，webpack给我们提供了大量的PlaceHolders来显示不同的内容：

我们可以在文档中查阅自己需要的placeholder：https://webpack.js.org/loaders/file-loader/#placeholders

**这里介绍几个最常用的placeholder：**

- [ext]：处理文件的扩展名

- [name]：处理文件的名称

- [hash]：文件的内容，使用MD4的散列函数处理，生成的一个128位的hash值（32个十六进制）；

- [contentHash]：在file-loader中和[hash]结果是一致的（在webpack的一些其他地方不一样，后面会讲到）；

- [hash:\<length>]：截图hash的长度，默认32个字符太长了；

- [path]：文件相对于webpack配置文件的路径；

**配置示例：**

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  use: {
    loader: "file-loader",
    options: {
      name: "img/[name]_[hash:8].[ext]" // 表示放在img 文件夹下。如果相对文件夹名称单独设置，见下面一行注释的代码。
      // outputPath: "img"
    }
  }
}
```

另外，想要对文件名进行设置，还可以在output 对象中使用 `assetModuleFilename` 设置：

```js
output: {
  ...,
  assetModuleFilename: "img/[name]_[hash:8].[ext]"
}
```

##### webpack@5 asset module type 资源模块类型

在webpack5之前，加载这些资源我们需要使用一些loader，比如 raw-loader 、url-loader、file-loader

在webpack5开始，我们可以直接使用<font color=FF0000>**资源模块类型 ( asset module type )**</font>来替代上面的这些loader（<font color=FF0000>因为是内置的，所以，甚至不需要再下载 loader 了</font>）

**资源模块类型 ( asset module type )，通过添加 4 种新的模块类型，来替换所有这些 loader：**

- **<font color=FF0000>passet/resource</font>：**发送一个单独的文件并导出URL。之前通过使用 <font color=FF0000>**file-loader**</font> 实现

- **<font color=0000FF>asset/inline</font>：**导出一个资源的data URL 。之前通过使用 <font color=0000FF>**url-loader**</font> 实现

- **asset/source：**导出资源的<font color=FF0000>源代码</font>。之前通过使用 raw-loader 实现（一般用于字体文件）

- **asset：**<font color=FF0000>在导出一个data URI 和发送一个单独的文件之间自动选择</font>。之前<font color=FF0000>通过使用 url-loader，并且配置资源体积限制实现</font>

  ```js
  {
    test: /\.(png|jpe?g|gif|svg)$/i,
    type: 'asset',
    generator: {
      filename: "img/[name]_[hash:6].[ext]"
    },
    parser: {
      dataUrlConditon: {
  	    maxSize: 100 * 1024
      }
    }
  }
  ```



#### webpack plugins

Loader是用于特定的模块类型进行转换；

Plugin可以用于执行更加广泛的任务，比如打包优化、资源管理、环境变量注入等

在webpack.config.js 中的 plugins部分，是一个数组，数组中包含的是一个个插件对象。

```js
plugins: [
  new CleanWebpackPlugin(),
  new HtmlWebpackPlugin()
]
```

在使用 HtmlWebpackPlugin时，如果没有index.html 作为模板，也能在打包后的文件夹中生成一个index.html； 实现原理是：包含ejs，是通过ejs模板 生成 index.html 文件。

写入下面的配置，并进行打包，将会报错，因为：模块中BASE_URL 的常量没有定义。因为在编译模板时，有一个BASE_URL：`<link rel="icon" href="<%= BASE_URL %>favicon.ico">` ，这是网页图标的地址；但是目前没有该常量值。这是要使用 webpack 自带的插件 DefinePlugin：

```js
plugins: [
  new DefinePlugin({
    BASE_URL: "'./'""
  })
]
```

另外，模板的 title 也是可以设置的。它是通过 `htmlWebpackPlugins.options.title` 设置的 `<title><%= htmlWebpackPlugins.options.title %></title> `，这时可以通过 htmlWebpackPlugin 的 title 参数进行设置：

```js
plugins: [
  new HtmlWebpackPlugin({
    template: "./public/index.html", // 模版的文件位置
    title: 'myTitle' // 自定义的title
  })
]
```

##### CopyWebpackPlugin

在vue的打包过程中，如果我们将一些文件放到public的目录下，那么这个目录会被复制到dist文件夹中。这个复制的功能（的实现机制 / 原理），我们可以使用CopyWebpackPlugin来完成。

```js
new CopyWebpackPlugin({
	// 匹配的文件夹
  pattern: [
    {
      from: 'public', // 从哪里复制
      to: './', // 复制到哪里。因为该插件会读取 webpack配置的上下文，会看到output.path 指向的位置；这里是一个相对路径，相对于 output.path 指向的文件夹。
      globalOptions: {
        ignore: [
          "**/index.html", // 忽略哪些文件，这里忽略的是index.html，因为他作为模板用于生成入口文件，不是作为静态文件进行访问之类。
        ]
      }
    }
  ]
})
```

##### mode

在webpack.config.js 中配置不同的mode，会有不同效果。比如：指定mode为"development"，将不会进行代码压缩；而指定为"production"，将会进行代码压缩。

Mode 配置选项，可以告知webpack使用响应模式的内置优化：

- 默认值是production（什么都不设置的情况下）；

- 可选值有：'none' | 'development' | 'production'

| 选项        | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| development | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 development. 为模块和 chunk 启用有效的名。 |
| production  | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 production。为模块和 chunk 启用确定性的混淆名称，FlagDependencyUsagePlugin，FlagIncludedChunksPlugin，ModuleConcatenationPlugin，NoEmitOnErrorsPlugin 和 TerserPlugin 。 |
| none        | 不使用任何默认优化选项                                       |

mode配置包含着更多透明的配置，如果加上配置，则会暴露出来。如下：

![image-20211113112923700](https://i.loli.net/2021/11/13/akU7jLHszw6YgdD.png)



#### babel

babel是将一种语言，转换成另一种语言，所以就是一个编译器。

**Babel也拥有编译器的工作流程：**

- 解析阶段（Parsing）

- 转换阶段（Transformation）

- 生成阶段（Code Generation）

![image-20211113121325223](https://i.loli.net/2021/11/13/8ptuXKoJlxqNWHk.png)

词法分析是将语句进行拆分，分析提取 关键字、变量、运算符、变量值，生成token数组。所以，token不仅仅是鉴权的概念，还有编译原理中的概念。

babel本身可以作为一个独立的工具（和postcss一样），不和webpack等构建工具配置来单独使用（类似于postCss）。

如果希望在命令行尝试使用babel，需要安装如下库：

- **@babel/core：**babel的核心代码，必须安装；

- **@babel/cli：**可以让我们在命令行使用babel；

**使用babel来处理我们的源代码：**

- src：是源文件的目录

- --out-dir：指定要输出的文件夹dist

```sh
npx babel src --out-dir dist # 这里是指输出到文件夹中
```

也可以输出到文件中：将 --out-dir 参数替换为 --out-file 即可。

不过，仅仅这样，输出的 js文件格式还是 ES6，这就需要使用插件。比如，需要转换箭头函数，就可以使用箭头函数转换相关的插件：@babel/plugin-transform-arrow-functions。并且在写命令时，要加上--plugins；命令行代码如下：

```sh
npx babel src --out-dir dist --plugins=@babel/plugin-transform-arrow-functions
```

另外，如果有多个插件，插件名称用逗号连接。

因为每个插件都要自己下载，实在过于麻烦，所以可以使用 预设( preset ) ：@bable/preset-env；要使用预设，需要使用--preset参数，命令行代码如下：

```sh
npx babel src --out-dir dist --presets=@babel/preset-env
```

当然，还有其他预设，比如React 的预设 ( @babel/preset-react )，TS的预设( @babel/preset-ts )。如果使用 多个预设，和--plugins参数一样，通过逗号连接。



如果要在webpack 中使用 babel解析ES6，需要使用 babel-loader

```js
{
  test: /\.js$/,
  use: {
    loader: "babel-loader",
    optinos: {
      // 配置使用的插件，和上面的命令行使用babel一样，直接使用插件太过于麻烦，所以使用预设
      /* plugins: [
        "@babel/plugin-transform-arrow-functions",
        "@babel/plugin-transform-block-scoping"
      ] */
      preset: [
        "@babel/preset-env"
      ]
    }
  }
}
```

和postcss-loader一样，babel可以把配置文件抽离出去；将配置放到一个独立的文件中，babel给我们提供了两种配置文件的编写：

- **babel.config.json（或者.js，.cjs，.mjs）文件**

- babelrc.json（或者.babelrc，.js，.cjs，.mjs）文件。其中rc的意思，似乎是 运行命令 ( run command ) 的意思。

**这两个配置文件名的区别：**

- **.babelrc.json：**早期使用较多的配置方式，但是对于配置Monorepos项目是比较麻烦的。目前很多的项目都采用了多包管理的方式（babel本身、element-plus、umi等）

- <font color=FF0000>**babel.config.json：**</font><font color=FF0000>babel7使用的配置方式，可以直接作用于Monorepos项目的子包，更加推荐</font>

这时候，webpack.config.js 可以变成：

```js
{
  test: /\/.js$/,
  use: "babel-loader"
}
```

##### jsx 的 babel 配置

**如果希望在项目中使用jsx，那么需要添加对jsx的支持：**jsx通常会通过Babel来进行转换（React编写的jsx就是通过babel转换的）；对于Vue来说，我们只需要在Babel中配置对应的插件即可。

- **安装babel支持的Vue的jsx插件**

  ```sh
  npm install @vue/babel-plugin-jsx -D
  ```

- 在 babel.config.js 配置文件中配置插件：

  ```js
  module.exports = {
    presets: [
      '@vue/cli-plugin-babel/preset'
    ],
    plugins: [
      '@vue/babel-plugin-jsx'
    ]
  }
  ```

- 使用 jsx（这部分可以结合下面的h()函数，参考阅读）

  ```html
  <script>
  	export default {
      render() {
        return (
          <div>Hello jsx</div>
        )
      }
    }
  </script>
  ```


##### Vue打包后不同版本解析，其中包含

- **runtime + compiler：**如果在js文件中的 Vue 实例有 template 选项（ vue createApp 实例中有 template ），则必须用runtime + compiler
- **runtime only：**默认使用

**更详细的版本：**

- **vue(.runtime).global(.prod).js：**通过浏览器中的 \<script src="..."> 直接使用。通过CDN引入和下载的Vue就是这个版本，它会暴露一个全局的Vue来使用。
  - **runtime版本：**不包含compiler，如果不需要对template 进行编译，则使用 **vue.runtime.global(.prod).js**，包会更小一点
  - **prod版本：**进行代码压缩的版本

- **vue(.runtime).esm-browser(.prod).js：**用于通过原生ES 模块导入使用(在浏览器中通过 \<script type="module"> 来使用)。

- **vue(.runtime).esm-bundler.js：**用于webpack，rollup 和parcel 等构建工具；构建工具中默认是vue.runtime.esm-bundler.js。如果我们需要解析模板template，那么需要手动指定vue.esm-bundler.js；

- **vue.cjs(.prod).js：**服务器端渲染使用，通过require()在Node.js中使用。

**在Vue的开发过程中我们有三种方式来编写DOM元素：**

- 方式一：template模板的方式（之前经常使用的方式）

- 方式二：render函数的方式，使用h函数来编写渲染的内容

- 方式三：通过.vue文件中的template来编写模板

##### 它们的模板分别是如何处理的

- 方式二中的h函数可以直接返回一个虚拟节点，也就是Vnode节点；

- 方式一和方式三的template都需要有特定的代码来对其进行解析：

  - 方式三 .vue文件中的template可以通过在vue-loader对其进行编译和处理；

  - 方式一中的template我们必须要通过源码中一部分代码来进行编译；

**所以，Vue在让我们选择版本的时候分为运行时+编译器vs 仅运行时**

- 运行时+编译器包含了对template模板的编译代码，更加完整，但是也更大一些；

- 仅运行时没有包含对template版本的编译代码，相对更小一些；

##### SFC 相关的 loader

要解析 sfc文件，需要使用 vue-loader，`npm install vue-loader@next`

```js
{
  test: /\.vue$/,
  loader: "vue-loader"
}
```

但是，仅仅安装 vue-loader，还是会报错，因为vue-loader 还依赖 @vue/compiler-sfc，这是专门用于解析 sfc 的工具（在Vue2 中是 vue-template-compiler）

再安装 @vue/compiler-sfc 依然是不够的，还是会报错，还需要配置 VueLoaderPlugin

```js
const { VueLoaderPlugin } = require('vue-loader/dist/index')

plugins: [
  // ...
  new VueLoadPlugin()
]
```

这时SFC在npm run build 时不会报错，但是会报关于 \_\_VUE_OPTIONS_API__ 和 \_\_VUE_PROD_DEVTOOLS__ 的警告，可以通过在DefinePlugin中配置，以消除经过

```js
plugins: [
  new DefinePlugin({
    __VUE_OPTIONS_API__: true,
    __VUE_PROD_DEVTOOLS__: false
  })
]
```

##### 搭建本地服务器

在开发中，当文件发生变化时，希望可以自动的完成编译和展示。

**为了完成自动编译，webpack提供了几种可选的方式：**

- webpack watch mode：webpack的watch模式

- webpack-dev-server（常用）

- webpack-dev-middleware

##### webpack watch

**webpack给我们提供了watch模式：**在该模式下，webpack依赖图中的所有文件，只要有一个发生了更新，那么代码将被重新编译；我们不需要手动去运行npm run build指令了；

**开启watch有两种方式：**

- 方式一：在导出的配置中，添加 watch: true

  ```js
  // webpack.config.js
  module.exports = {
    watch: true,
  }
  ```

- 方式二：在启动webpack的npm scripts 中，添加 --watch的标识

  ```json
  "scripts": {
    "watch": "webpack --watch"
  }
  ```

  添加 --watch 后，会被 webpack-cli 处理，变成一个配置，即 watch: true

##### webpack-dev-server

<font color=FF0000>使用 webpack watch 是没有自动刷新浏览器的功能的</font>。<mark>虽然可以在VSCode中使用live-server来完成这样的功能</mark>。但是，我们希望在不适用live-server的情况下，可以具备live reloading（实时重新加载）的功能。可以使用 webpack-dev-server。

```js
scripts: {
  "serve": "webpack serve"
}
```

webpack-dev-server 自动在本地搭建了一个 express 服务器。webpack-dev-server 也会对项目进行打包，但是没有进行输出（文件），而是放在了内存中。webpack-dev-server 使用了一个库叫memfs，是第三方开发的，webpack-dev-server打包后的内容由它托管（类似的项目，memory-fs webpack自己写的，但是好久不维护了）

**对webpack-dev-server进行配置，进行自定义配置**

```js
devServer: {
  // 如果有些资源无法从webpack打包后的内容中获得，可以从contentBase指定的路径获取。比如CopyWebpackPlugin 插件中 pattern.globOptions.ignore参数，设置不复制的文件；甚至根本就不用copyWebpackPlugin，这些文件 webpack会找不到，可以通过contentBase 进行设置；且一般设置的路径为 './public'
  contentBase: './foo',
}
```

由于上面，所说的 copyWebpackPlugin在开发阶段是比较费事的，因为静态文件中，可能包含大的文件，复制会导致webpack-dev-server 性能变差，所以，可以在开发阶段使用 不使用 CopyWebpackPlugin，而将webpack-dev-server的静态文件读取指向contentBase；而在生产环境中，使用 CopyWebpackPlugin

##### webpack-dev-server 设置 HMR（热模块更新）

模块热替换是指：在应用程序运行过程中，替换、添加、删除模块，而无需重新刷新整个页面。

**自己的理解：**无论是硬件（比如服务器）还是软件，仅仅对自己修改的模块进行替换，其他模块状态保持不变。而如果webpack-dev-server 如果不设置 HMR，会做的事情是：所有模块都重置（页面完全刷新，状态完全丢失；而不是局部刷新）

**HMR通过如下几种方式，来提高开发的速度：**

- 不重新加载整个页面，这样可以保留某些应用程序的状态不丢失

- 只更新需要变化的内容，节省开发的时间

- 修改了css、js源代码，会立即在浏览器更新，相当于直接在浏览器的devtools中直接修改样式

##### 如何使用 HMR？

- 默认情况下，webpack-dev-server已经支持HMR，我们只需要开启即可

  ```js
  // 另外，HMR有时候只加上hot配置会有问题，还要加上 target配置
  moodule.exports = {
  	target: 'web', // 如果是web环境设置，则为web；如果是Node，则为node
    //...
    devServer: {
    	hot: true
  	}
  }
  ```

  另外，仅仅配置 `devServer.hot: true` 还是不足以使模块局部刷新（就是HMR），还需要在 main.js 中添加如下代码。

  ```js
  if(module.hot) {
    // 使得 ./js/foo.js 支持 HMR
    module.hot.accept('./js/foo.js', () => {console.log('foo.js 已更新')})
    // 使得 其他 文件 支持 HMR
    // ...
  }
  ```

  而上面的方法是在过于麻烦，每添加一个模块，就要添加对应配置过于麻烦。而实际上 Vue-loader支持Vue 组件的 HMR，在每一个sfc中会自动添加上面的配置代码。另外，在React中有对应的方案：React-refresh。

- 在不开启HMR的情况下，当我们修改了源代码之后，整个页面会自动刷新，使用的是live reloading

另外，在使用HMR时，浏览器控制台会出现如下打印（见下），其中  WDS 就是 webpack-dev-server。可以用其来知道 webpack-dev-server开启了HMR

```markdown
[HMR] Waiting for update signal from WDS...
```



##### HMR的原理，如何可以做到只更新一个模块中的内容呢？

<font color=FF0000>webpack-dev-server会创建两个服务：提供静态资源的服务 ( express ) 和 Socket服务( net.Socket )</font>

- express server负责直接提供静态资源的服务（打包后的资源直接被浏览器请求和解析）；

- <font color=FF0000>**HMR Socket Server，是一个socket的长连接：**</font>

  - 长连接有一个最好的好处是建立连接后双方可以通信（服务器可以直接发送文件到客户端）

  - <font color=FF0000>当服务器监听到对应的模块发生变化时，会生成两个文件.json（manifest文件）和 .js文件（update chunk）</font>

  - <font color=FF0000>通过长连接，可以直接将这两个文件主动发送给客户端（浏览器）</font>

  - <font color=FF0000>浏览器拿到两个新的文件后，通过HMR runtime机制，加载这两个文件，并且针对修改的模块进行更新</font>

![image-20211113172716575](https://i.loli.net/2021/11/13/BriX5eIUdZy1PWL.png)



**devServer 配置：**

- **host：**

  - **默认是localhost，即 127.0.0.1 回环地址：**我们主机自己发出去的包，直接被自己接收（在网络层直接就被获取到，不经过 数据链路层和物理层。所以，监听127.0.0.1时，在同一个网段下的主机中，通过ip地址是不能访问的）
  - **0.0.0.0：**监听IPV4上所有的地址，再根据端口找到不同的应用程序。监听0.0.0.0时，在同一个网段下的主机中，通过ip地址是可以访问的

- **port：**设置监听的端口，默认情况下是8080

- **open：**否打开浏览器。默认值是false，设置为true会打开浏览器；也可以设置为类似于Google Chrome等值

- **compress：**是否为静态文件开启gzip compression。默认值是false，可以设置为true。另外，作为开发环境没有特别大的必要使用，因为是在本地开发，传输速度很快，这样压缩与解压反而会带来性能的影响。

- **proxy：**

  ```js
  proxy: {
    // 做映射，代理
    "/api": 'http://localhost:8888'
  }
  ```

  但是，只有这个配置项还不够。因为当访问 /foo 时，会访问本地devServer的http://localshot:8888/api/foo，而不是想要的http://localhost:8888/foo，所以需要pathRewrite。

  ```js
  proxy: {
    '/api': {
      target: 'http://localhost:8888',
      pathRewrite: {
        "^/api": ""
      }
    }
  }
  ```

  - **secure：**默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false
  - **changeOrigin：**它表示是否更新代理后请求的headers中host地址，一般为true；将host的地址，改为 代理服务器的 地址

- **historyApiFallback：**historyApiFallback是开发中一个非常常见的属性，它主要的作用是解决SPA页面在路由跳转之后，进行页面刷新时，返回404的错误。

  **值的类型：**

  - **boolean值：**默认是false。如果设置为true，那么在刷新时，返回404错误时，会自动返回index.html 的内容

  - **object类型的值：**可以配置rewrites属性；可以配置from来匹配路径，决定要跳转到哪一个页面；

  devServer中实现historyApiFallback功能是通过 [connect-history-api-fallback](https://github.com/bripkens/connect-history-api-fallback) 库的



**resolve**

**resolve用于设置模块如何被解析：**

- 在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库

- resolve可以帮助webpack从每个require/import 语句中，找到需要引入到合适的模块代码

- webpack 使用 [enhanced-resolve](https://github.com/webpack/enhanced-resolve) 来解析文件路径

**webpack能解析三种文件路径：**

- **绝对路径：**由于已经获得文件的绝对路径，因此不需要再做进一步解析。

- **相对路径：**

  - 在这种情况下，使用import 或require 的资源文件所处的目录，被认为是上下文目录；

  - 在import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径；

- **模块路径**

  - 在resolve.modules中指定的所有目录检索模块

  - 默认值是['node_modules']，所以默认会从node_modules中查找文件

  - 我们可以通过设置别名的方式来替换初识模块路径，具体后面讲解alias的配置

**确实引入的路径对应的是 文件还是文件夹**

- **如果是一个文件：**如果文件具有扩展名，则直接打包文件；否则，将使用resolve.extensions选项作为文件扩展名解析

- **如果是一个文件夹：**会在文件夹中根据resolve.mainFiles配置选项中指定的文件顺序查找

  - resolve.mainFiles的默认值是['index']

  - 再根据resolve.extensions来解析扩展名

    extensions是解析到文件时自动添加扩展名，默认值是 ['.wasm', '.mjs', '.js', '.json']；所以如果我们代码中想要添加加载.vue 或者jsx 或者ts 等文件时，我们必须自己写上扩展名

**resolve中的alias配置**

当我们项目的目录结构比较深的时候，或者一个文件的路径可能需要 ../../../这种路径片段；我们可以给某些常见的路径起一个别名。

```js
resolve: {
  extension: [".wasm", ".mjs", ".js", ".json", ".jsx", ".ts", ".vue"],
  alias: {
    "@": path.resolve(__dirname, './src')
     page: path.resolve(_dirname, './src/page')
  }
}
```

##### webpack代码分包

**默认的打包过程：**默认情况下，在构建整个组件树的过程中，因为组件和组件之间是通过模块化直接依赖的，那么webpack在打包时就会将组 件模块打包到一起（比如一个app.js文件中）；这个时候随着项目的不断庞大，app.js文件的内容过大，会造成首屏的渲染速度变慢

**代码的分包：**所以，对于一些不需要立即使用的组件（非首屏使用的），我们可以单独对它们进行拆分，拆分成一些小的代码块chunk.js；这些chunk.js会在需要时从服务器加载下来，并且运行代码，显示对应的内容。<font color=FF0000 size=4>可以使用 **import()**，异步引入模块，在webpack打包时，就会进行分包操作</font>。

另外，代码分包，可以用在异步组件上

##### Vue CLI

**Vue CLI 步骤原理：**

![image-20211113201445367](https://i.loli.net/2021/11/13/7lXCuLq59rcDNTx.png)

建议配合视频观看L10 30min之后？

#### Vite2

vite的官方定位：下一代前端开发与构建工具。

**当前开发的痛点：**

我们编写代码往往是不能被浏览器直接识别的，比如ES6、TypeScript、Vue文件等；所以我们必须通过构建工具来对代码进行转换、编译，类似的工具有webpack、rollup、parcel；但是随着项目越来越大，需要处理的JavaScript呈指数级增长，模块越来越多。构建工具需要很长的时间才能开启服务器，HMR也需要几秒钟才能在浏览器反应出来。所以也有这样的说法：天下苦webpack久矣。

##### Vite的构造

Vite主要由两部分组成：

- 一个开发服务器，它基于原生ES模块提供了丰富的内建功能，HMR的速度非常快速
- 一套构建指令，它使用rollup打开我们的代码，并且它是预配置的，可以输出生成环境的优化过的静态资源

Vite2 使用的服务器框架是 Connect，替换了Vite1中用的 koa。之所以使用 connect，因为connect非常容易做请求转发。

##### Vite的技术思路

浏览器原生支持模块化的，我们可以通过浏览器做一些简单的js项目编写。但是如果我们不借助于其他工具，直接使用ES Module来开发有什么问题呢？

- 首先，我们会发现在使用 lodash 时，加载了上百个模块的js代码，对于浏览器发送请求是巨大的消耗
- 其次，我们的代码中如果有 TypeScript、less、vue 等代码时，浏览器并不能直接识别

vite就帮助我们解决了上面的所有问题。

##### Vite的安装和使用

Vite 依赖Node，要求Node版本大于12。

用 vite 启动项目：

```sh
npx vite
```

Vite对于样式文件（包含css，以及预处理器） 的支持

- vite可以直接支持css的处理，直接导入css即可

- vite可以直接支持css预处理器，比如less；但是需要在项目中安装 less等预处理器

  ```sh
  npm install less -D
  ```

- vite直接支持postcss的转换，只需要安装postcss，并且配置 postcss.config.js 的配置文件即可

  ```sh
  npm install postcss postcss-preset-env -D
  ```

##### Vite对TypeScript的支持

vite对TypeScript是原生支持的，它会直接使用ESBuild来完成编译；只需要直接导入即可。

**如果我们查看浏览器中的请求，会发现请求的依然是ts的代码：**

- 这是因为vite中的服务器Connect会对我们的请求进行转发
- 获取ts编译后的代码，给浏览器返回（虽然文件名和扩展名和之前一样都是 xxx.ts，但是里面的内容已经被替换成编译后的js 代码了；另外，预处理的css文件也一样，虽然文件名和扩展名和之前一样，但是内容已经被转译了），浏览器可以直接进行解析

Vite2 使用的服务器框架是 Connect，替换了Vite1中用的 koa。之所以使用 connect，因为connect非常容易做请求转发。

##### Vite对vue的支持

- **vite对vue提供第一优先级支持：**

  - Vue 3 单文件组件支持：@vitejs/plugin-vue

  - Vue 3 JSX 支持：@vitejs/plugin-vue-jsx

  - Vue 2 支持：underfin/vite-plugin-vue2

- 安装支持vue的插件：

  ```sh
  npm install @vitejs/plugin-vue -D
  ```

- 在vite.config.js中配置插件：

  ```js
  import vue from '@vitejs/plugin-vue'
  
  module.exports = {
    plugins: [
      vue()
    ]
  }
  ```


##### 使用 vite打包项目

- 可以直接通过 `vite build` 来完成对当前项目的打包工具：

  ```sh
  npx vite build
  ```

- 也可以通过 `preview` 的方式，开启一个本地服务来 预览**打包后** 的效果：

  ```sh
  npx vite preview
  ```
> 💡 有点类似于 [http-server](https://github.com/http-party/http-server) 或 [serve](https://github.com/vercel/serve) 的功能，不过是内置的功能

##### Vite的脚手架工具

<mark>在开发中，我们不可能所有的项目都使用vite从零去搭建，比如一个react项目、Vue项目； 这个时候vite还给我们提供了对应的脚手架工具</mark>

**所以Vite实际上是有两个工具的：**

- **vite：**相当于是一个构件工具，类似于webpack、rollup

- **@vitejs/create-app：**类似vue-cli、create-react-app

**使用脚手架工具：**

```SH
npm init @vitejs/app
```

**上面的做法相当于省略了安装脚手架的过程：**

```sh
npm install @vitejs/create-app -g
create-app
```

##### Vite 快速的原因

**Vite之所以快，是因为使用了ESBuild**。ESBuild的特点：

- 超快的构建速度，并且不需要缓存
- 支持ES6和CommonJS的模块化
- 支持ES6的Tree Shaking
- 支持Go、JavaScript的API
- 支持TypeScript、JSX等语法编译
- 支持SourceMap
- 支持代码压缩
- 支持扩展其他插件

**ESBuild为什么这么快？**

- 使用Go语言编写的，可以直接转换成机器代码，而无需经过字节码

- ESBuild可以充分利用CPU的多内核，尽可能让它们饱和运行
- ESBuild的所有内容都是从零开始编写的，而不是使用第三方，所以从一开始就可以考虑各种性能问题



#### 父子组件通信方式

![图片来自 11_Vue3组件化开发（一），第 5 页](https://i.loli.net/2021/11/14/Wvn4zDPp36wBmVH.png)

- <font color=FF0000 size=4>**父组件传递给子组件：通过props属性**</font>

  父组件有一些数据，需要子组件来进行展示；这个时候我们可以通过props来完成组件之间的通信。

  **什么是Props？** 

  Props是你可以在组件上注册一些自定义的attribute，父组件给这些attribute赋值，子组件通过attribute的名称获取到对应的值

  **Props有两种常见的用法：**

  - **字符串数组：**数组中的字符串就是attribute的名称

    ```js
    props: ['foo', 'bar', 'baz']
    ```

  - **对象类型：**对象类型我们可以在指定attribute 名称的同时，指定它需要传递的类型、是否是必须的、 默认值等等。

    **type的类型都可以是**：String、 Number、 Boolean、 Array、 Object、 Date、 Function、 Symbol。

    另外，props中可以包含type、required、default、validator 等

    <font color=FF0000 size=4>**如果默认值传递的是 对象或数组，则必须从一个工厂函数获取**</font>。原因是：如果传递的是一个对象或数组（引用赋值），且组件是会被复用的；如果父组件修改了这个对象，其他子组件的实例因为引用赋值，也会收到影响。
    
    ```js
    propFoo: {
      type: Object,
      default() {
        return { message: 'foo'}
      }
    }
    ```

    <font color=FF0000 size=4>**类似的：sfc (vue) 文件中的data属性，必须要使用return 返回一个对象，也是这个道理**。</font>

    

    #### Prop 的大小写命名(camelCase vs kebab-case)

    <font color=FF0000>HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符</font>。这意味着当你使用 DOM 中的模板时，<font color=FF0000>**camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名**</font>

    **非Prop的Attribute：**

    - 当我们传递给一个组件某个属性，但是该属性并没有定义对应的 props 或者 emits 时，就称之为 非Prop的Attribute。常见的包括class、style、id属性等

    - <font color=FF0000 size=4>**当组件有单个根节点时，非Prop的Attribute将自动添加到（子组件）根节点的Attribute中**</font>

      <font color=FF0000>如果不希望组件的根元素继承attribute，可以在组件中设置 **inheritAttrs: false**</font>。禁用attribute继承的常见情况是需要将attribute 应用于根元素之外的其他元素；可以通过 \$attrs 来访问所有的 非props的attribute。 另外，如果 nonProp 有很多的话，可以使用 **v-bind="$attrs"** 以获取所有

      <font color=FF0000 size=4>**多个根节点的attribute：**</font>多个根节点的attribute如果没有显示的绑定，那么会报警告，我们必须手动的指定要绑定到哪一个属性上。
    
      ```html
      <tempalte>
      	<div :class="$attrs.class">nonProp attribute component 1</div>
        <div>nonProp attribute component 2</div>
        <div>nonProp attribute component 3</div>
      </tempalte>
      ```

  **父组件传值示例：**

  ```html
  <!-- 父组件传值 -->
  <child-component foo="foo" bar="bar" baz="baz" />
  
  <!-- 对象传值 -->
  <child-component :foo="obj.foo" :bar="obj.bar" :baz="obj.baz" />
  <!-- 对象传值 更简洁的方法 -->
  <child-component v-bind="obj" />
  ```

- <font color=FF0000 size=4>**子组件传递给父组件：通过$emit触发事件**</font>

  **什么情况下子组件需要传递内容到父组件？**

  - 当子组件有一些事件发生时，比如在组件中发生了点击，引发父组件内容改变
  - 子组件<font color=FF0000>有一些内容（事件相关的参数？）想要传递给父组件</font>
  
  **如何完成上面的操作？**
  
  - 首先，我们需要<font color=FF0000>在子组件中定义好在某些情况下触发的事件名称（自定义事件名称）</font>
  - 其次，在父组件中以 v-on 的方式传入要监听的事件名称，并且绑定到对应的方法中
  - 最后，在子组件中发生某个事件的时候，根据事件名称触发对应的事件
  
  
  
  emits参数 可以用数组定义（更常用），也可以用对象（用于做参数的验证），示例如下：
  
  ```js
  emits: {
    noneParamFn: null, // add函数没有参数
    // 具有验证函数
    multiParamsFn: (param1, param2, ..., paramN) => {
      // ... 验证内容，返回true / false，表明是否通过验证，如果没有通过，则会在控制台报警告
    }
  }
  ```
  
  

#### 非父子组件通信

**非父子组件通信<font color=FF0000>主要</font>有两种方式：**

- **Provide / Inject**

  父组件有一个 provide 选项来提供数据，子组件有一个 inject 选项来开始使用这些数据。

  **可以将provide / inject 的依赖注入看作是 “长距离的prop” ( long range props )，除了：** 

  - 父组件不需要知道哪些子组件使用它 provide 的数据
  - 子组件不需要知道 inject 的数据来自哪里

  <font size=4>**provide 提供的数据需要使用 return {...} ，即：返回对象的函数**</font>。有两个原因：1. 共用数据会被修改 2. 如果provide 中使用 this，不使用 返回对象函数 这里的this指向 将不是vue示例。参考：[vue3 官方文档 - Provide / Inject](https://v3.cn.vuejs.org/guide/component-provide-inject.html)

  如果在父组件中修改了 provide 中提供的值，比如 names 数组的长度 lengthValue；子组件中的 lengthValue 将不会发生改变，因为它不是响应式的。如何使其变成响应式的？<font size=4>**在provide 中使用 composition api中的 计算方法（ computed函数 ）以传递**</font>（注意：不是在 inject 中）；而在子组件中就要使用 lengthValue.value 了

  ```js
  provide() {
    return {
      length: computed(() => this.names.length)
    }
  }
  ```

- **Mitt 全局事件总线**

  Vue3从实例中移除了 \$on、\$off 和 \$once 方法，所以如果希望继续使用全局事件总线，要通过第三方的库Vue3；官方有推荐一些库，例如 mitt 或 tiny-emitter。

  **mitt使用示例：**

  - 封装一个工具文件 eventBus.js

    ```js
    import mitt from 'mitt'
    
    const emitter = mitt()
    
    export default emitter;
    ```

    另外，这里可以创建多个emitter 对象

  - 监听事件

    ```js
    import emitter from './eventBus'
    
    export default {
      created() {
        // 注册监听
        emitter.on('foo', onFoo)
      },
      // 取消监听
      beforeUnmount() {
        emitter.off('foo', onFoo)
      },
      methods: {
        // foo事件处理函数
        onFoo(params) {
          console.log('foo event: ', params)
        }
        
        // 取消当前emitter中所有的监听
        clearAll() {
    	    emitter.all.clear()
        }
      }
    }
    ```

  - 触发事件

    ```js
    import emitter from './eventBus'
    
    export default {
      methods: {
        triggerEvent() {
          emitter.emit('foo', {bar: 'baz', quz: 'foo'})
        }
      }
    }

​	另外，更多参考官方文档：https://github.com/developit/mitt



#### 插槽

**插槽的默认内容：**如果希望在使用插槽时，如果没有插入对应的内容，需要显示一个默认的内容。这个默认的内容只会在没有提供插入的内容时，才会显示。

**在不使用具名插槽的情况下：**如果一个组件中含有多个插槽，在使用组件时插入多个内容时是什么效果？会发现默认情况下每个插槽都会获取到我们插入的内容来显示（即：每个插槽中都会放入插入的多个内容）。所以才需要具名插槽。

具名插槽顾名思义就是给插槽起一个名字，在使用时可以指定内容插入到指定的插槽中。\<slot> 元素有一个特殊的 attribute：**name**。<font color=FF0000>一个不带 name 的 slot，会带有隐含的名字 default</font>。另外，具名插槽v-slot 有缩写 #

**动态插槽名**：目前我们使用的插槽名称都是固定的，比如 v-slot:left、v-slot:center等。可以通过 **v-slot:[dynamicSlotName]** 方式动态绑定一个名称（<font size=4>**这里类似于JS中的计算属性**</font>）。**使用场景：**一般用于高级组件，<font color=FF0000>通过配置绑定插槽名</font>，比如父组件使用 prop 将插槽名称传递给子组件；子组件在写的时候自动绑定。

**渲染作用域：**在Vue中有渲染作用域的概念，<font color=FF0000>父级模板里的所有内容都是在父级作用域中编译的，子模板里的所有内容都是在子作用域中编译的</font>。<font size=4>**有时也会希望插槽可以访问到子组件中的内容：** </font>当一个组件被用来渲染一个数组元素时，我们使用插槽，并且希望插槽中没有显示每项的内容；<font color=FF0000 size=4>**Vue给我们提供了作用域插槽（插槽prop）**</font>

代码示例如下：

```html
<!-- showNames.vue -->

<!-- 这里的name是app.vue传来的，还要通过下面的slotProp传回去；当然，slotProps的名字是可以改的 -->
<template v-for="(item, index) in names" :key="item">
	<slot :item="item" :index="index"> </slot>
</template>

<script>
	export default {
    props: {
      data: {
        type: Array,
        default: () => []
      }
    }
  }
</script>
```

```html
<!-- app.vue -->
<show-name :names="names">
  <!-- 接收showName.vue传来的 slotProps，并使用 -->
  <!-- 如果插槽有名字，比如<slot name="foo" ...></slot>，这时候，下面的v-slot需要加上名字；即：v-slot:foo="slotProps"。另外，由于插槽没有名字，也叫default、并被省略；即也可以被写作：v-slot:default="slotProps" --> 
	<template v-slot="slotProps">
  	<button>{{slotProps.item}}-{{slotProps.index}}</button>
  </template>
</show-name>

<script>
	export default {
    data() {
      return {
        names: ['foo', 'bar', 'baz', 'quz']
      }
    }
  }
</script>
```

**独占默认插槽的缩写**

由于上面需要加上 \<template v-slot="slotProps">比较麻烦，所以可以去掉template；不过这是有条件的

<font color=FF0000 size=4>**当且仅当，这个插槽是一个独占默认插槽时才可使用，即这个插槽是组件中唯一的插槽。如果还有其他具名插槽，并且使用了这个插槽，则不可以这样写。还是要分别用\<tempalte>封装，并使用插槽**</font>

```html
<!-- app.vue -->
<show-name :names="names" v-slot="slotProps">
	<button>{{slotProps.item}}-{{slotProps.index}}</button>
</show-name>
```



#### 动态组件

**动态组件：**动态组件是使用 component 组件（component是一个内置组件），通过一个特殊的attribute is 来实现。官方示例如下：

```html
<div id="dynamic-component-demo" class="demo">
  <button
     v-for="tab in tabs"
     :key="tab"
     :class="['tab-button', { active: currentTab === tab }]"
     @click="currentTab = tab"
   >
    {{ tab }}
  </button>

  <component :is="currentTabComponent" class="tab"></component>
</div>
```

**上面的currentTab的值需要是什么内容？**

- 可以是通过component函数（非SFC文件中使用component函数注册组件）注册的组件
- 也可以是 在一个组件对象的components对象中注册的组件

**动态组件可以给它们传值和监听事件么？**可以。只是需要将属性和监听事件放到component上来使用

```html
<component 
  name="why"
  :age="18"
  :is="currentTab"
  @pageClick="pageClick"
/>
```

#### keep-alive

使用之前的异步组件，切换组件再回来，组件中的状态会消失（重置），无法保持；因为在切换组件后，组件会被销毁，切换回来时，会重新创建组件。这时候可以使用vue内置组件keep-alive 以使切换组件时，组件不会销毁。

**keep-alive 包含的一些属性**

- **include：**string | RegExp | Array。<font color=FF0000>只有名称匹配的组件会被缓存</font>
  - **如果使用正则，需要在include前面加上冒号，否则会被认成字符串**。类似的：一个prop要传递一个数字，要propName前面加上冒号
- **exclude：**string | RegExp | Array。<font color=FF0000>任何名称匹配的组件都不会被缓存</font>
  - 同上
- **max：**number | string。<font color=FF0000>最多可以缓存多少组件实例，一旦达到这个数字，那么缓存组件中最近没有被访问的实例会被销毁</font>

示例如下：

```html
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
	<component :is="view"></component>
</keep-alive>

<!-- 正则，使用v-bind -->
<keep-alive :include="/a|b/">
	<component :is="view"></component>
</keep-alive>

<!-- Array，使用v-bind -->
<keep-alive include="">
	<component :is="view"></component>
</keep-alive>
```

**缓存组件（\<keep-alive>中的组件）的生命周期：**的对于缓存的组件来说，再次进入是不会执行created或者mounted等生命周期函数的。 但有时候我们确实希望监听到<font color=FF0000>何时重新进入到了组件、何时离开了组件</font>；这个时候我们<font color=FF0000>可以使用activated 和 deactivated 这两个生命周期钩子函数</font>来监听



#### Vue中实现异步组件

如果项目过大，对于某些组件我们希望通过异步的方式来进行加载（目的是可以对其进行分包处理），那么Vue中给我们提供了一个函数：defineAsyncComponent

**defineAsyncComponent接受两种类型的参数：**

- **工厂函数：**该工厂函数需要返回一个Promise对象

  ```js
  import { defineAsyncComponent } from 'vue'
  const AsyncHome = defineAsyncComponent(() => import('./AsyncHome.vue'))
  
  export default {
    components: {
      Async
    }
  }
  ```

- **对象类型：**接受一个对象类型，对异步函数进行配置

  ```js
  const AsyncHome = defineAsyncComponent({
  	// 工厂函数
    loader: () => import('./AsyncHome.vue')
    // 加载过程中显示的组件
    loadingComponent: Loading,
    // 加载失败时显示的组件
    errComponent: Error,
    // 在显示LoadingComponent-之前的延迟 ｜ 默认值: 200 (单位ms)
    delay: 200,
    // 如果提供了timeout，并且加载组件的时间超过了设定值，将显示错误组件
    // 默认值: Infinity(即永不超时,单位 ms)
   	// timeout: 0,
  	// 定义组件是否可挂起 ｜ 默认值: true
    suspensible: true
  })
  ```

**suspense（2021/11/22 suspense在官方文档中还是实验性质，api还有可能修改）**<font color=FF0000>是一个内置的全局组件，该组件有两个插槽</font>：

- **default：**<font color=FF0000>如果default可以显示，那么显示default的内容</font>
- **fallback：**<font color=FF0000>如果default无法显示，那么会显示fallback插槽的内容</font>

```html
<suspense>
	<template #default>
  	<async-home />
  </template>
  <template #fallback>
  	<loading />
  </template>
</suspense>
```



#### Vue 实例

##### $refs

**某些情况下，我们在组件中想要直接获取到元素对象或者子组件实例：**<font color=FF0000>在Vue开发中是不推荐进行DOM操作的，我们可以给元素或者组件绑定一个 ref 的 attribute 属性</font>

组件实例有一个$refs属性：它是一个对象，持有注册过 ref attribute 的所有 DOM 元素和组件实例

```js
visitElement() {
  // 访问元素
  console.log(this.$refs.title)
  // 访问组件实例，$el是组件实例正在使用的根 DOM 元素
  console.log(this.$refs.helloCpn.$el)
	// 通过ref使用组件方法
  console.log(this.$refs.helloCpn.showMsg())
}
```

##### \$parent 和 \$root

可以通过$parent来访问父元素，通过\$root访问根元素

通过 \$el：组件实例正在使用的根 DOM 元素

另外，<font color=FF0000 size=4>在Vue3中已经移除了 \$children的属性，所以不可以使用</font>



#### 生命周期函数

<font color=FF0000>每个组件都可能会经历从创建( create )、挂载 ( mount )、更新( update )、卸载 ( unmount )等一系列的过程</font>。生命周期函数是一些钩子函数，在某个时间会被Vue源码内部进行回调

<img src="https://cn.vuejs.org/assets/lifecycle.16e4c08e.png" alt="组件生命周期图示" style="zoom:55%;" />



#### 组件的v-model

在\<input> 中可以使用 v-model 来完成双向绑定，这往往非常方便，因为 <font color=FF0000>v-model 默认帮助我们完成了两件事</font>：v-bind:value 的数据绑定和 @input的事件监听。即：

```html
<input v-model="searchText" />
<!-- 等价于 -->
<input :value="searchText" @input="searchText = $event.target.value" />
```

**如果我们封装了一个组件，其他地方在使用这个组件时，是否也可以使用v-model来同时完成这两个功能呢**？ 可以。<font color=FF0000 size=4>**vue也支持在组件上使用v-model**</font>。

当我们在组件上使用的时候，等价于如下的操作：

```html
<my-input v-model="message" />
<!-- 等价于 -->
<my-input :modelValue="message" @update:model-value="message = $event" />
```

发现：my-input 和 input元素不同的只是属性的名称和事件触发的名称而已。

**为了我们的MyInput组件可以正常的工作，这个组件内的 \<input> 必须：** **将MyInput的 value attribute 绑定到一个名叫 modelValue 的 prop 上；在MyInput的 input 事件被触发时，将新的值通过自定义的 update:modelValue 事件抛出( emit )**。<font color=FF0000>即：modelValue要定义在props中，update:mode要定义在emits中</font>。

示例如下：

```html
<!-- MyInput.vue 定义 -->
<template>
	<input :value="modelValue" @input="inputChange" />
</template>

<script>
	export default {
    props: ['modelValue'],
    emits: ['update:modelValue'],
    methods: {
      inputChange(event) {
        this.$emit('update:modelValue', event.target.value)
      }
    }
  }
</script>
```

这时候就可以使用了：

```html
<my-input v-model="message" />
```

但是在组件的封装中还是希望使用v-model，而不是modelValue，可以使用计算属性实现：

```html
<!-- MyInput.vue -->
<template>
	<input v-model="value" />
</template>

<script>
	export default {
    props: ['modelValue'],
    emits: ['update:modelValue'],
    computed: {
      value: {
        get() { return this.modelValue },
        // 在修改value时，直接触发(emit) update:modelValue事件
        set(value) { this.$emit('update:modelValue', value) }
      }
    }
  }
</script>
```

另外，也可以不使用计算属性，使用methods即可，在methods中定义类似计算属性set的方法。

```js
methods: {
  inputUpdate(event) {
    this.$emit('update:modelValue', event.target.value)
  }
}
```

##### v-model绑定多个数据

v-model绑定多个数据，即在一个组件上使用多个v-model。

默认情况下，v-model其实是绑定了 modelValue 属性和 @update:modelValue的 事件；<font color=FF0000>如果我们希望绑定更多，可以给v-model传入一个参数，那么这个参数的名称就是我们绑定属性的名称</font>。

示例：

```html
<!-- 定义 -->
<template>
	<input v-model="value" />
  <input v-model="titleVal" />
</template>

<script>
	export default {
    props: ['modelValue', 'title'],
    emits: ['update:modelValue', 'update:title'],
    computed: {
      value: {
        get() { return this.modelValue}
        set(value) { this.$emit('update:modelValue', value) }
      },
      titleVal: {
        get() { return this.title }
        set(titleValue) { this.$emit('update:title', titleValue) }
      }
    }
  }
</script>
```

```html
<!-- 使用 -->
<!-- 这里v-model:title中的title就是传入的参数，是绑定属性的名称；上面实现props中的title也就是这个 -->
<my-input v-model="message" v-model:title="title" />
```



#### Vue中的动画

在开发中，给一个组件的显示和消失（比如v-if / v-show）添加某种过渡动画，可以很好的增加用户体验。

- React框架本身并没有提供任何动画相关的API，所以在React中使用过渡动画我们需要使用一个第三方库 react-transition-group

- Vue中为我们提供一些内置组件和对应的API来完成动画，利用它们我们可以方便的实现过渡动画效果

直接调用 v-if / v-show，而不使用动画，元素的消失和出现会变得非常生硬。如果希望给单元素或者组件实现过渡动画，可以使用 \<transition> 内置组件来完成动画

##### transition动画

Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡：

```html
<transition name="fade">
	<h2 v-if="show">Hello world</h2>
</transition>

<style scoped>
  // 含义见下面的 class
  .fade-enter-from, .fade-leave-to { opacity: 0 },
  .fade-enter-to, .fade-leave-from { opacity: 1 },
  .fade-enter-active, .fade-leave-active { transition: opacity 1s ease }
</style>
```

##### Transition组件的原理

当插入或删除 <font color=FF0000>包含在 transition 组件中的元素时</font>，Vue 将会做以下处理：

1. <font color=FF0000>自动嗅探目标元素是否应用了CSS过渡或者动画，如果有，那么在恰当的时机添加 / 删除 CSS类名</font>
2. <font color=FF0000>如果 transition 组件提供了 JavaScript钩子函数，这些钩子函数将在恰当的时机被调用</font>
3. 如果没有找到JavaScript钩子并且也没有检测到CSS过渡/动画，DOM插入、删除操作将会立即执行

**过渡动画class：**Vue通过在这些class之间来回切换完成的动画。

- **v-enter-from：**<font color=FF0000>定义进入过渡的开始状态</font>。在元素被插入之前生效，在元素被插入之后的下一帧移除。
- **v-enter-active：**<font color=FF0000>定义进入过渡生效时的状态</font>。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

- **v-enter-to：**<font color=FF0000>定义进入过渡的结束状态</font>。在元素被插入之后下一帧生效（与此同时 v-enter-from 被移除），在过渡 / 动画完成之后移除。
- **v-leave-from：**<font color=FF0000>定义离开过渡的开始状态</font>。在离开过渡被触发时立刻生效，下一帧被移除
- **v-leave-active：**<font color=FF0000>定义离开过渡生效时的状态</font>。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
- **v-leave-to：**<font color=FF0000>离开过渡的结束状态</font>。在离开过渡被触发之后下一帧生效（与此同时 v-leave-from 被删除），在过渡 / 动画完成之后移除。

##### class添加的时机和命名规则

![Transition Diagram](https://v3.cn.vuejs.org/images/transitions.svg)

**class的name命名规则如下：**如果我们使用的是一个没有 name的 transition，那么所有的class是以 v- 作为默认前缀；如果我们添加了一个name属性，比如 \<transtion name="foo">，那么所有的class会以 foo- 开头。

**过渡css动画：**也可以通过css的animation属性 来实现动画效果

```html
<button @click="show != show">Toggle</button>

<transitoin name="bounce">
	<h2 v-if="show">Lorem ipsum</h2>
</transitoin>

<style>
  .bounce-enter-active { animation: bounce-in 0.5s },
  .bounce-leave-active { animation: bounce-in 0.5s reverse},
  @keyframe bounce-in {
    0% { transform: scale(0) }
    50% { transform: scale(1.25) }
    100% { transform: scale(1) }
  }
</style>
```

**同时使用过渡( transition )和动画( animation )：**<font color=FF0000>Vue为了知道过渡的完成，内部是在监听 transitionend 或 animationend，到底使用哪一个取决于元素应用的 CSS规则</font>： <mark>如果只是使用了其中的一个，那么Vue能自动识别类型并设置监听</mark>；但是如果同时使用了过渡和动画，并且在这个情况下可能某一个动画执行结束时，另外一个动画还没有结束。这种情况下，我们可以设置 type 属性为 animation 或者 transition 来明确的告知Vue监听的类型（使用type 以指定）

```html
<transition name="foo"
            type="transition"
            @after-enter="afterEnter">
  <h2 v-if="show">Hello world</h2>
</transition>
```

**设置显示的指定动画时间：**可以显示的来指定过渡的时间，通过 duration 属性；单位为毫秒

**duration可以设置两种类型的值：**

- **number类型：**<font color=FF0000>同时设置进入和离开的过渡时间</font>

  ```html
  <transition :duration="1000">...</transition>
  ```

- **object类型：**<font color=FF0000>分别设置进入和离开的过渡时间</font>

  ```html
  <transition :duration="{enter: 800, leave: 100}">...</transition>
  ```



**过渡模式mode**

动画在两个元素之间切换（一个元素出现，另一个元素消失）的时候可能存在的问题：会发现两个元素会出现“同时存在”的情况。

这是因为<font color=FF0000>默认情况下进入和离开动画是同时发生的</font>； 如果确实我们希望达到这个的效果，那么是没有问题。但是<font color=FF0000>如果我们不希望同时执行进入和离开动画，那么我们需要设置transition的过渡模式</font>：

- **in-out：**新元素先进行过渡，完成之后当前元素过渡离开
- **out-in：**当前元素先进行过渡，完成之后新元素过渡进入；

另外，动态组件是过渡模式的一个使用场景



**初次渲染 appear**

<font color=FF0000>默认情况下，首次渲染时是没有动画的</font>，<mark>如果希望添加上去动画，那么就可以增加另外一个属性 appear</mark>。（这个和watch 的惰性类似）

```html
<button @click="show != show">Toggle</button>
<transition name="why" appear>
	<component :is=" show ? 'home' : 'about' " />
</transition>
```



**animate.css**

如果手动一个个来编写动画，那么效率是比较低的，所以在开发可以引用一些第三方库的动画库， 比如animate.css。[Animate.css](https://animate.style) 是一个已经准备好的、跨平台的动画库为我们的web项目，对于强调、主页、滑动、注意力引导 非常有用

使用Animate.css 步骤：

1. 安装Animate.css：

   ```sh
   npm install animate.css --save`
   ```

2. 导入样式：

   ```js
   import 'animate.css';
   ```

3. 使用animation动画或者animate提供的类，有两种用法：

   - 直接使用animate库中定义的 keyframes 动画

     ```css
     .foo-enter-active { animation: flip 1s; }
     .foo-leave-active { animation: flip 1s reverse; }
     ```

   - 直接使用animate库提供给我们的类

     ```html
     <!-- 这里类的使用见下面 -->
     <transition name="foo"
                 enter-acive-class="animation__animated animate__listSpeedInRight"
                 leave-acive-class="animation__animated animate__listSpeedOutRight"
     >
       <h2 v-if="show">Hello world</h2>
     </transition>
     ```


##### 可以通过以下 attribute 来自定义过渡类名

- enter-from-class 
- enter-active-class
- enter-to-class
- leave-from-class
- leave-active-class
- leave-to-class

<font color=FF0000>他们的优先级高于普通的类名</font>，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css. 结合使用十 分有用。

##### JavaScript钩子

transition组件给我们提供的JavaScript钩子，这些钩子可以帮助我们监听动画执行到 什么阶段

```html
<transition
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
  @enter-cancelled="enterCancelled"
  @before-leave="beforeLeave"
  @leave="leave"
  @after-leave="afterLeave"
  @leave-cancelled="leaveCancelled"
  :css="false"
>
  <!-- ... -->
</transition>

<script>
  default export {
  	methods: {
      beforeEnter(el, done) {
	    	// ...
    		done()
  		},
      // ...
    }
  }
</script>
```

<font color=FF0000>当使用JavaScript来执行过渡动画时，需要进行 done 回调，否则它们将会被同步调用，过渡会立即完成</font>。

添加 `:css="false"`，会让 Vue 会跳过 CSS 的检测，除了性能略高之外，这可以避免过渡过程中 CSS 规则的影响

##### GSAP

某些情况下我们希望通过 JavaScript（更加灵活，方便修改变量）来实现一些动画的效果，这个时候我们可以选择使用gsap库来完成

GSAP 可以通过JavaScript为CSS属性、SVG、Canvas等设置动画，并且是浏览器兼容的。

**使用示例：**

```html
<transition name="foo"
            @enter="enter"
            @leave="leave"
>
  <h2 v-if="show">Hello world</h2>
</transition>

<script>
  export default {
   	methods: {
      enter(el, done) {
      	gsap.from(el, {
      		scale: 0,
      		x: 200,
      		onComplete: done
    		})
      },
      leave(el, done) {
        gsap.to(el, {
          scale: 0,
          x: 200,
          onComplete: done
        })
      }
    }
  }
</script>
```

gsap实现数字变化示例：

```html
<div>
  <input type="number" v-model.number="counter" step="100" />
	<!-- 方法一 -->
  <h2>{{ animatedNumber }}</h2>
  <!-- 方法二 -->
  <h2>{{ showNumber.toFixed(0) }}</h2>
</div>
```

```js
data() {
  return { counter: 0, showNumber: 0 }
},
computed() {
  animatedNumber() { return this.showNumber.toFixed(0) }
},
watch: {
  conter(newValue) {
    gsap.to(this, { duration: 1, showNumber: newValue })
  }
}
```

##### 列表的过渡

目前，过渡动画我们只要是针对单个元素或者组件的： 要么是单个节点，要么是同一时间渲染多个节点中的一个。

<font color=FF0000>如果希望渲染的是一个列表，并且该列表中添加删除数据也希望有动画执行</font>；这时我们要<font color=FF0000>使用 \<transition-group> 内置组件来完成</font>。

**使用 \<transition-group> 有如下的特点：**

- 默认情况下，它不会渲染一个元素的包裹器，但是你可以指定一个元素并以 tag attribute 进行渲染

- 过渡模式不可用，因为我们不再相互切换特有的元素
- 内部元素总是需要提供唯一的 key attribute 值
- CSS 过渡的类将会应用在内部的元素中，而不是这个组 / 容器本身

关于\<transition-group> 的示例：略



#### Mixin

组件和组件之间有时候会存在相同的代码逻辑，可以对相同的代码逻辑进行抽取。

**在Vue2和Vue3中都支持的一种方式就是使用Mixin来完成：**

- Mixin提供了一种非常灵活的方式，来分发Vue组件中的可复用功能
- Mixin对象<font color=FF0000>可以包含任何组件选项</font>（data、methods、生命周期钩子函数、...）
- <font color=FF0000>当组件使用Mixin对象时，所有Mixin对象的选项将被 混合 进入该组件本身的选项中</font>

**Mixin的合并规则**

如果Mixin对象中的选项和组件对象中的选项发生了冲突，那么Vue会如何操作呢？分成不同的情况来进行处理：

- **data 函数返回值对象：**返回值对象默认情况下会进行合并；如果data返回值对象的属性发生了冲突，那么会保留组件自身的数据
- **生命周期钩子函数：**生命周期的钩子函数会被合并到数组中，都会被调用
- **值为对象的选项**（例如 methods、components 和 directives）**将被合并为同一个对象**：比如都有methods选项，并且都定义了方法（对象的key不同），那么它们都会生效；如果对象的key相同，那么会取组件对象的键值对。

**全局混入Mixin：**如果组件中的某些选项，是所有的组件都需要拥有的，那么这个时候我们可以使用全局的mixin。全局的Mixin可以使用 应用 app 的方法 mixin 来完成注册。 一旦注册，那么全局混入的选项将会影响每一个组件。



#### extends

extends属性类似于 mixin，允许声明扩展另外一个组件。

```html
<!-- BasePage.vue -->
<script>
	export default {
    data() {
      return { message: 'hello world' }
    }
  }
</script>
```

```js
import BasePage from './BasePage.vue'

export default {
  extends: BasePage
}
```

<font color=FF0000>**在开发中extends用的非常少，在Vue2中比较推荐使用Mixin，而在Vue3中推荐使用Composition API**</font>



#### Composition API

在Vue组件中，使用 Composition API 的地方就是setup函数。

**setup函数有两个参数：props 和 context**

- **props：**就是父组件传递过来的属性会被放到props对象中，在setup中如果需要使用，可以直接通过props参数获取。
  - <font color=FF0000>**对于定义props的类型，还是和之前一样，在props选项中定义**</font>。在template中依然是可以正常去使用props中的属性
  - 如果我们在setup函数中想要使用props，由于setup中无法使用this，所以无法通过this去获取。因为props有直接作为参数传递到setup函数中，所以我们可以直接通过参数来使用即可。
- **context：**也被称为SetupContext，包含三个属性
  - **attrs：**所有的非prop的attribute
  - **slots：**父组件传递过来的插槽，这个在以渲染函数返回时会有作用
  - **emit：**当我们组件内部需要发出事件时会用到emit，还是由于setup中无法使用this

**setup是一个函数，它可以有返回值：**

- setup的返回值可以在模板template中被使用，即：可以通过setup的返回值来替代data选项

- 可以返回一个执行函数来代替在methods中定义的方法

**setup中不可以使用this**

> 在 setup 中你应该避免使用 this，因为它不会找到组件实例。setup 的调用发生在 data property、computed property 或 methods 被解析之前，所以它们无法在 setup 中被获取。

另外，SFC中有一个使用组合式 API 的编译时语法糖 \<script setup>；相较一般的\<script> 有如下优势：

- 更少的样板内容，更简洁的代码。
- 能够使用纯 Typescript 声明 props 和抛出事件。
- 更好的运行时性能 (其模板会被编译成与其同一作用域的渲染函数，没有任何的中间代理)。
- 更好的 IDE 类型推断性能 (减少语言服务器从代码中抽离类型的工作)。

##### Reactive API

想为<font color=FF0000>在setup中定义的数据提供响应式的特性</font>，那么我们可以使用 reactive 的函数。

**原理：**<font color=FF0000>当使用reactive函数处理数据（被proxy进行劫持）后，数据再次被使用时就会进行依赖收集</font>；当数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面）；另外，写Options API的data选项，也是在内部交给了reactive函数将其编程响应式对象的

##### Ref API

Reactive API对传入的类型是有限制的，<font color=FF0000>要求必须传入的是一个对象或者数组类型</font>；如果传入一个基本数据类型( String、Number、Boolean )会报一个警告。这时可以使用 Ref API

ref 会返回一个可变的响应式对象，该对象作为一个“响应式的引用”维护着它内部的值，这就是ref名称的来源。<font color=FF0000>它内部的值是在ref的 value 属性中被维护的</font>

**注意：**

- <font color=FF0000 size=4>在模板中引入ref的值时，Vue会自动帮助我们进行 **解包操作**，所以不需要在模板中通过 ref.value 的方式来使用</font>

- 但在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，依然需要使用 ref.value的方式

**Ref自动解包**

- **模板中的Ref自动解包是浅层的解包：**在ref值外面用一个对象包装，不会自动解包
- **如果将 ref 放到一个reactive的属性当中**，那么在模板中使用时，它 <font size=4>**会自动解包**</font>

##### readonly

通过reactive或者 ref 可以获取到一个响应式的对象，<font color=FF0000>在某些情况下，我们传入给其他地方（组件）的这个 响应式对象希望在另外一个地方（组件）被使用，但是不能被修改</font>，如何实现？

Vue3 提供了readonly的方法，<font color=FF0000>readonly会返回原生对象的只读代理</font>（也就是它依然是一个Proxy，但是proxy的set方法被劫持，并且不 能对其进行修改）

**在开发中常见的readonly方法会传入三个类型的参数：** 

- 普通对象
- reactive返回的对象
- ref的对象

**readonly 使用规则**

- **readonly 返回的对象是不允许修改的**

- **经过readonly处理的原来的对象是允许被修改的**

  比如 const info = readonly(obj) ，info对象不允许被修改。当obj被修改时，readonly返回的info对象也会被修改；但是不能去修改readonly返回的对象info

- 本质上就是readonly返回的对象的setter方法被劫持了



**Reactive判断的API**

- **isProxy：**检查对象是否是由 reactive 或 readonly 创建的 proxy。

- **isReactive：**检查对象是否是由 reactive创建的响应式代理。如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true；

- **isReadonly：**检查对象是否是由 readonly 创建的只读代理。

- **toRaw：**<font color=FF0000>返回 reactive 或 readonly 代理的原始对象</font>（<mark>不建议保留对原始对象的持久引用。请谨慎使用</mark>）

- **shallowReactive：**创建一个响应式代理，跟踪其自身（第一层级）的 property 响应性，但不执行嵌套对象的深层（第二、第三等层级）响应式转换（深层中的数据 还是原生对象）

- **shallowReadonly：**创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的），和shallowReactive类似

##### toRefs

如果使用ES6的 解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive 返回的state对象，数据都不再是响应式的。示例如下：

```js
const state = reactive( {name: 'foo', age: 18} )

const { name, age } = state
```

**如何让解构出来的属性是响应式？**Vue为提供了 toRefs函数，可以将reactive返回的对象中的属性都转成ref

```js
const { name, age } = toRefs(state)
```

<font color=FF0000 size=4>**这种做法相当于已经在state.name和ref.value之间建立了 链接，任何一个修改都会引起另外一个变化**</font>

**toRef**

如果只希望转换一个reactive对象中的属性为ref，那么可以使用toRef的方法：

```js
const name = toRef(state, 'name')
```

##### ref的其他API

- **unref：**<font color=FF0000>想要获取一个ref引用中的value，可以通过 unref方法</font>

  如果参数是一个 ref，则返回内部值，否则返回参数本身；unref 是 `val = isRef(val) ? val.value : val` 的语法糖函数

- **isRef：**判断值是否是一个ref对象。

- **shallowRef：**创建一个浅层的ref对象，如果传入的对象是一个深层次的对象，则深层次的对象不是响应式的

- **triggerRef：**手动触发和 shallowRef 相关联的副作用，让深层的ref 响应式级别的更新

##### Composition API中的computed

**使用computed有两种方法：**

- 接收一个getter函数，并为 getter 函数返回的值，返回一个不变的 ref 对象
- 接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象

##### watchEffect 和 watch

- **watchEffect：**用于<font color=FF0000 size=4>**自动收集 响应式**</font>数据的依赖
- **watch：**需要<font color=FF0000 size=4>**手动指定侦听**</font>的数据源

##### watchEffect

当侦听到某些响应式数据变化时，我们希望执行某些操作，这个时候可以使用 watchEffect。

**watchEffect的特性如下：**

- <font color=FF0000>watchEffect 传入的函数 <font size=4>**会被立即执行一次**</font>，并且 <font size=4>**在执行的过程中会收集依赖（只要在函数中使用了，就添加到依赖，开始监听）**</font></font>（可以没有参数，根据执行结果，了解需要侦听哪些东西）
- <font color=FF0000>只有收集的依赖发生变化时，watchEffect传入的函数才会再次执行</font>
- watchEffect传入的函数，可以没有参数。

**watchEffect结束侦听：**获取watchEffect的返回值函数，调用该函数即可

##### 使用watchEffect清除副作用

**什么是清除副作用？**比如在<mark>开发中需要在侦听函数中执行网络请求</mark>，但是<font color=FF0000>**在网络请求还没有达到的时候，停止了侦听器、或者侦听器侦听函数被再次执行了：那么上一次的网络请求应该被取消掉**（因为请求失去意义了），这时就可以清除上一次的副作用</font>

**给watchEffect传入的函数被回调时，其实可以获取到一个参数：onInvalidate：**当 副作用即将重新执行 或者 侦听器被停止 时会执行该函数传入的回调函数。我们可以在传入的回调函数中，执行一些清除工作。

示例如下：

```js
const stopWatch = watchEffect((onInvalidate) => {
  console.log('watchEffect执行：', name.value, age.value)
  const timer = setTimeout(() => {
    console.log('2s后执行的操作')
  }, 2000)
  onInvalidate(() => {
    clearTimeout(timer)
  })
})
```



**watchEffect 的执行时机**

默认情况下，组件的更新会在副作用函数执行之前： 如果希望在副作用函数中获取到元素

```js
setup() {
  const titleRef = ref(null)
  const counter = 0
  
  watchEffect(() => {
    // 打印ref的值
    console.log(titleRef.value)
  })
  
  return {
    titleRef,
    counter
  }
}
```

结果：

```
null
<h2></h2>
```

**会发现打印结果打印了两次：**

- **第一次：打印null** <font color=FF0000>因为setup函数在执行时就会立即执行传入的副作用函数，这个时候DOM并没有挂载，所以打印为null</font>
- **第二次：打印\<h2>\</h2> **当DOM挂载时，会给title的ref对象赋值新的值，副作用函数会再次执行，打印出来对应的元素

**如果希望在第一次的时候就打印出来对应的元素，则可以调整watchEffect的执行时机**

watchEffect的第二个参数options中有一个flush的属性，默认执行的时机是pre；这个属性表示：watchEffect副作用会在元素 挂载 或者 更新 之前执行；所以会先打印出来一个null，当依赖的title发生改变时（即：挂载了），就会再次执行一次，打印出元素

把flush设置为post，则表示watchEffect 会在挂载之后执行。另外，flush 还可以设置为 sync，这将强制效果始终同步触发；然而，这是低效的，应该很少需要。



**watch的使用**

**watch的API完全等同于组件watch选项的Property：**

- watch需要<font color=FF0000>侦听特定的数据源，并在回调函数中执行副作用</font>
- <font color=FF0000 size=4>默认情况下它是惰性的</font>，<font color=FF0000>只有当被侦听的源发生变化时才会执行回调</font>

**与watchEffect相比，watch可以：**

- 懒性执行副作用（第一次不会直接执行）
- 更具体的说明当哪些状态发生变化时，触发侦听器的执行
- 访问侦听状态变化前后的值

**watch侦听单个数据源**

**watch侦听函数的数据源有两种类型：**

- **一个getter函数：**该<font color=FF0000>getter函数必须引用可响应式的对象</font>（比如reactive或者ref）

  ```js
  const state = reactive({
    name: 'foo',
    age: 18
  })
  
  // 第一个参数是getter
  watch(() => state.name, (newValue, oldValue) => {
    cnosole.log(newValue, oldValue)
  })
  
  const changeName = () => {
    state.name = 'bar'
  }
  ```

- **一个可响应式的对象：**reactive 或者 ref（比较常用的是ref）。

  - **侦听ref：**

    ```js
    const name = ref('foo')
    
    watch(name, (newValue, oldValue) => {
      console.log(newValue, oldValue)
    })
    
    const changeName = () => {
      name.value = 'bar'
    }
    ```

  - **侦听reactive：**如果写入的是reactive对象或数组，则newVal oldVal 获取到的都是reactive对象；如果想要获得值，则需要写入 getter，且使用解构赋值 `() => ( { ...info } )` 。如果写入的是ref对象，则newVal oldVal 都是值本身

    ```js
    const names = reactive(['foo', 'bar', 'baz'])
    watch(() => [...name], (newValue, oldValue) => {
      console.log(newValue, oldValue)
    })
    const changeName = () => {
      names.push('quz')
    }
    ```

**侦听多个数据源**

```js
const name = ref('foo')
const age = ref(18)

const changeName = () => {
  name.value = 'bar'
}

watch([name, age], (newValues, oldValues) => {
  console.log(newValues, oldValues)
})
```

**watch的选项**

- 如果希望侦听一个深层的侦听，那么依然需要设置 deep 为true
- 可以传入 immediate，表示希望 立即执行

```js
const state = reactive({
  name: 'foo',
  age: 18,
  friend: {
    name: 'bar'
  }
})

watch(() => state, (newValue, oldValue) => {
  console.log(newValue, oldValue)
}, {
  deep: true,
  immediate: true
})

const changeName = () => {
  state.friend.name = 'baz'
}
```



**setup中使用ref**

```html
<template>
	<h2 ref="titleRef">title</h2>
</template>

<script>
	import { ref } from 'vue'
  export default {
    setup() {
      const titleRef = ref(null)
      
      return {
        titleRef
      }
    }
  }
</script>
```



**setup函数中的生命周期钩子**

**setup中可以使用生命周期函数：**使用直接导入的 onX 函数注册生命周期钩子

| 选项式 API      | Hook inside setup |
| --------------- | ----------------- |
| beforeCreate    | Not needed*       |
| created         | Not needed*       |
| beforeMount     | onBeforeMount     |
| mounted         | onMounted         |
| beforeUpdate    | onBeforeUpdate    |
| updated         | onUpdated         |
| beforeUnmount   | onBeforeUnmount   |
| unmounted       | onUnmounted       |
| errorCaptured   | onErrorCaptured   |
| renderTracked   | onRenderTracked   |
| renderTriggered | onRenderTriggered |
| activated       | onActivated       |
| deactivated     | onDeactivated     |

> 因为 setup 是围绕 beforeCreate 和 created 生命周期钩子运行的，所以不需要显式地定义它们。换句话说，在这些钩子中编写的任何代码都应该直接在 setup 函数中编写。
>
> **coderwhy读源码的补充：**说是这么说，但是setup还是要在beforeCreate和created生命周期函数之前执行的

另外，<font color=red>**在一个setup中，可以写多个同样的生命周期函数，比如onMounted 可以写多个。作用：如果两个相同生命周期函数干了不同的事情，可以写两个进行逻辑区分**</font>



**setup中使用provide / inject**

provide可以传入两个参数：

- name：提供的属性名称
- value：提供的属性值

```js
let conter = 100
let info = {
  name: 'why',
  age: 10
}

provide('counter', counter)
provide('info', info)
```

**在 后代组件 中可以通过 inject 来注入需要的属性和对应的值**

可以通过 inject 来注入需要的内容，inject可以传入两个参数：

- 要 inject 的 property 的 name
- 默认值（如果祖先组件没有传递provide）

```js
const counter = inject('counter')
const info = inject('info')
```



**provide / inject 数据的响应式**

为了增加 provide 值和 inject 值之间的响应性，可以在 provide 值时使用 ref 和 reactive

```js
let conter = ref(100)
let info = reactive({
  name: 'why',
  age: 10
})

provide('counter', counter)
provide('info', info)
```

**修改响应式Property**

如果需要修改可响应的数据，那么最好是在数据提供的位置来修改（类似于子组件中的emit）： 可以将修改方法进行共享，在后代组件中进行调用。

```js
const changeInfo = () => {
  info.name = 'foo'
}
// provide的不是数据，而是修改数据的函数
provide('changeInfo', changeInfo)
```



#### `<script setup>` 中的 `define` 家族

> （👀 在 `<script setup>` 中 ）<font color=red>为了在声明 props 和 emits 选项时获得完整的 **类型推导支持**</font>，我们可以使用 defineProps 和 defineEmits API，它们将自动地在 `<script setup>` 中可用。
>
> 摘自：[Vue3 官方文档 - API - 单文件组件 `<script setup>` # `defineProps()` 和 `defineEmits()`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits)

> 👀 注：在使用 TS 开发中， 使用 `<script setup>` 的情况下，defineProps 是对 props 提供 “类型推导支持”；defineEmits 可以根据 `emits` 选项推导暴露在 setup 上下文中的 `emit` 函数的类型。<font color=dodgerBlue>如果没有使用 `<script setup>` ，还想要获得 相关的支持</font>，需要使用 `defineComponent()`，详见 [[#defineComponent 的作用]] 中官方文档的摘抄。

##### defineProps

在 `<script setup>` 中 可以使用 defineProps 在composition API 中注册 props

```js
import {defineProps} from 'vue'

const props = defineProps({
  message: {
    type: String,
    default: 'fooo'
  }
})
```

##### 关于 withDefaults 的补充

> 针对类型的 <font color=dodgerBlue>`defineProps` 声明的不足之处</font>在于，<font color=red>它没有可以给 props 提供默认值的方式</font>。<font color=red>为了解决这个问题</font>，我们还<font color=red>提供了 `withDefaults` 编译器宏</font>：
>
> ```ts
> export interface Props {
>   msg?: string
>   labels?: string[]
> }
> 
> const props = withDefaults(defineProps<Props>(), {
>   msg: 'hello',
>   labels: () => ['one', 'two']
> })
> ```
>
> 上面代码会被编译为等价的运行时 props 的 `default` 选项。此外，`withDefaults` 辅助函数提供了对默认值的类型检查，并确保返回的 `props` 的类型删除了已声明默认值的属性的可选标志。
>
> 摘自：[Vue3 官方文档 - API - 单文件组件 `<script setup>` # 使用类型声明时的默认 props 值](https://cn.vuejs.org/api/sfc-script-setup.html#default-props-values-when-using-type-declaration)

##### defineEmit

使用 defineEmit 以注册事件

```js
import { defineEmit } from 'vue'

const emit = defineEmit(['incr', 'desc'])
emit("incr", { params })
```

**defineExpose**

另外，还有 `defineExpose` 以设置需要暴露出去的值（ 类似于 `setup()` 中的 return ）

> 👀 上面说的不容易理解，参考 [Vue3 官方文档 - 模板引用 # 组件上的 ref](https://cn.vuejs.org/guide/essentials/template-refs.html#ref-on-component) 中的内容：
>
> > <font color=dodgerBlue>如果一个子组件使用的是 **选项式 API**</font> ，<font color=fuchsia>被引用的组件实例和该子组件的 this 完全一致</font>，<font color=red>这意味着父组件对子组件的每一个属性和方法都有完全的访问权</font>。<font color=lightSeaGreen>这使得在父组件和子组件之间创建紧密耦合的实现细节变得很容易</font>，<font color=fuchsia>当然也因此，应该只在绝对需要时才使用组件引用</font>。大多数情况下，你应该首先使用标准的 props 和 emit 接口来实现父子组件交互。
> >
> > 有一个例外的情况，<font color=fuchsia><font size=4>使用了 `<script setup>` 的组件是**默认私有**的</font>：一个父组件无法访问到一个使用了 `<script setup>` 的子组件中的任何东西，除非子组件在其中通过 `defineExpose` 宏显式暴露</font>
> >
> > ```vue
> > <script setup>
> > import { ref } from 'vue'
> > 
> > const a = 1
> > const b = ref(2)
> > 
> > defineExpose({ a, b })
> > </script>
> > ```
> >
> > <font color=red>当父组件通过模板引用获取到了该组件的实例时，得到的实例类型为 `{ a: number, b: number }`</font> （ ref 都会自动解包，和一般的实例一样）。



#### defineComponent 的作用

在使用 TS 编写 SFC 时，`export default { ... }` 应该写成 `export default defineComponent( { ... })` ；这里 defineComponent 函数 相当于做了一个约束，在其定义中给出了各种类型定义，使得其中编写的内容有了类型约束；使得在编写中可以主动约束，并做好类型推导，减少错误。

> 👀 注：上面是当时学习时的总结，并不准确，下面摘抄下 [Vue3 官方文档 - TypeScript 与组合式 API](https://cn.vuejs.org/guide/typescript/composition-api.html#without-script-setup) 中的内容：
>
> > <mark style="background: LightSkyBlue">在文档的  “# 为组件的 props 标注类型 # 非 \<script setup>` 场景下” 部分：</mark>
> >
> > <font color=dodgerBlue>如果没有使用 `<script setup>`</font> ，那么<font color=red>为了开启 props 的类型推导，必须使用 `defineComponent()`</font> 。传入 `setup()` 的 props 对象类型是从 `props` 选项中推导而来。
> >
> > ```ts
> > import { defineComponent } from 'vue'
> > 
> > export default defineComponent({
> >   props: {
> >     message: String
> >   },
> >   setup(props) {
> >     props.message // <-- 类型：string
> >   }
> > })
> > ```
> >
> > <mark style="background: LightSkyBlue">在文档的 “# 为组件的 emits 标注类型” 部分：</mark>
> >
> > 若没有使用 `<script setup>` ，`defineComponent()` 也可以根据 `emits` 选项推导暴露在 setup 上下文中的 `emit` 函数的类型：
> >
> > ```ts
> > import { defineComponent } from 'vue'
> > 
> > export default defineComponent({
> >   emits: ['change'],
> >   setup(props, { emit }) {
> >     emit('change') // <-- 类型检查 / 自动补全
> >   }
> > })
> > ```



#### h函数

Vue推荐在绝大数情况下使用模板来创建HTML，然后一些特殊的场景，真的需要JavaScript的完全编程的能力，这个时候可以使用 渲染函数 ，它比模板更接近编译器。

Vue在生成真实的DOM之前，会将节点转换成VNode（转换是通过编译，即：compile），而 VNode组合在一起形成一颗树结构，就是虚拟DOM（VDOM）。<font color=FF0000>事实上，之前编写的 template 中的HTML 最终也是使用渲染函数（即：render函数）生成对应的VNode</font>；如果想充分的利用 JavaScript的编程能力，<font color=FF0000>可以自己来编写 <font size=4>**createVNode 函数，生成对应的 VNode**</font></font>。

**至于该怎么做，使用h() 函数：**h() 函数是一个用于创建 vnode 的一个函数，<font color=FF0000>更准确的命名是 createVNode() 函数，但是为了简便在Vue将之简化为 h() 函数</font>

**h() 函数的使用方法**

h() 函数接受三个参数：

- **组件名 / 标签名：**必需
- **标签中的属性 prop：**可选
- **子VNode：**可选

```js
// @returns {VNode}
h(
  // {String | Object | Function} tag
  // 一个 HTML 标签名、一个组件、一个异步组件、或一个函数式组件。
  //
  // 必需的。
  'div',

  // {Object} props
  // 与 attribute、prop 和事件相对应的对象。这会在模板中用到。
  //
  // 可选的。
  {},

  // {String | Array | Object} children
  // 子 VNodes，使用 `h()` 构建，或使用字符串获取 "文本 VNode" 或者有插槽的对象。
  //
  // 可选的。
  [
    'Some text comes first.',
    h('h1', 'A headline'),
    h(MyComponent, {
      someProp: 'foobar'
    })
  ]
)
```

**注意事项：**如果没有props，那么通常可以将children作为第二个参数传入；如果会产生歧义，可以将null作为第二个参数传入，将children作为第三个参数传入。

**h函数可以在两个地方使用：**

- **render 函数中：**

  ```js
  import { h } from 'vue'
  
  export default {
    render() {
      return () => h('div', {class: 'app'}, 'hello world')
    }
  }
  ```

- **setup函数选项中：**（setup本身需要是一个函数类型，函数再返回h函数创建的VNode）

  ```js
  import { h } from 'vue'
  
  export default {
    setup() {
      return () => h('div', {class: 'app'}, 'hello world')
    }
  }
  ```

**h函数计数器案例：**

```js
data() {
  return { counter: 0 }
},
render() {
  return h(
  	'div',
    {class: 'app'},
    [
      h('h2', null, `当前计数：${this.counter}`),
      h('button', { onClick: () => this.counter++ }, '+1')
      h('button', { onClick: () => this.counter-- }, '-1')
    ]
  )
}
```

**h函数 组件和插槽的使用**

```js
// HelloWorld.vue 定义组件
import { h } from 'vue'

export default {
  render() {
    return h(
    	'div',
      {class: 'hello-world'},
      [
        h('h2', null, 'hello world'),
        this.$slots.default
          ? this.$slots.default({info: 'hahaha'}) // {info: 'hahaha'}是作用域插槽
        	: h('span', null, '我是默认值') // 如果调用者没有传递插槽的默认值
      ]
    )
  }
}
```

```js
// app.vue 使用HelloWorld组件
export default {
  render() {
    return h(HelloWrold, null, {
      default: prop => h('span', null, 'app 传入到Helloworld中的内容')
    })
  }
}
```



**使用h() 函数还是有些复杂，可以使用跟符合编写习惯的jsx：**

jsx的babel 在上面的webpack babel配置部分。

**jsx计数器案例：**

```js
export default {
  setup() {
    const counter = ref(0)
    const increment = () => counter.value ++
    const decrement = () => counter.value --
    
    return {
      counter,
      increment,
      decrement
    }
  }
  
  render() {
    return {
      <div>
      	<h2>当前计数：{this.counter}</h2>
    		<button onClick={this.increment}>+1</button>
    		<button onClick={this.decrement}>-1</button>
      </div>
    }
  }
}
```



#### nextTick

将回调推迟到下一个 DOM 更新周期之后执行。在更改了一些数据以等待 DOM 更新后立即使用它

**nextTick的实现原理：**

事件循环队列，下面的都是微任务

- **watch（回调函数）：**preQueue

  另外，<font color=FF0000>如果使用for循环给foo加+1，加100次，同时使用watch监听foo，foo的watch不会执行100次；因为watch是微任务，在同步代码for执行完后，再执行，所以只会执行一次</font>

- 组件的更新：jobqueue

- 生命周期回调：postQueue



#### 自定义指令

Vue 也允许我们来自定义自己的指令。 注意：在Vue中，代码的复用和抽象主要还是通过组件；通常在某些情况下，你需要对DOM元素进行底层操作，这个时候就会用到自定义指令。

**自定义指令分为两种：**

- **自定义局部指令：**组件中通过 <font color=FF0000>**directive<font size=5>s</font>**</font> 选项，只能在当前组件中使用
- **自定义全局指令：**app的 <font color=FF0000>**directive**</font> 方法，可以在任意组件中被使用

**代码示例：**

- 实现v-focus自动聚焦功能

  - **局部指令：**

    需要在组件选项中使用 directives，directives是一个对象，在对象中编写自定义指令的名称（注意：这里不需要加 v- ）。<font color=FF0000>自定义指令有一个生命周期，是在组件挂载后调用的 mounted，可以在其中完成操作</font>

    ```html
    <input v-focus />
    
    <script>
    	export default {
    	  directives: {
    	    focus: {
            // 使用了生命周期
    	      mounted(el) { el.focus() }
    	    }
    	  }
    	}
    </script>
    ```

  - **全局指令：**

    ```js
    app.directive('focus', {
      mounted(el) {
        el.focus()
      }
    })
    ```

  **指令的生命周期**

  <font color=FF0000>一个指令定义的对象，Vue提供了如下的几个钩子函数：</font>

  - created：在绑定元素的 attribute 或事件监听器被应用之前调用
  - beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用
  - mounted：在绑定元素的父组件被挂载后调用
  - beforeUpdate：在更新包含组件的 VNode 之前调用
  - updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用
  - beforeUnmount：在卸载绑定元素的父组件之前调用
  - unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次；

  **指令的参数和修饰符**

  指令需要接受一些参数或者修饰符：比如info是参数名称，aaa和bbb是修饰符名称，等号后面是传入具体的值：

  ```html
  <button v-foo:info.aaa.bbb="{name: 'coderwhy', age: 18}">{{counter}}</button>
  ```

  <font color=FF0000>在指令的生命周期中，可以通过 bindings 获取到对应的内容</font>

  ![image-20211125105019540](https://i.loli.net/2021/11/25/hAv5j3DPwpE6fal.png)

如上图所示：参数可以通过 binding.arg 获取，修饰符通过 binding.modifiers 中的对象获取，值通过binding.value 中的对象获取。



#### Teleport

##### 需求

组件化开发中，封装一个组件A，在另外一个组件B中使用：那么组件A中 template 的元素，会被挂载到组件B中 template 的某个位置；最终我们的应用程序会形成一颗DOM树结构。

但在某些情况下，<mark>希望组件不是挂载在这个组件树上，需要移动到Vue app之外的其他位置：比如移动到body元素上，或者我们有其他的div#app 之外的元素上</mark>；这时就可以通过teleport来完成。

##### Teleport 是什么

它是一个Vue提供的内置组件，类似于react的Portals。

teleport翻译过来是心灵传输、远距离运输的意思。**teleport有两个属性**：

- **to：**<font color=FF0000>指定将其中的内容移动到的目标元素，可以使用选择器</font>
- **disabled：** 是否禁用 teleport 的功能

**Teleport 和组件结合使用**

```html
<teleport to="body">
	<hello-world message="foo"></hello-world>
</teleport>
```

**多个Teleport同时使用**

可以将多个Teleport应用到同一个目标上（to的值相同），<font color=FF0000>这些Teleport会进行合并</font>

```html
<template>
	<teleport to="#why">
  	<h2>coderwhy</h2>
  </teleport>
  <teleport to="#why">
  	<hello-world message="我是App中的message" />
  </teleport>
</template>
```

![image-20211125113556338](https://i.loli.net/2021/11/25/GVsy6biwFKrOA1N.png)



#### Vue插件

通常向Vue全局添加一些功能时，会采用插件的模式，**插件有两种编写方式**：

- **对象类型：**一个对象，但是<font color=FF0000>必须包含一个 install 的函数</font>，<font color=FF0000>该函数会在安装插件时执行</font>

  ```js
  export default {
    name: 'foo',
    install(app, options) {
      console.log('组件安装', app, options)
      console.log(this.name)
    }
  }
  ```

- **函数类型：** 一个function，<font color=FF0000>这个函数会在安装插件时自动执行</font>

  ```js
  export default function(app, options) {
    console.log('组件安装', app, options)
  }
  ```

**插件可以完成的功能没有限制，比如下面的几种都是可以的：**

- 添加全局方法或者 property，通过把它们添加到 config.globalProperties 上实现
- **添加全局资源：** 指令/过滤器/过渡等
- **通过全局 mixin 来添加一些组件选项**
- 一个库，提供自己的 API，同时提供上面提到的一个或多个功能



#### 阅读源码

<font color=FF0000>听起来还是有些不理解，建议参考原视频 L20、L21 阅读</font>



#### 虚拟DOM的优势

目前框架（包含前端三大框架 React Vue Angular ）都会引入虚拟 DOM 来对真实的 DOM 进行抽象，这样做有很多的好处：

- 可以对真实的元素节点进行抽象，抽象成 VNode（虚拟节点），这样方便后续对其进行各种操作
  - 直接操作DOM来说是有很多的限制的，比如 diff 、clone 等等，但是使用 JavaScript 编程语言来操作这 些，就变得非常的简单
  - 可以使用 JavaScript 来表达非常多的逻辑，而对于 DOM 本身来说是非常不方便的
  - **自己补充** 还有一种说法是：浏览器的重绘回流等操作，对 dom 操作效率没有 JS 高
- 方便实现跨平台，包括你可以将VNode节点渲染成任意你想要的节点（作为中间层，便于转化）
  - 如渲染在canvas、WebGL、SSR、Native（iOS、Android）上
  - 并且Vue允许你开发属于自己的渲染器（renderer），在其他的平台上渲染



**虚拟DOM的渲染过程**

![image-20211128002120961](https://i.loli.net/2021/11/28/b7MrTiZLv8sUIcp.png)



**Vue源码三大核心**

**Vue的源码包含三大核心：**

- **Compiler模块：**编译模板系统。将template转换为render()函数

- **Runtime模块：**也可以称之为Renderer模块，真正渲染的模块。

- **Reactivity模块：**响应式系统。当数据发生改变时，组件进行刷新。对新旧VNode做diff和patch

源码中三大核心对应的文件模块

![image-20211128002442727](https://i.loli.net/2021/11/28/TonBkxZGr8wqHma.png)

三个系统之间的协同工作示意图：

![image-20211128002640088](https://i.loli.net/2021/11/28/V8bvJKRaFtDi7QZ.png)



**简单实现渲染系统**

该模块主要包含三个功能：

- h函数：用于返回一个VNode对象
- mount函数：用于将VNode挂载到DOM上
- patch函数：用于对两个VNode进行对比，决定如何处理新的VNode



**简单的实现h函数**

直接返回一个VNode对象即可：

```js
const h = (tag, props, children) => {
  return {
    tag,
    props,
    children
  }
}
```



**简单实现 mount函数**

mount函数的实现：

1. 根据tag，创建HTML元素，并且存储 到vnode的el中

2. 处理props属性
   1. 如果以on开头，那么监听事件
   2. 普通属性直接通过 setAttribute 添加即可；

3. 处理子节点
   1. 如果是字符串节点，那么直接设置 textContent
   2. 如果是数组节点，那么遍历调用 mount 函数



// TODO 抄不下去了...



**createApp**

Vue.createApp函数 的定义 在runtime-dom 中的index.ts 文件，createApp主要是通过渲染器实现的；而渲染器是在 runtime-core 中实现的，文件主要是renderer.ts，其返回了一个 render、hydrate（用于SSR）、createApp（createApp是createAppAPI 函数的返回值，即：上面的createApp就是用createAppAPI实现 ）

h函数，就是在调用createVNode 函数

**面试题：组件的VNode 和 组件的instance 有什么区别？**

- **VNode：**是虚拟DOM的虚拟结点
- **组件的instance：**用于保存组件的各种状态（data/ setup / computed / methods / watch 中定义的数据）

setupComponent(instance) 函数是对 组件所有的数据进行操作和赋值的代码。

hostCreateElement 实际上是在调用 document.createElemet()，而之所以加上host的前缀，是因为它是跨平台的方法；类似的host前缀也一样。

**总结，createApp以及mount过程示例：**

createApp -> app.mount -> rerender 的 render 函数 -> patch（会判断类型：比如元素element类型 / 组件类型） --如果是组件类型--> processComponent（会判断组件是否挂载）--如果没有挂载--> mountComponent -> 创建组件实例instance -> setupComponent(instance) -> setupRenderEffect（会调用effect，effect中传入一个函数：effect(() => { })，判断是否挂载；如果没挂载，就挂载；否则就更新 ）--如果没有挂载--> 取出subtree -> 再次来到patch --如果是一个element--> processElement -> 通过document.createElement() -> mountChildren() -> 做for循环，拿到child -> 进行patch，再次判断是元素类型还是组件类型，递归的走上面patch的逻辑。

图示如下：

![image-20211118153749751](https://i.loli.net/2021/11/18/9K8zfp1DFen7bXU.png)

![image-20211118153809289](https://i.loli.net/2021/11/18/YCMSyrWKDg9Pob4.png)

**初始化组件的过程：**入口是使用了setupComponent 函数

![image-20211118161044876](https://i.loli.net/2021/11/18/EUu8Qlob5cqDY7x.png)

- const app = {props: {message: String}}   

- instance 

- 1. 处理props和attrs

  - instance.props 
  - instance.attrs

- 2. 处理slots

  - instance.slots 

- 3. 执行setup

  - const result = setup()
  - instance.setupState = proxyRefs(result);

- 4. 编译template -> compile 

  - \<template> -> render函数
  - instance.render = Component.render = render函数

- 5. 对vue2的options api进行支持，比如：data/methods/computed/生命周期



面试题：data和setup中定义同名不同值的数据，并使用，会使用data中的值还是setup的值？

答案：setup中的数据。因为setup中的数据如果（使用if）存在，则优先获取；没有（使用else / else-if ）才会从data / computed / ... 中获取



#### Vue Router

##### 前端路由的两种实现思路：hash / history

一般 一个url对应着后端的一个网页，所以<font color=FF0000>输入一个url或url发生改变，就会向后端请求资源</font>。但是<font color=FF0000>有两种方法可以让url改变，但是不会向后端请求资源；分别是hash和history</font>。通过这两种方式，是前端自己渲染。

前端路由是通过监听URL的改变，做到URL和内容进行映射。

##### URL的 hash

URL 的 hash也就是锚点(#)，<font color=FF0000>本质上是改变window.location的href属性； 可以通过直接赋值 location.hash 来改变href，但保证页面不发生刷新</font>

hash的优势就是兼容性更好，在老版IE中都可以运行，但是缺陷是有一个#，显得不像一个真实的路径；另外SEO 不友好

##### HTML5 History

history接口是HTML5新增的，它有六种模式改变URL而不刷新页面：

- **replaceState：**替换原来的路径
- **pushState：**使用新的路径
- **popState：**路径的回退
- **go：**向前或向后改变路径
- **forward：**向前改变路径
- **back：**向后改变路径



**Vue-Router是<font color=FF0000>基于路由和组件</font>的：**

- 路由<font color=FF0000>用于设定访问路径，将路径和组件映射起来</font>

- 在vue-router的<font color=FF0000>单页面应用中，页面的路径的改变就是组件的切换</font>



**路由的使用步骤**

1. 创建路由组件的组件

2. 配置路由映射：组件和路径映射关系的routes数组

   ```js
   // router/index.js
   import Home from '../pages/Home.vue'
   
   const routes = [
     { path: '/home', component: Home},
     { path: '/about', component: import('../pages/About.vue') }
   ]
   ```

   另外，<font color=FF0000>router数组中一个对象就是一个route；整体是router</font>。

3. 通过createRouter创建路由对象，并且传入routes 和 history模式

   ```js
   // router/index.js
   import { createRouter, createWebHashHistory } from 'vue-router'
   
   const router = createRouter({
     routes,
     history: createWebHashHistory()
   })
   
   export default router
   ```

4. 使用路由: 通过 \<router-link> 和 \<router-view>

   ```js
   // main.js
   import router from './router'
   
   createApp(App).use(router).mount('#app')
   ```

   ```html
   <!-- app.vue -->
   <tempalte>
   	<div class='app'>
       <p>
         <router-link to="/home">首页</router-link>
         <router-link to="/about">关于</router-link>
       </p>
       <router-view />
     </div>
   </tempalte>
   ```



**history模式**

上面createRouter使用的是hash模式，还可以选择history 模式：

```js
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  routes,
  history: createWebHistory()
})
```



**路由的默认路径**

默认情况下, 进入网站的首页, 我们希望 \<router-view>渲染首页的内容；但是目前上面的实现中，默认没有显示首页组件，必须让用户点击才可以。如何让路径默认跳到到首页, 并且 \<router-view> 渲染首页组件？

```ts
// router/index.js
const routes = [
  { path: '/', redirect: '/home'},
  { path: '/home', component: Home},
  // ...
]
```

这里是用的是重定向 redirect，也可以用component: Home。<font color=FF0000>用redirect更好些，毕竟如果Home的组件名修改了，默认路径也要跟着修改；用redirect就无需再管Home组件名是否改变了</font>



**router-link**

router-link事实上有很多属性可以配置：

- **to属性：**是一个字符串，或者是一个对象

- **replace属性：**设置 replace 属性的话，当点击router-link对应的组件时，会调用 router.replace()，这样history不会压栈，无法返回；而不是 router.push()，history会压栈，可以返回。另外，默认是push模式( router.push() )

  ```html
  <router-link to="/home" replace>首页</router-link>
  ```

- **active-class属性：**设置激活a元素后应用的class（被添加到router-link 标签上），默认是router-link-active；通过给active-class 赋值，从而改变class的名字

- **exact-active-class 属性：**链接精准激活时，应用于渲染的 \<a> 的 class，默认是router-link-exact-active。同样，可以修改名字。

  exact-active-class 表示 精准激活（匹配），即：选中了当前的router-link（完全匹配），虽然这时自己的父组件也被激活，但父组件不精准（没有完全匹配），所以父组件不会加上 exact-active-class

**router-link的v-slot**

在vue-router3.x的时候，router-link有一个tag属性，可以决定router-link到底渲染成什么元素。从vue-router4.x开始，该属性被移除了； 而<font color=FF0000>提供了更加具有灵活性的v-slot的方式来定制渲染的内容</font>。<mark>更灵活，即：tag只能用一般元素，**还可以用组件**，**甚至可以用多个元素和组件**</mark>

**router-link 使用v-slot 的方法：**

- <font color=FF0000>使用 custom 表示我们整个元素要自定义</font>，如果不写，那么自定义的内容会被包裹在一个 a 元素中。

  这里如果只加上custom，外层的 \<a> 将会消失，点击slot内容不会发生跳转，因为未定义。除了@click="$router.push()" 之外，还可以用 v-slot传值方式 v-slot="props"，比如：\<p @click=“props.navigate”>\</p>

- 其次，使用 v-slot 来作用域插槽来获取内部传给我们的值

  - href：解析后的 URL
  - route：解析后的规范化的route对象
  - navigate：触发导航的函数
  - isActive：是否匹配的状态
  - isExactActive：是否是精准匹配的状态

  ```html
  <router-link to="/about" v-slot="{href, route, navigate, isActive, isExactActive}">
  	<p @click="navigate">跳转about</p>
    <div>
      <p>href: {{ href }}</p>
      <p>route: {{ route }}</p>
      <p>isActive: {{ isActive }}</p>
      <p>isExactActive: {{ isExactActive }}</p>
    </div>
  </router-link>
  ```

  

**router-view**

\<router-view>用于占位，表示组件的内容放在哪里

和router-view一样，router-view也提供给我们一个插槽，可以用于 \<transition> 和 \<keep-alive> 组件来包裹路由组件

**v-slot中可以传递的参数：**

- Component：要渲染的组件
- route：解析出的标准化路由对象

```html
<router-view v-slot="{ Component, route }">
	<transition name="foo">
    <compoent :is="Component"></compoent>
  </transition>
</router-view>
```



**路由懒加载**

默认情况下，当打包构建应用时，JavaScript 包会变得非常大（打成一个js文件），影响页面加载（首屏渲染）。这时候需要分包，Vue Router默认就支持动态来导入组件。<font color=FF0000>component可以传入一个组件，也可以接收一个函数，该函数需要返回一个Promise</font>； 而 import 函数就是返回一个Promise。

打包之后的结果是hash，难以定位，可以使用image commet在路由中为打包后的组件命名。这是由于：webpack 从 3.x 开始支持对分包进行命名 ( chunk name )

```js
component: import(/* webpackChunkName: "home-chunk"*/ '../pages/About.vue')
```



**路由的其他属性**

- **name：**路由记录<font color=FF0000>独一无二的名称</font>
- **meta：**自定义的数据

```js
{
  path: '/about',
  name: 'about-router',
  component: () => import('../pages/About.vue'),
  meta: {
    name: 'foo',
    age: 18
  }
}
```



**动态路由匹配**

有时需要将 给定匹配模式的路由映射到同一个组件：比如User组件，根据用户ID的不同，显示不同的内容。

在Vue Router中，可以在路径中使用一个动态字段来实现，我们称之为 路径参数 ( params )

```js
{
  path: '/user/:id',
  component: () => import('../pages/User.vue')
}
```

使用：

```html
<router-link to="/router/123">user 123</router-link>
```

**动态路由的值可以通过 $route.param 获取**

- 在template中，直接通过 $route.params[.youParam] 获取

- 在options api中，通过 this.$route.params[.youParam] 获取

- 在setup中，<font color=FF0000 size=4>要使用 vue-router库提供的一个hook：**useRoute**</font>。<font color=FF0000>该Hook会返回一个Route对象，对象中保存着当前路由相关的值</font>

  ```js
  setup() {
    const route = useRoute()
    console.log(route)
    console.log(route.params.id)
  }
  ```

**匹配多个参数：**

```js
{
  path: '/user/:id/info/:name',
  component: () => import('../pages/User.vue')
}
```

| 匹配模式              | 匹配路径           | $route.params              |
| --------------------- | ------------------ | -------------------------- |
| /users/:id            | /users/123         | { id: '123' }              |
| /users/:id/info/:name | /user/123/info/foo | { id: '123', name: 'foo' } |



**NotFound**

对于那些没有匹配到的路由，我们通常会匹配到固定的某个页面。比如匹配到NotFound页面，这个时候可编写一个动态路由用于匹配所有的页面。

```js
{
  // 表示上面所有的route都不匹配
  path: '/:pathMatch(.*)',
  component: () => import('../pages/NotFound.vue')
}
```

同时，可以通过 $route.params.pathMatch获取到传入的参数，是以 `/` 分隔的字符串。

另外，可以在 `'/:pathMatch(.*)'` 的后面加上一个*，即 `'/:pathMatch(.*)*'` ，可以让 $route.params.pathMatch获取到解析 `/` 之后的参数，一个数组。如下图所示：

<img src="https://i.loli.net/2021/11/24/SscULJIugnKqF5T.png" alt="image-20211124235358723" style="zoom:50%;" />



**路由的嵌套**

在页面上，路由中包含多个子路由，子路由中包含多个孙子路由；这就需要嵌套路由。而在子路由中，同样需要使用 router-view 来占位之后需要渲染的组件。示例如下：

```js
{
  path: '/home',
  component: () => import('../pages/Home.vue'),
  children: [
    {
      path: '/',
      redirect: '/home/product',
    },
    // 注意：这里children中route的path不需要再写父组件的路径了，直接写自己的路径即可；但是对于redirect还是要添加完整路径
    {
      path: 'product',
      component: () => import('../pages/HomeProduct.vue')
    },
    {
      path: 'message',
      component: () => import('../pages/HomeMessage.vue')
    }
  ]
}
```



**代码的页面跳转**

有时希望通过代码来完成页面的跳转，比如点击的是一个按钮

```js
jumpToProfile() {
  this.$router.push('/profile')
}
```

**传入的是对象：**

```js
jumpToProfile() {
  this.$router.push( { path: '/profile' } )
}
```

**传入的对象包含其他数据 ( query )：**

```js
jumpToProfile() {
  this.$router.push({ 
    path: '/profile',
    query: { name: 'foo', age: 18}
  })
}
```

传递的数据通过 $route.query 获取

**在setup函数中编写，可以通过useRoute来进行**

```js
const router = useRouter()

const jumpToProfile = () => {
  router.replace('/profile')
}
```

**Vue router的跳转的方法借鉴了 History API**，使用方法是一样的。

- **router.push()**
- **router.replace()**
- **router.go()**
  - router.go(1)：向前移动一条记录，和router.forward() 相同
  - router.go(-1)：向后移动一条记录，和router.back() 相同
  - router.go(100)：如果没有这么多记录，静默失败
- **router.back()：**调用 history.back() 回溯历史。相当于 router.go(-1)
- **router.forward()：**历史中前进。相当于 router.go(1)



**动态添加删除路由**

某些情况可能需要动态的来添加路由：比如根据用户不同的权限，注册不同的路由；<font color=FF0000>这时可以使用一个方法 addRoute</font>。另外，仅仅删除跳转按钮是不够的，用户还可以通过url手动进入，这时会带来安全隐患；所以还需要删除对应的路由。

```js
// 添加顶层路由
const categoryRoute = {
  path: '/category',
  component: () => import('../pages/Category.vue')
}
router.addRoute(categoryRoute)

// 添加作为home的子路由
const homeMonentRoute = {
  path: 'moment',
  component: () => import('../pages/HomeMonent.vue')
}
router.addRoute('home', homeMomentRoute)
```

**动态删除路由**

**删除路由有以下三种方式：**

1. **添加一个name相同的路由：**覆盖掉想要删除掉路由（类似于cookie）

   ```js
   router.addRoute( {path: '/about', name: 'about', component: About} )
   // 这回删除掉之前添加的同名路由，因为他们具有相同的名字，且名字必须唯一
   router.addRoute( {path: '/other', name: 'about', component: Other} )
   ```

2. **通过removeRoute方法，传入路由的名称**

   ```js
   router.addRoute( {path: '/about', name: 'about', component: About} )
   // 删除路由
   router.removeRoute('about')
   ```

3. **通过addRoute方法的返回值回调：**类似于watch的返回值

   ```js
   const removeRoute = router.addRoute( routeRecord )
   removeRoute()
   ```

**其他路由方法：**

- **router.hasRoute()：**检查路由是否存在。
- **router.getRoutes()：**获取一个包含所有路由记录的数组



**路由导航守卫**

**Vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航**。全局的前置守卫beforeEach是在导航触发时会被回调的

**有两个参数：**

- **to：**即将进入的路由Route对象
- **from：**即将离开的路由Route对象
- **可选的第三个参数：next**
  - 在Vue2中我们是通过next函数来决定如何进行跳转的
  - <font color=FF0000>在**Vue3**中我们是 **通过返回值来控制**的，**不再推荐使用next函数**，这是因为开发中很容易调用多次next</font>；具体返回值见下。

**返回值：**

- **false：**取消当前导航

- **不返回或者undefined：**进行默认导航

- **返回一个路由地址：**

  - 可以是一个string 类型的路径，跳转到对应的路径中

  - 可以是一个对象，对象中包含path、query、params等信息

**其他导航守卫**

Vue还提供了很多的其他守卫函数，目的都是在某一个时刻给予我们回调，让我们可以更好的控制程序的流程或者功能。

**完整的导航解析流程：**

- 导航被触发
- 在失活的组件里调用 beforeRouteLeave 守卫
- 调用全局的 beforeEach 守卫
- 在重用的组件里调用 beforeRouteUpdate 守卫(2.2+)
- 在路由配置里调用 beforeEnter
- 解析异步路由组件
- 在被激活的组件里调用 beforeRouteEnter
- 调用全局的 beforeResolve 守卫(2.5+)
- 导航被确认
- 调用全局的 afterEach 钩子
- 触发 DOM 更新
- 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入



#### Vuex

**状态管理**

在开发中，应用程序需要处理各种各样的数据，这些数据需要保存在我们应用程序中的某一个位置，对于这些数据 的管理我们就称之为是 **状态管理**

**当前（不使用Vuex）管理状态的方法：**

- 在组件中定义data 或者在setup 中返回使用的数据，这些数据称之为 state
- 在模块template 中可以使用这些数据，模块最终会被渲染成DOM，称之为View
- 在模块中我们会产生一些行为事件，处理这些行为事件时，有可能会修改state，这些行为事件称之为actions

<img src="https://i.loli.net/2021/11/25/FNYXnUmg92ViT3B.png" alt="img" style="zoom:33%;" />



**Vuex思想**

<font color=FF0000>**将组件的内部状态抽离出来，以一个全局单例的方式来管理：**</font>

在这种模式下，<font color=FF0000>组件树构成了一个巨大的 “视图View”； 不管在树的哪个位置，任何组件都能获取状态或者触发行为</font>。<font color=FF0000>通过定义和隔离状态管理中的各个概念，并通过强制性的规则来维护视图和状态间的独立性，我们的代码边会 变得更加结构化和易于维护、跟踪</font>

<img src="https://i.loli.net/2021/11/25/pt9GgQ5ayDUhbYq.png" alt="vuex" style="zoom:90%;" />

**单一状态树**

**Vuex 使用单一状态树：**

- 用<font color=FF0000>一个 Store 对象就包含了全部的应用层级状</font>
- 采用的是SSOT，Single Source of Truth，也可以翻译成单一数据源
- 也意味着，<font color=FF0000>每个应用将仅仅包含一个 store 实例</font>
- <font color=FF0000>单状态树和模块化并不冲突</font>，因为有module的概念

**单一状态树的优势：**

<mark>如果状态信息是保存到多个Store对象中的，那么之后的管理和维护等等都会变得特别困难；所以Vuex也使用了单一状态树来管理应用层级的全部状态</mark>。<font color=FF0000>单一状态树能够让我们最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中，也可以非常方便 的管理和维护</font>



**创建Store**

每一个Vuex应用的核心就是store（仓库）：store本质上是一个容器，它包含着你的应用中大部分的状态（state）

**Vuex和 单纯的全局对象的区别：**

- <font color=FF0000>**Vuex的状态存储是响应式的：**</font>当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会被更新

- <font color=FF0000>**不能直接改变store中的状态**</font>
  - 改变store中的状态的唯一途径就显示提交 (commit) mutation
  - 这样使得我们可以方便的跟踪每一个状态的变化，从而让我们能够通过一些工具帮助我们更好的管理应用的状态



**组件获取状态**

可以使用计算属性获取状态：

```js
computed: {
  counter() {
    return this.$store.state.counter
  }
}
```

如果我们有很多个状态都需要获取（也就是说要写很多个计算属性，将会很麻烦），<font color=FF0000>可以使用mapState的辅助函数</font>，有两种方式（另外，<font color=FF0000>也可以使用展开运算符和来原有的computed混合在一起</font>）：

- **数组类型：**mapState的返回值是一个对象，所以可以这样写：数组形式使用展开运算符

  ```js
  computed() {
    ...mapState(['name', 'height', 'age'])
  }
  ```

- **对象类型：**使用数组无法自定义名字，可能会与组件内部数据名冲突；所以，使用数组起别名：

  ```js
  computed: {
  	...mapState({
  		sName: state => state.name; // 为name起别名sName
  		// ...
  	})
  }
  ```

**在setup中使用mapState**

在setup中获取store中的属性，通过useStore拿到store后去获取某个状态即可

```js
import { computed } from 'vue'
import { useStore } from 'vuex'

setup() {
	const store = useStore() // 获取store
	const sName = computed( () => store.state.counter )
}
```

默认情况下，<font color=FF0000>Vuex并没有提供非常方便的使用mapState的方式，这里进行了一个函数的封装</font>

```js
import { useStore, mapState } from 'vuex'
import { computed } from 'vue'

export function useState(mapper) {
  const store = useStore()
  
  // 返回值是一个函数数组，因为 ...mapState(...) 放在computed中，也是一个个函数
  const stateFns = mapState(mapper)
  
  const state = {}
  Object.keys(stateFns).forEach(fnKey => {
    state[fnKey] = computed(stateFn[fnKey].bind({$store: strore}))
  })
  return state
}
```

使用：

```js
setup() {
  const state = useState({
    name: state => state.name,
    age: state => state.age,
    height: state => state.height
  })
  return {
    ...state
  }
}
```



**getters的使用**

某些属性我们可能需要经过变化后来使用，这个时候可以使用getters。

另外，之所以这样的数据的处理：将逻辑封装在store的getter中，而不是组件中，是因为避免同样逻辑多次编写。

```js
const store = createStore({
  state() {
    return {
      counter: 0,
      name: 'foo',
      age: 18,
      height: 1.88,
      books: [
        {name: 'vuejs', count: 2, price: 110},
        {name: 'react', count: 3, price: 120},
        {name: 'webpack', count: 4, price: 130},
      ]
    }
  },
  getters: {
    totalPrice(total) {
      let totalPrice = 0
      for ( const book of state.books) {
        totalPrice += book.count * book.price
      }
      return totalPrice
    }
  }
})
```

使用：

```html
<h2>{{ $store.getters.totalPrice }}</h2>
```

**getters 可以有第二个参数 getters：**可以在一个getter中使用另一个getter，即：传入第二个参数getters，以便获取其他的getter

示例如下：

```js
getters: {
  totalPrice(total, getters) {
    let totalPrice = 0
    for ( const book of state.books) {
      totalPrice += book.count * book.price
    }
    return totalPrice + ', ' + getters.myName
  },
  myName(state) {
    return state.name
  }
}
```

**getters的返回函数**

getters中的<font color=FF0000>函数本身，可以返回一个函数，那么在使用的地方相当于可以调用这个函数</font>。

```js
getters: {
  totalPrice(state) {
    return (price) => {
      let totalPrice = 0
      for ( const book of state.books) {
        if (book.price < price) continue
        totalPrice += book.count * book.price
      }
      
      return totalPrice
    }
  }
}
```

**mapGetters辅助函数**

可以使用mapGetters的辅助函数

```js
computed: {
  ...mapGetters(['totalPrice', 'myName']),
  // 起别名
  ...mapGetters({
    finalPrice: 'totalPrice',
    finalName: 'myName'
  })
}
```

**在setup中使用mapGetters**

Vuex并没有提供非常方便的使用mapGetters的方式，还是要进行一个函数的封装。代码思路和 mapState的封装一样

```js
import { useStore, mapGetters } from 'vuex'
import { computed } from 'vue'

export function useGetters(mapper) {
  const store = useStore()
  
  cosnt storeFns = mapGetters(mapper)
  
  const state = {}
  Object.keys(stateFns).forEach(fnKey => {
    state[fnKey] = computed(stateFns[fnKey].bind({$store: store}))
  })
  
  return state
}
```



**Mutation**

<font color=FF0000>更改 Vuex 的 store 中的状态的唯一方法是提交 ( commit ) mutation</font>

```js
mutation: {
  increment(state) {
    state.counter ++
  },
  decrement(state) {
    state.counter --
  }
}
```

**Mutation携带数据**

很多时候我们在提交mutation的时候，会携带一些数据，这个时候我们可以使用参数 payload：

```js
mutation: {
  increment(state, payload) {
    state.counter += payload
  },
}
```

**payload为对象类型**

```js
addNumber(state, payload) {
  state.counter += payload.count
}
```

**对象风格的提交方式**

```js
$route.commit({
  type: 'addNumber',
  count: 100
})
```

**Mutation常量类型**

防止开发时发生低级错误（拼写错误），所以定义为常量。

- **定义常量**

  ```js
  export const ADD_NUMBER = 'ADD_NUMBER'
  ```

- **定义mutation**

  ```js
  [ADD_NUMBER](state, payload) {
    state.counter += payload.count
  }
  ```

- **提交mutation**

  ```js
  $store.commit({
    type: ADD_NUMBER,
    count: 100
  })
  ```

**mapMutations辅助函数**

```js
methods: {
  ...mapMutation({
    addNumber: ADD_NUMBER,
  }),
  ...mapMutation(['increment', 'decrement'])
}
```

**在setup中使用mapMutations**

Setup中使用，和options api中使用没有差异；因为事件触发的就是函数，本身就是函数没有问题。

```JS
const mutations = mapMutation(['increment', 'decrement']);
const mutations2 = mapMutation({
    addNumber: ADD_NUMBER,
})
```

**mutation的重要原则**

<font color=FF0000>**mutation 必须是同步函数：**</font>这是因为 devtool 工具会记录 mutation。<font color=FF0000>每一条mutation被记录，devtools都需要捕捉到前一状态和后一状态的快照。但是在mutation中执行异步操作，就无法追踪到数据的变化；所以Vuex的重要原则中要求 mutation必须是同步函数</font>



**Actions**

**Action类似于mutation，不同在于**：<font color=FF0000>Action提交的是mutation，而不是直接变更状态；Action可以包含任意异步操作</font>

```js
mutations: {
  increment(state) {
    state.counter ++
  }
},
actions: {
  increment(context){
    context.commit('increment')
  }
}
```

上面的Actions中有一个参数context：<font color=FF0000>**context是一个和store实例均有相同方法和属性的context对象，所以可以从context 获取到commit方法来提交一个mutation，或者通过 context.state 和 context.getters 来 获取 state 和 getters**</font>。

因为模块 ( module ) 的原因，使得 context不是store对象。而最外层（不分模块）的context 就是store，而modules 中的 context不是store；这就是为什么context不是store对象的原因，因为两者不完全等价。

另外，**打印context中的内容，发现其有如下属性：**
- **commit：**提交一个mutation
- **dispatch：**在一个action中调用另一个action 
- **getters：**获取数据，使用getter
- **rootGetters：**只有在使用module时才会用到，在一个模块中获取rootGetter
- **rootState：**只有在使用module时才会用到，在一个模块中获取rootState
- **state**

**actions的分发 ( dispatch ) 操作**

分发使用的是 store 上的dispatch函数：

```js
add() {
  this.$route.dispatch('increment')
}
```

dispatch可以携带参数：

```js
add() {
  this.$route.dispatch('increment', {count: 100} )
}
```

也可以以对象的形式进行分发，类似于mutation

```js
add() {
  this.$store.dispatch({
    type: 'increment',
    count: 100
  })
}
```

**Actions的辅助函数 mapActions**

action也有对应的辅助函数：对象类型写法 和 数组类型的写法

```js
methods: {
  ...mapActions(['increment', 'decrement'])
  ...mapActions({
    add: 'increment',
    sub: 'decrement'
  })
}
```

**Actions的异步操作**

Action 通常是异步的，<font color=FF0000>如何知道 Action 什么时候结束</font>？<font color=FF0000>可以**通过让 Action 返回 Promise（示例如下）**，在Promise的then中来处理完成后的操作；如果没有返回值，则默认返回undefined</font>

```js
// 定义
actions: {
  increment(context) {
    return new Promise((resolve) => {
      setTimeout(() => {
        context.commit('increment')
        resolve('异步完成')
      }, 1000)
    })
  }
}
```

在组件中使用

```js
const store = useStore()
const increment = () => {
  store.dispatch('increment').then(res => {
    console.log(res, '异步完成')
  })
}
```



**module**

**什么是Module？**由于使用单一状态树，应用的所有状态会集中到一个比较大的对象，<mark>当应用变得非常复杂时，store 对象就有可 能变得相当臃肿</mark>；为了解决以上问题，<font color=FF0000>**Vuex 允许我们将 store 分割成模块( module )。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块**</font>。<font color=FF0000>另外，一个模块就是一个对象</font>

示例如下：定义模块（建议在一个模块放在一个文件中？）

```js
// modules/moduleA.js
const moduleA = {
  state: () => ({}),
  mutation: {},
  actions: {},
  getters: {}
}

// modules/moduleB.js
const moduleB = {
  state: () => ({}),
  mutation: {},
  actions: {},
  getters: {}
}
```

注册模块

```js
const store = createStore({
	modules: {
		a: moduleA,
		b: moduleB
	}
})

store.state.a // moduleA 中的state
store.state.b // moduleA 中的state
```

**module的局部状态**

<font color=FF0000>对于**模块内部的 mutation 和 getter**，**接收的第一个参数是模块的局部状态对象 ( state )**</font>：

```js
mutations: {
  changeName(state) {
    state.name = 'foo'
  }
}
getters: {
  info(state, getters, rootState) {
    return `name:${state.name} age:${state.age} height:${state.height}`
  }
}
actions: {
  changeNameAction( {state, commit, rootState} ) {
    commit('changeName', 'kobe')
  }
}
```

**module的命名空间**

<font color=FF0000>默认情况下，模块内部的action和mutation仍然是注册在全局的命名空间中的</font>。**这样使得多个模块能够对同一个 action 或 mutation 作出响应，Getter 同样也默认注册在全局命名空间**。

**如果希望模块具有更高的封装度和复用性**，<font color=FF0000>可以添加 namespaced: true 的方式使其成为带命名空间的模块</font>；<font color=FF0000>**当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名**</font>

```js
// user module
namespaced: true
// 访问：通过$route.state.user.xxx 获取数据；要加上user
state() {
  return {
    name: 'foo',
    age: 18,
    height: 1.88
  }
},
mutations: {
  // 访问：$store.commit('user/changeName')
  changeName(state) {
    state.name = 'foo'
  }
},
getters: {
  // 访问：$store.getters['/user/info']，会有四个参数
  // 对于模块内部的getter：根节点状态rootState会作为第三个参数暴露出来，根结点getters( rootGetters )作为第四个参数
  info(state, getters, rootState, rootGetters) {
    return `name:${state.name} age:${state.age} height: ${state.height}`
  }
},
actions: {
  // 一共有六个参数
  changeNameAction({ commit, dipatch, state, rootState, getters, rootGetters }) {
    commit('changeName', 'kobe')
  }
}
```

**module修改或派发根组件**

如果希望在action中修改root中的state：

> 若需要在全局命名空间内分发 action 或提交 mutation，将 { root: true } 作为第三参数传给 dispatch 或 commit 即可
>
>  																																													-- 官方文档

```js
actions: {
  changeNameAction({ commit, dipatch, state, rootState, getters, rootGetters }) {
    commit('changeName', 'kobe')
    
    // 第二个参数是 payload？
    commit('changeNameAction', null, {root: true})
    dispatch('changeRootNameAction', null, {root: true})
  }
}
```

**module的辅助函数 mapXXXX**

**辅助函数有三种使用方法：**

1. <font color=FF0000>通过**完整的模块空间名称**来查找</font>

   ```js
   computed: {
     ...mapState({
       a: state => state.some.nested.module.a,
       b: state => state.some.nested.module.b
     })
   },
   methods: {
     ...mapActions([
       // 传入路径，和上面的namespace: true 中的访问类似
       'some/nested/module/foo', // -> this['some/nested/module/foo']()
       'some/nested/module/bar' // -> this['some/nested/module/bar']()
     ])
   }
   ```

2. 第一个参数传入模块空间名称，后面写上要使用的属性

   ```js
   computed: {
     // 第一个参数传入模块空间名称
     ...mapState('some/nested/module', {
       a: state => state.a,
       b: state => state.b
     })
   },
   methods: {
     ...mapActions('some/nested/module', [
       'foo', // -> this.foo()
       'bar' // -> this.bar()
     ])
   }
   ```

3. 通过 createNamespacedHelpers 生成一个模块的辅助函数。<font color=FF0000>相当于内部创建了一个mapState、mapActions</font>。另外，<font color=FF0000>这种更常用</font>

   ```js
   import { createNamespacedHelpers } from 'vuex'
   
   const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')
   
   export default {
     computed: {
       // 在 `some/nested/module` 中查找
       ...mapState({
         a: state => state.a,
         b: state => state.b
       })
     },
     methods: {
       // 在 `some/nested/module` 中查找
       ...mapActions([
         'foo',
         'bar'
       ])
     }
   }
   ```



#### TypeScript

##### TS 解决 JS 的痛点

**编程开发中我们有一个共识：错误出现的越早越好**

- 能在写代码的时候发现错误，就不要在代码编译时再发现（IDE的优势就是在代码编写过程中帮助我们发现错误）。

- 能在代码编译期间发现错误，就不要在代码运行期间再发现（类型检测就可以很好的帮助我们做到这一点）。 

- 能在开发阶段发现错误，就不要在测试期间发现错误，能在测试期间发现错误，就不要在上线后发现错误。

而JS在代码编译期间也不是很好的发现代码中的错误：很大的原因就是因为JavaScript没有对我们传入的参数进行任何的限制，只能等到运行期间才发现这个错误；并且当这个错误产生时，会影响后续代码的继续执行，也就是整个项目都因为一个小小的错误而深入崩溃



##### TS 介绍

TypeScript是拥有类型的JavaScript超集，它可以编译成普通、干净、完整的JavaScript代码。

- JS 所拥有的特性，TS全部都是支持的，并且它紧随ECMAScript的标准，所以ES6、ES7、ES8等新语法标准，都是支持的
- 在语言层面上，TS不仅增加了类型约束，而且包括一些语法的扩展，比如枚举类型（Enum）、元组类型（Tuple）等
- TypeScript在实现新特性的同时，总是保持和ES标准的同步甚至是领先
- TypeScript最终会被编译成JavaScript代码，所以你并不需要担心它的兼容性问题，在编译时也不需要借助于Babel这样的工具



##### TS 编译环境

TS最终会被编译成JS来运行；需要在电脑上安装TS，这样就可以通过 TypeScript的Compiler（即：tsc，安装TS自带）将其编译成JS

##### TS 运行环境

在不使用其他工具的话，为了查看 TypeScript 代码的运行效果，都需要通过tsc编译TS为JS，并在浏览器或Node环境运行；这太麻烦了

**如何简化该操作？**有两种解决方案

- **通过 webpack，配置本地的 TypeScript 编译环境和开启一个本地服务，可以直接运行在浏览器上**

  <font color=FF0000>**TS 项目中配置 webpack，需要使用 ts-loader；另外，还需要添加 `tsconfig.json` 配置文件；可以使用 `tsc --init` 命令以自动生成一个初始配置。另外，在 `resolve.extensions` 中至少加上 `['ts', 'js']`**</font> 
  
  > 💡 `tsc --init` 将创建一个 `tsconfig.json` 文件


- **通过ts-node库，为TypeScript的运行提供执行环境**

  除了安装 ts-node 外，还需要安装 依赖 tslib 和 `@types/node`

> 👀 还有第三种方式：[deno](https://github.com/denoland/deno) 提供了 TS 运行环境的支持；可以使用 `deno run target.ts` 命令 运行 ts 文件。

> 💡 补充：在 vue-cli 中会让用户选择是使用 babel 编译 TS，还是使用 tsc ；这里推荐 babel（官方文档中也推荐 babel  ），因为 babel 会打上 polyfill



##### 变量声明

```typescript
[var | let | const] 标识符 : 数据类型 = 赋值
```

声明了类型后TypeScript就会进行类型检测，声明的类型可以称之为<font color=FF0000>类型注解</font>。在tslint中并不推荐使用var来声明变量

示例：

```typescript
let message: string = 'hello world'
```

注意：这里的string是小写的s，也有大写的String。string是TypeScript中定义的字符串类型，String是ECMAScript中定义的一个类



##### 变量的类型推导（推断）

在开发中，有时候为了方便起见我们并不会在声明每一个变量时都写上对应的数据类型，我们更希望可以通过TypeScript本身的特性帮助我们推断出对应的变量类型，比如：let message = 'hello world'，TS会自动推断出是string类型。

这是因为在一个变量第一次赋值时，会根据后面的赋值内容的类型，来推断出变量的类型。



##### TS 中的数据类型

因为TS是JS的超集，所以JS中的数据类型，TS都有。

###### number类型

TypeScript和JavaScript一样，不区分整数类型（int）和浮点型 （double），统一为number类型

另外，ES6有二进制、八进制、十六进制的表示方法；TS同样有。

```typescript
num = 100
num = 0b100
num = 0o100
num = 0x100
```

###### boolean类型

只有两个取值：true和false

```typescript
let flag: boolean = true
```

###### string 类型

```typescript
let msg: string = 'hello world'
```

另外，TS同样支持ES6的模版字符串

###### Array 类型

数组类型的定义也非常简单，有两种方式

- ```typescript
  const names: string[] = ['foo', 'bar', 'baz']
  ```

- ```typescript
  const names: Array<string> = ['foo', 'bar', 'baz']
  ```

  不推荐第二种，因为在React的jsx（\<el>\</el>）是有冲突的

**另外，TS中的Array 类型 只能放入定义时定义的那种类型的数据**

###### Object 类型

```typescript
const myInfo: object = {
  name: 'foo',
  age: 18,
  height: 1.88
}
```

但是从myinfo中我们不能获取数据，也不能设置数据

```typescript
myInfo['name'] = 'foobar' // 无法设置数据，会报错
console.log(myInfo['age']) // 无法获取数据，会报错
```

另外，这样写并不推荐；<font color=FF0000>更推荐自动的类型推导</font>。因为下面的name拿不到（编译通不过）。虽然，也可以通过类型断言解决这个问题

###### Symbol 类型

略

###### null & undefined 类型

> 👀 TS 特有

在 JavaScript 中，undefined 和 null 是两个基本数据类型。 <font color=FF0000 size=4>在TypeScript中，它们各自的类型也是undefined和null，也就意味着它们既是实际的值，也是自己的类型</font>

```typescript
let n: null = null
let u: undefined = undefined
```

<font color=FF0000 size=4>**如果写上类型注解null，则可选值只有null；如果不加类型注解，类型推导的值会是any**</font>。另外，在非“严格检查模式”下，可以将null 赋值给string类型之类的变量

###### any 类型

在某些情况下，确实无法确定一个变量的类型，并且可能它会发生一些变化，这时可以使用any类型（类似 于Dart语言中的dynamic类型）。

**any 类型有点像一种讨巧的TypeScript手段：**

- 我们可以对any类型的变量进行任何的操作，包括获取不存在的属性、方法
- 我们给一个any类型的变量赋值任何的值，比如数字、字符串的值

###### unknown类型

unknown是TypeScript中比较特殊的一种类型，它用于描述类型不确定的变量

unknown类型只能赋值给 any / unknown类型；而any类型可以赋值给任意类型。一个变量，如果既可以使用unknown，也可以使用any，则推荐使用unknown

使用示例：

```typescript
function foo(): string { return 'foo' }
function bar(): number { return 123 }

const flag = true
let resulet: unknown

if( flag ) { result = foo() }
else { result = bar() }

export {}
```

默认情况下，所有的ts 文件 都会在统一作用域下进行编译，所以如果一个项目中出现两个name变量的定义，将会冲突 并报错；这<font color=FF0000 size=4>**需要把每个ts文件看作不同的作用域，需要使用export {}，将其作为一个模块**</font>。另外，还有其他的方法解决这个问题。

###### void 类型

void通常用来指定一个函数是没有返回值的，那么它的返回值就是void类型

<font color=red>可以将 null 和 undefined 赋值给 void 类型</font>，也就是函数可以返回 null 或者 undefined

```typescript
function sum(num1: number, num2: number): void {
  console.log( num1 + num2 )
}
```

###### never 类型

never 表示永远不会发生值的类型，比如一个函数：

<font color=FF0000>**如果一个函数中是一个死循环或者抛出一个异常，那么这个函数不会返回东西；那么写void类型或者其他类型作为返回值类型都不合适，我们就可以使用never类型**</font>。

##### tuple 类型 

tuple是元组类型。tuple和数组的区别：

- 数组中通常建议存放相同类型的元素，不同类型的元素是不推荐放在数组中（可以放在对象或者元组 中）
- <font color=FF0000>元组中每个元素都有自己特性的类型，根据索引值获取到的值可以确定对应的类型</font>

示例如下：

```typescript
const info: (string|number)[] = ['foo', 18, 1.88]
const item1 = info[0] // 不能确定类型

const tupleInfo: [ string, number, number ] = ['foo', 18, 1.88]
const item2 = tupleInfo[0] // 一定是string类型
```

**Tuple的使用场景：**通常可以作为返回的值，在使用的时候会非常的方便

```typescript
function useState<T>(state: T): [T, (newState: T) => void] {
  let curState = state
  const change = (newState: T) => {
    curState = newState
  }
  
  return [curState, changeState]
}

const [counter, setCounter] = useState(10)
```

###### 函数的参数类型

函数是JavaScript非常重要的组成部分，<font color=FF0000>TypeScript允许我们指定函数的参数和返回值的类型</font>

- **参数的类型注解：**声明函数时，可以在每个参数后添加类型注解，以声明函数接受的参数类型

  ```typescript
  function greet(name: string) {
    console.log('hello ' + name.toUpperCase())
  }
  ```

- **函数的返回值类型：**可以添加返回值的类型注解，这个注解出现在函数列表的后面

  ```typescript
  function sum(num1: number, num2: number): number {
    return num1 + num2
  }
  ```

  和变量的类型注解一样，<font color=FF0000>通常情况下不需要返回类型注解，因为TypeScript会根据 return 返回值推断函数的 返回类型</font>

- **匿名函数的参数：**当一个函数出现在TypeScript可以确定该函数会被如何调用的地方时，该函数的参数会自动指定类型。

  ```ts
  const names = ['foo', 'bar', 'baz']
  names.forEach(item => {
    console.log(item.toUpperCase())
  })
  ```

  上面的item的类型不需要指定，<font color=FF0000>这是因为TypeScript会根据forEach函数的类型以及数组的类型推断出item的类型</font>。<font color=FF0000 size=4>这个过程称之为**上下文类型** ( contextual typing )，**因为函数执行的上下文可以帮助确定参数和返回值的类型**</font>

  即：上下文中的函数可以不添加类型注解，当然添加也可以。

###### 对象类型

> 👀 TS 特有

限定一个函数接受的参数是一个对象：

```ts
function printCoordinate(point: {x: number, y: number}) { ... }

printCoordinate({x: 100, y: 30})
```

上面使用了一个对象来作为类型：在对象可以添加属性，并且告知TypeScript 该属性需要是什么类型；属性之间可以使用 `, `或者 `;` 来分割，最后一个分隔符是可选的。<font color=FF0000>每个属性的类型部分也是可选的，如果不指定，那么就是any类型</font>

###### 可选类型

对象类型也可以指定哪些属性是可选的，可以在属性的后面添加一个`?`

```ts
function printCoordinate(point: {x: number, y: number, z?: number}) { ... }

printCoordinate({x: 100, y: 30})
printCoordinate({x: 100, y: 30, z: 40})
```

另外：<font color=FF0000>**可选类型可以看做是 类型 和 undefined 的联合类型**</font>

```ts
// 等价于 string | undefined
function print(message?: string) { console.log(message) }

// 如下都不会报错
print()
print('foo')
print(undefined)

// 会报错，因为不是 string 和 undefined 的联合了
print(null)
```

###### 联合类型

> 👀 TS 特有

联合类型是由两个或者多个其他类型组成的类型，表示指定的类型可以是这些类型中的任何一个值。联合类型中的每一个类型被称之为联合成员

```ts
function printId(id: number|string) { console.log(id)}

printId(10)
printId('10')
```

**使用联合类型**

传入给一个联合类型的值是非常简单的：只要保证是联合类型中的某一个类型的值即可。但是拿到这个值之后，应该如何使用它呢？因为它可能是任何一种类型。 比如拿到的值可能是 string或者number，就不能对其调用string上的一些方法。如何处理？

<font color=FF0000>**这就需要使用缩小（narrow）联合，TypeScript可以根据我们缩小的代码结构，推断出更加具体的类型**</font>：

```ts
function printId(id: number|string) {
  if (typeof id === 'string') { console.log(id.toUpperCase()) }
  else { console.log(id) }
}
```

###### 字面量类型

> 👀 TS 特有

literal types。<font color=FF0000>字面量类型必须类型和值一样</font>。

```ts
let msg: 'hello world' = 'hello world'

// 这时会报错，因为字面量类型必须类型和值一样
msg = 'foobar'
```

如下代码，<font color=FF0000>**因为使用了const，且没有指定类型注解，所以类型推断的结果会是字面量类型**</font>

```ts
const message = 'hello world'
// 相当于
const message: 'hello world' = 'hello world'
```

字面量类型也可以是数组，比如：

```ts
let num: 123 = 123
```

默认情况下字面量类型是没有太大的意义的，但是可以将其用在 将多个类型联合在一起

```ts
type Alignment = 'left' | 'right' | 'center'
function changeAlign(align: Alignment) {
  console.log('方向：', align)
}

changeAlign('left')
```

这里的用法类似于枚举

**字面量推理**

> 👀 // TODO 没看懂...

###### 函数类型

使用函数的过程中，函数也可以有自己的类型。可以编写函数类型的表达式 ( Function Type Expressions ) ，来表示函数类型

```ts
type calcFunc = (num1: number, num2: number) => void
function calc(fn: calcFunc, num1: number, num2: number) { console.log( fn(num1, num2) ) }

function sum(num1: number, num2: number) { return num1 + num2 }
function mul(num1: number, num2: number) { return num1 * num2 }

calc(sum, 10, 20)
calc(mul, 30, 40)
```

上面的 `(num1: number, num2: number) => void` ，代表的就是一个函数类型。接收两个参数的函数：num1和num2，并且都是number类型；并且这个函数是没有返回值的，所以是void

- **参数的可选类型**

  ```ts
  function foo(x: number, y?: number) {
    console.log(x, y)
  }
  ```

  可选类型需要在必传参数的后面，否则将会报错。

- **默认参数**

  从ES6开始，JavaScript是支持默认参数的，TypeScript也是支持默认参数的。

  ```ts
  function foo(x: number, y; number = 6) {
    console.log(x, y)
  }
  foo(10)
  foo(10, 20)
  ```

  <font color=FF0000>这时y的类型其实是 undefined 和 number 类型的联合</font>

  <font color=FF0000>默认参数的位置没有限制，可以放在最前面</font>。<font size=4>**但一般推荐的参数使用顺序是：必传参数 - 有默认值的参数 - 可选参数**</font>

- **剩余参数**

  从ES6开始，JavaScript也支持剩余参数，剩余参数语法允许我们将一个不定数量的参数放到一个数组中。另外，<font color=FF0000>剩余参数前面也可以加上其他参数，但是剩余参数后面不能跟参数</font>

  ```ts
  function sum(...nums: number[]) {
    let total = 0
    for (const num of nums) {
      total += num
    }
    return total
  }
  
  sum(10, 20, 30)
  sum(10, 20, 30, 40)
  ```

###### 枚举类型

> 👀 TS 特有

枚举就是将一组可能出现的值，一一列举出来，定义在一个类型中，这个类型就是枚举类型。枚举允许开发者定义一组命名常量，常量可以是数字、字符串类型。

```ts
enum Direction {
  LEFT,
  RIGHT,
  TOP,
  BOTTOM
}

function turnDir(dir: Direction) {
  switch(dir) {
    case Direction.LEFT ... break;
    case Direction.RIGHT ... break;
    case Direction.TOP ... break;
    case Direction.BOTTOM ... break;
    default: const myDir: never = dir
  }
}
```

**枚举类型的值**

枚举类型默认是有值的，默认值 是从零开始逐次加一。也可以手动赋值

- 数字类型，这时会从100进行递增

  ```ts
  enum Direction {
    LEFT = 100,
    RIGHT,
    TOP,
    BOTTOM
  }
  ```

- 其他类型：

  ```ts
  enum Direction {
    LEFT,
    RIGHT,
    TOP = 'TOP',
    BOTTOM = 'BOTTOM'
  }
  ```

###### 泛型

参数的类型也可以参数化，程序员可以选择传递参数的类型。这就<font color=FF0000>需要使用一种特性的变量：**类型变量** ( type variable )，它**作用于类型**，而不是值</font>。示例如下：

```ts
// 这里的<Type> 可以理解为定义Type
function foo<Type>(arg: Type): Type {
  return arg
}
```

**有两种方式来调用它：**

1. 通过 <类型> 的方式将类型传递给函数

   ```ts
   foo<string>('abc')
   foo<number>(123)
   ```

2. **通过类型推导，自动推导出传入变量的类型**

   ```ts
   foo('abc')
   foo(123)
   ```

**泛型可以传入多个类型：**

```ts
function foo<T, E>(a1: T, a2: E) { ... }
```

平时在开发中我们可能会看到一些常用的名称：

- **T：**Type的缩写，类型 
- **K、V：**key和value的缩写，键值对
- **E：**Element的缩写，元素
- **O：**Object的缩写，对象

**泛型接口：**在定义接口的时候我们也可以使用泛型

```ts
interface IFoo<T> {
  initialValue: T,
  valueList: T[],
  handleValue: (value: T) => void
}

const foo: IFoo<number> = {
  initialValue: 0,
  valueList: [0, 1, 3],
  handleValue: function(value: number) { ... }
}
```

另外，定义接口的时候也是可以给泛型加上默认值的：

```ts
interface IFoo<T = number> {
  initialValue: T,
  valueList: T[],
  handleValue: (value: T) => void
}
```

**泛型类：**

```ts
class Point<T> {
  x: T
  y: T
  
  constructor(x: T, y: T) {
    this.x = x
    this.y = y
  }
}

const p1 = new Point(10, 20)
const p2 = new Point<number>(10, 20)
const p3: Point<number> = new Point(10, 20)
```

**泛型约束：**我们希望传入的类型有某些共性，但<font color=FF0000>这些共性可能不是在同一种类型中</font>

比如string和array都是有length的，或者某些对象也是会有length属性的；那么<font color=FF0000>只要是拥有length的属性都可以作为我们的参数类型</font>，那么该如何操作？

```ts
// 定义一个约束接口
interface ILength {
  length: number
}

function getLength<T extends ILength>(args: T) {
  return args.length
}

getLength('foo')
getLength(['foo', 'bar', 'baz'])
getLength({length: 100, name: 'foo'})
```



##### 类型别名

可以给对象类型起一个别名，避免在类型注解中重复编写 对象类型 和 联合类型，过于麻烦。

```ts
type ID = number | string

function printId(id: ID) { console.log(id) }
```



##### 类型断言

有时TypeScript无法获取具体的类型信息，这时就需要使用类型断言 (Type Assertions)

比如通过 `document.getElementById()`，TypeScript只知道该函数会返回 HTMLElement ，但并不知道它 具体的类型。

```ts
const myEl = document.getElementById('my-img') as HTMLImageElement
myEl.src = '...'
```

TypeScript 只允许类型断言转换为 更具体（父类转化为子类） 或者 不太具体（即：any / unknown） 的类型版本，此规则可防止不可能的强制转换



##### 非空类型断言

```ts
function printMsg(msg?: string) {
  // 因为这里ts不确定，用户是否会传msg，所以会报错
  console.log(msg.toUpperCase())
}

printMsg('hello')
```

另外，这段代码在VSC上（如果没有tsconfig.json）是不报错的（有tsconfig.json会报错），但是在ts-node中运行，就会报错；因为 ts-node运行时，会使用内置的tsconfig.json文件

**但是，如果确定传入的参数是有值的，这时可以使用非空类型断言：** 非空断言使用的是 `!` ，表示可以确定某个标识符是有值的，跳过ts在编译阶段对它的检测

```ts
function printMsg(msg?: string) {
  // 👀 这里加上了 `!`
  console.log(msg!.toUpperCase())
}
```

补充：关于断言 可以看下：[TypeScript 断言总结](https://juejin.cn/post/6981112029100802085)



**可选链**

可选链是ES11 ( ES2020 ) 中增加的特性，操作符是：`?.`。它的作用是<font color=FF0000>当对象的属性不存在时，会短路直接返回undefined</font>，如果存在，那么才会继续执行。

补充：**空值合并操作符 ( ?? )** 是一个逻辑操作符，当操作符的左侧是 null 或者 undefined 时，返回其右侧操作数， 否则返回左侧操作数



**类型缩小**

- 类型缩小的英文是 Type Narrowing

- 可以通过类似于 typeof padding === 'number' 的判断语句，来改变TS的执行路径。

- 在给定的执行路径中，我们可以缩小比声明时更小的类型，这个过程称之为 缩小。
  - typeof padding === 'number' 可以称之为 类型保护 ( type guards )

##### 常见的类型保护有

- **typeof：**检查返回的值typeof是一种类型保护：因为 TypeScript 对如何typeof操作不同的值进行编码

  ```ts
  type ID = number | string
  
  function printId(id: ID) {
    if(typeof id === 'string') { ... }
    else { ... }
  }
  ```

- 平等缩小（比如 `===`、`!==`；还有 `=` 、`==`、`!=`、`switch`）

  ```ts
  type Direction = 'left' | 'right' | 'center'
  function turnDir(direction: Direction) {
    switch(direction) {
      case 'left': console.log(...) break
      case 'right': console.log(...) break
      case 'center': console.log(...) break
      default: console.log(...)
    }
  }
  ```

- **instanceof：**检查一个值是否是另一个值的“实例”

  ```ts
  function printVal(date: Date | string) {
    if(date instanceof Date) {
      console.log(date.toLocalString())
    } else {
      console.log(date)
    }
  }
  ```

- **in：**用于确定对象是否具有带名称的属性。<font color=FF0000>如果指定的属性在指定的对象或其原型链中，则in 运算符返回true</font>

  ```ts
  type Fish = { swim: () => void }
  type Dog = {run: () => void }
  
  function move(animal: Fish | Dog) {
    if('swim' in animal) {
      animal.swin()
    } else {
      animal.run()
    }
  }
  ```

- 等等



##### TS 中的 this

```ts
const info = {
  name: 'foo',
  sayHello() {
    console.log(this.name)
  }
}
info.sayHello()
```

上面的代码是可以正常运行的，也就是TypeScript在编译时，认为我们的this是可以正确去使用的。TypeScript认为函数 sayHello 有一个对应的this的外部对象 info，所以在使用时，就会把this当做该对象。

但是对于如下情况，ts不知道this是什么。如下：

```ts
function sayHello() {
  console.log(this.name)
}
const info = {
  name: 'foo',
  sayHello
}
info.sayHello()
```

这段代码运行会报错。TypeScript进行类型检测的目的是让我们的代码更加的安全，所以这里对于 sayHello 的调用来说，我们虽然将其放到了info中，通过info去调用，this依然是指向info对象的；但对于TypeScript编译器来说，这个代码是非常不安全的，因为我们也有可能直接调用函数，或者通过别的对象来 调用函数。

###### 指定this的类型

对上面的代码进行修补：

```tsx
type NameType = {
	name: string
}
function sayHello(this: NameType) {
	console.log(this.name)
}
```



##### 函数重载

```jsx
function sum(a1: number, a2: number): number;
function sum(a1: string, a2: string): string;
function sum(a1: any, a2: any): any {
	return a1 + a2
}
```

在我们调用sum的时候，它会根据我们传入的参数类型来决定执行函数体时，到底执行哪一个函数的重载签名。



##### 联合类型和重载

联合和重载是两者非常相似，都可以实现类似的功能。在开发中我们选择使用哪一种呢？ 在可能的情况下，尽量选择使用联合类型来实现。

个人思考：如果考虑代码的可修改性，感觉还是使用重载更好一点啊？



##### 类 class

在早期的JavaScript开发中（ES5）我们需要通过函数和原型链来实现类和继承，从ES6开始，引入了class关键字，可以 更加方便的定义和使用类。TypeScript作为JavaScript的超集，也是支持使用class关键字的，并且还可以对类的属性和方法等进行静态类型检测。

**实际上在JavaScript的开发过程中，我们更加习惯于函数式编程：**

- 比如React开发中，目前更多使用的函数组件以及结合Hook的开发模式
- 比如在Vue3开发中，目前也更加推崇使用 Composition API

但是在封装某些业务的时候，类具有更强大封装性，所以我们也需要掌握它们。



##### 类的定义

可以声明一些类的属性，在类的内部声明类的属性以及对应的类型。

- 如果类型没有声明，那么它们默认是any的
- 可以给属性设置初始化值
- 在默认的strictPropertyInitialization模式下面我们的属性是必须初始化的，如果没有初始化，那么编译时就会报错。如果我们在strictPropertyInitialization模式下确实不希望给属性初始化，可以使用 name!: string语法



##### 类的继承

使用 extends 关键字来实现继承，子类中使用 super 来访问父类；使用 super() 调用父类的构造方法，使用 super.method() 调用父类的方法



##### 类的成员修饰符

在TypeScript中，类的属性和方法支持三种修饰符： public、private、protected。默认的修饰符是public



**只读属性readonly**

如果有一个属性我们不希望外界可以任意的修改，只希望确定值后直接使用，那么可以使用readonly。

<mark>只读属性是可以在constructor中赋值的，也推荐在constructor中赋值；但是赋值之后，就不能再修改了</mark>

另外，<font color=FF0000>readonly属性本身不能进行修改，但如果它是对象类型，对象中的属性是可以修改</font>

```ts
class Person {
  readonly name: string
  
  constructor(name: string) {
    this.name = name
  }
}

// 这里会报错，因为对readonly赋值了
const p = new Person('why')
console.log(p.name)

export {}
```



**getters / setters**

私有属性是无法直接访问的，或者某些属性我们想要监听它的获取(getter)和设置(setter)的过程；这时候就要用到getter / setter

```typescript
class Person {
  // 私有属性，建议以_name，借鉴的py？
  private _name: string
  
  set name(newName) {
    this._name = newName
  }
  get name() {
    return this._name
  }
  
  constructor(name: string) {
    this.name = name
  }
}
```



**静态成员**

在类中定义的成员和方法都属于对象级别的，有时也需要定义类级别的成员和方法。在TypeScript中通过关键字static来定义静态成员。

```ts
class Student {
  static time: string = '20:00'
  
  static attendClass() { ... }
}

// 注意：这里是大写的Student
console.log(Student.time)
Student.attendClass()
```



**抽象类abstract**

继承是多态使用的前提，所以在定义很多通用的调用接口时，通常会让调用者传入父类，通过多态来实现更加灵活的调用方式。 但是，<font color=FF0000>父类本身可能并不需要对某些方法进行具体的实现，所以父类中定义的方法，我们可以定义为抽象方法</font>。

**抽象方法是：在TypeScript中没有具体实现的方法（没有方法体）**

- 抽象方法必须存在于抽象类中
- <mark>抽象类是使用abstract声明的类</mark>

**抽象类有如下的特点：**

- 抽象类是不能被实例的话（也就是不能通过new创建）

- <font color=FF0000>抽象方法必须被子类实现，否则该类必须是一个抽象类</font>

```ts
abstract class Shape {
  abstract getArea(): number
}

class Circle extends Shape {
  private r: number
  contructor(r: number) {
    super()
    this.r = r
  }
  getArea() {
    return this.r * this.r * 3.14
  }
}
```



**类的类型**

类本身也是可以作为一种数据类型的。

```ts
class Person {
  name: string
  constructor(name: string) {
    this.name = name
  }
  running() { ... }
}

const p1: Person = new Person('foo')
// 不用构造函数，而是用一个对象的字面量赋值
const p2: Person = {
  name: 'foo',
  running: function() {...}
}
```



**接口的声明**

前面可以用type来声明一个对象类型：

```ts
type Point = {
  x: number
  y: number
}
```

<font color=FF0000>对象的另外一种声明方式就是通过接口来声明</font>

```ts
interface Point {
  x: number
  y: number
}
```

和类类似，接口中同样可以定义 可选属性 ( `?:` )、只读属性 ( readonly )，形式和类的定义一样。



**索引类型**

前面使用interface来定义对象类型，这个时候其中的属性名、类型、方法都是确定的，但是有时候我们会遇到不确定的情况：

```ts
interface FrontLang {
  [index: number]: string
}

const fe: frontLang = {
  1: 'html',
  2: 'css',
  3: 'js'
}
```

```ts
interface langBirth {
  [name: string]: number
}

const lang: langBirth = {
  'java': 1995,
  'js': 1996,
  'c': 1972
}
```



**函数类型**

接口也可以用来定义函数类型

```ts
interface CalcFunc {
  (num1: number, num2: number): number
}

const add: CalcFunc = (num1, num2) => {
  return num1 + num2
}

const sub: CalcFunc = (num1, num2) => {
  return num1 - num2
}
```

当然，除非特别的情况，还是推荐使用类型别名 ( type ) 来定义函数：

```ts
type CalcFunc = (num1: number, num2: number) => number
```



**接口的继承**

接口和类一样是可以进行继承的，也是使用extends关键字。并且，接口是支持多继承的（类不支持多继承）

```ts
interface Person {
  name: string
  eating: () => void
}

interface Animal {
  running: () => void
}

interface Student extends Person, Animal {
  sno: number
}

const sut: Student = {
  sno: 110,
  name: 'foo'
  eating: function() { ... },
  running: function() { ... }
}
```



**接口的实现**

接口定义后，也是可以被类实现的。如果被一个类实现，那么在之后需要传入接口的地方，都可以将这个类传入（向下转型）。

示例如下：

```ts
interface ISwin {
  swimming: () => void
}
interface IRun {
  running: () => void
}

class Person implements ISwim, IRun {
  swimming() { ... }
  running() { ... }
}
              
function swim(swimmer: ISwim) {
  swimmer.swimming()
}
              
const p = new Person()
swim(p)
```



**交叉类型**

联合类型表示定义的类型是 “多个类型中一个”，<font color=FF0000>还有另一种类型合并，就是交叉类型（Intersection Types）</font>

- 交叉类型<font color=FF0000>表示**需要满足多个类型的条件**</font>
- 交叉类型<font color=FF0000>**使用 & 符号**</font>

如下示例：

```ts
type MyType = number & string
```

表达的含义是number和string要同时满足，但是不存在同时满足一个值 既是number类型又是string类型，所以MyType其实是一个never类型。

在开发中，使用交叉类型，通常是对 对象类型进行交叉。<font color=FF0000>即：交叉类型可以用于接口的**多继承**</font>。另外，联合类型可以用于只实现多个接口中的一个接口。

**示例如下：**

```ts
interface Colorful { color: string}
interface IRun { running: () => void }

type newType = Colorful & IRun
// 字面量赋值
const obj: newType = {
  color: 'red',
  running: function() { ... }
}
```



##### interface 和 type 的区别

<font color=FF0000>interface和type都可以用来定义对象类型</font>，在开发中定义对象类型时，选择哪一个？

- 如果<font color=FF0000>定义的是非对象类型</font>，通常<font color=fuchsia>推荐使用 type</font>，比如上面示例的 Direction、Alignment、一些 Function

- 如果是定义对象类型，它们是有区别的： 

  - <font color=FF0000>**interface 可以重复的对某个接口来定义属性和方法**</font>。<mark>即：在实现接口时，需要实现所有同名的接口；也就是合并</mark>

    ```ts
    interface IPerson {
      name: string
      running: () => void
    }
    // 不会报错
    interface IPerson {
      age: number
    }
    ```

  - <font color=FF0000>**type定义的是别名，别名是不能重复的**</font>

    ```ts
    type Person {
      name: string
      running: () => void
    }
    // 会报错
    type Person {
      age: number
    }
    ```

    

##### 字面量赋值

代码示例如下：

```ts
interface IPerson {
  name: string
  eating: () => void
}

// 这里会报错，因为对象中加入了age
const p: IPerson = {
  name: 'foo',
  age: 18,
  eating: function() { ...}
}
```

修改：

```ts
interface IPerson {
  name: string
  eating: () => void
}

const p: IPerson = {
  name: 'foo',
  age: 18,
  eating: function() { ...}
}

// 添加了这行代码
// 这里直接使用对象引用做赋值时，虽然比接口多了属性，但是这里做了freshness操作，如果满足的话，就允许赋值。
const p: IPerson = obj
```

上面会报错是因为 <font color=FF0000>TypeScript在字面量直接赋值的过程中，为了进行类型推导会进行严格的类型限制</font>。 但是之后<font color=FF0000>如果将一个 变量标识符 赋值给其他的变量时，会进行freshness擦除操作</font>



##### 模块化开发

**TypeScript支持<font color=FF0000>两种方式</font>来控制我们的作用域：**

- **模块化：**每个文件可以是一个独立的模块，支持ES Module，也支持CommonJS

  ```ts
  export function add(num1: number, num2: number) {
    return num1 + num2
  }
  export function sub(num1: number, num2: number) {
    return num1 - num2
  }
  ```

- **命名空间：**通过namespace来声明一个命名空间。

  命名空间在TypeScript早期（ES6推出之前）时，称之为内部模块，主要目的是将一个模块内部再进行作用域的划分，防止一些命名 冲突的问题；当时还在使用module关键字。

  另外：<font color=FF0000 size=4>**命名空间中的内容，默认属于自身，如果不导出，在命名空间的外部将无法调用**</font>

  ```ts
  // 导出namespace
  export namespace Time {
    export function formate(time: string) {
      return '2022-02-22'
    }
  }
  export namespace Price {
    export function format(price: number) {
      return '222.22'
    }
  }
  ```



**类型查找（声明文件）**

在使用第三方库，以及使用其他类型时，ts会不知道这个对象 / 方法 /类型 是从哪里来的；这就涉及到<font color=FF0000>typescript对类型的管理和查找规则</font>

这里先说一下ts中的文件：.d.ts文件；其中d表示declare 声明的意思。

通常编写代码的 .ts 文件最终会输出 .js 文件；<font color=FF0000>还有另外一种文件 .d.ts 文件，它是用来做类型的声明(declare)</font>。 它<mark>仅仅用来做类型检测，告知typescript我们有哪些类型</mark>。

TS会在哪里查找我们的类型声明呢？

- **内置类型声明：**是typescript自带的、帮助我们内置了 JavaScript运行时的一些标准化API的声明文件；包括比如Math、Date等内置类型，也包括DOM API，比如Window、Document等。

  内置类型声明通常在我们安装typescript的环境中会带有的，在：https://github.com/microsoft/TypeScript/tree/main/lib

- **外部定义类型声明：**外部类型声明通常是我们使用一些库（比如第三方库）时，需要的一些类型声明。 

  **这些库通常有两种类型声明方式：**

  - **在自己库中进行类型声明（编写.d.ts文件），比如axios**

  - **通过社区的一个公有库DefinitelyTyped存放类型声明文件**
    - 该库的GitHub地址：https://github.com/DefinitelyTyped/DefinitelyTyped/
    - 该库查找声明安装方式的地址：https://www.typescriptlang.org/dt/search?search=
    - 比如安装react的类型声明： npm i @types/react --save-dev

- **自己定义类型声明：**什么情况下需要自己来定义声明文件？

  - 情况一：我们使用的第三方库是一个纯的JavaScript库，没有对应的声明文件；比如 lodash
  - 情况二：我们给自己的代码中声明一些类型，方便在其他地方直接进行使用

**声明变量 - 函数 -  类**

<font color=FF0000>声明文件变量不需要赋值 / 实现，只需要声明；同时 也不会被编译</font>

```ts
// 定义 .ts文件
let name = 'foo'
let age = 18
let weight = 1.88

function foo() { ... }
function bar() { ... }
function Person(name, age) {
  this.name = name
  this.age = age
}
```

```ts
// 声明 .d.ts文件
declare let name: string
declare let age: number
declare let weight: number

declare function foo(): void
declare function bar(): void

declare class Person {
  name: string
  age: number
  
  constructor(name: string, age: number)
}
```

**声明模块**

也可以声明模块，比如 lodash模块默认不能使用的情况，可以自己来声明这个模块：

```ts
declare module "lodash" {
  export function join(args: any[]): any
}
```

声明模块的语法：

```ts
declare module "moduleName" { ... }
```

在声明模块的内部，我们可以通过 export 导出对应库的类、函数等

**declare 文件**

在某些情况下，<font color=FF0000>也可以声明文件</font>：

- 比如在开发vue的过程中，默认是不识别 .vue文件的，那么就需要对其进行文件的声明
- 比如在开发中使用了 jpg 这类图片文件，默认typescript也是不支持的，也需要对其进行声明

```ts
declare module '*.vue' {
  import { DefineComponent } from 'vue'
  const component: DefineComponent
  
  export default component
}

declare module '*.jpg' {
  const src: string
  export default src
}
```

**declare 命名空间**

比如我们在index.html中直接引入了jQuery：CDN地址 https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js

- 我们可以进行命名空间的声明：

  ```ts
  declare namespace $ {
    function ajax(setting: any): void
  }
  ```

- 在main.ts中就可以使用了：

  ```ts
  $.ajax {
    url: 'https://...',
    success: (res: any) => {
      console.log(res)
    }
  }
  ```



**tsconfig.json 文件**

tsconfig.json是用于配置TypeScript编译时的配置选项：https://www.typescriptlang.org/tsconfig

```json
{
  "compilerOptions": {
    // 目标代码(ts -> js(es5/6/7))；这里用esnext，因为还是通过babel来编译；而不是tsc
    "target": "esnext",
    // 目标代码需要使用的模块化方案(commonjs require/module.exports/es module import/export)
    "module": "esnext",
    // 严格一些严格的检查(any)
    "strict": true,
    // 对jsx进行怎么样的处理
    "jsx": "preserve",
    // 辅助的导入功能
    "importHelpers": true,
    // 按照node的方式去解析模块 import "/index.node"
    "moduleResolution": "node",
    // 跳过一些库的类型检测 (axios -> 类型/ lodash -> @types/lodash / 其他的第三方)，以提升性能
    // 还有在不同的苦中会导出相同类型的东西，会冲突；是否要检测。
    // 比如：import { Person } from 'foo' / import { Person } from 'bar'
    "skipLibCheck": true,
    // 这两个是一起使用的
    // export default/module.exports = {}
    // es module 和 commonjs
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    // 要不要生成映射文件(ts -> js)
    "sourceMap": true,
    // 文件路径在解析时, 基本url
    "baseUrl": ".",
    // 指定具体要解析使用的类型
    "types": ["webpack-env"],
    // 编译阶段的路径解析(类似于webpack alias)
    "paths": {
      "@/*": ["src/*"],
      "components/*": ["src/components/*"]
    },
    // 可以指定在项目中可以使用哪里库的类型(Proxy/Window/Document)
    "lib": ["esnext", "dom", "dom.iterable", "scripthost"]
  },
  // 解析目标
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": ["node_modules"]
}
```



#### 前端工程

##### 前端项目规范

- **EditorConfig（配置文件名为 .editorconfig ）：**有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的编码风格：比如回车键所对应的ASCII，一个Tab对应几个空格。

  示例如下：

  ```yaml
  # 文件名为 .editorconfig，这里的示例是vue官方项目中的.editorconfig
  # http://editorconfig.org
  
  root = true # 表示当前文件是在项目的的根目录的
  
  [*] # 表示所有文件适用
  charset = utf-8 # 设置文件字符集为 utf-8
  indent_style = space # 缩进风格（tab | space）
  indent_size = 2 # 缩进大小
  end_of_line = lf # 控制换行类型(lf | cr | crlf)
  trim_trailing_whitespace = true # 去除行首的任意空白字符
  insert_final_newline = true # 始终在文件末尾插入一个新行
  
  [*.md] # 表示仅 md 文件适用以下规则
  max_line_length = off
  trim_trailing_whitespace = false
  ```

  另外，在VSC中是默认不会读取该文件的，需要在VSC中安装插件 EditorConfig for VS Code，才会生效

- **prettier（配置文件名为 .prettierrc）：**

  Prettier 是一款强大的代码格式化工具（默认是：在代码被保存时（Ctrl + S）生效，自动格式化代码；另外，这个比较麻烦，可以写一个npm script），支持 JavaScript、TypeScript、CSS、SCSS、Less、JSX、Angular、Vue、GraphQL、JSON、Markdown 等语言，基本上前端能用到的文件格式它都可以搞定，是当下最流行的代码格式化工具。

  1. **安装 prettier**

     ```sh
     npm install prettier -D
     ```

  2. **配置 .prettierrc 文件：**

  * **useTabs：**使用tab缩进还是空格缩进，选择false；

  * **tabWidth：**tab是空格的情况下，是几个空格，选择2个；

  * **printWidth：**当行字符的长度，推荐80，也有人喜欢100或者120；

  * **singleQuote：**使用单引号还是双引号，选择true，使用单引号；

  * **trailingComma：**在多行输入的尾逗号是否添加，设置为  `none`；

  * **semi：**语句末尾是否要加分号，默认值true，选择false表示不加；

    ```json
    {
      "useTabs": false,
      "tabWidth": 2,
      "printWidth": 80,
      "singleQuote": true,
      "trailingComma": "none",
      "semi": false
    }
    ```

  3. **创建.prettierignore忽略文件（prettier不对其生效的文件，类似于.gitignore）**

     ```
     /dist/*
     .local
     .output.js
     /node_modules/**
     
     **/*.svg
     **/*.sh
     
     /public/*
     ```

  4. **如果在 VSC 中，需要使用插件 Prettier - Code formatter**

  5. 如果嫌 “保存后prettier生效” 麻烦（毕竟需要一个一个文件去保存），可以写一个npm scripts。

     ```json
     "scripts": {
       "prettier": "prettier --write ."
     }
     ```

     这时候只需要使用如下命令，就可以批量（全局）对代码进行prettier 处理：

     ```sh
     npm run prettier
     ```

- **ESLint**

  - 如果使用VSC，则需要安装插件：ESLint

  - 解决 eslint 和prettier冲突的问题：

    安装插件：（使用Vue CLI 创建项目时，如果选择prettier，那么这两个插件会自动安装）

    ```shell
    npm i eslint-plugin-prettier eslint-config-prettier -D
    ```

  - 使用Vue CLI 创建项目时，eslintrc.js 文件中会自动添加如下配置：

    ```js
    module.exports = {
      root: true,
      env: {
        node: true,
      },
      extends: [
        "plugin:vue/vue3-essential",
        "eslint:recommended",
        "@vue/typescript/recommended",
        "@vue/prettier",
        "@vue/prettier/@typescript-eslint",
      ],
      parserOptions: {
        ecmaVersion: 2020,
      },
      rules: {
        "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
        "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",
      },
    };
    ```

    这时候，在同时使用 prettier 和 ESLint 时，还是会有冲突；所以，在extends中添加：

    ```js
    extends: [
       // ...
      'plugin:prettier/recommended'
    ]
    ```

- **git Husky 和 ESLint**

  虽然我们已经要求项目使用eslint了，但是不能保证其他开发人员提交代码之前都将eslint中的问题解决掉了：

  - 也就是我们希望保证代码仓库中的代码都是符合eslint规范的；
  - 那么我们需要在其他开发人员 执行  `git commit `  命令的时候对其进行校验，如果不符合eslint规范，那么自动通过规范进行修复；

  那么如何做到这一点呢？**可以通过Husky工具：**<font color=FF0000>husky是一个git hook工具，可以帮助我们触发git提交的各个阶段：pre-commit、commit-msg、pre-push</font>，并做一些事。

  使用husky，这里可以使用自动配置命令：

  ```shell
  npx husky-init && npm install
  ```

  这里相当于做了三件事：

  - 安装husky，并且在package.json的devDependence中添加了husky

  - 项目根目录中多了一个 .husky，并且包含 pre-commit的文件，这里存储了在git 提交前运行（拦截）的代码

  - 在npm scripts中多一个 script，使得：在拦截时自动执行的操作：

    ```json
    "prepare": "husky install"
    ```

  还需要在 修改 .husky 中的 pre-commit ，将其中的 npm test 修改为 npm run lint（lint的 npm scripts 已经被vue cli自动添加）

- **代码提交风格**

  通常我们的git commit会按照统一的风格来提交，这样可以快速定位每次提交的内容，方便之后对版本进行控制。比如 vue github上的commit内容。

  如果每次手动来编写这些是比较麻烦的事情，我们可以使用一个工具：Commitizen。Commitizen 是一个帮助我们编写规范 commit message 的工具

  1. 安装Commitizen

     ```sh
     npm install commitizen -D
     ```

  2. 安装cz-conventional-changelog，并且初始化cz-conventional-changelog。这个命令会帮助我们安装cz-conventional-changelog

     ```sh
     npx commitizen init cz-conventional-changelog --save-dev --save-exact
     ```

  3. 在 package.json中进行配置（似乎是会自动被添加）：

     ```json
     // 与devDependence同级
     "config": {
       "commitizen": {
         "path": "./node_modules/cz-conventional-changelog"
       }
     }
     ```

  4. 在提交代码时，不再是git commit；而是，运行命令 npx cz。这时会出现接连的引导：

     - 第一步：选择type，本次更新的类型

       | Type     | 作用                                                         |
       | :------- | :----------------------------------------------------------- |
       | feat     | 新增特性 (feature)                                           |
       | fix      | 修复 Bug(bug fix)                                            |
       | docs     | 修改文档 (documentation)                                     |
       | style    | 代码格式修改(white-space, formatting, missing semi colons, etc) |
       | refactor | 代码重构(refactor)                                           |
       | perf     | 改善性能 (A code change that improves performance)           |
       | test     | 测试(when adding missing tests)                              |
       | build    | 变更项目构建或外部依赖（例如 scopes: webpack、gulp、npm 等） |
       | ci       | 更改持续集成软件的配置文件和 package 中的 scripts 命令，例如 scopes: Travis, Circle 等 |
       | chore    | 变更构建流程或辅助工具(比如更改测试环境)                     |
       | revert   | 代码回退                                                     |

     - 第二步：选择本次修改的范围（作用域）

       ![image-20210723150147510](https://i.loli.net/2021/11/28/iS9wjTKWI1HzAYJ.jpg)

     - 第三步选择提交的信息

     ![image-20210723150204780](https://i.loli.net/2021/11/28/LxKGW5fNCdRBlFs.jpg)

     - 第四步提交详细的描述信息

     ![image-20210723150223287](https://i.loli.net/2021/11/28/bCRt2TOl3nwchXP.jpg)

     - 第五步是否是一次重大的更改

     ![image-20210723150322122](https://i.loli.net/2021/11/28/bg6iRr7WTzx9Mwy.jpg)

     - 第六步是否影响某个open issue

     ![image-20210723150407822](https://i.loli.net/2021/11/28/MzngbwC379iyAEN.jpg)

     由于使用npx进行提交是有点不符合习惯的，所以也可以在npm scripts中构建一个命令来执行 cz：

     ```json
     "scripts": {
       // ...
       "commit": 'cz'
     }
     ```

     但是，仅仅这样，提交者还是可以通过 git commit 进行提交，可能会提交不规范的message。这时可以通过commitlint来限制提交：

     1. 安装 @commitlint/config-conventional 和 @commitlint/cli

        ```sh
        npm i @commitlint/config-conventional @commitlint/cli -D
        ```

     2. 在根目录创建commitlint.config.js文件，配置commitlint

        ```js
        module.exports = {
          extends: ['@commitlint/config-conventional']
        }
        ```

     3. 使用husky生成commit-msg文件，验证提交信息：

        ```sh
        npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
        ```

        

**编写 vue.config.js**

```js
const path = require('path')

module.exports = {
  // 打包后的文件命名，默认为dist
  outputDir: './build',
  
  // 在configureWebpack中，可以使用webpack的配置去配置vue.config.js
  // 下面三个其实做的事情是一样的。
  // 方法一：正常键值对的配置
  // configureWebpack: {
  //   resolve: {
  //     alias: {
  //       views: '@/views'
  //     }
  //   }
  // }
  // 方法二：函数式配置
  // configureWebpack: (config) => {
  //   config.resolve.alias = {
  //     '@': path.resolve(__dirname, 'src'),
  //     views: '@/views'
  //   }
  // },
  // 方法三：使用chainWebpack配置（这个属性时vue-cli基于webpack-chain的），链式调用进行配置
  chainWebpack: (config) => {
    config.resolve.alias.set('@', path.resolve(__dirname, 'src')).set('views', '@/views')
  }
}
```



### vue-router集成

安装vue-router的最新版本：

```shell
# 注意：这里如果是vue3 必须安装vue-router@next
npm install vue-router@next
```

创建router对象：

```ts
import { createRouter, createWebHashHistory } from 'vue-router'
// 这里的type可加可不加，表示import 的是一个类型
import type { RouteRecordRaw } from 'vue-router'

// 这个RouteRecordRaw 是vue-router中定义的一个route对象的类型
const routes: RouteRecordRaw[] = [
  {
    path: '/',
    redirect: '/main'
  },
  {
    path: '/main',
    component: () => import('../views/main/main.vue')
  },
  {
    path: '/login',
    component: () => import('../views/login/login.vue')
  }
]

const router = createRouter({
  routes,
  history: createWebHashHistory()
})

export default router
```

安装router：

```ts
import router from './router'

createApp(App).use(router).mount('#app')
```

在App.vue中配置跳转：

```html
<template>
  <div id="app">
    <router-link to="/login">登录</router-link>
    <router-link to="/main">首页</router-link>
    <router-view></router-view>
  </div>
</template>
```



**vuex集成**

安装vuex：

```shell
# 同样，使用vuex@next
npm install vuex@next
```

创建store对象：

```ts
import { createStore } from 'vuex'

const store = createStore({
  state() {
    return {
      name: 'coderwhy'
    }
  }
})

export default store
```

安装store：

```ts
createApp(App).use(router).use(store).mount('#app')
```

在App.vue中使用：

```html
<h2>{{ $store.state.name }}</h2>
```



**element-plus集成**

安装element-plus

```shell
npm install element-plus
```

**全局引入：**一种引入element-plus的方式是全局引入，代表的含义是所有的组件和插件都会被自动注册：

```js
import ElementPlus from 'element-plus'
import 'element-plus/lib/theme-chalk/index.css'

import router from './router'
import store from './store'

createApp(App).use(router).use(store).use(ElementPlus).mount('#app')
```

**局部引入：**也就是在开发中用到某个组件对某个组件进行引入：

```vue
<template>
  <div id="app">
    <router-link to="/login">登录</router-link>
    <router-link to="/main">首页</router-link>
    <router-view></router-view>

    <h2>{{ $store.state.name }}</h2>

    <el-button>默认按钮</el-button>
    <el-button type="primary">主要按钮</el-button>
    <el-button type="success">成功按钮</el-button>
    <el-button type="info">信息按钮</el-button>
    <el-button type="warning">警告按钮</el-button>
    <el-button type="danger">危险按钮</el-button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

import { ElButton } from 'element-plus'

export default defineComponent({
  name: 'App',
  components: {
    ElButton
  }
})
</script>

<style lang="less">
</style>
```

但是我们会发现是没有对应的样式的，引入样式有两种方式：

- 全局引用样式（像之前做的那样）；
- 局部引用样式（通过babel的插件）；

1. 安装babel的插件：

```sh
npm install babel-plugin-import -D
```

2. 配置babel.config.js

```js
module.exports = {
  plugins: [
    [
      'import',
      {
        libraryName: 'element-plus',
        customStyleName: (name) => {
          return `element-plus/lib/theme-chalk/${name}.css`
        }
      }
    ]
  ],
  presets: ['@vue/cli-plugin-babel/preset']
}
```

但是这里依然有个弊端：

- 这些组件我们在多个页面或者组件中使用的时候，都需要导入并且在components中进行注册；
- 所以我们可以将它们在全局注册一次；

```ts
import {
  ElButton,
  ElTable,
  ElAlert,
  ElAside,
  ElAutocomplete,
  ElAvatar,
  ElBacktop,
  ElBadge,
} from 'element-plus'

const app = createApp(App)

const components = [
  ElButton,
  ElTable,
  ElAlert,
  ElAside,
  ElAutocomplete,
  ElAvatar,
  ElBacktop,
  ElBadge
]

for (const cpn of components) {
  app.component(cpn.name, cpn)
}
```



**shims-due.d.ts 的作用**

这是vue-cli自动生成的一个文件，用于声明 vue文件。

```ts
/* eslint-disable */
declare module '*.vue' {
  import type { DefineComponent } from 'vue'
  const component: DefineComponent<{}, {}, any>
  export default component
}
```



**setup中使用TS，对于组件定义Ref；需要给Ref添加类型，而这个组件是什么类型的？这该怎么办：**

```ts
const refName = ref<InstanceType<typeof componentName>>()
```

这里 `InstanceType` 是TS的内部语法，用 typeof 获取 componentName 的类型。详见 [[TypeScript学习笔记#InstanceType]]

**对上面内容的补充：**

ref是有类型的，如果没有指定，则是any类型；如果想要指定则：

```ts
const refName = ref<Type>()
```

但是，这里的Type是什么？这就涉及到组件的类型了。<font color=FF0000 size=4>**一个SFC文件中的内容只是组件的描述，而调用这个组件则是产生一个组件实例；这两者的关系类似于类和对象**</font>

上面的 InstanceType\<typeof componet> （这里的component实际上是组件的实例）返回的是一个type，即：type componentType = InstanceType\<typeof componetInstance> 。那么conponentInstance是组件的实例，而componentType是组件的类型；上面的



vuex中保存的数据是放置在内容中的，一旦刷新页面，那么就会消失。所以需要对vuex中的内容做持久化，可以选择的方案是存入localStorage中。并且在 mian.js（即：调用createApp的文件），调用一个将localStorage中的对应数据存入vuex的Action



##### 在 webpack / vue loader 项目中 `~` 的使用

如果在 webpack / vue-cli 设置了路径别名，如：`src`  -> `@`，则可以很方便地使用。但是如何在 template 中如何使用？在别名前面加上 `~` 即可；vue-loader 会自动解析的。示例如下：

```html
<img src="~@/assets/img/logo.svg" />
```

对应的文件是：`src/assets/img/logo.svg`

这个用法，在 [vue-loader 文档](https://vue-loader.vuejs.org/zh/guide/asset-url.html#转换规则) 中有介绍（ [Vue CLI 文档](https://cli.vuejs.org/zh/guide/html-and-static-assets.html#%E4%BB%8E%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84%E5%AF%BC%E5%85%A5:~:text=URL%20%E8%BD%AC%E6%8D%A2%E8%A7%84%E5%88%99-,%23,-%E5%A6%82%E6%9E%9C%20URL%20%E6%98%AF) 中也有介绍）：

> 如果路径以 `~` 开头，其后的部分将会被看作模块依赖



## antfu Vue conf 2021 演讲

<font color=fuchsia>在同时可以使用 ref 和 reactive ，更推荐使用 ref</font> ： 

- **ref**
  - 显式调用（需要使用 `.value` ），类型检查
  - 相比Reactive局限更少
- **reactive**
  - 自动 Unwrap（即：不需要 `.value` ）
  - 在类型上和一般对象没有区别
  - 使用ES6解构会使响应性丢失
  - 由于担心响应式丢失，所以需要使用箭头函数包装才能使用 watch


## antfu  Vue conf 2022 演讲
// TODO