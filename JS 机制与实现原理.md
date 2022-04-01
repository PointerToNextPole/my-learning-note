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

- **var：**全局的 var 变量其实仅仅是为 global 对象添加了一条属性。毕竟，全局环境下的 var变量 会被挂载到「全局对象」上（即：window / gloabl）

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

红框内仅有变量 a，而变量 b 已经消失不见了。**注：**这里 “变量a” 不是局部变量，寸在「堆」中；“变量b” 是局部变量，存在「栈」中。

摘自：[JS 变量存储？栈 & 堆？NONONO!](https://juejin.cn/post/6844903997615128583)