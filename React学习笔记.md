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

  其实 babel 并不是必须的，不过如果使用 JSX，则必须使用 babel 编译，让代码能让浏览器识别。如果不使用 babel 的话，也可以使用 `React.createElement` 底层的 API 来写源代码；不过，它编写的代码非常的繁琐和可读性差。如下：

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

加入 `crossorigin` 是为了防止跨域。

相关可以参考文档 [React Doc - CDN Links](https://reactjs.org/docs/cdn-links.html)

###### 引入可选的 babel

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

相关可以参考文档 [React Doc - Add React to a Website # Quickly Try JSX](https://reactjs.org/docs/add-react-to-a-website.html#quickly-try-jsx)

##### ReactDOM.createRoot

###### 示例如下

```react
const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render( rootJSX )

const app = ReactDOM.createRoot(document.querySelector('#app'))
root.render( appJSX )
```

在没有脚手架的情况下，上面代码需要写在 `<script>` 中。另外，需要注意的是：仅仅是 `<script></script>` 浏览器将不会将通过 babel 进行编译，需要加上：`type="text/babel` 即： `<script type="text/babel'>`

另外，可以多次调用 `React.createRoot ` ，创建多个实例并进行渲染；如上面的 root 和 app。不过，正常情况下一个应用程序只需要创建一个根 ( root ) 。

###### react.render

在 React18 之前，`ReactDOM.createRoot` 对应的 API 是 `React.render`。语法如下：

```react
React.render( appJSX, document.querySelector('#app') )
```









在使用 babel 的情况下，如果 <



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