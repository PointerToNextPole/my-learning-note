# JS 机制与实现原理



### 常见垃圾回收算法与 JavaScript 中垃圾回收原理

根据不同的需求而有了不同的取舍，从而产生了不同的算法。不论哪个垃圾回收算法，都有一套共同的流程：

1. 标记内存空间中的活动对象（在使用中的对象）和非活动对象（可以回收的对象）。
2. 删除非活动对象，释放内存空间。
3. 整理内存空间，避免频繁回收后产生的大量内存碎片（不连续内存空间）。

#### 常见的 垃圾回收算法

- **标记-清除法**

  标记-清除法由 John McCarthy（**注：**即是那位“约翰·麦肯锡”） 于 1960 年发表的一篇论文提出，其主要分两个阶段：

  1. 第一阶段是标记，从一个 GC root 集合出发，沿着「指针」找到所有对象，将其标记为活动对象。
  2. 第二阶段是清除，将内存中未被标记的对象删除，释放内存空间。

  <img src="https://mmbiz.qpic.cn/mmbiz_png/3xDuJ3eiciblma6w3Nia143DPDV4EqRemzhKRPW4tMtCdctjOS2C5cO3MW3SiaR3hcC7zicfU8RPicAOm08mekv4yZXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:70%;" />

  从上面的描述来看，<mark>标记-清除算法可以说是非常简单的</mark>，现在的各类垃圾回收算法也都是它的思想的延续。

  <mark>虽然简单，但其也有着很明显的缺点</mark>，即<font color=FF0000>在多次回收操作后，会产生大量的内存碎片，由于算法没有再整理内存空间，内存空间将变得很碎</font>，此时<mark>如果需要申请一个较大的内存空间，即使剩余内存总大小足够，也很容易因为没有足够的连续内存而分配失败</mark>。

  **注：**这个感觉 有点类似“位示图”。另外，感觉缺陷在于：没有考虑“（定期）内存资源整理”。

- **复制算法**

  为了解决以上问题，Marvin L. Minsky 在 1963 年提出了著名的 **「复制算法」** ：

  1. <mark>将整个空间平均分成 from 和 to 两部分</mark>。

  2. 先在 from 空间进行内存分配，当空间被占满时，标记活动对象，并将其复制到 to 空间。（**注：**感觉是先在 from 空间中随意使用空间，当空间不足（无法放下一个新的任务）时，再进行整理并放入 to 空间）

  3. 复制完成后，将 from 和 to 空间互换。

  <img src="https://s2.loli.net/2022/04/01/RAK8wbHrWIdkl3j.png" alt="图片" style="zoom:70%;" />

  由于直接将活动对象复制到另一半空间，没有了清除阶段的开销，所以能在较短时间内完成回收操作，并且每次复制的时候，对象都会集中到一起，相当于同时做了整理操作，避免了内存碎片的产生。

  虽然复制算法有吞吐量高、没有碎片的优点，但其缺点也非常明显。**首先**，<font color=FF0000>复制操作也是需要时间成本的，若堆空间很大且活动对象很多，则每次清理时间会很久</font>。**其次**，<font color=FF0000>将空间二等分的操作，让可用的内存空间直接减少了一半</font>。

- **引用计数**

  该算法由 George E. Collins 于 1960 年提出，主要操作为：

  1. 实时统计指向对象的引用数（指针数量）。

  2. 当引用数为 0 时，实时回收对象。

  <img src="https://s2.loli.net/2022/04/01/UfDnaNpCo6YXGrJ.png" alt="图片" style="zoom:75%;" />

  该算法可以即时回收垃圾数据，（**优点**）<font color=FF0000>对程序的影响时间很短，效率很高。高性能、实时回收</font>，看似完美的方案其实也有个**问题**，<font color=FF0000>当对象中存在循环引用时，由于引用数不会降到 0，所以对象不会被回收</font>。

  <img src="https://mmbiz.qpic.cn/mmbiz_png/3xDuJ3eiciblma6w3Nia143DPDV4EqRemzhazhF7co1Uz57TJHaaKmJ4qKbeic0wl99jeLIAvVBjjvvceXb8hLcJYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:75%;" />

上面三大算法的出现，基本奠定了垃圾回收的根本性内容，后续出现的垃圾回收算法，基本都是基于上面三个算法的取舍和组合。

- **标记-压缩算法**

  该算法于 1970 年出现，其<font color=FF0000>**结合了 标记-清除法 和 复制算法 的优点**</font>，主要操作如下：

  1. 从一个 GC root 集合出发，<font color=FF0000>标记所有活动对象</font>。

  2. <font color=FF0000>将所有活动对象移到内存的一端，集中到一起</font>。
  3. 直接清理掉边界以外的内存，释放连续空间。（**注：**直接移动指针即可）

  <img src="https://s2.loli.net/2022/04/01/6Qi9fNKctTHpPn3.png" alt="图片" style="zoom:70%;" />

  可以发现，该算法既避免了标记-清除法产生内存碎片的问题，又避免了复制算法导致可用内存空间减少的问题。当然，<mark>该算法也不是没有缺点的</mark>，<font color=FF0000>由于其清除和整理的操作很麻烦，甚至需要对整个堆做多次搜索，故而堆越大，耗时越多</font>。

- **代际假设和分代收集**

  **「代际假说」**

  > It has been empirically observed that in many programs, the most recently created objects are also those most likely to become unreachable quickly.
  >
  > 经过调查发现，大多数应用程序内的数据有以下两个特点：
  >
  > - 大多数对象的生命周期很短，很快就不再被需要了
  > - 那些一直存活的对象通常会存在很久

  简单讲就是对象的生存时间有点两极化的情况：

  ![图片](https://s2.loli.net/2022/04/01/6Q7bv9MTnaeZOdC.png)

  **「分代收集」：** 所以可以将对象进行分代，从而 <font color=FF0000>对不同分代实施不同的垃圾回收算法，以达到更高的效率</font>（如 Java GC: https://plumbr.io/handbook/garbage-collection-in-java/generational-hypothesis）。

#### JavaScript 垃圾回收

JavaScript 的 <font color=FF0000 size=4>**原始数据类型存在「栈」中，引用数据类型存在「堆」中**</font>（**注：**引用类型存在「堆」中，同时，<font color=FF0000>「栈」中存储堆内的地址的引用</font>），所以 **讨论 JavaScript 的垃圾回收，即讨论其「栈」中数据的回收以及「堆」中数据的回收**。

##### 栈中垃圾回收

> **ESP ( Extended <font color=FF0000>Stack</font> Pointer )：**扩展栈指针寄存器，用于存放函数栈顶指针。

<font color=FF0000>JavaScript 在 **执行函数时，会将其「上下文」压入栈中，ESP 上移**，而当函数执行完成后，其执行上下文可以销毁了</font>，<mark>此时仅需将 ESP 下移到下一个函数执行上下文即可，当下一个函数入栈时，会将 ESP 以上的空间直接覆盖</mark>。

所以 JavaScript 引擎是通过下移 ESP 来完成栈的垃圾回收的。

<img src="https://s2.loli.net/2022/04/01/sGRZhVHibCKQO3g.png" alt="图片" style="zoom:65%;" />

##### 堆中垃圾回收

不同于栈中的垃圾回收，**<font color=FF0000>「堆」</font>中的垃圾数据回收需要用到 <font color=FF0000>JavaScript 的垃圾回收器</font>**。

<font color=FF0000>JavaScript **「堆」中垃圾数据回收就使用到了「分代收集」的思想**</font>，引擎将堆空间分为 **「新生代 ( young-space ) 」** 和 **「老生代 ( old-space ) 」** ，并且<mark>对两个区域实施不同的垃圾回收策略</mark>。

- **「新生代」：** <font color=FF0000>新生代用于 **存放生存时间短的对象**，大多数新创建的小的对象都会被分配到该区域，该区域的垃圾回收会比较频繁</font>。

  在「新生代」中，引擎使用 Scavenge 算法 ( https://v8.dev/blog/trash-talk ) 进行垃圾回收，<font color=FF0000>即上面提到的「复制算法」</font>。

  <mark>其将「新生代」空间对半分为 “from-space” 和 “to-space” 两个区域</mark>。新创建的对象都被存放到 “from-space”，当空间快被写满时触发垃圾回收。先对 “from-space” 中的对象进行标记，完成后将标记对象复制到 “to-space” 的一端，然后将两个区域角色反转，就完成了回收操作。

  <font color=FF0000>**由于每次执行清理操作都需要复制对象，而复制对象需要时间成本，所以「新生代」空间会设置得比较小 ( 1~8M )**</font>。

- **「老生代」：** 老生代被用于<font color=FF0000>存放生存 “时间长的对象” 和 “大的对象”</font>：

  - 即<font color=FF0000>一些 **大的对象** 会被直接分配到老生代空间</font>
  - <font color=FF0000>**新生代中经过两次垃圾回收后仍然存活的对象，会晋升到老生代空间**</font>

  引擎在该（老生代）空间主要使用上面提到的 **「标记-压缩算法」** 。首先对活动对象进行标记，标记完成后，将所有存活对象移到内存的一段，然后清理掉边界外的内存。

由于 JavaScript 是单线程运行的，意味着<mark>垃圾回收算法和脚本任务在同一线程内运行，在执行垃圾回收逻辑时，后续的脚本任务需要等垃圾回收完成后才能继续执行</mark>。<font color=FF0000>若堆中的数据量非常大，一次完整垃圾回收的时间会非常长，将导致应用的性能和响应能力都直线下降</font>。

<font color=FF0000>为了避免垃圾回收影响应用的性能，V8 将标记的过程拆分成多个子标记</font>，<mark>让垃圾回收标记和应用逻辑交替执行，避免脚本任务等待较长时间</mark>。

摘自：[科普文：常见垃圾回收算法与 JS GC 原理](https://mp.weixin.qq.com/s/KZsgQxlrsfYMvJejbZqGHw)

##### 现代JS教程关于 GC 的补充

JavaScript 中主要的内存管理概念是 **可达性**。简而言之，“可达”值是那些以某种方式可访问或可用的值。它们一定是存储在内存中的。另外：对外引用不重要，只有传入引用才可以使对象可达。

**内部算法：**垃圾回收的基本算法被称为 “mark-and-sweep”（**注：**即上面所说的「标记」法）

**定期执行以下“垃圾回收”步骤：**

- 垃圾收集器找到所有的根，并“标记”（记住）它们。
- 然后它遍历并“标记”来自它们的所有引用。
- 然后它遍历标记的对象并标记 **它们的** 引用。所有被遍历到的对象都会被记住，以免将来再次遍历到同一个对象。
- ……如此操作，直到所有可达的（从根部）引用都被访问到。
- 没有被标记的对象都会被删除。

摘自：[现代js教程 - 垃圾回收](https://zh.javascript.info/garbage-collection)



### JS 中变量的存储

在百度上搜索「 JavaScript 变量存储」，能看到很多文章，无外乎一个结论：

> 对于原始类型，数据本身是存在栈内，对于对象类型，在栈中存的只是一个堆内地址的引用。

但是，突然想到一个问题：如果说原始类型存在在栈中，那么 JavaScript 中的闭包是如何实现的？（**注：**下面会说不合理之处）

##### 栈

栈是内存中一块用于存储局部变量和函数参数的线性结构，遵循着先进后出的原则。数据只能顺序的入栈，顺序的出栈。当然，栈只是内存中一片连续区域一种形式化的描述，数据入栈和出栈的操作仅仅是栈指针（**注：**栈指针寄存器）在内存地址上的上下移动而已。

栈的特点：轻量，不需要手动管理，函数调时创建，调用结束则消失。

##### 堆

堆可以简单的认为是一大块内存空间，就像一个篮子，你往里面放什么都没关系，但是篮子是私人物品，<font color=FF0000>操作系统并不会管你的篮子里都放了什么，也不会主动去清理你的篮子</font>，<mark>因此在 C 语言中，堆中内容是需要程序员手动清理的，不然就会出现内存溢出的情况</mark>。

既然堆是一个大大的篮子，那么<font color=FF0000>在栈中存储不了的数据（比如一个对象），就会被存储在堆中，**栈中就仅仅保留一个对该数据的引用（也就是该块数据的首地址）**</font>。

**注：**这里补充一张 NJU《计算机系统基础》课程的 PPT 截图：可以看到「栈」和「堆」在内存中的位置，以及他们两者的“数据存储方向”。另外，关于「栈」：「栈」又被称为栈帧 ( stack frame ) 结构，ESP ( Extended Stack Pointer ) 是栈顶指针寄存器（亦即，栈指针），EBP ( Extended Base Pointer ) 是栈底指针寄存器（亦即，帧指针）。

<img src="https://s2.loli.net/2022/04/01/1ESFxit6IGcrKmv.png" alt="image-20220401182634913" style="zoom: 33%;" />

**问题：**既然栈中数据在函数执行结束后就会被销毁，那么 JavaScript 中函数闭包该如何实现？先简单来个闭包：

```js
function count () {
    let num = -1;
    return function () {
        num++;
        return num;
    }
}

let numCount = count();
numCount(); // 0
numCount(); // 1
```

按照结论，num 变量在调用 count 函数时创建，在 return 时从栈中弹出。既然是这样的逻辑，那么调用 numCount 函数如何得出 0 呢？num 在函数 return 时已经在内存中被销毁了啊！

因此，<font color=FF0000>在本例中 JavaScript 的基础类型并不保存在栈中，而应该保存在堆中，供 numCount 函数使用</font>。那么网上大家的结论就是错的了？非也！接下来谈谈我对 JavaScript 变量存储的理解。

既然在 `JavaScript` 中有闭包的问题，抛开栈（`Stack`），仅用堆能否实现变量存储？我们来看一个特殊的例子：

```js
function test () {
    let num = 1;
    let string = 'string';
    let bool = true;
    let obj = {
        attr1: 1,
        attr2: 'string',
        attr3: true,
        attr4: 'other'
    }
    return function log() {
        console.log(num, string, bool, obj);
    }
}
```

伴随着 “test 函数” 的调用，为了保证变量不被销毁，在堆中先生成一个对象就叫 Scope 吧，把变量作为 Scope 的属性给存起来。堆中的数据结构大致如下所示（**注：**注意下图 Scope 和箭头的位置）：

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/11/15/16e6cfe8d94e1d84~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.image" alt="使用 Scope 保存变量" style="zoom:50%;" />

那么，这样就能解决闭包的问题了吗？当然可以。由于 Scope 对象是存储在堆中，因此返回的 log 函数完全可以拥有 Scope 对象 的访问。下图是该段代码在 Chrome 中的执行效果：

<img src="https://s2.loli.net/2022/04/01/BHMCzFkaQtRsdSJ.jpg" alt="Chrome 中 Scope 的表示" style="zoom:55%;" />

红框部分，与上述一致，同时也反应出了之前提及的问题：<font color=FF0000>例子中 JS 变量并没有存在栈中，而是在堆里，用一个特殊的对象 Scope 保存</font>。（**注：**这里还是很重要，第一次见到 <font color=FF0000>内部属性 \[\[scope]] 中的 Closure 元素，且其中保存着闭包内使用的所有的「自由变量」</font>。另外，这里打印使用的是 console.dir()，自行尝试了下：如果使用 console.log() 只能看到最外层的函数定义）

#### JS 中变量有三种类型

- 局部变量（ [[#局部变量]] ）
- 被捕获的变量（ [[#被捕获的变量]] ）
- 全局变量（ [[#全局变量]] ）

##### 局部变量

局部变量很好理解：<font color=FF0000>**在函数中声明**，且 **在函数返回后不会被其他作用域所使用** 的对象</font>。下面代码中的 `local*` 都是局部变量。

```js
function test () {
  let local1 = 1;
  var local2 = 'str';
  const local3 = true;
  let local4 = {a: 1};
  return;
}
```

##### 被捕获变量

<mark>被捕获变量就是 **局部变量的反面**</mark>：<font color=FF0000>在函数中声明，但在函数返回后仍有未执行作用域（函数或是类）使用到该变量，那么该变量就是被捕获变量</font>。下面代码中的 `catch*` 都是「被捕获变量」。**注：**应该可以理解为「自由变量」把？

```js
function test1 () {
  let catch1 = 1;
  var catch2 = 'str';
  const catch3 = true;
  let catch4 = {a: 1};
  return function () {
    console.log(catch1, catch2, catch3, catch4)
  }
}

function test2 () {
  let catch1 = 1;
  let catch2 = 'str';
  let catch3 = true;
  var catch4 = {a: 1};
  return class {
    constructor(){
      console.log(catch1, catch2, catch3, catch4)
    }
  }
}

console.dir(test1())
console.dir(test2())
```

复制代码到 Chrome 即可查看输出对象下的 \[\[Scopes]] 下有对应的 Scope。

##### 全局变量

全局变量就是 global，在 浏览器上为 window 在 node 里为 global。、<font color=FF0000>全局变量会被默认添加到「函数作用域链」的最低端</font>，<font color=FF0000>也就是上述函数中 \[\[Scopes]] 中的最后一个</font>。

全局变量需要特别注意一点：var 和 let / const 的区别：

- **var：**全局的 var 变量其实仅仅是为 global 对象添加了一条属性。毕竟，全局环境下的 var变量 会被挂载到「全局对象」上（即：window / gloabl）。

  **注：**根据 MDN 的说法，在全局作用域中定义的函数，同样会挂载到 全局对象下。示例如下：

  > ```js
  > function greeting() {
  >    console.log("Hi!");
  > }
  > 
  > window.greeting(); // It is the same as the normal invoking: greeting();
  > ```
  >
  > **解释：**全局函数 greeting 存储在 window 对象中，像这样：
  >
  > ```js
  > greeting: function greeting() {
  >    console.log("Hi!");
  > }
  > ```
  >
  > 摘自：[MDN - 全局对象](https://developer.mozilla.org/zh-CN/docs/Glossary/Global_object)

- **let / const：**全局的 let / const 变量不会修改 window 对象，而是<font color=FF0000>将变量的声明放在了一个特殊的对象下（与 Scope 类似）</font>。示例及在 Chrome Devtools 的运行结果如下：

  ```js
  let testLet = 1
  console.dir(() => {})
  ```

  <img src="https://s2.loli.net/2022/04/01/tWzVYAuJI2f6egD.png" alt="image-20220401220051892" style="zoom:55%;" />

#### 结论

根据上面的说法：<font color=FF0000>**除了局部变量，其他的**</font>（**注：**即，被捕获的变量、全局变量）<font color=FF0000>**全都存在「堆」中**</font>。**注：**即，「局部变量」存在「栈」中

根据变量的数据类型，分为以下两种情况：

- 如果是基础类型，那栈中存的是数据本身
- 如果是对象类型，那栈中存的是堆中对象的引用。

但这是理想情况，再问一个问题：<mark>JS 解析器如何判断一个变量是局部变量呢？</mark> <font color=FF0000>判断出是否被内部函数引用即可</font>（**注：**如果不被使用，则是「局部属性」）。那<mark>如果 JS 解析器并没有判断呢？</mark>那就只能存在堆里。

那么你一定想问，Chrome 的 V8 能否判断出？从结果看应该是可以的。

<img src="https://s2.loli.net/2022/04/01/Z1CcojehHLKIywz.jpg" alt="Chrome 下的局部变量" style="zoom:60%;" />

红框内仅有变量 a，而变量 b 已经消失不见了。**注：**这里 “变量a” 不是局部变量，存在「堆」中；“变量b” 是局部变量，存在「栈」中。

摘自：[JS 变量存储？栈 & 堆？NONONO!](https://juejin.cn/post/6844903997615128583)



### JS 执行上下文栈

**注：**在 [[前端面试点总结#词法环境 和 执行上下文的区别]] 中有讲述 「执行上下文」和「执行上下文栈」的内容，但是由于这部分内容太多，不该放到名为“前端面试点**总结**” 的笔记中，同时也比较忙；所以，这里先在这里做笔记，等有时间在将那边的内容搬（整理）过来。

##### 铺垫

如果要问到 JavaScript 代码执行顺序的话，想必写过 JavaScript 的开发者都会有个直观的印象，那就是顺序执行，毕竟：

```js
var foo = function () { console.log('foo1'); }
foo();  // foo1
var foo = function () { console.log('foo2'); }
foo(); // foo2
```

然而去看这段代码：

```js
function foo() { console.log('foo1'); }
foo();  // foo2
function foo() { console.log('foo2'); }
foo(); // foo2
```

打印的结果却是两个 `foo2`。因为 <font color=FF0000>JavaScript 引擎 **并非一行一行地分析和执行程序**，而是**一段一段地分析执行**</font>。<font color=FF0000>当执行一段代码的时候，会进行一个“准备工作”</font>，比如第一个例子中的变量提升，和第二个例子中的函数提升。

但是本文真正想让大家思考的是：<font color=FF0000>这个 “一段一段”中的 **“段” 究竟是怎么划分的** 呢？到底 JavaScript引擎 **遇到一段怎样的代码** 时**才会做“准备工作”**呢？</font>

##### 可执行代码

这就要说到 <font color=FF0000>**JavaScript 的可执行代码 ( executable code ) 的类型**</font> 有哪些了？

其实很简单，就<font color=FF0000>**三种，1) 全局代码、2) 函数代码、3) eval代码**</font>。如下是一些补充：

> **执行上下文总共有三种类型：**
>
> - **全局执行上下文：** <mark>这是默认的、最基础的执行上下文</mark>。<font color=FF0000>不在任何函数中的代码都位于全局执行上下文中</font>。**它做了两件事**：1. <font color=FF0000>创建一个全局对象</font>，在浏览器中这个全局对象就是 window 对象。2. <font color=FF0000>将 this 指针指向这个全局对象</font>。一个程序中只能存在一个全局执行上下文。
>
>   另外，<font color=FF0000 size=4>**一旦所有代码执行完毕，Javascript 引擎把「全局执行上下文」从「执行栈」中移除**</font>。
>
> - **函数执行上下文：** <mark>每次调用函数时，都会为该函数创建一个新的执行上下文。每个函数都拥有自己的执行上下文</mark>，<font color=FF0000>**但是只有在函数被调用的时候才会被创建**</font>。一个程序中可以存在任意数量的函数执行上下文。每当一个新的执行上下文被创建，它都会按照特定的顺序执行一系列步骤，具体过程将在本文后面讨论。
>
> - **Eval 函数执行上下文：** 运行在 eval 函数中的代码也获得了自己的执行上下文，但由于 JavaScript 开发人员不常用 eval 函数，所以在这里不再讨论。
>
> 摘自：[【译】理解 Javascript 执行上下文和执行栈](https://juejin.cn/post/6844903704466833421)

举个例子，当执行到一个函数的时候，就会进行准备工作，这里的“准备工作”，让我们用个更专业一点的说法，就叫做"执行上下文 ( execution context )"。

##### 执行上下文栈

接下来问题来了，我们写的函数多了去了，<font color=FF0000>如何管理创建的那么多执行上下文</font>呢？

所以 JavaScript 引擎创建了 <font color=FF0000>执行上下文**栈**</font> ( Execution context stack, ECS ) 来<font color=FF0000>管理执行上下文</font>

<mark>为了**模拟**执行上下文栈的行为，让我们定义执行上下文栈是一个数组</mark>：

```js
ECStack = [];
```

试想<font color=FF0000>当 JavaScript 开始要解释执行代码的时候，最先遇到的就是全局代码</font>，所以<font color=FF0000>初始化的时候首先就会向执行上下文栈压入一个全局执行上下文</font>，我们<font color=FF0000>用 globalContext 表示它</font>，并且<font color=FF0000>只有当整个应用程序结束的时候，ECStack 才会被清空</font>，所以<font color=FF0000>程序结束之前， ECStack 最底部永远有个 globalContext</font>：

```js
ECStack = [
    globalContext
];
```

现在 JavaScript 遇到下面的这段代码了：

```js
function fun3() { console.log('fun3') }
function fun2() { fun3(); }
function fun1() { fun2(); }
fun1();
```

<font color=FF0000>当 <font size=4>**执行** 一个函数的时候</font></font>（**注：**注意是 **执行**，不是初始化。另外，可以看下面 push 的顺序，是执行的 fun1 最先被 push；这样也起到了类似 “tree shaking” 的作用），<font color=FF0000>就会创建一个执行上下文，并且压入执行上下文栈</font>，<font color=FF0000>当函数执行完毕的时候，就会将函数的执行上下文从栈中弹出</font>。知道了这样的工作原理，让我们来看看如何处理上面这段代码：

```js
/* 伪代码 */

// fun1()
ECStack.push(<fun1> functionContext);

// fun1中竟然调用了fun2，还要创建fun2的执行上下文
ECStack.push(<fun2> functionContext);

// 擦，fun2还调用了fun3！
ECStack.push(<fun3> functionContext);

// fun3执行完毕
ECStack.pop();

// fun2执行完毕
ECStack.pop();

// fun1执行完毕
ECStack.pop();

// javascript接着执行下面的代码，但是ECStack底层永远有个globalContext
```

##### 《JavaScript深入之词法作用域和动态作用域》思考题解惑

另外，在文章[《JavaScript深入之词法作用域和动态作用域》](https://github.com/mqyqingfeng/Blog/issues/3)有这样的问题：

```js
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope(); // 注：未形成闭包
```

```js
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
checkscope()(); // 注：形成闭包
```

两段代码执行的结果一样，但是两段代码究竟有哪些不同呢？答案就是执行上下文栈的变化不一样。

模拟第一段代码：

```js
ECStack.push(<checkscope> functionContext);
ECStack.push(<f> functionContext);
ECStack.pop();
ECStack.pop();
```

模拟第二段代码：

```js
ECStack.push(<checkscope> functionContext);
ECStack.pop();              // 注：这里 checkscope 函数执行完毕，所以销毁了
ECStack.push(<f> functionContext);
ECStack.pop();
```

摘自：[JavaScript深入之执行上下文栈](https://github.com/mqyqingfeng/Blog/issues/4)

**注：**本文没讲到「执行上下文」中包含什么，这要在作者“连载”的其他文章中寻找答案（下面有做笔记 [[#JS 执行上下文]]）。虽然 [[前端面试点总结#词法环境 和 执行上下文的区别]] 中也有...



### JS 执行上下文

##### 对于每个执行上下文，都有三个重要属性

- 变量对象 ( Variable object，VO )
- 作用域<font color=FF0000 size=4>**链**</font> ( Scope chain ) 注：注意有个「链」字
- this

本文按照原博客的顺序而言，应该放在 变量对象（[[#JS 变量对象 （词法环境）]]）、作用域链（[[#JS 作用域链]]）、this 的后面，不过感觉「执行上下文」放在「执行上下文栈」的后面并不坏；同时，先父后子的结构，能让结构清晰些。有遗忘的再到后面查找即可。

在[《JavaScript深入之词法作用域和动态作用域》](https://github.com/mqyqingfeng/Blog/issues/3)中，提出这样一道思考题（**注：**由于源链接中并没有做太多讲解，所以下面的 [[#JS 词法作用域]] 中没有做摘抄；具体参见 [[#《JavaScript深入之词法作用域和动态作用域》思考题解惑]] ；另外，下面有以“执行上下文”为角度，更详细的讲解）

#### 具体执行分析

我们分析第一段代码：

```js
var scope = "global scope";
function checkscope(){
  var scope = "local scope";
  function f(){
      return scope;
  }
  return f();
}
checkscope();
```

##### 执行过程如下：

1. 执行全局代码，创建「全局执行上下文」，「全局上下文」被压入「执行上下文栈」

   ```js
   ECStack = [ // exec context stack
       globalContext
   ];
   ```

2. 全局上下文初始化

   ```js
   globalContext = {
       VO: [global], // 变量对象
       Scope: [globalContext.VO], // 作用域链
       this: globalContext.VO // this 的绑定
   }
   ```

   初始化的同时，checkscope 函数被创建，<font color=FF0000>保存「作用域链」到函数的 内部属性 \[\[scope]]</font>

   ```js
   checkscope.[[scope]] = [
       globalContext.VO
   ];
   ```

3. <font color=FF0000>**执行 checkscope 函数**</font>，创建 checkscope 函数「执行上下文」，checkscope 函数「执行上下文」被压入「执行上下文栈」

   ```js
   ECStack = [
       checkscopeContext,
       globalContext
   ];
   ```

4. checkscope 函数执行上下文初始化：

   1. <font color=FF0000 size=4>**复制函数 \[\[scope]] 属性创建作用域链**</font>
   2. 用 arguments 创建活动对象，
   3. 初始化活动对象，即加入形参、函数声明、变量声明。<mark>**注：**这里初始化 VO 的内容，可以看 [[# JS 变量对象 （词法环境）]] 部分，那里有详细的讲述。</mark>
   4. 将活动对象压入 checkscope 作用域链顶端。

   同时 f 函数被创建，保存作用域链到 f 函数的内部属性 \[\[scope]]

   ```js
   checkscopeContext = {
       AO: {
           arguments: {
               length: 0
           },
           scope: undefined,
           f: reference to function f(){}
       },
       Scope: [AO, globalContext.VO],
       this: undefined
   }
   ```

5. 执行 f 函数，创建 f 函数执行上下文，f 函数执行上下文被压入执行上下文栈

   ```js
   ECStack = [
       fContext,
       checkscopeContext,
       globalContext
   ];
   ```

6. f 函数执行上下文初始化, 以下跟第 4 步相同：

   1. 复制函数 [[scope]] 属性创建作用域链
   2. 用 arguments 创建活动对象
   3. 初始化活动对象，即加入形参、函数声明、变量声明
   4. 将活动对象压入 f 作用域链顶端

   ```js
   fContext = {
       AO: {
           arguments: {
               length: 0
           }
       },
       Scope: [AO, checkscopeContext.AO, globalContext.VO],
       this: undefined
   }
   ```

7. f 函数执行，沿着作用域链查找 scope 值，返回 scope 值

8. f 函数执行完毕，f 函数上下文从执行上下文栈中弹出

   ```js
   ECStack = [
       checkscopeContext,
       globalContext
   ];
   ```

9. checkscope 函数执行完毕，checkscope 执行上下文从执行上下文栈中弹出

   ```js
   ECStack = [
       globalContext
   ];
   ```

摘自：[JavaScript深入之执行上下文](https://github.com/mqyqingfeng/Blog/issues/8)



### ES6 中的执行上下文

#### 什么是执行上下文

简而言之，执行上下文就是当前 JavaScript 代码被解析和执行时所在环境的抽象概念， JavaScript 中运行任何的代码都是在执行上下文中运行。

> Simply put, an execution context is an abstract concept of an environment where the Javascript code is evaluated and executed. Whenever any code is run in JavaScript, it’s run inside an execution context.

#### 执行上下文的类型

执行上下文总共有三种类型：

- **全局执行上下文：** 这<mark>是默认的、最基础的执行上下文</mark>。不在任何函数中的代码都位于全局执行上下文中。<font color=FF0000>**它做了两件事**</font>：1. <font color=FF0000>**创建一个全局对象**</font>，在浏览器中这个全局对象就是 window 对象。2. <font color=FF0000>**将 this 指针指向这个全局对象**</font>。<font color=FF0000 size=4>**一个程序 中只能存在 一个全局执行上下文**</font>。

  另外，<font color=FF0000>一旦所有代码执行完毕，Javascript 引擎把全局执行上下文从执行栈中移除</font>。

- **函数执行上下文：** <mark>每次调用函数时，都会为该函数创建一个新的执行上下文</mark>。每个函数都拥有自己的执行上下文，但是只有在函数被调用的时候才会被创建。一个程序中可以存在任意数量的函数执行上下文。每当一个新的执行上下文被创建，它都会按照特定的顺序执行一系列步骤，具体过程将在本文后面讨论。

- **Eval 函数执行上下文：** 运行在 eval 函数中的代码也获得了自己的执行上下文，但由于 Javascript 开发人员不常用 eval 函数，所以在这里不再讨论。

#### 执行上下文是如何被创建的

执行上下文分两个阶段创建：**1）创建阶段；** **2）执行阶段**

#### 创建阶段

（在任意的 JavaScript 代码被执行前，执行上下文处于创建阶段）执行上下文在创建阶段产生。在创建阶段发生了两件事：

- **LexicalEnvironment（词法环境）** 组件被创建
- **VariableEnvironment（变量环境）** 组件被创建

因此，执行上下文可以在概念上表示如下：

```js
ExecutionContext = {
  LexicalEnvironment = <ref. to LexicalEnvironment in memory>,
  VariableEnvironment = <ref. to VariableEnvironment in  memory>,
}
```

#### 词法环境 LexicalEnvironment

[官方 ES6 文档](http://ecma-international.org/ecma-262/6.0/) 将词法环境定义为：

> 「词法环境」是一种<font color=FF0000>规范类型</font>，基于 ECMAScript 代码的词法嵌套结构来定义标识符与特定变量和函数的关联关系。<font color=FF0000 size=4>「词法环境」由环境记录 ( environment record ) 和可能为空引用 ( null ) 的「外部词法环境」组成</font>。

简而言之，<font color=FF0000>词法环境是一个包含 **「标识符 变量 <font size=4>映射</font>」** 的结构</font>。（这里的<font color=FF0000>**「标识符」**表示 变量 / 函数 的 **名称**</font>，**「变量」**是对 实际对象【包括函数类型对象、数组对象】或原始值的 引用）。**注：**变量是 实际对象 和 原始值 的引用。这里有点不太好断句，附上原文：

> Simply put, A *lexical environment* is a structure that holds **identifier-variable mapping**. (here **identifier** refers to the name of variables/functions, and the **variable** is the reference to actual object [including function object and array object] or primitive value).

举个例子，假设有如下的代码片段：

```js
var a = 20;
var b = 40;
function foo() {
  console.log('bar');
}
```

所以，上面的代码片段对应的 词法环境应该像这样：

```js
lexicalEnvironment = {
  a: 20,
  b: 40,
  foo: <ref. to foo function>
}
```

每一个「词法环境」由三个组件（ 原文是 components，不过翻译成 “部分“ 更容易理解些）组成

1. **环境记录** ( Environment Record )
2. **对外部环境的引用** ( Reference to the outer environment )
3. **确定 this 的值** ( **This binding** )

##### 环境记录 Environment Record

「环境记录」是「词法环境」中存储变量和函数定义的地方

**环境记录同样有两种类型：**

- **声明性环境记录 Declarative environment record：**正如它的名字所表明的，它是用来存储 变量 和 函数声明的；一个函数的词法环境包含一个「声明性环境记录」

  **译文中的总结：**「声明性环境记录」存储 变量、函数和参数。一个<font color=FF0000 size=4>**函数环境包含声明性环境记录**</font>。

- **对象环境记录 Object environment record：** 全局代码的「词法环境」包含 一个「对象环境记录」。除了变量和函数声明外，对象环境记录还存储全局绑定对象（浏览器环境下是 window 对象）。因此，每有一个绑定对象的属性（在浏览器环境下，「对象环境记录」包含浏览器向 window 对象提供的 属性 和 方法），就会在「对象环境记录」中创建一个新条目。

  **译文中的总结：**「对象环境记录」用于<font color=FF0000>定义在「全局执行上下文」中出现的 变量 和 函数 的**关联**</font>，<font color=FF0000 size=4>**「全局环境」包含「对象环境记录」**</font>（**注：**注意和上面「声明性环境记录」的对比）。

**注意：**对于函数，环境记录也包含一个 arguments 对象，其中包含着 索引 ( index )和 “传递给函数的实参”的映射，另外还有“实参个数”（即：length）。举个例子，一个下面的函数的 arguments 对象就像这样：

```js
function foo(a, b) { var c = a + b; }
foo(2, 3);

// argument object
Arguments: {0: 2, 1: 3, length: 2},
```

##### 对外部环境的引用 ( Reference to the outer environment )

「对外部环境的引用」意味着它可以访问其外部词法环境。如果，在当前「词法环境」中找不到变量，JS 引擎会到外层词法环境中寻找。

##### 确定 this 的值 ( This binding )

在这个组件 ( this binding ) 中，this 的值会被决定或设置。

- 在「全局执行上下文」中，this 的值指向全局对象（在浏览器环境下，this 的值指向 window 对象）。
- 在「函数执行上下文」中，this 的值取决于函数的调用方式。如果它被一个对象引用调用，那么 this 的值被设置为该对象，否则 this 的值被设置为全局对象或 undefined（严格模式下）

示例如下：

```js
const person = {
  name: 'peter',
  birthYear: 1994,
  calcAge: function() {
    console.log(2018 - this.birthYear);
  }
}

person.calcAge();  // 'this' refers to 'person', because 'calcAge' was called with 'person' object reference

const calculateAge = person.calcAge;
calculateAge(); // 'this' refers to the global window object, because no object reference was given
```

抽象地说，词法环境在「伪代码」中看起来像这样：

```js
GlobalExectionContext = { // 全局执行上下文
  LexicalEnvironment: {   // 词法环境
    EnvironmentRecord: {  // 环境记录
      Type: "Object",     // 环境记录的类型，这里是「对象环境记录」
      // 标识符绑定在这里
    }
    outer: <null>,        // 外部环境引用
    this: <global object> // this 的值
  }
}
FunctionExectionContext = { // 函数执行上下文
  LexicalEnvironment: {     // 词法环境
    EnvironmentRecord: {    // 环境记录
      Type: "Declarative", // 环境记录的类型，这里是「声明环境记录」
      // 标识符绑定在这里 
    }
    outer: <Global or outer function environment reference>, // 外部环境引用
    this: <depends on how function is called>                // this 的值
  }
}
```

#### 变量环境 VariableEnvironment

「变量环境」也是一个「词法环境」，它的「环境记录」包含了由  VariableStatements 在此执行上下文创建的绑定。

如上所述，变量环境也是一个词法环境，因此它具有上面所说的「词法环境」所被定义的所有属性。

在 ES6 中，LexicalEnvironment 组件和 VariableEnvironment 组件的区别在于：<font color=FF0000>**前者用于存储「函数声明」和变量（ let 和 const ）绑定**</font>，而<font color=FF0000>**后者仅用于存储变量（ var ）绑定**</font>。

#### 执行阶段

在此阶段，完成对所有变量的分配，最后执行代码。

**译文中的补充：**

> 注： 在执行阶段，如果 Javascript 引擎在源代码中声明的实际位置找不到 let 变量的值，那么将为其分配 undefined 值。

#### 执行上下文的示例

让我们结合一些代码示例来理解上述概念：

```js
let a = 20;  
const b = 30;  
var c;

function multiply(e, f) {  
 var g = 20;  
 return e * f * g;  
}

c = multiply(20, 30);
```

当上面的代码被执行，JS 引擎 创建了 一个「全局执行上下文」来执行全局环境的代码。所以，「全局执行上下文」在「创建阶段」将会像这样：

```js
GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 标识符绑定在这里 
      a: < uninitialized >,
      b: < uninitialized >,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 标识符绑定在这里 
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

在执行阶段，变量赋值已经完成。所以，在执行阶段，全局执行上下文将会像下面这样：

```js
GlobalExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 标识符绑定在这里 
      a: 20,
      b: 30,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // 标识符绑定在这里 
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

当遇到 函数 multiply(20, 30) 被调用时，一个新的「函数执行上下文」会被创建来执行函数代码。因此：在创建阶段，函数执行上下文看起来会像这样：

```js
FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 标识符绑定在这里
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 标识符绑定在这里 
      g: undefined
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

在这之后，执行上下文将经过「执行阶段」；这意味着对函数内的变量进行赋值已经完成。所以，在执行阶段，函数执行上下文看起来会像这样：

```js
FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      g: 20
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

当函数完成，返回值被保存在变量 c 中。所以，全局词法环境已经被更新。在这之后，全局代码完成，程序完毕。

**注意：**你或许注意到 let 和 const 定义的变量 在「创建阶段」没有任何关联的值。但是，var 定义的变量被设置为 undefined。

**这是因为：**<font color=FF0000>在「创建阶段」，代码会被扫描并解析 变量 和 函数声明</font>，其中函数声明存储在环境中，而变量会被设置为 undefined（在 var 的情况下）或 保持未初始化 uninitialized（在 let 和 const 的情况下）。

这就是为什么你可以在声明之前访问 var 定义的变量（尽管是 undefined ），但如果在声明之前访问 let 和 const 定义的变量就会提示引用错误的原因。

这就是我们所谓的变量提升。

**注意：**在「执行阶段」，如果 JS 引擎 不能找到 在源代码中在具体地方被定义的 let 变量的值，它将会被复制为 undefined。

摘自：[Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0) 及部分译文：[【译】理解 Javascript 执行上下文和执行栈](https://juejin.cn/post/6844903704466833421) 

**注：**上面的摘抄是阅文前端团队对本文进行了翻译，本来准备只摘抄译文的；但是发现译文和原文相差有点多（应该是原文之后修改了...），便还是看了原文，遇见相同的则摘抄译文。另外，在评论区看见了这个评论，解答了我为什么两者内容不一致：

> ES2018 里把 this 划分到 语法环境 里了，并且不仅仅有词法环境和变量环境，还有 code evaluation state，Function，ScriptOrModule，Realm，Generator

另外，评论区有人在问 “为什么没有看见 变量对象 概念了”，评论回复如下：

> <font color=FF0000>那（注：即 变量对象）是 ES3 里面的概念</font>了，<mark>基本上 **变量对象 VO** 相当于这里的 **全局词法环境 + 全局变量环境**</mark>，<font color=FF0000>推出「词法作用域」和「变量作用域」应该是为了更好得区分 var 与 let / const的区别</font>

#### 内部属性 和 词法环境

##### \[\[environment]] 相关

在《现代JS教程》中有说：

> 所有的函数在“诞生”时都会记住创建它们的词法环境。从技术上讲，这里没有什么魔法：<font color=FF0000>**<font size=4>所有函数</font> 都有名为 `[[Environment]]` 的隐藏属性**</font>，<font color=FF0000>**该属性 <font size=4>保存了对创建该函数的「词法环境」的 引用</font>**</font>。

在 《JS 忍者秘籍》（第二版）5.4.2 中：

> 无论何时创建函数，都会创建一个与之相关联的词法环境，并存储在名为 [[Environment]] 的内部属性上（也就是说无法直接访问或操作）。
>
> 无论何时调用函数，都会创建一个新的执行环境，被推入执行上下文栈。此外，还会创建一个与之相关联的词法环境。现在来看最重要的部分：<font color=FF0000>外部环境与新建的词法环境，**JavaScript 引擎 将调用函数的内置 [[Environment]] 属性与创建函数时的环境进行关联**</font>。
>
> 摘自：JavaScript忍者秘籍（第2版）5.4.2

博文 [温故而知新 - 重新认识JavaScript的Closure](https://segmentfault.com/a/1190000039786991) 中有说：

> 规范中关于[FunctionInitialize](https://link.segmentfault.com/?enc=k%2BighkKqljzLwV4Op6%2BqTQ%3D%3D.6dLKKfSD%2F8VuWQYB5UoCPFqpN4auFrWXlVP0Z8WdYkENynL%2FSnenyXgcSbWLOCgLX%2F49E2zETX5JvYltZ5TLfw%3D%3D)，可以看到 “4.Set F.[[Environment]] to Scope.“ ，看下规范关于 \[\[Environment]] 的说明
>
> > | Type                | Description                                                  |
> > | ------------------- | ------------------------------------------------------------ |
> > | Lexical Environment | The Lexical Environment that the function was closed over. Used as the outer environment when evaluating the code of the function. |

再者：在 stackoverflow 的问题：[What is the Javascript [[Environment\]] Property?](https://stackoverflow.com/questions/51748127/what-is-the-javascript-environment-property) 中有解答如下：

> Looks like [[Scope]] is an old name for [[Environment]]; [here](http://ecma-international.org/ecma-262/#sec-functioninitialize)
>
> ```js
> Set F.[[Environment]] to Scope.
> ```
>
> While ES5 docs call it [[Scope]]; [here](https://www.ecma-international.org/ecma-262/5.1/#sec-13.2)
>
> ```js
> Set the [[Scope]] internal property of F to the value of Scope.
> ```
>
> **自我补充：**还是这个链接 https://262.ecma-international.org/5.1/#sec-13.2 中有：
>
> | Internal Property | **Value Type Domain**                                        | **Value Type Domain**                                        |
> | ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | [[Scope]]         | [Lexical Environment](https://262.ecma-international.org/5.1/#sec-10.2) | A [lexical environment](https://262.ecma-international.org/5.1/#sec-10.2) that defines the environment in which a Function object is executed. Of the standard built-in ECMAScript objects, only Function objects implement [[Scope]]. |



### JS 变量对象 （词法环境）

**注：**在 ES3 中称为「变量对象」，而在 ES6 中称为 「词法环境」。这里文章是讲述 ES3 的，所以是「变量对象」，下面会做「词法环境」的笔记。

**对于每个执行上下文，都有三个重要属性**

- 变量对象 ( Variable object，VO )
- 作用域链 ( Scope chain )
- this

##### 关于「执行上下文」Kuitos 博文中的补充

> ```js
> executionContext：{
>     variable object：vars, functions, arguments,        // 注意，这里的成员
>     scope chain: variable object + all parents scopes   // 注意：all parents scope，重要
>     thisValue: context object
> }
> ```
>
> 摘自：[一道js面试题引发的思考](https://github.com/kuitos/kuitos.github.io/issues/18)

下面重点讲讲创建变量对象的过程

#### 变量对象

变量对象是 与执行上下文相关的 <font color=FF0000>**数据作用域**</font>，<font color=FF0000>存储了在 **上下文中定义的变量和函数声明**</font>。

因为不同执行上下文下的变量对象稍有不同，所以我们来聊聊「全局上下文」下的「变量对象」和「函数上下文」下的「变量对象」。

**补充：**

> 变量对象对于程序而言是不可读的，只有编译器才有权访问变量对象。
>
> 摘自：[一道js面试题引发的思考](https://github.com/kuitos/kuitos.github.io/issues/18)

#### 全局上下文

我们先了解一个概念，叫全局对象。在 [W3School](http://www.w3school.com.cn/jsref/jsref_obj_global.asp) 中也有介绍：

> <mark>「全局对象」是预定义的对象，作为 JavaScript 的 「全局函数」 和 「全局属性」 的占位符</mark>。通过使用全局对象，可以访问所有其他所有预定义的对象、函数和属性。

> 在顶层 JavaScript 代码中，可以用关键字 this 引用全局对象。因为 <font color=FF0000>**全局对象是作用域链的头**</font>，这意味着所有非限定性的变量和函数名都会作为该对象的属性来查询。

> 例如，当 JavaScript 代码引用 parseInt() 函数时，它引用的是全局对象的 parseInt 属性。全局对象是作用域链的头，还意味着在顶层 JavaScript 代码中声明的所有变量都将成为全局对象的属性。

##### 这里简单介绍下「全局对象」

1. 可以通过 this 引用，在客户端 JavaScript 中，全局对象就是 Window 对象。

2. 全局对象是由 Object 构造函数实例化的一个对象。

   ```js
   this instanceof Object === true
   ```

3. 预定义了一堆函数和属性

4. 作为全局变量的宿主

   ```js
   var a = 1
   console.log(a === global.a) // true
   ```

5. 客户端 JavaScript（**注：**这里应该是指“浏览器”） 中，全局对象有 window 属性指向自身

   ```js
   var a = 1
   console.log(this.window.a) // 2 
   ```

以上，即：「全局上下文」中的变量对象就是「全局对象」

#### 函数上下文

<font color=FF0000>在**「函数上下文」**中，我们 **用「活动对象」**( activation object, AO ) 来**表示「变量对象」**</font>。

<font color=FF0000>「活动对象」和「变量对象」其实是一个东西</font>，只是<font color=FF0000>**「变量对象」是「规范上的」或者说是「引擎实现上」的，不可在 JavaScript 环境中访问**</font>。<font color=FF0000>**只有到当进入一个「执行上下文」中，这个「执行上下文」的「变量对象」才会被激活**</font>；所以才叫 Activation Object，而 <font color=FF0000>只有被激活的变量对象、也就是活动对象上的各种属性，**才能被访问**</font>。

「活动对象」是在进入「函数上下文」时刻被创建的（**注：**根据下面 [[#关于「函数上下文」Kuitos 博文中的补充]] 的说法，“进入”的意思就是“被调用”、”执行到“），<font color=FF0000>**它（AO）通过函数的 arguments 属性初始化**</font>。arguments 属性值是 Arguments 对象。

##### 关于「函数上下文」Kuitos 博文中的补充

> **<font color=FF0000>当函数被激活</font>，那么一个活动对象 (activation object) 就会被创建并且分配给执行上下文。<font color=FF0000>「活动对象」由特殊对象 arguments 初始化而成</font>。<font color=FF0000>随后，他被当做变量对象 ( variable object ) 用于变量初始化</font>。**
>
> 用代码来说明就是：
>
> ```js
> function a(name, age){
>     var gender = "male";
>     function b(){}
> }
> a(“k”,10);
> ```
>
> <font color=FF0000 size=4>**a 被调用时**</font>（**注：**这里说的很明白了，是 a 被调用之后，创建 AO），<font color=FF0000>**在 a 的执行上下文会创建一个 活动对象 AO，并且被初始化为 AO = [arguments]**</font>。随后 AO 又被当做变量对象 ( variable object ) VO进行变量初始化，此时 VO = [arguments].concat([name,age,gender,b])。
>
> 摘自：[一道js面试题引发的思考](https://github.com/kuitos/kuitos.github.io/issues/18)

#### 执行过程

<font color=FF0000>**「执行上下文」的代码会分成 两个阶段 进行处理：分析 和 执行**</font>，我们也可以叫做：

1. <font color=FF0000>进入执行上下文</font>：[[#进入执行上下文]]
2. <font color=FF0000>代码执行</font>：[[#代码执行]]

##### 进入执行上下文

<font color=FF0000>当进入「执行上下文」时，这时候还没有执行代码</font>，**「变量对象」会包括**：

- <font color=FF0000>函数的**所有形参**</font>（如果是「函数上下文」）。**注：**这个应该对应下面例子中的 有实参的“形参a“

  - 由名称和对应值组成的一个变量对象的属性被创建

  - 没有实参（**注：**可以理解为：调用时没有实参），属性值设为 undefined

- 函数声明

  - <font color=FF0000>由“名称”和“对应值”（**函数对象** ( function-object ) ）组成一个变量对象的属性被创建</font>
  - 如果变量对象已经存在相同名称的属性，则完全替换这个属性（**注：**应该是覆盖？）

- 变量声明。**注：**这里对应下面的变量 b 和 d（ d 是变量声明）

  - 由名称和对应值 ( undefined ) 组成一个「变量对象」的属性被创建（注：如下面的）
  - <font color=FF0000 size=4>**如果「变量名称」跟 “已经声明的形式参数”或函数相同，则变量声明不会干扰已经存在的这类属性**</font>。**注：** 这句话非常重要，解释了「函数提升」和 「var 变量提升」冲突，谁优先（**不是覆盖**）的问题。具体示例参见下面 [[#变量对象的思考题 第二题]]

  **补充：**

  > 这里有一点特殊就是只有 函数声明 ( function declaration ) 会被加入到「变量对象」中，而 <font color=FF0000>**函数表达式 ( function expression )** 则不会</font>。看代码：
  >
  > ```js
  > // 函数声明
  > function a(){}
  > console.log(typeof a); // "function"
  > 
  > // 函数表达式
  > var a = function _a(){};
  > console.log(typeof a); // "function"
  > console.log(typeof _a); // "undefined"
  > ```
  >
  > **函数声明** 的方式下，a 会被加入到变量对象中，故当前作用域能打印出 a。
  > **函数表达式** 情况下，a 作为变量会加入到变量对象中，\_a 作为函数表达式则不会加入，故 a 在当前作用域能被正确找到，\_a 则不会。
  >
  > 另外，关于变量如何初始化，看这里：
  > ![https://github.com/kuitos/kuitos.github.io/blob/assets/images/image2015-3-10%2013-20-41.png](https://s2.loli.net/2022/04/06/7FsKhj6BX9OWpNY.png)
  >
  > 摘自：[一道js面试题引发的思考](https://github.com/kuitos/kuitos.github.io/issues/18)

举个例子：

```js
function foo(a) {
  var b = 2;
  function c() {}
  var d = function() {};

  b = 3;
}

foo(1);
```

在进入执行上下文后，这时候的 AO 是：

```js
AO = {
    arguments: { // arguments
        0: 1,
        length: 1
    },
    a: 1, // 有实参传递，所以有值，不为 undefined
    b: undefined, // 变量声明
    c: reference to function c(){}, // 函数声明
    d: undefined // 函数表达式，是一个变量声明；所以初始化为 undefined
}
```

##### 代码执行

在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值。

还是上面的例子，当代码执行完后，这时候的 AO 是：

```js
AO = {
    arguments: {
        0: 1,
        length: 1
    },
    a: 1,
    b: 3,
    c: reference to function c(){},
    d: reference to FunctionExpression "d"
}
```

到这里变量对象的创建过程就介绍完了，让我们简洁的总结我们上述所说：

1. 全局上下文的变量对象初始化是全局对象
2. 函数上下文的<font color=FF0000>变量对象初始化只包括 Arguments 对象</font>
3. 在进入执行上下文时会给变量对象添加形参、函数声明、变量声明等初始的属性值
4. 在代码执行阶段，会再次修改变量对象的属性值

#### 变量对象的思考题

##### 变量对象的思考题 第一题

```js
function foo() {
    console.log(a);
    a = 1;
}
foo(); // ???

function bar() {
    a = 1;
    console.log(a);
}
bar(); // ???
```

第一段会报错：`Uncaught ReferenceError: a is not defined`。第二段会打印：`1`。

这是因为：<font color=FF0000 size=4>**函数中的 “a” 并没有通过 var 关键字声明，所以不会被存放在 AO 中**</font>。

第一段执行 console 的时候， AO 的值是：

```js
AO = {
    arguments: {
        length: 0
    }
}
```

没有 a 的值，然后就会到全局去找，全局也没有，所以会报错。

当第二段执行 console 的时候，全局对象已经被赋予了 a 属性，这时候就可以从全局找到 a 的值，所以会打印 1。

##### 变量对象的思考题 第二题

```js
console.log(foo);
function foo(){
    console.log("foo");
}
var foo = 1;
```

会打印函数，而不是 undefined 。 <font color=FF0000>这是因为在进入执行上下文时，首先会处理函数声明，其次会处理变量声明</font>，<font color=FF0000 size=4>**如果「变量名称」跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性**</font>。另外，上面这句话同样有说过，见 [[#进入执行上下文]]，那里做了一些补充。

摘自：[JavaScript深入之变量对象](https://github.com/mqyqingfeng/Blog/issues/5)



### JS 词法作用域

- <font size=4>**作用域**</font>

  **「作用域」是指：<font color=FF0000>程序源代码中定义变量的区域</font>**。

  <font color=FF0000 size=4>**「作用域」规定了如何查找变量**</font>，也就是确定当前执行代码对变量的访问权限。

  <font color=FF0000 size=4>JavaScript 采用词法作用域 ( lexical scoping)，也就是静态作用域</font>。

- <font size=4>**静态作用域 和 动态作用域**</font>

  因为 JavaScript 采用的是词法作用域，<font color=FF0000 size=4>**函数的作用域 在函数定义的时候就决定了**</font>。

  与词法作用域相对的是 <font color=FF0000>**动态作用域**，函数的作用域是在 **函数调用** 的时候才决定的</font>。动态作用域的语言有：lisp、bash

  **证明 js 是静态作用域的例子：**

  ```js
  var value = 1;
  function foo() { console.log(value); }
  
  function bar() {
      var value = 2;
      foo();
  }
  
  bar(); // 1
  ```

  - **如果是静态作用域：**执行 foo 函数，先从 foo 函数内部查找是否有局部变量 value，如果没有，就根据书写的位置，查找上面一层的代码，也就是 value 等于 1，所以结果会打印 1。
  - **如果是动态作用域：**执行 foo 函数，依然是从 foo 函数内部查找是否有局部变量 value。如果没有，就从调用函数的作用域，也就是 bar 函数内部查找 value 变量，所以结果会打印 2。

  但结果为1

摘自：[JavaScript深入之词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)

##### 博主“冴羽”在issue评论区的引用

此条评论链接：https://github.com/mqyqingfeng/Blog/issues/3#issuecomment-308667350

> 和大多数的现代化编程语言一样，JavaScript 是采用词法作用域的，这就意味着<font color=FF0000>函数的执行依赖于函数定义的时候所产生（而不是函数调用的时候产生的）的变量作用域</font>。<mark style=background:aqua>**为了去实现这种词法作用域**</mark>，<font color=FF0000>JavaScript 函数对象的内部状态不仅包含函数逻辑的代码</font>，除此之外<font color=FF0000>还包含当前作用域链的引用</font>。函数对象可以通过这个作用域链相互关联起来，<mark>如此，函数体内部的变量都可以保存在函数的作用域内，这在计算机的文献中被称之为「闭包」</mark>。
>
> <mark>从技术的角度去将，所有 JavaScript 函数都是「闭包」</mark>：他们都是对象，他们都有一个关联到他们的作用域链。绝大多数函数在调用的时候使用的作用域链和他们在定义的时候的作用域链是相同的，但是这并不影响闭包。当调用函数的时候闭包所指向的作用域链和定义函数时的作用域链不是同一个作用域链的时候，闭包 become interesting。这种 interesting 的事情往往发生在这样的情况下： <mark>当一个函数嵌套了另外的一个函数，外部的函数将内部嵌套的这个函数作为对象返回</mark>。一大批强大的编程技术都利用了这类嵌套的函数闭包，当然，Javascript 也是这样。可能你第一次碰见闭包觉得比较难以理解，但是去明白闭包然后去非常自如的使用它是非常重要的。
>
> 通俗点说，在程序语言范畴内的闭包是指函数把其的变量作用域也包含在这个函数的作用域内，形成一个所谓的“闭包”，这样的话外部的函数就无法去访问内部变量。所以按照第二段所说的，严格意义上所有的函数都是闭包。
>
> 需要注意的是：我们常常所说的闭包指的是让外部函数访问到内部的变量，也就是说，按照一般的做法，是使内部函数返回一个函数，然后操作其中的变量。这样做的话一是可以读取函数内部的变量，二是可以让这些变量的值始终保存在内存中。
>
> 内容的源链接：[rccoder 博客链接](https://github.com/rccoder/blog/issues/3)



### JS 作用域链

在[《JavaScript深入之执行上下文栈》](https://github.com/mqyqingfeng/Blog/issues/4)中讲到，<font color=FF0000>当 JavaScript 代码执行一段可执行代码 (executable code) 时，**会创建对应的执行上下文( execution context )**</font>。

**对于每个执行上下文，都有三个重要属性：**

- 变量对象 ( Variable object，VO )。**注：**下面会更详细的说明 VO
- 作用域链 ( Scope chain)
- this

#### 作用域链

在[《JavaScript深入之变量对象》](https://github.com/mqyqingfeng/Blog/issues/5)中讲到，<font color=FF0000>当查找变量的时候，会先从当前上下文的变量对象中查找，**如果没有找到，就会从父级（词法层面上的父级）执行上下文的变量对象中查找**，一直找到全局上下文的变量对象，也就是全局对象</font>。<font size=4>**这样由多个执行上下文的变量对象构成的链表就叫做<font color=FF0000>「作用域链」</font>**</font>。

下面，让我们以一个<font color=FF0000>函数的 <font size=4>**创建**</font> 和 <font size=4>**激活**</font> 两个时期</font>来讲解作用域链是如何创建和变化的（**注：**函数创建是指函数定义，函数激活是指函数被调用？）：

- <font size=4>**函数创建**</font>

  在[《JavaScript深入之词法作用域和动态作用域》](https://github.com/mqyqingfeng/Blog/issues/3)中讲到，<font color=FF0000 size=4>**函数的作用域在函数定义的时候就决定了**</font>。

  这是因为 <font color=FF0000>**函数有一个 内部属性 \[\[scope]]，当函数创建的时候，就会 <font size=4>保存所有父「变量对象」到其中</font>**</font>（**注：**会保存 父变量对象（即：父「执行上下文环境」的 VO ）到 [[scope]] 中，这点要注意！！ 另外，下面有 testScope 函数的 \[\[scope]] 内部属性截图），你 <font color=FF0000 size=4>**可以理解 [[scope]] 就是所有父变量对象的层级链**</font>，但是<font color=FF0000>**注意⚠️：\[\[scope]] 并不代表 完整的作用域链**</font>！

  <img src="https://s2.loli.net/2022/04/05/vT5iDzLo4cqYWPy.png" alt="image-20220405214215002" style="zoom:60%;" />

  **注：**Scope 本来也有 “作用域” 的含义，如下图 Chrome 调试中的 Scope。图片摘自：[现代JS方法 - 在浏览器中调试 - Debugger 命令](https://zh.javascript.info/debugging-chrome#debugger-ming-ling)

  > <img src="https://s2.loli.net/2022/04/05/ATwHPeGcvMKgQnu.png" alt="image-20220405215320848" style="zoom:60%;" />

  **举个例子：**

  ```js
  function foo() { // 注：虽然文中没说，但是要注意：foo 函数是在全局作用域下
      function bar() { ... }
  }
  ```

  函数创建时，各自的 [[scope]] 为：

  ```js
  // 注：之所以这里写成 foo.[[scope]] 是因为 [[scope]] 就是包含在 foo 对象中的；如上面“打印 testScope 函数内容”的图
  foo.[[scope]] = [
    globalContext.VO // variable object
  ];
  bar.[[scope]] = [
    fooContext.AO, // 注：上面有说，AO 是 Activation Object 的缩写
    globalContext.VO
    /* 注：bar 在 foo 的内部，所以 bar 的 [[scope]] 有 fooContext；同时，还有 globalContext，这是我没有想到的；不过，否则的话，[[scope]] 也不会是个数组了。
    另外，开始我以为 [[scope]] 是一个指针，指向上一级的作用域；但现在看来，不是一般认知的指针；或许是个指针数组？*/
  ];
  ```

- <font size=4>**函数激活**</font>
  当函数激活时，进入函数上下文，创建 VO/AO 后，就会将「活动对象」添加到作用链的前端。（**注：**添加到作用链的前端，也就是下面写的操作：[AO].concat(\[\[Scope]])。<font color=FF0000>另外，添加到作用链（一个数组）的最前端，是方便依次查找</font>）

  这时候执行上下文的作用域链，我们命名为 Scope：

  ```js
  Scope = [AO].concat([[Scope]]);
  ```

  至此，作用域链创建完毕。

#### 示例与总结

以下面的例子为例，结合着之前讲的「变量对象」和「执行上下文栈」，我们来总结一下「函数执行上下文」中「作用域链」和「变量对象」的创建过程：

```js
var scope = "global scope";
function checkscope(){
    var scope2 = 'local scope';
    return scope2;
}
checkscope();
```

##### 函数「执行上下文」执行过程如下

1. <font color=FF0000>**checkscope 函数被 <font size=4>创建</font>，保存「作用域链」到 内部属性  \[\[scope]]**</font>（**注：**亦即，父「执行上下文环境」的 VO 保存到 \[\[scope]] ）

   ```js
   checkscope.[[scope]] = [
       globalContext.VO
   ];
   ```

2. <font color=FF0000>执行 checkscope 函数，创建 checkscope 函数「执行上下文」，**checkscope 函数「执行上下文」被压入「执行上下文栈」**</font>

   ```js
   ECStack = [ // 注：这里 ECStack 全称应该是：Execution Context Stack
       checkscopeContext,
       globalContext
   ];
   ```

3. <font color=FF0000 size=4>**checkscope 函数并不立刻执行，开始做准备工作**</font>，<mark>第一步</mark>：<font color=FF0000>复制函数 [[scope]] 属性 <font size=4>**创建作用域链**</font></font>

   ```js
   checkscopeContext = { // 注：这里checkscopeContext 是步骤二中 ECStack 的成员。
       Scope: checkscope.[[scope]],
   }
   ```

4. <mark>第二步</mark>：<font color=FF0000>**用 arguments 创建活动对象**</font>，<font color=FF0000>随后初始化活动对象，加入形参、函数声明、变量声明</font>。（**注：**这里创建 VO 的内容，可以看 [[# JS 变量对象 （词法环境）]] 部分，那里有详细的讲述）

   ```js
   checkscopeContext = {
       AO: { // 这就是创建的 活动对象 Activion object
           arguments: {
               length: 0 // 这里 arguments 是一个实现了 @@iterator 的对象，所以包含有 length 属性。
           },
           scope2: undefined
       }，
       Scope: checkscope.[[scope]],
   }
   ```

5. <mark>第三步</mark>：<font color=FF0000>将活动对象压入 checkscope 作用域链顶端</font>（**注：**即 Scope 数组的开头）

   ```js
   checkscopeContext = {
       AO: {
           arguments: {
               length: 0
           },
           scope2: undefined
       },
       Scope: [AO, [[Scope]]] // 这里，AO 被压入 checkscope作用域顶端
   }
   ```

6. <font color=FF0000 size=4>准备工作做完，开始执行函数</font>，随着函数的执行，修改 AO 的属性值

   ```js
   checkscopeContext = {
       AO: {
           arguments: {
               length: 0
           },
           scope2: 'local scope' // scope2 的值被修改
       },
       Scope: [AO, [[Scope]]]
   }
   ```

7. 查找到 scope2 的值，<font color=FF0000>返回后函数执行完毕，<font size=4>**函数上下文从执行上下文栈中弹出**</font></font>（注：即 checkscopeContext 被弹出、销毁）

   ```js
   ECStack = [
       globalContext
   ];
   ```

摘自：[JavaScript深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/6)
