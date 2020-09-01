# Vue.js学习笔记



#### Vue.js是什么

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的<font color=FF0000>**渐进式框架**</font>。与其它大型框架不同的是，<mark>Vue 被设计为可以自底向上逐层应用</mark>。<mark>Vue 的核心库只关注视图层</mark>，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

代码示例：

```html
<div id="app">
    {{message}}
</div>
<script>
    var app = new Vue({
        el: '#app',  //el表示element
        data: {      //data用于保存数据，用于绑定
            message: 'hello vue!'
        }
    });
</script>
```

注意，我们不再和 HTML <font color=FF0000>直接交互</font>了。一个 Vue 应用会将其挂载到一个 DOM 元素上 (对于这个例子是 `#app`) 然后对其进行完全控制。那个 HTML 是我们的入口，但其余都会发生在新创建的 Vue 实例内部。

**另外，这里的vue是一个全局变量**



#### 绑定attribute的另一种方法（第一种是上面的示例）

示例：

```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
<script>
    var app2 = new Vue({
        el: '#app-2',
        data: {
            message: '页面加载于 ' + new Date().toLocaleString()
        }
    })
</script>
```

你看到的 `v-bind` attribute 被称为**指令**。指令带有前缀 `v-`，以表示<font color=FF0000>它们是 Vue 提供的特殊 attribute</font>。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。在这里，该指令的意思是：“将这个元素节点的 `title` attribute 和 Vue 实例的 `message` property 保持一致”。



#### 条件（v-if）

示例：

```html
<div id="app-3">
    <p v-if="seen">现在你看到我了</p>
</div>
<script>
    var app3 = new Vue({
        el: '#app-3',
        data: {
            seen: false
        }
    })
</script>
```



#### 循环

示例：

```html
<div id="app-4">
    <ol>
        <li v-for="todo in todos">  <!--注意这里的v-for的写法-->
            {{ todo.text }}
        </li>
    </ol>
</div>
<script>
    var app4 = new Vue({
        el: '#app-4',
        data: {
            todos: [
                { text: '学习 JavaScript' },
                { text: '学习 Vue' },
                { text: '整个牛项目' }
            ]
        }
    })
</script>
```



#### 处理用户输入

为了让用户和你的应用进行交互，我们<font color=FF0000>可以用 **`v-on`** 指令添加一个事件监听器</font>，通过它调用在 Vue 实例中定义的方法。

示例如下：

```html
<div id="app-5">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">反转消息</button>
</div>
<script>
    var app5 = new Vue({
        el: '#app-5',
        data: {
            message: 'Hello Vue.js!'
        },
        methods: {
            reverseMessage: function () {
                this.message = this.message.split('').reverse().join('')
            }
        }
    })
</script>
```

##### <mark>Vue 还提供了<font color=FF0000> `v-model` </font>指令，它能轻松<font color=FF0000>实现**表单输入**和**应用状态**之间的**双向绑定**</font>。</mark>



#### 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

<img src="https://s1.ax1x.com/2020/08/31/dOeVaT.png" style="zoom: 43%;" />

在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。

#### //todo，这里没有完全看懂...不过可能要等到看完“组件基础”之后才会好些

https://cn.vuejs.org/v2/guide/index.html#%E4%B8%8E%E8%87%AA%E5%AE%9A%E4%B9%89%E5%85%83%E7%B4%A0%E7%9A%84%E5%85%B3%E7%B3%BB



#### Vue实例

每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 **Vue 实例**开始的：

```js
var vm = new Vue({  //这里的vm表示viewModel，这个命名很常见（参考自MVVM模型）
  // 选项
})
```

当创建一个 Vue 实例时，你<font color=FF0000>可以传入一个**选项对象**</font>。这篇教程主要描述的就是如何使用这些选项来创建你想要的行为。作为参考，你也可以在 [API 文档](https://cn.vuejs.org/v2/api/#选项-数据)中浏览完整的选项列表。

一个 Vue 应用由一个通过 `new Vue` 创建的<font color=FF0000>**根 Vue 实例**</font>，以及可选的嵌套的、可复用的组件树组成。



#### 数据与方法

当一个<font color=FF0000> Vue 实例被创建</font>时，<font color=FF0000>它将 `data` 对象中的所有的 property 加入到 Vue 的**响应式系统**中</font>。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

示例：

```js
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的 property
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置 property 也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

<font color=FF0000>当这些数据改变时，视图会进行重渲染</font>。值得注意的是<font color=FF0000>只有**当实例被创建时就已经存在**于 `data` 中的 property 才是**响应式**的</font>。也就是说如果你添加一个新的 property，那么对其改动将不会触发任何视图的更新。

<font color=FF0000>如果你知道你会在晚些时候需要一个 property，但是一开始它（在Vue元素中）为空或不存在，那么你仅需要设置一些初始值</font>。比如：

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

<mark>唯一的例外是使用 `Object.freeze()`，<font color=FF0000>这会阻止修改现有的 property，也意味着响应系统无法再追踪变化</font></mark>。示例：

```js
//obj为需要被freeze的对象
Object:freeze(obj);
```

除了数据 property，<font color=FF0000>Vue 实例还暴露了一些有用的实例 property 与方法</font>。它们<font color=FF0000>都有前缀</font> `$`，<font color=FF0000>以便与用户定义的 property 区分开来</font>（相当于java中的this）

另外，关于`$`有一个watch方法，可以查看一个属性 <font color=FF0000>变化之前</font> 和 <font color=FF0000>变化之后</font> 的值

```js
// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

补充：上面说的是：`vm.$watch`，此外，还有 watch侦听器 ，示例：

```js
watch: {
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
```



#### 实例生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。<mark>同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会</mark>。（类似于AOP）

比如 [`created`](https://cn.vuejs.org/v2/api/#created) 钩子可以用来在一个实例被创建之后执行代码：

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {        //注意这里的created
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 [`mounted`](https://cn.vuejs.org/v2/api/#mounted)、[`updated`](https://cn.vuejs.org/v2/api/#updated) 和 [`destroyed`](https://cn.vuejs.org/v2/api/#destroyed)。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。<font color=FF0000>生命周期钩子的 `this` 上下文指向调用它的 Vue 实例</font>。

**vue示例的生命周期示意图如下：**

<img src="https://s1.ax1x.com/2020/08/31/dOZOqP.png" style="zoom:40%;" />

#### 模板语法

Vue.js 使用了基于 HTML 的模板语法，<font color=FF0000>允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据</font>。<mark>所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析</mark>。

在底层的实现上，<font color=FF0000>Vue **将模板编译成虚拟 DOM 渲染函数**</font>。结合响应系统，<font color=FF0000>Vue 能够**智能地计算出最少需要重新渲染多少组件**，并把 DOM 操作次数减到最少</font>。

如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，[直接写渲染 (render) 函数](https://cn.vuejs.org/v2/guide/render-function.html)，使用可选的 JSX 语法。

**文本：**

<font color=FF0000>数据绑定</font>最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

```html
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 `msg` property 的值。无论何时，<font color=FF0000>绑定的数据对象上 `msg` property 发生了改变，插值处的内容都会更新</font>。

通过使用 [**v-once 指令**](https://cn.vuejs.org/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。<mark>但请留心这会影响到该节点上的其它数据绑定</mark>：

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

**原始HTML**

双大括号会将数据解释为<font color=FF0000>普通文本</font>（无格式的（raw）文本），<font color=FF0000>而非 HTML 代码</font>（有格式的文本）。为了输出真正的 HTML（想输出的带格式的HTML），你需要使用 [`v-html` 指令](https://cn.vuejs.org/v2/api/#v-html)：

示例：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

效果：

- Using mustaches: \<span style="color: red">This should be red.\</span>

- Using v-html directive: <font color=FF0000>This should be red.</font>

这个 `span` 的内容将会被替换成为 property 值 `rawHtml`，直接作为 HTML——会忽略解析 property 值中的数据绑定。注意，你不能使用 `v-html` 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。反之，对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位。

**Attribute**

<font color=FF0000>Mustache 语法不能作用在 HTML attribute（HTML属性） 上</font>，遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)，这样可以动态的为标签绑定属性：

```html
<div v-bind:id="dynamicId"></div>
```

对于<font color=FF0000>布尔 attribute</font> (它们只要存在就意味着值为 `true`)，`v-bind` 工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` attribute 甚至不会被包含在渲染出来的 `<button>` 元素中。

**使用 JavaScript 表达式**

迄今为止，在我们的模板中，我们一直都只绑定简单的 property 键值。但实际上，<font color=FF0000>对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持</font>。

```js
{{ number + 1 }}  //对变量的算数运算

{{ ok ? 'YES' : 'NO' }}  //三目运算

{{ message.split('').reverse().join('') }}  //函数运算

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。<font color=FF0000>有个限制就是，每个绑定都只能包含**单个表达式**</font>，所以下面的例子都**不会**生效。

```js
//这是语句，不是表达式
{{ var a = 1 }}

//流控制也不会生效，请使用三元表达式
{{ if (ok) { return message } }}
```

**指令**

<font color=FF0000>指令</font> (Directives) 是<font color=FF0000>带有 `v-` 前缀的特殊 attribute</font>。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。<mark>指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM</mark>。

**指令的参数**

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```html
<a v-bind:href="url">...</a>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` attribute 与表达式 `url` 的值绑定。

另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
```

在<font color=FF0000>这里参数是监听的事件名</font>。

**动态参数（2.6.0 新增）**

从 2.6.0 开始，<mark>可以用<font color=FF0000>方括号括起来</font>的<font color=FF0000> JavaScript 表达式</font>作为一个指令的参数</mark>，语法：

```html
<!--注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，<font color=FF0000>求得的值将会作为最终的参数来使用</font>。

同样地，你可以<mark><font color=FF0000>使用动态参数</font>为一个<font color=FF0000>动态的事件名<font color=FF0000>绑定处理函数</mark>：

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

在这个示例中，当 `eventName` 的值为 `"focus"` 时，`v-on:[eventName]` 将等价于 `v-on:focus`。

- **对动态参数的值的约束**

  <font color=FF0000>动态参数预期会求出一个字符串，异常情况下值为 `null`</font>。这个特殊的<font color=FF0000> `null` 值**可以被显性地用于移除绑定**</font>。任何其它非字符串类型的值都将会触发一个警告。

- **对动态参数表达式的约束**

  - 动态参数表达式<font color=FF0000>有一些语法约束</font>，因为某些字符，<font color=FF0000>如空格和引号，放在 HTML attribute 名里是无效的</font>。例如：

    ```html
    <!-- 这会触发一个编译警告 -->
    <a v-bind:['foo' + bar]="value"> ... </a>
    ```

    <mark>变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式</mark>。

  - 在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还<font color=FF0000>需要避免使用大写字符来命名键名</font>，因为浏览器会把 attribute 名全部强制转为小写：

    ```html
    <!-- 在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
    除非在实例中有一个名为“someattr”的 property，否则代码不会工作。-->
    <a v-bind:[someAttr]="value"> ... </a>
    ```

<mark><font color=FF0000>修饰符</font> (modifier) 是以半角句号<font color=FF0000> `.` </font>指明的特殊后缀</mark>，<font color=FF0000>用于指出一个指令应该以特殊方式绑定</font>。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```



#### 缩写

<font color=FF0000>**v- 前缀**</font>作为一种视觉提示，用来识别模板中 Vue 特定的 attribute。<mark>当你在使用 Vue.js 为现有标签添加动态行为 (dynamic behavior) 时，v- 前缀很有帮助</mark>，<font color=FF0000>然而</font>，<mark>对于一些频繁用到的指令来说，就会感到使用繁琐</mark>。同时，在构建由 Vue 管理所有模板的单页面应用程序 (SPA - single page application) 时，v- 前缀也变得没那么重要了。因此，Vue 为 v-bind 和 v-on 这两个最常用的指令，提供了特定简写：

- **v-bind 缩写（<font color=FF0000>:</font>）**

  ```html
  <!-- 完整语法 -->
  <a v-bind:href="url">...</a>
  
  <!-- 缩写 -->
  <a :href="url">...</a>
  
  <!-- 动态参数的缩写 (2.6.0+) -->
  <a :[key]="url"> ... </a>
  ```

- **v-on 缩写（<font color=FF0000>@</font>）**

  ```html
  <!-- 完整语法 -->
  <a v-on:click="doSomething">...</a>
  
  <!-- 缩写 -->
  <a @click="doSomething">...</a>
  
  <!-- 动态参数的缩写 (2.6.0+) -->
  <a @[event]="doSomething"> ... </a>
  ```

  它们看起来可能与普通的 HTML 略有不同，但 <font color=FF0000>`:` 与 `@` 对于 attribute 名来说都是合法字符</font>，<mark>在所有支持 Vue 的浏览器都能被正确地解析</mark>。而且，<font color=FF0000>它们不会出现在最终渲染的标记中</font>。缩写语法是完全可选的，但随着你更深入地了解它们的作用，你会庆幸拥有它们。



#### 计算属性

**计算属性**
模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。<mark>在模板中放入太多的逻辑会让模板过重且难以维护</mark>。对于任何复杂逻辑，你都应当使用<font color=FF0000>**计算属性**</font>。示例如下：

```html
<div id="example">
    <p>Original message: "{{ message }}"</p>
    <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
<script>
    var vm = new Vue({
        el: '#example',
        data: {
            message: 'Hello'
        },
        computed: {
            // 计算属性的 getter
            reversedMessage: function () {
                // `this` 指向 vm 实例
                return this.message.split('').reverse().join('')
            }
        }
    })
</script>
```

这里我们声明了一个计算属性 `reversedMessage`。我们提供的函数将用作 property `vm.reversedMessage` 的 getter 函数：

你<font color=FF0000>可以像绑定普通 property 一样在模板中绑定计算属性</font>（<mark>类似于类中的方法</mark>）。Vue 知道 `vm.reversedMessage` 依赖于 `vm.message`，因此当 `vm.message` 发生改变时，所有依赖 `vm.reversedMessage` 的绑定也会更新。而且<mark>最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 getter 函数是没有副作用 (side effect) 的，这使它更易于测试和理解</mark>(因为解耦？)。

<font color=FF0000>**计算属性缓存 vs 方法**</font>

<mark>我们可以将同一函数定义为一个方法而不是一个计算属性。<font color=FF0000>两种方式的**最终结果**确实是**完全相同的**。</font></mark>然而，**不同的**是<font color=FF0000>**计算属性是基于它们的响应式依赖进行缓存的**</font>。<font color=FF0000>只在相关响应式依赖发生改变时它们才会重新求值</font>。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

我们<font color=FF0000>为什么需要缓存</font>？假设我们有一个性能开销比较大的计算属性 **A**，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 **A**。<mark>如果没有缓存，我们将不可避免的多次执行 **A** 的 getter！</mark><font color=FF0000>如果你不希望有缓存，请用方法来替代</font>。

**计算属性 vs 侦听属性**

Vue 提供了一种<mark><font color=FF0000>更通用</font>的方式来<font color=FF0000>观察和响应</font>Vue 实例上的<font color=FF0000>数据变动</font>：**侦听属性**</mark>。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 `watch`——特别是如果你之前使用过 AngularJS。然而，<font color=FF0000>通常更好的做法是使用计算属性而不是命令式的 `watch` 回调</font>。

**计算属性的 setter**

<mark>计算属性默认只有 getter</mark>，不过在需要时你也可以提供一个 setter，示例：

```js
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```



#### 侦听器

虽然<mark>计算属性在大多数情况下更合适</mark>，但有时<mark>也需要一个自定义的侦听器</mark>。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。<font color=FF0000>当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的</font>。

示例：

```js
var watchExampleVM = new Vue({
  //...
	watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  //...
}
```



#### Class 与 Style 绑定

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。<font color=FF0000>表达式结果的类型除了字符串之外，还可以是对象或数组</font>。

- **对象语法**
  我们可以传给 `v-bind:class` 一个对象，以动态地切换 class：
  
  ```html
  <div v-bind:class="{ active: isActive }"></div>
  ```
  
  上面的语法表示 active 这个 class 存在与否将取决于数据 property isActive 的 truthiness。
  
  你<font color=FF0000>可以在对象中传入更多字段来动态切换多个 class</font>。此外，`v-bind:class` 指令也可以与普通的 class attribute 共存。当有如下模板：
  
  ```html
  <div
    class="static"
    v-bind:class="{ active: isActive, 'text-danger': hasError }"
  ></div>
  <script>
  data: {
    isActive: true,
    hasError: false
  }
  </script>
  ```
  
  结果渲染为：
  
  ```html
  <div class="static active"></div>
  ```
  
  当 `isActive` 或者 `hasError` 变化时，class 列表将相应地更新。例如，如果 `hasError` 的值为 `true`，class 列表将变为 `"static active text-danger"`。
  
  渲染的结果和上面一样。我们也可以在这里绑定一个返回对象的[**计算属性**](https://cn.vuejs.org/v2/guide/computed.html)。这是一个常用且强大的模式：
  
  ```html
  <div v-bind:class="classObject"></div>
  
  <script>
  data: {
    isActive: true,
    error: null
  },
  computed: {
    classObject: function () {
      return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal'
      }
    }
  }
  </script>
  ```

- **数组语法**
  我们可以把一个数组传给` v-bind:class`，以应用一个 class 列表：
  
  ```html
  <div v-bind:class="[activeClass, errorClass]"></div>
  <script>
  data: {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
  </script>
  ```
  
  渲染为：
  
  ```html
  <div class="active text-danger"></div>
  ```
  
  如果你也想根据条件切换列表中的 class，可以用三元表达式：
  
  ```html
  <div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
  ```

- **用在组件上**

  当在一个自定义组件上使用 `class` property 时，这些 class 将被添加到该组件的根元素上面。这个元素上已经存在的 class 不会被覆盖。

  例如，如果你声明了这个组件：

  ```html
  Vue.component('my-component', {
    template: '<p class="foo bar">Hi</p>'
  })
  ```

  然后在使用它的时候添加一些 class：

  ```html
  <my-component class="baz boo"></my-component>
  ```

  HTML 将被渲染为：

  ```html
  <p class="foo bar baz boo">Hi</p>
  ```

  对于带数据绑定 class 也同样适用：

  ```html
  <my-component v-bind:class="{ active: isActive }"></my-component>
  ```

  当 `isActive` 为 truthy 时，HTML 将被渲染成为：

  ```html
  <p class="foo bar active">Hi</p>
  ```

**绑定内联样式**

- **对象语法**

  `v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名：

  ```html
  <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  
  <script>
  data: {
    activeColor: 'red',
    fontSize: 30
  }
  </script>
  ```

  当然：较上面的写法，直接绑定到一个样式对象通常更好，这会让模板更清晰：

  ```html
  <div v-bind:style="styleObject"></div>
  <script>
  data: {
    styleObject: {
      color: 'red',
      fontSize: '13px'
    }
  }
  </script>
  ```

  同样的，对象语法常常结合返回对象的计算属性使用。

- **数组语法**
  `v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上：

  ```html
  <div v-bind:style="[baseStyles, overridingStyles]"></div>
  ```

- **自动添加前缀**
  当 `v-bind:style` 使用需要添加浏览器引擎前缀的 CSS property 时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

- **多重值（2.3.0+）**

  你可以为 `style` 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

  ```html
  <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
  ```

  这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 `display: flex`。



#### 条件渲染

- **v-if**

  `v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

  ```html
  <h1 v-if="awesome">Vue is awesome!</h1>
  ```

  也可以用 `v-else` 添加一个“else 块”：

  ```html
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
  ```

  **在 \<template> 元素上使用 v-if 条件渲染分组**
  因为 `v-if` 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 \<template> 元素当做不可见的包裹元素，并在上面使用 v-if。最终的渲染结果将不包含 \<template> 元素。

  ```html
  <template v-if="ok">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
  </template>
  ```

- **v-else**
  你可以使用 v-else 指令来表示 v-if 的“else 块”：

  ```html
  <div v-if="Math.random() > 0.5"> Now you see me </div>
  <div v-else> Now you don't </div>
  ```

- **v-else-if**（2.1.0 新增）

  v-else-if，顾名思义，充当 v-if 的“else-if 块”，可以连续使用：

  ```html
  <div v-if="type === 'A'"> A </div>
  <div v-else-if="type === 'B'"> B </div>
  <div v-else-if="type === 'C'"> C </div>
  <div v-else> Not A/B/C </div>
  ```

- **用 key 管理可复用的元素**

  Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 `key` attribute 即可：

  ```html
  <template v-if="loginType === 'username'">
    <label>Username</label>
    <input placeholder="Enter your username" key="username-input">
  </template>
  <template v-else>
    <label>Email</label>
    <input placeholder="Enter your email address" key="email-input">
  </template>
  ```

**v-show**

另一个用于根据条件展示元素的选项是 v-show 指令。用法大致一样：

```html
<h1 v-show="ok">Hello!</h1>
```

不同的是<font color=FF0000>带有 `v-show` 的元素始终会被渲染并保留在 DOM 中</font>（不过display将会为none）。v-show 只是简单地切换元素的 CSS property display。

<font color=FF0000>注意，v-show 不支持 \<template> 元素，也不支持 v-else。</font>

**v-if vs v-show**

- **v-if 是“真正”的条件渲染**，因为<mark>它会<font color=FF0000>确保在切换过程中条件块内的事件监听器和子组件适当地**被销毁和重建**</font></mark>。

- **v-if 也是<font color=FF0000>惰性</font>的：**<mark>如果<font color=FF0000>在初始渲染时条件为假，则什么也不做</font>——<font color=FF0000>直到条件第一次变为真时，才会开始渲染条件块</font></mark>。

- 相比之下，**v-show** 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。一般来说，<mark>**v-if** 有**更高的切换开销**，而 **v-show** 有**更高的初始渲染开销**</mark>。因此，<mark>如果需要非常频繁地切换，则使用 v-show 较好</mark>；如果在运行时条件很少改变，则使用 v-if 较好。

**v-if 与 v-for 一起使用**
<font color=FF0000>不推荐**（在同一个元素上）**同时使用 v-if 和 v-for</font>，当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。请查阅[列表渲染指南](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if)以获取详细信息。



#### 列表渲染

我们可以用 `v-for` 指令<font color=FF0000>基于一个数组</font>来渲染一个列表。`v-for` 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的**别名**。

示例：

```html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>

<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</script>
```

**在 v-for 里使用对象**

- 可以用 `v-for` 来<font color=FF0000>遍历一个对象的 property</font>

  ```html
  <ul id="v-for-object" class="demo">
    <li v-for="value in object">
      {{ value }}
    </li>
  </ul>
  
  <script>
  new Vue({
    el: '#v-for-object',
    data: {
      object: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
      }
    }
  })
  </script>
  ```

  结果：

  - How to do lists in Vue
  - Jane Doe
  - 2016-04-10

- 也可以提供<font color=FF0000>第二个的参数</font>为<font color=FF0000> property 名称</font> (也就是键名)：

  ```html
  <div v-for="(value, name) in object"> <!--注意这里写法-->
    {{ name }}: {{ value }}  
  </div>
  ```

  结果：

  title: How to do lists in Vue

  author: Jane Doe

  publishedAt: 2016-04-10

- 还可以用<font color=FF0000>第三个参数</font>作为<font color=FF0000>索引</font>：

  ```html
  <div v-for="(value, name, index) in object"> <!--注意这里写法-->
    {{ index }}. {{ name }}: {{ value }}
  </div>
  ```

  结果：

  0. title: How to do lists in Vue
  1. author: Jane Doe
  2. publishedAt: 2016-04-10

**维护状态**

当 Vue 正在更新使用 `v-for` 渲染的元素列表时，它<font color=FF0000>默认使用“就地更新”的策略</font>。<font color=FF0000>如果数据项的顺序被改变</font>，Vue 将<font color=FF0000>**不会**移动 DOM 元素来匹配数据项的顺序</font>，<font color=FF0000>**而是**就地更新每个元素，并且确保它们在每个索引位置正确渲染</font>。

这个默认的模式是高效的，但是<mark>**<font color=FF0000>只适用于</font>不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**</mark>。

<mark>为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素</mark>，你需要为每项提供一个唯一 `key` attribute：

```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```

建议尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

因为它是 Vue 识别节点的一个通用机制，`key` 并不仅与 `v-for` 特别关联。后面我们将在指南中看到，它还具有其它用途。

##### **数组更新检测**

- **变更方法：**Vue <mark>将被侦听的</mark><font color=FF0000>数组的变更方法</font><mark>进行了包裹</mark>，所以它们（的使用？？？）也将会触发视图更新。这些被包裹过的方法包括：

  - **push()**
  - **pop()**
  - **shift()**
  - **unshift()**
  - **splice()**
  - **sort()**
  - **reverse()**

  你可以打开控制台，然后对前面例子的 items 数组尝试调用变更方法。比如 example1.items.push({ message: 'Baz' })。

- **替换数组**
  变更方法，顾名思义：会变更调用了这些方法的原始数组。相比之下，<font color=FF0000>也有非变更方法</font>，例如 <mark>`filter()`、`concat()` 和 `slice()`；<font color=FF0000>它们不会变更原始数组，**而总是返回一个新数组**</font></mark>。当使用非变更方法时，可以用新数组替换旧数组：

  ```js
  example1.items = example1.items.filter(function (item) {
    return item.message.match(/Foo/)
  })
  ```

  你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。<mark>Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的启发式方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作</mark>。

**显示过滤/排序后的结果**
有时，我们想要显示一个数组经过过滤或排序后的版本，而<font color=FF0000>不实际变更或重置原始数据</font>。在这种情况下，<mark>可以创建一个计算属性，来返回过滤或排序后的数组</mark>。

示例：

```html
<li v-for="n in evenNumbers">{{ n }}</li>

<script>
new Vue({
  data: {
  	numbers: [ 1, 2, 3, 4, 5 ]
	},
	computed: {
  	evenNumbers: function () {
    	return this.numbers.filter(function (number) {
      	return number % 2 === 0
    	})
  	}
	}
});
</script>
```

在计算属性不适用的情况下<font color=FF0000> (例如，在嵌套 `v-for` 循环中)</font> 你可以使用一个方法：

```html
<ul v-for="set in sets">
  <li v-for="n in even(set)">{{ n }}</li>
</ul>

<script>
new Vue({
  data: {
	  sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
	},
	methods: {
	  even: function (numbers) {
	    return numbers.filter(function (number) {
	      return number % 2 === 0
	    })
	  }
	}
});
</script>
```

**在组件上使用 v-for**

在自定义组件上，你可以像在任何普通元素上一样使用 `v-for`。（2.2.0+ 的版本里，当在组件上使用 `v-for` 时，`key` 现在是必须的）

```html
<my-component v-for="item in items" :key="item.id"></my-component>
```

然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要使用 prop：

```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```

