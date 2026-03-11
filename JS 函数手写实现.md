# JS 函数手写实现



#### 参考资料

[测试一下前端基本功--简单的源码重写？](https://www.bilibili.com/video/BV1dS4y1X7pn) 

[前端面试常见的手写功能](https://juejin.cn/post/6873513007037546510)

[56 个 JavaScript 实用工具函数助你提升开发效率！](https://mp.weixin.qq.com/s/oK--hjpAscsvyT1U4rZHKw) 虽然其中多数是没什么用的，但是深拷贝、正则判断、浏览器操作，还是值得借鉴的；有时间了，摘抄下

有了这32个手写js技巧，让你面试不凉凉！(上中下)：https://www.bilibili.com/video/BV1v341167fi / https://www.bilibili.com/video/BV1gb4y1z736 / https://www.bilibili.com/video/BV16q4y1U7GD 👀 “下”中省略了 “打印当前网页使用了多少种 html 元素”

[GitHub - Organizations - ECMAScript Shims](https://github.com/es-shims) 的主页有很多 shims repo，每一个 repo 都是一个方法的 shim 实现。



#### bind 函数

这里可以参考下 [[JS 机制与原理#bind 模拟实现]]

```js
Function.prototype.bind = function(context, ...args) {
  if (typeof this !== 'function') {
    throw new TypeError('Type Error')
  }
  
  // 保存 this 的值
  var self = this

  return function F() {
    // 这是为了实现 new 的优先级高于 bind
    if (this instanceof F) {
      return new self(...args, ...arguments)
    }
    return self.apply(context, [...args, ...arguments]) // 👀 也可使用 args.concat(arguments) ，兼容性更好些
  }
}
```

#### call 和 apply 的实现

> 👀 可以参考下 [[JS 机制与原理#call 和 apply 实现]] 中的内容。另外，call 和 apply 两者实现极为类似，可一起记忆。

##### call 实现

```js
Function.prototype.myCall = function(context, ...args) {
  if(typeof this !== 'function') {
    throw new TypeError('type error')
  }

  if(context === undefined || context === null) {
    context = globalThis
  } else {
    context = Object(context)
  }

  // 新建一个唯一的Symbol变量避免重复
  const fnSym = Symbol('fn')
  context[fnSym] = this

  const res = context[fnSym](...args)
  delete context[fnSym]

  return res
}
```

##### apply 实现

```js
Function.prototype.myApply = function(context, args) {
  if(typeof this !== 'function') {
    throw new TypeError('type error')
  }

  if(context === undefined || context === null) {
    context = globalThis
  } else {
    context = Object(context)
  }

  const fnSym = Symbol('fn')
  context[fnSym] = this

  const res = args === undefined ? context[fnSym]() : context[fnSym](...args)
  delete context[fnSym]

  return res
}
```



#### new 运算符

> 👀 可以参考下 [[JS 机制与原理#new 运算符实现]] 中的讲解。另外，其中还有另一种 new 的调用方法的源码实现 [[JS 机制与原理#视频《new实例化的重写--检测一下自己this 指向？？》的补充#new 的实现]]

```js
function myNew(fn, ...args) {
  // 基于原型链 创建一个新对象。👀 使用 Object.create() 是最常用，且简洁的方法
  let newObj = Object.create(fn.prototype)
  // 添加属性到新对象上 并获取obj函数的结果
  let res = fn.call(newObj, ...args)
  // 如果执行结果有返回值并且是一个对象, 返回执行的结果, 否则, 返回新创建的对象
  return res && typeof res === 'object' ? res : newObj;
}
```

摘自：[金三银四：20道前端手写面试题](https://juejin.cn/post/7079681931662589960)

#### instanceof 实现

```js
const instanceof = (left, right) => {
  let proto = Object.getPrototypeOf(left)
  while (true) {
    if (proto === null) return false
    if (proto === right.prototype) return true
    proto = Object.getPrototypeOf(proto)
  }
}
```

> 💡 补充：摘抄实现的代码版本的最上面，有一个判断
>
> ```js
> if(typeof left !== 'object' || typeof right !== 'object') return false
> ```
>
> 用于判断：如果不是引用类型，则直接返回 false。不过在测试的过程中，发现 `instanceof( (() => {}), Object )` 返回值为 false， 但是 `( () => {} ) instanceof Object` 显然是 `true` ，这时发现 function 也是引用类型，这里就直接被排除了；那是不是就要改成下面的方式？
>
> ```js
> const isRef = ['object', 'function']
> if(!isRef.includes(typeof left) || !isRef.includes(typeof right)) return false
> ```
>
> 看了其他博文的实现，都没有加上这个，我也不加了...

#### 原型继承

```js
function Parent() {
  this.name = 'parent'
}
function Child() {
  Parent.call(this)
  this.type = 'child'
}
Child.prototype = Object.create(Parent.prototype)
Child.prototype.constructor = Child
```

#### Object.assign 实现

`Object.assign()` 方法用于：将所有枚举属性值从一个或多个源对象复制到目标对象。它将返回目标对象（该操作是浅拷贝）

```js
Object.defineProperty(Object, 'assign', {
  value: function(target, ...args) {
    if (target == null) {
      return new TypeError('cannot convert undefined or null to object')
    }

    // 目标对象需要统一引用数据类型，若不是会自动转换
    const to = Object(target)

    for (let i = 0; i < args.length; i++) {
      // 每一个源对象
      const nextSource = args[i]
      if (nextSource !== null) {
        // 使用 for...in 和 hasOwnProperty 双重判断，确保只拿到本身的属性、方法（不包含继承的）
        for (const nextKey in nextSource) {
          if (Object.prototype.hasOwnProperty.call(nextSource, nextKey)) {
            to[nextKey] = nextSource[nextKey]
          }
        }
      }
    }
    return to
  },
  // 不可枚举
  enumerable: false,
  writable: true,
  configurable: true
})
```

#### 深拷贝实现

该实现考虑到了 Symbol 为键的属性

```js
const isObj = (val) => val !== null && typeof val === "object";

const deepClone = (target, hash = new WeakMap()) => {
  if (!isObj(target)) return target;
  if (hash.has(target)) return hash.get(target);

  const cloneTarget = Array.isArray(target) ? [] : {};
  hash.set(target, cloneTarget);

  const symKeys = Object.getOwnPropertySymbols(target);
  symKeys.forEach((s) => {
    cloneTarget[s] = isObj(s) ? deepClone(target[s]) : target[s];
  });

  for (const i in target) {
    if (Object.prototype.hasOwnProperty.call(target, i)) {
      cloneTarget[i] = isObj(target[i])
        ? deepClone(target[i], hash)
        : target[i];
    }
  }
  
  return cloneTarget
};
```

##### 使用 `MessageChannel` API 的实现

使用 `JSON.parse(JSON.stringify(target))` 实现深拷贝的不合适的，除了 JSON 不安全外，也有`JSON.stringify` 对于循环引用的对象操作会直接报错的原因。就这一问题，可以使用 `MessageChannel` API 自带的拷贝传值进行解决

```js
function deepClone(obj) {
  return new Promise(resolve => {
    const { port1, port2 } = new MessageChannel()
    port1.postMessage(obj)
    port2.onMessage = msg => resolve(msg.data)
  })
}
```

学习自：[惊呆面试官的深度克隆方式，我不说你一定不知道【渡一教育】](https://www.bilibili.com/video/BV1tu411M76h)



#### 判断两个对象是否相等

```js
const isObj = obj => typeof obj === 'object' && obj !== null

const objEq = (val, val2) => {
  if (!isObj(val) || !isObj(val2)) return Object.is(val, val2)
  if (val === val2) return true
  const valKeys = Object.keys(val)
  const val2Keys = Object.keys(val2)
  if (valKeys.length !== val2Keys.length) return false
  for (const key of valKeys) {
    if (!val2Keys.includes(key)) return false
    const res = objEq(val[key], val2[key])
    if (!res) return false
  }

  return true
}
```

类似的实现是 [Lodash 的 `isEqual`](https://lodash.com/docs#isEqual) 。此外，如果想要实现一个获取内容各不相同的对象数组，按照 Opus 4.6 的说法，存在如下简易的方法：

```ts
import { uniqWith, isEqual } from 'lodash';

const objectSet = <T extends object>(arr: T[]): T[] => uniqWith(arr, isEqual);
```

学习自：[对象数组去重【渡一教育】](https://www.bilibili.com/video/BV1NNeMeGE7N)



#### `Object.is()` 实现

`Object.is()` 主要解决的是如下两个不太合理的问题。

```js
+0 === -0 // true
NaN === NaN // false
```

##### 实现

```js
const is = (x, y) => {
  if(x === y) {
    return x !== 0 || y !== 0 || 1/x === 1/y
  }
  return x !== x && y !== y
}
```

#### `isNaN()` 的实现

根据上面的实现，可以知道 isNaN() 的实现方法。另外，[MDN - isNaN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN) 也有说实现方法。还有，`isNaN('NaN') === true`

下面就摘抄的 MDN 的实现方法：

```js
const isNaN(value) {
  cosnt n = Number(value) // isNaN('NaN') === true
  return n !== n
}
```



#### 防抖和节流

总的来说：是用来 限制函数 的执行次数

##### 防抖

通过 `setTimeout`，在一定时间间隔内，将多次触发变成一次触发。

> [!NOTE]
> 
> 这里的 `fn.apply(this, arguments)` 是正常的，虽然 `arguments` 是一个 arr-like 。参考 [MDN - `Function.prototype.apply()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 的内容：
>
> > `Function` 实例的 **`apply()`** 方法会以给定的 `this` 值和作为数组（或<font color=fuchsia>**类数组对象**</font>）提供的 `arguments` 调用该函数。

```js
const debounce = (fn, time) => {
  let timeout = null
  /* 必须要返回一个 function 而不是箭头函数，因为箭头函数没法用 arguments */
  return function() {
    clearTimeout(timeout)
    timeout = setTimeout(() => {
      fn.apply(this, arguments)
    }, time)
  }
}
```

##### 节流

减少一段时间的触发频率

```js
const throttle = (fn, time) => {
  let flag = true
  return function() {
    // 👀 定时器没有执行，flag 始终都是 false；始终 return 掉。一旦定时器执行，flag 变成 true，就执行下一次，不会 return
    if (!flag) return
    flag = false
    setTimeout(() => {
      fn.apply(this, arguments)
      flag = true
    }, time)
  }
}
```



#### 单例模式实现

##### 一些前置的细节

在同一个文件中，定义一个 class ，创建一个实例，并导出，这样并不能规避使用者无法创建另一个实例。代码示例如下：

```js
// foo.js
class Foo {
  constructor() {}
}

export default new Foo

// call.js
import foo from './Foo.js'
const fooIns = foo
const fooIns2 = new foo.constructor()
console.log(fooIns === fooIns2) // false
```

这时候可以使用代理实现单例

##### 具体实现

```js
function singleton(className) {
  let ins = null
  cosnt proxy new Proxy(className, {
    constructor(target, args) {
      if (!ins) {
        ins = Reflect.constructor(target, args)
      }
      return ins
    }
  })
  className.prototype.constructor = proxy // 如果不加上这个，依然防不住 new foo.constructor()
  return proxy
}

const singletonFoo = singleton(Foo)
export default singletonFoo
```

学习自：[使用代理实现单例【渡一教育】](https://www.bilibili.com/video/BV1qxwgeXE5m)



#### promise 实现

https://www.bilibili.com/video/BV1gb4y1z736 最后有实现。另外，https://github.com/ractivejs/ractive/blob/dev/src/polyfills/Promise.js 的实现也非常值得借鉴



##### `Promise.all()` 实现

```js
Promise.myAll = arr => {
  const ret = []
  let count = 0
  return new Promise((resolve, reject) => {
    arr.forEach((p, index) => {
      Promise.resolve(p)
        .then(res => {
          ret[index] = res // ⚠️ 这里不能用 push，是因为异步执行结果未必是有序返回的，如果使用 push：arr 和 return 的结果未必能一一对应
          count++
          if (count === arr.length) {
            resolve(ret)
          }
        }).catch(err => reject(err))
    })
  })
}
```

> 💡 可以看下 [手写 Promise.all【渡一教育】](https://www.bilibili.com/video/BV1mG411178Y) ，其中提及了，arr 未必是数组，也有可能是可迭代对象，所以未必可以用 `arr.length` ，可以先迭代统计 count 为长度 ；另外，<font color=red>如果传入的是空的迭代对象，那么一定是 resolve</font>

###### 测试用例1

```js
const p1 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000)
})
const p2 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(2), 2000)
})
const p3 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(3), 3000)
})

Promise.myAll([p1(), p2(), p3()])
  .then(res => console.log(res))
  .catch(e => console.log(e)) // [1, 2, 3]
```

###### 测试用例2

```js
const p1 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000)
})
const p2 = () => new Promise((resolve, reject) => {
  setTimeout(() => reject(2), 2000)
})
const p3 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(3), 3000)
})

Promise.myAll([p1(), p2(), p3()])
  .then(res => console.log(res))
  .catch(e => console.log(e)) // 2
```

##### `Promise.any()` 实现

```js
Promise.any = arr => {
  const errs = []
  let count = 0
  return new Promise((resolve, reject) => {
    arr.forEach((p, index) => {
      Promise.resolve(p)
        .then(
          res => resolve(res),
          err => {
            errs[index] = err
            count++
            if(count === arr.length) {
              reject(new AggregateError(errs))
            }
          }
        )
    })
  })
}
```

> 👀 感觉 Promise.any 和 Promise.all 的实现原理相当类似，所以在这里特意将他们的写法写成类似的

代码修改自：[Promise.any 的作用，如何自己实现一个 Promise.any](https://juejin.cn/post/6965596525388890142)

##### `Promise.allSettled()` 实现

```js
Promise.myAllSettled = arr => {
  const ret = []
  let count = 0
  return new Promise((resolve, reject) => {
    arr.forEach((p, index) => {
      Promise.resolve(p)
        .then(res => Promise.resolve({ status: 'fulfilled', value: res }))
        .catch(err => Promise.resolve({ status: 'rejected', reson: err }))
        .then(res => {
          ret[index] = res
          count ++
          if(count === arr.length) {
            resolve(ret)
          }
        })
    })
  })
}
```

##### `Promise.race()` 实现

```js
Promise.myRace = function(promiseArr) {
  return new Promise((resolve, reject) => {
    promiseArr.forEach(p => {
      // 如果不是 Promise 示例需要转换为 Promise 实例
      Promise.resolve(p).then(
        val => resolve(val),
        err => reject(err)
      )
    })
  })
}
```

> ⚠️ 对于 `Promise.race()` 的定义有所遗忘，以为是 fulfilled ，实际上是 settled

##### `Promise.prototype.catch()` 实现

```js
Promise.prototype.catch = function (onRejected) {
  return this.then(undefined, onRejected)
}
```

之所以写成这样，是因为 [MDN - `Promise.prototype.catch()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) 给出了如下介绍

> ( `Promise.prototype.catch()` ) 此方法是 `Promise.prototype.then(undefined, onRejected)` 的一种简写形式。

也因为 this 为 `Promise.prototype` ，所以直接写为 `return this.then(undefined, onRejected)`

##### `Promise.prototype.finally()` 实现

```js
Promise.prototype.finally = function (onFinally) {
  return this.then(
    value => Promise.resolve(onFinally()).then(() => value),
    err => Promise.resolve(onFinally()).then(() => { throw err })
  );
}
```

与 `Promise.prototype.catch()` 类似，之所以写成这样是因为 [MDN - `Promise.prototype.finally()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally) 给出了如下介绍：

> 立即返回一个新的 Promise。<font color=dodgerBlue>无论当前 promise 的状态如何</font>，此新的 promise 在返回时始终处于待定 ( pending ) 状态。<font color=dodgerBlue>如果 `onFinally` **抛出错误或返回被拒绝的 promise**</font>，则<font color=red>新的 promise 将使用该值进行拒绝</font>。<font color=dodgerBlue>**否则**</font>，<font color=red>新的 promise 将以与当前 promise 相同的状态敲定 ( settled )</font>。

##### `Promise.resolve` 实现

```js
Promise.resolve = function (value) {
  if (value instanceof Promise) return value;
  if (isPromiseLike(value)) {
    return new Promise((resolve, reject) => value.then(resolve, reject));
  }
  return new Promise((resolve) => resolve(value))
}

function isPromiseLike(obj) {
  return obj && typeof obj.then === 'function';
}
```

##### `Promise.reject` 实现

> 👀  `Promise.reject` 的实现，可以和 `Promise.resolve` 的最后一种情况（不符合 Promise 定义的）的实现作参照，辅助记忆

```js
Promise.reject = function (reason) {
  return new Promise((resolve, reject) => reject(reason))
}
```



#### Promise 工具函数

##### 顺序执行

```js
const sequencePromises = promises =>
  promises.reduce(
    (prev, curTask) => prev.then(curTask),
    Promise.resolve()
  );
```

###### 测试验证

```js
const p1 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000)
})
const p2 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(2), 2000)
})
const p3 = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve(3), 3000)
})
const promiseArr = [p1, p2, p3]

console.time('test')
sequencePromises(promiseArr)
  .then(res => {
    console.timeEnd('test')
  }).catch(e => console.log(e))
```

经过测试：结果为 “test: 6.004s” ，约等于 1 + 2 + 3 的 6s



##### 可重试的 Promise

```js
const retryPromise = (promiseFn, maxAttempts, interval) => {
  return new Promise((resolve, reject) => {
    const attempt = (attemptNumber) => {
      if (!attemptNumber) {
        reject(new Error("Max attempts reached"));
        return;
      }
      promiseFn()
        .then(resolve)
        .catch(() => {
          setTimeout(() => {
            attempt(attemptNumber - 1);
          }, interval);
        });
    };
    attempt(maxAttempts);
  });
};
```

摘自：[9 Must-Know Advanced Uses of Promises](https://blog.stackademic.com/9-must-know-advanced-uses-of-promises-a6d1ab195dfc) ，这里看的译文 [分享 9个 使用promises的技巧](https://mp.weixin.qq.com/s/MUr29XUJjAouaYrwmXvrOg)



#### Promise 并行调度器

> [!TIP]
>
> 该功能对应的专业术语是 “p-limit” ，chatgpt 的介绍如下：
>
> <img src="https://s2.loli.net/2023/04/03/qpfRs4FATjMQYSW.png" alt="image-20230403160136690" style="zoom:48%;" />

```js
// 类的现实
class Scheduler {
  constructor() {
    this.queue = []
    this.maxCount = 2
    this.runCounts = 0
  }

  add(promiseCreator) {
    this.queue.push(promiseCreator)
  }

  taskStart() {
    for (let i = 0; i < this.maxCount; i++) {
      this.request()
    }
  }

  request() {
    if (!this.queue || !this.queue.length || !this.runCounts >= this.maxCount) {
      return
    }
    this.runCounts++

    this.queue.shift()().then(() => {
      this.runCounts--
      this.request()
    })
  }
}

const timeout = time => new Promise(resolve => {
  setTimeout(resolve, time)
})

const scheduler = new Scheduler()

const addTask = (time, order) => {
  scheduler.add(() => timeout().then(() => console.log(order)))
}

addTask(1000, '1')
addTask(500, '2')
addTask(300, '3')
addTask(400, '4')
```

##### 神说要有光的实现

```js
const pLimit = (concurrency) => {
  const queue = [];
  let activeCount = 0;

  const next = () => {
    activeCount--;

    if (queue.length > 0) {
      queue.shift()();
    }
  };

  const run = async (fn, resolve, ...args) => {
    activeCount++;

    const result = (async () => fn(...args))();

    resolve(result);

    try {
      await result;
    } catch { }

    next();
  };

  const enqueue = (fn, resolve, ...args) => {
    queue.push(run.bind(null, fn, resolve, ...args));

    if (activeCount < concurrency && queue.length > 0) {
      queue.shift()();
    }
  };

  const generator = (fn, ...args) =>
    new Promise((resolve) => {
      enqueue(fn, resolve, ...args);
    });

  return generator;
};
```

摘自：[手写 p-limit，40 行代码实现并发控制](https://juejin.cn/post/7197246543208071205)

##### 群友分享的实现

```ts
export const mapLimited = async <
  T extends readonly (() => Promise<unknown>)[] | [],
  R extends { -readonly [P in keyof T]: Awaited<ReturnType<T[P]>> }
>(
  list: T,
  concurrency = list.length
): Promise<R> => {
  const result: any = [];
  await Promise.all(
    new Array(Math.min(concurrency, list.length))
      .fill(list.entries())
      .map(
        async (
          iteratee: IterableIterator<[number, () => Promise<unknown>]>
        ) => {
          for (const [index, executor] of iteratee) {
            const res = await executor();
            result[index] = res;
          }
        }
      )
  );
  return result as R;
};
```



#### sleep 函数实现

即实现强制阻塞

```js
const sleep = async msCount => new Promise((resolve) => setTimeout(resolve, msCount))
```

##### 测试

```js
;(async () => {
  await sleep(5000)
  console.log('sleep ending')
})()
```

##### 其他补充

```js
const isb = new Int32Array(new SharedArrayBuffer(4))
function sleep (ms) {
  Atomics.wait(isb, 0, 0, ms)
}
```

摘自：[为什么JavaScript中没有sleep方法？ - justjavac的回答 - 知乎](
https://www.zhihu.com/question/22578576/answer/3387064599)



#### 排序实现

在写排序前，先实现一个 `swap` 的工具函数

```js
function swap(arr, index, index2) {
  [arr[index], arr[index2]] = [arr[index2], arr[index]]
}
```

##### 冒泡排序

```js
function bubbleSort(arr) {
  const last = arr.length - 1
  for (let i = 0; i < last; i++) {
    let flag = false // 表示是否发生交换
    for (let j = last; j > i; j--) {
      if (arr[j - 1] > arr[j]) {
        swap(arr, j - 1, j)
        flag = true
      }
    }
    if (!flag) return
  }
}
```

##### 选择排序

```js
const selectSort = (arr) => {
  const len = arr.length;

  for (let i = 0; i < len - 1; i++) {
    let min = i;
    for (let j = i + 1; j < len; j++) {
      if (arr[j] < arr[min]) min = j;
    }
    if (min != i) swap(arr, i, min);
  }
}
```


##### 快速排序

```js
function quickSort(arr, low, high) {
  if (low < high) {
    const pivotPos = partition(arr, low, high)
    quickSort(arr, low, pivotPos - 1)
    quickSort(arr, pivotPos + 1, high)
  }
}

function partition(arr, low, high) {
  const pivot = arr[low]
  while (low < high) {
    while (low < high && arr[high] >= pivot) high--
    arr[low] = arr[high]
    while (low < high && arr[low] <= pivot) low++
    arr[high] = arr[low]
  }
  arr[low] = pivot
  return low
}
```

##### 测试

```js
const arr = [1, 3, 2, 0, 4, -1]
quickSort(arr, 0, arr.length - 1)

console.log(arr) // [ -1, 0, 1, 2, 3, 4 ]
```


#### 数组转树

##### 第一种

###### 测试数据与格式

```js
const list = [
  { id: 0, name: '1', parent: -1, childNode: [] },
  { id: 1, name: '1', parent: 0, childNode: [] },
  { id: 99, name: '1-1', parent: 1, childNode: [] },
  { id: 111, name: '1-1-1', parent: 99, childNode: [] },
  { id: 66, name: '1-1-2', parent: 99, childNode: [] },
  { id: 1121, name: '1-1-2-1', parent: 112, childNode: [] },
  { id: 12, name: '1-2', parent: 1, childNode: [] },
  { id: 2, name: '2', parent: 0, childNode: [] },
  { id: 21, name: '2-1', parent: 2, childNode: [] },
  { id: 22, name: '2-2', parent: 2, childNode: [] },
  { id: 221, name: '2-2-1', parent: 22, childNode: [] },
  { id: 3, name: '3', parent: 0, childNode: [] },
  { id: 31, name: '3-1', parent: 3, childNode: [] },
  { id: 32, name: '3-2', parent: 3, childNode: [] }
]
```

###### 解法

```js
function listToTree(list, parentId = -1) {
  const filtered = list.filter(({ parent }) => parent === parentId)

  filtered.forEach(item => item.childNode = listToTree(list, item.id))

  return filtered
}

const tree = listToTree(list)
```

结果自行运行，有点长，略

##### 第二种

###### 测试数据与格式

```js
const list = [
  { value: '江苏', parent: null, id: '1' },
  { value: '苏州', parent: '1', id: '1.1' },
  { value: '吴中区', parent: '1.1', id: '1.1.1' },
  { value: '浙江', parent: null, id: '2' },
  { value: '杭州', parent: '2', id: '2.1' },
  { value: '余杭区', parent: '2.1', id: '2.1.1' }
]

const tree = [
  { value: '江苏', id: '1', children: [
       { value: '苏州', id: '1.1', children: [
            { value: '吴中区', id: '1.1.1' }
        ]} 
   ]},
  { value: '浙江', id: '2', children: [
     { value: '杭州',  id: '2.1', children: [
        { value: '余杭区', id: '2.1.1' } 
      ]}
  ]}
]
```

###### 解法

```js
function listToTree(list, parentId = null) {
  const tree = list.filter(({ parent }) => parent === parentId)
    .map(({ parent, ...rest }) => rest)

  tree.forEach(item => {
    const childTree = listToTree(list, item.id)
    if (childTree.length) {
      item.children = childTree
    }
  })

  return tree
}

const tree = listToTree(list)
```



### 函数高阶函数实现

> 👀 这些函数编程的实现有点难度了。不过，很多东西是共通的，就像有个模版。

#### Array.prototype.forEach() 实现

##### 语法

```js
arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

##### 实现

```js
Array.prototype.map = function(cb, thisArg) {
  if (this == undefined) {
    // 👀 TypeError（类型错误） 对象用来表示值的类型非预期类型时发生的错误。
    throw new TypeError('this is null or undefined')
  }
  if (typeof cb !== 'function') {
    throw new TypeError(cb + 'is not a function')
  }
  // 让 O 成为回调函数的对象传递（强制转换对象）
  const O = Object(this)
  // `>>> 0` 保证 length 为 number 类型，且为正整数
  const len = O.length >>> 0
  let k = 0
  while (k < len) {
    if (k in O) {
      cb.call(thisArg, O[k], k, O)
    }
  }
}
```

#### Array.prototype.map() 实现

##### 语法

```js
var new_array = arr.map(callback(currentValue[, index[, array]])[, thisArg])
```

##### 实现

```js
Array.prototype.map = function(cb, thisArg) {
  if (this == undefined) {
    // 注：TypeError（类型错误） 对象用来表示值的类型非预期类型时发生的错误。
    throw new TypeError('this is null or undefined')
  }
  if (typeof cb !== 'function') {
    throw new TypeError(cb + 'is not a function')
  }
  const res = []
  // 让 O 成为回调函数的对象传递（强制转换对象）
  const O = Object(this)
  // >>> 0 是保证 length 为 number 类型，且为正整数
  const len = O.length >>> 0
  for (let i = 0; i < len; i++) {
    if (i in O) {
      // 调用回调函数并传入新数组
      res[i] = cb.call(thisArg, O[i], i, this)
    }
  }
  return res
}
```

#### Array.prototype.filter() 实现

##### 语法

```js
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
```

##### 实现

```js
Array.prototype.filter = function(cb, thisArg) {
  if (this == undefined) {
    // 注：TypeError（类型错误） 对象用来表示值的类型非预期类型时发生的错误。
    throw new TypeError('this is null or undefined')
  }
  if (typeof cb !== 'function') {
    throw new TypeError(cb + 'is not a function')
  }
  const res = []
  // 让 O 成为回调函数的对象传递（强制转换对象）
  const O = Object(this)
  // >>> 0 是保证 length 为 number 类型，且为正整数
  const len = O.length >>> 0
  for (let i = 0; i < len; i++) {
    if (i in O) {
      // 回调函数调用传参
      if (cb.call(thisArg, O[i], i, O)) {
        res.push(O[i])
      }
    }
  }
  return res
}
```

#### Array.prototype.reduce() 实现

##### 语法

```js
reduce(callback(previousValue, currentValue[, currentIndex, array])[, initialValue])
```

##### 实现

```js
Array.prototype.reduce = function(cb, initialValue) {
  if (this == undefined) {
    // 注：TypeError（类型错误） 对象用来表示值的类型非预期类型时发生的错误。
    throw new TypeError('this is null or undefined')
  }
  if (typeof cb !== 'function') {
    throw new TypeError(cb + 'is not a function')
  }
  // 让 O 成为回调函数的对象传递（强制转换对象）
  const O = Object(this)
  // >>> 0 保证 length 为 number 类型，且为正整数
  const len = O.length >>> 0
  let accumulator = initialValue
  let k = 0
  if (accumulator === undefined) {
    while (k < len && !(k in O)) {
      k++
    }
    if (k >= len) {
      throw new TypeError('reduce of empty array with no initial value')
    }
    accumulator = O[k++]
  }
  while (k < len) {
    if (k in O) {
      accumulator = cb.call(undefined, accumulator, O[k], k, O)
    }
    k++
  }
  return accumulator
}
```





### utils 与功能实现



#### flat 实现

👀 可以参考下 [JavaScript专题之数组扁平化](https://github.com/mqyqingfeng/Blog/issues/36)

##### 使用 reduce() 实现

```js
const flat = arr => arr.reduce((acc, cur) =>
  Array.isArray(cur)
    ? [...acc, ...flat(cur)]
    : [...acc, cur]
  , [])
```

👀 这里用了递归。另外，兼容性方面，`Array.prototype.reduce()` 是 ES5 甚至更早 的方法（兼容 IE9），`Array.isArray()` 是 ES5 的方法

###### 带深度的 flat 实现

```js
const flat = (arr, depth = 1) => arr.reduce((acc, cur, index) =>
  depth && Array.isArray(cur)
    ? [...acc, ...flat(cur, depth - 1)]
    : [...acc, cur]
  , [])
```

这里也可以用 concat，但是鉴于，concat 默认做一层解构（  `[].concat([1, 2])` 的结果是 `[1, 2]` 而不是以为的 `[[1, 2]]` ）；所以，需要多加一个 `[]` ；如下：

```js
const flatten = (arr, depth = 1) => arr.reduce((acc, cur, index) =>
  depth && Array.isArray(cur)
    ? acc.concat(flatten(cur, depth - 1))
    : acc.concat([cur]) // 这里多加一个 []
    // 或者也可以使用 (acc.push(cur), acc)
  , [])
```

##### 使用递归

原理和上面差不多，只不过上面是 reduce 累加，以及 concat 合并；这里是 push

```js
const retArr = []
const flat = arr => arr.map(e => Array.isArray(e) ? flat(e) : resArr.push(e))
```



#### 数组去重

##### 使用 Set

```js
const targetArr = [ ... ]
const res = Array.from(new Set(targetArr))
```

##### 使用 indexOf / includes

```js
const uniqueArr = []
// indexOf
const unique = arr => arr.map(e => uniqueArr.indexOf(e) === -1 && uniqueArr.push(e))
// includes
const unique = arr => arr.map(e => !uniqueArr.includes(e) && uniqueArr.push(e))
```

##### 使用 filter

```js
const unique = arr => arr.filter((e, index) => arr.indexOf(e) === index)
```

##### 使用 reduce

```js
const unique = arr => arr.reduce(
  (acc, cur) => acc.includes(cur) 
    ? acc
    : acc.concat(cur), []
)
```

> ⚠️ 开始时没有对 `acc.includes(cur) === true` 的情况进行返回 ( `acc` )，这是会报错的；因为没有返回 acc 的话，默认返回 undefined，而 undefined 没有 includes 方法，将会报错。所以，无论如何都要返回 acc，哪怕本次操作没有对其进行任何操作。



#### 数组转置

##### 使用 Array.prototype.map

```js
const transpose = matrix => matrix[0].map((col, i) => matrix.map(row => row[i]))
```

摘自：[js小众且好用的技巧【一行代码】](https://juejin.cn/post/7228449980108423224)

##### 使用 Array.from

```ts
export const transpose = (arr: number[][]): number[][] => {
  return Array.from({length: arr[0].length}, (_: never, i) => arr.map(k => k[i]))
}
```



#### 多数组取交集

```js
const intersection = (first, ...rest) => [...new Set(first)].filter(el => rest.every(arr => arr.includes(el)))

// 测试
intersection([1, 2, 3, 4], [2, 3, 4, 7, 8], [1, 3, 4, 9]) // [3, 4]
```



#### 类数组转为数组

##### 使用 Array.from

```js
const arr = Array.from(arrLike)
```

##### 使用 Array.prototype.slice.call()

```js
const arr = Array.prototype.slice.call(arrLike)
```

##### 扩展运算符

```js
const arr = [ ...arrLike ]
```

##### 使用 concat

> 👀 没想到

```js
const arr = Array.prototype.concat.apply([], arrLike)
```



#### 柯里化

##### 理想情况下的简单实现

如果是实现简单的 `add(1)(2) = 3` ，则利用闭包，有如下代码：

```js
function curry(num) {
  let sum = num
  
  return function(num) {
    return sum += num
  }
}

const ret = curry(1)(2)
console.log(ret)
```

更进一步，实现：`add(1)(2)(3)(4) = 10` 甚至 `add(1)(1, 2, 3)(2) = 9`

```js
function add() {
  const _args = [...arguments]
  function fn() {
    _args.push(...arguments)
    return fn
  }
  fn.toString = function() {
    return _args.reduce((sum, cur) => sum + cur)
  }
  return fn
}
```

> 💡 一点补充
>
> 如果要求是实现如下效果：
>
> ```js
> const r1 = add[1][2][3] + 4    // 10
> const r2 = add[10][20] + 30    // 60
> const r3 = add[100][200] + 300 // 600
> ```
>
> 可以通过代理实现：
>
> ```js
> function createProxy(value = 0) {
>   const valueGetter = () => value
>   
>   return new Proxy({}, {
>     get(target, prop) {
>       if (prop === Symbol.toPrimitive) {
>         // 为了处理 add[1][2][3] 的值为**代理**和 4 相加的错误
>         return valueGetter
>       }
>       return createProxy(value + Number(prop))
>     }
>   })
> }
> ```
>
> 测试如下：
>
> ```js
> const add = createProxy()
> 
> const r1 = add[1][2][3] + 4    // 10
> const r2 = add[10][20] + 30    // 60
> const r3 = add[100][200] + 300 // 600
> ```
>
> 学习自：[一道promise的社招面试题【渡一教育】](https://www.bilibili.com/video/BV1sqP5eUEFc)

##### 通用实现

```javascript
const curry = fn =>
  judge = (...args) =>
    args.length >= fn.length
      ? fn(...Array.prototype.slice.call(args, 0, fn.length))
      : (...arg) => judge(...args, ...arg)
```



#### AJAX 实现

```js
// 使用 GET 方法，获取 JSON
const ajax = function (url) {
  return new Promise((resolve, reject) => {
    const xhr = XMLHttpRequest
      ? new XMLDocument()
      : new ActiveXObject("MicroSoft.XMLHttp")
    xhr.open("GET", url, false);
    xhr.setRequestHeader("Accept", "application/json")
    xhr.onReadyStateChange = function () {
      if (xhr.readyState !== 4) return
      if (xhr.status === 200 || xhr.status === 304) {
        resolve(xhr.responseText)
      } else {
        reject(new Error(xhr.responseText))
      }
    }
    xhr.send()
  })
}
```



#### JSONP 实现

script 标签不遵循 “同源协议”，可用来进行跨域请求；兼容性好，但是仅限于 GET 请求

```js
const jsonp = ({ url, params, callbackName }) => {
  const generateUrl = () => {
    let dataSrc = ''
    for (let key in params) {
      if (Object.prototype.hasOwnProperty.call(params, key)){
        dataSrc = `${key}=${params[key]}&`
      }
    }
    dataSrc += `callback=${callbackName}`
    return `${url}?${dataSrc}`
  }
  return new Promise((resolve, reject) => {
    const scriptEle = document.createElement('script')
    scriptEle.src = generateUrl()
    document.body.appendChild(scriptEle)
    window[callbackName] = data => {
      resolve(data)
      document.removeChild(scriptEle)
    }
  })
}
```



#### 图片懒加载

给 img 标签统一加上自定义属性 `data-src="default.png"` ，当检测到图片出现在窗口之后再补充 src 属性，此时进行图片懒加载

```js
function lazyload() {
  const imgs = document.getElementsByTagName('img')
  const len = imgs.length
  // 视口的高度
  const viewHeight = document.documentElement.clientHeight
  // 滚动条的高度
  const scrollHeight = document.documentElement.scrollTop || document.body.scrollTop
  for (let i = 0; i < len; i++) {
    const offsetHeight = imgs[i].offsetTop
    if (offsetHeight < viewHeight + scrollHeight) {
      const src = imgs[i].dataset.src
      imgs[i].src = src
    } 
  }
}

window.addEventListener('scroll', lazyload)
```

#### 滚动加载

原理就是监听页面滚动事件，分析 clientHeight、scrollTop、scrollHeight 三者关系

```js
window.addEventListener('scroll', function() {
  const clientHeight = document.documentElement.clientHeight
  const scrollTop = document.documentElement.scrollTop
  const scrollHeight = document.documentElement.scrollHeight
  if (clientHeight + scrollTop >= scrollHeight) {
    // 监测到滚动到页面底部，进行后续操作
  }
}, false)
```

#### 渲染几万条数据不卡住页面

渲染大数据时，合理使用 createDocumentFragment（文档碎片） 和 requestAnimationFrame，将操作切分为 一小段一小段执行

```js
setTimeout(() => {
  // 插入十万条数据
  const total = 100000
  // 一次插入的数据条数
  const once = 20
  // 插入数据需要的次数
  const loopCount = Math.ceil(total / once)
  let countOfRender = 0
  const ul = document.querySelector('url')
  // 添加数据的方法
  function add() {
    const fragment = document.createDocumentFragment()
    for (let i = 0; i < once; i++) {
      const li = document.createElement('li')
      li.innerText = Math.floor(Math.random() * total)
      fragment.appendChild(li)
    }
    ul.appendChild(fragment)
    countOfRender += 1
    loop()
  }
  function loop() {
    if (countOfRender < loopCount) {
      window.requestAnimationFrame(add)
    }
  }
  loop()
}, 0)
```

#### 将 虚拟DOM 转化为真实 DOM 结构

这是当前 SPA 应用核心概念之一

```js
/* vnode 结构
 {
   tag,
   attr,
   children,
 }
 */

// virtual dom -> dom
function render(vnode, container) {
  container.appendChild(_render(vnode))
}

function _render(vnode) {
  // 如果是数字类型，转化为字符串
  if (typeof vnode === 'number') {
    vnode = String(vnode)
  }
  // 字符串类型，就是文本结点
  if (typeof vnode === 'string') {
    return document.createTextNode(vnode)
  }
  // 普通 dom
  const dom = document.createElement(vnode.tag)
  if (vnode.attrs) {
    // 遍历属性
    Object.keys(vnode.attr).forEach(key => {
      const value = vnode.attrs[key]
      dom.setAttribute(key, value)
    })
  }
  // 子数组进行递归操作
  vnode.children.forEach(child => render(child, dom))
  return dom
}
```



#### 检测当前 UA 是否为 IE

```js
const isIE = () => !!document.documentMode
```



#### 检测函数是否为异步函数

```js
function isAsyncFn = fn => Object.prototype.toString.call(fn) === '[object AsyncFunction]'
```

##### 通过 `@@toStringTag` 实现

```js
function isAsyncFn = fn => fn[Symbol.toStringTag] === 'AsyncFunction'
```

参考自：[判断函数是否标记为async【渡一教育】](https://www.bilibili.com/video/BV1oG41117RU)



#### 生成 UUID

虽然可以是通过调用 [`Crypto.randomUUID()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Crypto/randomUUID) 来实现，不过，除了兼容性不够好之外；必须在 https 环境下使用

##### 实现

```js
const genUUID = seed => seed
  ? (seed ^ ((Math.random() * 16) >> (seed / 4))).toString(16)
  : ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, uuid)
```

摘自：[js小众且好用的技巧【一行代码】](https://juejin.cn/post/7228449980108423224)



#### 大数之和

> 👀 虽然也可以用 BigInt，但是这显然不是本题本意

```js
function sum(a, b) {
  let result = ''
  
  // 由于两数长度不一，所以需要将两数对其，即：短的数字前面补足零
  const len = Math.max(a.length, b.length)
  a = a.padStart(len, '0')
  b = b.padStart(len, '0')
  
  // carry 用于记录进位，显然开始时为 0
  let carry = 0
  for (let i = len - 1; i >= 0; i--) {
    const n = +a[i] + +b[i] + carry
    carry = Math.floor(n / 10)
    result = (n % 10) + result
  }
  
  // 一个容易遗漏的点：如果最高位相加需要进位，需要单独处理
  if ( carry ) {
    result = '1' + result
  }
  return result
}
```



### LeetCode

#### 位运算

##### 一些必要的知识

位运算 `&` ( AND )、`|`（ OR ) 和 `^` ( XOR ) 在 <font color=fuchsia>支持自身运算的</font> 交换律、结合律和分配律。意为：<font color=dodgerBlue>**如果该表达式中只包含单一种类的 `&` / `|` / `^` **</font>，则支持 交换律、结合律和分配律。

如果，包含不止一种的 `&` / `|` / `^`  ，则只能说：只支持交换律，不支持结合律和分配律。

> <font color=dodgerBlue>反例如下：</font>
>
> ```js
> // 结合律
> (3 | 1) & 2  = 2
> 3 | 1 & 2 = 3
> 
> // 分配律
> (4 | 3) ^ 1 = 6;
> (4 ^ 1) | (3 ^ 1) = 7
> ```
>
> 摘自：[位运算总结](https://zhuanlan.zhihu.com/p/37167219)

另外，在问题 [grok3 - 编程语言中的位运算 `&` 、 `|` 、`^` 是否支持 交换律、结合律、分配律 等定律？](https://grok.com/share/bGVnYWN5_1206e71b-28d4-4793-bbe7-d3055ed8d595) 给出了更细化的结论：

> 分配律涉及一个操作如何分布到另一个操作上。我们检查以下关系：
>
> - **AND 对 OR 的分配性**：`a & (b | c) = (a & b) | (a & c)`。例如，`a = 5 (101)`、`b = 3 (011)`、`c = 1 (001)` ，左边 `5 & (3 | 1)` 为 `5 & 3 = 1 (001)`，右边 `(5 & 3) | (5 & 1) = 1 | 1 = 1 (001)`，相等。
> - **OR 对 AND 的分配性**：`a | (b & c) = (a | b) & (a | c)`。同上，`5 | (3 & 1)` 为 `5 | 1 = 5 (101)`，右边 `(5 | 3) & (5 | 1) = 7 & 5 = 5 (101)`，相等。
> - **AND 对 XOR 的分配性**：`a & (b ^ c) = (a & b) ^ (a & c)`。例如，`5 & (3 ^ 1)` 为 `5 & 2 = 0 (000)`，右边 `(5 & 3) ^ (5 & 1) = 1 ^ 1 = 0 (000)`，相等。
>
> 然而，<font color=red>**XOR 对 AND 或 OR 的分配性不成立**</font>。例如，`a | (b ^ c) = (a | b) ^ (a | c)` 不总成立，之前例子中 `a = 1`、`b = 1`、`c = 0` 时，左边为 1，右边为 0。

###### 优先级

参见 [MDN - 运算符优先级 # 汇总表](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_precedence#%E6%B1%87%E6%80%BB%E8%A1%A8) 中的内容（ [[JavaScript备忘录#运算符优先级汇总表]] 中有摘抄）

- 按位左移 `<<`、按位右移 `>>`、无符号右移 `>>>` 优先级在位运算中最高，结合性从左到右。
- 按位与 `&` 其次，按位异或 `^` 再次，按位或 `|` 最低；结合性从左到右。

##### 231 Power of Two

> 🔗 [231 Power of Two](https://leetcode.com/problems/power-of-two/)

没看懂数学原理... 虽然用传统写法也能 AC... 暂时略...
##### 136. Single Number

> 🔗 [136. Single Number](https://leetcode.com/problems/single-number)

```ts
function singleNumber(nums: number[]): number {
    return nums.reduce((acc, cur) => acc ^= cur, 0)
};
```

原理如下：两个相同的数作异或结果一定为 0，而该数组中除了目标元素之外，其他元素都出现了两次，所以这些数相异或的结果一定为 0（参考 [[#一些必要的知识]] 中的内容：单一的异或运算支持 “交换律” 和 “结合律” ），相当于相互抵消了；剩下的那个目标元素，再与 0 作异或，结果一定是那个目标元素。

学习自：[用二进制思维重构前端权限系统](https://juejin.cn/post/7474862152928493631)
