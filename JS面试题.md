# js 面试题



### 函数参数相关

<details>
  <summary>这类题重点 / 易错点 总结：</summary>
  参数相当于函数内部的变量（详见 第一题）。<br>
  给参数赋默认值时，是有顺序的，同时也是有类型死区的。（详见 第二题）
</details>
##### 函数参数第1题

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

##### 函数参数第2题

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
##### 函数参数第3题

```js
console.log(['1','2','3'].map(parseInt));
```

<details>
  <summary>点击查看答案</summary>
  [1, NaN, NaN]
</details>

##### 解析

Array.prototype.map() 的 语法如下：

> ```js
> var new_array = arr.map(function callback(currentValue[, index[, array]]) {
>  // Return element for new_array 
> }[, thisArg])
> ```

可知，map 中的 callback，除了必传的 currentValue 外，还有选传的 index 和 array。

而 parseInt 的语法如下：

> ```js
> parseInt(string, radix);
> ```
>
> **参数：**
>
> - **string：**要被解析的值。<font color=FF0000>如果参数不是一个字符串，则将其转换为字符串（使用  ToString 抽象操作）</font>。<font color=FF0000>字符串开头的空白符将会被忽略</font>。
> - **radix：**可选，<font color=FF0000>从 2 到 36</font>；表示字符串的基数。例如指定 16 表示被解析值是十六进制数。请注意，10 不是默认值！<mark>文章后面的“描述” 解释了当参数 radix 不传时该函数的具体行为</mark>。另外，根据 [高频网红面试题['1','2','3'].map(parseInt) 原理解析](https://juejin.cn/post/6844903781369380872)  评论区的说法：之所以 radix 最大是 36，是因为 10个数字 + 26个字母。
>
> **返回值：**
>
> 从给定的字符串中解析出的一个整数，或者 NaN。<font color=FF0000>当 radix 小于 2 或大于 36 ，或第一个非空格字符不能转换为数字，为NaN</font>。注：radix 的范围，不仅仅是 2 - 36；根据下面的 “描述”，还有 0、undefined 或 未指定 radix，也是可行的（见下面）。
>
> **描述：**
>
> <font color=FF0000>如果 parseInt 遇到的字符不是指定 radix 参数中的数字</font>，它 <font color=FF0000>将忽略该字符以及所有后续字符，并返回 **到该点为止已解析的整数值**</font>
>
> <font color=FF0000>**parseInt 可以理解两个符号**</font>。+ 表示正数，- 表示负数（从ECMAScript 1开始）。它是在去掉空格后作为解析的初始步骤进行的。如果没有找到符号，算法将进入下一步；否则，它将删除符号，并对字符串的其余部分进行数字解析。
>
> ##### <font color=FF0000>如果 radix 是 undefined、0或未指定的，JavaScript会假定以下情况</font>：
>
> 1. 如果输入的 string以 "0x"或 "0x"（一个0，后面是小写或大写的X）开头，那么radix被假定为16，字符串的其余部分被当做十六进制数去解析。
> 2. 如果输入的 string以 "0"（0）开头， radix被假定为 8（八进制）或10（十进制）。具体选择哪一个radix取决于实现。ECMAScript 5 澄清了应该使用 10 (十进制)，但不是所有的浏览器都支持。因此，在使用 parseInt 时，一定要指定一个 radix。
> 3. 如果输入的 string 以任何其他值开头， radix 是 10 (十进制)。

由于这里 是 map 直接调用 parseInt，而不是 map 调用 包裹 parseInt 的函数（间接调用 parseInt ）；所以，这里对于 parseInt 的调用第二个参数 index，使用 数组 ['1', '2', '3'] 中元素的索引。所以，代码相当于：

```js
parseInt('1', 0, ['1', '2', '3']) // '1'为currentValue，0 为 index，['1', '2', '3'] 为 thisArg。下同
parseInt('2', 1, ['1', '2', '3'])
parseInt('3', 2, ['1', '2', '3'])
```

第一种情况：radix 为 0，参考上面 “如果 radix 是 undefined、0或未指定的” 的情况，所以 radix 为 10，没问题；结果为 1
第二种情况：radix 为 1，由于不为 0，也不在 2 - 36 之间，所以 为 NaN
第三种情况：radix 为 2，由于 string 为 “3” 不在 二进制的可用数字范围内，所以也为 NaN

本题摘自：[高频网红面试题['1','2','3'].map(parseInt) 原理解析](https://juejin.cn/post/6844903781369380872) 其中也有解析。




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
  另外，这题也有相关的博文进行解答：https://cloud.tencent.com/developer/article/1093667
</details>



### 函数

##### 函数第1题


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



### 变量 / 函数提升

##### 提升第1题


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
##### 提升第2题

```js
console.log(a)
var a = 1
var getNum = function() { a = 2 }
function getNum() { a = 3 }
console.log(a)
getNum()
console.log(a)
```

这一题，第三个输出错了。其他没什么问题，只有第三个输出，涉及到的原理是知识盲区。

<details>
  <summary>点击查看答案</summary>
  undefined、1、2
</details>
<details>
  <summary>点击查看解析</summary>
  首先因为 var 声明的变量提升作用，所以 a 变量被提升到顶部，未赋值，所以第一个打印出来的是 undefined。<br>
  接下来 <font color=FF0000>是函数声明和函数表达式</font><mark>（注：var getNum = function() {} 是函数表达式 ）</mark><font color=FF0000>的区别</font>，<font color=FF0000 size=4>函数声明会有提升作用，在代码执行前就把函数提升到顶部，在执行上下文上中生成函数定义，所以第二个getNum会被最先提升到顶部</font><mark>（注：这里是变量和声明都提升到顶部）</mark>，然后是 var 声明 getNum 的提升，但是因为 getNum 函数已经被声明了，所以就不需要再声明一个同名变量，接下来开始执行代码，执行到 var getNum = fun... 时，虽然声明被提前了，但是赋值操作还是留在这里，所以 getNum 被赋值为了一个函数，下面的函数声明直接跳过，最后，getNum 函数执行前 a 打印出来还是 1，执行后，a 被修改成了 2，所以最后打印出来的2。<br>
  运行时代码：function getNum() { a = 3 }; var a = undefined; var getNum = undefined; console.log(a); a = 1; getNum = function() { a = 2 }; console.log(a); getNum(); console.log(a);<br>
  另外：根据 《现代JS教程 - 变量作用域，闭包》的说法：<mark>这种行为（函数提升）仅适用于函数声明，而不适用于我们将函数分配给变量的函数表达式，例如 let say = function(name)...</mark>
</details>
##### 提升第3题

```js
var a = 1
function a(){}
console.log(a)
/////////////////////
var b
function b(){}
console.log(b)
////////////////////
function b(){}
var b
console.log(b)
```

这题，第一个对了，下面两个错了。另外，这题可以参考下 [[JS 机制与实现原理#变量对象的思考题 第二题]] 

<details>
  <summary>点击查看答案</summary>
  1、b函数本身、b函数本身
</details>
<details>
  <summary>点击查看解析</summary>
  这三小题都涉及到函数声明和var声明，这两者都会发生提升，但是函数会优先提升，所以<font color=FF0000 size=4>如果变量和函数同名的话，<strong>变量的提升就忽略了</strong></font>。<strong>注：</strong>可以理解为：同名的函数和变量，变量将不会提升<br>
  1. 提升完后，执行到赋值代码，a被赋值成了1，函数因为已经声明提升了，所以跳过，最后打印a就是1。对应代码：function a() {}; var a = undefined; a = 1; console.log(a);<br>
  2. 和第一题类似，只是b没有赋值操作，那么执行到这两行相当于都没有操作，b当然是函数。<br>
  3. 和第二题类似，只是先后顺序换了一下，但是并不影响两者的提升顺序，仍是函数优先，同名的var声明提升忽略，所以打印出b还是函数。<br>
</details>

**补充：** [[JS 机制与实现原理#变量对象的思考题 第二题]]  中有这样的问题：

```js
console.log(foo);
function foo(){
  console.log("foo");
}
var foo = 1;
```

和第一种情况同样的道理，变量提升被忽略。




### （隐式）类型转换

##### 类型转换第1题

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

##### 类型转换第2题

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

##### this 指向第1题

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

<details>
  <summary>点击查看答案</summary>
  25、20、20、25
</details>

<details>
  <summary>点击查看解析</summary>
  这道题考察的是this指向问题：<br>
  1. 逗号操作符会返回表达式中的最后一个值，这里为 inner.func 对应的函数，注意是函数本身，然后执行该函数，该函数并不是通过对象的方法调用，而是在全局环境下调用，所以 this 指向 window，打印出来的当然是 window 下的 out<br>
  2. 这个显然是以对象的方法调用，那么 this 指向该对象<br>
  3. 加了个括号，看起来有点迷惑人，但实际上 (inner.func) 和 inner.func 是完全相等的，所以还是作为对象的方法调用<br>
  4. 赋值表达式和逗号表达式相似，都是返回的值本身，所以也相对于在全局环境下调用函数<br>
</details>

**注：**这条在复习了 coderwhy 和 冴羽 的 this 文章后，重新做了遍，对了。

##### this 指向第2题

```js
var obj = {
  name: 'abc',
  fn: () => {
    console.log(this.name)
  }
};
obj.name = 'bcd'
obj.fn()
```

这题不记得是对还是错了，但是记得当时很懵...当然，这题很简单，不应该懵

<details>
  <summary>点击查看答案</summary>
  undefined
</details>

<details>
  <summary>点击查看解析</summary>
  这道题考察的是this的指向问题，<font color=FF0000>箭头函数执行的时候上下文是不会绑定 this 的，所以它里面的 this 取决于外层的 this，这里函数执行的时候外层是全局作用域</font>，所以 this 指向 window，window 对象下没有 name 属性，所以是 undefined。
</details>

##### this 指向第3题

```js
function fn (){ 
  console.log(this) 
}
var arr = [fn]
arr[0]()
```

这题对了，但是很模糊。

<details>
  <summary>点击查看答案</summary>
  arr本身
</details>

<details>
  <summary>点击查看解析</summary>
  <font color=FF0000 size=4><strong>函数作为某个对象的方法调用，this 指向该对象</strong></font>，数组显然也是对象，只不过我们都习惯了对象引用属性的方法：obj.fn，但是实际上obj['fn']引用也是可以的。
</details>
另外， coderwhy 的文章[前端面试之彻底搞懂this指向](https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA) 中也有不错的面试题，在 [[前端面试点总结#this 相关面试题]] 中有做题目，并解释答案。



### 赋值问题

##### 赋值第1题

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

##### 赋值第2题

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
##### 赋值第3题

```js
const obj = {
  a: { a: 1 }
};
const obj1 = {
  a: { b: 1 }
};
console.log(Object.assign(obj, obj1))
```

这题不清楚有没有做对，反正当时很不确定。这题只要知道 assign 的属性之后，很清楚。

<details>
  <summary>点击查看答案</summary>
  {a: { b: 1 } }
</details>

<details>
  <summary>点击查看解析</summary>
  这道题很简单，因为 <font color=FF0000>assign 方法执行的是 浅拷贝</font>，所以源对象的a属性会直接覆盖目标对象的 a 属性。
</details>
##### 赋值第4题

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


### 运算问题

##### 运算第1题

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


### 继承

##### 继承第1题

```js
const person = {
	address: {
		country:"china",
		city:"hangzhou"
	},
	say: function () { console.log(`it's ${this.name}, from ${this.address.country}`) },
	setCountry:function (country) { this.address.country = country }
}

const p1 = Object.create(person)
const p2 = Object.create(person)

p1.name = "Matthew"
p1.setCountry("American")

p2.name = "Bob"
p2.setCountry("England")

p1.say()
p2.say()
```

<details>
  <summary>点击查看答案</summary>
  it's Matthew, from England<br>
  it's Bob, from England
</details>

<details>
  <summary>点击查看解析</summary>
  原型式继承 和 原型链继承 一样，引用类型的属性被所有实例共享。所以，这里「引用类型」的数据 address 会被共享。而 name 不是引用类型的数据，所以不会共享。
</details>



### 作用域 和 闭包

##### 闭包第1题

```js
var i = 1
function b() {
  console.log(i)
}
function a() {
  var i = 2
  b()
}
a()
```

错了。这题是很经典的闭包题目了，不该错。

<details>
  <summary>点击查看答案</summary>
  1
</details>

<details>
  <summary>点击查看解析</summary>
  这道题考察的是作用域的问题，<font color=FF0000>作用域其实就是一套变量的查找规则</font>，<font color=FF0000 size=4><strong>每个函数在执行时都会创建一个执行上下文，其中会关联一个变量对象，也就是它的作用域，上面保存着该函数能访问的所有变量</strong></font><mark>（<strong>注：</strong>这里的作用域链很重要，在笔记的其他地方也有提及）</mark>，另外，<font color=FF0000 size=4><strong>上下文中的代码在执行时还会创建一个作用域链，如果某个标识符在当前作用域中没有找到，会沿着外层作用域继续查找，直到最顶端的全局作用域</strong></font>，因为 <font color=FF0000 size=4><strong>js 是词法作用域，（标识符）在写代码阶段作用域就已经确定了，换句话说，是在函数定义的时候确定的，而不是执行的时候</strong></font>，所以a函数是在全局作用域中定义的，虽然在 b 函数内调用，但是它只能访问到全局的作用域而不能访问到 b 函数的作用域。
</details>
##### 闭包第2题

```js
function test(fn) {
  const a = 1
  fn()
}

const a = 2
function fn() { console.log('a', a)}

test(fn)
```

这题来自：[快速掌握 JS 面试题之『作用域和闭包』- 闭包](https://www.bilibili.com/video/BV1Kv411778c?p=2)。这里要注意的是，哪里形成了闭包。另外，这题不应该错！




### 事件队列

<details>
  <summary>一些重点</summary>
  process.nextTick 的优先级要高于微任务。关于为什么，参见 [[前端面试点总结#事件循环 event loop#Node 的 Event Loop]] 中的内容
</details>

##### 事件队列第1题

```js
setTimeout(function() {
  console.log(1)
}, 0)
new Promise(function(resolve) {
  console.log(2)
  for( var i=0 ; i<10000 ; i++ ) {
    i == 9999 && resolve()
  }
  console.log(3)
}).then(function() {
  console.log(4)
})
console.log(5)
```

这题做对了，但是 对于 3要不要打印，不清楚。也就是 resolve() 后 是不是直接返回不确定。

<details>
  <summary>点击查看答案</summary>
  2 3 5 4 1
</details>

<details>
  <summary>点击查看解析</summary>
</details>
##### 事件队列第2题

```js
console.log('1');

setTimeout(function() {
  console.log('2');
  process.nextTick(function() {
    console.log('3');
  });
  new Promise(function(resolve) {
    console.log('4');
    resolve();
  }).then(function() {
    console.log('5');
  });
}); 

process.nextTick(function() {
  console.log('6');
});

new Promise(function(resolve) {
  console.log('7');
  resolve();
}).then(function() {
  console.log('8');
});

setTimeout(function() {
  console.log('9');
  process.nextTick(function() {
    console.log('10');
  }) 
  new Promise(function(resolve) {
    console.log('11');
    resolve();
  }).then(function() {
    console.log('12')
  });
})
```

**注：**这题文章的答案 和 实际运行的结果有出入；应该按实际运行结果为准。这里就不放答案了，自行运行即可。

另外，2022/5/9 又做了一遍，对了。

##### 事件队列第3题

```js
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
```

**注：**这题错了，有点意外；但是说明还是存在知识盲点：promise 中 resolve() 之后的函数是会执行的，也是同步的；reject 之后的函数就不会执行了
