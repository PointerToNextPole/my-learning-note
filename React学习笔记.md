# React 学习笔记



## 官方文档阅读笔记



#### Quick Start

##### Creating and nesting components

<font color=dodgerBlue>Notice that `<MyButton />` starts with a capital letter</font>. That’s how you know it’s a React component. <font color=red>**React component names must always start with a capital letter**</font>, <font color=dodgerBlue>**while**</font> <font color=red>HTML tags must be lowercase</font>.

##### Writing markup with JSX

<font color=red>JSX is stricter than HTML</font>. You have to close tags like `<br />`. <font color=red>Your component also can’t return multiple JSX tags</font>. You have to wrap them into a shared parent, like a `<div>...</div>` <font color=red>or an empty `<>...</>` wrapper</font>:

```jsx
function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```

##### Rendering lists

```jsx
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

Notice how `<li>` has a `key` attribute. For each item in a list, you <font color=red>should pass a string or a number that **uniquely identifies** that item among its siblings</font>. Usually, a key should be coming from your data, such as a database ID. React uses your keys to know what happened if you later insert, delete, or reorder the items.

##### Responding to events

```jsx
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

<font color=fuchsia>Notice how `onClick={handleClick}` has no parentheses at the end</font>! Do not *call* the event handler function: you only need to *pass it down*. React will call your event handler when the user clicks the button.

##### Updating the screen 

You’ll get two things from `useState`: the current state (`count`), and the function that lets you update it (`setCount`). You can give them any names, but the convention is to write `[something, setSomething]`.

The first time the button is displayed, `count` will be `0` because you passed `0` to `useState()`. When you want to change state, <font color=fuchsia>call `setCount()` and **pass the new value to it**</font>. Clicking this button will increment the counter:

```jsx
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1); // 👀
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

> ⚠️ 值得注意的是：不可以直接修改 `count` 的值 ，比如 `setCount(count ++)` ，首先 count 是 const 定义的；另外，也是会报错的

##### Using Hooks

Functions starting with `use` are called *Hooks*. `useState` is a built-in Hook provided by React. <font color=lightSeaGreen>You can find other built-in Hooks in the [API reference](https://react.dev/reference/react)</font> . <font color=red>You can also write your own Hooks by combining the existing ones</font>.

<font color=red>**Hooks are more restrictive than other functions**</font>. You <font color=fuchsia>**can only**</font> <font color=red>call Hooks *at the top* of your components (or other Hooks)</font>. <font color=dodgerBlue>If you want to use `useState` in a condition or a loop</font>, <font color=fuchsia>**extract a new component and put it there**</font>.

摘自：[React doc - Quick Start](https://react.dev/learn)



#### Thinking in React

##### Step 1: Break the UI into a component hierarchy

Depending on your background, you can think about splitting up a design into components in different ways:

- **Programming**—use the same techniques for deciding if you should create a new function or object. <font color=red>One such technique is the **single responsibility principle**</font> , <font color=lightSeaGreen>that is, a component should ideally only do one thing</font>. If it ends up growing, it should be decomposed into smaller subcomponents.

  > 💡single responsibility principle 即 “单一职责原则”

- **CSS**—consider what you would make class selectors for. (However, components are a bit less granular.)

- **Design**—consider how you would organize the design’s layers.

##### Step 2: Build a static version in React

To build a static version of your app that renders your data model, you’ll want to build [components](https://react.dev/learn/your-first-component) that reuse other components and pass data using [props.](https://react.dev/learn/passing-props-to-a-component) <font color=dodgerBlue>**Props are a way of passing data from parent to child**</font>. (If you’re familiar with the concept of [state](https://react.dev/learn/state-a-components-memory), <font color=red>don’t use state at all to build this **static version**</font>. <font color=fuchsia>**State is reserved only for interactivity**</font>, that is, <font color=red>data that changes over time</font>. Since this is a static version of the app, you don’t need it.)

You can either build <font color=lightSeaGreen>**“top down”**</font> by starting with building the components higher up in the hierarchy (like `FilterableProductTable`) or <font color=lightSeaGreen>**“bottom up”**</font> by working from components lower down (like `ProductRow`). <font color=dodgerBlue>In simpler examples</font>, <font color=red>it’s usually easier to go top-down</font>, and <font color=dodgerBlue>on larger projects</font>, <font color=red>it’s easier to go bottom-up</font>.

> 👀 这里 “top down” 是自顶向下，“bottom up” 是自底向上

The component at the top of the hierarchy (`FilterableProductTable`) will take your data model as a prop. <font color=red>This is called ***one-way data flow*** because the data flows down from the top-level component to the ones at the bottom of the tree</font>.

##### Step 3: Find the minimal but complete representation of UI state

<font color=dodgerBlue>Which of these are state? **Identify the ones that are not**:</font>

- Does it <font color=red>**remain unchanged** over time</font>? If so, it <font color=lightSeaGreen>isn’t state</font>.
- Is it <font color=red>**passed in from a parent** via props</font>? If so, it <font color=lightSeaGreen>isn’t state</font>.
- **Can you <font color=red>compute it</font>** <font color=red>based on existing state or props in your component</font>? If so, it *definitely* <font color=lightSeaGreen>isn’t state</font>!

<font color=dodgerBlue>**What’s left is probably state.**</font>

###### Props vs State

There are two types of “model” data in React: props and state. <font color=dodgerBlue>The two are very different</font>:

- [**Props** are like arguments you pass](https://react.dev/learn/passing-props-to-a-component) to a function. They let a parent component pass data to a child component and customize its appearance. For example, a `Form` can pass a `color` prop to a `Button`.
- [**State** is like a component’s memory.](https://react.dev/learn/state-a-components-memory) It lets a component keep track of some information and change it in response to interactions. For example, a `Button` might keep track of `isHovered` state.

##### Step 4: Identify where your state should live

> 👀 这里的 “live” 是 “放置” 的意思

<font color=dodgerBlue>**For each piece of state in your application:**</font>

1. Identify *every* component that renders something based on that state.
2. Find their closest common parent component—a component above them all in the hierarchy.
3. Decide where the state should live:
   1. Often, you can put the state directly into their common parent.
   2. You can also put the state into some component above their common parent.
   3. If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common parent component.



#### Using TypeScript

Out of the box, TypeScript [supports JSX](https://react.dev/learn/writing-markup-with-jsx) and <font color=lightSeaGreen>you can get full React Web support by adding `@types/react` and `@types/react-dom` to your project</font>.

###### Adding TypeScript to an existing React project 

To install the latest version of React’s type definitions:

```sh
npm install @types/react @types/react-dom
```

<font color=dodgerBlue>**The following compiler options need to be set in your `tsconfig.json`:**</font>

1. <font color=red>`dom` must be included in [`lib`](https://www.typescriptlang.org/tsconfig/#lib)</font> (Note: If no `lib` option is specified, `dom` is included by default).

   > 👀 这里的 `lib` 是 `tsconfig.json` 中的字段

2. [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) must be set to one of the valid options. `preserve` should suffice for most applications. If you’re publishing a library, consult the [`jsx` documentation](https://www.typescriptlang.org/tsconfig/#jsx) on what value to choose.

##### TypeScript with React Components

> 💡 Note
>
> Every file containing JSX <font color=red>must use the `.tsx` file extension</font>. This is a TypeScript-specific extension that tells TypeScript that this file contains JSX.

Taking the [`MyButton` component](https://react.dev/learn#components) from the [Quick Start](https://react.dev/learn) guide, we can add a type describing the `title` for the button:

```tsx
/* App.tsx */
function MyButton({ title }: { title: string }) { // 👀
  return (
    <button>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="I'm a button" />
    </div>
  );
}
```

The type describing your component’s props can be as simple or as complex as you need, <font color=red>though they should be an object type described with either a `type` or `interface`.</font>



#### Your First Component

##### Defining a component 

Traditionally when creating web pages, web developers marked up their content and then added interaction by sprinkling on some JavaScript. This worked great when interaction was a nice-to-have on the web. Now it is expected for many sites and all apps. <font color=dodgerBlue>React puts interactivity first while still using the same technology</font>: <font color=red>**a React component is a JavaScript function that you can *sprinkle with markup*.**</font> Here’s what that looks like (you can edit the example below):

```react
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

> ⚠️ Pitfall
>
> React components are regular JavaScript functions, but <font color=fuchsia>**their names must start with a capital letter** or they won’t work!</font>

<font color=dodgerBlue>**And here’s how to build a component:**</font>

<font color=red>Return statements **can** be written all on one line</font>, as in this component:

```react
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

But <font color=dodgerBlue>if your markup **isn’t all on the same line** as the `return` keyword</font>, you <font color=red>**must wrap it in a pair of parentheses**</font>:

```react
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

> ⚠️ Pitfall
>
> Without parentheses, any code on the lines after `return` [will be ignored](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)!

##### Using a component

```react
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

###### What the browser sees

<font color=dodgerBlue>Notice the difference in casing:</font>

- `<section>` is lowercase, so React knows we refer to an HTML tag.
- `<Profile />` starts with a capital `P`, so React knows that we want to use our component called `Profile`.



#### Writing Markup with JSX

*JSX* is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file. <font color=lightSeaGreen>Although there are other ways to write components</font>, <font color=red>most React developers prefer the conciseness of JSX, and most codebases use it</font>.

> 💡 关于 jsx 和 js
>
> > `.jsx` 表示这是一个 JavaScript XML 文件
> >
> > JavaScript 是能够被浏览器直接识别的，JavaScript XML 需要经过编译器（webpack 等 👀 babel ）转换成 JavaScript
> >
> > **但<font color=red>在正常使用上，两者没有什么区别</font>，<font color=fuchsia>`.js` 的语法和`.jsx` 的后缀可以互换，语法上也完全兼容</font>**
> >
> > `.ts` 的文件，内容上不支持 `<div>` 这种HTML语法，会报错，而且 VS Code 这类代码编辑器也不会提供相关代码提示和补全的功能。反之， `.tsx` 的文件，在遵循TypeScript的基础上，支持 JSX 语法。
> >
> > 所以我们在使用时，辅助的函数文件使用 `.ts` 即可；React 组件方面，还是必须使用`.tsx`
> >
> > 摘自：[JS 和 JSX 、TS 和 TSX 的区别](https://zhuanlan.zhihu.com/p/625039917)

##### JSX: Putting markup into JavaScript

<font color=red>Keeping a button’s rendering logic and markup together ensures that they stay in sync with each other on every edit</font>. Conversely, details that are unrelated, such as the button’s markup and a sidebar’s markup, <font color=lightSeaGreen>are isolated from each other</font>, <font color=red>making it safer to change either of them on their own</font>.

Each React component is a JavaScript function that may contain some markup that React renders into the browser. <font color=lightSeaGreen>React components use a syntax extension called JSX to represent that markup</font>. JSX looks a lot like HTML, but <font color=red>it is a bit stricter and can display dynamic information</font>. The best way to understand this is to convert some HTML markup to JSX markup.

> 💡 Note
>
> <font color=dodgerBlue>JSX and React are two separate things</font>. They’re often used together, but you *can* [use them independently](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform) of each other. <font color=lightSeaGreen>**JSX is a syntax extension, while React is a JavaScript library**</font>.

##### The Rules of JSX

###### 1. Return a single root element

To return multiple elements from a component, **wrap them with a single parent tag.**

you can use a `<div>` , <font color=dodgerBlue>If you don’t want to add an extra `<div>` to your markup</font>, you can <font color=lightSeaGreen>write `<>` and `</>` instead</font>.

This empty tag is called a [Fragment](https://react.dev/reference/react/Fragment) . <font color=red>Fragments let you group things without leaving any trace in the browser HTML tree</font>.

> 💡 **Why do multiple JSX tags need to be wrapped?**
>
> <font color=dodgerBlue>JSX looks like HTML</font>, but <font color=fuchsia>under the hood it is **transformed into plain JavaScript objects**</font>. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment.

##### 2. Close all the tags

JSX requires tags to be explicitly closed: self-closing tags like `<img>` must become `<img />`

##### 3. camelCase ~~all~~ most of the things!

<font color=red>JSX turns into JavaScript and attributes written in JSX **become keys of JavaScript objects**</font>. In your own components, you will often want to read those attributes into variables. But <font color=lightSeaGreen>JavaScript has limitations on variable names</font>. For example, their names can’t contain dashes or be reserved words like `class`.

This is why, in React, many HTML and SVG attributes are written in camelCase. For example, <font color=red>instead of `stroke-width` you use `strokeWidth`</font>. <font color=fuchsia>Since `class` is a reserved word</font>, in React you write `className` instead, named after the [corresponding DOM property](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)

> 👀 因为 jsx 最后会转变为 js obj ，所以 jsx 中的 attr 也会变成 obj 中的属性；也正因此：因为 `class` 是 js 中的保留字，无法使用它作为一个变量/对象属性，所以使用 `className` 替代

You can [find all these attributes in the list of DOM component props.](https://react.dev/reference/react-dom/components/common) If you get one wrong, don’t worry—React will print a message with a possible correction to the [browser console.](https://developer.mozilla.org/docs/Tools/Browser_Console)

> ⚠️ Pitfall
>
> For historical reasons, <font color=red>`aria-*` and **`data-*`** attributes are written as in HTML with dashes</font>.



#### JavaScript in JSX with Curly Braces

##### Using curly braces: A window into the JavaScript world

JSX is a special way of writing JavaScript. That means it’s possible to <font color=red>**use JavaScript inside it**—with curly braces `{ }`</font>. The example below first declares a name for the scientist, `name`, then embeds it with curly braces inside the `<h1>` :

> 👀 上面说的是：可以在 `{}` 中使用 js，不过，没有说使用 js 中的什么；可以参考下 [[#JavaScript in JSX with Curly Braces#Recap]]
>
> 就下面的示例试了下，下面的示例初始只有 `<h1>{name}'s To Do List</h1>` ，这和 Vue template 插值表达式没什么差别；又试了一下 `{}` 中加入模版字符串，也是同样的效果

<img src="https://s2.loli.net/2023/09/22/4EkbS9ZvLjGUT5g.png" alt="image-20230922161546833" style="zoom:40%;" />

###### Where to use curly braces 

You can only use curly braces in two ways inside JSX:

1. **As text** directly inside a JSX tag: `<h1>{name}'s To Do List</h1>` works, but <font color=red>`<{tag}>Gregorio Y. Zara's To Do List</{tag}>` will not</font>.
2. **As attributes** immediately following the `=` sign: `src={avatar}` will read the `avatar` variable, but `src="{avatar}"` will pass the string `"{avatar}"` .

##### Using “double curlies”: CSS and other objects in JSX

<font color=dodgerBlue>In addition to strings, numbers, and other JavaScript expressions</font>, you <font color=red>can even pass objects in JSX</font>. Objects are also denoted with curly braces, like `{ name: "Hedy Lamarr", inventions: 5 }`. Therefore, to pass a JS object in JSX, you <font color=red>**must wrap the object in another pair of curly braces: `person={{ name: "Hedy Lamarr", inventions: 5 }}`**</font>.

You may see this with inline CSS styles in JSX. React does not require you to use inline styles (CSS classes work great for most cases). But <font color=lightSeaGreen>when you need an inline style, you **pass an object to the `style` attribute**</font>:

```jsx
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```

You can really see the JavaScript object inside the curly braces when you write it like this:

```jsx
<ul style={
  {
    backgroundColor: 'black',
    color: 'pink'
  }
}>
```

> ⚠️ Pitfall
>
> Inline `style` properties are written in camelCase. For example, HTML `<ul style="background-color: black">` would be written as `<ul style={{ backgroundColor: 'black' }}>`  in your component.

##### Recap

<font color=dodgerBlue>Now you know almost everything about JSX:</font>

- JSX attributes inside quotes are passed as strings.
- Curly braces let you <font color=red>**bring JavaScript logic and variables into your markup**</font>.
- They work inside the JSX tag content or immediately after `=` in attributes.
- `{{` and `}}` is not special syntax: it’s a JavaScript object tucked inside JSX curly braces.



#### Passing Props to a Component

##### Specifying a default value for a prop

If you want to <font color=lightSeaGreen>give a prop a default value to fall back on when no value is specified</font>, you can do it with the destructuring by putting `=` and the default value right after the parameter:

```react
function Avatar({ person, size = 100 }) {
  // ...
}
```

Now, if `<Avatar person={...} />` is rendered with no `size` prop, the `size` will be set to `100`.

The default value is only used if the `size` prop is missing or if you pass `size={undefined}`. But <font color=lightSeaGreen>if you pass `size={null}` or `size={0}`, the default value will **not** be used</font>.

##### Forwarding props with the JSX spread syntax 

Sometimes, passing props gets very repetitive:

```jsx
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

There’s nothing wrong with repetitive code—<font color=lightSeaGreen>it can be more legible</font>. But at times you may value （👀 重视）conciseness. Some components forward（👀 转发） all of their props to their children, like how this `Profile` does with `Avatar`. <font color=dodgerBlue>**Because they don’t use any of their props directly**</font>, it <font color=red>can make sense to use a more concise “spread” syntax</font>:

```jsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} /> {/* 👀 */}
    </div>
  );
}
```

> 👀 这样的写法有点类似于 Vue 中的 `v-bind="propObj"`

This forwards all of `Profile`’s props to the `Avatar` <font color=lightSeaGreen>without listing each of their names</font>.

<font color=red>**Use spread syntax with restraint.**</font> If you’re using it in every other component, something is wrong. Often, it indicates that you should split your components and pass children as JSX.

> 👀 如上面所说 “Use spread syntax with restraint” ，确实有必要克制使用。
>
> 个人感觉：首先，这相当于无脑地将对象所有的属性传给了子组件；有些时候，这是比较危险的；毕竟没有做好应该暴露哪些成员的设置，这个数据暴露的“粒度”有些大。其次，这也牺牲了代码的可读性，开发者不知道自己传递了什么给子组件

##### Passing JSX as children

> 👀 感觉可以简单理解为插槽，虽然只能传递一个 child；这和 Vue 或者 webComponent 的 slot 定义还是有较大区别

When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. For example, the `Card` component below will receive a `children` prop set to `<Avatar />` and render it in a wrapper div:

```jsx
import Avatar from './Avatar.js';

function Card({ children }) { { /* ⚠️ 注意：这里的 children 是要设置的！不要忘了*/}
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      { /* 👀  */ }
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```

> 👀 这里省去 Avatar.js 和 utils.js 的定义，这并不影响理解

Try replacing the `<Avatar>` inside `<Card>` with some text to see how the <font color=lightSeaGreen>`Card` component can wrap any nested content</font>. <font color=red>**It doesn’t need to “know” what’s being rendered inside of it**</font>. You will see this flexible pattern in many places.

You can think of <font color=fuchsia>a component with a `children` prop as having a “hole” that can be “filled in” by its parent components with arbitrary JSX</font>. You will often use the `children` prop for visual wrappers: panels, grids, etc.

<img src="https://s2.loli.net/2023/09/23/SER7qUYL4diNfZ9.png" style="zoom:18%;" />

##### How props change over time

The `Clock` component below receives two props from its parent component: `color` and `time`. (The parent component’s code is omitted because it uses [state](https://react.dev/learn/state-a-components-memory), which we won’t dive into just yet.)

<img src="https://s2.loli.net/2023/09/23/gKAW8dc7CPkEHDw.png" alt="image-20230923022026639" style="zoom:50%;" />

This example illustrates that **a component <font color=red>may receive different props over time</font>.** <font color=fuchsia>Props are not always static!</font> Here, the <font color=lightSeaGreen>`time` prop changes every second</font>, and the <font color=lightSeaGreen>`color` prop changes when you select another color</font>. Props reflect a component’s data at any point in time, rather than only in the beginning.

However, <font color=fuchsia>**props are [immutable](https://en.wikipedia.org/wiki/Immutable_object)** — a term from computer science meaning “unchangeable”</font>. <font color=dodgerBlue>**When a component needs to change its props**</font> (for example, in response to a user interaction or new data), <font color=fuchsia>**it will have to “ask” its parent component to pass it *different props*—a new object**!</font> Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them.

<font color=fuchsia>**Don’t try to “change props”.**</font> <font color=dodgerBlue>When you need to respond to the user input</font> (like changing the selected color), you will <font color=red>need to “set state”</font>, which you can learn about in [State: A Component’s Memory.](https://react.dev/learn/state-a-components-memory)

##### Recap

- To pass props, add them to the JSX, just like you would with HTML attributes.
- To read props, use the `function Avatar({ person, size })` destructuring syntax.
- You can specify a default value like `size = 100`, which is used for missing and `undefined` props.
- You can forward all props with `<Avatar {...props} />` JSX spread syntax, but don’t overuse it!
- Nested JSX like `<Card><Avatar /></Card>` will appear as `Card` component’s `children` prop.
- Props are read-only snapshots in time: every render receives a new version of props.
- You can’t change props. When you need interactivity, you’ll need to set state.



#### Conditional Rendering

Your components will often need to display different things depending on different conditions. In React, <font color=lightSeaGreen>you can conditionally render JSX using JavaScript syntax like `if` statements, `&&`, and `? :` operators</font>.

##### Conditionally returning nothing with `null`

<font color=dodgerBlue>In some situations, you won’t want to render anything at all</font>. For example, say you don’t want to show packed items at all. <font color=fuchsia>**A component must return something**</font>. In this case, you can return `null`:

> 👀 一个组件的“工厂函数”必须要有返回内容，如果确实没有，返回 `null`

```jsx
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

If `isPacked` is true, the component will return nothing, `null`. Otherwise, it will return JSX to render.

In practice, <font color=red>**returning `null` from a component isn’t common**</font> because it might surprise a developer trying to render it. <font color=dodgerBlue>**More often**</font>, you would <font color=fuchisa>**conditionally include or exclude the component in the parent component’s JSX**</font>.

##### Conditional (ternary) operator (`? :`)

```jsx
if (isPacked) {
  return <li className="item">{name} ✔</li>;
}
return <li className="item">{name}</li>;
```

You can write this:

```jsx
return (
  <li className="item">
    {isPacked ? name + ' ✔' : name}
  </li>
);
```

###### Are these two examples fully equivalent?

<font color=dodgerBlue>If you’re coming from an object-oriented programming background</font>, you might assume that the two examples above are subtly different because one of them may create two different “instances” of `<li>`. But <font color=fuchsia>JSX elements aren’t “instances”</font> <font color=red>because they don’t hold any internal state and aren’t real DOM nodes</font>. They’re lightweight descriptions, like blueprints. <font color=fuchsia>So these two examples, in fact, *are* **completely equivalent**</font>. [Preserving and Resetting State](https://react.dev/learn/preserving-and-resetting-state) goes into detail about how this works.

##### Logical AND operator (`&&`)

<font color=dodgerBlue>Another common shortcut</font> you’ll encounter is the JavaScript logical AND (`&&`) operator. Inside React components, it often comes up when you want to render some JSX when the condition is true, **or render nothing otherwise.** With `&&`, you could conditionally render the checkmark only if `isPacked` is `true`:

```jsx
return (
  <li className="item">
    {name} {isPacked && '✔'}
  </li>
);
```

You can read this as *“if `isPacked`, then (`&&`) render the checkmark, otherwise, render nothing”*.

> 👀 就这里的代码，感觉使用 `&&` 是一种更简洁的写法

A JavaScript && expression <font color=red>**returns the value of its right side**</font> (in our case, the checkmark) <font color=dodgerBlue>if the left side (our condition) is `true`</font>. But if the condition is `false`, the whole expression becomes `false`. <font color=red>React considers `false` as a “hole” in the JSX tree, just like `null` or `undefined`, and doesn’t render anything in its place</font>.

> ⚠️ Pitfall
>
> **Don’t put numbers on the left side of `&&`.**
>
> To test the condition, JavaScript converts the left side to a boolean automatically. However, if the left side is `0`, then the whole expression gets that value (`0`), and React will happily render `0` rather than nothing.
>
> For example, a common mistake is to write code like `messageCount && <p>New messages</p>`. It’s easy to assume that it renders nothing when `messageCount` is `0`, but it really renders the `0` itself!
>
> <font color=dodgerBlue>To fix it</font>, <font color=red>make the left side a boolean</font>: `messageCount > 0 && <p>New messages</p>`.

##### Conditionally assigning JSX to a variable

> 👀 除了上述的方法，还可以将逻辑从 jsx 中拎出来，放到 `return` 前面

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✔";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}
```

<font color=dodgerBlue>This style is the most verbose</font>, but <font color=red>it’s also the most flexible</font>.

Like before, <font color=red>this works not only for text</font>, <font color=fuchsia>**but for arbitrary JSX too**</font>:

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = ( { /* 👀 */ }
      <del>
        {name + " ✔"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}
```

> 👀 这是之前没有想到的，不过这也和 “父组件将 jsx 传给子组件” 思路是一样的

##### Recap

- In React, you control branching logic with JavaScript.
- You can return a JSX expression conditionally with an `if` statement.
- You can conditionally save some JSX to a variable and then include it inside other JSX by using the curly braces.
- In JSX, `{cond ? <A /> : <B />}` means *“if `cond`, render `<A />`, otherwise `<B />`”*.
- In JSX, `{cond && <A />}` means *“if `cond`, render `<A />`, otherwise nothing”*.
- The shortcuts are common, but you don’t have to use them if you prefer plain `if`.



#### Rendering Lists

You will often want to display multiple similar components from a collection of data. You can use the [JavaScript array methods](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) to manipulate an array of data.

##### Rendering data from arrays

```jsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

> 👀 playground 也给出提示 “Warning: Each child in a list should have a unique "key" prop.” ，显然这里 map 中的 `<li>` 缺少了 `key` 

##### Keeping list items in order with `key`

You need to give each array item a `key` — a string or a number that uniquely identifies it among other items in that array:

```jsx
<li key={person.id}>...</li>
```

> 💡 Note
>
> JSX elements directly inside a `map()` call <font color=fuchsia>always need keys</font>!

<font color=red>Keys tell React which array item each component corresponds to</font>, so that it can match them up later. <font color=red>This becomes important if your array items can move</font> (e.g. due to sorting), <font color=red>get inserted, or get deleted</font>. A well-chosen `key` helps React infer what exactly has happened, and make the correct updates to the DOM tree.

> 💡 DEEP DIVE
>
> ###### Displaying several DOM nodes for each list item
>
> <font color=dodgerBlue>What do you do when each item needs to render not one, but several DOM nodes?</font>
>
> <font color=red>The short [`<>...` Fragment](https://react.dev/reference/react/Fragment) syntax **won’t let you pass a key**</font>, so you need to either group them into a single `<div>`, or <font color=red>use the slightly longer and [more explicit `<Fragment>` syntax:](https://react.dev/reference/react/Fragment#rendering-a-list-of-fragments)</font>
>
> ```jsx
> import { Fragment } from 'react';
> 
> // ...
> 
> const listItems = people.map(person =>
>   <Fragment key={person.id}>
>     <h1>{person.name}</h1>
>     <p>{person.bio}</p>
>   </Fragment>
> );
> ```

###### Where to get your `key`

<font color=dodgerBlue>Different sources of data provide different sources of keys:</font>

- **Data from a database:** <font color=dodgerBlue>If your data is coming from a database</font>, you <font color=red>can use the database keys/IDs</font>, which are unique by nature.
- **Locally generated data:** <font color=dodgerBlue>**If your data is generated and persisted locally**</font> (e.g. notes in a note-taking app), <font color=fuchsia>use an incrementing counter, `crypto.randomUUID()` or a package like [`uuid`](https://www.npmjs.com/package/uuid) when creating items</font>.

###### Rules of keys

- **Keys must be unique among siblings.** However, it’s okay to use the same keys for JSX nodes in *different* arrays.
- <font color=red>**Keys must not change**</font> or that defeats their purpose! Don’t generate them while rendering.

###### Why does React need keys?

> ⚠️ Pitfall
>
> <font color=fuchsia>You might be tempted to use an item’s index in the array as its key</font>. In fact, <font color=fuchsia>**that’s what React will use if you don’t specify a `key` at all**</font>. But <font color=red>the order in which you render items will change over time</font> if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs.
>
> Similarly, <font color=red>**do not generate keys on the fly, e.g. with `key={Math.random()}`**</font> . <font color=fuchsia>This will cause keys to **never match up between renders**, leading to all your components and DOM being recreated every time</font>. <font color=lightSeaGreen>Not only is this slow</font>, but <font color=fuchsia>**it will also lose any user input inside the list items**</font>. Instead, use a stable ID based on the data.
>
> Note that <font color=red>**your components won’t receive `key` as a prop**</font>. It’s only used as a hint by React itself. If your component needs an ID, you have to pass it as a separate prop: `<Profile key={id} userId={id} />`.



#### Keeping Components Pure

Some JavaScript functions are *pure.* <font color=red>Pure functions only perform a calculation and nothing more</font>. <font color=dodgerBlue>By strictly only writing your components as pure functions</font>, you can <font color=red>**avoid an entire class of baffling bugs and unpredictable behavior as your codebase grows**</font>. To get these benefits, though, there are a few rules you must follow.

##### Purity: Components as formulas

In computer science (and especially the world of functional programming), <font color=dodgerBlue>[a pure function](https://wikipedia.org/wiki/Pure_function) is a function with the following characteristics</font>:

- **It minds its own business.** It <font color=red>**does not change any objects or variables that existed before it was called**</font>.

- **Same inputs, same output.** Given the same inputs, a pure function should always return the same result.

  > 👀 幂等

You might already be familiar with one example of pure functions: formulas in math.

<font color=fuchsia size=4>**React assumes that every component you write is a pure function.**</font> This means that React components you write must always return the same JSX given the same inputs.

##### Side Effects: (un)intended consequences

<font color=fuchsia size=4>**React’s rendering process must always be pure**</font>. <font color=fuchsia>Components should only *return* their JSX</font>, and not *change* any objects or variables that existed before rendering—that would make them impure!

<font color=dodgerBlue>Here is a component that **breaks this rule**:</font>

```jsx
let guest = 0;

function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1; // 👀 这里也产生闭包了
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

This component is reading and writing a `guest` variable declared outside of it. This means that **calling this component multiple times will produce different JSX!** And what’s more, if *other* components read `guest`, they will produce different JSX, too, depending on when they were rendered! <font color=red>That’s not predictable</font>.

You can fix this component by [passing `guest` as a prop instead](https://react.dev/learn/passing-props-to-a-component):

```jsx
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

<font color=dodgerBlue>In general</font>, you <font color=red>**should not** expect your components to be rendered in any particular order</font>. It doesn’t matter if you call $y = 2x$ before or after $y = 5x$: both formulas will resolve independently of each other. In the same way, each component should only “think for itself”, and not attempt to coordinate with or depend upon others during rendering. <font color=red>Rendering is like a school exam: each component should calculate JSX on their own</font>!

###### Detecting impure calculations with StrictMod

Although you might not have used them all yet, <font color=red>in React there are three kinds of inputs that you can read while rendering: [props](https://react.dev/learn/passing-props-to-a-component), [state](https://react.dev/learn/state-a-components-memory), and [context](https://react.dev/learn/passing-data-deeply-with-context)</font>. <font color=fuchsia>**You should always treat these inputs as read-only**</font>.

<font color=dodgerBlue>When you want to *change* something in response to user input</font>, you <font color=red>**should [set state](https://react.dev/learn/state-a-components-memory) instead of writing to a variable**</font>. You should never change preexisting variables or objects while your component is rendering.

<font color=fuchsia>React offers a **“Strict Mode”** in which **it calls each component’s function twice during development**</font>. **<font color=dodgerBlue>By calling the component functions twice</font>, Strict Mode helps find components that break these rules.**

> 💡 这句话有点不理解，询问了朋友，得到的答复是：
>
> 严格模式会额外帮你把组件激活一下 再马上销毁 用来检测你组件写的有没有问题

Notice how the original example displayed “Guest #2”, “Guest #4”, and “Guest #6” instead of “Guest #1”, “Guest #2”, and “Guest #3”. The original function was impure, so <font color=red>**calling it twice broke it**</font>. But the fixed pure version works even if the function is called twice every time. **Pure functions only calculate, so calling them twice won’t change anything**—just like calling `double(2)` twice doesn’t change what’s returned, and solving $y = 2x$ twice doesn’t change what y is. Same inputs, same outputs. Always.

<font color=fuchsia>**Strict Mode has no effect in production**</font>, so it won’t slow down the app for your users. <font color=dodgerBlue>To opt into Strict Mode</font>, you <font color=fuchsia>can wrap your root component into `<React.StrictMode>`</font>. Some frameworks do this by default.

###### Local mutation: Your component’s little secret 

In the above example, the problem was that the component changed a *preexisting* variable while rendering. This is often called a **“mutation”** to make it sound a bit scarier. Pure functions don’t mutate variables outside of the function’s scope or objects that were created before the call—that makes them impure!

However, <font color=red>**it’s completely fine to change variables and objects that you’ve *just* created while rendering.**</font> In this example, you create an `[]` array, assign it to a `cups` variable, and then `push` a dozen cups into it:

```jsx
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

<font color=red>If the `cups` variable or the `[]` array were created outside the `TeaGathering` function</font>, this would be a huge problem! You would be changing a *preexisting* object by pushing items into that array.

However, it’s fine because you’ve created them *during the same render*, inside `TeaGathering`. No code outside of `TeaGathering` will ever know that this happened. <font color=fuchsia>This is called **“local mutation”**</font>—it’s like your component’s little secret.

##### Where you *can* cause side effects

While <font color=fuchsia>**functional programming** relies heavily on purity</font>, at some point, somewhere, *something* has to change. <font color=red>That’s kind of the point of programming! These changes—updating the screen, starting an animation, changing the data—are called **side effects.**</font> They’re <font color=fuchsia>things that happen *“on the side”*, not during rendering</font>.

<font color=dodgerBlue>In React</font>, <font color=fuchsia>**side effects usually belong inside [event handlers](https://react.dev/learn/responding-to-events)**</font>. Event handlers are functions that React runs when you perform some action—for example, when you click a button. <font color=lightSeaGreen>Even though event handlers are defined *inside* your component</font>, <font color=red>**they don’t run *during* rendering**</font>! **So event handlers don’t need to be pure.**

<font color=dodgerBlue>If you’ve exhausted all other options and **can’t find the right event handler for your side effect**</font>, you <font color=fuchsia>can still **attach it to your returned JSX with a [`useEffect`](https://react.dev/reference/react/useEffect) call in your component**</font>. <font color=fuchsia>This tells React to **execute it later**, **after rendering**, when side effects are allowed</font>. **However, <font color=lightSeaGreen>this approach should be your last resort</font>.**

When possible, try to express your logic with rendering alone. You’ll be surprised how far this can take you!

###### Why does React care about purity?

Writing pure functions takes some habit and discipline. But <font color=dodgerBlue>**it also unlocks marvelous opportunities**</font>:

- <font color=red>**Your components could run in a different environment**—for example, on the server</font>! Since they return the same result for the same inputs, one component can serve many user requests.
- <font color=red>You can **improve performance by [skipping rendering](https://react.dev/reference/react/memo) components whose inputs have not changed**</font>. This is safe because pure functions always return the same results, so <font color=red>they are safe to cache</font>.
- <font color=dodgerBlue>If some data changes in the middle of rendering a deep component tree</font>, <font color=red>React can **restart rendering without wasting time to finish the outdated render**</font>. Purity makes it <font color=red>safe to stop calculating at any time</font>.

<font color=fuchsia>Every new React feature we’re building takes advantage of purity</font>. From data fetching to animations to performance, keeping components pure unlocks the power of the React paradigm.

##### Recap

- A component must be pure, meaning:
  - **It minds its own business.** It should not change any objects or variables that existed before rendering.
  - **Same inputs, same output.** Given the same inputs, a component should always return the same JSX.
- <font color=fuchsia>**Rendering can happen at any time**</font>, so <font color=fuchsia>components should not depend on each others’ rendering sequence</font>.
- You should not mutate any of the inputs that your components use for rendering. That includes props, state, and context. To update the screen, [“set” state](https://react.dev/learn/state-a-components-memory) instead of mutating preexisting objects.
- Strive to express your component’s logic in the JSX you return. When you need to “change things”, you’ll usually want to do it in an event handler. As a last resort, you can `useEffect`.
- Writing pure functions takes a bit of practice, but it unlocks the power of React’s paradigm.



## coderwhy React18 学习笔记



#### React 介绍

##### React 的使用场景

- React DOM ，包含 SSR ( ReactDOMServer  )
- React Native
- React VR

##### 声明式编程

> 👀 这里还提了一下 “命令式” 和 “声明式”，不过只是一笔带过；相关内容可以看下《Vue.js 设计与实现》§1.1 命令式和声明式

声明式编程是目前整个大前端开发的模式：Vue、React、Flutter、SwiftUI 。它允许我们只需要维护自己的状态，当状态改变时，React 可以根据最新的状态去渲染我们的 UI 界面。

<img src="https://s2.loli.net/2022/10/02/ZJfk4NSLXHiz3rU.png" alt="A mathematical formula of UI = f(state). 'UI' is the layout on the screen. 'f' is your build methods. 'state' is the application state." style="zoom:85%;" />

> 👀 上图摘自 https://docs.flutter.dev/development/data-and-backend/state-mgmt/declarative

##### render 函数

不论在 Vue 还是在 React 中都包含一个 render 函数。



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

相关可以参考文档 [React Doc - CDN Links](https://reactjs.org/docs/cdn-links.html) 。另外，也可以参考 [react 文档给出的 GitHub Gist](https://gist.githubusercontent.com/gaearon/0275b1e1518599bbeafcde4722e79ed1/raw/db72dcbf3384ee1708c4a07d3be79860db04bff0/example.html)

###### 引入可选的 babel

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

相关可以参考文档 [React Doc - Add React to a Website # Quickly Try JSX](https://reactjs.org/docs/add-react-to-a-website.html#quickly-try-jsx)

###### 引入 babel 所连带的问题

> ⚠️ 值得注意的是：经过 babel 处理的代码，默认会使用严格模式；主要的影响：this 的 undefined 时的指向；还有尾调用优化等。

##### ReactDOM.createRoot

###### 示例

```react
const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(<h1>root</h1>) // ⚠️ 这里的 jsx 不需要加引号，如果加上引号，将会被理解为字符串

const app = ReactDOM.createRoot(document.querySelector('#app'))
root.render(<h1>app</h1>)
```

> 💡 这部分的代码可以参考下：[React doc - Add React to an Existing Project](https://react.dev/learn/add-react-to-an-existing-project) ，其中包含两种根的用法。除了包含 `index.js` 中的 `document.body.innerHTML = '<div id="app"></div>';` 作为根（没有 `index.html` ），也包含 `index.html` 中的 `<nav id="navigation"></nav>` 作为根，`createRoot` 在 `index.js` 中

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

> ⚠️ 注意：render 方法中的 JSX 不允许有多个根。
>
> 💡 另外，这里用于包裹的 `<div>` ，没有语意化，也是无意义的作为一个 wrapper。建议使用 `<React.Fragment></React.Fragment> ` 或缩写 `<></>` ；这有点类似于 Vue 的 template

###### 组件化

另外，由于 React 采用了组件化的编程思想，所以 render 函数中除了传入一个 html 元素，也是可以传入一个组件的；如下示例：

```react
root.render(<App/>)
```

这也让代码更易于封装、也更加简洁。具体见 [[#组件化封装]]

###### `<script type="text/babel">`

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
        <button onClick={ this.onBtnClick.bind(this) }>修改文本</button> {/* ⚠️ 使用了 bind */}
      </div>
    )
  }
}

const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(<App/>)
```

###### 示例代码解释

setState 方法是来自继承的 `React.Component` 类中，setState 在内部做了两件事：1) 修改 state 中的值 2) 自动重新执行 render 函数

[[#类组件#示例]] 中使用 bind 的部分原因：因为 JSX 被编译转变为 `React.createElement` 的处理过程，导致 this 的丢失，示例如下：

```react
React.createElement('button', { onClick: this.btnClick } )
```

它的内部可以这样处理（示例代码）： `const click = config.onClick` ，这就导致了 this 的丢失；同时，因为 babel 默认使用了严格模式，丢失的 this 将会是 undefined ；所以需要使用 bind 方法进行绑定 this。

<font color=dodgerBlue>更详细的原因是</font>：在正常的 DOM 操作中监听点击，监听函数中的 this 其实是节点对象（比如说是 button 对象）；<font color=fuchsia>因为 React 并不是直接渲染成真实的 DOM，我们所编写的 button 只是一个语法糖，它的本质 React 的 Element 对象</font>； 那么在这里发生监听的时候，React 给我们的函数绑定的 this，默认情况下就是一个 undefined。

在 [[#JSX 中 this 丢失的解决方法]] 中还有更多的解决 JSX this 丢失的方法

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

列表渲染使用 map 方法可以说是“最佳实践”了。上面的 `this.state.list.map( item => <li>{item}</li> )` 返回的是一个列表，也就是说：在 React 中的 `{}` 是可以放入一个 JSX 列表，并且无需做其他操作，直接让 React 渲染的。同时，该特性在对象上无法直接实现（除非开发者对 对象做处理... 将其转变成列表）。相关内容可以参考 [[#JSX 嵌入变量]]

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

<font color=red>React 认为渲染逻辑本质上与其他 UI 逻辑存在内在耦合</font>：比如 UI 需要绑定事件（ button、a 等）；比如 UI 中需要展示数据状态，在某些状态发生改变时，又需要改变UI。因为 它们之间是密不可分的，所以 React 没有将标记 ( html tag ) 分离到不同的文件中，而是将它们组合到了一起，即组件 Component 中

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

##### JSX 中 this 丢失的解决方法

###### 使用 bind

上面有示例，这里略。

###### 使用 ES6 class fields

使用 “箭头函数” 中，从外层获取 this 的特性。

```react
class App extends React.component {
  constructor() { super() }
  onBtnClick = () => { /* ... */ }
  render() {
    return (
      <button onClick={ onBtnClick }>click me</button>
    )
  }
}
```

###### 事件监听时传入箭头函数

> ⭐️ 推荐，这也是最佳实践

`{}` 本身就是传入一个函数，所以也可以传入一个箭头函数。原理上和 [[#使用 ES6 class fields]] 类似

```jsx
<button onClick={ () => onBtnClick() }>click me</button>
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



#### React 动态异步组件

##### 总述

Lazy loading of React components can:

- reduce the initial load time,
- download components only as needed

<font color=fuchsia>React’s Suspense APIs</font> let you handle the asynchronous loading of components in the UI. It lets components “wait” for something before rendering.

##### Use of React.lazy()

<font color=fuchsia>`React.lazy()`</font> lets you define a component that is loaded dynamically.

To dynamically load a component `LazyComponent.js` we would need to dynamically import and load it as below.

```react
import { lazy } from 'react';
...
const LazyLoadComponent = lazy(() => import('./LazyComponent');
```

<font color=red>`React.lazy` is only supported for default imports</font>. We have to modify the promise returned by the dynamic import to have a default for named imports.

```react
const LazyLoadComponent = lazy(() => import('./LazyComponent')
  .then(
     module => ({ default: module.Content })
  )
);
```

<font color=dodgerBlue>Dynamically importing and loading</font> <font color=fuchsia>makes the component not part of the main bundle or chunk of code</font>. Thus, <font color=LightSeaGreen>reducing the initial page loading</font>. <font color=red>This way we can split our components based on if it’s a large component</font>, if it’s part of the initial render, its visibility rate to users, if it’s conditionally rendered, or if it’s not so critical.

##### Use of React.Suspense

<font color=dodgerBlue>Lazy load components might cost some waiting if it’s heavy to import and load, poor network connection, processing time in old devices, etc</font>. In this case, <font color=red>we need to provide a fallback UI to indicate to the users that the component is loading</font>. This is where `React.Suspense` comes in.

`React.Suspense` lets you specify the loading indicator in case some components in the tree below it are not yet ready to render.

```react
import { lazy, Suspense } from 'react;
const Content = lazy(() => import("./Content"));

export const AppComponent = () => (
   <Suspense fallback={<div>Loading...</div>}> {/* 👀 使用了 Suspense */}
      <Content cost={COST} />
   </Suspense>
);
```

##### Use of Intersection Observer API

<font color=dodgerBlue>`React.Suspense` APIs don’t support lazy data fetching</font>. Using [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) we <font color=red>can under the hood fetch data **only if the component comes in the visibility**</font>. This way we can reduce the load time again on top of lazy loading, we are also lazy fetching the component. We can also use the same principle to supply data for the component.

To use Intersection Observer API we can create an observer as below:

```react
const options = {
  root: null,
  rootMargin: '750px',
  threshold: 1.0,
};

const observer = new IntersectionObserver(callback, options);
observer.observe(targetElement);
```

We can also use [`react-intersection-observer`](https://www.npmjs.com/package/react-intersection-observer) npm package that uses the API to provide hooks, props, etc. for your React application.

摘自：[How to Handle Dynamic & Async Components in React](https://javascript.plainenglish.io/how-to-handle-dynamic-async-components-in-react-99ca13578fd8)



#### React Best Practices – Tips for Writing Better React Code in 2022 笔记

// TODO

[React Best Practices – Tips for Writing Better React Code in 2022](https://freecodecamp.org/news/best-practices-for-react/)