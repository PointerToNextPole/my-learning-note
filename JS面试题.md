# js 面试题



### 函数参数相关

<details>
  <summary>这类题重点 / 易错点 总结：</summary>
  参数相当于函数内部的变量（详见 第一题）。<br>
  给参数赋默认值时，是有顺序的，同时也是有类型死区的。（详见 第二题）
</details>


##### 第一题

```js
let a = 1
function b(a) {
  a = 2
  console.log(a)
}
b(a)
console.log(a)
```

这题做的时候，没错。但是解析中 加重点的一句 很重要；之前并不知道。⭐️

<details>
  <summary>点击查看答案</summary>
  2, 1
</details>

<details>
  <summary>点击查看解析</summary>
  首先基本类型数据是按值传递的，所以执行b函数时，b的参数a接收的值为1，<font color=FF0000 size=4>参数</font>a<font color=FF0000 size=4>相当于函数内部的变量</font>，当本作用域有和上层作用域同名的变量时，无法访问到上层变量，所以函数内无论怎么修改a，都不影响上层，所以函数内部打印的a是2，外面打印的仍是1。
</details>

##### 第二题

```js
function a (b = c, c = 1) {
  console.log(b, c)
}
a()
```

这题做的时候，<font color=FF0000>错了</font>。并且解析中加重点的部分，<font color=FF0000>非常重要</font>。⭐️⭐️

<details>
  <summary>点击查看答案</summary>
  报错：ReferenceError: Cannot access 'c' before initialization
</details>
<details>
  <summary>点击查看解析</summary>
  <font color=FF0000>给函数多个参数<font size=4>设置默认值实际上跟按顺序定义变量一样</font>，所以<font size=4> 会存在暂时性死区</font></font>的问题，即<mark>前面定义的变量不能引用后面还未定义的变量，而后面的可以访问前面的</mark>。
</details>

##### 



```js
let a = b = 10
;(function(){ 
  let a = b = 20 
})()
console.log(a)
console.log(b)
```

这题做的时候，<font color=FF0000>错了</font>。看答案发现了知识盲区，<font color=FF0000>非常重要</font> ⭐️⭐️

<details>
  <summary>点击查看答案</summary>
  10, 20
</details>

<details>
  <summary>点击查看解析</summary>
  <font color=FF0000>连等操作是从右向左执行的</font> <mark>（注：联等？不是赋值操作么？应该是连续赋值吧？另外，”从右到左运行“有前提：优先级相等，以及结合性一致，才是从右到左。参考：juejin.cn/post/6844903928073568264 ）</mark>，<font color=FF0000 size=4>相当于b = 10、let a = b</font>，很明显b没有声明就直接赋值了，所以会隐式创建为一个全局变量，函数内的也是一样，并没有声明b，直接就对b赋值了，因为作用域链，会一层一层向上查找，找了到全局的b，所以全局的b就被修改为20了，而函数内的a因为重新声明了，所以只是局部变量，不影响全局的a，所以a还是10。
</details>



```js
var a = {n:1}
var b = a
a.x = a = {n:2}
console.log(a.x)
console.log(b.x)
```

这题是真的不会，结果也是大大超出预期。⭐️⭐️⭐️

<details>
  <summary>点击查看答案</summary>
  undefined, {n: 2}
</details>

<details>
  <summary>点击查看解析</summary>
  按照网上大部分的解释是：<font color=FF0000 size=4>因为.运算符优先级最高，所以会先执行a.x</font> <mark>（注：这里 成员运算符的优先级最高，先于赋值运算符执行 ）</mark>，<font color=FF0000 size=4>此时a、b共同指向的 { n: 1 } 变成了 { n: 1, x: undefined }</font>，然后按照连等操作从右到左执行代码，a = { n: 2 }，显然，a现在指向了一个新对象，然后 a.x = a，因为 a.x 最开始就执行过了，所以这里其实等价于：({ n: 1, x: undefined }).x = b.x = a = { n: 2 }。<br>
  到这里教程就没有讲了，接下来是我自己的理解：a = { n: 2 }，就是将 a 指向了新的地址，而 b 依然还指向 a 之前的地址<mark>（注：示例代码</mark>
  <code> let a = { n: 1}; let b = a; a = { n: 2 }; console.log(a, b) </code>
  <mark>结果为：a = { n: 2 }, b = { n: 1 } ）</mark>。这个时候，最前面的 a.x（该地址所对应的值是 { n: 1, x: undefined }，这时候只有b在指向它。 ）以及不会对a产生影响（对a.x赋值）了(因为找不到对a的引用了)，只会对b.x赋值；所以 b.x = {n: 2} 也就 b.x 指向 “此时的 a” (a 即 {n: 2} )。具体如下图：
  <img src="https://s2.loli.net/2022/03/14/SjJ6XUxovNcPF7W.png" />
</details>



```js
var arr = [0, 1, 2]
arr[10] = 10
console.log(arr.filter(function (x) {
  return x === undefined
}))
```

这题错了，没想到还有这种说法...

<details>
  <summary>点击查看答案</summary>
  []
</details>

<details>
  <summary>点击查看解析</summary>
  这题比较简单，arr[10]=10，那么索引3到9位置上都是undefined，arr[3]等打印出来也确实是undefined，但是，这里其实涉及到<font color=FF0000>ECMAScript 版本不同对应方法行为不同的问题</font>，<font color=FF0000 size=4>ES6之前的遍历方法都会跳过数组未赋值过的位置，也就是空位</font><mark>（注：比如 for in，经过测试确实是 ）</mark>，但是 ES6 新增的 for of方法就不会跳过。
</details>



```js
var name = 'World'
;(function () {
  if (typeof name === 'undefined') {
    var name = "Jack"
    console.info('Goodbye ' + name)
  } else {
    console.info('Hello ' + name)
  }
})()
```

这题错了，事后分析原因是粗心。不过，仍然感觉有价值。

<details>
  <summary>点击查看答案</summary>
  这道题考察的是变量提升的问题，var声明变量时会把变量自动提升到当前作用域顶部，所以函数内的name虽然是在if分支里声明的，但是也会提升到外层，因为和全局的变量name重名，所以访问不到外层的name，最后因为已声明未赋值的变量的值都为undefined，导致if的第一个分支满足条件。
</details>



### （隐式）类型转换

##### 第一题

```js
console.log(1 + null)
console.log(1 + {})          
console.log(1 + [])
console.log([] + {})
```

第一个：做对了，但属于模糊；后面三个属于不会。

<details>
  <summary>点击查看答案</summary>
  1, 1[object Object], 1, [object Object]
</details>

<details>
  <summary>点击查看解析</summary>
  这道题考察的显然是+号的行为：<br>
  1.如果<font color=FF0000>有一个操作数是字符串</font>，那么<font color=FF0000 size=4>把另一个操作数转成字符串执行连接</font><br>
  2.如果 <font color=FF0000 size=4> 有一个操作数是对象</font>，那么<font color=FF0000 size=4>调用对象的valueOf方法转成原始值</font>，<font color=FF0000 size=4>如果没有该方法或调用后仍是非原始值，则调用toString方法</font><br>
  3.<font color=FF0000>其他情况下，两个操作数都会被转成数字执行加法操作</font><br>
  注：这里要注意顺序。
</details>

##### 第二题

```js
var a={},
    b={key:'b'},
    c={key:'c'}
a[b]=123
a[c]=456
console.log(a[b])
```

这题没看懂题意。时候感觉：这题和上一题很类似

<details>
  <summary>点击查看答案</summary>
  456
</details>

<details>
  <summary>点击查看解析</summary>
  对象有两种方法设置和引用属性，obj.name 和 obj['name']，方括号里可以字符串、数字和变量设置是表达式等，但是最终计算出来得是一个字符串，对于上面的 b 和 c，它们两个都是对象，所以会调用 toString() 方法转成字符串，对象转成字符串和数组不一样，和内容无关，结果都是[object Obejct]，所以a[b]=a[c]=a['[object Object]']。
</details>



### this 的 指向

##### 第一题

```js
var out = 25
var inner = {
  out: 20,
  func: function () {
    var out = 30
    return this.out
  }
};
console.log((inner.func, inner.func)())
console.log(inner.func())
console.log((inner.func)())
console.log((inner.func = inner.func)())
```

这题属于不会。同时，这题建议在浏览器 devtools 中运行，不要在 node 中运行。因为，如果 this 指向 undefined，浏览器会将其指向 window，而 node 不会。

# // TODO



### 赋值问题

##### 第一题

```js
let {a,b,c} = { c: 3, b: 2, a: 1 }
console.log(a, b, c)
```

这题做对了，但并不是非常确定。

<details>
  <summary>点击查看答案</summary>
  1, 2, 3
</details>

<details>
  <summary>点击查看解析</summary>
  这题考察的是变量解构赋值的问题，<font color=FF0000>数组解构赋值是按位置对应的，而对象只要变量与属性同名，顺序随意</font>。
</details>

##### 第二题

```js
console.log( Object.assign([1, 2, 3], [4, 5]) )
```

这题做错了，很懵...因为用法很奇怪。

<details>
  <summary>点击查看答案</summary>
  [4, 5, 3]
</details>

<details>
  <summary>点击查看解析</summary>
  是不是从来没有用assign方法合并过数组？<font color=FF0000>assign方法可以用于处理数组，不过会把数组视为对象</font>，比如<font color=FF0000 size=4>这里会把目标数组视为是属性为0、1、2的对象</font>，<font color=FF0000>所以源数组的0、1属性的值覆盖了目标对象的值</font>。
</details>



### 运算问题

```js
var x=1
switch( x++ ) {
  case 0: ++x
  case 1: ++x
  case 2: ++x
}
console.log(x)
```

本题错了，但是本题想考察的内容，我想对了。还是对 switch case 的运行逻辑有误解...

<details>
  <summary>点击查看答案</summary>
  4
</details>

<details>
  <summary>点击查看解析</summary>
  这题考查的是自增运算符的前缀版和后缀版，以及switch的语法，后缀版的自增运算符会在语句被求值后才发生，所以x会仍以1的值去匹配case分支，那么显然匹配到为1的分支，此时，x++生效，x变成2，再执行++x，变成3，因为没有break语句，所以会进入当前case后面的分支，所以再次++x，最终变成4。<br>
  注：这题我是错在：switch case 是找到第一个<font color=FF0000 size=4>匹配的case</font>，如果没有break，会暴力遍历接下来的所有case，逐个匹配。而我以为是：全部遍历，所有的逐个匹配。所以当时错误答案会是5
</details>



### 原型与原型链

```js
console.log(typeof undefined == typeof NULL)
console.log(typeof function () {} == typeof class {})
```

这题两个都错了，第一个是陷入了出题人的陷阱；第二个是知识盲区。

<details>
  <summary>点击查看答案</summary>
  true, true
</details>

<details>
  <summary>点击查看解析</summary>
  1.首先不要把 NULL 看成是null，<font color=FF0000>js 的关键字是区分大小写的</font>，所以这就是一个普通的变量，而且没有声明，typeof 对没有声明的变量使用是不会报错的，返回 'undefined'，typeof对 undefined 使用也是 'undefined'，所以两者相等
  2.<font color=FF0000>typeof对函数使用返回'function'，<font size=4>class只是es6新增的语法糖，本质上还是函数</font>，所以两者相等</font>
</details>

