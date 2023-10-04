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

##### Example Hooks

The type definitions from `@types/react` include types for the built-in hooks, so <font color=red>you can use them in your components without any additional setup</font>. They are built to take into account the code you write in your component, so you will get [inferred types](https://www.typescriptlang.org/docs/handbook/type-inference.html) a lot of the time and ideally do not need to handle the minutiae of providing the types.

###### `useState`

The [`useState` hook](https://react.dev/reference/react/useState) will <font color=lightSeaGreen>re-use the value passed in</font> <font color=red>as the initial state to **determine what the type of the value should be**</font>. For example:

```tsx
// Infer the type as "boolean"
const [enabled, setEnabled] = useState(false);
```

Will assign the type of `boolean` to `enabled`, and <font color=red>`setEnabled` will be a function accepting either a `boolean` argument, **or a function that returns a `boolean`**</font>. <font color=dodgerBlue>If you want to explicitly provide a type for the state</font>, you can do so by providing a type argument to the `useState` call:

```tsx
// Explicitly set the type to "boolean"
const [enabled, setEnabled] = useState<boolean>(false);
```

<font color=lightSeaGreen>This isn’t very useful **in this case**</font>, but <font color=red>a common case where you may want to provide a type is when you **have a union type**</font>. For example, `status` here can be one of a few different strings:

```tsx
type Status = "idle" | "loading" | "success" | "error";

const [status, setStatus] = useState<Status>("idle");
```

Or, as recommended in [Principles for structuring state](https://react.dev/learn/choosing-the-state-structure#principles-for-structuring-state), you can group related state as an object and describe the different possibilities via object types:

```tsx
type RequestState =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success', data: any }
  | { status: 'error', error: Error };

const [requestState, setRequestState] = useState<RequestState>({ status: 'idle' });
```

###### `useReducer`

The [`useReducer` hook](https://react.dev/reference/react/useReducer) is a more complex hook that <font color=red>takes a reducer function and an initial state</font>. The types for the reducer function are inferred from the initial state. <font color=lightSeaGreen>You can optionally provide a type argument to the `useReducer` call to provide a type for the state</font>, but it is often better to set the type on the initial state instead:

```tsx
import {useReducer} from 'react';

interface State {
   count: number 
};

type CounterAction =
  | { type: "reset" }
  | { type: "setCount"; value: State["count"] }

const initialState: State = { count: 0 };

function stateReducer(state: State, action: CounterAction): State {
  switch (action.type) {
    case "reset":
      return initialState;
    case "setCount":
      return { ...state, count: action.value };
    default:
      throw new Error("Unknown action");
  }
}

export default function App() {
  const [state, dispatch] = useReducer(stateReducer, initialState);

  const addFive = () => dispatch({ type: "setCount", value: state.count + 5 });
  const reset = () => dispatch({ type: "reset" });

  return (
    <div>
      <h1>Welcome to my counter</h1>

      <p>Count: {state.count}</p>
      <button onClick={addFive}>Add 5</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

We are using TypeScript in a few key places:

- `interface State` describes the shape of the reducer’s state.
- `type CounterAction` describes the different actions which can be dispatched to the reducer.
- `const initialState: State` provides a type for the initial state, and also the type which is used by `useReducer` by default.
- `stateReducer(state: State, action: CounterAction): State` sets the types for the reducer function’s arguments and return value.

A more explicit alternative to setting the type on `initialState` is to provide a type argument to `useReducer`:

```tsx
import { stateReducer, State } from './your-reducer-implementation';

const initialState = { count: 0 };

export default function App() {
  const [state, dispatch] = useReducer<State>(stateReducer, initialState);
}
```

###### `useContext`

The [`useContext` hook](https://react.dev/reference/react/useContext) is a technique for <font color=red>passing data down the component tree **without having to pass props through components**</font>. It is used by <font color=red>creating a **provider component**</font> （👀 见下面的 `<ThemeContext.Provider>` ）and <font color=dodgerBlue>**often**</font> <font color=lightSeaGreen>by creating a hook to consume the value in a child component</font>.

The type of the value provided by the context is inferred from the value passed to the `createContext` call:

```tsx
import { createContext, useContext, useState } from 'react';

type Theme = "light" | "dark" | "system";
const ThemeContext = createContext<Theme>("system");

const useGetTheme = () => useContext(ThemeContext);

export default function MyApp() {
  const [theme, setTheme] = useState<Theme>('light');

  return (
    <ThemeContext.Provider value={theme}>
      <MyComponent />
    </ThemeContext.Provider>
  )
}

function MyComponent() {
  const theme = useGetTheme();

  return (
    <div>
      <p>Current theme: {theme}</p>
    </div>
  )
}
```

This technique works when you have an default value which makes sense - <font color=dodgerBlue>but there are occasionally cases when you do not</font>, and <font color=red>in those cases `null` can feel reasonable as a default value</font>. However, <font color=dodgerBlue>**to allow the type-system to understand your code**</font>, you <font color=red>need to explicitly **set `ContextShape | null` on the `createContext`**</font>.

<font color=dodgerBlue>This causes the issue that you **need to eliminate the `| null` in the type for context consumers**</font>. Our recommendation is to have the hook do a runtime check for it’s existence and throw an error when not present:

```jsx
import { createContext, useContext, useState, useMemo } from 'react';

// This is a simpler example, but you can imagine a more complex object here
type ComplexObject = {
  kind: string
};

// The context is created with `| null` in the type, to accurately reflect the default value.
const Context = createContext<ComplexObject | null>(null);

// The `| null` will be removed via the check in the hook.
const useGetComplexObject = () => {
  const object = useContext(Context);
  if (!object) { throw new Error("useGetComplexObject must be used within a Provider") }
  return object;
}

export default function MyApp() {
  const object = useMemo(() => ({ kind: "complex" }), []);

  return (
    <Context.Provider value={object}>
      <MyComponent />
    </Context.Provider>
  )
}

function MyComponent() {
  const object = useGetComplexObject();

  return (
    <div>
      <p>Current object: {object.kind}</p>
    </div>
  )
}
```

###### `useMemo`

The [`useMemo`](https://react.dev/reference/react/useMemo) hooks will <font color=red>create/re-access a memorized value from a function call</font>, <font color=dodgerBlue>re-running the function only when</font> <font color=red>dependencies passed as the 2nd parameter are changed</font>. The result of calling the hook is inferred from the return value from the function in the first parameter. You can be more explicit by providing a type argument to the hook.

```jsx
// The type of visibleTodos is inferred from the return value of filterTodos
const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
```

> 💡 除了减少计算外，也可以减少 render 的次数

###### `useCallback`

The [`useCallback`](https://react.dev/reference/react/useCallback) <font color=red>provide a stable reference to a function **as long as the dependencies passed into the second parameter are the same**</font>. <font color=dodgerBlue>Like `useMemo`</font>, the function’s type is inferred from the return value of the function in the first parameter, and you can be more explicit by providing a type argument to the hook.

```jsx
const handleClick = useCallback(() => {
  // ...
}, [todos]);
```

<font color=dodgerBlue>When working in TypeScript **strict mode**</font> `useCallback` <font color=red>requires adding types for the parameters in your callback</font>. This is because the type of the callback is inferred from the return value of the function, and without parameters the type cannot be fully understood.

Depending on your code-style preferences, you could use the `*EventHandler` functions from the React types to provide the type for the event handler at the same time as defining the callback:

```jsx
import { useState, useCallback } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  const handleChange = useCallback<React.ChangeEventHandler<HTMLInputElement>>((event) => {
    setValue(event.currentTarget.value);
  }, [setValue])
  
  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>Value: {value}</p>
    </>
  );
}
```

##### Useful Types

There is quite an expansive set of types which come from the `@types/react` package, it is worth a read when you feel comfortable with how React and TypeScript interact. You can find them [in React’s folder in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react/index.d.ts). We will cover a few of the more common types here.

###### DOM Events 

When working with DOM events in React, the type of the event can often be inferred from the event handler. However, <font color=dodgerBlue>when you want to extract a function to be passed to an event handler</font>, you will need to explicitly set the type of the event.

```jsx
import { useState } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  function handleChange(event: React.ChangeEvent<HTMLInputElement>) { {/*👀*/}
    setValue(event.currentTarget.value);
  }

  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>Value: {value}</p>
    </>
  );
}
```

There are many types of events provided in the React types - the full list can be found [here](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/b580df54c0819ec9df62b0835a315dd48b8594a9/types/react/index.d.ts#L1247C1-L1373) which is based on the [most popular events from the DOM](https://developer.mozilla.org/en-US/docs/Web/Events).

<font color=dodgerBlue>If you need to use an event that is **not included in this list**</font>, you <font color=red>can use the `React.SyntheticEvent` type</font>, <font color=red>**which is the base type for all events**</font>.

###### Children

<font color=lightSeaGreen>There are two common paths to describing the children of a component</font>. <font color=dodgerBlue>The first</font> is to <font color=red>**use the `React.ReactNode` type**</font>, which <font color=red>is **a union of all the possible types** that can be passed as children in JSX</font>:

```tsx
interface ModalRendererProps {
  title: string;
  children: React.ReactNode;
}
```

This is a very broad definition of children. <font color=dodgerBlue>The second</font> is to <font color=red>use the `React.ReactElement` type</font>, which is <font color=red>only JSX elements and not JavaScript primitives like strings or numbers</font>:

```tsx
interface ModalRendererProps {
  title: string;
  children: React.ReactElement;
}
```

Note, that <font color=red>you cannot use TypeScript to describe that the children are a certain type of JSX elements</font>, so you cannot use the type-system to describe a component which only accepts `<li>` children.

You can see all an example of both `React.ReactNode` and `React.ReactElement` with the type-checker in [this TypeScript playground](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgIilQ3wChSB6CxYmAOmXRgDkIATJOdNJMGAZzgwAFpxAR+8YADswAVwGkZMJFEzpOjDKw4AFHGEEBvUnDhphwADZsi0gFw0mDWjqQBuUgF9yaCNMlENzgAXjgACjADfkctFnYkfQhDAEpQgD44AB42YAA3dKMo5P46C2tbJGkvLIpcgt9-QLi3AEEwMFCItJDMrPTTbIQ3dKywdIB5aU4kKyQQKpha8drhhIGzLLWODbNs3b3s8YAxKBQAcwXpAThMaGWDvbH0gFloGbmrgQfBzYpd1YjQZbEYARkB6zMwO2SHSAAlZlYIBCdtCRkZpHIrFYahQYQD8UYYFA5EhcfjyGYqHAXnJAsIUHlOOUbHYhMIIHJzsI0Qk4P9SLUBuRqXEXEwAKKfRZcNA8PiCfxWACecAAUgBlAAacFm80W-CU11U6h4TgwUv11yShjgJjMLMqDnN9Dilq+nh8pD8AXgCHdMrCkWisVoAet0R6fXqhWKhjKllZVVxMcavpd4Zg7U6Qaj+2hmdG4zeRF10uu-Aeq0LBfLMEe-V+T2L7zLVu+FBWLdLeq+lc7DYFf39deFVOotMCACNOCh1dq219a+30uC8YWoZsRyuEdjkevR8uvoVMdjyTWt4WiSSydXD4NqZP4AymeZE072ZzuUeZQKheQgA).

###### Style Props

<font color=dodgerBlue>When using inline styles in React</font>, you <font color=red>can use `React.CSSProperties`</font> to describe the object passed to the `style` prop. <font color=red>**This type is a union of all the possible CSS properties**</font>, and <font color=red>is a good way to ensure you are passing valid CSS properties to the `style` prop, and to get auto-complete in your editor</font>.

```tsx
interface MyComponentProps {
  style: React.CSSProperties;
}
```



### Describing the UI



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



### Adding Interactivity

In React, data that changes over time is called *state.* 



#### Responding to Events

React <font color=dodgerBlue>lets you add *event handlers* to your JSX</font>. <font color=red>Event handlers are your own functions that will be triggered in response to interactions like clicking, hovering, focusing form inputs, and so on</font>.

##### Adding event handlers

You defined the `handleClick` function and then [passed it as a prop](https://react.dev/learn/passing-props-to-a-component) to `<button>`.  `handleClick` is an **event handler.** <font color=dodgerBlue>Event handler functions</font>:

- Are <font color=red>usually defined *inside* your components</font>.
- Have <font color=red>names that start with `handle`</font>, followed by the name of the event.

<font color=dodgerBlue>**By convention**</font>, it is common to name event handlers as `handle` followed by the event name. You’ll often see `onClick={handleClick}`, `onMouseEnter={handleMouseEnter}`, and so on.

Alternatively, you can define an event handler inline in the JSX:

```jsx
<button onClick={function handleClick() {
  alert('You clicked me!');
}}>
```

Or, more concisely, using an arrow function:

```jsx
<button onClick={() => {
  alert('You clicked me!');
}}>
```

All of these styles are equivalent. Inline event handlers are convenient for short functions.

> ⚠️ Pitfall
>
> Functions passed to event handlers must be passed, not called. For example:
>
> | passing a function (correct)     | calling a function (incorrect)     |
> | -------------------------------- | ---------------------------------- |
> | `<button onClick={handleClick}>` | `<button onClick={handleClick()}>` |
>
> <font color=dodgerBlue>The difference is subtle</font>. In the first example, the `handleClick` function is passed as an `onClick` event handler. This tells React to remember it and only call your function when the user clicks the button.
>
> <font color=dodgerBlue>In the second example</font>, <font color=lightSeaGreen>the `()` at the end of `handleClick()` fires the function *immediately* during [rendering](https://react.dev/learn/render-and-commit), without any clicks</font>. This is because <font color=red>JavaScript inside the [JSX `{` and `}`](https://react.dev/learn/javascript-in-jsx-with-curly-braces) executes right away</font>.
>
> When you write code inline, the same pitfall presents itself in a different way:
>
> | passing a function (correct)            | calling a function (incorrect)    |
> | --------------------------------------- | --------------------------------- |
> | `<button onClick={() => alert('...')}>` | `<button onClick={alert('...')}>` |
>
> Passing inline code like this won’t fire on click—<font color=red>it fires every time the component renders</font>:
>
> ```jsx
> // This alert fires when the component renders, not when clicked!
> <button onClick={alert('You clicked me!')}>
> ```
>
> If you want to define your event handler inline, wrap it in an anonymous function like so:
>
> ```jsx
> <button onClick={() => alert('You clicked me!')}>
> ```
>
> Rather than executing the code inside with every render, this creates a function to be called later.
>
> In both cases, what you want to pass is a function:
>
> - `<button onClick={handleClick}>` passes the `handleClick` function.
> - `<button onClick={() => alert('...')}>` passes the `() => alert('...')` function.

##### Passing event handlers as props

<font color=dodgerBlue>**Often you’ll want the parent component to specify a child’s event handler**</font>. Consider buttons: depending on where you’re using a `Button` component, you might want to execute a different function — <font color=lightSeaGreen>perhaps one plays a movie and another uploads an image</font>.

> 👀 感觉没什么东西，代码略

##### Naming event handler props

<font color=dodgerBlue>Built-in components like `<button>` and `<div>` only support [browser event names](https://react.dev/reference/react-dom/components/common#common-props) like `onClick`</font>. However, <font color=lightSeaGreen>**when you’re building your own components**</font>, <font color=red>**you can name their event handler props any way that you like**</font>.

<font color=dodgerBlue>**By convention**</font>, event handler props should start with `on`, followed by a capital letter.

For example, the `Button` component’s `onClick` prop could have been called `onSmash`:

```jsx
function Button({ onSmash, children }) {
  return (
    <button onClick={onSmash}> {/* 👀 */}
      {children}
    </button>
  );
}

export default function App() {
  return (
    <div>
      <Button onSmash={() => alert('Playing!')}>
        Play Movie
      </Button>
      <Button onSmash={() => alert('Uploading!')}>
        Upload Image
      </Button>
    </div>
  );
}
```

> 👀 React 中组件的自定义事件，感觉相较 Vue 要自然很多...

In this example, `<button onClick={onSmash}>` shows that the browser `<button>` (lowercase) still needs a prop called `onClick`, but <font color=lightSeaGreen>the prop name received by your custom `Button` component is up to you</font>!

##### Event propagation

> ⚠️ Pitfall
>
> All events propagate in React <font color=red>except `onScroll`</font> , which only works on the JSX tag you attach it to.

###### Stopping propagation

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}
```

###### Capture phase events

In rare cases, you might need to catch all events on child elements, *even if they stopped propagation*. For example, maybe you want to log every click to analytics, regardless of the propagation logic. You can do this by adding `Capture` at the end of the event name:

```jsx
<div onClickCapture={() => { /* this runs first */ }}>
  <button onClick={e => e.stopPropagation()} />
  <button onClick={e => e.stopPropagation()} />
</div>
```

Each event propagates in three phases:

1. It travels down, calling all `onClickCapture` handlers.
2. It runs the clicked element’s `onClick` handler.
3. It travels upwards, calling all `onClick` handlers.

Capture events are useful for code like routers or analytics, but you probably won’t use them in app code.



#### State: A Component's Memory

<font color=dodgerBlue>Components need to “remember” things</font>: the **current** input value, the **current** image, **the shopping cart**. In React, <font color=red>this kind of component-specific memory is called *state*</font>.

##### When a regular variable isn’t enough

> 👀 原文这里是有一个错误示例的，用来引出 setState；不过有点长且略去影响不算大，所以略。修正后的结果见 [[#Adding a state variable]]

two things prevent that change from being visible:

1. <font color=fuchsia>**Local variables don’t persist between renders.**</font> When React renders this component a second time, it renders it from scratch—it doesn’t consider any changes to the local variables.

   > 🌏 **局部变量无法在多次渲染中持久保存**。当 React 再次渲染这个组件时，它会从头开始渲染——不会考虑之前对局部变量的任何更改。

2. <font color=fuchsia>**Changes to local variables won’t trigger renders.**</font> React doesn’t realize it needs to render the component again with the new data.

To update a component with new data, <font color=dodgerBlue>two things need to happen:</font>

1. **Retain** the data between renders.
2. <font color=fuchsia>**Trigger** React to render the component with new data (re-rendering)</font>.

<font color=dodgerBlue>The `useState` Hook provides those two things:</font>

1. A <font color=dodgerBlue>**state variable**</font> to <font color=red>retain the data between renders.</font>
2. A <font color=dodgerBlue>**state setter function**</font> to <font color=red>update the variable</font> and <font color=red>trigger React to render the component again</font>.

##### Adding a state variable

```jsx
import { useState } from 'react';

const [index, setIndex] = useState(0);
```

`index` is a state variable and `setIndex` is the setter function.

> 💡 The `[` and `]` syntax here is called [array destructuring](https://javascript.info/destructuring-assignment) and it lets you read values from an array. The array returned by `useState` always has exactly two items.

```jsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);

  function handleClick() {
    setIndex(index + 1);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i> 
        by {sculpture.artist}
      </h2>
      <h3>  
        ({index + 1} of {sculptureList.length})
      </h3>
      <img 
        src={sculpture.url} 
        alt={sculpture.alt}
      />
      <p>
        {sculpture.description}
      </p>
    </>
  );
}
```

###### Meet your first Hook

<font color=dodgerBlue>In React</font>, <font color=red>`useState`, as well as **any other function starting with ”`use`”**, is **called a Hook**</font>.

<font color=fuchsia>*Hooks* are special functions</font> that are <font color=fuchsia>only available while React is [rendering](https://react.dev/learn/render-and-commit#step-1-trigger-a-render)</font>. They let you “hook into” different React features.

State is just one of those features, but you will meet the other Hooks later.

> ⚠️ Pitfall
>
> **Hooks—functions starting with `use`—<font color=fuchsia>can only be called at the top level of your components or [your own Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)</font>.** You <font color=red>can’t call Hooks inside conditions, loops, or other nested functions</font>. Hooks are functions, but <font color=red>it’s helpful to think of them as unconditional declarations about your component’s needs</font>. You “use” React features at the top of your component similar to how you “import” modules at the top of your file.

###### Anatomy of `useState`

When you call [`useState`](https://react.dev/reference/react/useState), you are <font color=red>telling React that you want this component to remember something</font>:

```jsx
const [index, setIndex] = useState(0);
```

In this case, you want React to remember `index`.

> 💡 Note
>
> The convention is to name this pair like `const [something, setSomething]`. <font color=lightSeaGreen>You could name it anything you like</font>, but conventions make things easier to understand across projects.

<font color=dodgerBlue>**Every time your component renders**</font>, `useState` gives you an array containing two values:

1. The **state variable** (`index`) with the value you stored.
2. The **state setter function** (`setIndex`) which can <font color=red>update the state variable</font> and <font color=fuchsia>**trigger React to render the component again**</font>.

<font color=dodgerBlue>Here’s how that happens in action:</font>

1. **Your component renders the first time.** Because you passed `0` to `useState` as the initial value for `index`, it will return `[0, setIndex]`. React remembers `0` is the latest state value.
2. **You update the state.** When a user clicks the button, it calls `setIndex(index + 1)`. `index` is `0`, so it’s `setIndex(1)`. This tells React to remember `index` is `1` now and <font color=red>triggers another render</font>.
3. **Your component’s second render.** <font color=lightSeaGreen>React still sees `useState(0)`</font>, but because <font color=red>React *remembers* that you set `index` to `1`</font>, it <font color=fuchsia>returns `[1, setIndex]` instead</font>.
4. And so on!

##### Giving a component multiple state variables

<font color=dodgerBlue>If you find that you often change two state variables together</font>, it might <font color=red>be easier to combine them into one</font>. For example, if you have a form with many fields, <font color=red>it’s more convenient to have a single state variable that **holds an object** than state variable per field</font>. Read [Choosing the State Structure](https://react.dev/learn/choosing-the-state-structure) for more tips.

###### How does React know which state to return?

You might have noticed that the `useState` call does not receive any information about *which* state variable it refers to. <font color=red>There is no “identifier” that is passed to `useState`</font>, so how does it know which of the state variables to return? Does it rely on some magic like parsing your functions? The answer is no.

Instead, to enable their concise syntax, <font color=fuchsia>Hooks **rely on a stable call order on every render of the same component**</font>. This works well in practice because <font color=lightSeaGreen>if you **follow the rule above (“only call Hooks at the top level”)**</font>, <font color=fuchsia>**Hooks will always be called in the same order**</font>. Additionally, a [linter plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks) catches most mistakes.

<font color=fuchsia>Internally, React **holds an array of state pairs for every component**</font>. <font color=dodgerBlue>It also</font> <font color=fuchsia>maintains the current pair index</font>, which is set to `0` before rendering. Each time you call `useState`, React gives you the next state pair and increments the index. You can read more about this mechanism in [React Hooks: Not Magic, Just Arrays.](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e)

> 🌏 在 React 内部，为每个组件保存了一个数组，其中每一项都是一个 state 对。它维护当前 state 对的索引值，在渲染之前将其设置为 “0”。每次调用 useState 时，React 都会为你提供一个 state 对并增加索引值。

> 👀 这里有一段，不直接使用 react 实现页面可响应的代码，感觉很受启发；但是由于较长，只摘抄了部分：

```js
let componentHooks = [];
let currentHookIndex = 0;

// How useState works inside React (simplified).
function useState(initialState) {
  let pair = componentHooks[currentHookIndex];
  if (pair) {
    // This is not the first render, so the state pair already exists.
    // Return it and prepare for next Hook call.
    currentHookIndex++;
    return pair;
  }

  // This is the first time we're rendering, so create a state pair and store it.
  pair = [initialState, setState];

  function setState(nextState) {
    // When the user requests a state change, put the new value into the pair.
    pair[0] = nextState;
    updateDOM();
  }

  // Store the pair for future renders and prepare for the next Hook call.
  componentHooks[currentHookIndex] = pair;
  currentHookIndex++;
  return pair;
}

function Gallery() {
  // Each useState() call will get the next pair.
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);

  function handleNextClick() {
    setIndex(index + 1);
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  // This example doesn't use React, so return an output object instead of JSX.
  return {
    onNextClick: handleNextClick,
    onMoreClick: handleMoreClick,
    header: `${sculpture.name} by ${sculpture.artist}`,
    counter: `${index + 1} of ${sculptureList.length}`,
    more: `${showMore ? 'Hide' : 'Show'} details`,
    description: showMore ? sculpture.description : null,
    imageSrc: sculpture.url,
    imageAlt: sculpture.alt
  };
}
```

##### State is isolated and private

State is local to a component instance on the screen. In other words, **if you render the same component twice, each copy will have completely isolated state!** Changing one of them will not affect the other.

notice how the `Page` ( 👀 parent ) component doesn’t “know” anything about the `Gallery` ( 👀 child ) state or even whether it has any. <font color=red>Unlike props, **state is fully private to the component declaring it**</font>. The parent component can’t change it. This lets you add state to any component or remove it without impacting the rest of the components.

What if you wanted both galleries to keep their states in sync? The right way to do it in React is to *remove* state from child components and add it to their closest shared parent. The next few pages will focus on organizing state of a single component, but we will return to this topic in [Sharing State Between Components.](https://react.dev/learn/sharing-state-between-components)

##### Recap

- Use a state variable when a component needs to “remember” some information between renders.
- State variables are declared by calling the `useState` Hook.
- Hooks are special functions that start with `use`. They let you “hook into” React features like state.
- Hooks might remind you of imports: they need to be called unconditionally. Calling Hooks, including `useState`, is only valid at the top level of a component or another Hook.
- The `useState` Hook returns a pair of values: the current state and the function to update it.
- You can have more than one state variable. Internally, React matches them up by their order.
- State is private to the component. If you render it in two places, each copy gets its own state.



#### Render and Commit

<font color=dodgerBlue>Before your components are displayed on screen</font>, <font color=fuchsia>they must be rendered by React</font>.

Imagine that your components are cooks in the kitchen, assembling tasty dishes from ingredients. <font color=lightSeaGreen>In this scenario</font>, <font color=red>React is the waiter who puts in requests from customers and brings them their orders</font>. <font color=lightSeaGreen>This process of requesting and serving UI has three steps</font>:

1. <font color=fuchsia>**Triggering** a render</font> (delivering the guest’s order to the kitchen)
2. <font color=fuchsia>**Rendering** the component</font> (preparing the order in the kitchen)
3. <font color=red>**Committing** to the DOM</font> (placing the order on the table)

> 👀 上面的内容也说明了：React 在使用虚拟 DOM，等待虚拟 DOM 渲染/生成完成后，再将其绘制到页面上；浏览器绘制的步骤见 [[#Epilogue: Browser paint]]

<img src="https://s2.loli.net/2023/09/28/EzYotsbXrqmTnd2.png" alt="image-20230928112144925" style="zoom:45%;" />

##### Step 1: Trigger a render 

<font color=dodgerBlue>**There are two reasons for a component to render:**</font>

1. It’s the component’s **initial render.**
2. The component’s (or one of its ancestors’) **state has been updated.**

###### Initial render

<font color=dodgerBlue>**When your app starts**</font>, <font color=red>you need to trigger the initial render</font>. <font color=lightSeaGreen>Frameworks and sandboxes sometimes hide this code</font>, but <font color=fuchsia>it’s done by calling `createRoot` with the target DOM node</font>, and then calling its `render` method with your component:

```js
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

###### Re-renders when state updates

Once the component has been initially rendered, you can trigger further renders by updating its state with the [`set` function.](https://react.dev/reference/react/useState#setstate) <font color=red>Updating your component’s state **automatically queues a render**</font>. (You can imagine these as a restaurant guest ordering tea, dessert, and all sorts of things after putting in their first order, depending on the state of their thirst or hunger.)

<img src="https://s2.loli.net/2023/09/28/NMemWdgOfkRLPDI.png" alt="image-20230928153109074" style="zoom:45%;" />

##### Step 2: React renders your components

After you trigger a render, React calls your components to figure out what to display on screen. **“Rendering” is React calling your components.**

- **On initial render,** React will call the root component.
- **For subsequent renders,** <font color=red>React will call the function component **whose state update triggered the render**</font>.

<font color=red>This process is **recursive**</font>: if the updated component returns some other component, React will render *that* component next, and if that component also returns something, it will render *that* component next, and so on. <font color=red>The process will continue until there are no more nested components</font> and React knows exactly what should be displayed on screen.

In the following example, React will call `Gallery()` and  `Image()` several times:

> 👀 这里代码略

- **During the initial render,** React will [create the DOM nodes](https://developer.mozilla.org/docs/Web/API/Document/createElement) for `<section>`, `<h1>`, and three `<img>` tags.
- **During a re-render,** <font color=fuchsia>React will calculate which of their properties</font>, <font color=red>if any, have changed since the previous render</font>. It won’t do anything with that information until the next step, the commit phase.

> ⚠️ Pitfall
>
> <font color=fuchsia>**Rendering must always be a [pure calculation](https://react.dev/learn/keeping-components-pure)**</font>:
>
> - **Same inputs, same output.** Given the same inputs, a component should always return the same JSX. (When someone orders a salad with tomatoes, they should not receive a salad with onions!)
> - **It minds its own business.** It <font color=red>should not change any objects or variables that existed before rendering</font>. (One order should not change anyone else’s order.)
>
> <font color=dodgerBlue>Otherwise</font>, <font color=lightSeaGreen>you can encounter confusing bugs and unpredictable behavior</font> as your codebase grows in complexity. When developing in “Strict Mode”, React calls each component’s function twice, which can help surface mistakes caused by impure functions.

###### Optimizing performance

The default behavior of rendering all components nested within the updated component <font color=red>is **not optimal for performance**</font> <font color=dodgerBlue>if the updated component is very high in the tree</font>. If you run into a performance issue, there are several opt-in ways to solve it described in the [Performance](https://reactjs.org/docs/optimizing-performance.html) section. **Don’t optimize prematurely!**

> 👀 不要过早优化

##### Step 3: React commits changes to the DOM 

After rendering (calling) your components, React will modify the DOM.

- **For the initial render,** React will <font color=red>use the `appendChild()` DOM API</font> to put all the DOM nodes it has created on screen.
- **For re-renders,** React will <font color=red>apply the minimal necessary operations</font> (calculated while rendering!) to make the DOM match the latest rendering output.

**React only changes the DOM nodes if there’s a difference between renders.**

##### Epilogue: Browser paint

<font color=dodgerBlue>After rendering is done and React updated the DOM</font>, <font color=red>the browser will repaint the screen</font>. Although this process is known as “browser rendering”, we’ll refer to it as “painting” to avoid confusion throughout the docs.

<img src="https://s2.loli.net/2023/09/28/4pcxYRwoU69ZAvB.png" style="zoom: 18%;" />



#### State as a Snapshot

State variables might look like regular JavaScript variables that you can read and write to. However, <font color=fuchsia>state behaves more like a snapshot</font>. <font color=fuchsia>Setting it **does not change the state variable you already have**</font>, but <font color=red>instead **triggers** a re-render</font>.

> 👀 关于这里的 “setting it” 是指 `setState` ，而 “does not change the state” ，是因为要进行 diff，oldState 要和 newState 进行对比，所以自然不能覆盖掉；这里的标题也是解释的一部分 “ state 就像快照”，而快照是不可能覆盖或者修改的。另外，这里的 “trigger” 也是 trigger -> render -> commit 中的第一步。

##### Setting state triggers renders

In this example, when you press “send”, `setIsSent(true)` tells React to re-render the UI:

```jsx
import { useState } from 'react';

export default function Form() {
  const [isSent, setIsSent] = useState(false);
  const [message, setMessage] = useState('Hi!');
  if (isSent) {
    return <h1>Your message is on its way!</h1>
  }
  return (
    <form onSubmit={(e) => {
      e.preventDefault();
      setIsSent(true);
      sendMessage(message);
    }}>
      <textarea
        placeholder="Message"
        value={message}
        onChange={e => setMessage(e.target.value)}
      />
      <button type="submit">Send</button>
    </form>
  );
}

function sendMessage(message) {
  // ...
}
```

<font color=dodgerBlue>Here’s what happens when you click the button:</font>

1. The `onSubmit` event handler executes.
2. `setIsSent(true)` sets `isSent` to `true` and <font color=fuchsia>queues a new render</font>.
3. React re-renders the component according to the new `isSent` value.

##### Rendering takes a snapshot in time

[“Rendering”](https://react.dev/learn/render-and-commit#step-2-react-renders-your-components) means that React is calling your component, which is a function. <font color=dodgerBlue>The JSX you return from that function</font> is <font color=red>like a snapshot of the UI in time</font>. <font color=lightSeaGreen>Its props, event handlers, and local variables</font> were <font color=fuchsia>all calculated **using its state at the time of the render**</font>.

> 🌏 它的 props、事件处理函数和内部变量都是 **根据当前渲染时的 state** 被计算出来的

Unlike a photograph or a movie frame, <font color=red>the UI “snapshot” you return is **interactive**</font>. <font color=lightSeaGreen>**It**</font> includes logic like event handlers that specify what happens in response to inputs. <font color=lightSeaGreen>React updates the screen to match this snapshot and connects the event handlers</font>. As a result, pressing a button will trigger the click handler from your JSX.

When React re-renders a component:

1. React calls your function again.
2. <font color=fuchsia>Your function **returns a new JSX snapshot**.</font>
3. <font color=red>React then updates the screen to match the snapshot your function returned</font>.

<img src="https://s2.loli.net/2023/09/29/qYKgMjWubXN8OeP.png" alt="image-20230929100326250" style="zoom: 45%;" />

<font color=red>As a **component’s memory**</font>, state is not like a regular variable that disappears after your function returns. <font color=red>State actually “lives” in React itself</font>—as if on a shelf!—outside of your function. When React calls your component, <font color=red>it gives you a **snapshot of the state** for that particular render</font>. Your component returns a **snapshot of the UI** with a fresh set of props and event handlers in its JSX, all calculated **using the state values from that render!**

<img src="https://s2.loli.net/2023/09/29/3o94vdXKErSfYUP.png" alt="image-20230929101521524" style="zoom: 45%;" />

<font color=dodgerBlue>Here’s a little experiment to show you how this works</font>. In this example, you might expect that clicking the “+3” button would increment the counter three times because <font color=lightSeaGreen>it calls `setNumber(number + 1)` three times</font>.

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

Notice that <font color=dodgerBlue>**`number` only increments once per click**</font>!

<font color=fuchsia>**Setting state only changes it for the *next* render.**</font> During the first render, `number` was `0`. This is why, in *that render’s* `onClick` handler, <font color=fuchsia>the value of `number` is still `0` even after `setNumber(number + 1)` was called</font>:

```jsx
<button onClick={() => {
  setNumber(number + 1);
  setNumber(number + 1);
  setNumber(number + 1);
}}>+3</button>
```

<font color=dodgerBlue>Here is what this button’s click handler tells React to do:</font>

1. `setNumber(number + 1)` : `number` is `0` so `setNumber(0 + 1)`.

   <font color=fuchsia>React prepares to change `number` to `1` on the next render.</font>

2. `setNumber(number + 1)` : `number` is `0` so `setNumber(0 + 1)`.

   React prepares to change `number` to `1` on the next render.

3. `setNumber(number + 1)` : `number` is `0` so `setNumber(0 + 1)`.

   React prepares to change `number` to `1` on the next render.

<font color=dodgerBlue>Even though you called `setNumber(number + 1)` three times</font>, <font color=lightSeaGreen>**in *this render’s***</font> event handler `number` is always `0`, <font color=red>so you set the state to `1` three times</font>. This is why, after your event handler finishes, React re-renders the component with `number` equal to `1` rather than `3`.

You can also visualize this by mentally substituting state variables with their values in your code. Since the `number` state variable is `0` for *this render*, its event handler looks like this:

```jsx
<button onClick={() => {
  setNumber(0 + 1);
  setNumber(0 + 1);
  setNumber(0 + 1);
}}>+3</button>
```

For the next render, `number` is `1`, so *that render’s* click handler looks like this:

```jsx
<button onClick={() => {
  setNumber(1 + 1);
  setNumber(1 + 1);
  setNumber(1 + 1);
}}>+3</button>
```

> 💡 虽然不建议在一次渲染中，多次调用同一个 state 的 `setState` ；如果非要这样做，可以参考 [[#Updating the same state multiple times before the next render]] 中在 `setState` 传入函数的写法，比如 `setState(state => state + 1)`

##### State over time

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        alert(number);
      }}>+5</button>
    </>
  )
}
```

If you use the substitution method from before, you can guess that the alert shows “0”:

```jsx
setNumber(0 + 5);
alert(0);
```

what if you put a timer on the alert, so it only fires *after* the component re-rendered? Would it say “0” or “5”? 

```jsx
setNumber(number + 5);
setTimeout(() => { alert(number); }, 3000);
```

<font color=lightSeaGreen>The state stored in React **may have changed** by the time the alert runs</font>, but <font color=red>it was scheduled using a snapshot of the state</font> at the time the user interacted with it!

<font color=fuchsia>**A state variable’s value never changes within a render**</font> , even if its event handler’s code is asynchronous. Inside *that render’s* `onClick`, the value of `number` continues to be `0` even after `setNumber(number + 5)` was called. <font color=fuchsia>Its value was **“fixed”** when React “took the snapshot” of the UI by calling your component</font>.

##### Recap

- <font color=fuchsia>Setting state **requests a new render**</font>.

  > 👀 感觉 “申请开启一个新的渲染” 似乎更好些？

- React stores state outside of your component, as if on a shelf.

- <font color=dodgerBlue>**When you call `useState`**</font>, <font color=red>React gives you a snapshot of the state *for that render*</font>.

- <font color=fuchsia>Variables and event handlers don’t “survive” re-renders</font>. <font color=red>Every render has its own event handlers</font>.

  > 🌏 变量和事件处理函数不会在重渲染中“存活”。
  >
  > 👀 上面这句话也说明了：state 可以看作 component’s memory。相关概念可参见 [[#Props vs State]]

- Every render (and functions inside it) will always “see” the snapshot of the state that React gave to *that* render.

- You can mentally substitute state in event handlers, similarly to how you think about the rendered JSX.

- Event handlers created in the past have the state values from the render in which they were created.

##### Try out some challenges

Calling `setWalk` ( 👀 `setState` ) will only change it for the *next* render, but will not affect the event handler from the previous render.



#### Queueing a Series of State Updates

<font color=fuchsia>Setting a state variable will **queue another render**</font>. But <font color=dodgerBlue>sometimes you might want to perform multiple operations on the value</font> before queueing the next render. To do this, it helps to understand how React <font color=fuchsia>batches state updates</font>.

##### React batches state updates

there is one other factor at play here. <font color=fuchsia>**React waits until *all* code in the event handlers has run before processing your state updates**</font>. This is why the re-render only happens *after* all these `setNumber()` calls.

> 🌏 **React 会等到事件处理函数中的** 所有 **代码都运行完毕再处理你的 state 更新。**

This might remind you of a waiter taking an order at the restaurant. <font color=lightSeaGreen>A waiter doesn’t run to the kitchen at the mention of your first dish</font>! Instead, they let you finish your order, let you make changes to it, and even take orders from other people at the table.

> 🌏 服务员不会在你说第一道菜的时候就跑到厨房！相反，他们会让你把菜点完，让你修改菜品，甚至会帮桌上的其他人点菜。

<img src="https://s2.loli.net/2023/09/29/MG7uIzsUoDfbiHR.png" alt="An elegant cursor at a restaurant places and order multiple times with React, playing the part of the waiter. After she calls setState() multiple times, the waiter writes down the last one she requested as her final order." style="zoom: 18%;" />

This lets you update multiple state variables—even from multiple components — <font color=red>without triggering too many [re-renders](https://react.dev/learn/render-and-commit#re-renders-when-state-updates)</font>. But this also means that the UI won’t be updated until *after* your event handler, and any code in it, completes. <font color=red>This behavior, also known as **batching**, makes your React app run much faster</font>. It also avoids dealing with confusing “half-finished” renders where only some of the variables have been updated.

> 🌏 它还会帮你避免处理只更新了一部分 state 变量的令人困惑的“半成品”渲染。

**React does not batch across *multiple* intentional events like clicks**—<font color=red>each click is handled separately</font>. <font color=dodgerBlue>Rest assured</font> ( 👀 请放心 ) that <font color=red>React only does batching when it’s generally safe to do</font>. This ensures that, for example, if the first button click disables a form, the second click would not submit it again.

##### Updating the same state multiple times before the next render

It is an uncommon use case, but <font color=dodgerBlue>if you would like to update the same state variable multiple times before the next render</font>, instead of passing the *next state value* like `setNumber(number + 1)`, <font color=red>you can pass a *function* that **calculates the next state based on the previous one in the queue**, like `setNumber(n => n + 1)`</font>. <font color=fuchsia>It is a way to tell React to **“do something with the state value” instead of just replacing it**</font>.

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1); { /* 👀 */ }
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}
```

<font color=dodgerBlue>Here</font>, <font color=fuchsia>`n => n + 1` is called an **updater function**</font>. <font color=dodgerBlue>When you pass it to a state setter</font>:

1. React <font color=fuchsia>queues this function to be processed **after all the other code in the event handler has run**</font>.
2. <font color=fuchsia>**During the next render**</font>, <font color=red>React goes through the queue</font> and <font color=fuchsia>gives you the final updated state</font>.

```jsx
setNumber(n => n + 1);
setNumber(n => n + 1);
setNumber(n => n + 1);
```

<font color=dodgerBlue>Here’s how React works through these lines of code while executing the event handler</font>:

1. `setNumber(n => n + 1)` : `n => n + 1` is a function. <font color=fuchsia>React adds it to a queue</font>.
2. `setNumber(n => n + 1)` : `n => n + 1` is a function. <font color=fuchsia>React adds it to a queue</font>.
3. `setNumber(n => n + 1)` : `n => n + 1` is a function. <font color=fuchsia>React adds it to a queue</font>.

> 👀 这里 React 三次把 `setState` 放入 queue 中。因为传入的是一个函数，而不是一个写死的数值，所以，state 的值可以多次变化。另外，按照 [[#What happens if you update state after replacing it]] 的说法，显然传入一个函数是 “update” ，而传入一个写死的值是 “replace”

When you call `useState` during the next render, React goes through the queue. The previous `number` state was `0`, so that’s what React passes to the first updater function as the `n` argument. Then <font color=fuchsia>React takes the **return value of your previous updater function** and passes it to the next updater as `n`</font> , and so on:

| queued update | `n`  | returns     |
| ------------- | ---- | ----------- |
| `n => n + 1`  | `0`  | `0 + 1 = 1` |
| `n => n + 1`  | `1`  | `1 + 1 = 2` |
| `n => n + 1`  | `2`  | `2 + 1 = 3` |

React stores `3` as the final result and returns it from `useState`.

This is why clicking “+3” in the above example correctly increments the value by 3.

###### What happens if you update state after replacing it

What about this event handler? What do you think `number` will be in the next render?

```jsx
<button onClick={() => {
  setNumber(number + 5);
  setNumber(n => n + 1);
}}> { /* 最后 n === 6*/ }
```

Here’s what this event handler tells React to do:

1. `setNumber(number + 5)` : `number` is `0` , so `setNumber(0 + 5)` . React **adds *“<font color=fuchsia>replace</font> with `5`”* to its queue**.
2. `setNumber(n => n + 1)` : `n => n + 1` is an <font color=fuchsia>updater</font> function. React **adds *that function* to its queue**.

During the next render, React goes through the state queue:

| queued update      | `n`          | <font color=fuchsia>returns</font> |
| ------------------ | ------------ | ---------------------------------- |
| “replace with `5`” | `0` (unused) | `5`                                |
| `n => n + 1`       | `5`          | `5 + 1 = 6`                        |

React stores `6` as the final result and returns it from `useState`.

> 💡 Note
>
> You may have noticed that `setState(5)` actually works like `setState(n => 5)` , but `n` is unused!

###### What happens if you replace state after updating it

```jsx
<button onClick={() => {
  setNumber(number + 5);
  setNumber(n => n + 1);
  setNumber(42);
}}> { /* 最后 n === 42 */ }
```

Here’s how React works through these lines of code while executing this event handler:

1. `setNumber(number + 5)` : `number` is `0` , so `setNumber(0 + 5)`. React **adds *“replace with `5`”* to its queue**.
2. `setNumber(n => n + 1) `: `n => n + 1` is an updater function. React **adds *that function* to its queue**.
3. `setNumber(42)` : React **adds *“replace with `42`”* to its queue**.

During the next render, React goes through the state queue:

| queued update       | `n`          | returns     |
| ------------------- | ------------ | ----------- |
| “replace with `5`”  | `0` (unused) | `5`         |
| `n => n + 1`        | `5`          | `5 + 1 = 6` |
| “replace with `42`” | `6` (unused) | `42`        |

Then React stores `42` as the final result and returns it from `useState`.

<font color=dodgerBlue>To **summarize**</font>, here’s how you can think of what you’re passing to the `setNumber` state setter:

- <font color=red>**An updater function**</font> ( e.g. `n => n + 1` ) gets added to the queue.
- <font color=red>**Any other value**</font> ( e.g. number `5` ) adds “replace with `5`” to the queue, ignoring what’s already queued.

After the event handler completes, React will trigger a re-render. During the re-render, React will process the queue. Updater functions run during rendering, so **updater functions must be [pure](https://react.dev/learn/keeping-components-pure)** and only *return* the result. Don’t try to set state from inside of them or run other side effects. In Strict Mode, React will run each updater function twice (but discard the second result) to help you find mistakes.

##### Recap

- <font color=fuchsia>Setting state does not change the variable in the **existing render**</font>, but <font color=red>it requests a new render</font>.
- React <font color=red>processes state updates after event handler<font size=4>**s**</font> have finished running</font>. This is <font color=red>called batching</font>.
- To update some state multiple times in one event, you can use `setNumber(n => n + 1)` updater function.



#### Updating Objects in State

<font color=dodgerBlue>State can hold any kind of JavaScript value, including objects</font>. But you <font color=red>shouldn’t change objects that you hold in the React state directly</font>. <font color=dodgerBlue>Instead</font>, when you want to update an object, you <font color=red>need to create a new one (or make a copy of an existing one)</font>, and <font color=dodgerBlue>then</font> <font color=red>set the state to use that copy</font>.

##### What’s a mutation? 

You can store any kind of JavaScript value in state.

```jsx
const [x, setX] = useState(0);
```

<font color=lightSeaGreen>So far you’ve been working with numbers, strings, and booleans</font>. <font color=red>These kinds of JavaScript values are “immutable”, meaning unchangeable or “read-only”</font>. You can trigger a re-render to *replace* a value:

> ⚠️ 注意这里的说法： JS 值（比如说上面的初始值 `0` ）是不可变的，参考下面的说法：使用 `setState` 变的是 state，而非 `0`

```jsx
setX(5);
```

<font color=red>The `x` state changed from `0` to `5`</font> , but <font color=red>**the *number `0` itself* did not change**</font>. <font color=fuchsia>It’s **not possible to make any changes to the built-in primitive values** like numbers, strings, and booleans in JavaScript</font>.

<font color=dodgerBlue>Now consider an object in state:</font>

```jsx
const [position, setPosition] = useState({ x: 0, y: 0 });
```

Technically, <font color=red>it is **possible** to change the contents of *the object itself*</font>. **This is called a mutation:**

```jsx
position.x = 5;
```

However, <font color=dodgerBlue>although objects in React state are technically mutable</font>, you should treat them **as if** they were immutable—like numbers, booleans, and strings. <font color=dodgerBlue>Instead of mutating them</font>, you <font color=red>should **always replace** them</font>.

##### Treat state as read-only 

In other words, <font color=red>you should **treat any JavaScript object that you put into state as read-only**</font>.

This example holds an object in state to represent the current pointer position. The red dot is supposed to move when you touch or move the cursor over the preview area. <font color=dodgerBlue>But the dot stays in the initial position</font>:

```jsx
import { useState } from 'react';
export default function MovingDot() {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  return (
    <div
      onPointerMove={e => {
        position.x = e.clientX; { /*👀*/ }
        position.y = e.clientY;
      }}
      style={{
        position: 'relative',
        width: '100vw',
        height: '100vh',
      }}>
      <div style={{
        position: 'absolute',
        backgroundColor: 'red',
        borderRadius: '50%',
        transform: `translate(${position.x}px, ${position.y}px)`,
        left: -10,
        top: -10,
        width: 20,
        height: 20,
      }} />
    </div>
  );
}
```

The problem is with this bit of code.

```jsx
onPointerMove={e => {
  position.x = e.clientX;
  position.y = e.clientY;
}}
```

<font color=red>This code **modifies the object assigned to `position` from [the previous render](https://react.dev/learn/state-as-a-snapshot#rendering-takes-a-snapshot-in-time)**</font>. But <font color=lightSeaGreen>without using the state setting function</font>, <font color=fuchsia>React has no idea that object has changed</font>. <font color=lightSeaGreen>So **React does not do anything in response**</font>. It’s like trying to change the order after you’ve already eaten the meal. <font color=lightSeaGreen>While mutating state can work in some cases, we **don’t recommend it**</font>. You should treat the state value you have access to in a render as read-only.

To actually [trigger a re-render](https://react.dev/learn/state-as-a-snapshot#setting-state-triggers-renders) in this case, **create a *new* object and pass it to the state setting function:**

```jsx
onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}
```

With `setPosition`, you’re telling React:

- Replace `position` with this new object
- And render this component again

###### Local mutation is fine

> 👀 感觉这小节并没有什么关键点，除了 “modify existing object in state” 和 “local mutation”，这两个概念值得注意

<font color=dodgerBlue>Code like this is a problem because</font> <font color=red>it modifies an *existing* object in state</font>:

```jsx
position.x = e.clientX;
position.y = e.clientY;
```

But code like this is **absolutely fine** because you’re mutating a fresh object you have *just created*:

```jsx
const nextPosition = {};
nextPosition.x = e.clientX;
nextPosition.y = e.clientY;
setPosition(nextPosition);
```

In fact, it is completely equivalent to writing this:

```jsx
setPosition({
  x: e.clientX,
  y: e.clientY
});
```

Mutation is only a problem when you change *existing* objects that are already in state. Mutating an object you’ve just created is okay because *no other code references it yet.* <font color=lightSeaGreen>Changing it isn’t going to accidentally impact something that depends on it</font>. <font color=red>This is called a “local mutation”</font>. You can even do local mutation [while rendering](https://react.dev/learn/keeping-components-pure#local-mutation-your-components-little-secret). Very convenient and completely okay!

##### Copying objects with the spread syntax 

In the previous example, the `position` object is always created fresh from the current cursor position. <font color=dodgerBlue>But often</font>, you will <font color=red>want to include *existing* data as a part of the new object you’re creating</font>. For example, you may want to update *only one* field in a form, but keep the previous values for all other fields.

You can use the `...` object spread syntax so that you don’t need to copy every property separately.

> 👀 之前的思路是：使用 omit 提取出目标成员以外的剩余部分，比如 `const { tragetKey, ...rest } = obj` ，在 set 时再将其组装 `{ targetKey: targetNewValue, ...rest }` 。
>
> 显然这是不必要的，按照下面的示例，可以直接将其覆盖；比如： `{...obj, targetKey: targetNewValue}`  。不过，`targetKey: targetNewValue` **必须**放在 `...obj` 的后面，否则无法实现覆盖

```jsx
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleFirstNameChange(e) {
    setPerson({
      ...person,
      firstName: e.target.value
    });
  }

  function handleLastNameChange(e) {
    setPerson({
      ...person,
      lastName: e.target.value
    });
  }

  function handleEmailChange(e) {
    setPerson({
      ...person,
      email: e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          value={person.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={person.lastName}
          onChange={handleLastNameChange}
        />
      </label>
      <label>
        Email:
        <input
          value={person.email}
          onChange={handleEmailChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

<font color=dodgerBlue>Note that</font> <font color=red>the `...` spread syntax is “shallow”</font>—it <font color=red>only copies things one level deep</font>. This makes it fast, but it also means that if you want to update a nested property, you’ll have to use it more than once.

###### Using a single event handler for multiple fields

>  👀 即使用 js 的 “计算属性”，这个方法思路确实巧妙。另外值得注意的是，这里每一个 `<input>` 中加上了 `name` prop ，这个属性之前写 

You can also use the `[` and `]` braces inside your object definition to specify a property with dynamic name. Here is the same example, but with a single event handler instead of three different ones:

```jsx
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name] : e.target.value { /* 👀 */ }
    })
  }

  return (
    <>
      <label>
        First name:
        { /* 👀 注意下面的 name prop */ }
        <input
          name="firstName"
          value={person.firstName}
          onChange={handleChange}
        />
      </label>
      <label>
        Last name:
        <input
          name="lastName"
          value={person.lastName}
          onChange={handleChange}
        />
      </label>
      <label>
        Email:
        <input
          name="email"
          value={person.email}
          onChange={handleChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

##### Updating a nested object

Consider a nested object structure like this:

```jsx
const [person, setPerson] = useState({
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
});
```

> 👀 文档中并没有给出使用 js 计算属性 和 `name` prop 的代码，我自己实现了一下
>
> ```jsx
> function handleChange(e) {
>     const { name, value } = e.target;
>     if (name === "name") {
>       setPerson({
>         ...person,
>         [name]: value
>       });
>     } else {
>       setPerson({
>         ...person,
>         artwork: { ...person.artwork, [name]: value }
>       });
>     }
> }
> ```
>
> 详细的代码见 [codesandbox : react doc - Updating Objects in State # Updating a nested object](https://codesandbox.io/s/react-doc-updating-objects-in-state-updating-a-nested-object-ydjh7q)

###### Objects are not really nested

> 👀 开始时没有想把这部分放到笔记中，也感觉有点不知所云；看了 [[#Updating objects inside arrays]] 有点明白了：主要还是讲浅拷贝、深拷贝中的一些概念，即：对象（也包含数组）中的成员如果是对象，那么存储的是成员的索引（地址），而不是成员的内容，通过浅拷贝的方法获取到新对象，对新对象中的对象成员进行内容的修改也是会影响源对象的对应的对象成员的。
>
> 就是这样的意思，原文这里略

##### Write concise update logic with Immer

<font color=dodgerBlue>If your state is deeply nested</font>, you might want to consider [flattening it.](https://react.dev/learn/choosing-the-state-structure#avoid-deeply-nested-state) But, <font color=dodgerBlue>if you don’t want to change your state structure</font>, you might prefer a shortcut to nested spreads. <font color=red>[Immer](https://github.com/immerjs/use-immer) is a popular library that lets you write using the convenient but mutating syntax and takes care of producing the copies for you</font>. <font color=dodgerBlue>With Immer</font>, the code you write <font color=lightSeaGreen>looks like you are “breaking the rules” and mutating an object</font>:

```jsx
updatePerson(draft => {
  draft.artwork.city = 'Lagos';
});
```

> 👀 看了下 immer 的 文档，感觉 `draft` 作为 immutable 变量的名称算是一种惯例。
>
> ⚠️ 经过实践，需要注意的是：`updateState(draft => {})` 的 `{}` 是必须要加上的，否则将会报错：`An immer producer returned a new value *and* modified its draft. Either return a new value *or* modify the draft.` 。参考 [Immer doc - Returning new data from producers # Inline shortcuts using `void`](https://immerjs.github.io/immer/return/#inline-shortcuts-using-void) 的内容，也可以使用 `updateState(draft => void statements)` 解决；不过为了代码可读性的考虑，还是更推荐使用  `{}` 

But unlike a regular mutation, it doesn’t overwrite the past state!

> 💡 关于 immer 和 use-immer
>
> ##### immer
>
> > Immer (German for: always) is a tiny package that allows you to work with **immutable** state in a more convenient way.
> >
> > 摘自：[Immer doc - Introduction to Immer](https://immerjs.github.io/immer/)
>
> Immer 是 JS 生态下的工具，可以说它是框架无关的，在 Vue 中也可以使用；Vue3 官方文档中也介绍了 Immer：
>
> > 如果你正在实现一个撤销/重做的功能，你可能想要对用户编辑时应用的状态进行快照记录。然而，如果状态树很大的话，Vue 的可变响应性系统没法很好地处理这种情况，因为在每次更新时都序列化整个状态对象对 CPU 和内存开销来说都是非常昂贵的。
> >
> > [不可变数据结构](https://en.wikipedia.org/wiki/Persistent_data_structure)通过永不更改状态对象来解决这个问题。与 Vue 不同的是，它会创建一个新对象，保留旧的对象未发生改变的一部分。在 JavaScript 中有多种不同的方式来使用不可变数据，但我们推荐使用 [Immer](https://immerjs.github.io/immer/) 搭配 Vue，因为它使你可以在保持原有直观、可变的语法的同时，使用不可变数据。
> >
> > 我们可以通过一个简单的组合式函数来集成 Immer：
> >
> > ```js
> > import produce from 'immer'
> > import { shallowRef } from 'vue'
> > 
> > export function useImmer(baseState) {
> >   const state = shallowRef(baseState)
> >   const update = (updater) => {
> >     state.value = produce(state.value, updater)
> >   }
> > 
> >   return [state, update]
> > }
> > ```
> >
> > 摘自：[Vue doc - 深入响应式系统 # 不可变数据](https://cn.vuejs.org/guide/extras/reactivity-in-depth.html#immutable-data)
>
> 另外，等用到 Immutable.js，可以将 Immer 与 Immutable.js 做一下对比
>
> ###### use-immer
>
> > **About** : Use immer to drive state with a React hooks
>
> use-immer 是 Immer 专门适配 React hooks 的版本，让我们更方便地在 React 中使用 Immer。

<font color=dodgerBlue>To try Immer:</font>

1. Run `npm install use-immer` to add Immer as a dependency
2. Then <font color=red>**replace `import { useState } from 'react'`** with `import { useImmer } from 'use-immer'`</font>

> 👀 下面的代码是照着上面的 “统一 `handleChange` ” 改写的，通过测试是可行的；和原文中的不一样；不过很好的体现了 use-immer 的使用方法

```jsx
import { useImmer } from 'use-immer';

export default function Form() {
  const [person, updatePerson] = useImmer({ // 👀
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleChange(e) {
    const { name, value } = e.target

    updatePerson(draft => {
      if(name === 'name') {
        draft.name = value // 👀 “直接修改”
      } else {
        draft.artwork[name] = value
      }
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          name="name"
          value={person.name}
          onChange={handleChange}
        />
      </label>
      <label>
        Title:
        <input
          name="title"
          value={person.artwork.title}
          onChange={handleChange}
        />
      </label>
      <label>
        City:
        <input
          name="city"
          value={person.artwork.city}
          onChange={handleChange}
        />
      </label>
      <label>
        Image:
        <input
          name="image"
          value={person.artwork.image}
          onChange={handleChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img 
        src={person.artwork.image} 
        alt={person.artwork.title}
      />
    </>
  );
}
```

Notice how much **more concise** the event handlers have become. <font color=lightSeaGreen>You can mix and match `useState` and `useImmer` in a single component as much as you like</font>. Immer is a great way to keep the update handlers concise, <font color=lightSeaGreen>**especially if there’s nesting in your state**</font>, and copying objects leads to repetitive code.

###### How does Immer work?

<font color=fuchsia>The `draft` provided by Immer is a special type of object, **called a Proxy**</font>, that <font color=red>“records” what you do with it</font>. This is why you can mutate it freely as much as you like! <font color=dodgerBlue>Under the hood</font>, <font color=red>Immer figures out which parts of the `draft` have been changed</font>, and <font color=red>**produces a completely new object that contains your edits**</font>.

##### Why is mutating state not recommended in React?

There are a few reasons:

- **Debugging:** If you use `console.log` and don’t mutate state, <font color=lightSeaGreen>your past logs won’t get clobbered by the more recent state changes</font>. So you can clearly see how state has changed between renders.

  > 🌏 你之前日志中的 state 的值就不会被新的 state 变化所影响

- **Optimizations:** <font color=red>Common React [optimization strategies](https://react.dev/reference/react/memo) rely on skipping work if previous props or state are the same as the next ones</font>（👀 即批处理）. If you never mutate state, it is very fast to check whether there were any changes. If `prevObj === obj` , you can be sure that nothing could have changed inside of it.

- **New Features:** <font color=red>The new React features we’re building **rely on state being [treated like a snapshot](https://react.dev/learn/state-as-a-snapshot)**</font>. If you’re mutating past versions of state, that <font color=lightSeaGreen>may prevent you from using the new features</font>.

- **Requirement Changes:** Some application features, <font color=lightSeaGreen>like implementing **Undo/Redo**, **showing a history of changes**, or letting the user reset a form to earlier values, are easier to do when nothing is mutated</font>. This is <font color=red>because you can keep past copies of state in memory, and reuse them when appropriate</font>. If you start with a mutative approach, features like this can be difficult to add later on.

- **Simpler Implementation:** <font color=red>Because React **does not rely on mutation**</font>, it does not need to do anything special with your objects. <font color=fuchsia>It **does not need to hijack their properties**, always **wrap them into Proxies**, or do other work at initialization as many “reactive” solutions do</font>. This is also why React lets you put any object into state—no matter how large—<font color=red>**without additional performance** or correctness pitfalls</font>.

  > 👀 这里变相的在阐述 React 没有使用响应式的原因，只能说各有各的设计考量

In practice, you can often “get away” with mutating state in React, but we strongly advise you not to do that so that you can use new React features developed with this approach in mind. Future contributors and perhaps even your future self will thank you!

##### Recap

- <font color=red>Treat all state in React as immutable</font>.
- When you store objects in state, mutating them will not trigger renders and will change the state in previous render “snapshots”.
- Instead of mutating an object, create a *new* version of it, and trigger a re-render by setting state to it.
- You can use the `{...obj, something: 'newValue'}` object spread syntax to create copies of objects.
- Spread syntax is shallow: it only copies one level deep.
- To update a nested object, you need to create copies all the way up from the place you’re updating.
- To reduce repetitive copying code, use Immer.



#### Updating Arrays in State

Arrays are mutable in JavaScript, but <font color=red>you should treat them as immutable when you store them in state</font>. <font color=dodgerBlue>Just like with objects</font>, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array.

##### Updating arrays without mutation 

In JavaScript, arrays are just another kind of object. [Like with objects](https://react.dev/learn/updating-objects-in-state), **you should treat arrays in React state as read-only.** <font color=lightSeaGreen>This means that you shouldn’t reassign items inside an array like `arr[0] = 'bird'`</font> , and <font color=red>you also **shouldn’t use methods that mutate the array, such as `push()` and `pop()`** </font>.

Instead, every time you want to update an array, you’ll want to pass a *new* array to your state setting function. <font color=dodgerBlue>To do that</font>, <font color=red>you can create a new array from the original array in your state by **calling its non-mutating methods like `filter()` and `map()`**</font>. Then you can set your state to the resulting new array.

Here is a reference table of common array operations. When dealing with arrays inside React state, <font color=lightSeaGreen>you will need to avoid the methods in the left column</font>, and instead prefer the methods in the right column:

|           | avoid (mutates the array)           | prefer (returns a new array)                                 |
| --------- | ----------------------------------- | ------------------------------------------------------------ |
| adding    | `push`, `unshift`                   | `concat`, `[...arr]` spread syntax ([example](https://react.dev/learn/updating-arrays-in-state#adding-to-an-array)) |
| removing  | `pop`, `shift`, `splice`            | `filter`, `slice` ([example](https://react.dev/learn/updating-arrays-in-state#removing-from-an-array)) |
| replacing | `splice`, `arr[i] = ...` assignment | `map` ([example](https://react.dev/learn/updating-arrays-in-state#replacing-items-in-an-array)) |
| sorting   | `reverse`, `sort`                   | copy the array first ([example](https://react.dev/learn/updating-arrays-in-state#making-other-changes-to-an-array)) |

Alternatively, you can [use Immer](https://react.dev/learn/updating-arrays-in-state#write-concise-update-logic-with-immer) which lets you use methods from both columns.

###### Removing from an array

The easiest way to remove an item from an array is to *filter it out*. In other words, you will produce a new array that will not contain that item. To do this, <font color=red>use the `filter` method</font>, for example:

> 👀 用 filter 实现 “删除” 的效果，这是之前没有想到的

> 👀 下面的代码，和文档中的不一致；把内联的函数提取出来了

```jsx
import { useState } from 'react';

let initialArtists = [
  { id: 0, name: 'Marta Colvin Andrade' },
  { id: 1, name: 'Lamidi Olonade Fakeye'},
  { id: 2, name: 'Louise Nevelson'},
];

export default function List() {
  const [artists, setArtists] = useState(
    initialArtists
  );

  function onDel(id) {
    setArtists(
      artists.filter(a => a.id !== id)
    );
  }

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>
            {artist.name}{' '}
            <button onClick={() => onDel(artist.id)}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </>
  );
}
```

###### Transforming an array 

<font color=dodgerBlue>If you want to change some or all items of the array</font>, you <font color=red>can use `map()` to create a **new** array</font>. The function you will pass to `map` can decide what to do with each item, based on its data or its index (or both).

###### Replacing items in an array

> 👀 这个场景可以使用  ES2023 语法 [`Array.prototype.toSpliced()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced)

It is particularly common to want to replace one or more items in an array. Assignments like `arr[0] = 'bird'` are mutating the original array, so instead you’ll want to <font color=red>use `map` for this as well</font>.

To replace an item, create a new array with `map`. Inside your `map` call, you will receive the item index as the second argument. Use it to decide whether to return the original item (the first argument) or something else:

```jsx
import { useState } from 'react';

let initialCounters = [
  0, 0, 0
];

export default function CounterList() {
  const [counters, setCounters] = useState(
    initialCounters
  );

  function handleIncrementClick(index) {
    const nextCounters = counters.map((c, i) => {
      if (i === index) { // Increment the clicked counter
        return c + 1;
      } else { // The rest haven't changed
        return c;
      }
    });
    setCounters(nextCounters);
  }

  return (
    <ul>
      {counters.map((counter, i) => (
        <li key={i}>
          {counter}
          <button onClick={() => {
            handleIncrementClick(i);
          }}>+1</button>
        </li>
      ))}
    </ul>
  );
}
```

###### Inserting into an array

> 👀 这个场景可以使用  ES2023 语法 [`Array.prototype.toSpliced()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced)

Sometimes, you may want to <font color=dodgerBlue>insert an item at a particular position that’s neither at the beginning nor at the end</font>. To do this, <font color=red>you can use the `...` array spread syntax together with the `slice()` method</font>. The `slice()` method lets you cut a “slice” of the array. To insert an item, you will create an array that spreads the slice *before* the insertion point, then the new item, and then the rest of the original array.

> 👀 由于文档中示例的 jsx 有些繁琐，所以这里给出简化逻辑代码

```jsx
setState([
  ...state.slice(0, insertAt),
  insertValue,
  ...state.slice(insertAt)
])
```

###### Making other changes to an array

> 👀 这个场景可以使用  ES2023 的语法 [`Array.prototype.toSorted()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted) 和 [`Array.prototype.toSpliced()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced) 以及 [`Array.prototype.with()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/with)

There are some things you can’t do with the spread syntax and non-mutating methods like `map()` and `filter()` alone. <font color=dodgerBlue>For example, you may want to reverse or sort an array</font>. The JavaScript `reverse()` and `sort()` methods are mutating the original array, so you can’t use them directly.

**However, you can copy the array first, and then make changes to it.**

##### Updating objects inside arrays

<font color=red>Objects are not *really* located “inside” arrays</font>. <font color=lightSeaGreen>They might appear to be “inside” in code, but each object in an array is a separate value, to which the array “points”</font>. This is why you need to be careful when changing nested fields like `list[0]` . Another person’s artwork list may point to the same element of the array!

**When updating nested state, you need to create copies from the point where you want to update, and all the way up to the top level.**

> 👀 这里实际是在讲浅拷贝和深拷贝中的一些概念。
>
> 下面给出了示例：不过没有用深拷贝（无脑）解决，而是用 map，对应不同情况手动使用 `...` 浅拷贝

```jsx
import { useState } from 'react';

let nextId = 3;
const initialList = [
  { id: 0, title: 'Big Bellies', seen: false },
  { id: 1, title: 'Lunar Landscape', seen: false },
  { id: 2, title: 'Terracotta Army', seen: true },
];

export default function BucketList() {
  const [myList, setMyList] = useState(initialList);
  const [yourList, setYourList] = useState(initialList);

  function handleToggleMyList(artworkId, nextSeen) {
    setMyList(myList.map(artwork => {
      if (artwork.id === artworkId) { // Create a *new* object with changes
        return { ...artwork, seen: nextSeen };
      } else { 
        return artwork;
      }
    }));
  }

  function handleToggleYourList(artworkId, nextSeen) {
    setYourList(yourList.map(artwork => {
      if (artwork.id === artworkId) { // Create a *new* object with changes
        return { ...artwork, seen: nextSeen };
      } else { // No changes
        return artwork;
      }
    }));
  }

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>My list of art to see:</h2>
      <ItemList
        artworks={myList}
        onToggle={handleToggleMyList} />
      <h2>Your list of art to see:</h2>
      <ItemList
        artworks={yourList}
        onToggle={handleToggleYourList}
      />
    </>
  );
}

function ItemList({ artworks, onToggle }) {
  return (
    <ul>
      {artworks.map(artwork => (
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              checked={artwork.seen}
              onChange={e => {
                onToggle(artwork.id, e.target.checked);
              }}
            />
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}
```

##### Write concise update logic with Immer 

Updating nested arrays without mutation can get a little bit repetitive. [Just as with objects](https://react.dev/learn/updating-objects-in-state#write-concise-update-logic-with-immer):

- Generally, you shouldn’t need to update state more than a couple of levels deep. If your state objects are very deep, you might want to [restructure them differently](https://react.dev/learn/choosing-the-state-structure#avoid-deeply-nested-state) so that they are flat.
- <font color=dodgerBlue>If you don’t want to change your state structure</font>, <font color=red>you might prefer to use [Immer](https://github.com/immerjs/use-immer)</font>, which lets you write using the convenient but mutating syntax and takes care of producing the copies for you.

```jsx
const [myList, updateMyList] = useImmer(initialList);
const [yourList, updateYourList] = useImmer(initialList);

function handleToggleMyList(id, nextSeen) {
  updateMyList(draft => {
    const artwork = draft.find(a => a.id === id);
    artwork.seen = nextSeen;
  });
}

function handleToggleYourList(artworkId, nextSeen) {
  updateYourList(draft => {
    const artwork = draft.find(a => a.id === artworkId);
    artwork.seen = nextSeen;
  });
}
```

This is because you’re not mutating the *original* state, but you’re mutating a special `draft` object provided by Immer. <font color=dodgerBlue>Similarly</font>, <font color=red>you can apply mutating methods like `push()` and `pop()` to the content of the `draft`</font>.

Behind the scenes, Immer always constructs the next state from scratch according to the changes that you’ve done to the `draft`. This keeps your event handlers very concise without ever mutating state.

##### Recap

- You can put arrays into state, but you can’t change them.
- Instead of mutating an array, create a *new* version of it, and update the state to it.
- You can use the `[...arr, newItem]` array spread syntax to create arrays with new items.
- You can use `filter()` and `map()` to create new arrays with filtered or transformed items.
- You can use Immer to keep your code concise.



### Managing State

<font color=dodgerBlue>As your application grows</font>, it <font color=red>helps to be more intentional about **how your state is organized** and **how the data flows between your components**</font>. <font color=fuchsia>Redundant or duplicate state is a **common source of bugs**</font>. In this chapter, you’ll learn how to structure your state well, how to keep your state update logic maintainable, and how to share state between distant components.

#### Reacting to Input with State

<font color=lightSeaGreen>React provides a declarative way to manipulate the UI</font>（🌏 React 控制 UI 的方式是声明式的）. Instead of manipulating individual pieces of the UI directly, you describe the different states that your component can be in, and switch between them in response to the user input. This is similar to how designers think about the UI.

##### How declarative UI compares to imperative

> 🌏 声明式 UI 与命令式 UI 的比较

> 👀 这部分不少篇幅在讲 “命令式 UI” 的缺点，感觉不算是重点；在读完《Vue.js 设计与实现》 的第一章后，感觉很多部分都很熟悉，这里略，只保留 React 相关的部分。
>
> 另外，也可以看下 《Vue.js 设计与实现》 的第一章，感觉更加全面的比较了“声明式 UI” 与 “命令式 UI”

<font color=dodgerBlue>In React, you don’t directly manipulate the UI</font>—meaning you don’t enable, disable, show, or hide components directly. Instead, <font color=red>you **declare what you want to show,** and React figures out how to update the UI</font>. Think of getting into a taxi and telling the driver where you want to go instead of telling them exactly where to turn. It’s the driver’s job to get you there, and they might even know some shortcuts you haven’t considered!

<img src="https://s2.loli.net/2023/10/02/AvEnowyH15aWezB.png" alt="In a car driven by React, a passenger asks to be taken to a specific place on the map. React figures out how to do that." style="zoom:24%;" />

##### Thinking about UI declaratively

You’ve seen how to implement a form imperatively above. To better understand how to think in React, <font color=dodgerBlue>you’ll walk through reimplementing this UI in React below</font>:

1. **Identify** your component’s different visual states
2. **Determine** what triggers those state changes
3. <font color=red>**Represent** the state in memory using `useState`</font>
4. **Remove** any non-essential state variables
5. <font color=red>**Connect** the event handlers to set the state</font>

###### Step 1: Identify your component’s different visual states 

In computer science, you may hear about a [“state machine”](https://en.wikipedia.org/wiki/Finite-state_machine) being in one of several “states”. If you work with a designer, you may have seen mockups for different “visual states”. <font color=lightSeaGreen>React stands at the intersection of design and computer science</font>, so both of these ideas are sources of inspiration.

First, you need to visualize all the different “states” of the UI the user might see:

- **Empty**: Form has a disabled “Submit” button.
- **Typing**: Form has an enabled “Submit” button.
- **Submitting**: Form is completely disabled. Spinner is shown.
- **Success**: “Thank you” message is shown instead of a form.
- **Error**: Same as Typing state, but with an extra error message.

###### Step 2: Determine what triggers those state changes

<font color=dodgerBlue>You can trigger state updates in response to two kinds of inputs:</font>

- **Human inputs,** like clicking a button, typing in a field, navigating a link.
- **Computer inputs,** like a network response arriving, a timeout completing, an image loading.

<img src="https://s2.loli.net/2023/10/03/GQVHxMaTfDkLznX.png" alt="image-20231003090953865" style="zoom:45%;" />

##### Step 3: Represent the state in memory with `useState`

Next you’ll need to represent the visual states of your component in memory with [`useState`.](https://react.dev/reference/react/useState) <font color=red>Simplicity is key</font>: each piece of state is a “moving piece”, and **you want as few “moving pieces” as possible.** <font color=lightSeaGreen>More complexity leads to more bugs!</font>

Your first idea likely won’t be the best, but that’s ok—refactoring state is a part of the process!

##### Step 4: Remove any non-essential state variables

You want to avoid duplication in the state content so you’re only tracking what is essential. Spending a little time on refactoring your state structure will make your components easier to understand, reduce duplication, and avoid unintended meanings. <font color=red>Your goal is to **prevent the cases where the state in memory doesn’t represent any valid UI that you’d want a user to see**</font>. (For example, you never want to show an error message and disable the input at the same time, or the user won’t be able to correct the error!)

Here are some questions you can ask about your state variables:

- **Does this state cause a paradox?** A paradox usually means that the state is not constrained enough.

- **Is the same information available in another state variable already?**

  > 👀 感觉可以理解为：找到元信息，而非衍生信息

- **Can you get the same information from the inverse of another state variable?**

###### Eliminating “impossible” states with a reducer

These three variables are a good enough representation of this form’s state. <font color=dodgerBlue>However, there are still some intermediate states that don’t fully make sense</font>. For example, a non-null `error` doesn’t make sense when `status` is `'success'`. <font color=dodgerBlue>**To model the state more precisely**</font>, <font color=red>you can [extract it into a reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)</font>. <font color=fuchsia>Reducers let you **unify multiple state variables into a single object** and consolidate all the related logic!</font>

##### Step 5: Connect the event handlers to set state

(👀 和命令式编程比较，声明式编程) it is much less fragile. Expressing all interactions as state changes lets you later introduce new visual states without breaking existing ones. It also lets you change what should be displayed in each state without changing the logic of the interaction itself.

##### Recap

- Declarative programming means describing the UI for each visual state <font color=lightSeaGreen>rather than micromanaging the UI (imperative)</font>.
- When developing a component:
  1. Identify all its visual states.
  2. Determine the human and computer triggers for state changes.
  3. Model the state with `useState`.
  4. <font color=red>Remove non-essential state to avoid bugs and paradoxes</font>.
  5. Connect the event handlers to set state.



#### Choosing the State Structure

<font color=dodgerBlue>Structuring state well</font> <font color=red>can make a difference between a component that is pleasant to modify and debug</font>, and one that is a constant source of bugs. Here are some tips you should consider when structuring state.

##### Principles for structuring state 

When you write a component that holds some state, you’ll have to make choices about how many state variables to use and what the shape of their data should be. While it’s possible to write correct programs even with a suboptimal state structure, <font color=dodgerBlue>there are a few principles that can guide you to make better choices</font>:

1. **Group related state.** <font color=dodgerBlue>If you always update two or more state variables at the same time</font>, <font color=red>consider merging them into a single state variable</font>.
2. **Avoid contradictions in state.** When the state is structured in a way that several pieces of state may contradict and “disagree” with each other, you leave room for mistakes. Try to avoid this.
3. **Avoid redundant state.** If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state.
4. **Avoid duplication in state.** <font color=dodgerBlue>When the same data is duplicated between multiple state variables, or within nested objects</font>, <font color=red>it is difficult to keep them in sync</font>. Reduce duplication when you can.
5. <font color=red>**Avoid deeply nested state**</font>. Deeply hierarchical state is not very convenient to update. When possible, prefer to structure state in a flat way.

The goal behind these principles is to *make state easy to update without introducing mistakes*. Removing redundant and duplicate data from state helps ensure that all its pieces stay in sync. <font color=lightSeaGreen>This is similar to how a database engineer might want to [“normalize” the database structure](https://docs.microsoft.com/en-us/office/troubleshoot/access/database-normalization-description) to reduce the chance of bugs</font>. To paraphrase Albert Einstein, **“Make your state as simple as it can be—but no simpler.”**

Now let’s see how these principles apply in action.

##### Group related state 

You might sometimes be unsure between using a single or multiple state variables.

Should you do this?

```jsx
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```

Or this?

```jsx
const [position, setPosition] = useState({ x: 0, y: 0 });
```

Technically, you can use either of these approaches. But **if some two state variables always change together, it might be a good idea to unify them into a single state variable.** Then you won’t forget to always keep them in sync.

Another case where you’ll <font color=red>group data into an object or an array</font> is <font color=dodgerBlue>when **you don’t know how many pieces of state you’ll need**</font>. For example, it’s helpful when you have a form where the user can add custom fields.

##### Avoid contradictions in state

**Since `isSending` and `isSent` should never be `true` at the same time, it is better to replace them with one `status` state variable that may take one of *three* valid states:** `'typing'` (initial), `'sending'`, and `'sent'` .

##### Avoid redundant state 

If you can calculate some information from the component’s props or its existing state variables during rendering, you **should not** put that information into that component’s state.

For example, take this form. It works, but can you find any redundant state in it?

> 👀 需要注意的是：加上 `fullName` 这个 state 之后，在连续输入时要比“不加上 `fullName` ” 要卡不少

```jsx
import { useState } from 'react';

export default function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  const [fullName, setFullName] = useState('');

  function handleFirstNameChange(e) {
    setFirstName(e.target.value);
    setFullName(e.target.value + ' ' + lastName);
  }

  function handleLastNameChange(e) {
    setLastName(e.target.value);
    setFullName(firstName + ' ' + e.target.value);
  }
  
  return ( { /* ... */ } )
}
```

This form has three state variables: `firstName`, `lastName`, and `fullName`. However, `fullName` is redundant. **You can always calculate `fullName` from `firstName` and `lastName` during render, so remove it from state**.

###### Don’t mirror props in state

A common example of redundant state is code like this:

```jsx
function Message({ messageColor }) {
  const [color, setColor] = useState(messageColor);
}
```

Here, a `color` state variable is initialized to the `messageColor` prop. The problem is that <font color=red>**if the parent component passes a different value of `messageColor` later (for example, `'red'` instead of `'blue'`), the `color` *state variable* would not be updated!**</font> The state is only initialized during the first render.

This is why “mirroring” some prop in a state variable can lead to confusion. Instead, use the `messageColor` prop directly in your code. If you want to give it a shorter name, use a constant:

```jsx
function Message({ messageColor }) {
  const color = messageColor;
}
```

This way it won’t get out of sync with the prop passed from the parent component.

“Mirroring” props into state only makes sense when you *want* to ignore all updates for a specific prop. <font color=fuchsia>By convention, **start the prop name with `initial` or `default` to clarify** that its new values are ignored</font>:

```jsx
function Message({ initialColor }) {
  // The `color` state variable holds the *first* value of `initialColor`.
  // Further changes to the `initialColor` prop are ignored.
  const [color, setColor] = useState(initialColor);
}
```

##### Avoid duplication in state

> 👀 感觉完全在讲例子，没有其他东西；略

##### Avoid deeply nested state

[Updating nested state](https://react.dev/learn/updating-objects-in-state#updating-a-nested-object) involves making copies of objects all the way up from the part that changed. Deleting a deeply nested place would involve copying its entire parent place chain. Such code can be very verbose.

**If the state is too nested to update easily, consider making it “flat”.**

> 👀 演示代码有点长，略。不过感觉很有价值，很受启发；见 [codesandbox.io - Avoid deeply nested state's demo](https://codesandbox.io/s/mlsd2f?file=/App.js&utm_medium=sandpack)

Sometimes, you can also reduce state nesting by moving some of the nested state into the child components. This works well for ephemeral UI state that doesn’t need to be stored, like whether an item is hovered.





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