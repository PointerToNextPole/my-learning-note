# Vue2 学习笔记



## Vue2 官方文档笔记

#### Vue.js 是什么

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的<font color=FF0000>**渐进式框架**</font>。与其它大型框架不同的是，<mark>Vue 被设计为可以自底向上逐层应用</mark>。<mark>Vue 的核心库只关注视图层</mark>，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

代码示例：

```html
<div id="app">
    {{message}}
</div>rorws
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

#### **指令（用于自定义指令（以v-开头））**

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

#### 侦听器watch

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

然而，<font color=FF0000>任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要使用 prop</font>：

```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```

不自动将 `item` 注入到组件里的原因是，这会使得组件与 `v-for` 的运作紧密耦合。明确组件数据的来源能够使组件在其他场合重复使用。

#### 事件处理

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

- 简单使用，示例：
  
  ```html
  <div id="example-1">
    <button v-on:click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
  </div>
  
  <script>
  var example1 = new Vue({
    el: '#example-1',
    data: {
      counter: 0
    }
  })
  </script>
  ```

- 然而许多事件处理逻辑会更为复杂，所以<mark>直接把 JavaScript 代码写在 `v-on` 指令中是不可行的</mark>。因此<font color=FF0000> `v-on` 还可以接收一个需要调用的方法名称</font>。
  
  示例：
  
  ```html
  <div id="example-2">
    <!-- `greet` 是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
  </div>
  
  <script>
  var example2 = new Vue({
    el: '#example-2',
    data: {
      name: 'Vue.js'
    },
    // 在 `methods` 对象中定义方法
    methods: {
      greet: function (event) {
        // `this` 在方法里指向当前 Vue 实例
        alert('Hello ' + this.name + '!')
        // `event` 是原生 DOM 事件
        if (event) {
          alert(event.target.tagName)
        }
      }
    }
  })
  
  // 也可以用 JavaScript 直接调用方法
  example2.greet() // => 'Hello Vue.js!'
  </script>
  ```

- <font color=FF0000>除了直接绑定</font>到一个方法，也可以在<font color=FF0000>内联 JavaScript 语句</font>中调用方法：
  
  ```html
  <div id="example-3">
    <button v-on:click="say('hi')">Say hi</button>
    <button v-on:click="say('what')">Say what</button>
  </div>
  
  <script>
  new Vue({
    el: '#example-3',
    methods: {
      say: function (message) {
        alert(message)
      }
    }
  })
  </script>
  ```

- <font color=FF0000>有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法</font>；另外 `$event` 可以放在任意的一个参数的位置，事件处理函数内均可以接收到：
  
  ```html
  <button v-on:click="warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>
  
  <script>
  new Vue({
    // ...
      methods: {
        warn: function (message, event) {
          // 现在我们可以访问原生事件对象
          if (event) {
            event.preventDefault()
          }
          alert(message)
        }
      }
  })
  </script>
  ```

**事件修饰符**

在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但<font color=FF0000>更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。</font>

为了解决这个问题，<font color=FF0000>Vue.js 为 v-on 提供了事件修饰符</font>。之前提过，修饰符是由点开头的指令后缀来表示的。

- **.stop：**阻止事件的冒泡传播（防止向上传播）
  
  ```html
  <!-- 阻止单击事件继续传播 -->
  <a v-on:click.stop="doThis"></a>
  ```

- **.prevent：**用来阻止标签的默认行为
  
  ```html
  <!-- 提交事件不再重载页面 -->
  <form v-on:submit.prevent="onSubmit"></form>
  
  <!-- 修饰符可以串联 -->
  <a v-on:click.stop.prevent="doThat"></a>
  
  <!-- 只有修饰符 -->
  <form v-on:submit.prevent></form>
  ```

- **.capture**
  
  ```html
  <!-- 添加事件监听器时使用事件捕获模式 -->c
  <!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
  <div v-on:click.capture="doThis">...</div>
  ```

- **.self：**只关心并触发自己标签上的特定动作的事件（不关心冒泡上来的事件，和.stop类似）
  
  ```html
  <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
  <!-- 即事件不是从内部元素触发的 -->
  <div v-on:click.self="doThat">...</div>
  ```

- **.once**：2.1.4 新增，事件只会触发一次
  
  ```html
  <!-- 点击事件将只会触发一次 -->
  <a v-on:click.once="doThis"></a>
  ```

- **.passive**：2.3.0 新增
  
  Vue 还对应 addEventListener 中的 passive 选项提供了 .passive 修饰符。
  
  ```html
  <!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 而不会等待 `onScroll` 完成  -->
  <!-- 这其中包含 `event.preventDefault()` 的情况 -->
  <div v-on:scroll.passive="onScroll">...</div>
  ```
  
  这个 `.passive` 修饰符尤其能够提升移动端的性能。

**按键修饰符**

在监听键盘事件时，我们经常需要检查详细的按键。<font color=FF0000>Vue 允许为 v-on 在监听键盘事件时添加按键修饰符</font>：

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

你可以直接将 KeyboardEvent.key 暴露的<font color=FF0000>任意有效按键名转换为 kebab-case（使用`-`连接符连接） 来作为修饰符</font>。

```html
<input v-on:keyup.page-down="onPageDown">
```

在上述示例中，处理函数只会在 `$event.key` 等于 `PageDown` 时被调用。

**按键码**（keyCode 的事件用法已经被废弃了并可能不会被最新的浏览器支持）

使用 `keyCode` attribute 也是允许的：

```html
<input v-on:keyup.13="submit">
```

为了在必要的情况下支持旧浏览器，Vue 提供了绝大多数常用的按键码的别名：

- **.enter**
- **.tab**
- **.delete**： (捕获“删除”和“退格”键)
- **.esc**
- **.space**
- **.up**
- **.down**
- **.left**
- **.right**

你还可以通过全局 config.keyCodes 对象自定义按键修饰符别名：

```js
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

**系统修饰键（2.1.0 新增）**

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- **.ctrl**
- **.alt**
- **.shift**
- **.meta**

注意：在 Mac 系统键盘上，<font color=FF0000>meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)</font>。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

示例：

```html
<!-- Alt + C 这里也是修饰符的串联--> 
<input v-on:keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div v-on:click.ctrl="doSomething">Do something</div>
```

**.exact 修饰符（2.5.0 新增）**

`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button v-on:click.exact="onClick">A</button>
```

**鼠标按钮修饰符**（2.2.0 新增）

- **.left**
- **.right**
- **.middle**

这些修饰符会限制处理函数仅响应特定的鼠标按钮。

#### 表单输入绑定

你<font color=FF0000>可以用 `v-model` 指令</font>在表单 `<input>`、`<textarea>` 及 `<select>` 元素上<font color=FF0000>创建双向数据绑定</font>。<font color=FF0000>它会根据控件类型自动选取正确的方法来更新元素</font>。尽管有些神奇，但<mark> `v-model` 本质上不过是语法糖</mark>。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

补充：<mark>`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源</mark>。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。

**`v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：**

- text 和 textarea 元素使用 `value` property 和 `input` 事件；
- checkbox 和 radio 使用 `checked` property 和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。

注意：<mark>对于需要使用输入法 (如中文、日文、韩文等) 的语言</mark>，你会发现 <font color=FF0000>v-model 不会在输入法组合文字过程中得到更新</font>。如果你也想处理这个过程，<font color=FF0000>请使用 input 事件</font>。

**值绑定**

对于单选按钮，复选框及选择框的选项，v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

```html
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

但是有时我们可能<font color=FF0000>想把值绑定到 Vue 实例的一个动态 property 上</font>，<font color=FF0000>这时可以用 `v-bind` 实现</font>，并且这个 property 的值可以不是字符串。

**修饰符**

- .lazy：<font color=FF0000>在**默认情况**下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步</font> (除了上述输入法组合文字时)。你<font color=FF0000>可以添加 lazy 修饰符，从而转为在 change 事件之后进行同步</font>：
  
  ```html
  <!-- 在“change”时而非“input”时更新 -->
  <input v-model.lazy="msg">
  ```

- **.number**：如果<font color=FF0000>想**自动将用户的输入值转为数值类型**，可以给 v-model 添加 number 修饰符</font>：
  
  ```html
  <input v-model.number="age" type="number">
  ```
  
  这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。

- **.trim**：如果<font color=FF0000>要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符</font>：
  
  ```html
  <input v-model.trim="msg">
  ```

#### 组件基础

示例：

```html
<div id="components-demo">
    <button-counter></button-counter>
</div>el

<script>
    // 定义一个名为 button-counter 的新组件
    Vue.component('button-counter', {
        data: function () {
            return {
                count: 0
            }
        },
        template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
    })

    new Vue({
        el: '#components-demo'
    })
</script>
```

因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

**data 必须是一个函数**

当我们定义这个 `<button-counter>` 组件时，你可能会发现它的 `data` 并不是像这样直接提供一个对象：

```js
data: {
  count: 0
}
```

取而代之的是，<font color=FF0000>**一个组件的 `data` 选项必须是一个函数**</font>，因此<font color=FF0000>每个实例可以维护一份被返回对象的独立的拷贝</font>：

```js
data: function () {
  return {
    count: 0
  }
}
```

**组件的组织**

通常一个应用会<font color=FF0000>以一棵嵌套的组件树的形式来组织</font>：

<img src="https://i.loli.net/2020/09/02/5oAQZ3tPiYILTlz.png" style="zoom: 43%;" />

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里<font color=FF0000>**有两种组件的注册类型**：**全局注册**和**局部注册**</font>。<mark>目前，我们的组件都只是通过 `Vue.component` **全局注册**的</mark>：

```js
Vue.component('my-component-name', {
  // ... options ...
})
```

全局注册的组件可以用在其被注册之后的任何 (通过 `new Vue`) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

**通过 Prop 向子组件传递数据**

早些时候，我们提到了创建一个博文组件的事情。**问题是**<mark>如果你<font color=FF0000>不能向这个组件传递某一篇博文的标题或内容之类的我们想展示的数据</font>的话，它是没有办法使用的。这也正是 prop 的由来</mark>。

**Prop 是你可以在组件上注册的一些自定义 attribute**。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。为了给博文组件传递一个标题，我们可以用一个 `props` 选项将其包含在该组件可接受的 prop 列表中：

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

<font color=FF0000>Prop 是你可以在组件上注册的一些自定义 attribute</font>。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。<mark>为了给博文组件传递一个标题，我们可以用一个 `props` 选项将其包含在该组件可接受的 prop 列表中</mark>：

```js
Vue.component('blog-post', {
  props: ['title'],                                  //注意这里的props
  template: '<h3>{{ title }}</h3>'
})
```

<font color=FF0000>一个组件默认可以拥有任意数量的 prop</font>，任何值都可以传递给任何 prop。在上述模板中，<mark>你会发现<font color=FF0000>我们能够在组件实例中访问这个值，就像访问 `data` 中的值一样</font></mark>。

**单个根元素**

当构建一个 `<blog-post>` 组件时，f你的模板最终会包含的东西远不止一个标题：

```html
<h3>{{ title }}</h3>
```

最起码，你会包含这篇博文的正文：

```html
<h3>{{ title }}</h3>
<div v-html="content"></div>
```

然而如果你在模板中尝试这样写，Vue 会显示一个错误，并解释道 **every component must have a single root element (每个组件必须只有一个根元素)**。<font color=FF0000>你可以将模板的内容包裹在一个父元素内，来修复这个问题</font>，例如：

```html
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```

**监听子组件事件**

在我们开发 \<blog-post> 组件时，它的一些功能<font color=FF0000>可能要求我们和父级组件进行沟通</font>。 <mark>Vue 实例提供了一个自定义事件的系统来解决这个问题</mark>。<font color=FF0000>父级组件可以像处理 native DOM 事件一样通过 v-on 监听子组件实例的任意事件。同时子组件可以通过调用内建的 `$emit` 方法并传入事件名称来触发一个事件</font>

**使用事件抛出一个值**

有的时候<font color=FF0000>用一个事件来抛出一个特定的值是非常有用的</font>。例如我们可能想让 `<blog-post>` 组件决定它的文本要放大多少。这时可以使用 `$emit` 的第二个参数来提供这个值：

```html
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```

然后<font color=FF0000>当在父级组件监听这个事件的时候，我们可以通过 `$event` 访问到被抛出的这个值</font>：

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```

或者，如果这个事件处理函数是一个方法：

```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```

那么这个值将会作为第一个参数传入这个方法：

```html
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

**在组件上使用 v-model**

自定义事件也可以用于创建支持 `v-model` 的自定义输入组件。

```html
<input v-model="searchText">
```

等价于：

```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

<font color=FF0000>当用在组件上时，`v-model` 则会这样：</font>

```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

<font color=FF0000>为了让它正常工作，这个组件内的 `<input>` 必须：</font>

- 将其 `value` attribute 绑定到一个名叫 `value` 的 prop 上
- 在其 `input` 事件被触发时，将新的值通过自定义的 `input` 事件抛出

写成代码之后是这样的：

```html
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

现在 `v-model` 就应该可以在这个组件上完美地工作起来了：

```html
<custom-input v-model="searchText"></custom-input>
```

**通过插槽分发内容**

和 HTML 元素一样，我们经常<font color=FF0000>需要向一个组件传递内容</font>，像这样：

```html
<alert-box>
  Something bad happened.
</alert-box>
```

可能会渲染出这样的东西：

<img src="https://i.loli.net/2020/09/02/R3di65CIomZn1O4.png" style="zoom: 45%;" />

幸好，Vue 自定义的 `<slot>` （插槽）元素让这变得非常简单：

```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```

如你所见，我们只要在需要的地方加入插槽就行了——就这么简单！

**动态组件**

有的时候，在<font color=FF0000>不同组件之间进行动态切换</font>是非常有用的，比如在一个多标签的界面里：

<img src="https://i.loli.net/2020/09/02/ZtpCuUbs2Kjdi9m.png" style="zoom:50%;" />

<font color=FF0000>上述内容可以通过 Vue 的 `<component>` 元素加一个特殊的 <font size = 5>**`is`** </font>attribute 来实现：</font>

```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```

在上述示例中，`currentTabComponent` 可以包括

- 已注册组件的名字
- 一个组件的选项对象

请留意，这个 attribute 可以用于常规 HTML 元素，但这些元素将被视为组件，这意味着所有的 attribute 都会作为 DOM attribute 被绑定。对于像 value 这样的 property，若想让其如预期般工作，你需要使用 .prop 修饰器。

**//todo 这里还是没有看懂...**

**解析 DOM 模板时的注意事项**

有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。

这会导致我们使用这些有约束条件的元素时遇到一些问题。例如：

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

这个自定义组件 `<blog-post-row>` 会被作为无效的内容提升到外部，并导致最终渲染结果出错。幸好这个特殊的 `is` attribute 给了我们一个变通的办法：

```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```

需要注意的是**如果我们从以下来源使用模板的话，这条限制是不存在的**：

- 字符串 (例如：template: '...')
- 单文件组件 (.vue)
- \<script type="text/x-template">

#### 组件注册

**组件名**

在注册一个组件的时候，我们始终需要给它一个名字。比如在全局注册的时候我们已经看到了：

```js
Vue.component('my-component-name', { /* ... */ })
```

该组件名就是 `Vue.component` 的第一个参数。

你给予组件的名字可能依赖于你打算拿它来做什么。当直接在 DOM 中使用一个组件 (而不是在字符串模板或单文件组件) 的时候，我们<font color=FF0000>强烈推荐遵循 W3C 规范中的自定义组件名 (字母全小写且必须包含一个连字符)</font>。这会帮助你避免和当前以及未来的 HTML 元素相冲突。

**组件名大小写**

定义组件名的方式有两种：

- **使用 kebab-case**
  
  ```js
  Vue.component('my-component-name', { /* ... */ })
  ```
  
  当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也<font color=FF0000>必须在引用这个自定义元素时使用 kebab-case</font>，例如 `<my-component-name>`。

- **使用 PascalCase**
  
  ```js
  Vue.component('MyComponentName', { /* ... */ })
  ```
  
  <font color=FF0000>当使用 PascalCase (首字母大写命名) 定义一个组件时，你在**引用这个自定义元素时两种命名法都可以使用**</font>。<mark>也就是说 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的</mark>。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。

**局部注册**

**全局注册往往是不够理想的**。比如，如果你使用一个像 webpack 这样的构建系统，<font color=FF0000>全局注册所有的组件意味着**即便你已经不再使用一个组件了**，**它仍然会被包含在你最终的构建结果中**</font>。这造成了用户下载的 JavaScript 的无谓的增加。

在这些情况下，你<font color=FF0000>可以通过一个普通的 JavaScript 对象来定义组件</font>：

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后<font color=FF0000>在 `components` 选项中定义你想要使用的组件</font>：

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

<font color=FF0000>对于 `components` 对象中的每个 property 来说，其 property 名就是自定义元素的名字</font>，其 property 值就是这个组件的选项对象。

<mark>**注意**：**局部注册的组件在其<font color=FF0000>子组件</font>中不可用**</mark>。例如，如果你希望 `ComponentA` 在 `ComponentB` 中可用，则你需要这样写：

```js
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA  //
  },
  // ...
}
```

#### prop

**Prop 的大小写 (camelCase vs kebab-case)**

<mark>HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符</mark>。这意味着<font color=FF0000>当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名**需要**使用其等价的 kebab-case (短横线分隔命名) 命名</font>：

```html
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```

重申一次，如果你使用字符串模板，那么这个限制就不存在了。

**Prop 类型**

到这里，我们只看到了以字符串数组形式列出的 prop：

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

但是，<mark>通常你希望每个 prop 都有指定的值类型</mark>。这时，你<font color=FF0000>可以以对象形式列出 prop，这些 property 的名称和值分别是 prop 各自的名称和类型</font>：

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

**传递静态或动态 Prop**

像这样，你已经知道了可以像这样给 prop 传入一个<font color=FF0000>静态</font>的值：

```html
<blog-post title="My journey with Vue"></blog-post>
```

你也知道 prop 可以通过 `v-bind` <font color=FF0000>动态赋值</font>，例如：

```html
<!-- 动态赋予一个变量的值 -->
<blog-post v-bind:title="post.title"></blog-post>

<!-- 动态赋予一个复杂表达式的值 -->
<blog-post
  v-bind:title="post.title + ' by ' + post.author.name"
></blog-post>
```

<mark>在上述两个示例中，我们<font color=FF0000>传入的值都是字符串类型的</font></mark>，但实际上<font color=FF0000>任何类型的值都可以传给一个 prop</font>。

- 传入数字
  
  ```html
  <!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post v-bind:likes="42"></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:likes="post.likes"></blog-post>
  ```

- 传入布尔值
  
  ```html
  <!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
  <blog-post is-published></blog-post>
  
  <!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post v-bind:is-published="false"></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:is-published="post.isPublished"></blog-post>
  ```

- 传入数组
  
  ```html
  <!-- 即便数组是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:comment-ids="post.commentIds"></blog-post>
  ```

- 传入对象
  
  ```html
  <!-- 即便对象是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post
    v-bind:author="{
      name: 'Veronica',
      company: 'Veridian Dynamics'
    }"
  ></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:author="post.author"></blog-post>
  ```

- 传入一个对象的所有 property
  
  ```html
  <blog-post v-bind="post"></blog-post>
  
  <script>
  post: {
    id: 1,
    title: 'My Journey with Vue'
  }
  </script>
  ```
  
  等价于
  
  ```html
  <blog-post
    v-bind:id="post.id"
    v-bind:title="post.title"
  ></blog-post>
  ```

**单向数据流**

<font color=FF0000>所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。</font><mark>这样会<font color=FF0000>**防止**从子组件意外变更父级组件的状态</font>，从而导致你的应用的数据流向难以理解。</mark>

额外的，<font color=FF0000>每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值</font>。这意味着你<font color=FF0000>**不**应该</font>A在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

**这里有<font color=FF0000>两种常见的试图变更一个 prop 的情形</font>：**

- 这个 <font color=FF0000>prop 用来**传递一个初始值**；这个子组件接下来**希望将其作为一个本地的 prop 数据来使用**</font>。在这种情况下，<font color=FF0000>最好定义一个本地的 data property 并将这个 prop 用作其初始值</font>：
  
  ```js
  props: ['initialCounter'],
  data: function () {
    return {
      counter: this.initialCounter
    }
  }
  ```

- 这个 <font color=FF0000>prop **以一种原始的值传入**且**需要进行转换**</font>。在这种情况下，<font color=FF0000>最好使用这个 prop 的值来定义一个计算属性</font>：
  
  ```js
  props: ['size'],
  computed: {
    normalizedSize: function () {
      return this.size.trim().toLowerCase()
    }
  }
  ```

**Prop 验证**

我们'<font color=FF0000>'可以为组件的 prop 指定验证要求</font>，<mark>例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你</mark>。这在开发一个会被别人用到的组件时尤其有帮助。

<mark><font color=FF0000>为了定制 prop 的验证方式</font>，你<font color=FF0000>可以为 `props` 中的值提供一个带有验证需求的对象</font>，而不是一个字符串数组</mark>。

**<font color=FF0000>示例：</font>（看得出来，这个示例知识点不少）**

```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,

    // 多个可能的类型
    propB: [String, Number],

    // 必填的字符串
    propC: {
      type: String,
      required: true
    },

    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },

    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },

    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

**类型检查**

type 可以是下列原生构造函数中的一个：

- **String**
- **Number**
- **Boolean**
- **Array**
- **Object**
- **Date**
- **Function**
- **Symbol**

**额外的**，<font color=FF0000>`type` 还可以是一个自定义的构造函数，并且通过 `instanceof` 来进行检查确认</font>。例如，给定下列现成的构造函数：

```
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

你可以使用：

```
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```

来验证 `author` prop 的值是否是通过 `new Person` 创建的。

**非 Prop 的 Attribute**

一个非 prop 的 attribute 是指传向一个组件，但是该组件并没有相应 prop 定义的 attribute。

因为<mark>显式定义的 prop 适用于向一个子组件传入信息</mark>，然而<font color=FF0000>组件库的作者并不总能预见组件会被用于怎样的场景。这也是为什么组件可以接受任意的 attribute</font>，<font color=FF0000>而这些 attribute 会被添加到这个组件的根元素上</font>。

例如，想象一下你通过一个 Bootstrap 插件使用了一个第三方的 `<bootstrap-date-input>` 组件，这个插件需要在其 `<input>` 上用到一个 `data-date-picker` attribute。我们可以将这个 attribute 添加到你的组件实例上：

```html
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
```

然后这个 `data-date-picker="activated"` attribute 就会自动添加到 `<bootstrap-date-input>` 的根元素上。

**替换/合并已有的 Attribute**

想象一下 `<bootstrap-date-input>` 的模板是这样的：

```html
<input type="date" class="form-control">
```

为了给我们的日期选择器插件定制一个主题，我们可能需要像这样添加一个特别的类名：

```html
<bootstrap-date-input
  data-date-picker="activated"
  class="date-picker-theme-dark"
></bootstrap-date-input>
```

在这种情况下，我们定义了两个不同的 `class` 的值：

- `form-control`，这是<mark>在组件的模板内设置好的</mark>
- `date-picker-theme-dark`，这是<mark>从组件的父级传入的</mark>

对于<font color=FF0000>**绝大多数 attribute** </font>来说，<font color=FF0000>从外部提供给组件的值会替换掉组件内部设置好的值</font>。所以如果传入 `type="text"` 就会替换掉 `type="date"` 并把它破坏！庆幸的是，`class` 和 `style` attribute 会稍微智能一些，即两边的值会被合并起来，从而得到最终的值：`form-control date-picker-theme-dark`。

**禁用 Attribute 继承**

如果你<font color=FF0000>**不**希望组件的根元素**继承** attribute</font>，你<font color=FF0000>可以在组件的选项中设置 `inheritAttrs: false`</font>。例如：

```js
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```

这尤其适合配合实例的 `$attrs` property 使用，该 property 包含了传递给一个组件的 attribute 名和 attribute 值，例如：

```js
{
  required: true,
  placeholder: 'Enter your username'
}
```

有了 `inheritAttrs: false` 和 `$attrs`，你就可以手动决定这些 attribute 会被赋予哪个元素。在撰写[基础组件](https://cn.vuejs.org/v2/style-guide/#基础组件名-强烈推荐)的时候是常会用到的：

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    </label>
  `
})
```

#### 自定义事件

**事件名**

<mark>不同于组件和 prop</mark>，<font color=FF0000>**事件名**不存在任何自动化的大小写转换</font>。而是触发的事件名需要完全匹配监听这个事件所用的名称。举个例子，如果触发一个 camelCase 名字的事件：

```js
this.$emit('myEvent')
```

则监听这个名字的 kebab-case 版本是不会有任何效果的：

```html
<!-- 没有效果 -->
<my-component v-on:my-event="doSomething"></my-component>
```

不同于组件和 prop，事件名不会被用作一个 JavaScript 变量名或 property 名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 `v-on` 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到。

<font color=FF0000>因此，我们推荐你**始终使用 kebab-case 的事件名**。</font>

**自定义组件的 v-model**（2.2.0+ 新增）

一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value attribute 用于不同的目的。model 选项可以用来避免这样的冲突：

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 `v-model` 的时候：

```html
<base-checkbox v-model="lovingVue"></base-checkbox>
```

这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 prop。同时当 `<base-checkbox>` 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的 property 将会被更新。

**将原生事件绑定到组件**

你可能有很多次<font color=FF0000>想要在一个组件的根元素上直接监听一个原生事件</font>。这时，你<font color=FF0000>可以使用 `v-on` 的 `.native` 修饰符</font>：

```html
<base-input v-on:focus.native="onFocus"></base-input>
```

在有的时候这是很有用的，不过在你尝试监听一个类似 `<input>` 的非常特定的元素时，这并不是个好主意。比如上述 `<base-input>` 组件可能做了如下重构，所以根元素实际上是一个 `<label>` 元素：

```html
<label>
  {{ label }}
  <input
    v-bind="$attrs"
    v-bind:value="value"
    v-on:input="$emit('input', $event.target.value)"
  >
</label>
```

这时，<mark>父级的 `.native` 监听器将静默失败</mark>。它不会产生任何报错，但是<mark> `onFocus` 处理函数不会如你预期地被调用</mark>。

<mark>为了解决这个问题</mark>，<font color=FF0000>Vue 提供了一个 `$listeners` property，它是一个对象，里面包含了作用在这个组件上的所有监听器</font>。例如：

```js
{
  focus: function (event) { /* ... */ }
  input: function (value) { /* ... */ },
}
```

有了这个 `$listeners` property，你就可以配合 `v-on="$listeners"` 将所有的事件监听器指向这个组件的某个特定的子元素。对于类似 `<input>` 的你希望它也可以配合 `v-model` 工作的组件来说，为这些监听器创建一个类似下述 `inputListeners` 的计算属性通常是非常有用的：

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  computed: {
    inputListeners: function () {
      var vm = this
      // `Object.assign` 将所有的对象合并为一个新对象
      return Object.assign({},
        // 我们从父级添加所有的监听器
        this.$listeners,
        // 然后我们添加自定义监听器，
        // 或覆写一些监听器的行为
        {
          // 这里确保组件配合 `v-model` 的工作
          input: function (event) {
            vm.$emit('input', event.target.value)
          }
        }
      )
    }
  },
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on="inputListeners"
      >
    </label>
  `
})
```

现在 `<base-input>` 组件是一个**完全透明的包裹器**了，也就是说它可以完全像一个普通的 `<input>` 元素一样使用了：所有跟它相同的 attribute 和监听器都可以工作，不必再使用 `.native` 监听器。

**//todo 这里依然没看懂....**

**.sync 修饰符**（2.3.0+ 新增）

在有些情况下，我们<font color=FF0000>可能需要对一个 prop 进行“双向绑定”</font>。不幸的是，<mark>真正的**双向绑定会带来维护上的问题**，因为**子组件可以变更父组件**，且在父组件和子组件都没有明显的变更来源</mark>。

这也是为什么我们推荐以 `update:myPropName` 的模式触发事件取而代之。举个例子，在一个包含 `title` prop 的假设的组件中，我们可以用以下方法表达对其赋新值的意图：

```js
this.$emit('update:title', newTitle)
```

然后父组件可以监听那个事件并根据需要更新一个本地的数据 property。例如：

```html
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

为了方便起见，<font color=FF0000>我们为这种模式提供一个缩写，即 `.sync` 修饰符</font>：

```html
<text-document v-bind:title.sync="doc.title"></text-document>
```

**//todo 这里依然没看懂....**

#### 插槽（<font color=FF0000>向一个组件传递内容</font>）

在 2.6.0 中，我们为具名插槽和作用域插槽引入了一个新的统一的语法 (即 `v-slot` 指令)。它取代了 `slot` 和 `slot-scope` 这两个目前已被废弃但未被移除且仍在[文档中](https://cn.vuejs.org/v2/guide/components-slots.html#废弃了的语法)的 attribute。

**插槽内容**

Vue 实现了一套内容分发的 API，这套 API 的设计灵感源自 [Web Components 规范草案](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md)，将 `<slot>` 元素作为承载分发内容的出口。

它允许你像这样合成组件：

```html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

然后你在 `<navigation-link>` 的模板中可能会写为：

```html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

当组件渲染的时候，<font color=FF0000>`<slot></slot>` 将会被替换为“Your Profile”。插槽内可以包含任何模板代码，包括 HTML</font>：

```html
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```

甚至其它的组件：

```html
<navigation-link url="/profile">
  <!-- 添加一个图标的组件 -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>
```

如果 `<navigation-link>` 的 `template` 中**没有**包含一个 `<slot>` 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。

**编译作用域**

当你想在一个插槽中使用数据时，例如：

```html
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

该插槽跟模板的其它地方一样可以访问相同的实例 property (也就是相同的“作用域”)，<font color=FF0000>而**不能**访问 `<navigation-link>` 的作用域</font>。例如 `url` 是访问不到的：

```html
<navigation-link url="/profile">
  Clicking here will send you to: {{ url }}
  <!--
  这里的 `url` 会是 undefined，因为其 (指该插槽的) 内容是传递给
    <navigation-link>的，而不是在 <navigation-link> 组件内部定义的。
  -->
</navigation-link>
```

作为一条规则，请记住：<font color=FF0000>**父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的**</font>。

**后备 (也<font color=FF0000>就是默认的</font>) 内容**

有时为一个插槽设置具体的<font color=FF0000>后备</font>内容是很有用的，它<mark>只会在没有提供内容的时候被渲染</mark>。例如在一个 `<submit-button>` 组件中：

```html
<button type="submit">
  <slot></slot>
</button>
```

我们可能希望这个 `<button>` 内绝大多数情况下都渲染文本“Submit”。<font color=FF0000>为了将“Submit”作为后备内容，我们可以将它放在 `<slot>` 标签内</font>

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

现在当我在一个父级组件中使用 `<submit-button>` 并且不提供任何插槽内容时：

```html
<submit-button></submit-button>
```

后备内容“Submit”将会被渲染：

```html
<button type="submit">
  Submit
</button>
```

但是如果我们提供内容：

```html
<submit-button>
  Save
</submit-button>
```

则这个提供的内容将会被渲染从而取代后备内容：

```html
<button type="submit">
  Save
</button>
```

**具名插槽**（自 2.6.0 起有所更新，`slot`被废弃）

有时我们需要多个插槽。例如对于一个带有如下模板的 `<base-layout>` 组件：

```html
<div class="container">
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>
```

对于这样的情况，<font color=FF0000>`<slot>` 元素有一个特殊的 attribute：`name`</font>。这个 attribute 可以用来定义额外的插槽：

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

<font color=FF0000>一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”</font>。

<mark>在向具名插槽提供内容的时候</mark>，<font color=FF0000>我们可以在一个 `<template>` 元素上使用<font size=5> `v-slot` </font>指令</font>，**<font color=FF0000>并以 `v-slot` 的参数的形式提供其名称</font>**：

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。**任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。**

可以在一个 `<template>` 中包裹默认插槽的内容：

```html
<base-layout>
  <!-- ... -->

  <template v-slot:default>   <!-- 注意这里的default -->
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <!-- ... -->
</base-layout>
```

**作用域插槽**（自 2.6.0 起有所更新。 `slot-scope` 被废弃）

有时'<font color=FF0000>'让插槽内容能够**访问子组件中**才有**的数据**</font>是很有用的。例如，设想一个带有如下模板的 `<current-user>` 组件：

```html
<span>
  <slot>{{ user.lastName }}</slot>
</span
```

我们可能想换掉备用内容，用名而非姓来显示。如下：

```html
<current-user>
  {{ user.firstName }}
</current-user>
```

然而<font color=FF0000>上述代码**不会正常工作**</font>，因为<font color=FF0000>只有 `<current-user>` 组件可以访问到 `user`</font> 而我们提供的内容是在父级渲染的。

<font color=FF0000>为了让 `user` 在父级的插槽内容（子元素）中可用</font>，<font color=FF0000>我们可以将 `user` 作为 `<slot>` 元素的一个 attribute 绑定上去</font>：

```html
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

<font color=FF0000>绑定在 `<slot>` 元素上的 attribute 被称为**插槽 prop**</font>。现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字：

```html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 `slotProps`，但你也可以使用任意你喜欢的名字。

**独占默认插槽的缩写语法**

在上述情况下，<font color=FF0000>当被提供的内容**只有默认插槽**时，组件的标签才可以被当作插槽的模板来使用</font>。这样我们就可以把 `v-slot` 直接用在组件上：

```html
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

这种写法还可以更简单。就像假定未指明的内容对应默认插槽一样，<font color=FF0000>不带参数的 `v-slot` 被假定对应默认插槽（缩写写法）</font>：

```html
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

注意默认插槽的<font color=FF0000>缩写语法**不能**和具名插槽混用</font>，<font color=FF0000>因为它会导致作用域不明确</font>：

```html
<!-- 无效，会导致警告 -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

<mark>只要出现多个插槽，请始终为所有的插槽使用完整的基于 `<template>` 的语法</mark>，<font color=FF0000>即：不能省略</font>：

```html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

**解构插槽 Prop**

**作用域插槽的内部<font color=FF0000>工作原理</font>**是<font color=FF0000>将你的插槽内容包括在一个传入单个参数的函数里</font>：

```js
function (slotProps) {
  // 插槽内容
}
```

这意味着<mark> `v-slot` 的值实际上<font color=FF0000>可以是任何能够作为函数定义中的参数的 JavaScript 表达式</font></mark>。所以在支持的环境下 ([单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)或[现代浏览器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#浏览器兼容))，你也可以使用 [ES2015 解构](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#解构对象)来传入具体的插槽 prop，如下：

```html
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```

这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样<font color=FF0000>开启了 prop 重命名等其它可能</font>，<font color=FF0000>例如将 `user` 重命名为 `person`</font>：

```html
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

<mark>你甚至可以定义后备内容，用于插槽 prop 是 undefined 的情形</mark>：

```html
<current-user v-slot="{ user = { firstName: 'Guest' } }"> <!--设置默认值-->
  {{ user.firstName }}
</current-user>
```

**动态插槽名**（2.6.0 新增）

<font color=FF0000>动态指令参数</font>也可以用在 v-slot 上，来定义动态的插槽名：

```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

**具名插槽的缩写**（2.6.0 新增）

跟 `v-on` 和 `v-bind` 一样，<font color=FF0000>`v-slot` 也有缩写，即把参数之前的所有内容 (`v-slot:`) 替换为字符 <font size=5>`#`</font></font>。例如 `v-slot:header` 可以被重写为 `#header`：

```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

然而，和其它指令一样，<font color=FF0000>该缩写只在其有参数的时候才可用</font>。这意味着以下语法是无效的：

```html
<!-- 这样会触发一个警告 -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>
```

<mark>如果你希望使用缩写的话，你必须始终以明确插槽名取而代之</mark>：

```html
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```

**其它**

**<font color=FF0000>插槽 prop 允许我们将插槽转换为可复用的模板</font>，这些模板可以基于输入的 prop 渲染出不同的内容。**这在设计封装数据逻辑同时允许父级组件自定义部分布局的可复用组件时是最有用的。

例如，我们要实现一个 `<todo-list>` 组件，它是一个列表且包含布局和过滤逻辑：

```html
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

我们可以将每个 todo 作为父级组件的插槽，以此通过父级组件对其进行控制，然后将 `todo` 作为一个插槽 prop 进行绑定：

```html
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    <!--
    我们为每个 todo 准备了一个插槽，
    将 `todo` 对象作为一个插槽的 prop 传入。
    -->
    <slot name="todo" v-bind:todo="todo">
      <!-- 后备内容 -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```

现在当我们使用 `<todo-list>` 组件的时候，我们可以选择为 todo 定义一个不一样的 `<template>` 作为替代方案，并且可以从子组件获取数据：

```html
<todo-list v-bind:todos="todos">
  <template v-slot:todo="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

#### 动态组件 & 异步组件

**在动态组件上使用 keep-alive**

我们之前曾经在一个多标签的界面中使用 `is` attribute 来切换不同的组件：

```html
<component v-bind:is="currentTabComponent"></component>
```

当在这些组件之间切换的时候，你<font color=FF0000>有时会想保持这些组件的状态，以**避免反复重渲染**导致的性能问题</font>。例如我们来展开说一说这个多标签界面：

<img src="https://i.loli.net/2020/09/03/vYAouU64iLq7GHN.png" style="zoom:45%;" />

你会注意到，如果你选择了一篇文章，切换到 *Archive* 标签，然后再切换回 *Posts*，是不会继续展示你之前选择的文章的。这是因为你<font color=FF0000>每次切换新标签的时候，Vue 都创建了一个新的 `currentTabComponent` 实例</font>。

重新创建动态组件的行为通常是非常有用的，但是在这个案例中，<mark>我们更希望那些标签的组件实例能够被在它们第一次被创建的时候缓存下来</mark>。为了解决这个问题，<font color=FF0000>**我们可以用一个 `<keep-alive>` 元素将其动态组件包裹起来**</font>。

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

**补充：**

**keep-alive的Props**：

- `include` - 字符串或正则表达式。只有名称匹配的组件会被缓存。
- `exclude` - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
- `max` - 数字。最多可以缓存多少组件实例。

当组件在 \<keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。

摘自：[vue api文档 -  keep-alive](https://cn.vuejs.org/v2/api/#keep-alive)

**另外：更多详细的讲解可以看[Vue keep-alive深入理解及实践总结](https://juejin.cn/post/6844903919273918477)**

**异步组件**

在大型应用中，我们<mark>可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块</mark>。为了简化，<font color=FF0000>Vue 允许你以一个<font size=4>工厂函数</font>的方式定义你的组件</font>，<font color=FF0000>这个工厂函数会<font size=4>**异步解析你的组件定义**</font></font>。<font color=FF0000>Vue **只有在这个组件需要被渲染的时候才会触发该工厂函数**，<font size=4>**且会把结果缓存起来供未来重渲染**</font></font>。例如：

```js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

如你所见，这个工厂函数会收到一个 resolve 回调，<mark>这个回调函数<font color=FF0000>会在你从服务器得到组件定义的时候被调用</font></mark>。你也可以调用 reject(reason) 来表示加载失败。这里的 setTimeout 是为了演示用的，如何获取组件取决于你自己。一个推荐的做法是将异步组件和 webpack 的 code-splitting 功能一起配合使用：

```js
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack自动将你的构建代码切割成多个包
  // ，这些包会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
```

你也可以在工厂函数中返回一个 `Promise`，所以把 webpack 2 和 ES2015 语法加在一起，我们可以这样使用动态导入：

```js
Vue.component(
  'async-webpack-example',
  // 这个动态导入会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

当使用局部注册的时候，你也可以直接提供一个返回 Promise 的函数：

```js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

**处理加载状态**（2.3.0+ 新增）

这里的异步组件工厂函数也可以返回一个如下格式的对象：

```js
const AsyncComponent = () => ({
  // 需要加载的组件 (应该是一个 `Promise` 对象)
  component: import('./MyComponent.vue'),
  // 异步组件加载时使用的组件
  loading: LoadingComponent,
  // 加载失败时使用的组件
  error: ErrorComponent,
  // 展示加载时组件的延时时间。默认值是 200 (毫秒)
  delay: 200,
  // 如果提供了超时时间且组件加载也超时了，
  // 则使用加载失败时使用的组件。默认值是：`Infinity`
  timeout: 3000
})
```


#### 处理边界情况

**访问元素 & 组件**

在绝大多数情况下，我们<mark>最好不要触达另一个组件实例内部或手动操作 DOM 元素</mark>。不过也确实在一些情况下做这些事情是合适的。

**<font color=FF0000>访问根实例</font>**

在每个 `new Vue` 实例的子组件中，<font color=FF0000>其<font size=5>**根实例**</font>可以通过<font size=5> `$root`</font> property 进行访问</font>。例如，在这个根实例中：

```js
// Vue 根实例
new Vue({
  data: {
    foo: 1
  },
  computed: {
    bar: function () { /* ... */ }
  },
  methods: {
    baz: function () { /* ... */ }
  }
})
```

<font color=FF0000>所有的子组件</font>都可以将这个实例作为一个全局 store 来访问或使用。

```js
// 获取根组件的数据
this.$root.foo

// 写入根组件的数据
this.$root.foo = 2

// 访问根组件的计算属性
this.$root.bar

// 调用根组件的方法
this.$root.baz()
```

**<font color=FF0000>访问父级组件</font>实例**

和 `$root` 类似，<font color=FF0000><font size=5>`$parent`</font> property 可以用来**从一个子组件访问父组件**的实例</font>。它提供了一种机会，可以在后期随时触达父级组件，以替代将数据以 prop 的方式传入子组件的方式。

<mark>**<font color=FF0000>注意：</font>**在<font color=FF0000>绝大多数情况下</font>，<font color=FF0000>触达父级组件会使得你的应用更难调试和理解</font>，尤其是当你变更了父级组件的数据的时候。当我们稍后回看那个组件的时候，很难找出那个变更是从哪里发起的。</mark>

另外在一些可能适当的时候，你需要特别地共享一些组件库。举个例子，在和 JavaScript API 进行交互而不渲染 HTML 的抽象组件内，诸如这些假设性的 Google 地图组件一样：

```html
<google-map>
  <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map>
```

这个 `<google-map>` 组件可以定义一个 `map` property，所有的子组件都需要访问它。<mark>在这种情况下 `<google-map-markers>` 可能想要通过类似 `this.$parent.getMap` 的方式访问那个地图，以便为其添加一组标记</mark>。

请留意，尽管如此，<font color=FF0000>通过这种模式构建出来的那个组件的内部仍然是容易出现问题的</font>。比如，设想一下我们添加一个新的 `<google-map-region>` 组件，当 `<google-map-markers>` 在其内部出现的时候，只会渲染那个区域内的标记：

```html
<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>
```

那么在 `<google-map-markers>` 内部你可能发现自己需要一些类似这样的 hack：

```js
var map = this.$parent.map || this.$parent.$parent.map
```

很快它就会失控。这也是我们针对需要向任意更深层级的组件提供上下文信息时推荐[依赖注入](https://cn.vuejs.org/v2/guide/components-edge-cases.html#依赖注入)的原因。

**访问<font color=FF0000>子组件实例</font>或<font color=FF0000>子元素</font>**

<font color=FF0000>尽管存在 prop 和事件，有的时候你仍可能需要在 JavaScript 里直接访问一个子组件</font>。为了达到这个目的，<font color=FF0000>你可以通过<font size=5> `ref`</font> 这个 attribute **为子组件赋予一个 ID 引用</font>**。例如：

```html
<base-input ref="usernameInput"></base-input>
```

现在在你已经定义了这个 `ref` 的组件里，你可以使用：

```js
//注意：ref的使用
this.$refs.usernameInput
```

来访问这个 `<base-input>` 实例，以便不时之需。比如程序化地从一个父级组件聚焦这个输入框。在刚才那个例子中，该 `<base-input>` 组件也可以使用一个类似的 `ref` 提供对内部这个指定元素的访问，例如：

```html
<input ref="input">
```

甚至可以通过其父级组件定义方法：

```js
methods: {
  // 用来从父级组件聚焦输入框
  focus: function () {
    this.$refs.input.focus()
  }
}
```

这样就允许父级组件通过下面的代码聚焦 `<base-input>` 里的输入框：

```js
this.$refs.usernameInput.focus()
```

当 `ref` 和 `v-for` 一起使用的时候，你得到的 ref 将会是一个包含了对应数据源的这些子组件的数组。

**依赖注入**

在此之前，在我们描述访问父级组件实例的时候，展示过一个类似这样的例子：

```html
<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>
```

在这个组件里，所有 `<google-map>` 的后代都需要访问一个 `getMap` 方法，以便知道要跟哪个地图进行交互。不幸的是，使用 `$parent` property 无法很好的扩展到更深层级的嵌套组件上。这也是<font color=FF0000>依赖注入</font>的用武之地，<font color=FF0000>它用到了两个新的实例选项：<font size=5>`provide`</font>（发送） 和<font size=5> `inject`</font>（接收）</font>。

<font color=FF0000>`provide` 选项允许我们<font size=5>**指定**</font>我们想要**提供给后代组件的数据 / 方法**</font>。在这个例子中，就是 `<google-map>` 内部的 `getMap` 方法：

```js
provide: function () {
  return {
    getMap: this.getMap
  }
}
```

然后在任何后代组件里，我们都可以<font color=FF0000>使用<font size=5> `inject`</font> </font>选项来<font color=FF0000><font size=5>**接收**</font></font>指定的<font color=FF0000>我们想要添加在这个实例上的 property</font>：

```js
inject: ['getMap']
```

你可以在这里看到完整的示例。<mark>相比` $parent` 来说，这个用法可以让我们在任意后代组件中访问 getMap，而不需要暴露整个 `<google-map> `实例</mark>。这允许我们更好的持续研发该组件，而不需要担心我们可能会改变/移除一些子组件依赖的东西。同时这些组件之间的接口是始终明确定义的，就和 props 一样。

实际上，你<mark>可以把依赖注入看作一部分“大范围有效的 prop”</mark>，除了：

- 祖先组件不需要知道哪些后代组件使用它提供的 property
- 后代组件不需要知道被注入的 property 来自哪里

**程序化的事件侦听器**

现在，你已经知道了 `$emit` 的用法，它可以被 `v-on` 侦听，但是 Vue 实例同时在其事件接口中提供了其它的方法。我们可以：

- **$<font color=FF0000>on</font>(eventName, eventHandler)**：<font color=FF0000>侦听</font>一个事件
- **$<font color=FF0000>once</font>(eventName, eventHandler)**：<font color=FF0000>一次性侦听</font>一个事件
- **$<font color=FF0000>off</font>(eventName, eventHandler)** ：<font color=FF0000>停止侦听</font>一个事件

你<mark>通常不会用到这些</mark>，但是当你<mark>需要在一个组件实例上手动侦听事件时，它们是派得上用场的</mark>。它们也可以用于代码组织工具。例如，你可能经常看到这种集成一个第三方库的模式：

```js
// 一次性将这个日期选择器附加到一个输入框上
// 它会被挂载到 DOM 上。
mounted: function () {
  // Pikaday 是一个第三方日期选择器的库
  this.picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })
},
// 在组件被销毁之前，
// 也销毁这个日期选择器。
beforeDestroy: function () {
  this.picker.destroy()
}
```

<font color=FF0000>这里有两个潜在的问题：</font>

- 它需要在这个组件实例中保存这个 `picker`，如果可以的话最好只有生命周期钩子可以访问到它。这并不算严重的问题，但是它可以被视为杂物。
- 我们的建立代码独立于我们的清理代码，这使得我们比较难于程序化地清理我们建立的所有东西。

你应该通过一个程序化的侦听器解决这两个问题：

```js
mounted: function () {
  var picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })

  //调用$once()
  this.$once('hook:beforeDestroy', function () {
    picker.destroy()
  })
}
```

使用了这个策略，我甚至可以让多个输入框元素同时使用不同的 Pikaday，每个新的实例都程序化地在后期清理它自己：

```js
mounted: function () {
  this.attachDatepicker('startDateInput')
  this.attachDatepicker('endDateInput')
},
methods: {
  attachDatepicker: function (refName) {
    var picker = new Pikaday({
      field: this.$refs[refName],
      format: 'YYYY-MM-DD'
    })

    this.$once('hook:beforeDestroy', function () {
      picker.destroy()
    })
  }
}
```

#### **循环引用**

- **递归组件**
  
  <font color=FF0000>组件是可以在它们自己的模板中调用自身的</font>。不过<font color=FF0000>它们只能通过 `name` 选项来做这件事</font>：
  
  ```js
  name: 'unique-name-of-my-component'
  ```
  
  <mark>当你使用 `Vue.component` 全局注册一个组件时，这个全局的 ID 会自动设置为该组件的 `name` 选项</mark>。
  
  ```js
  Vue.component('unique-name-of-my-component', {
    // ...
  })
  ```
  
  稍有不慎，递归组件就可能导致无限循环：
  
  ```js
  name: 'stack-overflow',
  template: '<div><stack-overflow></stack-overflow></div>'
  ```
  
  类似上述的组件将会导致“max stack size exceeded”错误，所以请确保递归调用是条件性的 (例如使用一个最终会得到 `false` 的 `v-if`)。

- **组件之间的循环引用**
  
  假设你需要构建一个文件目录树，像访达或资源管理器那样的。你可能有一个 `<tree-folder>` 组件，模板是这样的：
  
  ```html
  <p>
    <span>{{ folder.name }}</span>
    <tree-folder-contents :children="folder.children"/>
  </p>
  ```
  
  还有一个 `<tree-folder-contents>` 组件，模板是这样的：
  
  ```html
  <ul>
    <li v-for="child in children">
      <tree-folder v-if="child.children" :folder="child"/>
      <span v-else>{{ child.name }}</span>
    </li>
  </ul>
  ```
  
  当你仔细观察的时候，<mark>你会发现这些组件在渲染树中互为对方的后代 和 祖先</mark>——一个悖论！当通过 `Vue.component` 全局注册组件的时候，这个悖论会被自动解开。如果你是这样做的，那么你可以跳过这里。
  
  然而，如果你使用一个模块系统依赖/导入组件，例如通过 webpack 或 Browserify，你会遇到一个错误：
  
  ```
  Failed to mount component: template or render function not defined.
  ```
  
  为了解释这里发生了什么，我们先把两个组件称为 A 和 B。模块系统发现它需要 A，但是首先 A 依赖 B，但是 B 又依赖 A，但是 A 又依赖 B，如此往复。这变成了一个循环，不知道如何不经过其中一个组件而完全解析出另一个组件。为了解决这个问题，我们需要给模块系统一个点，在那里“A *反正*是需要 B 的，但是我们不需要先解析 B。”
  
  在我们的例子中，把 `<tree-folder>` 组件设为了那个点。我们知道那个产生悖论的子组件是 `<tree-folder-contents>` 组件，所以我们会等到生命周期钩子 `beforeCreate` 时去注册它：
  
  ```js
  beforeCreate: function () {
    this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue').default
  }
  ```
  
  或者，在本地注册组件的时候，你可以使用 webpack 的异步 `import`：
  
  ```js
  components: {
    TreeFolderContents: () => import('./tree-folder-contents.vue')
  }
  ```
  
  这样问题就解决了！
  
  //todo 这里没用看懂...

#### 模板定义的替代品（两种）

- **内联模板**
  
  当 <font size=4>`inline-template`</font> 这个特殊的 attribute 出现在一个子组件上时，<font color=FF0000>这个组件将会使用<font size=4> **其** </font>里面的内容作为模板</font>，而不是将其作为被分发的内容。这使得模板的撰写工作更加灵活。
  
  ```html
  <my-component inline-template>
    <div>
      <p>These are compiled as the component's own template.</p>
      <p>Not parent's transclusion content.</p>
    </div>
  </my-component>
  ```
  
  内联模板需要定义在 Vue 所属的 DOM 元素内。

- **X-Template**
  
  另一个定义模板的方式是<font color=FF0000>在一个 `<script>` 元素中，并为其带上 `text/x-template` 的类型</font>，<font color=FF0000>然后通过一个 id 将模板引用过去</font>。例如：
  
  ```html
  <script type="text/x-template" id="hello-world-template">
    <p>Hello hello hello</p>
  </script>
  
  <script>
      Vue.component('hello-world', {
        template: '#hello-world-template'
      })
  </script>
  ```
  
  <mark>x-template 需要定义在 Vue 所属的 DOM 元素外</mark>（如上示例）。

#### **控制更新**

感谢 <font color=FF0000>Vue 的响应式系统</font>，<font color=FF0000>它始终知道何时进行更新</font> (如果你用对了的话)。不过还是有一些边界情况，你想要强制更新，尽管表面上看响应式的数据没有发生改变。也有一些情况是你想阻止不必要的更新。

- **强制更新**
  
  你可能还没有留意到数组或对象的变更检测注意事项，或者你可能依赖了一个未被 Vue 的响应式系统追踪的状态。
  
  然而，如果你已经做到了上述的事项仍然发现在极少数的情况下需要手动强制更新，那么你可以通过 `$forceUpdate` 来做这件事。

- **通过 v-once 创建低开销的静态组件**
  
  渲染普通的 HTML 元素在 Vue 中是非常快速的，但有的时候你可能有一个组件，这个组件包含了**大量**静态内容。在这种情况下，你可以在根元素上添加 `v-once` attribute 以确保这些内容只计算一次然后缓存起来，就像这样：
  
  ```js
  Vue.component('terms-of-service', {
    template: `
      <div v-once>
        <h1>Terms of Service</h1>
        ... a lot of static content ...
      </div>
    `
  })
  ```

#### 进入/离开 & 列表过渡

**概述**

Vue 在插入、更新或者移除 DOM （<font color=FF0000>即：修改DOM</font>）时，<font color=FF0000>提供多种不同方式的应用过渡效果</font>。包括以下工具：

- 在 CSS 过渡和动画中自动应用 class
- 可以配合使用第三方 CSS 动画库，如 Animate.css
- 在过渡钩子函数中使用 JavaScript 直接操作 DOM
- 可以配合使用第三方 JavaScript 动画库，如 Velocity.js

**单元素/组件的过渡**

Vue 提供了<font size=5> `transition`</font> （过渡）的封装组件，在下列情形中，可以给任何元素和组件添加进入 / 离开过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

示例：

```html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>

<script>
    new Vue({
      el: '#demo',
      data: {
        show: true
      }
    })
</script>

<style type="text/css">
    .fade-enter-active, .fade-leave-active {
      transition: opacity .5s;
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
      opacity: 0;
    }
</style>
```

**当插入或删除<font color=FF0000>包含在 `transition` 组件中的元素</font>时，Vue 将会做以下处理：**

1. <font color=FF0000>自动嗅探</font>目标元素<font color=FF0000>是否应用了 CSS 过渡或动画</font>，如果是，在恰当的时机添加 / 删除 CSS 类名。
2. 如果过渡组件提供了 JavaScript 钩子函数，这些钩子函数将在恰当的时机被调用。
3. 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 `nextTick` 概念不同)

##### 过渡的类名

<font color=FF0000>在进入/离开的过渡中，会有 <font size=5>6</font> 个 class 切换</font>。

1. **v-enter：**定义<font color=FF0000>进入过渡的**开始**状态</font>。<mark>在元素被插入之前生效，在元素被插入之后的下一帧移除</mark>。
2. **v-enter-active：**定义<font color=FF0000>进入过渡**生效时**的状态</font>。<font color=FF0000>在整个进入过渡的阶段中应用</font>，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
3. **v-enter-to（2.1.8 版及以上）：**定义<font color=FF0000>进入过渡的**结束**状态</font>。在元素被插入之后下一帧生效 (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。
4. **v-leave：**定义<font color=FF0000>离开过渡的开始状态</font>。在离开过渡被触发时立刻生效，下一帧被移除。
5. **v-leave-active：**定义<font color=FF0000>离开过渡**生效时**的状态</font>。<font color=FF0000>在整个离开过渡的阶段中应用</font>，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
6. **v-leave-to（2.1.8 版及以上）：**定义<font color=FF0000>离开过渡的**结束**状态</font>。在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，在过渡/动画完成之后移除。

<img src="https://i.loli.net/2020/09/03/l57omcAKrqJVjLP.png" style="zoom:55%;" />

对于这些在过渡中切换的类名来说，<font color=FF0000>如果你使用一个没有名字的 `<transition>`，则 `v-` 是这些类名的默认前缀</font>。如果你使用了 `<transition name="my-transition">`，那么 `v-enter` 会替换为 `my-transition-enter`。

<font color=FF0000>**`v-enter-active` 和 `v-leave-active`** 可以控制进入 / 离开过渡的不同的缓和曲线</font>

**CSS 过渡**

<font color=FF0000>**常用的过渡都是使用 CSS 过渡**</font>

下面是一个简单例子：

```html
<div id="example-1">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition name="slide-fade">
    <p v-if="show">hello</p>
  </transition>
</div>

<script>
    new Vue({
      el: '#example-1',
      data: {
        show: true
      }
    })
</script>

<style>
/* 可以设置不同的进入和离开动画 */
/* 设置持续时间和动画函数 */
    .slide-fade-enter-active {
      transition: all .3s ease;
    }
    .slide-fade-leave-active {
      transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }
    .slide-fade-enter, .slide-fade-leave-to
    /* .slide-fade-leave-active for below version 2.1.8 */ {
      transform: translateX(10px);
      opacity: 0;
    }
<style>
```

**CSS 动画**

<mark>CSS 动画用法同 CSS 过渡</mark>，<font color=FF0000>**区别是**</font><mark>在动画中 `v-enter` 类名在<font color=FF0000>节点插入 DOM 后不会立即删除</font>，而是在 `animationend` 事件触发时删除</mark>。

示例：(省略了兼容性前缀)

```html
<div id="example-2">
  <button @click="show = !show">Toggle show</button>
  <transition name="bounce">
    <p v-if="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
  </transition>
</div>

<script>
    new Vue({
      el: '#example-2',
      data: {
        show: true
      }
    })
</script>

<style>
    .bounce-enter-active {
      animation: bounce-in .5s;
    }
    .bounce-leave-active {
      animation: bounce-in .5s reverse;
    }
    @keyframes bounce-in {
      0% {
        transform: scale(0);
      }
      50% {
        transform: scale(1.5);
      }
      100% {
        transform: scale(1);
      }
    }
</style>
```

##### **自定义过渡的类名**

我们<font color=FF0000>可以通过以下 attribute 来自定义过渡类名</font>：

- **enter-class**
- **enter-active-class**
- **enter-to-class** (2.1.8+)
- **leave-class**
- **leave-active-class**
- **leave-to-class** (2.1.8+)

<font color=FF0000>他们的优先级高于普通的类名</font>（和6个过渡类名一样），这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 [Animate.css](https://daneden.github.io/animate.css/) 结合使用十分有用。

示例：

```html
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">

<div id="example-3">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>

<script>
    new Vue({
      el: '#example-3',
      data: {
        show: true
      }
    })
</script>
```

##### **同时使用过渡和动画**

<font color=FF0000>Vue 为了知道过渡的完成，**必须设置相应的事件监听器**</font>。<font color=FF0000>它可以是 <font size=4>`transitionend` </font>或<font size=4> `animationend`</font></font>，这取决于给元素应用的 CSS 规则。如果你使用其中任何一种，Vue 能自动识别类型并设置监听。

但是，在一些场景中，你<font color=FF0000>需要给同一个元素同时设置两种过渡动效</font>，比如<mark> `animation` 很快的被触发并完成了，而 `transition` 效果还没结束</mark>。在这种情况中，你就<font color=FF0000>需要使用 `type` attribute 并设置 `animation` 或 `transition` 来明确声明你需要 Vue 监听的类型</font>。

##### **显性的过渡持续时间**（2.2.0 新增）

在很多情况下，Vue 可以自动得出过渡效果的完成时机。默认情况下，Vue 会等待其在过渡效果的根元素的第一个 `transitionend` 或 `animationend` 事件。然而也可以不这样设定——比如，我们可以拥有一个精心编排的一系列过渡效果，其中一些嵌套的内部元素相比于过渡效果的根元素有延迟的或更长的过渡效果。

在这种情况下你可以用 `<transition>` 组件上的 <font size=5>`duration`</font> prop <font color=FF0000>定制一个显性的过渡持续时间 (以毫秒计)</font>：

```html
<transition :duration="1000">...</transition>
```

你<font color=FF0000>也可以定制进入和移出的持续时间</font>：

```html
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

##### 初始渲染的过渡

可以通过 `appear` attribute <font color=FF0000>设置节点在**初始渲染**的过渡</font>

```html
<transition appear>
  <!-- ... -->
</transition>
```

这里默认和进入/离开过渡一样，同样也可以自定义 CSS 类名。

```html
<transition
  appear
  appear-class="custom-appear-class"
  appear-to-class="custom-appear-to-class" (2.1.8+)
  appear-active-class="custom-appear-active-class"
>
  <!-- ... -->
</transition>
```

##### 多个元素的过渡

我们之后讨论多个组件的过渡，对于原生标签可以使用 v-if / v-else。最常见的多标签过渡是一个列表和描述这个列表为空消息的元素：

```html
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Sorry, no items found.</p>
</transition>
```

可以这样使用，但是<font color=FF0000>有一点需要注意</font>：

当有**相同标签名**的元素切换时，需要通过 `key` attribute 设置唯一的值来标记以让 Vue 区分它们，否则 Vue 为了效率只会替换相同标签内部的内容。即使在技术上没有必要，**给在 `<transition>` 组件中的多个元素设置 key 是一个更好的实践。**

示例：

```html
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
```

##### 多个组件的过渡

多个组件的过渡简单很多 - 我们不需要使用 key attribute。相反，我们只需要<font color=FF0000>使用动态组件</font>：

```html
<transition name="component-fade" mode="out-in">
  <component v-bind:is="view"></component>
</transition>

<script>
    new Vue({
      el: '#transition-components-demo',
      data: {
        view: 'v-a'
      },
      components: {
        'v-a': {
          template: '<div>Component A</div>'
        },
        'v-b': {
          template: '<div>Component B</div>'
        }
      }
    })
</script>

<style>
    .component-fade-enter-active, .component-fade-leave-active {
      transition: opacity .3s ease;
    }
    .component-fade-enter, .component-fade-leave-to
    /* .component-fade-leave-active for below version 2.1.8 */ {
      opacity: 0;
    }
</style>
```

##### 列表过渡

目前为止，关于过渡我们已经讲到：

- 单个节点
- 同一时间渲染多个节点中的一个

那么怎么同时渲染整个列表，比如使用 `v-for`？在这种场景中，使用 `<transition-group>` 组件。在我们深入例子之前，先了解关于这个组件的几个特点：

- 不同于 `<transition>`，它会以一个真实元素呈现：默认为一个 `<span>`。你也可以通过 `tag` attribute 更换为其他元素。
- 过渡模式不可用，因为我们不再相互切换特有的元素。
- 内部元素**总是需要**提供唯一的 `key` attribute 值。
- CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身。

**列表的排序过渡**

`<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，还可以改变定位。要使用这个新功能只需了解新增的 **`v-move` class**，它会在元素的改变定位的过程中应用。<mark>像之前的类名一样，可以通过 `name` attribute 来自定义前缀，也可以通过 `move-class` attribute 手动设置</mark>。

`v-move` 对于设置过渡的切换时机和过渡曲线非常有用

##### 可复用的过渡

过渡可以通过 Vue 的组件系统<font color=FF0000>实现复用</font>。要创建一个可复用过渡组件，你<font color=FF0000>需要做的就是将 `<transition>` 或者 `<transition-group>` 作为根组件，然后将任何子组件放置在其中就可以了</font>。

##### 动态过渡

在 Vue 中即使是过渡也是数据驱动的！动态过渡最基本的例子是通过 `name` attribute 来绑定动态值。

```html
<transition v-bind:name="transitionName">
  <!-- ... -->
</transition>
```

当你想用 Vue 的过渡系统来定义的 CSS 过渡/动画在不同过渡间切换会非常有用。

所有过渡 attribute 都可以动态绑定，但我们不仅仅只有 attribute 可以利用，还可以通过事件钩子获取上下文中的所有数据，因为事件钩子都是方法。这意味着，根据组件的状态不同，你的 JavaScript 过渡会有不同的表现

##### 状态过渡

Vue 的过渡系统提供了非常多简单的方法设置进入、离开和列表的动效。那么<font color=FF0000>对于数据元素本身的动效</font>呢，比如：

- 数字和运算
- 颜色的显示
- SVG 节点的位置
- 元素的大小和其他的 property

<mark>这些数据要么本身就以数值形式存储，要么可以转换为数值</mark>。有了这些数值后，我们就可以结合 Vue 的响应式和组件系统，使用第三方库来实现切换元素的过渡状态。

**把过渡放到组件里**

管理太多的状态过渡会很快的增加 Vue 实例或者组件的复杂性，幸好很多的动画可以提取到专用的子组件。

#### 混入

混入 (mixin) 提供了一种非常灵活的方式，来<font color=FF0000>分发 Vue 组件中的可复用功能</font>。<font color=FF0000>一个混入对象可以包含**任意组件选项**</font>。当<font color=FF0FFF>组件</font> <font color=FF0000>使用</font> <font color=0000FF>混入对象</font>时，**所有**<font color=FF0000>混入对象</font>的<font color=0000FF>**选项**</font>将被“混合”进入该组件本身的选项。

示例：

```js
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"
```

##### 选项合并

当 <font color=FF0000>组件</font> 和 <font color=FF0000>混入对象</font> 含<font color=FF0000>有同名选项</font>时，<font color=FF0000>这些选项将以恰当的方式进行“合并”</font>。

比如，<font color=FF0000>**数据对象**在内部会进行**递归合并**</font>，并<font color=FF0000>在发生**冲突时以组件数据优先**</font>。

```js
var mixin = {
  data: function () {  //有一个data函数
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {  //还有一个data函数
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }（组件优先）
  }
})
```

同名钩子函数将合并为一个数组，因此都将被调用。另外，<font color=FF0000>**混入对象的钩子**</font>将在<font color=000FFF>组件自身钩子</font><font color=FF0FFF>**之前**</font>调用。

```js
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```

值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

注意：`Vue.extend()` 也使用同样的策略进行合并。

**全局混入**

<font color=FF0000>混入也可以进行**全局注册**</font>。使用时格外小心！<font color=FF0000>一旦使用全局混入，它将影响**每一个**之后创建的 Vue 实例</font>。使用恰当时，这可以用来为自定义选项注入处理逻辑。

```js
// 为自定义的选项 'myOption' 注入一个处理器。
// 使用Vue.mixin以全局混入？
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```

##### 自定义选项合并策略

自定义选项将使用默认策略，即简单地覆盖已有值。<mark>如果想让<font color=FF0000>自定义选项以自定义逻辑合并</font>，可以向 `Vue.config.optionMergeStrategies` 添加一个函数</mark>：

```js
Vue.config.optionMergeStrategies.myOption = function (toVal, fromVal) {
  // 返回合并后的值
}
```

对于多数值为对象的选项，可以使用与 `methods` 相同的合并策略：

```js
var strategies = Vue.config.optionMergeStrategies
strategies.myOption = strategies.methods
```

#### 自定义指令（使用directive）

除了核心功能默认内置的指令 (`v-model` 和 `v-show`)，<font color=FF0000>Vue 也允许注册自定义指令</font>。注意，在 Vue2.0 中，代码复用和抽象的主要形式是组件。然而，<mark>有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令</mark>。

<img src="https://i.loli.net/2020/09/03/6GCYNlw29JhMdpL.png" style="zoom: 50%;" />

当页面加载时，该元素将获得焦点。事实上，只要你在打开这个页面后还没点击过任何内容，这个输入框就应当还是处于聚焦状态。现在让我们用指令来实现这个功能：

<font color=FF0000>注册全局指令，示例：</font>

```js
// 注册一个全局自定义指令 `v-focus`（注意：下面名字的是focus，而注册的是v-focus）
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

如果<font color=FF0000>想注册局部指令</font>，组件中也接受一个 `directives` 的选项：

```js
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

然后你可以在模板中任何元素上使用新的 `v-focus` property，如下：

```html
<input v-focus>
```

##### 钩子函数

一个指令定义对象可以<font color=FF0000>提供如下几个钩子函数</font> (均为可选)：

- **bind**：<font color=FF0000>只调用一次</font>，<font color=FF0000>指令第一次绑定到元素时调用</font>。在这里可以进行一次性的初始化设置。
- **inserted**：<font color=FF0000>被绑定元素插入父节点时调用</font> (仅保证父节点存在，但不一定已被插入文档中)。
- **update**：<font color=FF0000>所在组件的 VNode 更新时调用</font>，**但是<font color=FF0000>可能发生在其 子VNode 更新之前</font>**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。
- **componentUpdated**：指令所在<font color=FF0000>组件的 VNode 及其 子VNode</font> <font color=FF0000>**全部更新后**</font>调用。
- **unbind**：<font color=FF0000>只调用一次</font>，<font color=FF0000>**指令与元素解绑**时调用</font>。

**钩子函数参数（el、binding、vnode 和 oldVnode）**

指令钩子函数会被传入以下参数：

- **el**：<font color=FF0000>指令所绑定的元素</font>，可以用来直接操作 DOM。
- **binding**：<font color=FF0000>一个对象</font>，包含以下 property：
  - **name**：<font color=FF0000>指令名</font>，不包括 `v-` 前缀。
  - **value**：<font color=FF0000>指令的绑定值</font>，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - **oldValue**：<font color=FF0000>指令绑定的前一个值</font>，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - **expression**：<font color=FF0000>**字符串形式**的指令表达式</font>。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - **arg**：<font color=FF0000>传给指令的参数</font>，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - **modifiers**：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
- **vnode**：<font color=FF0000>Vue 编译生成的虚拟节点</font>。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
- **oldVnode**：<font color=FF0000>上一个虚拟节点</font>，仅在 `update` 和 `componentUpdated` 钩子中可用。

<font color=FF0000>**注意**</font>：<font color=FF0000>除了 `el` 之外，其它参数都应该是**只读的**</font>，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 [`dataset`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/dataset) 来进行。

示例：

```html
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>

<script>
    Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
</script>
```

**动态指令参数**

<font color=FF0000>指令的参数可以是**动态**的</font>。例如，在 `v-mydirective:[argument]="value"` 中，<font color=FF0000>`argument` 参数可以根据组件实例数据进行更新</font>！这使得自定义指令可以在应用中被灵活使用。

**函数简写**

在很多时候，你可能想在 `bind` 和 `update` 时触发相同行为，而不关心其它的钩子。比如这样写：

```js
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

**对象字面量**

如果指令需要多个值，可以传入一个 JavaScript 对象字面量。记住，指令函数能够接受所有合法的 JavaScript 表达式。

```html
<div v-demo="{ color: 'white', text: 'hello!' }"></div>

<script>
    Vue.directive('demo', function (el, binding) {
      console.log(binding.value.color) // => "white"
      console.log(binding.value.text)  // => "hello!"
    })
</script>  
```

补充：可以看看如下视频，[v-for？v-if？如何在Vue中自己写一个指令？](https://www.bilibili.com/video/BV1dK4y1h7Xj)

#### 渲染函数 & JSX

**基础**

<font color=FF0000>Vue 推荐在绝大多数情况下使用模板来创建你的 HTML</font>。然而在一些场景中，你真的需要 JavaScript 的完全编程的能力。这时你可以用**渲染函数**，它比模板更接近编译器。

**示例：** render函数

```js
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      this.$slots.default // 子节点数组
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

**节点、树以及虚拟 DOM**

在深入渲染函数之前，了解一些浏览器的工作原理是很重要的。以下面这段 HTML 为例：

```html
<div>
  <h1>My title</h1>
  Some text content
  <!-- TODO: Add tagline -->
</div>
```

<mark>当浏览器读到这些代码时，它会建立一个“DOM 节点”树来保持追踪所有内容</mark>，如同你会画一张家谱树来追踪家庭成员的发展一样。

上述 HTML 对应的 DOM 节点树如下图所示：

<img src="https://i.loli.net/2020/09/03/wNJg7vfEa4IpFds.png" style="zoom: 42%;" />

每个元素都是一个节点。每段文字也是一个节点。甚至注释也都是节点。一个节点就是页面的一个部分。就像家谱树一样，每个节点都可以有孩子节点 (也就是说每个部分可以包含其它的一些部分)。

高效地更新所有这些节点会是比较困难的，不过所幸你不必手动完成这个工作。你只需要告诉 Vue 你希望页面上的 HTML 是什么，这可以是在一个模板里：

```html
<h1>{{ blogTitle }}</h1>
```

<font color=FF0000>或者一个渲染函数里</font>：

```js
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```

在这两种情况下，Vue 都会自动保持页面的更新，即便 `blogTitle` 发生了改变。

**虚拟 DOM**

<font color=FF0000>Vue 通过建立一个**虚拟 DOM** 来追踪自己要如何改变真实 DOM</font>。请仔细看这行代码：

```js
return createElement('h1', this.blogTitle)
```

`createElement` 到底会返回什么呢？其实不是一个实际的 DOM 元素。它更准确的名字可能是 `createNodeDescription`，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。<font color=FF0000>我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“**VNode**”</font>。<mark>“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼</mark>。

**createElement 参数**

接下来你需要熟悉的是如何在 `createElement` 函数中使用模板中的那些功能。这里是 `createElement` 接受的参数：

```js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  'div',

  // {Object}
  // 一个与模板中 attribute 对应的数据对象。可选。
  {
    // (详情见下一节)
  },

  // {String | Array}
  // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

**深入数据对象**

有一点要注意：正如 `v-bind:class` 和 `v-bind:style` 在模板语法中会被特别对待一样，它们在 VNode 数据对象中也有对应的顶层字段。该对象也允许你绑定普通的 HTML attribute，也允许绑定如 `innerHTML` 这样的 DOM property (这会覆盖 `v-html` 指令)。

```js
{
  // 与 `v-bind:class` 的 API 相同，
  // 接受一个字符串、对象或字符串和对象组成的数组
  'class': {
    foo: true,
    bar: false
  },
  // 与 `v-bind:style` 的 API 相同，
  // 接受一个字符串、对象，或对象组成的数组
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 普通的 HTML attribute
  attrs: {
    id: 'foo'
  },
  // 组件 prop
  props: {
    myProp: 'bar'
  },
  // DOM property
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器在 `on` 内，
  // 但不再支持如 `v-on:keyup.enter` 这样的修饰器。
  // 需要在处理函数中手动检查 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅用于组件，用于监听原生事件，而不是组件内部使用
  // `vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
  // 赋值，因为 Vue 已经自动为你进行了同步。
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // 作用域插槽的格式为
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 如果组件是其它组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
  // 其它特殊顶层 property
  key: 'myKey',
  ref: 'myRef',
  // 如果你在渲染函数中给多个元素都应用了相同的 ref 名，
  // 那么 `$refs.myRef` 会变成一个数组。
  refInFor: true
}
```

补充：这里可以看看 [Vue中的Render函数原来是这个意思！](https://www.bilibili.com/video/BV1364y1F7H4)

补充：

v-text：将数据解析为纯文本

//todo

#### 模版占位符template

**需求：**下图div用v-for做了列表循环，现在想要span也一起循环，应该怎么做？

```html
<div id="app">
    <div>
        <div v-for="(item, index) in list" :key="item.id">{{item.text}}--{{index}}</div>
        <span>{{item.text}}</span>
    </div>
</div>
```

**有3种方法可以实现**

- 直接用v-for对span也循环一次
  
  ```html
  <div id="app">
      <div>
          <div v-for="(item, index) in list" :key="item.id">{{item.text}}--{{index}}</div>
          <span v-for="(item, index) in list" :key="item.id">{{item.text}}</span>
      </div>
  </div>
  ```

- 在div和span外面包裹一个div，给这个div加循环
  
  ```html
  <div id="app">
      <div v-for="(item, index) in list" :key="item.id">
          <div>{{item.text}}--{{index}}</div>
          <span>{{item.text}}</span>
      </div>
  </div>
  ```

- 不想额外增加一个div，此时应该使用template来实现<font color=FF0000>（推荐）</font>
  
  ```html
  <div id="app">
      <template v-for="(item, index) in list" :key="item.id">
          <div>{{item.text}}--{{index}}</div>
          <span>{{item.text}}</span>
      </template>
  </div>
  ```

**总结：**<font color=FF0000>template的作用是模板占位符，可帮助我们包裹元素，但在循环过程当中，template不会被渲染到页面上。</font>

摘自：[vue中template的作用及使用](https://www.cnblogs.com/tu-0718/p/11177236.html)

#### 非props

非Prop特性：指的是一个未被组件注册的特性。<font color=FF0000>当组件接收了一个非Prop特性时，**该特性会被添加到这个组件的根元素上**。</font>

##### **props特性 与 非props特性的区别**

- **props特性：**
  
  - <font color=0000FF>可以传递给子组件使用。</font>
  - <font color=FF0000>不会在dom元素中显示出来。</font>
  - <font color=fuchsia>不会替换已有的特性。</font>

- **非props特性：**
  
  - <font color=0000FF>不可以传递给子组件。</font>
  
  - <font color=FF0000>会在dom元素上显示出来。</font>
  
  - <font color=fuchsia>会替换掉已有的特性</font>。只有class和style特性才会与非prop特性合并。

摘自：[复习之props特性与非props特性](https://zhuanlan.zhihu.com/p/170645692)

#### vue import路径中的@

以根目录的方式定义相对路径，一般为src文件（ 在 <font color=FF0000>webpack.base.conf.js</font> 文件中配置，示例如下：）

```js
module.exports = {
  resolve: {
    // 在导入语句没带文件后缀时，webpack会自动按照顺序添加后缀名查找
    extensions: ['.js', '.vue', '.json'],
    // 配置别名
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      // 将项目根目录中，src的路径配置为别名@
      '@': resolve('src'),
    }
  }
}
```

摘自：[Vue 导入文件import、路径@和.的区别](https://www.cnblogs.com/taohuaya/p/10651902.html)

#### Vue.extend(options)

- **参数**：{Object} options
- **用法**：使用<font color=FF0000>基础 Vue 构造器，创建一个“子类”</font>。参数是一个包含组件选项的对象。

Vue.extend() 创建的是一个组件构造器，而不是一个具体的组件实例。所以他不能直接在new Vue中这样使用： new Vue({components: foo})；最终还是要通过Vue.components注册才可以使用的。 

**Vue.extend 和 Vue.component、component 的区别**

1. Vue.component、component两者都是需要先进行组件注册，然后在 template 中使用注册的标签名来实现组件的使用。Vue.extend 则是编程式的写法（<font color=FF0000>动态创建组件</font>）
2. 关于组件的显示与否，需要在父组件中传入一个状态来控制 或者 在组件外部用 v-if / v-show 来实现控制，而 Vue.extend 的显示与否是手动的去做组件的挂载和销毁。
3. <font color=FF0000>Vue.component component 在组件中需要使用 slot 等自定义UI时更加灵活，而 Vue.extend 由于没有 template的使用，没有slot 都是通过 props 来控制UI，更加局限一些。</font>
   摘自：[Vue.extend 编程式插入组件](https://juejin.cn/post/6844903998672076813)

#### Vue.use()

语法：

```js
Vue.use(MyPlugin, { someOption: true }) //也可以传入一个可选的选项对象：
```

- 如果插件是一个对象，必须提供 install 方法。
- **如果插件是一个函数，它会被作为 install 方法**。install 方法调用时，`会将 Vue 作为参数传入`。
- Vue.use(plugin)调用之后，插件的install方法就会默认接受到一个参数，这个参数就是Vue（原理部分会将）

Vue.use() 方法需要在调用 `new Vue()` 之前被调用。

通过全局方法 Vue.use() 使用插件。它需要<font color=FF0000>在你调用 new Vue() 启动应用之前完成</font>：

```vue
// 调用 MyPlugin.install(Vue)
Vue.use(MyPlugin)

new Vue({
  // ...组件选项
})
```

Vue.use <font color=FF0000>会自动阻止多次注册相同插件，届时即使多次调用也只会注册一次该插件。</font>

**补充：**（摘自：[vue.use()方法从源码到使用](https://juejin.cn/post/6844903842035793928)）

我们发现 <font color=FF0000>Vue.use() 的注册本质上就是执行了一个 install 方法，install 里的内容由开发者自己定义</font>，通俗讲就是一个钩子可能更贴近语义化而已。

在 install 里我们可以拿到 Vue 那么和 Vue 相关的周边工作都可以考虑放在 Vue.use() 方法里，比如：

- directive注册
- mixin注册
- filters注册
- components注册
- prototype挂载
- ...

另外：如果该插件 / 组件不提供`use()`方法，需要主动注册，示例如下：

```js
import VeLine from 'v-charts/lib/line.common'

Vue.component('ve-line', VeLine)
```

#### Vue.directive函数

##### 语法

```js
Vue.directive( id, definition] )
```

##### 参数

- `{string} id`
- `{Function | Object} [definition]`

**用法**：注册或获取全局指令。

可以参考vue官方文档的[自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html) 做进一步补充

#### provide / inject

**类型**：

- **provide**：`Object | () => Object`
- **inject**：`Array<string> | { [key: string]: string | Symbol | Object }`

**功能：**允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在其上下游关系成立的时间里始终生效。

代码示例如下：

```js
// 父级组件提供 'foo'
var Provider = {
  provide: {
    foo: 'bar'
  },
  // ...
}

// 子组件注入 'foo'
var Child = {
  inject: ['foo'],
  created () {
    console.log(this.foo) // => "bar"
  }
  // ...
}
```

#### 补充：（摘自：[聊聊 Vue 中 provide/inject 的应用](https://juejin.cn/post/6844903989935341581)）

- **解决痛点：**
  
  使用 $root 依然能够取到根节点，那么我们何必使用 provide/inject 呢？
  
  在实际开发中，一个项目常常有多人开发，每个人有可能需要不同的全局变量，如果所有人的全局变量都统一定义在根组件，势必会引起变量冲突等问题。
  
  使用 provide/inject 不同模块的入口组件传给各自的后代组件可以完美的解决该问题。

- <font color=FF0000>**慎用provide / inject**</font>
  
  Vuex 和 provide/inject 最大的区别在于：Vuex 中的全局状态的每次修改是可以追踪回溯的，而<font color=FF0000> provide/inject 中变量的修改是无法控制的，换句话说，你不知道是哪个组件修改了这个全局状态。</font>
  
  <font color=FF0000>Vue 的设计理念借鉴了 React 中的单向数据流原则</font>（虽然有 sync 这种破坏单向数据流的家伙），而 <font color=FF0000>provide/inject 明显破坏了单向数据流原则</font>。试想，<font color=FF0000>如果有多个后代组件同时依赖于一个祖先组件提供的状态，那么只要有一个组件修改了该状态，那么所有组件都会受到影响。这一方面增加了耦合度，另一方面，使得数据变化不可控</font>。如果在多人协作开发中，这将成为一个噩梦。
  
  在这里，我总结了两条条使用 provide/inject 做全局状态管理的原则：
  
  1. 多人协作时，做好作用域隔离
  2. 尽量使用一次性数据作为全局状态
  
  看起来，使用 provide/inject 做全局状态管理好像很危险，那么<font color=FF0000>有没有 provide/inject 更好的使用方式呢？当然有，那就是使用 provide/inject 编写组件。而使用 provide/inject 做组件开发，是 Vue 官方文档中提倡的一种做法。</font>

#### \$on & \$emit

**\$on & ​\$emit** 解决的问题分别是：事件的定义和消费。

**举个例子：**函数中的 <font color=FF0000>**this.$emit("start", args)**  触发了一个自定义事件 "start"</font>。然后<font color=0000FF>监听器 **this.$on('start',function(...) )** 监听到这个自定义事件 **"start"** 的触发，执行了监听器里面的函数。并接收了 $emit 传过来的参数 （args）</font>

- **$on** 是用来在<font color=FF0000>**监听**(注册)自定义事件</font>的 **（接收数据）**，<font color=FF0000>同时：可以为一个事件（event）绑定多个方法（callback）</font>
  
  **语法：**
  
  ```vue
  vm.$on(event, callback)
  ```
  
  **参数：**
  
  - **event  {string | Array}：**自定义事件的名称，<font color=FF0000>可以使用数组的方式多次注册</font>。数组方式必须在2.2.0+中才支持（与上面的为一个事件（event）绑定多个方法（callback）对应）
  
  - **callback  {Function}：**自定义事件触发后，所执行的方法、函数
  
  总结：$on可以定义多个事件（使用Array），也可以为同一个事件绑定多个处理函数（callback）
  
  **示例：**
  
  ```vue
  vm.$on('myEvent', function(data) {
      console.log(data);
  });
  ```

- **$emit** 是<font color=FF0000>手动**触发**当前实例上的一个指定事件</font> （发送数据）
  
  **语法：**
  
  ```vue
  vm.$emit(eventName, [...args])
  ```
  
  **参数：**
  
  - **eventName {string}：** 需要触发的事件名称
  - **[...args]：** 传递的参数，多个参数用数组，单个参数就可以直接用参数本身的格式
  
  **示例：**
  
  ```vue
  vm.$emit('myEvent', 'happy');
  ```

示例如下：

```html
<html>
  <head>
    <title>$emit 和 $on</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <!--使用$emit-->
      <button @click="boost">触发事件</button>
    </div>
    <script>
      new Vue({
        el: '#root',
        data() {
          return {
            message: 'hello vue'
          }
        },
        created() {
          /**声明$on*/
          this.$on('my_events', this.handleEvents)
        },
        methods: {
          handleEvents(e) {
            console.log(this.message, e)
          },
          boost() {
            /**声明$emit*/
            this.$emit('my_events', 'my params')            
          }
        }
      })
    </script>
  </body>
</html>
```

#### 补充：其他事件处理侦听器

- 通过 `$on(eventName, eventHandler)` 侦听一个事件
- 通过 `$once(eventName, eventHandler)` <font color=FF0000>一次性侦听</font>一个事件
- 通过 `$off(eventName, eventHandler)` <font color=FF0000>停止侦听</font>一个事件

#### vm.$mount

```vue
vm.$mount( elementOrSelector] )
```

##### **参数**

- {Element | string} [elementOrSelector]
- {boolean} [hydrating]

<font color=FF0000>**返回值：**vm - 实例自身</font>

##### 用法

<font color=FF0000>如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 vm.$mount() 手动地挂载一个未挂载的实例。</font>

如果没有提供 elementOrSelector 参数，模板将被渲染为文档之外的的元素，并且你必须使用原生 DOM API 把它插入文档中。

这个方法返回实例自身，因而可以链式调用其它实例方法。

##### 示例

```js
var MyComponent = Vue.extend({
  template: '<div>Hello!</div>'
})

// 创建并挂载到 #app (会替换 #app)
new MyComponent().$mount('#app')

// 同上
new MyComponent({ el: '#app' })

// 或者，在文档之外渲染并且随后挂载
var component = new MyComponent().$mount()
document.getElementById('app').appendChild(component.$el)
```

#### render

- **类型：**(createElement: () => VNode) => VNode

- **详细**：
  
  <font color=FF0000>**字符串模板的代替方案**</font>，<mark>允许你发挥 JavaScript 最大的编程能力。该渲染函数接收一个 `createElement` 方法作为第一个参数用来创建 `VNode`</mark>。
  
  如果组件是一个函数组件，渲染函数还会接收一个额外的 `context` 参数，为没有实例的函数组件提供上下文信息。

Vue 通过建立一个**虚拟 DOM** 来追踪自己要如何改变真实 DOM。请仔细看这行代码：

//....

**示例如下：**

```js
function anonymous() { 
  with(this){
    return _h('div',{attrs:{"id":"app"}},["\n  "+_s(message)+"\n"])} 
}
```

#### watch侦听器

侦听器的名字以被监视的对象命名，根据被监视的对象的变化自动发生变化

- 常见用法
- 绑定方法
- deep + handler
- immediate
- 绑定多个 handler
- 监听对象属性

#### 过滤器 filter

Vue.js 允许你自定义过滤器，<font color=FF0000>可被用于一些常见的文本格式化</font>。过滤器可以用在两个地方：**双花括号插值和 `v-bind` 表达式** (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号（`|`，这里类似于linux中的「管道」概念）指示：

```html
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

- 定义本地的过滤器：
  
  ```js
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
  ```

- 定义全局过滤器：
  
  ```js
  Vue.filter('capitalize', function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  })
  
  new Vue({
    // ...
  })
  ```

当全局过滤器和局部过滤器重名时，会采用局部过滤器（就近原则）

过滤器可以串联（类似于管道）：

```js
{{ message | filterA | filterB }}
```

过滤器可以是 JavaScript 函数，可以接收参数：

```js
{{ message | filterA('arg1', arg2) }}
```

这里，filterA 被定义为接收三个参数的过滤器函数。其中 message 的值作为第一个参数，普通字符串 'arg1' 作为第二个参数，表达式 arg2 的值作为第三个参数。

摘自：[Vue官方文档 - 过滤器](https://cn.vuejs.org/v2/guide/filters.html)



#### class 和 style 绑定的高级用法

```html
<div :class="['active', 'normal']">数组绑定多个class</div> <!--也可以使用'active' + ' normal'，不过使用数组更合理-->
<div :class="[{active: isActive}, 'normal']">数组包含对象绑定class</div>  <!--这里的isActive是bool-->
<div :class="[showWarning(), 'normal']">数组包含方法绑定class</div> <!--showWarning()会返回一个class名字的字符串-->
<div :style="[warning, bold]">数组绑定多个style</div> <!--这里的warning和bold是css的对象-->
<div :style="[warning, mix()]">数组包含方法绑定style</div>
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }">style多重值</div> <!--会优先取得最后一个值，不兼容的情况下，会取倒数第二个值；以此类推-->
```



#### checked属性

在Vue中通过checked属性值来判定radio / \<select>是否选中，示例如下：

```html
<input type='radio' :name='groupName' :checked='option.checked'>{{option.text}}
```

类似的还有value和selected。

**以下摘自官方文档：**

v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 <font color=FF0000>**value**</font> property 和 <font color=FF0000>**input** 事件</font>；
- checkbox 和 radio 使用 <font color=FF0000>**checked**</font> property 和 <font color=FF0000>**change** 事件</font>；
- select 字段将 value 作为<font color=FF0000> **prop**</font> 并将 <font color=FF0000>**change**</font> 作为事件。

摘自：[vue官方文档 -- 表单输入绑定](https://cn.vuejs.org/v2/guide/forms.html)



#### render函数

render 函数即渲染函数，它是个函数，render 函数的返回值是VNode（即：虚拟节点，也就是我们要渲染的节点）
<font color=FF0000>createElement 是 render 函数的参数，它本身也是个函数，并且有三个参数。</font>如下：

```js
createElement(tag, options, VNodes)
```

- **createElement 第一个参数<font color=FF0000>tag</font>，是<font color=FF0000>必填</font>的：可以是String | Object | Function** 
  - String，表示的是HTML 标签名
  - Object ，一个含有数据的组件选项对象
  - Function ，返回了一个含有标签名或者组件选项对象的async 函数
- **createElement 第二个参数<font color=FF0000>options</font>，是<font color=FF0000>选填</font>的：一个与模板中属性对应的数据对象** **常用的有class | style | attrs | domProps | on**
  - class：控制类名
  - style ：样式
  - attrs ：用来写正常的 html 属性 id src 等等
  - domProps :用来写原生的dom 属性
  - on：用来写原生方法
- **createElement 第三个参数<font color=FF0000>VNodes</font>，是<font color=FF0000>选填</font>的：代表子级虚拟节点 (VNodes)，由 createElement() 构建而成，正常来讲接收的是一个字符串或者一个数组，一般数组用的是比较多的**

<font color=FF0000>**将h作为createElement的别名是 Vue 生态系统中的一个通用惯例**</font>，实际上也是 JSX 所要求的。<mark>从 Vue 的 Babel 插件的3.4.0版本开始，我们会在以 ES2015 语法声明的含有 JSX 的任何方法和 getter 中 (不是函数或箭头函数中) 自动注入const h = this.$createElement，这样你就可以去掉(h)参数了</mark>。对于更早版本的插件，如果h在当前作用域中不可用，应用会抛错。

摘自：[Vue - 渲染函数render](https://juejin.cn/post/6844903919764635655)



#### Vue.nextTick( [callback, context] )

- **参数**：
  
  - {Function} [callback]
  - {Object} [context]

- **用法**：
  
  在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

摘自：[Vue.js API -- Vue.nextTick](https://cn.vuejs.org/v2/api/index.html#Vue-nextTick)



#### v-if和v-for为什么不能连用（写在一行）

由于v-for的优先级更高，会先运行；而v-if优先级较低，后运行。等v-for所有数据都运行完了，v-if再运行（去除异常情况）会造成性能浪费。



#### 一些命名规则

在vue的内部，<font color=FF0000>**\_**符号开头定义的变量是供内部私有使用的</font>，而<font color=FF0000>**$** 符号定义的变量是供用户使用的</font>，而且<font color=FF0000>用户自定义的变量不能以\_或$开头，以防止内部冲突</font>。



#### Vue template和is

- **vue template**
  
  渲染一个“元组件”为动态组件。依 `is` 的值，来决定哪个组件被渲染。

- **vue :is**
  
  预期：string | Object (组件的选项对象)
  
  用于<font color=FF0000>动态组件</font>且基于 DOM 内模板的限制来工作。

可以参考：官方文档中给的[**示例**](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-components)



#### $event

需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
<script>
// ...
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
</script>
```

摘自：[vue官方文档 - 事件处理](https://cn.vuejs.org/v2/guide/events.html)



#### 添加实例 property

//TODO

摘自：[Vue.js Cookbook - 添加实例 property](https://cn.vuejs.org/v2/cookbook/adding-instance-properties.html)

#### 深度作用选择器

如果你希望 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 >>> 操作符：

```html
<style scoped>
.a >>> .b { /* ... */ }
</style>
```

上述代码将会编译成：

```css
.a[data-v-f3f3eg9] .b { /* ... */ }
```

<font color=FF0000>有些像 Sass 之类的预处理器无法正确解析 `>>>`。这种情况下你可以使用 `/deep/` 操作符取而代之——这是一个 `>>>` 的别名，同样可以正常工作。</font>

摘自：[vue loader官方文档](https://vue-loader-v14.vuejs.org/zh-cn/features/scoped-css.html)

另外，***深度作用选择器*** 可以用作 ***样式穿透***

使用 `>>>` 提高自定义class / id的优先级

详见：[想要改变插件里组件的样式？使用样式穿透！【Vue】](https://www.bilibili.com/video/BV1Jv41117QN)



#### v-if和v-show的区别

v-if如果不满足，则对应的元素则不会在页面中渲染出来；而v-show如果不满足，对应的 元素会在页面中渲染出来，不过会加上`display: none`。

**两者的最佳实践：**

- v-if适用于结果定下来（很少改变）的情况
- v-show适用于结果经常变化的情况，这时只需要改动css（修改`display: none`）即可使其展示。

摘自：[v-if 和 v-show 的区别【Vue面试题】](https://www.bilibili.com/video/BV11y4y1h7hi)



#### v-model内置修饰符

当我们学习表单输入绑定时，我们看到 v-model 有内置修饰符—— `.trim`、`.number` 和 `.lazy`。

- `.trim`：会忽略掉输入的空格
- `.number`：会将输入的内容（str）转化为数字（num）
- `.lazy`：v-model绑定的值会开启“懒策略”，直到当前组件失焦（blur），v-model绑定的值才会变化

但是，在某些情况下，你可能还需要添加自己的自定义修饰符。

让我们创建一个示例自定义修饰符 `capitalize`，它将 `v-model` 绑定提供的字符串的第一个字母大写。



## Vue3及学习补充

#### 生命周期

![组件生命周期图示](https://cn.vuejs.org/assets/lifecycle.16e4c08e.png)

- **初始化事件 & 生命周期：**分析代码中的事件绑定和生命周期函数
- **beforeCreate：**在实例生成之前自动执行的函数
- **初始化注入 & 响应性：**分析数据和模版之间双向绑定，和依赖注入的相关内容
- **created：**在实例生成之后自动执行的函数
- **beforeMount：**在组建内容被渲染到页面之前执行；将模版（如果有）变成渲染函数之后自动执行
- **mounted：**在组件内容被渲染到页面之后自动执行
- **beforeUpdate：**当data中的数据发生变化时<font color=FF0000>立即</font>自动执行的函数。打印dom还是修改前的内容
- **updated：**当data中数据发生变化，<mark>同时页面完成更新后</mark>，自动执行的函数。打印dom是修改后的内容
- **beforeUnmount：**当组件销毁时，立即自动执行的函数。仍可以打印出dom（dom仍未被销毁）
- **unmounted：**当组件销毁时，<font color=FF0000>且dom完全被销毁</font>，自动执行的函数。不能打印出dom（dom已经被销毁）



#### $attrs

父组件使用子组件，如果在使用过程中添加新的属性，需要子组件接收，而接收可以通过 `$attrs.property-name`实现

示例如下，下面父组件向子组件传递了class属性：

```vue
template: `
	<div>
    <demo class="green"/>
	</div>
`

app.component('demo', {
	template: `
		<div :class="$attrs.class">foo</div>
	`
})
```

**官方文档解释：**

包含了父作用域中不作为组件 props 或自定义事件的 attribute 绑定和事件。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定，并且可以通过 v-bind="$attrs" 传入内部组件——这在创建高阶的组件时会非常有用。

摘自：[vue官方文档API - 实例 property - attr](https://v3.cn.vuejs.org/api/instance-properties.html#attrs)

**补充：**

$attrs其实是一个proxy对象，将父组件传给子组件的non-prop都放在里面，比如这里的例子中是

```js
Proxy {
  class: 'green'
}
```

既然$attrs是一个proxy对象，所以不仅仅在template中使用，也可以在子组件的data属性和生命周期钩子中使用



#### v-if 和 v-show

- v-if每次执行都是在执行渲染：如果v-if为false，则所对应的dom会被销毁；同时在原dom上会加上 \<!--v-if-->的注释
- v-show每次执行并非都真的渲染，如果v-show为false，对应的dom会被加上 style="display: none"



#### 一个事件绑定多个函数

在Vue中一个事件（比如@click）可以绑定多个函数，但是在调用时，函数要加上括号；如下示例：

```vue
<button @click='fn(), fn2()'>foobar</button>
```

>  👀 注：这种写法很不推荐



#### checkbox 的 vue 中的补充属性

由于checkbox对应的v-model数据类型只能是bool型，所以vue添加了 **true-value** 和 **false-value** 两个属性，使得v-model的数据类型可以是其他基础类型，也包括数组、对象和方法（不过，经过测试，似乎数组、对象和方法都会原封不动的变成字符串）；示例如下：

```vue
template: `
  <div>
    {{message}}
    <input type="checkbox" v-model="message" true-value="foo" false-value="bar" />
  </div>
`
```



## Element UI 备忘录

#### \<el-table>

- 如果要指定表格每列的「标题」和每列的「字段」的位置（左中右），可以在\<el-table-column>分别设置header-align和align为 ["left" "center" "right"] 以进行修改。

- 如果可以使用，每列的「标题」出现换行的情况，这样很影响美观；可以在\<el-table-column>中设置width="pixel number"以修改column宽度，使得该列不至于换行。示例如下：
  
  ```html
  <el-table-column :width='400'></el-table-column>
  ```
  
  当然：并不是推荐使用width，而是推荐使用<font color=FF0000>**min-width**</font>，因为min-width可实现自适应

- <font color=FF0000>通过 **Scoped slot** 可以获取到 row（当前行的信息）, column（当前列的信息）, $index（当前行的行号，从0开始） 和 store（table 内部的状态管理）的数据</font>

- **几个style属性**
  
  - **row-style**：设置的就是表格行的样式
    
    ```js
    :row-style="{height:'15px'}
    ```
  
  - **header-row-style**：设置了表头行的样式
    
    ```js
    :header-row-style="{height:'20px'}"
    ```
  
  - **cell-style**：设置表格单元格的样式
    
    ```js
    :cell-style="{padding:'0px'}"
    ```
  
  - **header-cell-style**：设置表头的单元格样式
    
    ```js
    :header-cell-style="{padding:'0px',background:'#eef1f6'}"
    ```

- <font color=FF0000>如果想要在**el-table**中使用（添加）class</font>，需要制定 Table 组件的 `row-class-name` 属性来为 Table 中的某一行添加 class，从而可以在类中添加属性
  
  **补充：**
  
  - **header-cell-class-name**：表头单元格的 className 的回调方法，也可以使用字符串为所有表头单元格设置一个固定的 className
    
    - **类型：**Function({row, column, rowIndex, columnIndex}) **/** String，分为 函数形式 和 字符串形式
    
    - **函数形式：**将headerStyle方法传递给header-cell-class-name
      
      ```html
      <el-table 
          :data="tableData[lang]" 
        class="table" 
        stripe 
        border 
        :header-cell-class-name="headerStyle"
      >
      
      <script>
      headerStyle ({row, column, rowIndex, columnIndex}) {
          return 'tableStyle'
      }
      </script>
      
      <style lang = "scss">
      .tableStyle{
        background-color: #1989fa!important;
        color:#fff;
        font-weight:400;
      }
      ```
    
    - **字符串形式：**直接将tableStyle名称赋值给header-cell-class-name
      
      ```html
      <el-table 
        :data="tableData[lang]" 
        class="table" 
        stripe 
        border 
        header-cell-class-name="tableStyle"
      >
      
      <style lang = "scss">
      .tableStyle{
        background-color: #1989fa!important;
        color:#fff;
        font-weight:400;
      }
      ```
  
  - **header-cell-style**：表头单元格的 style 的回调方法，也可以使用一个固定的 Object 为所有表头单元格设置一样的 Style。
    
    - **类型：**Function({row, column, rowIndex, columnIndex}) **/** Object 分为 函数形式 和 字符串形式
    
    - **函数形式：**将tableHeaderStyle方法传递给header-cell-style
      
      ```html
      <el-table 
        :data="tableData[lang]" 
        class="table" 
        stripe 
        border 
        :header-cell-style='tableHeaderStyle'
      >
      
      <script>
      tableHeaderStyle ({row, column, rowIndex, columnIndex}) {
          return 'background-color:#1989fa;color:#fff;font-weight:400;'
      }
      </script>
      ```
    
    - **对象形式：**直接在对象中编写样式
      
      ```html
      <el-table 
        :data="tableData[lang]" 
        class="table" 
        stripe 
        border 
        :header-cell-style="{
          'background-color': '#1989fa',
          'color': '#fff',
          'font-weight': '400'
      }">
      ```
  
  - **header-row-class-name：**表头行的className 的回调方法，也可以使用字符串为所有表头行设置一个固定的 className。
    
    类型：Function({row, rowIndex})/String 
    
    使用方式与header-cell-class-name类似，不过：<font color=FF0000>header-row-class-name是添加在tr上面的，header-cell-class-name是添加在th上面的</font>。
    
    所以<font color=FF0000>想让添加在tr上的样式显示，需要关闭element-ui中原本的th的样式，否则会被覆盖！</font>（例如背景色）
    
    <img src="https://i.loli.net/2020/11/17/LKqfCixXlEc9vRB.png" alt="DVVOaV.png" style="zoom:80%;" />
  
  - **header-row-style**：表头行的 style 的回调方法，也可以使用一个固定的 Object 为所有表头行设置一样的 Style。
    
    - 类型：Function({row, rowIndex})/Object
      
      使用方式与header-cell-style类似
  
  - **row-class-name**：行的 className 的回调方法，也可以使用字符串为所有行设置一个固定的 className。
    
    - 类型：Function({row, rowIndex})/String
      
      使用方式与header-cell-class-name类似
  
  - **row-style**：行的 style 的回调方法，也可以使用一个固定的 Object 为所有行设置一样的 Style。
    
    - 类型：Function({row, rowIndex})/Object，使用方式与header-cell-style类似
    
    - 函数形式：将tableRowStyle方法传给row-style
      
      ```html
      <el-table 
        :data="tableData[lang]" 
        class="table" 
        border 
        :row-style="tableRowStyle"
      >
      
      <script>
      // 修改table tr行的背景色
      tableRowStyle ({ row, rowIndex }) {
        return 'background-color:#ecf5ff'
      }
      </script>
      ```

- **滚动条功能**：通过设置<font color=FF0000>**max-height**</font>属性<font color=FF0000>为 Table 指定最大高度</font>。此时若表格所需的高度大于最大高度，则会显示一个滚动条。

- **自定义表头或者某一列有三种方法：**
  
  - 使用render-header方法，示例：
    
    ```html
    <el-table-column
        prop="delete"
        label="删除"
        :render-header="renderHeader"
        width="120">
        <template slot-scope="scope">
          <el-button @click="handleDelete(scope.row)">删除</el-button>
        </template>
    </el-table-column>
    
    <script>
    export default {
    methods:
        //修改表格的头信息
      renderHeader(h, { column }) {
        // 重新渲染表头
        if (column.property == 'delete') {
          return h('i', {
            class:
              this.clickDeleteTime == 1
                ? 'el-icon-delete c-red'
                : 'el-icon-delete',
            style: 'font-size:24px',
            on: {//这个是你的点击方法
              click: () => {
                this.clearAll()
              }
            }
          })
        }
      },
    }
    </script>
    ```
    
    不过这种方法，目前还不熟练
  
  - 使用slot（推荐使用），在官方文档中的解释：
    
    <img src="https://i.loli.net/2020/10/24/NEwPBFXAjonm1rL.png" alt="BVGnhD.png" style="zoom: 45%;" />
    
    示例：
    
    ```html
    <el-table-column
      prop="delete"
      label="删除"
      width="120">
      <template
        slot="header"
        slot-scope="{ column, $index }">
        <i class="el-icon-delete" @click="clearAll"></i>
      </template>
      <template slot-scope="scope">
        <el-button type="" @click="handleDelete(scope.row)">
          删除
        </el-button>
      </template>
    </el-table-column>
    ```
  
  - 使用cell-style等属性，官方文档说明如下：
    
    <img src="https://i.loli.net/2020/10/24/bN4ktDuMorFWnxl.png" alt="BVwqc8.png" style="zoom:50%;" />
    
    示例如下：
    
    ```html
    <el-table :data="tableData" :cell-style="cellStyle">
      ...
    </el-table>
    
    <script>
      export default {
        methods: {
          cellStyle(row,column,rowIndex,columnIndex){
            //根据报警级别显示颜色
            if(row.column.label==="告警级别"&& row.row.alarmLevel==="紧急告警"){
              return 'color:red'
            }else if(row.column.label==="告警级别"&& row.row.alarmLevel==="一般告警" ){
              return 'color:yellow'
            }
          }
        }
      }
    </script>
    ```

- **通过 <font color=FF0000>"scope.row.属性名"</font> 可以获取当前行对应的属性值**，示例如下：
  
  ```html
  <el-table-column label="操作" width="160">
      <template slot-scope="scope">
          <el-button size="mini" type="primary" plain 
                 @click = "showName(scope.row.name)">点击获取姓名属性</el-button>
      </template>
  </el-table-column>
  ```
  
  <img src="https://i.loli.net/2020/11/17/yzjw1fil5uJto8F.gif" alt="DEz4aj.gif" style="zoom: 45%;" />
  
  同样的：可以通过**"scope.row.属性名"**和三目运算符给特殊的属性值设定样式
  
  ```html
  <el-table-column prop="name" :label="langConfig.table.name[lang]" width="200">
      <template slot-scope="scope">
          <div :class="scope.row.name === '王大虎' ? 'specialColor':''">{{scope.row.name}}</div>
    </template>
  </el-table-column>
  
  <style>
      .specialColor{
      	color:red;
    }
  </style>
  ```
  
  再者：<font color=FF0000>**"scope.row"**是该列全部的数据</font>

- 对于在el-table中添加checkbox，获取所有选择的行的方法：
  
  ```html
  <el-table :data="dataSrc" ref="foo">
      <el-table-column type="selection"></el-table-column>
  </el-table>
  
  <script>
  	this.$refs.foo.selection //这是一个列表，包含所有选择的行中的信息
  </script>
  ```

- 选择多行数据时，使用 `type="selection"` ；同时，<font color=FF0000>如果不想要全选的checkbox</font>，可以使用如下方法：

  ```css
  .el-table__header-wrapper  .el-checkbox{
    display:none;
  }
  ```

  效果如下：

  ![image-20210303181036068](https://i.loli.net/2021/03/03/sNBvlkf1zFJqbLK.png)

  摘自：[element-ui 表格表头禁用全选功能](https://blog.csdn.net/qq_45364616/article/details/103370365)

- **el-table的checkbox的相关事件：**

  - **select：**当用户手动勾选数据行的 Checkbox 时触发的事件	参数：selection, row
  - **select-all：**当用户手动勾选全选 Checkbox 时触发的事件	参数：selection
  - **selection-change：**<font color=FF0000>**当选择项发生变化时会触发该事件**</font>	参数：selection

摘自：[element-ui自定义表格头部的两种方法](https://www.cnblogs.com/wenxinsj/p/10613764.html)   [自定义element-ui的table字体颜色，及背景色](https://blog.csdn.net/qq_32610671/article/details/90731672)  [Element-UI中关于table表格的那些骚操作](https://www.jianshu.com/p/2251cda42425)

[Element table 获取所有选择的行](https://blog.csdn.net/qq_36537108/article/details/89261394)

##### el-table-column 中的按钮等组件下面出现下划线

样式示例如下：

<img src="https://s2.loli.net/2022/07/28/b267v5BDKmS9yWH.png" alt="image-20220728231457438" style="zoom:55%;" />

解决方法如下：

```css
.el-table__fixed{
    height: 100% !important;
}
.el-table__fixed-left{
    height: 100% !important;
}
.el-table__fixed-right{
    height: 100% !important;
}
```

另外，<font color=fuchsia size=4>**似乎**</font> 使用 `this.$refs.tableRef.doLayout()` 可以解决；原理是：el-table-column 宽高是算的，如果有元素进行切换，可能会引起列宽列高布局紊乱。

> 👀 注：doLayout 可以在 el-table 文档中搜到：
>
> > 对 Table 进行重新布局。当 Table 或其祖先元素由隐藏切换为显示时，可能需要调用此方法
> >
> > 摘自：[Element - Table 表格 - Table Methods](https://element.eleme.cn/#/zh-CN/component/table#table-methods)



#### \<el-popover>

自定义**el-popover**示例：

```html
<el-popover placement="bottom" trigger="hover" width="300">
  <div style="display: flex; align-items: flex-start; padding: 20px">
    <i class="el-icon-info" style="font-size: 16px; color: red; margin-right: 10px; margin-top: 3px"></i>
    <div style="display: flex; flex-direction: column; justify-content: flex-start">
      <p style="font-size: 16px; margin-bottom: 10px">驳回原因</p>
      <p style="font-size: 14px">{{detail_info.check_reject}}</p>
    </div>
  </div>
  <i slot="reference" class="el-icon-info" style="color: red; padding-right: 2px; font-size: 16px"></i>
</el-popover>
```

对于：placement可以取的值有：top / top-start / top-end / bottom / bottom-start / bottom-end / left / left-start / left-end / right / right-start / right-end；其中默认值为bottom

摘自：[element.ui 官方文档 -- popover](https://element.eleme.cn/#/zh-CN/component/popover)



#### \<el-icon>

- 如果需要设置大小，可以设置font-size



#### \<el-upload>

- accept：对上传文件格式限制，示例如下：
  
  ```html
  <el-upload accept=".jpg,.jpeg,.png"></el-upload>
  ```
  
  摘自：[element UI upload组件上传附件格式限制](https://blog.csdn.net/github_37847992/article/details/80390673)

- list-type：用于设置文件列表的样式类型，可选值有[text, picture, picture-card]，默认值为text
  
  - **text，如下示例：**
    
    <img src="https://i.loli.net/2021/01/18/nyVGHNzXxptMlhc.png" style="zoom:50%; float: left" />
  
  - **picture，如下示例：**
    
    <img src="https://i.loli.net/2021/01/18/Gl3PEwtQYfmbWBJ.png" style="zoom:50%;float:left" />
  
  - picture-card，示例如下：
    
    <img src="https://i.loli.net/2021/01/18/y58l7pmwRT2KMHh.png" style="zoom:50%;float:left" />

#### \<el-dialog>

想要<font color=FF0000>**直接去掉**</font>**el-dialog\_\_header**(title)和**el-dialog\_\_footer**，似乎目前还没有找到方法...不过，找到了变相去掉的方法：可以用slot以单独控制其中的某个数据显示及样式。示例：

```html
<el-dialog :visible.sync="dialogVisible">
    <div slot="title" class="header-title">
      <span v-show="name" class="title-name">name {{ name }}</span>
    <span class="title-age">age {{ age }}</span>
   </div>
   <span>这是一段/span>
   <span slot="footer" class="dialog-footer">
       <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
   </span>
</el-dialog>
```

思路参考自：[element-ui之dialog对话框组件title插槽的使用](https://blog.csdn.net/hbjiankely/article/details/88218237)

<font color=FF0000>在el-dialog中：有默认间距，且间距较大，这时候可以修改margin为负值以缩小间距。</font>



#### \<el-date-picker>

el-date-picker如果想要选择一个日期范围，可以使用`picker-options`绑定一个函数进行设置；同时，这样做默认会对开始日期和结束如期产生限制，使其必须在选择的范围内，这时可以使用`unlink-panels`以取消绑定



#### \<el-form>

\<el-form> 中的 \<el-form-item> 中自带了 label 属性，用以为某一输入框之类的form作为标识，而由于并不方便对这个label的样式进行设置，所以可以使用slot对其进行自定义：

```html
<span slot="label">foo</span>
```

示例：

```html
<el-form-item>
  <span slot="label">foo</span>
  <template>
      <el-select v-model="newRecord.deliveryVehicle" placeholder="请选择" class="select"></el-select>
  </template>
</el-form-item>
```

同样的：可以使用slot的还有error和default

如果想要让多个表单在一行（默认每个表单一行），可以在el-form中配置inline / :inline="true"



#### \<el-form-item>

对于el-form-item中表单验证的功能，需要通过`rules`属性传入约定的验证属性规则，并且需要在表单中绑定`rules`（:rules="rules"），<font color=FF0000>另外，非常重要的是：需要在提交时进行验证</font>。示例如下：

```html
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
  <el-form-item label="活动名称" prop="name">
    <el-input v-model="ruleForm.name"></el-input>
  </el-form-item>
</el-form>

<script>
    data() {
    return {
            rules: {
                name: [
                    { required: true, message: '请输入活动名称', trigger: 'blur' },
                    { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
            ]
        }
  },
  methods: {
      submitForm(formName) {
      //this.$refs.ruleForm.validate()
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
}

</script>
```

摘自：[element.ui 官方文档](https://element.eleme.cn/#/zh-CN/component/form)

**更进一步的验证：**

- 官方文档上的验证（不确定是否有效）：
  
  ```js
  //type: [数字类型 'number', 整数: 'integer', 浮点数: 'float']
  {type: 'number', message: '请输入数字类型', trigger: 'blur'},
  {type: 'integer', message: '请输入数字类型', trigger: 'change'}, // 'change'是表单的值改变的时候会触发验
  ```

- 使用正则表达式验证：
  
  ```js
  {pattern: /^(([0-9]+)|([0-9]+\.[0-9]{0,8}))$/, message: '支持八位小数的正数',}
  ```

- 使用自由度更强的验证
  
  ```js
  data() {
    /** 设置验证函数 */
    const userValidator = (rule, value, callback) => {
      if (value.length > 3) {
        callback()
      } else {
        callback(new Error('用户名长度必须大于3'))
      }
    }
  
    return {
      data: {
        user: 'sam',
        region: '区域二'
      },
  
      rules: {
        user: [
          { required: true, trigger: 'change', message: '用户名必须录入' },
          /** 绑定验证函数 */
          { validator: userValidator, trigger: 'change' }
        ]
      }
    }
  }
  ```

##### 补充

动态添加验证条件

```js
addRule() {
    const userValidator = (rule, value, callback) => {
      if (value.length > 3) {
        this.inputError = ''
        this.inputValidateStatus = ''
        callback()
      } else {
        callback(new Error('用户名长度必须大于3'))
      }
    }
    const newRule = [
      ...this.rules.user,
      { validator: userValidator, trigger: 'change' }
    ]
    this.rules = Object.assign({}, this.rules, { user: newRule })
}
```

- **手动控制校验状态**

  - validate-status：验证状态，枚举值，共四种：
    
    - success：验证成功
    - error：验证失败
    - validating：验证中
    - (空)：未验证

  - error：自定义错误提示

  示例：

  - 设置 el-form-item 属性
    
    ```html
    <el-form-item
      label="用户名"
      prop="user"
      :error="error"
      :validate-status="status"
    >
    <!-- ... -->
    </el-form-item>
    ```

  - 自定义 status 和 error
    
    ```js
    showError() {
      this.status = 'error'
      this.error = '用户名输入有误'
    },
    showSuccess() {
      this.status = 'success'
      this.error = ''
    },
    showValidating() {
      this.status = 'validating'
      this.error = ''
    }
    ```

##### 动态表单的 prop 问题

在出现 两级甚至三级的 “动态表单验证”时候，使用 `:prop="'arrElement.' + index + '.elementKey'"` 之类的方法时，非常容易出现 invalid prop 的报错；在多次尝试，一筹莫展时，发现如下解决方案；示例如下：

```vue
<el-form-item
  ...
  prop="yourRules"
  :rules="yourRules(yourFormVModel)"
>
  <el-input v-model="yourFormVModel" />
</el-form-item>

<script>
  data() {
    return {
      yourRules: (data) => {
        return [
          {
            validator: (rule, value, callback) => {
              if(yourValidateCondition) {
                callback(new Error('your error hint'))
              }
              callback()
            },
            trigger: 'yourWanttedTriggerEvent' // etc: blur
          }
        ]
      }
    }
	}
</script>
```

另外，return 的数组中 似乎是不能放入 一般的类似： `{ required: true, message: 'yourMsg', trigger: 'blur' }` 的对象的，这似乎无法生效，还会导致 validator 无法生效。

学习自：[vue element嵌套表单验证](https://segmentfault.com/a/1190000039886801)



#### \<el-input>

在el-input中可以设置<font color=FF0000>**maxlength** 和 **minlength** 的HTML原生属性，用来限制输入框的字符长度</font>；其中字符长度是用 Javascript 的字符串长度统计的。<font color=FF0000>对于类型为 text 或 textarea 的输入框，在使用 maxlength 属性限制最大输入长度的同时，可通过设置 **show-word-limit** 属性来展示字数统计。</font>

如果想要实现“可自适应文本高度的文本域”可以使用autosize属性，另外，使用该属性后，表单的其他组件也会随着该组件的扩张 / 缩小而自适应。

<font color=FF0000>**教训：**</font>在使用autosize的同时，不要对input指定高度height，这样会让其他组件自适应失效，如果实在要用，可以使用min-height。

**\<el-input>边框消失：**可以使用深度作用选择器，示例如下：

```html
<div class="inputDeep">
    <el-input></el-input>
</div>

<style>
 /* 利用穿透，设置input边框隐藏 */
  .inputDeep>>>.el-input__inner {
    border: 0;
  }
  /* 如果你的 el-input type 设置成textarea ，就要用这个了 */
  .inputDeep>>>.el-textarea__inner {
    border: 0;
    resize: none;/* 这个是去掉 textarea 下面拉伸的那个标志，如下图 */
  }
</style>
```



#### \<el-select>

在使用\<el-select>时，可能会出现el-option的label过长，从而让el-select整体被动拉长的情况。这时可以使用如下方法，使得el-option过长则自动带滚轮

```html
<!-- 全局样式 -->
<style lang="scss">
.el-select-dropdown {
  max-width: 300px;
}

.el-select-dropdown__item {
  overflow: visible;
}
</style>
```

摘自：[设置el-option的宽度和el-select 宽度相同，选项内容超出显示滚动条](https://blog.csdn.net/qq_37548296/article/details/104899231)



#### \<el-button>

在\<el-button>中自定义图片之类，要用\<div>\</div>包裹整个自定义内容

示例如下：

```html
<el-button type="primary" style="padding: 0">
  <div class="add_btn">
    <img src="static/imgs/waybillcharge/icon_order@2x.png" class="order_icon">
    <p class="add">添加</p>
  </div>
</el-button>
```

如果想要自定义el-button中的内容，比如：将字体修改为蓝色，可以使用如下方法：

```html
<el-button type="foo">foo text</el-button>

<style scope lang="scss">
  .el-button--foo {
    // 自定义样式
  }
</style>
```

更多可以参考：[改变element-UI 中 el-button的颜色](https://blog.csdn.net/weixin_44242623/article/details/106851582)



#### \<el-scrollbar>

这个组件的功能是滚动条，而且关于该滚动条的说明在官方文档中是没有的...源码是：https://github.com/ElemeFE/element/blob/dev/packages/scrollbar/src/main.js

如果想要一个横向滚动的滚动条：只需要将标签的height设为100%。

**相关参数（非官方，存疑）**

| 参数        | 说明               | 类型      | 可选值 | 默认值   |
|:---------:|:----------------:|:-------:|:---:|:-----:|
| wrapClass | 可选参数，容器的样式名      | string  | -   | -     |
| viewClass | 可选参数，展示视图的样式名    | string  | -   | -     |
| wrapStyle | 可选参数，容器的样式       | string  | -   | -     |
| viewStyle | 可选参数，展示视图的样式     | string  | -   | -     |
| native    | 可选参数，是否使用原生滚动    | boolean | -   | false |
| noresize  | 可选参数，容器大小是否是不可变的 | boolean | -   | false |
| tag       | 可选参数，渲染容器的标签     | string  | -   | div   |

摘自：[Element-UI 框架 el-scrollbar 组件](https://juejin.im/post/6844903793377673230)

不过没有必要使用el-scrollbar，使用原生的overflow / overflow-x / overflow-y : scroll即可



#### \<el-drawer>

- **el-drawer设置宽度：**Drawer 抽屉 默认宽度为30%，想要改变宽度只需要使用 :size=“size” （或者size="valOfPixel + px"）给组件传值就可以了。示例如下：

```html
<el-drawer title="我是标题" :visible.sync="drawer" :with-header="false" :size="size">
  <span>我来啦!</span>
</el-drawer>

<script>
export default {
    data() {
        return {
            size: '400px'
        };
    }
}
</script>
```

摘自：[Element Drawer 抽屉改变默认宽度](https://blog.csdn.net/weixin_44640323/article/details/108794124)

#### $message（类似的还有Notification）

$message的type有：info（defalut） / success / warning / error

**$message有两种写法：**

- ```js
  this.$message({
    message: 'foo',
    type: 'success'
  })
  ```

- 为type注册了方法：
  
  ```js
  this.$message.success('foo')
  ```

#### Notification

**message属性支持传入HTML片段**

将dangerouslyUseHTMLString属性设置为 true，message 就会被当作 HTML 片段处理。示例如下：

```html
<template>
  <el-button
    plain
    @click="open">
    使用 HTML 片段
  </el-button>
</template>

<script>
  export default {
    methods: {
      open() {
        this.$notify({
          title: 'HTML 片段',
          dangerouslyUseHTMLString: true,
          message: '<strong>这是 <i>HTML</i> 片段</strong>'
        });
      }
    }
  }
</script>
```

<font color=FF0000>**注意：**</font>message 属性虽然支持传入 HTML 片段，但是<font color=FF0000>在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 XSS 攻击</font>。因此在 dangerouslyUseHTMLString 打开的情况下，请确保 message 的内容是可信的，永远不要将用户提交的内容赋值给 message 属性。

**全局方法**

Element 为 Vue.prototype 添加了全局方法 $notify。因此在 vue instance 中可以采用本页面中的方式调用 Notification。

**单独引用**单独引入 Notification：

```javascript
import { Notification } from 'element-ui';
```

此时调用方法为 Notification(options)。我们也为每个 type 定义了各自的方法，如 Notification.success(options)。并且可以调用 Notification.closeAll() 手动关闭所有实例。



#### v-loading

**v-loading提供了两种模式：指令与服务**

- 指令式是这样的：
  
  ```html
   <el-button
      type="primary"
      @click="openFullScreen"
      v-loading.fullscreen.lock="fullscreenLoading">
      指令方式
   </el-button>
  export default {
      data() {
        return {
          fullscreenLoading: false
        }
      },
  } 
  ```

- 服务式是这样的：
  
  ```js
  import { Loading } from 'element-ui';
  let loadingInstance = Loading.service(options);
  // 以服务的方式调用的 Loading 需要异步关闭
  this.$nextTick(() => { 
    loadingInstance.close();
  });
  ```
  
  可以看出，两种模式一个是自定义指令，一个是定义了一个全局方法。

打开element-ui源码v-loading文件夹下的index.js文件

```js
import directive from './src/directive'; // loading指令实现
import service from './src/index'; // loading服务方式实现

export default {
  install(Vue) {
    Vue.use(directive);
    Vue.prototype.$loading = service;
  },
  directive,
  service
};
```

此文件对外暴露了三个属性，分别是 **install函数**、**directive指令实现**以及**service服务方式实现**

此文件会被 element组件入口文件（src/index.js） 引入, 并且把 directive指令 全局注册了一遍以及在 Vue 原型上扩展了 $loading 方法

```js
//element-ui 2.14.1 line 178
Vue.use(Loading.directive);
//element-ui 2.14.1 line 185
Vue.prototype.$loading = Loading.service;
```

摘自：[从ElementUI的loading组件说起](https://alfxjx.github.io/2019/07/20/%E4%BB%8EElement%E7%9A%84loading%E7%BB%84%E4%BB%B6%E8%AF%B4%E8%B5%B7/)

**补充：**需要注意的是，<font color=FF0000>以服务的方式调用的全屏 Loading 是单例的</font>：若在前一个全屏 Loading 关闭前再次调用全屏 Loading，并不会创建一个新的 Loading 实例，而是返回现有全屏 Loading 的实例

```js
let loadingInstance1 = Loading.service({ fullscreen: true });
let loadingInstance2 = Loading.service({ fullscreen: true });
console.log(loadingInstance1 === loadingInstance2); // true
```

此时调用它们中任意一个的 close 方法都能关闭这个全屏 Loading。

如果完整引入了 Element，那么 Vue.prototype 上会有一个全局方法 `$loading`，它的调用方式为：`this.$loading(options)` ，同样会返回一个 Loading 实例。

摘自：[element-ui -- Loading 加载](https://element.eleme.cn/#/zh-CN/component/loading#zheng-ye-jia-zai)



## Vant UI

#### van-list

在 vant-ui 中，使用可以通过 van-list 组件来先实现下拉获取（添加）数据。同时，是在 van-list 组件中绑定 v-model ( loading ) 、finished 属性、load 事件来进行控制。另外，onload 的方法，只需要定义和绑定，控制调用 onload 是通过修改 loading 和 finished 实现的。

⚠️ 这里要注意的是：想要在同一个页面里面切换 van-list，也是通过 `loading = false; finished = false;` 重新获取以切换的。不过有一个问题，在 `loading === false && finished === false` 的情况下（也就是上一个 van-list 还没有全部加载），赋值操作 `loading = false; finished = false;` 是没有用的（可能是 van-list 内部做了防抖？）；我的解决方法是：

```js
listReset() {
  this.finished = true
  // other logic ...
  this.$nextTick(() => {
    this.loading = false
    this.finished = false
  })
}
```



## Vue Router

路由就是根据网址的不同，返回不同的内容给用户

#### **路由的两种显示模式：**

- hash模式（<font color=FF0000>vue-router 默认使用</font>）：地址栏包含`#`符号，`#`以后的内容不会被后台获取。可以减少到后台访问的次数；但需要参数传递时，将无法满足需求。出现404时，后台不会报错。<font color=FF0000>**另外，`#` 是特殊字符，在很多场合不被满足；所以使用较少**</font>

- history模式（更加普遍）：具有对 url 历史记录进行修改的功能。出现404时，后台会报错
  
  修改方式：
  
  ```js
  export default new Router {
    mode: 'history',
    router: [
      //...
    ]
  }
  ```

<font color=FF0000>组件也有生命周期</font>，所以vue router也可使用生命周期函数

#### 默认路由

```js
routers: [
  {path: '/', redirect: '/foo'},
  {path: '/foo', component: 'foo'}
  {path: '/bar', component: 'bar'}
]
```

#### 路由中的参数传递

- **接收参数：**通过传统的 `?` 传递参数：使用`this.$route.query.itemName`获取url中的`?itemName=val`的val
  
  而对于**传递参数：**
  
  ```js
  funcName(param1, param2, ...){
    this.$router.push({
        path: '/foo',
        query: {arg1: param1, arg2: param2, ...},
    })
  }
  ```

- 通过RESTful传递参数：使用`this.$route.params.itemName`获取参数
  
  比如下面的id和name
  
  ```js
  router: [{path: '/register/:id/:name', component: register}]
  ```
  
  通过`this.$route.params.id`和`this.$route.params.name`获取

每个路由应该映射一个组件。 其中"component" 可以是通过 Vue.extend() 创建的组件构造器，或者，只是一个组件配置对象。

#### **我们<font color=FF0000>可以在任何组件内通过 `this.$router` 访问路由器</font>，<font color=FF0000>也可以通过 `this.$route` 访问当前路由</font>**

#### \<router-link>

**router-link组件的props：**

- **to：**字符串或是对象类型。作用：目标路由的链接，相当于a标签的href属性；是必须的。对应的编程式的导航是router.push()方法，将to的值传入router.push()里

- **tag：**想要把\<router-link>渲染成其他标签（默认渲染成**\<a>**），可以使用tag属性，示例：
  
  ```html
  <router-link to="/login" tag=“span”>登录</router-link>
  
  <!-- 渲染结果 -->
  <span class>登录</span>
  ```

- **replace：**布尔类型值。相当于调用router.replace()，页面切换时不会留下历史记录。非必须。

- **active-class：**class属性字符串类型。表示激活这个链接时，添加的class，默认是router-link-class

- **exact：**布尔类型值（默认false）。表示开启router-link的严格模式；激活默认类名的依据是 inclusive match （全包含匹配）这个链接只有在地址是「这个」的时候被激活

- **append：** 布尔类型值。设置该属性后，则在当前的相对路径前加上基路径。

- **event：**字符串类型值或者是数组字符串。声明可以用来触发导航事件。

- **exact-active-class：**字符串类型值（默认router-link-exact-active）。配置当链接被精确匹配的时候应该激活的 class。

摘自：[Vue路由\<router-link>属性的使用](https://www.jianshu.com/p/d3f689309d7a) / [\<router-link>组件](https://www.jianshu.com/p/8b8616df24a6) / [vue学习笔记一之](https://www.cnblogs.com/wangpengfei8313/p/8074614.html)

#### **\<router-view>**

**\<router-view>的作用是挂载路由**（更通俗的讲就是：显示的是当前路由地址所对应的内容）

**举例：**点击这个链接跳转到其他组件的情况，通常会跳转到新的页面，蛋是，我们不想跳转到新页面，只在当前页面切换着显示，那么就要涉及到路由的嵌套了，也可以说是子路由的使用。

#### 动态路由匹配

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件（比如针对不同用户，进入同一个“用户主页”组件，不过用户id不同），我们可以在 `vue-router` 的路由路径中使用“动态路径参数”(dynamic segment) 来达到这个效果。代码示例如下：

```js
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

这时：像 `/user/foo` 和 `/user/bar` 都将映射到相同的路由。

一个“路径参数”使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到`this.$route.params`，可以在每个组件内使用。于是，我们可以更新 `User` 的模板，输出当前用户的 ID：

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

<font color=FF0000>**提醒：**</font>当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，<font color=FF0000>**原来的组件实例会被复用**</font>。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着<font color=FF0000>组件的生命周期钩子不会再被调用</font>**。

#### 嵌套路由

使用children（数组）配置：children 配置就是像 routes 配置一样的路由配置数组，<font color=FF0000>**可以嵌套多层路由**</font>。

#### Router 配置项

- **mode：** [hash histoty]
  
  作为有服务端渲染的应用，不希望有`#`，上述是 hash 模式，<font color=FF0000>`#` 更多是用来做定位的，同时它不会被搜索引擎解析，导致网站 SEO 效果不好</font>。

- **base**：页面基础路径
  
  设置之后，<font color=FF0000>使用 vue-router api 进行跳转 都会加上这个 base 路径</font>

摘自：[Vue-router之配置](https://www.jianshu.com/p/860c77649ba9)

#### 命名路由

给一个路由起一个名称（别名）来标识一个路由显得更方便一些（不需要写繁杂的path）。

#### router.push(location, onComplete?, onAbort?)

**参数：**

- location可以是path，也可以是router name，<font color=FF0000>还可以带参数</font>。如下示例：
  
  ```js
  // 字符串
  router.push('home')
  // 对象
  router.push({ path: 'home' })
  // 命名的路由
  router.push({ name: 'user', params: { userId: '123' }})
  // 带查询参数，变成 /register?plan=private
  router.push({ path: 'register', query: { plan: 'private' }})
  ```
  
  **另外：如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。**下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：
  
  ```js
  const userId = '123'
  router.push({ name: 'user', params: { userId }}) // -> /user/123
  router.push({ path: `/user/${userId}` }) // -> /user/123
  // 这里的 params 不生效
  router.push({ path: '/user', params: { userId }}) // -> /user
  ```
  
  同样的规则也适用于 router-link 组件的 to 属性。

- onComplete和onAbort（可省略）：是在路由跳转完成和失败时分别执行的<font color=FF0000>回调函数</font>

想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 <font color=FF0000>history 栈添加一个新的记录</font>，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

当你点击 \<router-link> 时，router.push()方法会在内部调用，所以说，点击 \<router-link :to="...">（声明式） 等同于调用 router.push(...)（编程式）

#### router.replace(location, onComplete?, onAbort?)

跟 router.push 很像，唯一的不同就是，它<font color=FF0000>不会向 history 添加新记录</font>，而是跟它的方法名一样 —— <font color=FF0000>替换掉当前的 history 记录</font>。

**router.replace(...)（编程式）和\<router-link :to="..." replace>（声明式）作用相同**

#### 其他router的导航方法

- **router.go(n)：**在 history 记录中向前或者后退多少步，类似于window.history.go(n)

- **router.forward()：**在浏览器记录中前进一步

- **router.back()：**在浏览器记录中回退一步

- **router.go(100) / router.go(-100)：**如果 history 记录不够用，那就默默地失败呗

#### router.addRoutes

语法：

```js
router.addRoutes(routes: Array<RouteConfig>)
```

作用：动态添加更多的路由规则，即：添加路由。参数必须是一个符合 routes 选项要求的数组。

示例如下：

```js
addRoute() {
    this.$router.addRoutes([
        {path: '/b', component: B}
    ])
},
```

#### 命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar (侧导航) 和 main (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。<font color=FF0000>如果 router-view 没有设置名字，那么默认为 default</font>。

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

#### 重定向

“重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`。重定向也是通过 `routes` 配置来完成。

- 从 path a 重定向到 path b：
  
  ```js
  const router = new VueRouter({
    routes: [
      { path: '/a', redirect: '/b' }
    ]
  })
  ```

- 重定向的目标也可以是一个命名的路由：
  
  ```js
  const router = new VueRouter({
    routes: [
      { path: '/a', redirect: { name: 'foo' }}
    ]
  })
  ```

- 还可以是一个方法，动态返回重定向目标：
  
  ```js
  const router = new VueRouter({
    routes: [
      { path: '/a', redirect: to => {
        // 方法接收 目标路由 作为参数
        // return 重定向的 字符串路径/路径对象
      }}
    ]
  })
  ```

#### 别名

`/a` 的别名是 `/b`，<font color=FF0000>意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`</font>，就像用户访问 `/a` 一样。示例如下：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

#### 导航守卫

vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。<font color=FF0000>有多种机会植入路由导航过程中：全局的、单个路由独享的、或者组件级的</font>。

参数（param）或查询（query）的改变并不会触发进入/离开的导航守卫。你可以通过观察 $route 对象来应对这些变化，或使用 beforeRouteUpdate 的组件内守卫。

- **全局前置守卫**
  
  你可以<font color=FF0000>使用 **router.beforeEach** 注册一个全局**前置守卫**</font>：
  
  ```js
  const router = new VueRouter({ ... })
  
  router.beforeEach((to, from, next) => {
    // ...
  })
  ```
  
  当一个导航触发时，全局前置守卫按照创建顺序调用。<font color=FF0000>守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**</font>。
  
  每个守卫方法接收三个参数：
  
  - **to: Route：**<font color=FF0000>即将要进入的目标路由对象</font>
  
  - **from: Route：**当前导航<font color=FF0000>正要离开的路由</font>
  
  - **next: Function：** <font color=FF0000>一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。</font>
    
    - **next()：** <font color=FF0000>进行管道中的下一个钩子</font>。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
    - **next(false)：** <font color=FF0000>中断当前的导航</font>。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。
    - **next('/') 或 next({ path: '/' })：** <font color=FF0000>跳转到一个不同的地址</font>。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在 router-link 的 to prop 或 router.push 中的选项。
    - **next(error)：** (2.4.0+) <font color=FF0000>如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。</font>
    
    另外：<font color=FF0000>确保 next 函数在任何给定的导航守卫中都被严格调用一次</font>。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错。

- **全局解析守卫**
  
  在 2.5.0+ 你可以用 <font color=FF0000>**router.beforeResolve**</font> 注册一个<font color=FF0000>**全局守卫**</font>。这和 router.beforeEach 类似，区别是<font color=FF0000>在**导航被确认之前**，同时**在所有组件内守卫和异步路由组件被解析之后**，**解析守卫就被调用**</font>。

- **全局后置钩子**
  
  你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 next 函数也不会改变导航本身：
  
  ```js
  router.afterEach((to, from) => {
    // ...
  })
  ```

- **路由独享的守卫（<font color=FF0000>局部</font>）**
  
  你可以在路由配置上直接定义 beforeEnter 守卫：
  
  ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        beforeEnter: (to, from, next) => {
          // ...
        }
      }
    ]
  })
  ```
  
  这些守卫与全局前置守卫的方法参数是一样的。

- **组件内的守卫**
  
  可以在路由组件内直接定义以下路由导航守卫：
  
  - **beforeRouteEnter(to, from, next)：**在渲染该组件的对应路由被 <font color=FF0000>confirm 前</font>调用。<font color=FF0000>**不能获取组件实例 this**，因为当守卫执行前，组件实例还没被创建</font>。
    
    不过，你可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。如下示例：
    
    ```js
    beforeRouteEnter (to, from, next) {
      next(vm => {
        // 通过 `vm` 访问组件实例
      })
    }
    ```
  
  - **beforeRouteUpdate(to, from, next)： (2.2 新增)** 在<font color=FF0000>当前路由改变</font>，但是该组件被复用时调用举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。<font color=FF0000>可以访问组件实例 this</font>。<font color=FF0000>**由于this 已经可用了，所以不支持传递回调**</font>
  
  - **beforeRouteLeave(to, from, next)：**<font color=FF0000>导航离开</font>该组件的对应路由时调用，<font color=FF0000>可以访问组件实例 this</font>。<font color=FF0000>**由于this 已经可用了，所以不支持传递回调**</font>

#### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。（离开某一路由）
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

#### 数据获取

有时候，进入某个路由后，需要从服务器获取数据。<mark>例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现</mark>：

- **导航完成之后获取**：先完成导航，然后<font color=FF0000>在接下来的组件生命周期钩子中获取数据</font>。在数据获取期间显示“加载中”之类的指示。
  
  使用这种方式时，会马上导航和渲染组件，然后<font color=FF0000>在组件的 created 钩子中获取数据</font>。这让我们有机会在数据获取期间展示一个 loading 状态，还可以在不同视图间展示不同的 loading 状态。

- **导航完成之前获取**：导航完成前，<font color=FF0000>在路由进入的守卫中获取数据，在数据获取成功后执行导航</font>。
  
  通过这种方式，我们<font color=FF0000>在导航转入新的路由前获取数据</font>。我们可以<font color=FF0000>在接下来的组件的 beforeRouteEnter 守卫中获取数据</font>，当数据获取成功后只调用 next 方法。

#### 滚动行为

使用前端路由，<font color=FF0000>当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样</font>。 vue-router 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

<font color=lightSeaGreen>**注意：这个功能只在支持 history.pushState 的浏览器中可用。**</font>

当创建一个 Router 实例，你可以提供一个 scrollBehavior 方法：

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

<font color=FF0000>scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。</font>（记录浏览器在按下前进/后退按钮前的位置状态）

这个方法返回滚动位置的对象信息，长这样：

- { x: number, y: number }
- { selector: string, offset? : { x: number, y: number }} (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 false，[参考这里](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

**举例：**

```js
scrollBehavior (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```

对于所有路由导航，简单地让页面滚动到顶部。



#### 补充内容

##### CDN 方式引入Vue-Router

```html
<script src="/static/js/vue-router.js"></script>
<script>
  const router = new VueRouter({
    mode: "history", // 默认使用hash模式，url会出现#
  });

  const app = new Vue({
    router,
    el: "#app",
  });
</script>
```

摘自：[JS：CDN方式引入Vue-Router](https://blog.csdn.net/mouday/article/details/106772721)

##### CDN 形式引入其他 lib

在使用 html `<script>` 标签使用 CDN 引入一些 lib 的项目中，会出现有些 lib 在官方文档中不提供 CDN 地址，这就需要自己去寻找（比如 [qrcodejs](https://github.com/davidshimjs/qrcodejs) ），但是因为类似名称的 lib 太多，可能无法确认当前找到的 CDN url 是否就是预期的 lib 的 CDN url，这就有些麻烦。

发现 jsdelivr 给出了公式化的 url 有点方便：`https://cdn.jsdelivr.net/gh/{github用户名}/{仓库名称}/{具体路径}` ，以 qrcodejs 为例，CDN 地址即：`https://cdn.jsdelivr.net/gh/davidshimjs/qrcodejs/qrcode.min.js`

参考自：[GitHub仓库当图床,免费使用cdn加速](https://blog.csdn.net/weixin_46171419/article/details/124125236)



## Vue CLI

Vue CLI 是一种脚手架，是一个基于 Vue.js 进行快速开发的<font color=FF0000>完整系统</font>

**Vue CLI的优势**

- 通过 `@vue/cli` 实现的交互式的项目脚手架。

- 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。

- 一个运行时依赖 ( `@vue/cli-service` )，该依赖：
  
  - 可升级
  - 基于 webpack 构建，并带有合理的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。

- 一个丰富的官方插件集合，集成了前端生态中最好的工具。(Node Vue Vue-Router Webpack Yarn)

- 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

##### 查看Vue CLI的版本，可以使用如下两种方式：

```sh
vue --version
vue -V
```

##### 项目启动命令

在vue-cli3中启动项目的命令是：

```sh
npm run serve
```

而在vue-cli2中启动项目的命令是：

```sh
npm run dev
```



#### preset 和 ~/.vuerc

一个 Vue CLI preset 是一个包含创建新项目所需预定义选项和插件的 JSON 对象，让用户无需在命令提示中选择它们。

在使用vue cli创建一个项目中，如果你决定手动选择特性，在操作提示的最后你可以选择将已选项保存为一个将来可复用的 preset。

在 `vue create` 过程中保存的 preset 会被放在你的 home 目录下的一个配置文件中 (`~/.vuerc`)。你可以通过直接编辑这个文件来调整、添加、删除保存好的 preset。

**preset实例如下：**

```json
{
  "useConfigFiles": true,
  "cssPreprocessor": "sass",
  "plugins": {
    "@vue/cli-plugin-babel": {},
    "@vue/cli-plugin-eslint": {
      "config": "airbnb",
      "lintOn": ["save", "commit"]
    },
    "@vue/cli-plugin-router": {},
    "@vue/cli-plugin-vuex": {}
  }
}
```



#### vue add 命令

如果你<font color=FF0000>想在一个已经被创建好的项目中安装一个插件，可以使用 vue add 命令</font>，示例如下：

```sh
vue add eslint
```

<font color=lightSeaGreen>这个命令将 `@vue/eslint` 解析为完整的包名 `@vue/cli-plugin-eslint`，然后从 npm 安装它，调用它的生成器</font>。<font color=FF0000>这个和之前的用法等价</font>

```sh
vue add cli-plugin-eslint
```

如果不带 `@vue` 前缀，该命令会换作解析一个 unscoped 的包。

**vue add 插件名解析**

| Vue add 语法          | 等价于                      | 解析的npm包               |
| :-------------------- | :-------------------------- | :------------------------ |
| `vue add @vue/eslint` | `vue add cli-plugin-eslint` | `@vue/cli-plugin-eslint`  |
| `vue add apollo`      | -                           | `vue-cli-plugin-apollo`   |
| `vue add @foo/bar`    | -                           | `@foo/vue-cli-plugin-bar` |

另外，`vue add` 命令接受两个参数：

- plugin： 插件名称，必填。
- registry： 安装插件指定的安装源，只针对于 npm 包管理器，选填。

**补充：**

vue add 的设计意图是为了安装和调用 Vue CLI 插件。这不意味着替换掉普通的 npm 包。对于这些普通的 npm 包，你仍然需要选用包管理器。

警告：我们推荐在运行 vue add 之前将项目的最新状态提交，因为<font color=FF0000>该命令可能调用插件的文件生成器并很有可能更改你现有的文件。</font>

**vue add和npm install的区别**（摘自：[Vue add 与 npm install 有什么区别？？？](https://forum.vuejs.org/t/vue-add-npm-install/58275)   [Vue创建一个新的项目、vue add 和npm install区别](https://codeleading.com/article/35174593221/)）

- 区别就是<font color=FF0000>vue add装的是vue cli插件</font>，<font color=FF0000>npm装的是npm插件</font>

- <font color=FF0000>vue add可能会改变现有的项目结构</font>，但是<font color=FF0000>npm install仅仅是安装包而不会改变项目的结构</font>
  
  add如果你下载的库, 特别是 Ui 库, 希望对脚手架结构产生影响，那就选择vue add xxx
  
  npm如果不希望对脚手架结构产生影响, 只是单纯的使用, 比如 axios 这个插件，那就选择npm install xxx

- vue add 除了會 npm install 之外，還會幫你配置好一個範例文件。需要注意的是這個指令會更改你現有的文件內容。
  特別的是使用 vue add router 或是 vue add vuex，他們雖然不是插件，但Vue CLI會幫你配置好文件，例如 vue add router 會幫你配置 router.js 文件以及生成 About.vue 和 Home.vue 並在 App.vue 內建立了簡單的路由範例，而 vue add vuex 會幫你配置好一個 store.js 文件。

#### 浏览器适配

你会发现有 package.json 文件里的 browserslist 字段 (或一个单独的 .browserslistrc 文件)，指定了项目的目标浏览器的范围。这个值会被 @babel/preset-env 和 Autoprefixer 用来确定需要转译的 JavaScript 特性和需要添加的 CSS 浏览器前缀。

即：vue-cli中自带了@babel/preset-env和Autoprefixer，以进行浏览器适配

#### 静态资源处理

静态资源可以通过两种方式进行处理：

- 在 JavaScript 被导入或在 template/CSS 中通过相对路径被引用。<font color=FF0000>这类引用会被 webpack 处理。</font>
  
  当你在 JavaScript、CSS 或 `*.vue` 文件中使用相对路径 (必须以 `.` 开头) 引用一个静态资源时，该资源将会被包含进入 webpack 的依赖图中。在其编译过程中，所有诸如 `<img src="...">`、`background: url(...)` 和 CSS `@import` 的资源 URL **都会被解析为一个模块依赖**。
  
  例如，`url(./image.png)` 会被翻译为 `require('./image.png')`，而：
  
  ```html
  <img src="./image.png">
  ```
  
  将会被编译到：
  
  ```js
  h('img', { attrs: { src: require('./image.png') }})
  ```

- 放置在 `public` 目录下或通过绝对路径被引用。这类资源将会直接被拷贝，而<font color=FF0000>不会经过 webpack 的处理</font>。<font color=FF0000>这样做是不被推荐的</font>
  
  推荐将资源作为你的模块依赖图的一部分导入，这样它们会<font color=FF0000>通过 webpack 的处理并获得如下好处</font>：
  
  - <font color=FF0000>脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。</font>
  
  - 文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。
  
  - 最终生成的文件名包含了内容哈希，因此你不必担心浏览器会缓存它们的老版本。
    
    `public` 目录提供的是一个**应急手段**，当你通过绝对路径引用它时，留意应用将会部署到哪里。
  
  <font color=FF0000> **那么何时使用 public 文件夹**</font>
  
  - 你需要在构建输出中指定一个文件的名字。
  - 你有上千个图片，需要动态引用它们的路径。
  - 有些库可能和 webpack 不兼容，这时你除了将其用一个独立的 \<script> 标签引入没有别的选择。

#### URL 转换规则

- 如果 URL 是一个绝对路径 (例如 `/images/foo.png`)，它将会被保留不变。

- 如果 URL 以 `.` 开头，它会作为一个相对模块请求被解释且基于你的文件系统中的目录结构进行解析。

- 如果 URL 以 `~` 开头，其后的任何内容都会作为一个模块请求被解析。这意味着你甚至可以引用 Node 模块中的资源：
  
  ```html
  <img src="~some-npm-package/foo.png">
  ```

- 如果 URL 以 `@` 开头，它也会作为一个模块请求被解析。它的用处在于 <font color=FF0000>Vue CLI 默认会设置一个指向 `<projectRoot>/src` 的别名 `@`。</font>**(仅作用于模版中)**

#### CSS相关

Vue CLI 项目天生支持 PostCSS、CSS Modules 和包含 Sass、Less、Stylus 在内的预处理器

你可以在创建项目的时候选择预处理器 (Sass/Less/Stylus)。如果当时没有选好，内置的 webpack 仍然会被预配置为可以完成所有的处理。你也可以手动安装相应的 webpack loader：

```bash
# Sass
npm install -D sass-loader sass

# Less
npm install -D less-loader less

# Stylus
npm install -D stylus-loader stylus
```

##### 向预处理器 Loader 传递选项

> 👀 之所以想要记这个笔记，是因为看 [【我是哈默】全局引入 Sass 变量【Vue小知识】](https://www.bilibili.com/video/BV1o14y1x7zL) 在 `vue.config.js` 中使用 `css.loaderOptions.scss.additionalData` 实现 SCSS 变量全局使用的效果。代码如下：
>
> ```js
> // vue.config.js
> const { defineConfig } = require('@vue/cli-service')
> 
> module.exports = defineConfig({
>   css: {
>     loaderOptions: {
>       scss: {
>         additionalData: `@import '~@/style/yourTargetGlobalVariableFile.scss'`
>         // 👀 另外，值得注意的是这里@import 与后面的写法。
>       }
>     }
>   }
> })
> ```

有的时候你想要向 webpack 的预处理器 loader 传递选项。你可以使用 `vue.config.js` 中的 `css.loaderOptions` 选项。<font color=fuchsia>比如你可以这样向所有 Sass / Less 样式传入共享的全局变量</font>：

```js
// vue.config.js
module.exports = {
  css: {
    loaderOptions: {
      // 给 sass-loader 传递选项
      sass: {
        // @/ 是 src/ 的别名，所以这里假设你有 `src/variables.sass` 这个文件
        // 注意：在 sass-loader v8 中，这个选项名是 "prependData"
        additionalData: `@import "~@/variables.sass"`
      },
      // 默认情况下 `sass` 选项会同时对 `sass` 和 `scss` 语法同时生效
      // 因为 `scss` 语法在内部也是由 sass-loader 处理的，但是在配置 `prependData` 选项的时候 
      // `scss` 语法会要求语句结尾必须有分号，`sass` 则要求必须没有分号
      // 在这种情况下，我们可以使用 `scss` 选项，对 `scss` 语法进行单独配置
      scss: {
        additionalData: `@import "~@/variables.scss";`
      },
      // 给 less-loader 传递 Less.js 相关选项
      less:{
        // http://lesscss.org/usage/#less-options-strict-units `Global Variables`
        // `primary` is global variables fields name
        globalVars: {
          primary: '#fff'
        }
      }
    }
  }
}
```



#### vue.config.js

**vue.config.js** 是一个可选的配置文件，如果项目的 (和 package.json 同级的) 根目录中存在这个文件，那么它会被 @vue/cli-service 自动加载。你也可以使用 package.json 中的 vue 字段，但是注意这种写法需要你严格遵照 JSON 的格式来写。

#### 模式

**模式**是 Vue CLI 项目中一个重要的概念。默认情况下，一个 Vue CLI 项目有三个模式：

- development 模式用于 vue-cli-service serve
- test 模式用于 vue-cli-service test:unit
- production 模式用于 vue-cli-service build 和 vue-cli-service test:e2e

你可以通过传递 --mode 选项参数为命令行覆写默认的模式。例如，如果你想要在构建命令中使用开发环境变量：

```text
vue-cli-service build --mode development
```

Vue CLI 是对 webpack 的封装，可以通过 `vue inspect` 命令，输出对应的 webpack 配置文件，如 `vue inspect > target-conf.js` 即可生成。



#### 在 `vue.config.js` 中实现 `resolve.alias`

在 vue cli 中默认是实现了 `@` 是 `/src` 的别名，如果想要自己定义别名，配置如下：

```js
module.exports = defineConfig({
  chainWebpack: (config) => {
    config.resolve.alias.set('myAlias', path.join(__dirname,  'yourTargetPath'))
  }
})
```

同时，默认情况下，点击使用别名无法像使用 `@` 的路径一样，跳转到定义处；这时，可以在 jsconfig.json（详见 [[#@vue/cli 5 的新特性#jsconfig.json]]） 中配置如下：

```diff
// jsconfig.jon
{
  "compilerOptions": {
    "target": "es5",
    "module": "esnext",
    "baseUrl": "./",
    "moduleResolution": "node",
    "paths": {
      "@/*": [ "src/*" ],
+     "myAlias/*": [ "yourTargetPath/*" ] 
    },
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  }
}
```

即可生效



#### @vue/cli 5 的新特性

##### jsconfig.json

使用 @vue/cli 5 创建项目，会发现：多了一个 `jsconfig.json` 的文件（ ⚠️ 注意不是 `tsconfig.json` ）

> ##### What is jsconfig.json?
>
> The presence of `jsconfig.json` file in a directory indicates that the directory is the root of a JavaScript Project. The `jsconfig.json` file <font color=red>specifies the root files and the options for the features provided by the [JavaScript language service](https://github.com/microsoft/TypeScript/wiki/JavaScript-Language-Service-in-Visual-Studio)</font> .
>
> > 💡 **Tip:** If you are not using JavaScript , you do not need to worry about `jsconfig.json`.
>
> > 💡 **Tip:** <font color=red>**`jsconfig.json` is a descendant of `tsconfig.json`**</font> , which is a configuration file for TypeScript. <font color=fuchsia>`jsconfig.json` is `tsconfig.json` with `"allowJs"` attribute set to `true`</font> .
>
> ##### Why do I need a jsconfig.json file?
>
> Visual Studio Code's JavaScript support can run in two different modes:
>
> - **File Scope - <font color=lightSeaGreen>no jsconfig.json</font>**: <font color=fuchsia>In this mode, JavaScript files opened in Visual Studio Code are **treated as independent units**</font>. <font color=red>As long as a file `a.js` doesn't reference a file `b.ts` explicitly</font> (either using `import` or **CommonJS** [modules](http://www.commonjs.org/specs/modules/1.0) ), <font color=fuchsia>there is **no common project context**</font>（共同的项目上下文） <font color=fuchsia>**between the two files**</font>.
> - **Explicit Project - <font color=lightSeaGreen>with jsconfig.json</font>**: <font color=fuchsia size=4>**A JavaScript project is defined via a `jsconfig.json` file**</font>. The presence of such a file in a directory indicates that the directory is the root of a JavaScript project. <font color=fuchsia>The file itself can optionally **list the files belonging to the project**, **the files to be excluded from the project**, as well as compiler options</font> (see below).
>
> The JavaScript experience is improved when you have a `jsconfig.json` file in your workspace that defines the project context. For this reason, we offer a hint to create a `jsconfig.json` file when you open a JavaScript file in a fresh workspace.
>
> 后面还有内容，略。
>
> 摘自：[VSC doc - jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig)



## Vuex

#### 介绍

Vuex 是一个专为 Vue.js 应用程序开发的<font color=FF0000>**状态管理模式**</font>。它<font color=FF0000>采用集中式存储管理应用的所有组件的状态</font>，并<font color=FF0000>以相应的规则保证状态以一种可预测的方式发生变化</font>。

#### 基本思想

把组件的共享状态抽取出来，以一个全局单例模式管理。在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！

通过定义和隔离状态管理中的各种概念并通过强制规则维持视图和状态间的独立性，我们的代码将会变得更结构化且易维护。

#### 使用场景

<mark>如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 store 模式 (opens new window)就足够您所需了</mark>。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。引用 Redux 的作者 Dan Abramov 的话说就是：

> Flux 架构就像眼镜：您自会知道什么时候需要它。

#### **补充使用场景：**

有什么状态时需要我们在多个组件间共享的呢？如果你做过大型开放，你一定遇到过多个状态，在多个界面间的共享问题。比如用户的登录状态、用户名称、头像、地理位置信息等等。比如商品的收藏、购物车中的物品等等。<font color=FF0000>这些状态信息，我们都可以放在统一的地方，对它进行保存和管理，而且它们还是响应式的</font>

Vue已经帮我们做好了单个界面的状态管理，但是如果是多个界面呢？多个试图都依赖同一个状态（一个状态改了，多个界面需要进行更新），不同界面的Actions都想修改同一个状态（Home.vue需要修改，Profile.vue也需要修改这个状态）。
也就是说对于某些状态(状态1/状态2/状态3)来说只属于我们某一个试图，但是也有一些状态(状态a/状态b/状态c)属于多个试图共同想要维护的。状态1/状态2/状态3你放在自己的房间中，你自己管理自己用，没问题。但是状态a/状态b/状态c我们希望交给一个大管家来统一帮助我们管理！！！没错，Vuex就是为我们提供这个大管家的工具。

**全局单例模式（大管家）**
我们现在要做的就是将共享的状态抽取出来，交给我们的大管家，统一进行管理。之后，你们每个试图，按照我规定好的规定，进行访问和修改等操作。这就是Vuex背后的基本思想。

摘自：[Vuex详细教程](https://www.cnblogs.com/wugongzi/p/13413274.html)

#### 安装

在一个模块化的打包系统中，您必须显式地通过 Vue.use() 来安装 Vuex：

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

<mark>当使用全局 script 标签引用 Vuex 时，不需要以上安装过程。</mark>

<font color=FF0000>**依赖：**</font>Vuex 依赖 Promise。如果你支持的浏览器并没有实现 Promise (比如 IE)，那么你可以使用一个 polyfill 的库，例如 es6-promise。

#### 核心基础

每一个 Vuex 应用的核心就是 store（仓库）。<font color=FF0000>“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。</font>Vuex 和单纯的全局对象有以下两点不同：

- **Vuex 的状态存储是响应式的**。<font color=FF0000>当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。</font>

- **你不能直接改变 store 中的状态**。<font color=FF0000>改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用</font>。

安装 Vuex 之后，让我们来创建一个 store。创建过程直截了当——仅需要提供一个初始 state 对象和一些 mutation：

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

现在，你可以通过 store.state 来获取状态对象，以及通过 store.commit 方法触发状态变更：

```js
store.commit('increment')

console.log(store.state.count) // -> 1
```

#### State

Vuex 使用单一状态树 —— 是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 (SSOT)”而存在。<font color=FF0000>这也意味着，每个应用将仅仅包含一个 store 实例</font>。<mark>单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照</mark>。

如何在 Vue 组件中展示状态呢？由于 Vuex 的状态存储是响应式的，<font color=FF0000>从 store 实例中读取状态最简单的方法就是在<mark><font color=FF0000>**计算属性**</font></mark>中返回某个状态</font>：

```js
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

每当 store.state.count 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。

然而，这种模式导致组件依赖全局状态单例。<font color=FF0000>在模块化的构建系统中，在每个需要使用 state 的组件中需要频繁地导入，并且在测试组件时需要模拟状态</font>。（这便造成麻烦）

##### 改进

Vuex 通过 store 选项，提供了一种机制将状态从根组件<font color=FF0000>“注入”到每一个子组件中</font>（需调用 Vue.use(Vuex)）：

```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

通过在根实例中注册 store 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 this.$store 访问到。让我们更新下 Counter 的实现：

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

#### mapstate 辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性。示例如下：

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

#### **对象展开运算符**

<font color=FF0000>mapState 函数返回的是一个对象</font>。我们如何将它与局部计算属性混合使用呢？通常，我们需要使用一个工具函数将多个对象合并为一个，以使我们可以将最终对象传给 computed 属性。但是自从有了对象展开运算符，我们可以极大地简化写法：

```js
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

#### Mutation

Vuex的store中的state是响应式的，当state中的数据发生改变时,，Vue组件会自动更新。这就要求我们必须遵守一些Vuex对应的规则:
提前在store中初始化好所需的属性。当给state中的对象添加新属性时, 使用下面的方式:

- 方式一：使用Vue.set(obj, 'newProp', 123)
- 方式二：用心对象给旧对象重新赋值

示例如下：

```js
//给state添加一个height属性
const store = new Vuex.Store({
  state: {
    name: 'why', age: 18
  }
  mutations: {
      updateInfo(state, payload) {
          //方法一：Vue.set()
          Vue.set(state.info, 'height', payload.height)
          //方法二：给info赋值一个新的对象
          state.info = {...state.info, 'height': payload.height}
        }
    }
})
```

**Mutation常量类型**

我们来考虑下面的问题：
在mutation中， 我们定义了很多事件类型(也就是其中的方法名称)。<font color=FF0000>当我们的项目增大时， Vuex管理的状态越来越多， 需要更新状态的情况越来越多， 那么意味着Mutation中的方法越来越多</font>。<font color=FF0000>方法过多， 使用者需要花费大量的经历去记住这些方法， 甚至是多个文件间来回切换， 查看方法名称， 甚至如果不是复制的时候， 可能还会出现写错的情况</font>。如何避免上述的问题呢?
在各种Flux实现中， 一种很常见的方案就是使用常量替代Mutation事件的类型。我们可以将这些常量放在一个单独的文件中， 方便管理以及让整个app所有的事件类型一目了然。具体怎么做呢?我们可以创建一个文件: mutation-types.js， 并且在其中定义我们的常量。定义常量时， 我们可以使用ES2015中的风格， 使用一个常量来作为函数的名称。

```js
// mutation-types.js
export const SOME_MUTATION = ''
```

```js
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

#### Action

context是什么？context是和store对象具有相同方法和属性的对象。也就是说， 我们可以通过context去进行commit相关的操作， 也可以获取context，state等。但是注意， 这里它们并不是同一个对象， 为什么呢? 我们后面学习Modules的时候， 再具体说。这样的代码是否多此一举呢？我们定义了actions， 然后又在actions中去进行commit， 这不是脱裤放屁吗？事实上并不是这样， 如果在Vuex中有异步操作， 那么我们就可以在actions中完成了。

#### Module

局部状态通过 context.state 暴露出来，根节点状态则为 context.rootState



## Vue项目杂项

#### assets与static的区别

- 相同点：资源在html中使用，都是可以的。

- 不同点：使用assets下面的资源，在js中使用的话，路径要经过webpack中file-loader编译，路径不能直接写。

<font color=FF0000>**assets**中的文件会经过webpack打包，**重新编译**，推荐该方式</font>。而<font color=FF0000>**static**中的文件，**不会经过编译**。项目在经过打包后，**会生成dist文件夹，static中的文件只是复制一遍而已**</font>。简单来说，<mark><font color=FF0000>static中建议放一些外部第三方，自己的放到assets，别人的放到static中</font></mark>。

注意：如果把图片放在assets与static中，html页面可以使用；但在动态绑定中，assets路径的图片会加载失败，因为webpack使用的是commenJS规范，必须使用require才可以。

摘自：[assets与static的区别](https://blog.csdn.net/qq_41115965/article/details/80796211)

##### 补充

**Webpacked Assets**

为了回答这个问题，我们首先需要了解Webpack如何处理静态资产。在 `*.vue` 组件中，所有模板和CSS都会被 `vue-html-loader` 及 `css-loader` 解析，并查找资源URL。例如，在 `<img src="./logo.png">`
和 `background: url(./logo.png)` 中，`"./logo.png"` 是相对的资源路径，将由**Webpack解析为模块依赖**。

<mark>因为 `logo.png` 不是 JavaScript，当被视为模块依赖时，需要使用 `url-loader` 和 `file-loader`
处理它。</mark>vue-cli 的 webpack 脚手架已经配置了这些 loader，因此可以使用相对/模块路径。

<font color=FF0000>由于这些资源可能在构建过程中被内联/复制/重命名，所以它们基本上是源代码的一部分。这就是为什么建议将
Webpack 处理的静态资源放在 `/src` 目录中和其它源文件放一起的原因</font>。事实上，甚至不必把它们全部放在 `/src/assets`：可以用`模块/组件`的组织方式来使用它们。例如，可以在每个放置组件的目录中存放静态资源。

**"Real" Static Assets**

相比之下，<font color=FF0000>`static/` 目录下的文件并不会被 Webpack 处理：它们会直接被复制到最终目录（默认是`dist/static`）下。必须使用绝对路径引用这些文件</font>，这是通过在 `config.js` 文件中的 `build.assetsPublicPath` 和 `build.assetsSubDirectory` 连接来确定的。

任何放在 `static/` 中文件需要以绝对路径的形式引用：`/static/[filename]`。如果更改 `assetSubDirectory` 的值为 `assets`，那么路径需改为 `/assets/[filename]`。

摘自：[segmentfault - assets 和static的区别](https://segmentfault.com/q/1010000009842688)（翻译的http://vuejs-templates.github.io/webpack/static.html中的内容）

#### Vue 项目文件结构

**（使用 `vue create proj_name` 或 `vue init webpack proj_name`生成）**

**一级目录：**

```
├── .babelrc           babel（语法解析器）配置
├── .editorconfig      编辑器语法的配置（比如Tab等于两个空格）
├── .eslintignore             eslint忽略文文件路径的配置
├── .eslintrc.js       eslint（代码规范工具）配置 
├── .gitignore         git忽略上传文件的配置
├── .postcssrc.js      postcss的配置项
├── README.md
├── build              项目打包webpack的配置内容（目前的层次，一般不需要修改...）
        ├── build.js 
        ├── check-versions.js 
        ├── logo.png 
        ├── utils.js 
        ├── vue-loader.conf.js 
        ├── webpack.base.conf.js     基础的webpack配置项
        ├── webpack.dev.conf.js      开发环境的webpack配置项
        └── webpack.prod.conf.js     生产环境的webpack配置项
├── config                         放置项目的配置文件
        ├── dev.env.js                             放置开发的配置信息
        ├── index.js                                 放置基础的配置信息
        └── prod.env.js                             放置生产的配置信息
├── index.html         项目首页默认的模版文件
├── node_modules              存放第三方依赖的包
├── package-lock.json  package锁文件，确定第三方包的具体版本，保持团队编程统一
├── package.json       第三方模块的依赖
├── src                              存放项目的源代码
            ├── App.vue                 项目最原始的根组件
            ├── assets                  项目中的图片类资源
            │   └── logo.png
            ├── components                            存放项目中的小组件
            │   └── HelloWorld.vue
            ├── main.js                                    项目的入口文件
            └── router
                └── index.js                        放置所有的路由
└── static             存放静态文件、模拟的json数据
```



