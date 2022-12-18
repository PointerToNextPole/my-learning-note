# Vue 原理与实现



### 《Vue 3 Deep Dive with Evan You》笔记

> 🔗 ：https://www.bilibili.com/video/BV1rC4y187Vw



#### 总述

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

首先，模板编译器将 HTML 转换成 render 函数，响应式模块初始化响应对象。然后，在渲染模块中，进入渲染阶段；调用 render 函数，render 函数引用了响应对象，开始侦听响应对象的变化；render 函数返回一个虚拟 DOM 节点。在挂载阶段，调用 mount 函数，使用 虚拟 DOM 节点创建 web 页面。最后，（补丁阶段）如果响应对象发生任何变化，将被监听到，渲染器将再次调用 render 函数，创建一个新的虚拟 DOM 节点。新的 vnode 和 旧的 vnode 被发送到 patch 函数中，根据需要更新网页



#### render 函数

##### Vue2 render 函数

```js
render(h) {
  return h('div', {
    attrs: { id: 'foo' },
    on: { click: this.onClick }
  }, 'hello')
}
```

h 函数的第一个参数是“类型”（👀 尤雨溪的说法），第二个参数对象，包含 vnode 上所有的数据或者属性。第三个参数，可以是字符串，表示是一个文本子节点；也可以是数组，以包含多个字节点

Vue2 render 有点啰嗦 verbose：第二个参数，需要指明传递给节点的绑定类型。比如，绑定属性，需要嵌套在 attrs 下；绑定事件侦听器，需要嵌套在 on 下。

##### Vue3 render 函数

Vue3 做了一些简化：对 第二个参数 props 对象做了扁平化处理。按照惯例，监听器以 on 开头，任何以 on 为前缀的都会自动绑定为一个监听器。另外，将 h 函数全局引入，这使得在同一个文件中拆分大的 render 函数，变得很方便（不需要多次传递）

```js
import { h } from 'vue'

render() {
  return h('div', {
    id: 'foo',
    onClick: this.onClick
  }, 'hello')
}
```

h 是 hyperscript 的缩写。

##### render 进一步用法

在 render 中没有 v-if，可以用 三元运算符 `expr ? :` 替代。如下：

```js
import { h } from 'vue'

const App = {
  render() {
    // 这里可以添加逻辑，比如下面 return 中使用的内容。感觉和 React 的 render 很像...
    
    return this.ok
      ? h('div', { id: 'hello' }, [h('span', 'world')])
      : h('p', 'other branch')
  }
}
```

类似的，没有 v-for，可以使用 map 函数来替代。



##### vdom 实现 ( h , mount , patch )

```html
<div id="app"></div>

<script>
  function h(tag, props, children) {
    return { tag, props, children };
  }

  function mount(vnode, container) {
    const el = (vnode.el = document.createElement(vnode.tag));

    if (vnode.props) {
      for (const key in vnode.props) {
        const value = vnode.props[key];
        el.setAttribute(key, value);
      }
    }
    // children
    if (vnode.children) {
      if (typeof vnode.children === "string") {
        el.textContent = vnode.children;
      } else {
        vnode.children.forEach((child) => {
          mount(child, el);
        });
      }
    }

    container.appendChild(el);
  }

  const vdom = h("div", { class: "red" }, [h("span", null, "hello")]);

  mount(vdom, document.getElementById("app"));

  function patch(n1, n2) {
    if (n1.tag == n2.tag) {
      const el = (n2.el = n1.el);

      // props
      const oldProps = n1.props || {};
      const newProps = n2.props || {};
      for (const key in newProps) {
        const oldValue = oldProps[key];
        const newValue = newProps[key];
        if (newValue !== oldValue) {
          el.setAttribute(key, newValue);
        }
      }
      for (const key in oldProps) {
        if (!(key in newProps)) {
          el.removeAttribute(key);
        }
      }

      // children
      const oldChildren = n1.children;
      const newChildren = n2.children;

      if (typeof newChildren === "string") {
        if (typeof oldChildren === "string") {
          if (newChildren !== oldChildren) {
            el.textContent = newChildren;
          }
        } else {
          el.textContent = newChildren;
        }
      } else {
        if (typeof oldChildren === "string") {
          el.innerHTML = "";
          newChildren.forEach((child) => {
            mount(child, el);
          });
        } else {
          const commonLength = Math.min(oldChildren.length, newChildren.length);
          for (let i = 0; i < commonLength; i++) {
            patch(oldChildren[i], newChildren[i]);
          }
          if (newChildren.length > oldChildren.length) {
            newChildren.slice(oldChildren.length).forEach((child) => {
              mount(child, el);
            });
          } else if (newChildren.length < oldChildren.length) {
            oldChildren.slice(newChildren.length).forEach((child) => {
              el.removeChild(child.el);
            });
          }
        }
      }
    } else {
      // replace, 略
    }
  }

  const vdom2 = h('div', { class: 'green' }, [
    h('span', null, 'changed')
  ])
  
  patch(vdom, vdom2)
</script>
```



可以使用 https://vue-next-template-explorer.netlify.app 来将 Vue3 的模板 按照一定的自定义的选项配置（比如静态提升）转化成 render 函数的版本。



#### 响应式原理

##### 响应式示例

```html
<span class="cell b1"> {{ state.a * 10 }} </span>

<script>
  onStateChanged(() => { 
    view = render(state) 
  })
</script>
```

那么，如何实现 上面抽象的 `onStateChanged` 函数？理想化的、假设状态是不变的，方法是：

```js
let update, state

const onStateChanged = _update => {
  update = _update // 缓存输入的更新函数 _update
}

// 暴露出的 setState 方法，将会使用新的状态替换旧的状态，并调用缓存的更新函数
const setState = newState => {
  state = newState
  update()
}

// 用户需要手动调用 setState，来告诉框架，数据发生了变化，需要做相应的响应。
setState({a: 5})
```

上面 `setState` 的操作 和 React 的工作原理很相似。而在 Vue 中是使用 `state.a = 5` ，要追踪依赖变得更加复杂



##### 依赖追踪的数据粒度

依赖追踪有好处也有坏处 ( pros and cons )，但是相较其他的解决方案，其最大的好处是：允许我们做非常细粒度的追踪实际状态的变化，只会在需要的部分触发变化。当然，依赖追踪本身也会在记录状态时产生运行时开销，所以需要找到一个好的依赖追踪关系的粒度。在实践中发现：在组件级别进行依赖追踪更加有效。



##### watchEffect API

watchEffect API 就是依赖追踪的实现函数（就如同上面 [[#响应式示例]] 的 `onStateChanged` ）。不像 Vue2 中的 watch 是监听某个属性，当属性发生变化，便执行回调；watchEffect 会监听整个函数，直接运行它；并追踪该函数执行时 所有使用过的响应式属性。示例如下：

```js
import { reactive, watchEffect } from 'vue'

const state = reactive({ count: 0 })

watchEffect(() => {
  console.log(state.count)
}) // 0

state.count++ // 1
```



#### 从头开始实现响应式

##### Dep & watchEffect 实现

```js
let activeEffect; // 保存添加哪一个函数作为订阅，另外这里没有说清楚 activeEffect 的作用，见下面

class Dep {
  constructor(value) {
    this.subscribers = new Set(); // 订阅者
    this._value = value // 将 dep 和一个值相关联
  }
  get value() {
    this.depend() // ⭐️
    return this._value
  }
  set value(newValue) {
    this._value = newValue
    this.notify() // ⭐️
  }
  depend() {
    if (activeEffect) {
      this.subscribers.add(activeEffect);
    }
  }
  notify() {
    // 通知订阅者
    this.subscribers.forEach(effect => {
      effect();
    });
  }
}

// 类似代码可见 https://cn.vuejs.org/guide/extras/reactivity-in-depth.html#how-reactivity-works-in-vue
function watchEffect(effect) {
  activeEffect = effect;
  /* 这是为了访问 dep.value，以触发 getter，从而收集依赖添加入 subscribers 中。否则 subscribers 为空 */
  effect(); // console.log -> 'hello'
  activeEffect = null; // 👀 因为每次调用 effect 后都会重置 activeEffect，所以 activeEffect 是一个单一变量是可以的
}

const dep = new Dep('hello'); 

watchEffect(() => {
  console.log(dep.value);
});

dep.value = 'changed' // console.log -> 'changed'
```

> 👀 关于 activeEffect 的作用，简单来说就是 “ 正在运行的 effect ，避免对一个依赖进行多次/重复收集 ”（也因此，上面 watchEffect 中最后的赋为 null，也得到了解释）；具体见 [[#activeEffect]]。另外，在源码中，也是有这个变量。
>
> <img src="https://s2.loli.net/2022/11/24/68mgj3DGoMdWzOs.png" alt="image-20221124233057515" style="zoom:60%;" />
>
> 见：https://github.dev/vuejs/core/blob/main/packages/reactivity/src/effect.ts#L48 。

这里的 Dep 实现，和 composition API 中的 ref 非常相似。

> ⚠️ 注意：这里使用的是 depend 和 notify，而不是 Vue3 源码中的 [track](https://github.dev/vuejs/core/blob/main/packages/reactivity/src/effect.ts#L213) 和 [trigger](https://github.dev/vuejs/core/blob/main/packages/reactivity/src/effect.ts#L259)，尤雨溪的说法是：为了减少其中与响应式原理不相关的优化，从而更好的理解工作原理。而我个人的感觉是：这里定义的是 Dep 类，这是 Vue2 特有的，Vue3 取消了 Dep 类的设计，所以自然使用 Dep 类相关的 depend 和 notify
>
> 👀 另外，关于 depend notify 和 track trigger ，建议去看下 [[#尤雨溪源码解读#depend notify v.s. track trigger]]



##### reactive 实现

实现 reactive 可以复用上面代码中的一些逻辑。

因为在响应式内部使用时，依赖类不需要追踪它自己的值，因为值就在对象上；所以可以去掉 `_value` 和 对应的 getter / setter。

###### Vue2 风格实现

使用 `Object.defineProperty` ：

```js
let activeEffect; // 保存添加哪一个函数作为订阅

class Dep {
  subscribers = new Set(); // 订阅者
  depend() {
    if (activeEffect) {
      this.subscribers.add(activeEffect);
    }
  }
  notify() {
    // 通知订阅者
    this.subscribers.forEach((effect) => {
      effect();
    });
  }
}

function watchEffect(effect) {
  activeEffect = effect;
  effect();
  activeEffect = null;
}

function reactive(raw) {
  Object.keys(raw).forEach((key) => {
    const dep = new Dep();
    let value = raw[key];

    // Vue2 style
    Object.defineProperty(raw, key, {
      get() {
        dep.depend();
        return value; // ⚠️ 这里形成了闭包，存储了属性关联的 Dep，即：value 变量保存了 raw[key]
      },
      set(newValue) {
        value = newValue;
        dep.notify();
      },
    });
  });
  return raw;
}

const state = reactive({
  count: 0,
});

watchEffect(() => {
  console.log(state.count);
});

state.count++;
```

###### Vue3 风格实现

在 ES6 Proxy 的加持下，可以讲 reactive 的实现做如下修改：

```js
let activeEffect; // 保存添加哪一个函数作为订阅

class Dep {
  subscribers = new Set(); // 订阅者
  depend() {
    if (activeEffect) {
      this.subscribers.add(activeEffect);
    }
  }
  notify() {
    // 通知订阅者
    this.subscribers.forEach((effect) => {
      effect();
    });
  }
}

function watchEffect(effect) {
  activeEffect = effect;
  effect();
  activeEffect = null;
}

// 因为 reactiveHandler 只创建一次，为了在运行时找到同一个那个 dep 实例，所以使用全局的 weakMap 来缓存 dep 实例，并保持唯一性 🤔 猜测：这也是没有形成闭包，无法保存依赖的解决方法？
// 另外，因为 weakMap 的 key 必须是对象，并且在 key 不可达，对应的 value 可以自动触发 GC。正因为此，weakMap 的 key 不可枚举。
const targetMap = new WeakMap()

function getDep(target, key) {
  let depsMap = targetMap.get(target)
  if(!depsMap) {
    depsMap = new Map()
    targetMap.set(target, depsMap)
  }
  let dep = depsMap.get(key)
  if(!dep) {
    dep = new Dep()
    depsMap.set(key, dep)
  }
  return dep
}

// reactiveHandler 拆出来是为了只创建一次，避免重新创建
const reactiveHandlers = {
  get(target, key, receiver) {
    const dep = getDep(target, key)

    dep.depend()
    return Reflect.get(target, key, receiver) 
    // 虽然也可以 return target[key]。但考虑到原型继承问题，在这种情况下，receiver 和 target 会指向不同的东西。总之，用 Reflect 让一切正常。👀 更详细一些的解释见下面
  },
  set(target, key, value, receiver) {
    const dep = getDep(target, key)
    const result = Reflect.set(target, key, value, receiver)
    dep.notify()
    return result
  }
}

function reactive(raw) {
  return new Proxy(raw, reactiveHandlers)
}

const state = reactive({
  count: 0,
});

watchEffect(() => {
  console.log(state.count);
});

state.count++;
```

> 👀 **补充**
>
> 根据 Vue Mastery 《Vue 3 Reactivity》（ 👀 笔记见 [[#《Vue 3 Reactivity》笔记]]）中的说法：使用 receiver 和 Reflect 保证了当操作的对象有继承自其它对象的值或者函数时，this 能够指向正确的目标对象，这将避免一些 使用 Vue2 时出现的响应式警告

上面的实现还有一些边界情况，比如：用户可能在同一个对象中调用 reactive 两次，在 reactive 过的 state 上再次调用 reactive；所以，需要跟踪以确保对同一个对象调用 reactive ，原始对象 ( raw object ) 将会返回相同的代理实例

使用 Proxy 一个非常棒的好处是：Proxy 和它的 handler（ 在这里是 reactiveHandler ） 也可以用于数组。这相当于使用 JS 内置的功能监听数组，而不是在 Vue2 中手动实现、进行处理 ( hack array prototype to override some of the array of the array built-in methods, like push and pop )。👀 具体哪些方法会被重写（劫持）参见 [[#Vue2 监听数组的原理]]

> 👀 来自弹幕：
>
> > Proxy 在数组 push 时会读取数组的 length，同样会触发 get 和 set。
>
> 那么显然，pop、unshift、shift、splice 也会产生类似效果。



##### mini-vue 实现

```html
<div id="app"></div>

<script>
  // vdom
  function h(tag, props, children) {
    return { tag, props, children };
  }

  function mount(vnode, container) {
    const el = (vnode.el = document.createElement(vnode.tag));
    // props
    if (vnode.props) {
      for (const key in vnode.props) {
        const value = vnode.props[key];
        if (key.startsWith("on")) {
          el.addEventListener(key.slice(2).toLowerCase(), value  )
        } else {
          el.setAttribute(key, value);
        }
      }
    }
    // children
    if (vnode.children) {
      if (typeof vnode.children === "string") {
        el.textContent = vnode.children;
      } else {
        vnode.children.forEach((child) => {
          mount(child, el);
        });
      }
    }
    container.appendChild(el);
  }

  function patch(n1, n2) {
    if (n1.tag == n2.tag) {
      const el = (n2.el = n1.el);

      // props
      const oldProps = n1.props || {};
      const newProps = n2.props || {};
      for (const key in newProps) {
        const oldValue = oldProps[key];
        const newValue = newProps[key];
        if (newValue !== oldValue) {
          el.setAttribute(key, newValue);
        }
      }
      for (const key in oldProps) {
        if (!(key in newProps)) {
          el.removeAttribute(key);
        }
      }

      // children
      const oldChildren = n1.children;
      const newChildren = n2.children;

      if (typeof newChildren === "string") {
        if (typeof oldChildren === "string") {
          if (newChildren !== oldChildren) {
            el.textContent = newChildren;
          }
        } else {
          el.textContent = newChildren;
        }
      } else {
        if (typeof oldChildren === "string") {
          el.innerHTML = "";
          newChildren.forEach((child) => {
            mount(child, el);
          });
        } else {
          const commonLength = Math.min(oldChildren.length, newChildren.length);
          for (let i = 0; i < commonLength; i++) {
            patch(oldChildren[i], newChildren[i]);
          }
          if (newChildren.length > oldChildren.length) {
            newChildren.slice(oldChildren.length).forEach((child) => {
              mount(child, el);
            });
          } else if (newChildren.length < oldChildren.length) {
            oldChildren.slice(newChildren.length).forEach((child) => {
              el.removeChild(child.el);
            });
          }
        }
      }
    } else {
      // replace, 略
    }
  }

  // reactivity
  let activeEffect; // 保存添加哪一个函数作为订阅

  class Dep {
    subscribers = new Set(); // 订阅者
    depend() {
      if (activeEffect) {
        this.subscribers.add(activeEffect);
      }
    }
    notify() {
      // 通知订阅者
      this.subscribers.forEach((effect) => {
        effect();
      });
    }
  }

  function watchEffect(effect) {
    activeEffect = effect;
    effect();
    activeEffect = null;
  }

  // 因为 reactiveHandler 只创建一次，为了在运行时找到同一个那个 dep 实例，所以使用全局 weakMap 来缓存 dep 实例，并保持唯一性
  // 另外，因为 weakMap 的 key 必须是对象，并且当 key 不可达，对应的 value 可以自动触发 GC。正因为此，weakMap 的 key 不可枚举
  const targetMap = new WeakMap();

  function getDep(target, key) {
    let depsMap = targetMap.get(target);
    if (!depsMap) {
      depsMap = new Map();
      targetMap.set(target, depsMap);
    }
    let dep = depsMap.get(key);
    if (!dep) {
      dep = new Dep();
      depsMap.set(key, dep);
    }
    return dep;
  }

  // reactiveHandler 拆出来是为了只创建一次，避免重新创建
  const reactiveHandlers = {
    get(target, key, receiver) {
      const dep = getDep(target, key);

      dep.depend();
      return Reflect.get(target, key, receiver);
      // 虽然也可以 return target[key]。但考虑到原型继承问题，在这种情况下，receiver 和 target 会指向不同的东西。总之，用 Reflect 让一切正常
    },
    set(target, key, value, receiver) {
      const dep = getDep(target, key);
      const result = Reflect.set(target, key, value, receiver);
      dep.notify();
      return result;
    },
  };

  function reactive(raw) {
    return new Proxy(raw, reactiveHandlers);
  }

  const App = {
    data: reactive({ count: 0 }),
    render() {
      return h(
        "div",
        {
          onClick: () => this.data.count++, 
          // 当前的代码并不能处理事件侦听器，上面 mount 的 props 部分需要修改，加上 addEventListener
        },
        String(this.data.count)
      );
    },
  };

  function mountApp(component, container) {
    let isMounted = false;
    let prevVdom = component;
    watchEffect(() => {
      if (!isMounted) {
        prevVdom = component.render();
        mount(prevVdom, container);
        isMounted = true;
      } else {
        const newVdom = component.render();
        patch(prevVdom, newVdom);
        prevVdom = newVdom;
      }
    });
  }

  mountApp(App, document.getElementById("app"));
</script>
```



##### Compostion API

> **Compostion API = Reactivity API + Lifecycle hooks**

Ref 就像是一个 “拥有内部值的容器，同时能够追踪它的依赖关系”，这和 dep 实例很像。

Setup 会是新的第一个被调用的 hook ，甚至在 beforeCreate 之前。👀 结合下面的摘抄，感觉类似于 main 函数之于 c-like 语言的项目

> `setup()` 钩子是在组件中使用组合式 API 的入口
>
> 摘自：[Vue3 Doc - API - 组合式 API：setup()](https://cn.vuejs.org/api/composition-api-setup.html)

因为大部分暴露在 this 上面的功能，使用 Compostion API 的函数 同样可以做到；所以 setup 内部并没有太多 this 的用例 ( use case )。

另外，<font color=fuchsia>**因为 setup 是在其他的所有 options**</font> ( 👀 Options API ) <font color=fuchsia>**被处理之前调用的；比如 data 和 computed ，它们都是在 setup 之后处理的，所以不能在 setup 中使用它们**</font>。所以，在同时使用这两者时，知道它们谁先被处理很重要。

一般的经验时：在 setup 内部，假设不会知道其他 Options 的内容；它有它自己的世界。但是，<font color=fuchsia>**当所有东西都从 setup 中返回，它们将在其他 Options 中可用**</font>



@vue/reactivity 是一个内部包，Vue 设计者只是碰巧暴露了它的一些 API，通过 Vue 接口 ( interface )；而一些 @vue/reactivity 的 API 被认为是底层或者进阶的 API；Vue 设计者甚至不通过 Vue 将其暴露。从技术上来讲，如果你是 Vue 的超级进阶用户，可以单独使用 响应式包，在它的基础上建立一个替代系统；但这不是 API 合同 ( API contract ) 的一部分。

对于 Vue 来说，watchEffect 是一个建立在原始的 Effect 上的包装器。当建立了一个 watchEffect（这里我们称为 watcher ），这个 watcher 将自动和组件实例关联；当这个组件实例被卸载时，这个 effect 也会自动停止。



##### how a component re-renders

```js
watchEffect(() => {
  const oldTree = component.vnode
  const newTree = component.render.call(renderContext)
  patch(oldTree, newTree)
})
```



在一个组件中的 setup 函数中，可以调用另一个组件的 setup 函数。这种 组合 ( composition ) 的方式相比传统的面向对象的扩展更为灵活（ 👀 弹幕有人总结：组合优于继承 ）



##### 为何 Compoistion API 的逻辑重用比 mixin 更好

###### 使用 Options + Mixin 实现封装与逻辑重用

```vue
<script src="https://unpkg.com/vue"></script>

<div id="app"></div>

<script>
  const { createApp } = Vue
  const MouseMixin = {
    data() {
      return { x: 0, y: 0 }
    },
    methods: {
      update(e) {
        this.x = e.pageX
        this.y = e.pageY
      }
    },
    mounted() {
      window.addEventListener('mousemove', this.update)
    },
    unmounted() {
      window.removeEventListener('mousemove')
    }
  }

  const App = {
    mixins: [MouseMixin],
    template: `{{x}} {{y}}`,
  }

  createApp(App).mount('#app')
</script>
```

多个 Mixin 在同一个组件中使用，导致的混乱，但并没有一个好的替代品；React 想出来 高阶组件 ( Higher-Order Component / HOC ) 的概念，作为解决方案。HOC 就是：不把所有东西都混在一起，而是需要什么就注入什么。代码如下：

```vue
<script src="https://unpkg.com/vue"></script>

<div id="app"></div>

<script>
  const { createApp, h } = Vue;
  function withMouse(Inner) { // 👀 相当于一个命名空间
    return {
      data() {
        return { x: 0, y: 0 };
      },
      methods: {
        update(e) {
          this.x = e.pageX;
          this.y = e.pageY;
        },
      },
      mounted() {
        window.addEventListener("mousemove", this.update);
      },
      unmounted() {
        window.removeEventListener("mousemove");
      },
      render() {
        return h(Inner, { x: this.x, y: this.y }); // 👀
      },
    };
  }

  const App = withMouse({ // 👀
    props: ["x", "y"],
    template: `{{x}} {{y}}`,
  });

  createApp(App).mount("#app");
</script>
```

不过，HOC 并没有真正解决根本问题：这里有仍然有可能存在命名空间的冲突，比如多个高阶组件相互包装，再加上 props 中也有很多属性（分不清哪个属性来自哪个高阶组件），如下：

```js
const App = withFoo(withBar(withMouse({
  props: ["x", "y", 'foo', 'bar'],
  template: `{{x}} {{y}}`,
})))
```

所以，HOC 也不能完全解决问题 ( not silver bullet )。于是，React 产生了 “渲染 props” 的概念；而在 Vue 生态中，有类似的概念：作用域插槽。写法如下：

```vue
<script src="https://unpkg.com/vue"></script>

<div id="app"></div>

<script>
  const { createApp, h } = Vue;
  const Mouse = {
    data() {
      return { x: 0, y: 0 };
    },
    methods: {
      update(e) {
        this.x = e.pageX;
        this.y = e.pageY;
      },
    },
    mounted() {
      window.addEventListener("mousemove", this.update);
    },
    unmounted() {
      window.removeEventListener("mousemove");
    },
    template: `<slot :x="x" :y="y"  />`,
    render() {
      return (
        this.$slots.default && this.$slots.default({
          x: this.x,
          y: this.y,
        })
      );
    },
  };
  const App = {
    components: { Mouse },
    template: `
      <Mouse v-slot="{ x, y }">
        {{ x }} {{ y }}
      </Mouse>
    `,
  };

  createApp(App).mount("#app");
</script>
```

而如果有多种类型的组件，相较于 “动态 props”：

```js
const App = {
  components: { Mouse, Foo },
  template: `
    <Mouse v-slot="{ x, y }">
      <Foo v-slot="{ foo }">
        {{ x }} {{ y }} {{ foo }}
      </Foo>
    </Mouse>
  `,
};
```

props 的归属关系就很明显了：x，y 属于 Mouse，foo 属于 Foo。

即使出现 相同名称的 prop，也是可以利用 对象的 alias 重命名的。假设 Foo 中也有 名为 x 的 prop ，可以通过如下方法 `{ x: alias }` 避免冲突。如下：

```js
const App = {
  components: { Mouse, Foo },
  template: `
    <Mouse v-slot="{ x, y }">
      <Foo v-slot="{ x: foo }">
        {{ x }} {{ y }} {{ foo }}
      </Foo>
    </Mouse>
  `,
};
```

##### Composition API 实现

```vue
<script src="https://unpkg.com/vue"></script>

<div id="app"></div>

<script>
  const { createApp, ref, onMounted, onUnmounted } = Vue;
  function useMouse() {
    const x = ref(0)
    const y = ref(0)
    const update = e => {
      x.value = e.pageX
      y.value = e.pageY
    }
    onMounted(() => { window.addEventListener('mousemove', update) })
    onUnmounted(() => { window.removeEventListener('mousemove', update) })
    return { x, y }
  }

  const App = {
    setup() {
      const { x, y } = useMouse() // 👀 见下面解释
      return { x, y }
    },
    template: `{{ x }} {{ y }}`,
  };

  createApp(App).mount("#app");
</script>
```

> 👀 **补充**
>
> 虽然上面 `const { x, y } = useMouse(); return { x, y }` 有点啰嗦，甚至可以直接写成
>
> ```js
> return { ...useMouse() }
> ```
>
> 但是这样就会造成黑箱化、表达不明确，导致类似 mixin 一样的使用（也会让阅读代码的人不得不去调用定义处查看代码的返回内容）。同样的，如果有变量冲突，同样可以使用别名
>
> ```js
> const { foo: bar } = useFoo()
> ```



###### 使用 Composition API 最后一个好处

当你尝试将多个东西合并在一起的时候，在类型系统中很难正确的进行类型推导是很困难的；但是在 Composition API 中，一切都是函数调用，Vue 也提供了开箱即用正确的类型定义。在 90% 的情况下，一起都是自动推断出来的。当你（在 setup 中）调用 （封装的函数）时，所有的东西都会有类型，所以返回的（暴露）的数据将会返回到模版时，IDE、vetur 插件将可以接收到它们，然后推断模版进行自动补全。



##### 使用 fetch 代码示例

```vue
<script src="https://unpkg.com/vue"></script>

<div id="app"></div>

<script>
  const { createApp, ref, watchEffect } = Vue;

  function usePost(getId) {
    return useFetch(() => `https://jsonplaceholder.typicode.com/todos/${getId()}`)
  }

  function useFetch(getUrl) { // ⚠️ getUrl 是一个函数
    const data = ref(null);
    const error = ref(null);
    const isPending = ref(true);

    watchEffect(() => { // watchEffect 会监听 props.id，一旦 id 发生变化，将会重新调用函数
      // watchEffect 监听到变化，调用 fetch 前，重置数据，
      isPending.value = true
      data.value = null
      error.value = null

      fetch(getUrl())
        .then((res) => res.json())
        .then((_data) => {
          data.value = _data;
          isPending.value = false;
        })
        .catch((err) => {
          error.value = err;
          isPending.value = false;
        });
    });

    return {
      // 👀 返回一个包含 ref 的对象时候
      data,
      error,
      isPending,
    };
  }

  const Post = {
    template: `
      <div v-if="isPending">loading...</div>
      <div v-else-if="data">{{ data }}</div>
      <div v-else-if="error">Something went wrong: {{ error.message }}</div>
    `,
    props: ['id'],
    setup(props) {
      // 👀 使用者使用解构
      const { data, error, isPending } = usePost(() => props.id)

      return {
        data,
        error,
        isPending,
      };
    },
  };

  const App = {
    components: { Post },
    data() {
      return {
        id: 1
      }
    },
    template: `
      <button @click="id++">change ID</button>
      <Post :id="id" />
    `
  }

  createApp(App).mount("#app");
</script>
```

###### ref vs reactive

使用 Composition API 时，尤雨溪个人倾向于使用 ref 。

<font color=fuchsia>当返回 ( return ) 一个包含 ref 的对象时</font>（ 👀 参见 [[#使用 fetch 代码示例]] ），<font color=fuchsia>允许使用者使用解构，解构之后数据依然有响应式</font>（ 👀 参见[[#使用 fetch 代码示例]] ）。但如果使用 reactive，当你返回 reactive 包裹的对象时，使用者无法在解构之后，仍保持响应式；因为当它们被解构，这些值将会变成原始值 ( primitive value )，不会和 Composition API 函数中的状态逻辑保持关联。解决方法是使用 toRefs ，示例如下；但是话说回来，与其再使用 toRefs 不如一开始使用 ref。

<font color=dodgerBlue>使用 ref 的另一个好的副作用 ( nice side effect ) 是</font>：它可以让你更简单的移动这些逻辑代码，重新组织它们（解耦 & 可重用性强）；相反，如果将所有东西都放入一个巨大的 data 对象中，想要重新组织数据就会变的很麻烦。

```js
const state = reactive({})
return toRefs(state) 
```

> 👀 关于 ref 和 reactive 区别相关内容，可以参考 [[Vue3 + TS 学习笔记#antfu Vue conf 2021 演讲]]



### 《Vue 3 Reactivity》笔记

> 🔗 https://www.bilibili.com/video/BV1SZ4y1x7a9



<img src="https://s2.loli.net/2022/11/23/G6U89sMVCr7blDa.jpg" style="zoom: 33%;" />

track 函数保存的是 effect，即上图所说的 Save this code。trigger 函数调用 effect，以及其他所有已经保存了的代码。

为了存储 effect<font color=red size=4>**s**</font> ，需要使用 dep 变量，表示依赖关系，它的数据结构是一个 Set（可避免重复）。在 track 函数中，使用 `dep.add(effect)` 来保存 effect 。

<img src="https://s2.loli.net/2022/11/23/5qeBxnjhzTDHUai.png" alt="image-20221123235212889" style="zoom:33%;" />

通常，对象会有多个属性，每一个属性都需要有自己的 dep（依赖关系），或者说 effect 的 Set 。那么，如何存储，或者说：让每个属性都拥有（自己的）依赖？     

dep 其实就是一个 effect Set，这个 <font color=fuchsia>**effect Set 应该在 值发生改变时 重新运行**</font>。

<img src="https://s2.loli.net/2022/11/24/PQ1Jrlv49US8CfD.jpg" alt="20221124_000713.jpeg" style="zoom:45%;" />

Set 中的每一个值，都只是一个需要执行的 effect；就如上面的匿名函数  `() => { total = product.price * product.quantity}`

而要把这些 deps 存储起来，并且<font color=fuchsia size=4>**方便以后再找到它们** </font>，需要创建一个 <font color=fuchsia size=4>**deps map**</font>（数据结构是 ES6 Map）；它是一张存储了每一个属性及其 dep 对象的 字典。结构如下图所示：

<img src="https://s2.loli.net/2022/11/24/fnCYwBA5apyhkde.jpg" alt="20221124_003042.jpeg" style="zoom: 40%;" />

###### track 和 trigger 实现

<img src="https://s2.loli.net/2022/11/24/H9qOvympxcC7WrD.jpg" alt="20221124_004115.jpeg" style="zoom:33%;" />

###### 具体调用结果

<img src="https://s2.loli.net/2022/11/24/2k3HijUndpZXfgD.jpg" alt="20221124_004330.jpeg" style="zoom:30%;" />

现在我们有了 对不同的属性有了一种跟踪依赖关系的方法，但<font color=dodgerBlue>如果有多个响应式对象呢</font>？<font color=dodgerBlue>甚至其中一个对象的某个属性指向的是另一个响应式对象呢</font>？

存储每个“响应式对象属性”相关联的依赖，在 Vue3 中，被称为 “ TargetMap ”。如下图：

![20221124_150709.jpeg](https://s2.loli.net/2022/11/24/EO5pATJbn9sFrfh.jpg)

TargetMap 的类型是 WeakMap（ 👀 为什么选择 WeakMap ，[[#reactive 实现#Vue3 风格实现]] 中有说）。那么，现在 TargetMap 的结构就是 `WeakMap<any, Map<any, Set>>` 。

> 👀 除了上面的 TargetMap，另外值得注意的是： `depsMap` ，以及 dep 指向的 “Effect to re-run”

<font color=dodgerBlue>看了下 Vue3 源码也确实如此：</font>  

<img src="https://s2.loli.net/2022/11/24/4ScDHJTghuGnIor.png" alt="image-20221124143602125" style="zoom:50%;" />

根据 L18～L19  ，targetMap 的结构是 `WeakMap<any, Map<any, Dep>>` ；而根据 L6 中 deps 的来源 `dep.ts` ：

<img src="https://s2.loli.net/2022/11/24/Ow7FQHlmSoruq4k.png" alt="image-20221124144053871" style="zoom:50%;" />

根据 L3 ，发现 Dep 的结构是 `Set<ReactiveEffect> & TrackedMarkers` ，所以 TargetMap 的结构可以简单理解为 `WeakMap<any, Map<any, Set<ReactiveEffect>>`

###### 使用 TargetMap 的 track 和 trigger 的实现

```js
const targetMap = new WeakMap(); // For storing the dependencies for each reactive object

function track(target, key) {
  let depsMap = targetMap.get(target); // Get the current depsMap for this target (relative object)
  if (!depsMap) {
    targetMap.set(target, (depsMap = new Map())); // If it doesn't exist, create it
  }
  let dep = depsMap.get(key); // Get the dependency object for this property
  if (!dep) {
    depsMap.set(key, (dep = new Set())); // If it doesn't exist, create it
  }
  dep.add(effect); // Add the effect to the dependency
}

function trigger(target, key) {
  const depsMap = targetMap.get(target); // Does this object have any properties that have dependencies
  if (!depsMap) return; // If no, return from the function immediately
  let dep = depsMap.get(key); // Otherwise, check if this property has a dependency
  if (dep) {
    dep.forEach((effect) => effect()); // Run those
  }
}
```

现在 track 和 trigger 确实是实现了，但还是需要像 [[#具体调用结果]] 一样，手动调用 track 和 trigger，没有办法让 effect 重新运行（自动进行 “依赖收集” 和 “派发更新” ）。这需要在 getter / setter 中收集和派发，这就需要使用 Proxy 的 handler 和 Reflect 。



##### 使用 Reflect 和 handler

代理是另一个对象的占位符，默认情况下对该对象进行委托 ( Proxy is a placeholder for another object , which by default delegates to that object)。如下图：在访问 `proxiedProduct.quantity` 时，<font color=fuchsia>会先调用 proxy，然后再调用 `product` ，<font size=4>**之后再返回 proxy**</font></font> ⚠️ 前面的调用顺序是之前所不知道的。

![image-20221205214516621](https://s2.loli.net/2022/12/05/E7MovZ68dimlT5p.png)

> 👀 注：除了使用 `obj.prop` 和 `obj['prop']` 访问对象的属性，<font color=fuchsia>**还可以使用 `Reflect.get('prop')`**</font> ，这是之前没有想到的。

###### 代码实现

> 👀 这里 track、trigger 函数省略，见上面 [[#track 和 trigger 实现]]

```js
function reactive(target) {
  const handler = {
    get(target, key, receiver) {
      let result = Reflect.get(target, key, receiver)
      track(target, key)
      return result
    },
    set(target, key, value, receiver) {
      let oldValue = target[key]
      let result = Reflect.set(target, key, value, receiver)
      // ⚠️ 不同于 Reflect.get，**Reflect.set 返回值是 bool**，所以不能将 oldValue !== result 拿来判断
      if(oldValue !== value) { // 👀
        trigger(target, key)
      }
      return result
    }
  }
  return new Proxy(target, handler)
}

let product = reactive({ price: 5, quantity: 2 })
let total = 0
let effect = () => {
  total = product.price * product.quantity
}

effect()
console.log('total',total)

product.quantity = 3
console.log('total',total)
```

###### 示例图示与变化

利用 Proxy 和 Reflect 实现了自动的 track 和 trigger，[[#具体调用结果]] 中的手动 track 和 trigger ，可以删掉了。下面是：示例代码中 运行结果 与对应的依赖收集的结构示意图，depsMap 中的 price 和 quantity 也分别指向一个内部结构为 Set 的 Dep 。

![image-20221205221733117](https://s2.loli.net/2022/12/05/S7e2GLxzbAoh81n.png)



##### activeEffect

现在的代码是：每次访问响应式属性都会调用 track 函数，也就会多次收集同一个依赖。这显然是不必要的，也是不被希望的；我们的预期是：<font color=fuchsia>只在 effect 中调用 track 函数</font>，这就需要用到变量 activeEffect ，<font color=fuchsia>它表示的是 “**正在运行的 effect** ”</font>，这也是 Vue3 中解决该问题的方法

```js
let activeEffect = null
// ...

function track(target, key) {
  if (activeEffect) { // ⚠️
    let depsMap = targetMap.get(target)
    if (!depsMap) {
      targetMap.set(target, (depsMap = new Map()))
    }
    let dep = depsMap.get(key)
    if (!dep) {
      depsMap.set(key, (dep = new Set()))
    }
    dep.add(activeEffect) // ⚠️
  }
}

function effect(eff) { // 👀
  activeEffect = eff   // Set this as the activeEffect
  activeEffect()       // Run it
  activeEffect = null  // Unset it
}

let product = reactive({ price: 5, quantity: 2 })
let total = 0
effect(() => { total = product.price * product.quantity } ) // 这里之前的代码是 let total = () => { product.price * product.quantity } ，这里使用 effect 将其包裹。也可以删掉下面一行代码了。
// effect() // 由于上面 effect 的存在，至此，也就不需要手动调用 effect() 了
```



##### Ref

Vue3 通过对象访问器 Object Accessors（即 getter / setter ）实现 `Ref` ，如下示例：

```js
let user = {
  firstName: 'Gregg',
  lastName: 'Pollack',

  get fullName() {
    return `${this.firstName} ${this.lastName}`
  },
  set fullName(value) {
    [this.firstName, this.lastName] = value.split(' ')
  }
}

console.log(`Name is ${user.fullName}`)
user.fullName = 'Adam Jahr'
console.log(`Name is ${user.fullName}`)
```



###### Ref 实现

```js
function ref(raw) {
  const r = {
    get value() {
      track(r, 'value')
      return raw
    },
    set value(newVal) {
      if (newVal !== raw) { // ⚠️
        raw = newVal
        trigger(r, 'value')
      }
    }
    return r
  }
}
```

以上，实际就是 Vue3 中 `refImpl` 的实现原理；不过，Vue3 实际实现要复杂不少，比如 `refImpl` 是一个 class ，也有自己的构造函数...



##### Computed 实现

###### 原理与思路

> 👀 当然，下面所说的 computed 实现，还是相当简化的：无论是计算属性中可以放入 getter 方法，也可以放入包含 getter setter 的对象；还是 computed 的值的缓存。

首先，computed 期望接收一个 getter（ 👀 也可见下面官方文档的引用），所以形如：

```js
function computed(getter) {
  // TODO
}
```

> `computed()` 方法期望接收一个 getter 方法，返回值为一个**计算属性 ref** 。
>
> 计算属性默认是只读的（ 👀 即默认/期望只接受一个 getter 方法）。
>
> 摘自：[Vue3 Doc - 计算属性 # 基础示例](https://cn.vuejs.org/guide/essentials/computed.html#basic-example)
>
> 👀 另外，<font color=dodgerBlue>**容易遗忘的是**</font>：computed 的返回值是一个 ref ，所以：和 ref 一样，computed 需要通过 `.value` 来访问计算结果；而在模板中，computed 会被自动解包，

这就需要在 `// TODO` 中添加了逻辑：

1. 创建一个响应式引用，称之为 `result` ( Create a reactive reference called `result` )
2. 在 `effect()` 中 运行 `getter` ，因为需要监听响应值（ 👀 即：收集依赖），然后将其 ( getter ) 赋值为 `result.value` 。 ( Run the `getter` in an `effect()` which sets the `result.value` )
3. 返回 `result` ( Return the `result` )

###### 代码实现

```js
function computed(getter) {
  let result = ref()
  
  effect( () => ( result.value = getter() ) )
  
  return result
}
```



###### Vue 2 的局限

Vue2 中，Get 和 Set 的钩子，是添加在（响应式对象的）各个属性下的；所以，对 `reactiveObj`  直接添加（不使用 `Vue.set(obj, prop, val)` ）一个属性，这个属性是不会得到响应式的；而 Vue3 因为使用了 Proxy，不会出现类似的问题，属性将自动变成响应式。



##### Vue3 响应式实现文件结构

实现响应式的代码，在 packages/reactivity/src/ 目录下；作用包含如下：

- effect.ts ：定义 `effect` ，`track` 和 `trigger`
- baseHandlers.ts ：定义 proxy handlers ( `get` ，`set` 等 )
- reactive.ts ：定义 `reactive` ，其创建了一个 ES6 Proxy
- ref.ts ：定义了响应式的引用，使用对象访问器。

###### 文件间的关系

![image-20221206224917968](https://s2.loli.net/2022/12/06/goDxz1hPRml2bXf.png)



##### 尤雨溪 Vue3 原理 Q&A

###### depend notify v.s. track trigger

![image-20221207000256378](https://s2.loli.net/2022/12/07/3GKsvgwDL7cAkiU.png)

在 Vue2 中使用的是 depend 和 notify ，在 Vue3 中改成了 track 和 trigger ；而这两对函数，在做同样的事情。

depend 和 notify 都是动词，和所有者 Dep（一个依赖实例）相关，可以说：一个依赖实例被依赖 ( depend )，或者它正在通知它的订阅者 ( sub )；而 Vue3 对依赖关系做了改变。从技术角度来说，Vue3 已经没有 Dep 类了，depend 和 notify 中的逻辑也被抽离到两个独立的函数 track 和 trigger 中。所以，当调用 track 和 trigger 时，它们的行为更像是在追踪什么，而不是什么被依赖。

![image-20221207000359984](https://s2.loli.net/2022/12/07/x9nIZ32VmKWSt7q.png)

<font color=dodgerBlue>在 Vue2 中有一个独立的 Dep 类，而在 Vue3 中，只有一个 Set ；为什么会有这样的改变？</font>

看下上图中的代码，Dep 类中的 `subscribers` 数组其实就是一个 Set ，和 Vue3 的 dep 的 Set 其实是一样的。而由于 Vue3 实现逻辑的改变，depend 和 notify 被抽离成了 track 和 trigger， Dep 类中只剩下了 `subscribers` Set 了，无论从必要性还是<font color=red>**性能**</font>而言，都没有必要再实现一个 Dep 类。



###### TargetMap<depsMap\<dep>> 结构

Vue2 中依赖收集过程中，包含在 forEach 内的 `Object.defineProperty()` 中的 getter 中形成了闭包（ 👀 [[#Vue2 风格实现]] 中相关代码有注释标记），闭包为属性存储关联的 Dep 。而 Vue3 是使用了 Proxy 和 Handler，<font color=fuchsia>Handler **直接接收** target</font> （👀 指的是代理的对象）和 key (👀 指的是代理对象的键)，无法创建闭包，为每个属性存储关联的依赖项。那么，如何给定目标对象以及该对象上的键，为键找到相应的实例？便需要将它们放入两个层级的嵌套 map ( given a target object and a key on that object, how do we manage to always locate the same dependency instance. The only way to do that put them into two level of nested maps ) 。



###### ref v.s reactive

![image-20221207215835463](https://s2.loli.net/2022/12/07/eqczmnwBKTG74bh.png)

如上图：<font color=dodgerBlue>可以通过 `return reactive({ value: initialValue })` 来 “实现一个” ref ，为什么不应这样做？</font>

<font color=fuchsia>ref 根据定义应该只暴露一个属性</font>，就是值 ( value ) 本身。但是，从技术层面，用响应式可以给 ref 变量附加一些新的属性，但这就违背了 ref 的目的。<font color=fuchsia>**ref 只能为包装一个内部值而服务，不应该被当作一个一般的响应式对象**</font>；另外，Vue 也实现了 isRef 检查，ref 对象实际上有一些特殊的东西（ 👀 看了下源码是 `__v_isRef` ，定义是 `public readonly __v_isRef = true` ），让 用户 / Vue 知道这实际上是一个 ref ，而不是一个 reactive 对象。最后，ref 在性能方面也是更好的。当创建一个 reactive 对象时，需要检查是否依旧有相应的 reactive 副本 ( already has a corresponding on the reactive copy of it ) ，是否有一个只读副本；总之，有很多额外工作要做。而如果仅仅需要使用一个对象字面量，使用 ref 更加节省性能。

###### 使用 Proxy 和 Reflect 除响应式之外的好处

直接说答案：Proxy 实现了类似“懒加载”的好处。

在 Vue2 中，当进行（响应式）转换时，Vue 必须尽快的完成（全部）转换，因为当 对象被传递给 Vue2 的响应式（系统）时，Vue 必须要遍历所有的键，并当场、立即 ( on the spot ) 转换；以便之后被访问时，它们已经被转换（为响应式的）了。

而在 Vue3 中，当对一个对象调用 reactive 时，Vue3 所做的就是返回一个代理对象；<font color=fuchsia>仅在需要时转换嵌套对象，即当访问它们时</font> ( 👀 这里很明显的说明了 按需和类似“懒加载” 的特性）。如果应用中有一个很大的对象列表，但，在分页的情况下，只需要渲染前十个，那么只有前十个会进行响应式处理。



#### 尤雨溪源码解读

> 👀 注：由于视频录制时还是 Vue3 早期版本，所以在做笔记时，会按照当前（2022/12）的 Vue3 实现去阅读与笔记。

##### reactivity/baseHandlers.ts

###### createGetter 函数

createGetter 中有 readonly 和 shallow 的判断，readonly 只允许创建只读的响应式对象，它可以被读取和追踪，但不能被改变；而 shallow 意味着：当你把一个对象放入另一个对象作为嵌套属性时，是不会将它转换为响应式的（ 👀 实现 shallowRef 和 shallowReactive ）。

在  createGetter 中使用了 `ArrayInstrumentations` ，这是一个处理边缘情况的 “数组检测器”

```ts
function createGetter(isReadonly = false, shallow = false) {
  return function get(target: Target, key: string | symbol, receiver: object) {
    if (key === ReactiveFlags.IS_REACTIVE) {
      return !isReadonly
    } else if (key === ReactiveFlags.IS_READONLY) {
      return isReadonly
    } else if (key === ReactiveFlags.IS_SHALLOW) {
      return shallow
    } else if (
      key === ReactiveFlags.RAW &&
      receiver ===
        (isReadonly
          ? shallow
            ? shallowReadonlyMap
            : readonlyMap
          : shallow
          ? shallowReactiveMap
          : reactiveMap
        ).get(target)
    ) {
      return target
    }

    const targetIsArray = isArray(target)

    if (!isReadonly) {
      if (targetIsArray && hasOwn(arrayInstrumentations, key)) { // arrayInstrumentations 处理边缘情况
        return Reflect.get(arrayInstrumentations, key, receiver)
      }
      if (key === 'hasOwnProperty') {
        return hasOwnProperty
      }
    }

    const res = Reflect.get(target, key, receiver)

    if (isSymbol(key) ? builtInSymbols.has(key) : isNonTrackableKeys(key)) { // 经测试：builtInSymbols 是为了拿到 JS 13个内置 symbol
      return res
    }

    if (!isReadonly) {
      track(target, TrackOpTypes.GET, key) // 👀 可写的属性进行 track。另外，这里的 GET，下面还有 ADD、SET 和 CLEAR
    }

    if (shallow) { // shallow 的处理
      return res
    }

    if (isRef(res)) { // ref 的处理，如果 target 不是一个数组且 key 不是整型，则自动对 ref 拆箱
      // ref unwrapping - skip unwrap for Array + integer key.
      return targetIsArray && isIntegerKey(key) ? res : res.value
    }

    if (isObject(res)) {
      // Convert returned value into a proxy as well. we do the isObject check
      // here to avoid invalid value warning. Also need to lazy access readonly
      // and reactive here to avoid circular dependency.
      return isReadonly ? readonly(res) : reactive(res)
    }

    return res
  }
}
```

###### createSetter 函数

```ts
function createSetter(shallow = false) {
  return function set(
    target: object,
    key: string | symbol,
    value: unknown,
    receiver: object
  ): boolean {
    let oldValue = (target as any)[key]
    if (isReadonly(oldValue) && isRef(oldValue) && !isRef(value)) {
      return false
    }
    if (!shallow) {
      if (!isShallow(value) && !isReadonly(value)) {
        oldValue = toRaw(oldValue)
        value = toRaw(value)
      }
      if (!isArray(target) && isRef(oldValue) && !isRef(value)) { // 检查 set 属性，属性是否是 ref
        oldValue.value = value
        return true
      }
    } else {
      // in shallow mode, objects are set as-is regardless of reactive or not
    }

    const hadKey =
      isArray(target) && isIntegerKey(key)
        ? Number(key) < target.length
        : hasOwn(target, key)
    const result = Reflect.set(target, key, value, receiver)
    // don't trigger if target is something up in the prototype chain of original
    if (target === toRaw(receiver)) {
      if (!hadKey) { // key 不存在，则添加 ( ADD )
        trigger(target, TriggerOpTypes.ADD, key, value)
      } else if (hasChanged(value, oldValue)) { // key 存在，则修改 ( SET )
        trigger(target, TriggerOpTypes.SET, key, value, oldValue)
      } // 虽然上面所说的 ADD 和 SET，但是对于 proxy 而言，没有区别，都是同样的 proxy trap
    }
    return result
  }
}
```

##### reactivity/effect.ts

###### track 函数

> 👀 关于 track 函数，可以看下上面的讲解与实现

```ts
export function track(target: object, type: TrackOpTypes, key: unknown) {
  if (shouldTrack && activeEffect) { // shouldTrack 是内部标志，作为检查条件
    let depsMap = targetMap.get(target)
    if (!depsMap) {
      targetMap.set(target, (depsMap = new Map()))
    }
    let dep = depsMap.get(key)
    if (!dep) {
      depsMap.set(key, (dep = createDep()))
    }

    const eventInfo = __DEV__
      ? { effect: activeEffect, target, type, key }
      : undefined

    trackEffects(dep, eventInfo)
  }
}

export function trackEffects(
  dep: Dep,
  debuggerEventExtraInfo?: DebuggerEventExtraInfo
) {
  let shouldTrack = false
  if (effectTrackDepth <= maxMarkerBits) {
    if (!newTracked(dep)) {
      dep.n |= trackOpBit // set newly tracked
      shouldTrack = !wasTracked(dep)
    }
  } else {
    // Full cleanup mode.
    shouldTrack = !dep.has(activeEffect!)
  }

  if (shouldTrack) {
    dep.add(activeEffect!)
    activeEffect!.deps.push(dep) // ⚠️ 注意这里：activeEffect 是一个列表，也可以有自己的 deps 集合。这表示一种双向关系，在 dep 和 effect 之间；即：effect 可以有多个 dep，dep 作为订阅者可以有多个 effect；也就是 many to many 的关系。之所以是这样的关系，是因为需要追踪以便做清理工作(cleanupEffect)
    if (__DEV__ && activeEffect!.onTrack) {
      activeEffect!.onTrack({
        effect: activeEffect!,
        ...debuggerEventExtraInfo!
      })
    }
  }
}
```

###### trigger 函数

```ts
export function trigger(
  target: object,
  type: TriggerOpTypes,
  key?: unknown,
  newValue?: unknown,
  oldValue?: unknown,
  oldTarget?: Map<unknown, unknown> | Set<unknown>
) {
  const depsMap = targetMap.get(target)
  if (!depsMap) {
    // never been tracked
    return
  }

  let deps: (Dep | undefined)[] = []
  if (type === TriggerOpTypes.CLEAR) { // 当清除集合时，必须要触发所有与之相关的 effects
    // collection being cleared
    // trigger all effects for target
    deps = [...depsMap.values()]
  } else if (key === 'length' && isArray(target)) {
    const newLength = Number(newValue)
    depsMap.forEach((dep, key) => {
      if (key === 'length' || key >= newLength) {
        deps.push(dep)
      }
    })
  } else {
    // schedule runs for SET | ADD | DELETE
    if (key !== void 0) {
      deps.push(depsMap.get(key))
    }

    // also run for iteration key on ADD | DELETE | Map.SET
    switch (type) {
      case TriggerOpTypes.ADD:
        if (!isArray(target)) {
          deps.push(depsMap.get(ITERATE_KEY))
          if (isMap(target)) {
            deps.push(depsMap.get(MAP_KEY_ITERATE_KEY))
          }
        } else if (isIntegerKey(key)) {
          // new index added to array -> length changes
          deps.push(depsMap.get('length'))
        }
        break
      case TriggerOpTypes.DELETE:
        if (!isArray(target)) {
          deps.push(depsMap.get(ITERATE_KEY))
          if (isMap(target)) {
            deps.push(depsMap.get(MAP_KEY_ITERATE_KEY))
          }
        }
        break
      case TriggerOpTypes.SET:
        if (isMap(target)) {
          deps.push(depsMap.get(ITERATE_KEY))
        }
        break
    }
  }

  const eventInfo = __DEV__
    ? { target, type, key, newValue, oldValue, oldTarget }
    : undefined

  if (deps.length === 1) {
    if (deps[0]) {
      if (__DEV__) {
        triggerEffects(deps[0], eventInfo)
      } else {
        triggerEffects(deps[0])
      }
    }
  } else {
    const effects: ReactiveEffect[] = []
    for (const dep of deps) {
      if (dep) {
        effects.push(...dep)
      }
    }
    if (__DEV__) {
      triggerEffects(createDep(effects), eventInfo)
    } else {
      triggerEffects(createDep(effects))
    }
  }
}
```



值得注意的是：视频中的 scheduleRun 方法改名为 triggerEffect 

onTrack 和 onTrigger 就是对应 track 和 trigger，另外，onTrack 和 onTrigger 包含的参数 e，包含了 Vue 使用 Proxy 运行时的一些过程细节；比如 GET / SET / ADD 操作类型，以及 SET 过程中的 oldvalue 和 newvalue （ 👀 见视频最后的 debugging 演示）。此外，还提到了 Dev 阶段专用的 renderTracked 和 renderTriggered 生命周期。



### Vue 实例生命周期



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



#### 《Vue 响应式原理解析》笔记

##### Dep

```javascript
var Dep = function Dep() {
  this.id = uid++
  this.subs = []
}
```

Dep 的含义，自然就是 dependency（也就是**依赖**，一个计算机领域的名词）。

就像编写 node.js 程序，常会使用 npm 仓库的依赖。<font color=red>在 Vue 中，依赖具体指的是**经过响应式处理的数据**</font>。后面会提到，<font color=red>响应式处理的关键函数之一是</font>在很多 Vue 原理文章都会提到的 <font color=red>`defineReactive`</font> 。

<font color=fuchsia>Dep 与每个响应式数据绑定后，该响应式数据就会成为一个依赖</font>（名词），下面介绍 Watcher 时会提到，<font color=red>**响应式数据 可能被 watch、computed、渲染模板 3 种函数依赖**</font>（动词）。

###### subs

Dep 对象下有一个 subs 属性，是一个数组，是 subscriber（订阅者）列表的意思。<font color=fuchsia>订阅者可能是 watch 函数、computed 函数、视图更新函数</font>（ 👀 render 函数？）。

##### Watcher

<font color=fuchsia>Watcher 是 Dep 里提到的**订阅者**</font> subs（不要和后面的 Observer 观察者搞混）。

因为 <font color=fuchsia>Watcher 的功能在于及时响应 Dep 的更新</font>，就像一些 App 的订阅推送，<font color=red>你 ( Watcher ) 订阅了某些资讯 ( Dep )，资讯更新时会提醒你阅读</font>。

###### deps

与 Dep 拥有 subs 属性类似，<font color=red>Watcher 对象也有 deps 属性</font>。这样构成了 <font color=fuchsia>**Watcher 和 Dep** 就是一个 <font size=4>**多对多的关系**</font>，互相记录的原因是 <font size=4>**当一方被清除的时候可以及时更新相关对象**</font></font>。

###### Watcher 如何产生

上面多次提到的 watch、computed、渲染模板产生 Watcher，在 Vue 源码里都有简明易懂的体现：

- `mountComponent` 的 `vm._watcher = new Watcher(vm, updateComponent, noop);`

  > 👀 见 https://github.com/vuejs/vue/blob/main/src/core/instance/lifecycle.ts#L219

- `initComputed` 的 `watchers[key] = new Watcher(vm, getter || noop, noop, computedWatcherOptions)`

  > 👀 见 https://github.com/vuejs/vue/blob/main/src/core/instance/state.ts#L192

- `$watcher` 的 `var watcher = new Watcher(vm, expOrFn, cb, options);`

  > 👀 当前 Vue2.7 没有找到 `$watcher` 相关定义，看定义应该是 https://github.com/vuejs/vue/blob/main/src/core/instance/state.ts#L376

##### Observer

Observer 是观察者，它 <font color=red>**负责递归地观察（或者说是处理）响应式对象（或数组）**</font>。在打印出的实例里，可以注意到<font color=fuchsia>**响应式的对象都会带着一个 `__ob__` ，这是已经被观察的证明**</font>。观察者没有上面的 Dep 和 Watcher 重要，稍微了解下就可以了。

> 👀 看了下 [Vue2 Observer 的主要逻辑]()，就是使用 Object.defineProperty 设置 getter / setter ，并在其中分别调用 depend 和 notify 。搜了下 Vue3 的代码 Observer 部分已经不存在了，相关功能应该在 `reactive.ts` 和 `ref.ts` 中已经通过调用 `baseHandlers` 和 `collectionHandlers` 中定义的 Proxy handler 实现了。
>
> 另外，根据 [Vue3 源码解析 06上篇--响应式 baseHandler](https://juejin.cn/post/6912030427519647751) 和 [Vue3 源码解析 06下篇--响应式 collectionHandler](https://juejin.cn/post/6912031278850113544) 中的说法：
>
> > <font color=red>baseHandlers 主要是针对基本数据类型</font>。其主要包含四种 handler:mutableHandlers、readonlyHandlers、shallowReactiveHandlers、shallowReadonlyHandlers，通过方法名称我们应该也能看出，这四种是针对目标数据 target 的普通类型、readonly、shallow 等类型的
>
> > <font color=red>collectionHandlers 针对的是集合数据类型（即 set、map、weakSet、weakMap）</font>。其主要包含三种：mutableCollectionHandlers（普通响应式数据）、shallowCollectionHandlers（浅层响应式数据）、readonlyCollectionHandlers（只读响应式数据）
>
> 至于为什么要设置两种 handler ，这是由 Proxy 对于 “内置对象” 的缺陷导致的；[Vue3 源码解析 06下篇--响应式 collectionHandler](https://juejin.cn/post/6912031278850113544) 中有提及：
>
> > 具体原因我们可以参考一下 [Proxy 的局限性](https://zh.javascript.info/proxy#proxy-de-ju-xian-xing)，这跟内置对象（例如 Map、Set、Date、Promise）的内部机制有关，他们的内部所有的数据存储在一个“internal slots”中。当我们访问 Set.prototype.add 其实就是通过内部的 this 来访问该方法的，但是数据代理的时候 **this=proxy**，但是 proxy 并没有相应的“internal slots”这个东西，所以会报错。 所以，可以用另一种方式来实现代理：
> >
> > ```JavaScript
> > let map = new Map();
> > 
> > let proxy = new Proxy(map, {
> >   get(target, prop, receiver) {
> >     let value = Reflect.get(...arguments);
> >     return typeof value == 'function' ? value.bind(target) : value;
> >   }
> > });
> > 
> > proxy.set('test', 1);
> > alert(proxy.get('test'));//1
> > ```
> >
> > 我们通过这种方式，改变了 this 的指向，这时候的 this 绑定为了原始对象（即 Map ），而不是之前说的 proxy。所以就避开了上面的坑。

##### 核心流程

按照上面几个概念的关系，如何搭配，该如何实现数据响应式更新？

首先定下我们的目标：自然是在数据更新时，自动刷新视图，显示最新的数据。

这就是上面提到的 Dep 和 Watcher 的关系，<font color=fuchsia>**数据是 Dep**</font>，而 <font color=fuchsia>**Watcher 触发的是页面渲染函数**</font>（这是最重要的 watcher）。

但是新问题随之而来，Dep 怎么知道有什么 Watcher 依赖于他？

Vue 采用了一个很有意思的方法：

- 在运行 Watcher 的回调函数前，先记下当前 Watcher 是什么（通过 Dep.target）
- 运行回调函数中用到响应式数据，那么**必然会调用响应式数据的 getter 函数**
- 在响应式数据的 **getter 函数中就能记下当前的 Watcher**，建立 Dep 和 Watcher 的关系
- 之后，在响应式数据更新时，必然会**调用响应式数据的 setter 函数**
- 基于之前建立的关系，在 setter 函数中就能触发对应 Watcher 的回调函数了