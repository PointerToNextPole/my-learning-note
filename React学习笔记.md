# React 学习笔记



## coderwhy React18 学习笔记



#### React 介绍

##### React 的使用场景

- React DOM ，包含 SSR ( ReactDOMServer  )
- React Native
- React VR

##### 声明式编程

声明式编程是目前整个大前端开发的模式：Vue、React、Flutter、SwiftUI 。它允许我们只需要维护自己的状态，当状态改变时，React 可以根据最新的状态去渲染我们的 UI 界面。

<img src="https://s2.loli.net/2022/10/02/ZJfk4NSLXHiz3rU.png" alt="A mathematical formula of UI = f(state). 'UI' is the layout on the screen. 'f' is your build methods. 'state' is the application state." style="zoom:85%;" />

> 👀 注：上图摘自 https://docs.flutter.dev/development/data-and-backend/state-mgmt/declarative



#### React hello world

##### 开发依赖

开发 React 必须依赖三个库，<font color=red>这三个库是各司其职的</font>，目的就是让每一个库只单纯做自己的事情。

- **react** ：包含 react 所必须的核心代码

- **react-dom** ：react 渲染在不同平台所需要的核心代码

  其实，在 React 的 0.14 版本之前是没有 react-dom 这个概念的，所有功能都包含在 react 里。

  不过因为 React Native 的出现，让 React DOM 分离出来，从而解耦：把 React DOM 和 React Native 共用的核心代码处理出来，放在 React 部分，其余各自放在 React DOM 和 React Native 中；之后引入新的成员，以此类推。

- **babel** ：将 JSX 转换成 React 代码的工具

  其实 babel 并不是必须的，不过如果使用 JSX，则必须使用 babel 编译，让代码能让浏览器识别。如果不使用 babel 的话，也可以使用 `React.createElement` 底层的 API 来写源代码（ 它是 JSX 的 实现本质，使用 render 方法最后还是会被编译成 `React.createElement` 方法）；不过，它编写的代码非常的繁琐和可读性差。如下：

  > ```react
  > const e = React.createElement;
  > 
  > // Display a "Like" <button>
  > return e(
  >   'button',
  >   { onClick: () => this.setState({ liked: true }) },
  >   'Like'
  > );
  > ```
  >
  > 摘自：[React Doc - Add React to a Website # Optional: Try React with JSX](https://reactjs.org/docs/add-react-to-a-website.html#optional-try-react-with-jsx)

##### CDN 引入依赖

###### 引入 React 和 React DOM

```html
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
```

使用 `crossorigin` 是为了防止跨域。

相关可以参考文档 [React Doc - CDN Links](https://reactjs.org/docs/cdn-links.html)

###### 引入可选的 babel

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

相关可以参考文档 [React Doc - Add React to a Website # Quickly Try JSX](https://reactjs.org/docs/add-react-to-a-website.html#quickly-try-jsx)

###### 引入 babel 所连带的问题

⚠️ 值得注意的是：经过 babel 处理的代码，默认会使用严格模式；主要的影响：this 的 undefined 时的指向；还有尾调用优化等。也正是因为 babel 处理过程中导致 this 的丢失（类似于 `const loseThisFn = this.fn` ），所以需要使用 bind 方法进行绑定 this；详见 [[#类组件#示例]]

##### ReactDOM.createRoot

###### 示例

```react
const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(<h1>root</h1>) // ⚠️ 这里的 jsx 不需要加引号，如果加上引号，将会被理解为字符串

const app = ReactDOM.createRoot(document.querySelector('#app'))
root.render(<h1>app</h1>)
```

> 👀 下面是对上面示例代码的讲解和扩展

###### root 和 app

在 React 中更多使用 root 作为根的变量名，而在 Vue 中则更多使用 app。这里只是顺带一提，不重要。

###### 多元素换行

上面 render 函数中的 jsx，如果根元素下有多个元素，想要分多行写的话，可以通过 `()` 将这些 jsx 包裹，如下示例：

```react
root.render((
  <div>
    <h1>root</h1>
    <p>root cnt</p>
  </div>
))
```

> ⚠️ 注意：render 方法中的 JSX 不允许有多个根

###### 组件化

另外，由于 React 采用了组件化的编程思想，所以 render 函数中除了传入一个 html 元素，也是可以传入一个组件的；如下示例：

```react
root.render(<App/>)
```

这也让代码更易于封装、也更加简洁。具体见 [[#组件化封装]]

###### \<script type="text/babel">

在没有脚手架的情况下，上面 [[#ReactDOM.createRoot#示例]] 中的代码需要写在 `<script>` 中。另外，需要注意 ⚠️ 的是：仅仅是 `<script> ... </script>` 浏览器将不会将通过 babel 进行编译，需要加上 `type="text/babel` ，即： `<script type="text/babel'>`

###### 多实例

另外，可以多次调用 `React.createRoot ` ，创建多个实例并进行渲染；如上面的 root 和 app 。不过，正常情况下一个应用程序只需要创建一个根 ( root ) 。

###### react.render

在 React18 之前，`ReactDOM.createRoot` 对应的 API 是 `React.render`。语法如下：

```react
React.render( rootJSX, document.querySelector('#root') )
```

##### 组件化封装

React 的组件封装分为：类组件 和 函数式组件。

##### 类组件

###### 示例

```react
class App extends React.Component { // 👀 这里 App 是自定义的组件名
  // 组件数据
  constructor() {
    super()
    this.state = { message: 'Hello World' }
  }
  
  // 组件方法
  onBtnClick() {
    this.setState( { message: 'Hello React' } ) // ⚠️ 将会触发（重新）渲染
  }
  
  // 渲染内容 render 方法
  render() {
    return (
      <div>
        <h2>{ this.state.message }</h2>
        <button onClick={ this.onBtnClick.bind(this) }>修改文本</button> <!-- ⚠️ 使用了 bind -->
      </div>
    )
  }
}

const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(<App/>)
```

###### 示例代码解释

setState 方法是来自继承的 `React.Component` 类中，setState 在内部做了两件事：1) 修改 state 中的值 2) 自动重新执行 render 函数

上面使用 bind 的部分原因见 [[#引入 babel 所连带的问题]]。<font color=dodgerBlue>更详细的原因是</font>：

在正常的 DOM 操作中监听点击，监听函数中的 this 其实是节点对象（比如说是 button 对象）；<font color=fuchsia>因为 React 并不是直接渲染成真实的 DOM，我们所编写的 button 只是一个语法糖，它的本质 React 的 Element 对象</font>； 那么在这里发生监听的时候，React 给我们的函数绑定的 this，默认情况下就是一个 undefined。

<font color=dodgerBlue>由于 jsx 中可能出现大量的事件处理，也就会出现大量的 bind 调用，就显得很啰嗦；所以：</font>除了在 jsx 中添加 bind，也可以在 `constructor` 中提前做好绑定，如下示例：

```react
this.onBtnClick = this.onBtnClick.bind(this)
```

这样，对于 “同一个” 事件处理方法，只需要绑定一次即可（上面的 事件处理方法，只要调用一次，就要写一次 bind 方法）。这种方法在 官方文档中也有体现：

> ```react
> this.handleClick = this.handleClick.bind(this);
> ```
>
> 摘自：[React Doc - Handling Events](https://reactjs.org/docs/handling-events.html)

还有，在上面的 JSX 中，要写 `this.state.message` 过于麻烦，可以使用 解构语法，如下示例：

```react
render() {
  const { message } = this.state
  
  return (
    <div>
      <h2>{ message }</h2>
      <button onClick={ this.onBtnClick.bind(this) }>修改文本</button>
    </div>
  )
}
```

###### 使用类组件注意点

- 定义一个类（类名大写。组件的名称是必须大写的，小写会被认为是 HTML 元素），继承自 React.Component
- 必须要实现当前组件的 render 函数

##### React 其他示例注意点

这里课程又讲了两个示例：渲染列表 和 计数器，由于没什么难度，这里略。不过，关于渲染列表还是有些注意内容

###### 渲染列表示例代码

```react
class App extends React.Component {
  constructor() {
    super()
    this.state = { list = [ /*...*/ ] }
  }

  render() {
    return (
      <div>
        <ul>
          { this.state.list.map( item => <li>{item}</li> ) }
        </ul>
      </div>
    )
  }
}
```

###### 注意点

列表渲染使用 map 方法可以说是“最佳实践”了。上面的 `this.state.list.map( item => <li>{item}</li> )` 返回的是一个列表，也就是说：在 React 中的 `{}` 是可以放入一个 jsx 列表，并且无需做其他操作，直接让 React 渲染的。同时，该特性在对象上无法直接实现（除非开发者对 对象做处理... 将其转变成列表）。相关内容可以参考 [[#JSX 嵌入变量]]

除了上面的 map，也可以使用 其他一些 “麻烦的方法”，虽然麻烦但是可以发现 React JSX 中的一些特性：

```react
render() {
  const liEls = []
  for(const el of this.state.list) {
    liEls.push( <li>{el}</li> ) // ⚠️ 注意：在写的过程中，包裹 el 的大括号漏了
  }
  
  return (
    <div>
      <ul>liEls</ul> {/*👀*/}
    </div>
  )
}
```



#### JSX

##### JSX 是什么

JSX 是一种 JavaScript 的语法扩展 ( eXtension ) ，也在很多地方称之为 JavaScript XML ，因为看起就是一段 XML 语法。感觉可以将 JSX 理解为 HTML in JS，另外还有 CSS in JS；所以有一种说法：React 的开发模式是 All in JS

它用于描述我们的 UI  界面，并且其完成可以和 JavaScript 融合在一起使用。

##### 为什么 React 选择了 JSX

> 👀 这部分内容可以参考下 [React Doc - Thinking in React](https://reactjs.org/docs/thinking-in-react.html)

<font color=red>React 认为渲染逻辑本质上与其他 UI 逻辑存在内在耦合</font>：比如 UI 需要绑定事件（button、a原生等）；比如 UI 中需要展示数据状态，在某些状态发生改变时，又需要改变UI。因为 它们之间是密不可分的，所以 React 没有将标记 ( html tag ) 分离到不同的文件中，而是将它们组合到了一起，即组件 Component 中

##### JSX 书写规范

- JSX 顶层只能有一个根元素，所以很多时候会在外层包裹一个 div 原生（或者使用 Fragment ）
- 为了方便阅读，通常在 JSX 外层包裹一个小括号 `()` ，这样可以方便阅读；将整个 JSX 当做一个整体，并且 JSX 可以进行换行书写
- JSX 中的标签可以是单标签，也可以是双标签。⚠️ 注意：如果是单标签，必须以 `/>` 结尾

##### JSX 中的注释

###### 示例

```jsx
{ /* some comment */ }
<div>other tags ...</div>
```

##### JSX 嵌入变量

- 当变量是 Number、String、Array 类型时，可以直接显示

- 当变量是 null、undefined、Boolean类型时，内容为空。

  如果希望可以显示 null、undefined、Boolean，那么需要转成字符串；转换的方式有很多，比如 toString 方法、空字符串拼接，`String(myVariable)` 等方式

- 对象类型不能作为子元素显示 ( Uncaught Error: Object are not valid as a React child )

##### JSX 嵌入表达式

1) 运算表达式，也包含 JS 的计算属性
2) 三元运算符
3) 执行一个函数（ 函数有返回值即可）。👀 感觉类似于 Vue 的计算属性

##### JSX 绑定属性

- 比如元素都会有 title 属性，img 元素会有 src 属性，a 元素会有 href 属性

  ```jsx
  <p title={ titleCnt }>paragraph</p>
  <img src={ srcURL } />
  <a href={ hrefLink }>link</a>
  ```

- 元素可能需要绑定 class，有三种方法

  ###### 方法一

  ```jsx
  {/* classNameStr = `foo bar` */}
  <h2 className={classNameStr}>h2 cnt</h2>
  ```

  ###### 方法二

  ```jsx
  {/* classList = ['foo', 'bar'] */}
  {/* 如果不加上 join，className 将会编译为 class="foo,bar"，显然不是我们想要的 */}
  <h2 className={classList.join(' ')}>h2 cnt</h2>
  ```

  ###### 方法三

  使用第三方库 [classnames](https://github.com/JedWatson/classnames) ，这里暂时略

- 原生使用内联样式 style

  ###### 方法一

  ```jsx
  <h2 style={ {color: 'red', fontSize: '30px'} }>h2 cnt</h2>
  ```

  ###### 方法二

  ```jsx
  {/* styleObj: {color: 'red', fontSize: '30px'} */}
  <h2 style={styleObj}>h2 cnt</h2>
  ```

  



## 其他笔记



#### 控制反转

有个设计模式叫：[控制反转](https://zh.wikipedia.org/wiki/%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC) 。

<font color=red>**在 React 中通常都是使用 `<Button />` 而不是 `Button()`**</font> 。 

因为<font color=fuchsia>只有写成 `<Button />` 的时候，React 才能够更好的知道 Button 组件的存在</font>；举个例子：

```react
// React 并不知道 ButtonGroup 和 Button 的存在。
// 因为你在调用它们
ReactDOM.render(
  ButtonGroup({ children: Button() }),
  document.getElementById('root')
)

// ✅ React 知道 ButtonGroup 和 Button 的存在
// React 来调用它们。
ReactDOM.render(
  <ButtonGroup><Button /></ButtonGroup>,
  document.getElementById('root')
)
```

<font color=fuchsia>把调用组件的操作交给 React 去做，React 可以更好的协调，避免没必要的代码执行和渲染，他可以让浏览器在组件调用之间做一些工作，这样渲染大量的组件树就不会阻塞主线程</font>。这里 Button 只是一个简单的示例，在复杂的业务场景中，会有很多层的组件嵌套的情况。

摘自：[React的两种渲染方式有什么区别？ - 郭小铭的回答 - 知乎](https://www.zhihu.com/question/548006973/answer/2623307955)