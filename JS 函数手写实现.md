# JS 函数手写实现



### bind 函数

这里可以参考下 [[[JS 机制与实现原理#bind 模拟实现]]

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
    return self.apply(context, [...args, ...arguments])
  }
}
```

### call 和 apply 的实现

**注：**可以参考下 [[JS 机制与实现原理#call 和 apply 实现]] 中的内容。另外，call 和 apply 两者实现极为类似，可一起记忆。

#### call 实现

```js
Function.prototype.call = function(context = functions, ...args) {
  if (typeof this !== 'function') {
    throw new TypeError('Type Error')
  }
  // 新建一个唯一的Symbol变量避免重复
  const fn = Symbol('fn')
  context[fn] = this

  const res = context[fn](...args)
  delete context[fn]
  return res
}
```

#### apply 实现

```js
Function.prototype.apply = function(context = window, args) {
  if (typeof this !== 'function') {
    throw new TypeError('Type Error')
  }
  const fn = Symbol('fn')
  context[fn] = this

  const res = context[fn](...args)
  delete context[fn]
  return res
}
```



#### new 运算符

**注：**可以参考下 [[JS 机制与实现原理#new 运算符实现]] 中的讲解。另外，其中还有另一种 new 的调用方法的源码实现 [[JS 机制与实现原理#视频《new实例化的重写--检测一下自己this 指向？？》的补充#new 的实现]]

```js
function myNew(fn, ...args) {
  // 基于原型链 创建一个新对象。注：使用 Object.create() 是最常用，且简洁的方法
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
  // 基本数据类型都返回 false
  if (typeof left !== 'object' || typeof right !== 'object') return false
  let proto = Object.getPrototypeOf(left)
  while (true) {
    if (proto === null) return false
    if (proto === right.prototype) return true
    proto = Object.getPrototypeOf(proto)
  }
}
```

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

Object.assign() 方法用于：将所有枚举属性值从一个或多个源对象复制到目标对象。它将返回目标对象（该操作是浅拷贝）

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

该实现考虑到了 Symbol 属性

```js
const cloneDeep = (target, hash = new WeakMap()) => {
  // 对如果是基本类型，且不是 null
  if (typeof target !== 'object' || target === null) return target
  // hash 中存在的，直接返回。避免循环引用
  if (hash.has(target)) return hash.get(target)
  
  // 对数组类型进行处理
  const cloneTarget = Array.isArray(target) ? [] : {}
  hash.set(target, cloneTarget)

  // 对 symbol 属性处理
  const symKeys = Object.getOwnPropertySymbols(target)
  if (symKeys.length) {
    symKeys.forEach(symKey => {
      // 	由于 typeof null === 'object'，排除这种情况。
      if (typeof target[symKey] === 'object' && target[symKey] !== null) {
        cloneTarget[symKey] = cloneDeep(target[symKey])
      } else {
        cloneTarget[symKey] = target[symKey]
      }
    })
  }

  for (const i in target) {
    // 是obj的属性才拷贝。对象原型上的属性，不进行拷贝
    if (Object.prototype.hasOwnProperty.call(target, i)) {
      cloneTarget[i] =
        typeof target[i] === 'object' && target[i] !== null
        ? cloneDeep(target[i], hash)
        : target[i]
    }
  }

  return cloneTarget
}
```



#### Object.is() 实现

Object.is() 主要解决的是：

```js
+0 === -0 // true
NaN === NaN // false
```

实现：

```js
const is = (x, y) => {
  if(x === y) {
    return x !== 0 || y !== 0 || 1/x === 1/y
  }
  return x !== x && y !== y
}
```

#### isNaN() 的实现

根据上面的实现，可以知道 isNaN() 的实现方法。另外，[MDN - isNaN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN) 也有说实现方法。还有，isNaN('NaN') === true

下面就摘抄的 MDN 的实现方法：

```js
const isNaN(value) {
  cosnt n = Number(value) // isNaN('NaN') === true
  return n !== n
}
```





### 防抖和节流

总的来说：是用来 限制函数 的执行次数

#### 防抖

通过 setTimeout，在一定时间间隔内，将多次触发变成一次触发。

```js
const debounce = (fn, time) => {
  let timeout = null
  return function() {
    clearTimeout(timeout)
    timeout = setTimeout(() => {
      fn.apply(this, arguments)
    }, time)
  }
}
```

#### 节流

减少一段时间的触发频率

```js
const throttle = (fn, time) => {
  let flag = true
  return function() {
    // 注：定时器没有执行，flag 始终都是 false；始终 return 掉。一旦定时器执行，flag 变成 true，就执行下一次，不会 return
    if (!flag) return
    flag = false
    setTimeout(() => {
      fn.apply(this, arguments)
      flag = true
    }, time)
  }
}
```



#### promise 实现

```js

```

https://www.bilibili.com/video/BV1gb4y1z736 最后有实现。



#### Promise.all() 实现

```js
Promise.myAll = function(promiseArr) {
  return new Promise((resolve, reject) => {
    const ans = []
    let index = 0
    for (let i = 0; i < promiseArr.length; i++) {
      promiseArr[i]
        .then(res => {
          ans[i] = res
            index++
            if (index === promiseArr.length) {
              resolve(ans)
            }
          })
        .catch(err => reject(err))
    }
  })
}
```

#### Promise.race() 实现

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

#### Promise 并行调度器

**注：**只有代码，没太看懂

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



### 函数高阶函数实现

**注：**这些函数编程的实现有点难度了。不过，很多东西是共通的，就像有个模版。

#### Array.prototype.forEach() 实现

语法：

```js
arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

实现：

```js
Array.prototype.map = function(cb, thisArg) {
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
  let k = 0
  while (k < len) {
    if (k in O) {
      cb.call(thisArg, O[k], k, O)
    }
  }
}
```

#### Array.prototype.map() 实现

语法：

```js
var new_array = arr.map(callback(currentValue[, index[, array]])[, thisArg])
```

实现：

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

语法：

```js
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
```

实现：

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

`语法：

```js
reduce(callback(previousValue, currentValue[, currentIndex, array])[, initialValue])
```

实现：

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



#### flat 实现

注：可以参考下 [JavaScript专题之数组扁平化](https://github.com/mqyqingfeng/Blog/issues/36)

##### 使用 reduce() 实现

```js
const flat = arr => arr.reduce(
  (pre, cur) => pre.concat(Array.isArray(cur) ? flat(cur) : cur), [])
```

**注：** 这里用到了递归。另外，兼容性方面，Array.prototype.reduce() 是 ES5 甚至更早 的方法（兼容 IE9），Array.isArray() 是 ES5 的方法

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
const unique = arr => arr.reduce((acc, cur) => acc.includes(cur) ? acc : acc.concat(cur), [])
```

注意：开始时没有对 `acc.includes(cur) === true` 的情况进行返回 ( acc )，这是会报错的；因为没有返回 acc 的话，默认返回 undefined，而 undefined 没有 includes 方法，将会报错。所以，无论如何都要返回 acc，哪怕本次操作没有对其进行任何操作。

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

**注：**这个挺巧妙，但没想到

```js
const arr = [ ...arrLike ]
```

##### 使用 concat

**注：**没想到。另外，这里 只能用 apply，不可用 call

```js
const arr = Array.prototype.concat.apply([], arrLike)
```



#### 柯里化

如果是实现简单的 add(1)(2) = 3，则利用闭包，有如下代码：

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

更进一步，实现：add(1)(2)(3)(4) = 10 甚至 add(1)(1, 2, 3)(2) = 9

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

给 img 标签统一加上自定义属性 data-src="default.png"，当检测到图片出现在窗口之后再补充 src 属性，此时进行图片懒加载

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





参考资料：

[测试一下前端基本功--简单的源码重写？](https://www.bilibili.com/video/BV1dS4y1X7pn) 

[前端面试常见的手写功能](https://juejin.cn/post/6873513007037546510)

[56 个 JavaScript 实用工具函数助你提升开发效率！](https://mp.weixin.qq.com/s/oK--hjpAscsvyT1U4rZHKw) 虽然其中多数是没什么用的，但是深拷贝、正则判断、浏览器操作，还是值得借鉴的；有时间了，摘抄下

有了这32个手写js技巧，让你面试不凉凉！(上中下)：https://www.bilibili.com/video/BV1v341167fi / https://www.bilibili.com/video/BV1gb4y1z736 / https://www.bilibili.com/video/BV16q4y1U7GD 注：“下”中省略了 “打印当前网页使用了多少种 html 元素”、