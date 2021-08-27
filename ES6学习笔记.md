# ES6学习笔记



#### 模版字符串

模板字面量 是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。它们在ES2015规范的先前版本中被称为“模板字符串”。

**描述：**模板字符串使用反引号 来代替普通字符串中的用双引号和单引号。模板字符串可以包含特定语法（`${expression}`）的占位符。占位符中的表达式和周围的文本会一起传递给一个默认函数，该函数负责将所有的部分连接起来，如果一个模板字符串由表达式开头，则该字符串被称为带标签的模板字符串，该表达式通常是一个函数，它会在模板字符串处理后被调用，在输出最终结果前，你都可以通过该函数来对模板字符串进行操作处理。在模版字符串内使用反引号（`）时，需要在它前面加转义符（\）。

摘自：[MDN - 模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)



#### 展开运算符

- **函数传参**：<font color=FF0000>展开运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了</font>。

  示例：

  ```js
  // ES5 的写法
  function f(x, y, z) {
  // ...
  }
  var args = [0, 1, 2];
  f.apply(null, args);
  
  // ES6 的写法
  function f(x, y, z) {
  // ...
  }
  var args = [0, 1, 2];
  f(...args);
  ```

- **数据解构**：其实就是把数组的每个数据拆开然后放进去。

  示例：

  ```js
  let arr = ['autumn', 'wscats'];
  // 析构数组
  let y;
  [autumn, ...y] = arr;
  console.log(y) // 结果为：["wscats"]
  ```

- **数据构造**：

  - 两个对象连接返回新的对象

    ```js
    let x = {name: 'autumn'}
    let y = {age: 18}
    let z = {...x,...y}
    console.log(z)  //结果为 {name: "autumn", age: 18}
    ```

  - 两个数组连接返回新的数组

    ```js
    let x = ['autumn']
    let y = ['wscats']
    let z = [...x, ...y]
    console.log(z)// ["autumn", "wscats"]
    ```

  - 数组加上对象返回新的数组

    ```js
    let x = [{name: 'autumn'}]
    let y = {name: 'wscats'}
    let z = [...x, y];
    console.log(z);  
    /**
     0: {name: "autumn"}
     1: {name: "wscats"}
     length: 2
     __proto__: Array(0)
    */
    ```

  - 数组+字符串

    ```js
    let x = ['autumn'];
    let y = 'wscats';
    let z = [...x, y];
    console.log(z);
    /**
     0: "autumn"
     1: "wscats"
     length: 2
     __proto__: Array(0)
    */
    ```

  - 数组+对象

    ```js
    let x = {
    	name: ['autumn','wscats'],
    	age:18
    }
    let y = {
    	...x,             //name: ['autumn','wscats'], age:18
    	arr: [...x.name]  //['autumn','wscats']
    }
    console.log(y)
    /**
    	age: 18
    	arr: (2) ["autumn", "wscats"]
    	name: (2) ["autumn", "wscats"]
    	__proto__: Object
    */
    ```

摘自：[ES6(...)展开运算符](https://blog.csdn.net/weixin_41615439/article/details/85319618)



#### export & import & export defalut

前言：

> “分而治之”的思想在计算机的世界非常普遍，但是在 ES2015 标准出现以前（*不了解没关系，后面会讲到*）， JavaScript 语言定义本身并没有模块化的机制，构建复杂应用也没有统一的接口标准。人们通常使用一系列的 `<script>` 标签来导入相应的模块（依赖）：
>
> ```js
> <head>
>   <script src="fileA.js"></script>
>   <script src="fileB.js"></script>
> </head>
> ```
>
> **这种组织 JS 代码的方式有很多问题，其中最显著的包括：**
>
> - <mark>导入的多个 JS 文件直接作用于全局命名空间，很容易产生**命名冲突**</mark>
> - <mark>导入的 JS 文件之间不能相互访问，例如 fileB.js 中无法访问 fileA.js 中的内容，很不方便</mark>
> - <mark>导入的 `<script>` 无法被轻易去除或修改</mark>
>
> ECMAScript 2015（也就是大家常说的 ES6）标准为 JavaScript 语言引入了全新的模块机制（称为 ES 模块，全称 ECMAScript Modules），并提供了 import 和 export 关键词。但是截止目前，Node.js 对 ES 模块的支持还处于试验阶段，因此这篇文章不会讲解、也不提倡使用。

- export用于<font color=FF0000>对外输出本模块（一个文件可以理解为一个模块）变量的接口</font>
- import用于<font color=FF0000>在一个模块中加载另一个含有export接口的模块</font>

**也就是说使用export命令定义了模块的对外接口以后，其他JS文件就可以通过import命令加载这个模块（文件）**

- export一个或多个模块：`export {module1[, module2, ...]}`
- import一个或多个模块：`import {module1[, module2, ...]} from pathStr`

**export与export default 的区别**

- export与export default均可用于导出常量、函数、文件、模块等

- 你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用

- <font color=FF0000>在一个文件或模块中，**export、import可以有多个**，**export default仅有一个**</font>
- <font color=FF0000>通过export方式导出，在导入时要加{ }，export default则不需要</font>

摘自：[认识Vue 的 export、export default、import](https://blog.csdn.net/harry5508/article/details/84025146)

#### **补充：** 

- 通常情况下，export输出的变量就是本来的名字，但是可以使用as关键字重命名。

  ```js
  function v1() { ... }
  function v2() { ... }
  
  export {
    v1 as streamV1,
    v2 as streamV2,
    v2 as streamLatestVersion
  };
  ```

- export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

  如下两种写法都会报错，因为没有提供对外的接口。第一种写法直接输出 1，第二种写法通过变量m，还是直接输出 1。1只是一个值，不是接口。

  ```js
  // 报错
  export 1;
  
  // 报错
  var m = 1;
  export m;
  ```

  正确的写法是下面这样。

  ```js
  // 写法一
  export var m = 1;
  
  // 写法二
  var m = 1;
  export {m};
  
  // 写法三
  var n = 1;
  export {n as m};
  ```

  同样的，function和class的输出，也必须遵守这样的写法。

  ```javascript
  // 报错
  function f() {}
  export f;
  
  // 正确
  export function f() {};
  
  // 正确
  function f() {}
  export {f};
  ```

- 如果想为输入的变量重新取一个名字，<font color=FF0000>import命令要使用as关键字，将输入的变量重命名</font>。示例如下：

  ```js
  import { lastName as surname } from './profile.js';
  ```

- <font color=FF0000>import命令输入的变量都是**只读的**，因为它的**本质是输入接口**</font>。也就是说，不允许在加载模块的脚本里面，改写接口。

  同时：由于import是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。

- <font color=FF0000>import命令具有提升效果，会提升到整个模块的头部，首先执行。</font>

##### **关于export default**

<font color=FF0000>使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载</font>。但是，用户肯定希望快速上手，未必愿意阅读文档，去了解模块有哪些属性和方法。<font color=FF0000>为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到`export default`命令，为模块指定默认输出</font>。示例如下：

```js
// 文件export-default.js中
export default function () {
  console.log('foo');
}
```

其他模块加载该模块时，`import`命令可以为该匿名函数指定任意名字。

```js
// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

<font color=FF0000>**export default命令用在非匿名函数前，也是可以的。**</font>

```javascript
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成
function foo() {
  console.log('foo');
}

export default foo;
export {foo as default}; //这样写也是可以的
```





## 《深入理解ES6》阅读笔记

