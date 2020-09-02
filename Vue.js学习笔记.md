# Vue.js学习笔记



### Vue.js是什么

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



### 绑定attribute的另一种方法（第一种是上面的示例）

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



### 条件（v-if）

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



### 循环

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



### 处理用户输入

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



### 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

<img src="https://s1.ax1x.com/2020/08/31/dOeVaT.png" style="zoom: 43%;" />

在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。

### //todo，这里没有完全看懂...不过可能要等到看完“组件基础”之后才会好些

https://cn.vuejs.org/v2/guide/index.html#%E4%B8%8E%E8%87%AA%E5%AE%9A%E4%B9%89%E5%85%83%E7%B4%A0%E7%9A%84%E5%85%B3%E7%B3%BB



### Vue实例

每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 **Vue 实例**开始的：

```js
var vm = new Vue({  //这里的vm表示viewModel，这个命名很常见（参考自MVVM模型）
  // 选项
})
```

当创建一个 Vue 实例时，你<font color=FF0000>可以传入一个**选项对象**</font>。这篇教程主要描述的就是如何使用这些选项来创建你想要的行为。作为参考，你也可以在 [API 文档](https://cn.vuejs.org/v2/api/#选项-数据)中浏览完整的选项列表。

一个 Vue 应用由一个通过 `new Vue` 创建的<font color=FF0000>**根 Vue 实例**</font>，以及可选的嵌套的、可复用的组件树组成。



### 数据与方法

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



### 实例生命周期钩子

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

### 模板语法

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



### 缩写

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



### 计算属性

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



### 侦听器

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



### Class 与 Style 绑定

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



### 条件渲染

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



### 列表渲染

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



### 事件处理

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

- <font color=FF0000>有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法</font>：

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

- **.stop**

  ```html
  <!-- 阻止单击事件继续传播 -->
  <a v-on:click.stop="doThis"></a>
  ```

- **.prevent**

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

- **.self**

  ```html
  <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
  <!-- 即事件不是从内部元素触发的 -->
  <div v-on:click.self="doThat">...</div>
  ```

- **.once**：2.1.4 新增

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



### 表单输入绑定

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



### 组件基础

示例：

```html
<div id="components-demo">
    <button-counter></button-counter>
</div>

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
  props: ['title'],  								//注意这里的props
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

**//todo 这里没有完全看懂...**



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

有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：

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



### 组件注册

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



### prop

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

**//todo 这里没有完全看懂...**



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