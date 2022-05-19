# JS 机制与原理



### 常见垃圾回收算法与 JavaScript 中垃圾回收原理

根据不同的需求而有了不同的取舍，从而产生了不同的算法。不论哪个垃圾回收算法，都有一套共同的流程：

1. 标记内存空间中的活动对象（在使用中的对象）和非活动对象（可以回收的对象）。
2. 删除非活动对象，释放内存空间。
3. 整理内存空间，避免频繁回收后产生的大量内存碎片（不连续内存空间）。

#### 常见的 垃圾回收算法

- **标记-清除法**

  标记-清除法由 John McCarthy（**注：**即那位“约翰·麦肯锡”） 于 1960 年发表的一篇论文提出，其主要分两个阶段：

  1. 第一阶段是标记，从一个 GC root 集合出发，沿着「指针」找到所有对象，将其标记为活动对象。
  2. 第二阶段是清除，将内存中未被标记的对象删除，释放内存空间。

  <img src="https://s2.loli.net/2022/04/15/zg5pDorOeMX9PWJ.png" alt="图片" style="zoom:70%;" />

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

  <img src="https://s2.loli.net/2022/04/15/2HEmQgS4JpBVwlR.png" alt="图片" style="zoom:75%;" />

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

##### 《现代JS教程》中关于 GC 的补充

JavaScript 中主要的内存管理概念是 **可达性**。简而言之，“可达”值 是那些以某种方式可访问或可用的值。它们一定是存储在内存中的。另外：对外引用不重要，只有传入引用才可以使对象可达。

**内部算法：**垃圾回收的基本算法被称为 “mark-and-sweep”（**注：**即上面所说的「标记」法）

**定期执行以下“垃圾回收”步骤：**

- 垃圾收集器找到所有的根，并“标记”（记住）它们。
- 然后它遍历并“标记”来自它们的所有引用。
- 然后它遍历标记的对象并标记 **它们的** 引用。所有被遍历到的对象都会被记住，以免将来再次遍历到同一个对象。
- ……如此操作，直到所有可达的（从根部）引用都被访问到。
- 没有被标记的对象都会被删除。

摘自：[现代js教程 - 垃圾回收](https://zh.javascript.info/garbage-collection)

### WeakMap

#### WeakMap 的特性

##### 1. WeakMap 只接受对象作为键名

```js
const map = new WeakMap();
map.set(1, 2); // TypeError: Invalid value used as weak map key
map.set(null, 2); // TypeError: Invalid value used as weak map key
```

##### 2. WeakMap 的键名所引用的对象是弱引用

这句话其实让我非常费解，我个人觉得这句话真正想表达的意思应该是：

> WeakMaps hold "weak" references to key objects

翻译过来应该是 <font color=FF0000>WeakMaps 保持了 **对键名所引用的对象的弱引用**</font>。

我们先聊聊弱引用：

> <mark>在计算机程序设计中，**弱引用与强引用相对**</mark>，是指 <font color=FF0000>**不能确保** 其引用的对象 **不会被垃圾回收器回收** 的引用</font>。 <font color=FF0000>一个对象若只被弱引用所引用，**则被认为是 不可访问**（或弱可访问）的，并 **因此可能在任何时刻被回收**</font>。

在 JavaScript 中，<font color=FF0000>一般我们创建一个对象，**都是建立一个强引用**</font>：

```js
var obj = new Object();
```

<font color=FF0000>只有当我们手动设置 `obj = null` 的时候，**才有可能回收 obj 所引用的对象**</font>。

而如果我们能创建一个弱引用的对象：

```js
// 假设可以这样创建一个
var obj = new WeakObject();
```

我们<font color=FF0000>什么都不用做，只用静静的等待垃圾回收机制执行，obj 所引用的对象就会被回收</font>。

我们再来看看这句：

> WeakMaps 保持了对键名所引用的对象的弱引用

正常情况下，我们举个例子：

```js
const key = new Array(5 * 1024 * 1024);
const arr = [ [key, 1] ];
```

使用这种方式，我们<mark>其实建立了 arr 对 key 所引用的对象</mark>（我们假设这个真正的对象叫 Obj ）<mark>的 **强引用**</mark>。

所以<font color=FF0000>**当你设置 `key = null` 时，只是去掉了 key 对 Obj 的强引用**</font>，<font color=FF0000>并 **没有去除 arr 对 Obj 的强引用，所以 Obj 还是不会被回收掉**</font>。

**Map 类型也是类似：**

```js
let map = new Map();
let key = new Array(5 * 1024 * 1024);

map.set(key, 1); // 建立了 map 对 key 所引用对象的强引用
key = null;      // key = null 不会导致 key 的原引用对象被回收
```

我们依然通过 Node 证明一下：

```js
// 注：假设文件名为 node-gc.js
global.gc();
process.memoryUsage(); // heapUsed: 4638376 ≈ 4.4M

let map = new Map();
let key = new Array(5 * 1024 * 1024);
map.set(key, 1);
global.gc();
process.memoryUsage(); // heapUsed: 46727816 ≈ 44.6M

map.delete(key);
global.gc();
process.memoryUsage(); // heapUsed: 46748352 ≈ 44.6M

key = null;
global.gc();
process.memoryUsage(); // heapUsed: 4808064 ≈ 4.6M
```

```sh
node --expose-gc node-gc.js # 注：注意 --expose-gc 必需放在 js 文件名前
```

**注：**要查看运行结果可以在 `process.memoryUsage();` 外面包裹一层 `console.log()`。打印结果为：

```js
{ rss: 27213824, heapTotal: 6070272, heapUsed: 3503776, external: 447189, arrayBuffers: 10430 }
{ rss: 69857280, heapTotal: 48553984, heapUsed: 45782136, external: 457993, arrayBuffers: 10430 }
{ rss: 69955584, heapTotal: 50651136, heapUsed: 45788184, external: 457993, arrayBuffers: 10430 }
{ rss: 70057984, heapTotal: 50651136, heapUsed: 45788296, external: 457993, arrayBuffers: 10430 }
```

如果你想要让 Obj 被回收掉，你需要先 `delete(key)` 然后再 `key = null`:

```js
let map = new Map();
let key = new Array(5 * 1024 * 1024);
map.set(key, 1);
map.delete(key);
key = null;
```

我们依然通过 Node 证明一下（**注：**代码略，见原文）

这个时候就要说到 WeakMap 了：

```js
const wm = new WeakMap();
let key = new Array(5 * 1024 * 1024);
wm.set(key, 1);
key = null;
```

当我们 <font color=FF0000>设置 `wm.set(key, 1)` 时，其实建立了 wm 对 key 所引用的对象的弱引用</font>，<mark>但因为 `let key = new Array(5 * 1024 * 1024)` **建立了 key 对所引用对象的强引用**，被引用的对象并不会被回收</mark>，但是<font color=FF0000>**当我们设置 `key = null` 的时候，就只有 wm 对所引用对象的弱引用**，下次垃圾回收机制执行的时候，该引用对象就会被回收掉</font>。

我们用 Node 证明一下：

```js
global.gc();
process.memoryUsage(); // heapUsed: 4638992 ≈ 4.4M

const wm = new WeakMap();
let key = new Array(5 * 1024 * 1024);
wm.set(key, 1);
global.gc();
process.memoryUsage(); // heapUsed: 46776176 ≈ 44.6M

key = null;
global.gc();
process.memoryUsage(); // heapUsed: 4800792 ≈ 4.6M
```

所以 <font color=FF0000>WeakMap 可以帮你省掉手动删除对象关联数据的步骤</font>，所以当你不能或不想控制关联数据的生命周期时就可以考虑使用 WeakMap

总结这个弱引用的特性，就是 WeakMaps 保持了对键名所引用的对象的弱引用，即垃圾回收机制不将该引用考虑在内。<font color=FF0000>只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存</font>。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。

也正是因为这样的特性，WeakMap 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而 <font color=FF0000 size=4>**垃圾回收机制何时运行是不可预测的**，**因此 ES6 规定 WeakMap 不可遍历**</font>。

所以 <font color=FF0000>WeakMap 不像 Map，一是没有遍历操作（即没有 keys()、values() 和 entries() 方法），也没有 size 属性，也不支持 clear 方法</font>；所以 <font color=FF0000>**WeakMap 只有四个方法可用**：get()、set()、has()、delete()</font>。

#### 应用

##### 1. 在 DOM 对象上保存相关数据

传统使用 jQuery 的时候，我们会<font color=FF0000>通过 \$.data() 方法在 DOM 对象上储存相关信息</font>（就比如在删除按钮元素上储存帖子的 ID 信息），jQuery 内部会使用一个对象管理 DOM 和对应的数据，当你将 DOM 元素删除，DOM 对象置为空的时候，相关联的数据并不会被删除，你必须手动执行 \$.removeData() 方法才能删除掉相关联的数据。<font color=FF0000>**WeakMap 就可以简化这一操作**</font>：

```js
let wm = new WeakMap(), element = document.querySelector(".element");
wm.set(element, "data");

let value = wm.get(elemet);
console.log(value); // data

element.parentNode.removeChild(element);
element = null;
```

##### 2. 数据缓存

从上一个例子，我们也可以看出，当我们需要关联对象和数据，比如在不修改原有对象的情况下储存某些属性或者根据对象储存一些计算的值等，而又不想管理这些数据的死活时非常适合考虑使用 WeakMap。数据缓存就是一个非常好的例子：

```js
const cache = new WeakMap();
function countOwnKeys(obj) {
    if (cache.has(obj)) {
        console.log('Cached');
        return cache.get(obj);
    } else {
        console.log('Computed');
        const count = Object.keys(obj).length;
        cache.set(obj, count);
        return count;
    }
}
```

##### 3. 私有属性

<font color=FF0000>WeakMap 也可以被用于实现私有变量</font>，不过在 ES6 中实现私有变量的方式有很多种，这只是其中一种：

```js
const privateData = new WeakMap();

class Person {
    constructor(name, age) { 
      privateData.set(this, { name: name, age: age }); 
    }
    getName() { return privateData.get(this).name; }
    getAge() { return privateData.get(this).age; }
}

export default Person;
```

摘自：[ES6 系列之 WeakMap](https://github.com/mqyqingfeng/Blog/issues/92)



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

<img src="https://s2.loli.net/2022/04/01/1ESFxit6IGcrKmv.png" alt="image-20220401182634913" style="zoom: 30%;" />

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

<img src="https://s2.loli.net/2022/04/15/nFN83Elpu7oQXq6.jpg" alt="使用 Scope 保存变量" style="zoom:50%;" />

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

#### 《「2021」高频前端面试题汇总之JavaScript篇》中的补充：

**简单数据类型** 直接存储在栈 ( stack ) 中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。

**引用数据类型** 存储在堆 ( heap ) 中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；<font color=FF0000 size=4>**引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址**</font>。<font color=FF0000>当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体</font>。

<font color=FF0000 size=4>**栈区内存由编译器自动分配释放**</font>，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

<font color=FF0000><font size=4>**堆区内存一般由开发着分配释放**</font>，若开发者不释放，程序结束时可能由垃圾回收机制回收</font>

##### IEEE 754 标准在 JS 数字存储 中的使用

先说背景：为什么 `0.1 + 0.2 !== 0.3` 且 `console.log(0.1 + 0.2)` 的结果为 `0.30000000000000004` ？

0.1 的二进制是 `0.0001100110011001100...`（ 1100 循环），0.2 的二进制是 `0.00110011001100...`（ 1100 循环）；所以加起来的结果是 `0.30000000000000004` 。如何解决：

一个直接的解决方法就是设置一个误差范围，通常称为 “机器精度”。对 JavaScript 来说，这个值通常为 2^-52^ ，在 ES6 中，提供了`Number.EPSILON` 属性，而它的值就是 2^-52^，只要判断 `0.1+0.2-0.3` 是否小于 `Number.EPSILON` ，如果小于，就可以判断为 0.1+0.2 ===0。

```js
function numberepsilon(arg1,arg2){                   
  return Math.abs(arg1 - arg2) < Number.EPSILON;        
}        

console.log(numberepsilon(0.1 + 0.2, 0.3)); // true
```

摘自：[「2021」高频前端面试题汇总之JavaScript篇（上）](https://juejin.cn/post/6940945178899251230)



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

<font color=FF0000>当 <font size=4>**执行** 一个函数的时候</font></font>（**注：**注意是 **执行**，不是初始化。另外，可以看下面 push 的顺序，是执行的 fun1 最先被 push），<font color=FF0000>就会创建一个执行上下文，并且压入执行上下文栈</font>，<font color=FF0000>当函数执行完毕的时候，就会将函数的执行上下文从栈中弹出</font>。知道了这样的工作原理，让我们来看看如何处理上面这段代码：

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

**注：**这里的摘抄很多是阅文前端团队对本文进行了翻译，本来准备只摘抄译文的；但是发现译文和原文相差有点多（应该是原文之后修改了...），便还是看了原文，遇见相同的则摘抄译文。另外，下面的 [[#文章《【译】理解 Javascript 执行上下文和执行栈 》的评论区补充]] 中的评论解释了为什么原文会改动。

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

<font color=FF0000>执行上下文分两个阶段创建：**1）创建阶段；** **2）执行阶段**</font>

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

**注：**这里相当于变相的说明了：<font color=FF0000 size=4>**「执行上下文」是由「词法环境」和「变量环境」所组成的**</font>（<font color=FF0000>至于有没有 this binding 不确定</font>，不过下面的[[#文章《【译】理解 Javascript 执行上下文和执行栈 》的评论区补充]] 中摘抄的第一条评论给出支持的观点；另外，这个不确定，要看 ES 官方文档了）。另外，为什么要划分「词法环境 」和 「变量环境」可参见 [[#文章《JS：深入理解JavaScript-执行上下文》的补充#为什么要有两个词法环境：LexicalEnvironment 和 VariableEnvironment]] 

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

**注：**在下面 [[#文章《JS：深入理解JavaScript-词法环境》的补充#词法环境有两个组成部分]] 还提及了 「对象式环境记录」用于 with 和 global 词法环境。另外，[[#文章《JS：深入理解JavaScript-词法环境》的补充#声明式环境记录的两种类型]] 中还提及了 「 声明性环境记录」的两种类型。

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

##### 变量提升在词法环境中的原理

**注意：**你或许注意到 let 和 const 定义的变量 在「创建阶段」没有任何关联的值。但是，var 定义的变量被设置为 undefined。

**这是因为：**<font color=FF0000>在「创建阶段」，代码会被扫描并解析 变量 和 函数声明</font>，其中函数声明存储在环境中，而变量会被设置为 undefined（在 var 的情况下）或 保持未初始化 uninitialized（在 let 和 const 的情况下）。

这就是为什么你可以在声明之前访问 var 定义的变量（尽管是 undefined ），但如果在声明之前访问 let 和 const 定义的变量就会提示引用错误的原因。

这就是我们所谓的变量提升。

**注意：**在「执行阶段」，如果 JS 引擎 不能找到 在源代码中在具体地方被定义的 let 变量的值，它将会被复制为 undefined。

摘自：[Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0) 及部分译文：[【译】理解 Javascript 执行上下文和执行栈](https://juejin.cn/post/6844903704466833421) 

#### 文章《【译】理解 Javascript 执行上下文和执行栈 》的评论区补充

在评论区看见了这个评论，解答了我为什么 原文和 译文 的内容不一致：

> ES2018 里把 this 划分到 语法环境 里了，并且不仅仅有词法环境和变量环境，还有 code evaluation state，Function，ScriptOrModule，Realm，Generator

另外，评论区有人在问 “为什么没有看见 变量对象 概念了”，评论回复如下：

> <font color=FF0000>那（注：即 变量对象）是 ES3 里面的概念</font>了，<mark>基本上 **变量对象 VO** 相当于这里的 **全局词法环境 + 全局变量环境**</mark>，<font color=FF0000>推出「词法作用域」和「变量作用域」应该是为了更好得区分 var 与 let / const的区别</font>

#### 文章《JS：深入理解JavaScript-执行上下文》的补充

##### 可执行代码（Executable Code）

事实上不仅仅是 function 可以作为执行上下文在执行栈中运行，在 <font color=FF0000>**JS 里定义了四种可执行代码**</font>：

- global code：整个 js 文件。
- function code：函数代码。
- module：模块代码
- eval code：放在eval的代码。

**注：**这里说明的是有几种执行上下文，在 [[#JS 执行上下文栈#可执行代码]] 中的说法是 三种 global、function、eval；没有 module；另外，[[#ES6 中的执行上下文#执行上下文的类型]] 中是三种。

##### 为什么要有两个词法环境：LexicalEnvironment 和 VariableEnvironment

<font color=FF0000>变量环境组件 ( VariableEnvironment ) 是用来 **登记 `var` `function` 变量声明**</font>，<font color=FF0000>词法环境组件 ( LexicalEnvironment ) 是用来 **登记 `let` `const` `class` 等变量声明**</font>。**注：**注意其中的 function 和 class

在 ES6 之前都没有块级作用域，ES6 之后我们可以用 `let` `const` 来声明块级作用域，<font color=FF0000>**有这两个词法环境是为了实现块级作用域的同时不影响 `var` 变量声明 和 函数声明**</font>，具体如下：

 1. 首先在一个正在运行的执行上下文内，词法环境由 LexicalEnvironment 和 VariableEnvironment 构成，用来登记所有的变量声明。
 2. <font color=FF0000>当执行到块级代码时候，会先 LexicalEnvironment 记录下来，记录为 oldEnv</font>。
3. <font color=FF0000>创建一个新的 LexicalEnvironment（ **outer 指向 oldEnv** ），记录为 newEnv，并将 newEnv 设置为正在执行上下文的 LexicalEnvironment</font>。
4.  块级代码内的 `let`  `const` 会登记在 newEnv 里面，但是 `var` 声明和函数声明还是登记在原来的 VariableEnvironment 里。
5. 块级代码执行结束后，将 oldEnv 还原为正在执行上下文的 LexicalEnvironment。

摘自：[JS：深入理解JavaScript-执行上下文](https://limeii.github.io/2019/05/js-execution-context/)

#### 文章《JS：深入理解JavaScript-词法环境》的补充

在介绍 Lexical Environment 之前，我们先看下<font color=FF0000>**在 V8 里 JS 的编译执行过程**</font>，<font color=FF0000>大致上可以分为三个阶段</font>：

- **第一步：**V8 引擎刚拿到「执行上下文」的时候（<mark>**注：**这里是 V8 引擎拿到 执行上下文？执行上下文不是 JS 引擎生成的？**这里持怀疑态度**</mark>），会把代码从上到下一行一行的先做 分词 / 词法分析 ( Tokenizing / Lexing )。分词是指：比如 `var a = 2;` 这段代码，会被分词为：`var`  `a`  `2` 和 `;` 这样的原子符号 ( atomic token )；词法分析是指：登记变量声明、函数声明、函数声明的形参。
- **第二步：**<mark>在分词结束以后，会做代码解析，引擎将 token 解析翻译成一个 AST（抽象语法树）</mark>， 在这一步的时候，如果发现语法错误，就会直接报错不会再往下执行。
- **第三步：**引擎生成 CPU 可以执行的机器码。

在第一步里有个词法分析，它用来登记变量声明、函数声明、函数声明的形参，后续代码执行的时候就知道去哪里拿变量的值和函数了，这个登记的地方就是 Lexical Environment（词法环境）。

##### 词法环境有两个组成部分

- **环境记录 ( Environment Record )**，这个就是真正登记变量的地方。
  - **声明式环境记录 ( Declarative Environment Record )**：用来记录直接有标识符定义的元素，<font color=FF0000>比如变量、常量、let、class、module、import以及函数声明</font>。
  - **对象式环境记录（Object Environment Record）**：<font color=FF0000>**主要用于 with 和 global 的词法环境**</font>。
- **对外部词法环境的引用 ( outer )**，它是作用域链能够连起来的关键。

##### 声明式环境记录的两种类型

其中 <font color=FF0000>**声明式环境记录 ( Declarative Environment Record )**，又分为两种类型</font>：

- **<font color=FF0000>函数</font>环境记录（Function Environment Record）**：<font color=FF0000>用于函数作用域</font>。
- **<font color=FF0000>模块</font>环境记录（Module Environment Record）**：模块环境记录<font color=FF0000>用于体现一个模块的外部作用域，即模块 export 所在环境</font>。

摘自：[JS：深入理解JavaScript-词法环境](https://limeii.github.io/2019/05/js-lexical-environment/)



#### 内部属性 和 词法环境

##### \[\[environment]] 相关

在《现代JS教程》中有说：

> 所有的函数在“诞生”时都会记住创建它们的词法环境。从技术上讲，这里没有什么魔法：<font color=FF0000>**<font size=4>所有函数</font> 都有名为 `[[Environment]]` 的隐藏属性**</font>，<font color=FF0000>**该属性 <font size=4>保存了对创建该函数的「词法环境」的 引用</font>**</font>。

在 《JS 忍者秘籍》（第二版）5.4.2 中：

> 无论何时创建函数，都会创建一个与之相关联的词法环境，并存储在名为a \[\[Environment]] 的内部属性上（也就是说无法直接访问或操作）。
>
> 无论何时调用函数，都会创建一个新的执行环境，被推入执行上下文栈。此外，还会创建一个与之相关联的词法环境。现在来看最重要的部分：<font color=FF0000>外部环境与新建的词法环境，**JavaScript 引擎 将调用函数的内置 \[\[Environment]] 属性与创建函数时的环境进行关联**</font>。
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

**注：**在 ES3 中称为「变量对象」，而在 ES6 中称为 「词法环境」。这里文章是讲述 ES3 的，所以是「变量对象」，上面做了 ES6 中「词法环境」的笔记  [[#词法环境 LexicalEnvironment]]。

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
   
   this.window.b = 2;
   console.log(this.b);       // 2
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

在[《JavaScript深入之执行上下文栈》](https://github.com/mqyqingfeng/Blog/issues/4)中讲到，<font color=FF0000>当 JavaScript 代码执行一段可执行代码 ( executable code ) 时，**会创建对应的执行上下文 ( execution context )**</font>。

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



### new 运算符实现

简而言之：new 运算符 创建一个用户定义的对象类型的实例 或 具有构造函数的内置对象类型之一

举个例子：

```js
// Otaku 御宅族，简称宅
function Otaku (name, age) {
    this.name = name;
    this.age = age;
    this.habit = 'Games';
}

// 因为缺乏锻炼的缘故，身体强度让人担忧
Otaku.prototype.strength = 60;

Otaku.prototype.sayYourName = function () {
    console.log('I am ' + this.name);
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // 60

person.sayYourName(); // I am Kevin
```

根据示例可知，new 创建的 person 实例可以：

- 访问 Otaku 构造函数里的属性
- 访问到 Otaku.prototype 中的属性

##### 接下来，我们可以尝试着模拟一下了

<mark>因为 new 是关键字，所以无法像 bind 函数一样直接覆盖</mark>，所以我们写一个函数，命名为 objectFactory，来模拟 new 的效果。用的时候是这样的：

```js
function Otaku () { …… }

// 使用 new
var person = new Otaku(……);
// 使用 objectFactory
var person = objectFactory(Otaku, ……) // 注：第一个参数为“构造函数”名
```

#### 初步实现

##### 分析

因为 new 的结果是一个新对象，所以在模拟实现的时候，我们也要建立一个新对象，假设这个对象叫 obj，因为 obj 会具有 Otaku 构造函数里的属性，<font color=FF0000>想想**「经典继承」**的例子</font>，我们<font color=FF0000>**可以使用 Otaku.apply(obj, arguments)来给 obj 添加新的属性**</font>。

在 [JavaScript 深入系列第一篇：JavaScript深入之从原型到原型链](https://github.com/mqyqingfeng/Blog/issues/2) 中，我们便讲了原型与原型链，我们知道实例的 \_\_proto__ 属性会指向构造函数的 prototype，也正是因为建立起这样的关系，实例可以访问原型上的属性。

现在，我们可以尝试着写第一版了：

```js
// 第一版代码
function objectFactory() {
    // 注：创建一个新的对象，最后返回
    var obj = new Object();
    // 注：弹出并使用 第一个实参（构造函数名），用名为 Constructor 的变量保存起来。另外，这里的 [].shift.call() 一般写作 Array.prototype.shift.call()；[].shift.call() 有点不易读
    Constructor = [].shift.call(arguments);
    // 注：将 Contructor 构造函数的原型 赋值到 创建的对象 obj 的原型上。这里 obj.__proto__ 是 ES5 的写法，已经不推荐使用；可以使用 ES6 的 Object.setPrototypeOf() 替代。另外，new 本身也是 ES6 的语法，不会出现不兼容的情况。
    obj.__proto__ = Constructor.prototype;
    // 注：使用 apply 调用 contructor 函数
    Constructor.apply(obj, arguments);
    // 返回 obj
    return obj;
};
```

在这一版中，我们：

1. 用new Object() 的方式新建了一个对象 obj
2. 取出第一个参数，就是我们要传入的构造函数。此外因为 shift 会修改原数组，所以 arguments 会被去除第一个参数
3. 将 obj 的原型指向构造函数，这样 obj 就可以访问到构造函数原型中的属性
4. 使用 apply，改变构造函数 this 的指向到新建的对象，这样 obj 就可以访问到构造函数中的属性
5. 返回 obj

复制以下的代码，到浏览器中，我们可以做一下测试：

```js
function Otaku (name, age) {
    this.name = name;
    this.age = age;
    this.habit = 'Games';
}

Otaku.prototype.strength = 60;

Otaku.prototype.sayYourName = function () {
    console.log('I am ' + this.name);
}

function objectFactory() {
    var obj = new Object(),
    Constructor = [].shift.call(arguments);
    obj.__proto__ = Constructor.prototype;
    Constructor.apply(obj, arguments);
    return obj;
};

var person = objectFactory(Otaku, 'Kevin', '18')

console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // 60

person.sayYourName(); // I am Kevin
```

#### 返回值效果实现

**注：**下面的内容讲的有点唐突，可以看下下面的补充 [[#视频《new实例化的重写--检测一下自己this 指向？？》的补充#不同方法返回值区别]] 中 不同返回值的总结

接下来我们再来看一种情况，假如构造函数有返回值，举个例子：

```js
function Otaku (name, age) {
    this.strength = 60;
    this.age = age;
  
    return {
        name: name,
        habit: 'Games'
    }
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // undefined 注：不是返回的属性，拿不到
console.log(person.age) // undefined      注：不是返回的属性，拿不到
```

在这个例子中，构造函数返回了一个对象，在 <font color=FF0000>**实例 person 中只能访问返回的对象中的属性**</font>。

而且还要注意一点，在这里我们是返回了一个对象，<font color=FF0000>假如我们只是返回一个基本类型的值呢</font>？再举个例子：

```js
function Otaku (name, age) {
    this.strength = 60;
    this.age = age;
  
    return 'handsome boy';
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // undefined
console.log(person.habit) // undefined
console.log(person.strength) // 60
console.log(person.age) // 18
```

结果完全颠倒过来，<font color=FF0000>这次尽管有返回值，但是相当于没有返回值进行处理</font>。

<font color=FF0000>**所以我们还需要判断返回的值是不是一个对象**</font>，<font color=FF0000>**如果是一个对象，我们就返回这个对象**</font>；<font color=FF0000>**如果没有，我们该返回什么就返回什么**</font>。

再来看第二版的代码，也是最后一版的代码：

```js
// 第二版的代码
function objectFactory() {
    var obj = new Object(),
    Constructor = [].shift.call(arguments);
    obj.__proto__ = Constructor.prototype;
    var ret = Constructor.apply(obj, arguments);
    // 注：只有这里改变了，加了判断
    return typeof ret === 'object' ? ret : obj;
};
```

摘自：[JavaScript深入之new的模拟实现](https://github.com/mqyqingfeng/Blog/issues/13)

#### 视频《new实例化的重写--检测一下自己this 指向？？》的补充

```js
function Person(name) {
  this.name = name
}
```

如上 Person 构造函数 有两种执行方法：1) 直接函数执行（直接调用 `Person()` ） 2) 使用 new 运算符执行 `new Person()`。

##### 不同方法返回值区别

- **直接函数执行：**返回的内容要看 函数内容 return 的内容：有返回值则有值，没有返回值则为 undefined

- **使用 new 运算符：**一般没有 return 返回值，且返回的内容是实例之后的对象。

  有 return：如果 return 的是一个对象（引用值），则返回该对象；如果 return 的是「原始类型」，则忽略，返回当前的实例对象。

##### new 的实现

```js
// new 的实现
Function.prototype.new = function() {
  var fn = this
  var obj = Object.create(fn.prototype)
  var res = fn.apply(obj, arguments)
  return res instanceof Object ? res : obj
}

// 构造函数
function Person(name) {
  this.name = name
}
Person.prototype.say = function() {
  console.log('hello')
}

// 检验效果
var p = Person.new('foo')
console.log(p)
p.say()
```

学习自：[【全网首发:更新完】new实例化的重写--检测一下自己this 指向？？](https://www.bilibili.com/video/BV16L411u7kh) 另外，这类源码重写的视频组成了一个合集：[测试一下前端基本功--简单的源码重写？](https://www.bilibili.com/video/BV1dS4y1X7pn)



### call 和 apply 实现

#### call 的模拟实现

一句话介绍 call：<mark>call() 方法在使用一个 
**指定的 this 值** 和 **若干个指定的参数值** 的前提下调用某个函数或方法</mark>。

举个例子：

```js
var foo = { value: 1 };
function bar() { console.log(this.value); }

bar.call(foo); // 1
```

注意两点：

1. call 改变了 this 的指向，指向到 foo
2. bar 函数执行了

##### 模拟第一步

那么我们该怎么模拟实现这两个效果呢？

试想当调用 call 的时候，把 foo 对象改造成如下：

```js
var foo = {
    value: 1,
    bar: function() {
        console.log(this.value)
    }
};

foo.bar(); // 1
```

这个时候 this 就指向了 foo，是不是很简单呢？

但是这样却<font color=FF0000>给 foo 对象本身添加了一个属性</font>，这可不行呐！不过也不用担心，（注：在执行完毕后）我们<font color=FF0000>用 delete 再删除它</font>不就好了~

**所以我们模拟的步骤可以分为：**

1. 将函数设为对象的属性
2. <font color=FF0000>执行该函数</font>
3. <font color=FF0000>**删除该函数**</font>

以上个例子为例，就是：

```js
// 第一步
foo.fn = bar
// 第二步
foo.fn()
// 第三步
delete foo.fn
```

fn 是对象的属性名，反正最后也要删除它，所以起成什么都无所谓。

根据这个思路，我们可以尝试着去写第一版的 **myCall** 函数：

```js
// 第一版
Function.prototype.myCall = function(context) {
    // 首先要获取调用 call 的函数，用 this 可以获取。注：根据下面的调用来看，这里是 this 指向的是 bar
    context.fn = this;
    context.fn();
    delete context.fn;
}

// 测试
var foo = { value: 1 };
function bar() { console.log(this.value); }

bar.myCall(foo); // 1
```

##### 模拟实现第二步

最一开始也讲了，<font color=FF0000>call 函数还能给定参数执行函数</font>。举个例子：

```js
var foo = { value: 1 };

function bar(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value);
}

bar.call(foo, 'kevin', 18); // kevin 18 1
```

**注意：**传入的参数并不确定，这可咋办？我们可以从 Arguments 对象中取值，取出第二个到最后一个参数，然后放到一个数组里。

比如这样：

```js
// 以上个例子为例，此时的arguments为：
// arguments = {
//      0: foo,
//      1: 'kevin',
//      2: 18,
//      length: 3
// }
// 因为arguments是类数组对象，所以可以用 for 循环
var args = [];
for(var i = 1, len = arguments.length; i < len; i++) {
    args.push('arguments[' + i + ']');
}
// 执行后 args为 ["arguments[1]", "arguments[2]", "arguments[3]"]。注：这一步是为下面的 eval 作准备。
```

不定长的参数问题解决了，我们接着要把这个参数数组放到要执行的函数的参数里面去。

```js
// 将数组里的元素作为多个参数放进函数的形参里
context.fn( args.join(',') ); // 这个方法肯定是不行的啦！！！
```

也许有人想到用 ES6 的方法（**注：**比如 “展开表达式”，结果测试确实可行 ），不过 call 是 ES3 的方法，我们为了模拟实现一个 ES3 的方法，要用到ES6的方法，好像……，嗯，也可以啦。但是我们这次用 eval 方法拼成一个函数，类似于这样：

```js
eval('context.fn(' + args +')')
```

<font color=FF0000>这里 args 会自动调用 Array.toString() 这个方法</font>。

所以我们的第二版克服了两个大问题，代码如下：

```js
// 第二版
Function.prototype.call2 = function(context) {
    context.fn = this;
    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    eval('context.fn(' + args +')');
    delete context.fn;
}

// 测试一下
var foo = { value: 1 };
function bar(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value);
}
bar.call2(foo, 'kevin', 18); // kevin, 18, 1
```

##### 模拟实现第三步

模拟代码已经完成 80%，还有两个小点要注意：

1. **this 参数可以传 null，当为 null 的时候，视为指向 window。**举个例子：

   ```js
   var value = 1;
   function bar() { console.log(this.value); }
   
   bar.call(null); // 1
   ```

   虽然这个例子本身不使用 call，结果依然一样。

2. **函数是可以有返回值的。**举个例子：

   ```js
   var obj = { value: 1 }
   
   function bar(name, age) {
       return {
           value: this.value,
           name: name,
           age: age
       }
   }
   
   console.log(bar.call(obj, 'kevin', 18)); // Object { value: 1,name: 'kevin',age: 18 }
   ```

   不过都很好解决，让我们直接看第三版也就是最后一版的代码：

   ```js
   // 第三版
   Function.prototype.call2 = function (context) {
       var context = context || window;
       context.fn = this;
   
       var args = [];
       for(var i = 1, len = arguments.length; i < len; i++) {
           args.push('arguments[' + i + ']');
       }
       var result = eval('context.fn(' + args +')');
   
       delete context.fn
       return result; // 返回了运行的返回值
   }
   
   // 测试一下
   var value = 2;
   var obj = { value: 1 }
   
   function bar(name, age) {
       console.log(this.value);
       return {
           value: this.value,
           name: name,
           age: age
       }
   }
   
   bar.call2(null); // 2
   console.log(bar.call2(obj, 'kevin', 18)); // 1, { value: 1, name: 'kevin', age: 18 }
   ```

#### apply 的模拟实现

apply 的实现跟 call 类似，在这里直接给代码，代码来自于知乎 @郑航 的实现：

```js
Function.prototype.apply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) { // 和 for (var i = 0; i < arr.length; i++) 有区别吗？
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }

    delete context.fn
    return result;
}
```

摘自：[JavaScript深入之call和apply的模拟实现 ](https://github.com/mqyqingfeng/Blog/issues/11)

### bind 模拟实现

**一句话介绍 bind：**bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。（来自于 MDN )

**由此我们可以首先得出 bind 函数的两个特点：**

1. 返回一个函数
2. 可以传入参数

#### 返回函数的模拟实现

从第一个特点开始，我们举个例子：

```js
var foo = { value: 1 };
function bar() { console.log(this.value); }

// 返回了一个函数
var bindFoo = bar.bind(foo); 
bindFoo(); // 1
```

关于指定 this 的指向，我们可以使用 call 或者 apply 实现，关于 call 和 apply 的模拟实现，可以查看[《JavaScript深入之call和apply的模拟实现》](https://github.com/mqyqingfeng/Blog/issues/11)。我们来写第一版的代码：

```js
// 第一版
Function.prototype.bind = function (context) {
    var self = this; // 谁调用的，也就是前面的要被 bind 函数绑定 this值 的 函数。
    return function () {
        return self.apply(context);
    }

}
```

此外，之所以 `return self.apply(context)`，是<font color=FF0000>考虑到绑定函数可能是有返回值的</font>。依然是这个例子：

```js
var foo = { value: 1 };
function bar() { return this.value; }

var bindFoo = bar.bind(foo);
console.log(bindFoo()); // 1
```

#### 传参的模拟实现

接下来看第二点，可以传入参数。这个就有点让人费解了，我在 bind 的时候，是否可以传参呢？我在执行 bind 返回的函数的时候，可不可以传参呢？让我们看个例子：

```js
var foo = { value: 1 };
function bar(name, age) {
    console.log(this.value);
    console.log(name);
    console.log(age);
}

var bindFoo = bar.bind(foo, 'daisy');
bindFoo('18'); // 1, daisy, 18
```

函数需要传 name 和 age 两个参数，竟然还可以在 bind 的时候，只传一个 name，在执行返回的函数的时候，再传另一个参数 age。这可以用 arguments 进行处理：

```js
// 第二版
Function.prototype.bind = function (context) {
    var self = this;
    // 获取 bind 函数从第二个参数到最后一个参数
    var args = Array.prototype.slice.call(arguments, 1);

    return function () {
        // 这个时候的arguments是指bind返回的函数传入的参数
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(context, args.concat(bindArgs));
    }

}
```

#### 构造函数效果的模拟实现

完成了这两点，<font color=FF0000>最难的部分到啦</font>！因为 <font color=FF0000>bind 还有一个特点</font>，就是

> 一个绑定函数也能使用 new 操作符创建对象：这种行为就像把原函数当成构造器（即：Constructor）。<font color=FF0000>**提供的 this 值被忽略**</font>，同时调用时的参数被提供给模拟函数。

也就是说当 bind 返回的函数作为构造函数的时候，bind 时指定的 this 值会失效，但传入的参数依然生效（**注：**这里可以参考 [[前端面试点总结#this的绑定（学习自 coderwhy 的文章）]] 中的：<font color=FF0000>**bind 的优先级没有 new 高**</font>）。举个例子：

```js
var value = 2;
var foo = { value: 1 };

function bar(name, age) {
    this.habit = 'shopping';
    console.log(this.value); // 这里的 this 指向的是 bar {habit: 'shopping'}，而不是obj，所以obj.value === undefined
    console.log(name);
    console.log(age);
}
bar.prototype.friend = 'kevin';
var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18'); // undefined, daisy, 18
console.log(obj.habit); // shopping
console.log(obj.friend); // kevin
```

**注意：**<font color=FF0000>尽管在全局和 foo 中都声明了 value 值，最后依然返回了 undefind，**说明绑定的 this 失效了**</font>；如果大家了解 new 的模拟实现，就会知道这个时候的 this 已经指向了 obj。

所以我们可以通过修改返回的函数的原型来实现，让我们写一下：

```js
// 第三版
Function.prototype.bind = function (context) {
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fBound = function () {
        // 注：这里的 arguments 和上面的 arguments 不同一次调用获取的，也就是说：不是一个东西。
        var bindArgs = Array.prototype.slice.call(arguments);
        // 当作为构造函数时，this 指向实例，此时结果为 true；将绑定函数的 this 指向该实例，可以让实例获得来自绑定函数的值。**注：**个人理解，作为构造函数使用时：this 在 new 操作符的作用下，指向实例；另外，此时 fBound 就是指定 this 指向后 的“构造函数”。
        // 以上面的是 demo 为例，如果改成 `this instanceof fBound ? null : context`，实例只是一个空对象，将 null 改成 this ，实例会具有 habit 属性
        // 当作为普通函数时，this 指向 window，此时（**注：**this instanceof fBound 的 ）结果为 false，将绑定函数的 this 指向 context
        return self.apply(this instanceof fBound ? this : context, args.concat(bindArgs));
    }
    
    // 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承绑定函数的原型中的值。
    fBound.prototype = this.prototype;
    return fBound; // 注：返回 fBound 函数
}
```

#### 构造函数效果的优化实现

但是在这个写法中，我们直接将 fBound.prototype = this.prototype，我们直接修改 fBound.prototype 的时候，也会直接修改绑定函数的 prototype。这个时候，我们可以通过一个空函数来进行中转：

```js
// 第四版
Function.prototype.bind = function (context) {

    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fNOP = function () {}; // 注：NOP 的意思是 No Operation，意为无操作。是汇编语言的一个指令，一系列编程语句，或网络传输协议中的表示不做任何有效操作的命令。

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```

#### 三个小问题

接下来处理些小问题：

##### 一：这一点如今找不到了，略。

##### 二：调用 bind 的不是函数咋办？

不行，我们要报错！

```js
if (typeof this !== "function") {
  throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
}
```

##### 要在线上用

那别忘了做个兼容：

```js
Function.prototype.bind = Function.prototype.bind || function () { ... };
```

当然最好是用 [es5-shim](https://github.com/es-shims/es5-shim) 啦。

#### 最终代码

```js
Function.prototype.bind2 = function (context) {

    // 比上一个版本 多了一个校验判断
    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fNOP = function () {};

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```

摘自：[JavaScript深入之bind的模拟实现](https://github.com/mqyqingfeng/Blog/issues/12)



### 深浅拷贝

##### 对数组进行拷贝

要对数组进行拷贝，我们可以利用数组的一些方法比如：slice、concat 返回一个新数组的特性来实现拷贝。对于元素都是基本类型的数组而言，这样的拷贝是可以的；但是如果数组嵌套了对象或者数组的话，克隆的并不彻底。可以看出：使用 concat 和 slice 是一种浅拷贝

#### JSON.parse( JSON.stringify() ) 实现深拷贝的问题

- JSON.parse( JSON.stringify() ) <font color=FF0000>无法拷贝函数</font>。

- 另外，根据 issue 评论区 naihe138 的评论，了解到《你不知道的 JavaScript 中卷》 中有关于 JSON.stringify 有如下说法：

  > ##### JSON 字符串化
  >
  > 工具函数 JSON.stringify(..) 在<font color=FF0000>将 JSON 对象序列化为字符串时也用到了 ToString</font>。
  >
  > 请注意，JSON 字符串化并非严格意义上的强制类型转换，因为其中也涉及 ToString 的相关规则，所以这里顺带介绍一下。
  >
  > 对大多数简单值来说， JSON 字符串化和 toString() 的效果基本相同，只不过序列化的结果总是字符串：
  >
  > ```js
  > JSON.stringify( 42 ); // "42"
  > JSON.stringify( "42" ); // ""42""（含有双引号的字符串）
  > JSON.stringify( null ); // "null"
  > JSON.stringify( true ); // "true"
  > ```
  >
  > <font color=FF0000>**所有安全的 JSON 值 ( JSON-safe ) 都可以使用 JSON.stringify(..) 字符串化**</font>。<font color=FF0000>安全的 JSON 值是指能够呈现为有效 JSON 格式的值</font>。
  >
  > 为了简单起见， 我们来看看 <font color=FF0000>什么是 不安全的 JSON 值</font>。 <font color=FF0000>**undefined**、**function**、**symbol** ( ES6+ ) 和 **包含循环引用**（对象之间相互引用，形成一个无限循环）**的对象** 都不符合 JSON 结构标准，支持 JSON 的语言无法处理它们</font>。<font color=FF0000>**JSON.stringify(..) 在 <font size=4>对象</font> 中遇到 undefined 、 function 和 symbol 时会自动将其忽略**</font>， <font color=FF0000>**在 <font size=4>数组</font> 中则会返回 null（以保证单元位置不变）**</font>。例如：
  >
  > ```js
  > JSON.stringify( undefined );                    // undefined
  > JSON.stringify( function(){} );                 // undefined
  > 
  > JSON.stringify( [1,undefined,function(){},4] ); // "[1,null,null,4]"
  > JSON.stringify({ a:2, b:function(){} } );       // "{"a":2}"
  > ```
  >
  > 对包含循环引用的对象执行 JSON.stringify(..) 会出错。
  >
  > 摘自：《你不知道的 JavaScript 中卷》P48 - P49

  **注：**以上总结：对于“不安全的 JSON 值”，包含：undefined、function、symbol 和 包含循环引用的对象，在 stringify 时，如果在对象中会被忽略；在数组中，则返回 null。

  2022/5/19 补充： 在 [MDN - JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 中也有类似的讲解。

- 文章 [JSON.stringify深拷贝的缺点](https://www.jianshu.com/p/52db1d0c1780) 中 还有其他上面没有说到的：
  - 如果深拷贝的对象 / 数组 里有 <font color=FF0000>**时间对象**</font>，则 JSON.stringify( JSON.parse ) 的结果，将只是<font color=FF0000>字符串</font>的形式，而不是对象的形式
  - 如果深拷贝的对象 / 数组 里有 <font color=FF0000>**正则表达式、Error 对象**</font>，则序列化的结果将只得到 <font color=FF0000>空对象 {}</font>。
  - 如果深拷贝的对象 / 数组 里有 <font color=FF0000>**NaN、Infinity 和 -Infinity**</font>，则序列化的结果会变成 <font color=FF0000>null</font>

摘自：[JavaScript专题之深浅拷贝](https://github.com/mqyqingfeng/Blog/issues/32)

##### jQuery extend 方法中 处理循环引用

为了避免这个问题，我们需要判断要复制的对象属性是否等于 target，如果等于，我们就跳过：

```js
...
src = target[name];
copy = options[name];

if (target === copy) {
    continue;
}
...
```

摘自：[JavaScript专题之从零实现jQuery的extend ](https://github.com/mqyqingfeng/Blog/issues/33)

#### 文章《你最少用几行代码实现深拷贝？》中的深拷贝

深度克隆就是为了解决引用数据类型不能被通过赋值的方式复制的问题。不妨来罗列一下引用数据类型都有哪些：

- ES6之前： Object, Array, Date, RegExp, Error
- ES6之后： Map, Set, WeakMap, WeakSet

所以，我们要深度克隆，就需要对数据进行遍历并根据类型采取相应的克隆方式。当然因为数据会存在多层嵌套的情况，采用「递归」

##### 简单粗暴的方法

```js
function deepClone(obj) {
    let res = {};
  
    // 类型判断的通用方法
    function getType(obj) {
        return Object.prototype.toString.call(obj).replaceAll(new RegExp(/\[|\]|object /g), "");
    }
    const type = getType(obj);
  
    const reference = ["Set", "WeakSet", "Map", "WeakMap", "RegExp", "Date", "Error"];
  
    if (type === "Object") {
        for (const key in obj) {
            if (Object.hasOwnProperty.call(obj, key)) {
                res[key] = deepClone(obj[key]);
            }
        }
    } else if (type === "Array") {
        obj.forEach((e, i) => { res[i] = deepClone(e); });
    }
    else if (type === "Date") { res = new Date(obj); }
    else if (type === "RegExp") { res = new RegExp(obj); }
    else if (type === "Map") { res = new Map(obj); }
    else if (type === "Set") { res = new Set(obj); }
    else if (type === "WeakMap") { res = new WeakMap(obj); }
    else if (type === "WeakSet") { res = new WeakSet(obj); }
    else if (type === "Error") { res = new Error(obj); }
    else { res = obj; }
    return res;
}
```

其中 `else if (type === 'typeVal')` 中存在冗余代码，可以合并。如下第二版：

```js
function deepClone(obj) {
    let res = null
    
    // 类型判断的通用方法
    function getType(obj) {
        return Object.prototype.toString.call(obj).replaceAll(new RegExp(/\[|\]|object /g), "");
    }
    const type = getType(obj);
  
    const reference = ["Set", "WeakSet", "Map", "WeakMap", "RegExp", "Date", "Error"];
    if (type === "Object") {
        res = {};
        for (const key in obj) {
            if (Object.hasOwnProperty.call(obj, key)) {
                res[key] = deepClone(obj[key]);
            }
        }
    } else if (type === "Array") {
        res = [];
        obj.forEach((e, i) => { res[i] = deepClone(e); });
    }
    else if (reference.includes(type)) { res = new obj.constructor(obj); }
    else { res = obj; }
    return res;
}
```

##### 优化

```js
// 判断类型的方法移到外部，避免递归过程中多次执行
const judgeType = origin => {
    return Object.prototype.toString.call(origin).replaceAll(new RegExp(/\[|\]|object /g), "");
};
const reference = ["Set", "WeakSet", "Map", "WeakMap", "RegExp", "Date", "Error"];

function deepClone(obj) {
    // 定义新的对象，最后返回，通过 obj 的原型创建对象
    const newObj = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));

    // 遍历对象，克隆属性
    for (let key of Reflect.ownKeys(obj)) {
        const val = obj[key];
        const type = judgeType(val);
        if (reference.includes(type)) {
            newObj[key] = new val.constructor(val);
        } else if (typeof val === "object" && val !== null) {
            newObj[key] = deepClone(val); // 递归克隆
        } else {
            newObj[key] = val; // 基本数据类型和 function
        }
    }
    return newObj;
}
```

##### 进一步优化，以及避免循环引用

```js
function deepClone(obj, hash = new WeakMap()) {
    if (hash.has(obj)) { return obj; } // 注：hash 中存在的，则直接返回
    let res = null;
  
    const reference = [Date, RegExp, Set, WeakSet, Map, WeakMap, Error];
    if (reference.includes(obj?.constructor)) { // 注：属于 reference 的，都返回构造的对象
        res = new obj.constructor(obj);
    } else if (Array.isArray(obj)) { // 注：数组处理
        res = [];
        obj.forEach((e, i) => {
            res[i] = deepClone(e);
        });
    } else if (typeof obj === "object" && obj !== null) { // 注：object 处理
        hash.set(obj);
        res = {};
        for (const key in obj) {
            if (Object.hasOwnProperty.call(obj, key)) {
                res[key] = deepClone(obj[key], hash);
            }
        }
    } else { res = obj; } // 注：其他情况
    return res;
}
```

摘自：[你最少用几行代码实现深拷贝？](https://juejin.cn/post/7075351322014253064)

另外，可以参见 [[JS 函数手写实现#深拷贝实现]] 中的代码



### JS 类型判断

#### typeof

typeof 除了是一个函数外，也是一个<font color=FF0000>运算符</font>。引用《JavaScript 权威指南》中对 typeof 的介绍：

> typeof 是一元操作符，放在其单个操作数的前面，操作数可以是任意类型。返回值为表示操作数类型的一个字符串。

在 ES6 前，JavaScript 共六种数据类型，分别是：Undefined、Null、Boolean、Number、String、Object

然而当我们使用 typeof 对这些数据类型的值进行操作的时候，返回的结果却不是一一对应，分别是：undefined、object、boolean、number、string、object。注意以上都是小写的字符串。Null 和 Object 类型都返回了 object 字符串。

尽管不能一一对应，但是 typeof 却能检测出 函数类型 function。

所以 <font color=FF0000>typeof 能检测出六种类型的值</font>（**注：**下面的 [[#typeof 新的可识别类型的补充]] 有补充：Symbol 和 BigInt 两种情况也可以识别其类型），但是，除此之外 Object 下还有很多细分的类型，如 Array、Function、Date、RegExp、Error 等。而对这些类型使用 typeof 操作符，返回的都是 object；无法良好的区分它们。这时候可以使用 Object.prototype.toString.call() 获取 '\[object RefObjectType]'；另外，可以通过正则表达式获取 refObjectType（获取类型代码，学习自：[你最少用几行代码实现深拷贝？](https://juejin.cn/post/7075351322014253064) ）：

```js
function getType(obj) {
    return Object.prototype.toString.call(obj).replaceAll(new RegExp(/\[|\]|object /g), "");
}

// 使用
getType(Promise.resolve('foo')) // Promise
```

##### typeof 新的可识别类型的补充

现在 <font color=FF0000>**不完全一样**</font> 了，至少对于 symbol、bigint 是可以判别的；至于其他的（比如 Date ）就不行了。如下摘自：[MDN - typeof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)

> | 型                                              | 结果             |
> | :---------------------------------------------- | :--------------- |
> | Undefined                                       | "undefined"      |
> | Null                                            | "object"         |
> | Boolean                                         | "boolean"        |
> | Number                                          | "number"         |
> | BigInt(ECMAScript 2020 新增)                    | "bigint"         |
> | String                                          | "string"         |
> | Symbol (ECMAScript 2015 新增)                   | "symbol"         |
> | 宿主对象（由 JS 环境提供）                      | *取决于具体实现* |
> | Function 对象 (按照 ECMA-262 规范实现 [[Call]]) | "function"       |
> | 其他任何对象                                    | "object"         |

#### Object.prototype.toString

先献上 ES5 规范地址：https://es5.github.io/#x15.2.4.2。

在第 15.2.4.2 节讲的就是 Object.prototype.toString()，为了不误导大家，我先奉上英文版：

> When the toString method is called, the following steps are taken:

> 1. If the **this** value is **undefined**, return "**[object Undefined]**".
> 2. If the **this** value is **null**, return "**[object Null]**".
> 3. Let *O* be the result of calling ToObject passing the **this** value as the argument.
> 4. Let *class* be the value of the [[Class]] internal property of *O*.
> 5. Return the String value that is the result of concatenating the three Strings "**[object** ", *class*, and "**]**".

凡是规范上加粗或者斜体的，在这里我也加粗或者斜体了，就是要让大家感受原汁原味的规范！

**如果没有看懂，就不妨看看我理解的：**

当 toString 方法被调用的时候，下面的步骤会被执行：

1. 如果 this 值是 undefined，就返回 [object Undefined]

2. 如果 this 的值是 null，就返回 [object Null]

   **注：**上面是两种异常情况 undefined 和 null 的处理

3. <font color=FF0000>让 O 成为 ToObject(this) 的结果</font>

4. <font color=FF0000>让 class 成为 O 的内部属性 [[Class]] 的值</font>

5. 最后返回由 "[object " 和 class 和 "]" 三个部分组成的字符串

通过规范，我们至少知道了调用 Object.prototype.toString 会返回一个由 "[object " 和 class 和 "]" 组成的字符串，而 class 是要判断的对象的内部属性。

经过实验（代码略，见原文），Object.prototyep.toString.call() 可以识别 Number、String、Boolean、Undefined、 Null、Object、Array、Date、Error、EegExp、Function 这 11 种类型之外，还可以识别 Math、JSON、arguments

```js
console.log(Object.prototype.toString.call(Math)); // [object Math]
console.log(Object.prototype.toString.call(JSON)); // [object JSON]
```

所以我们可以识别至少 14 种类型，当然我们也可以算出来，[[class]] 属性至少有 12 个。

**注：**还有上面提及的 Symbol 和 BigInt。另外，还有 Promise、Set、Map、weakSet、weakMap、location、history、window（只在浏览器下生效，结果为 '[object Window]' ）、 globalThis（ globalThis 在不同宿主环境的值不同，浏览器下的值为 '[object Window]'，在 Node 中为 '[object global]' ）

#### 实现 type 方法：

需要注意的是<font color=FF0000>边界情况</font>：在 IE6 中，null 和 undefined 会被 Object.prototype.toString 识别成 [object Object]

```js
var class2type = {};

// 生成class2type映射
"Boolean Number String Function Array Date RegExp Object Error".split(" ").map(function(item, index) {
    class2type["[object " + item + "]"] = item.toLowerCase();
})

function type(obj) {
    // 一箭双雕，因为：(undefined == null) === true
    if (obj == null) {
        return obj + "";
    }
    return typeof obj === "object" || typeof obj === "function"
      		 ? class2type[Object.prototype.toString.call(obj)] || "object"
    			 : typeof obj;
}
```

#### 判断类型有四种

1. **typeof**

2. **constructor**

   使用  ***.constructor.name** 可以实现判断类型功能（**注：**泛用性没有 toString 好，比如 null、undefined 不可用，arguments 的结果还是 Object。另外，这里的原理是： constructor 是一个函数，Function 有 name 这个属性 ），示例如下：

   ```js
   const reg = /abc/
   console.log(Object.getPrototypeOf(reg).constructor.name) // 'RegExp'
   ```

   另外，在看 [「2021」高频前端面试题汇总之CSS篇](https://juejin.cn/post/6905539198107942919) 时，发现这样一个之前没见过的写法（虽然没什么）：

   ```js
   console.log((2).constructor === Number); // true
   ```

   其他的基础类型也可以用，这就等价于：

   ```js
   const testNum = 2
   console.log(testNum.constructor === Number); // true
   ```

3. **instanceof**

4. **Object.prototype.toString**

#### 开发中的其他情况

上面编写了 type 函数，可以检测出常见的数据类型；然而在开发中还有更加复杂的判断，比如 plainObject、空对象、Window 对象等

##### plainObject

plainObject 来自于 jQuery，可以翻译成<mark>纯粹的对象，所谓“纯粹的对象”</mark>，就是<font color=FF0000>该对象是 **通过 "{}" 或 "new Object" 创建的**，该对象含有零个或者多个键值对</font>。

之所以要判断是不是 plainObject，是为了跟其他的 JavaScript对象如 null，数组，宿主对象（documents）等作区分，因为这些用 typeof 都会返回object。

jQuery 提供了 isPlainObject 方法进行判断，先让我们看看使用的效果：

```js
function Person(name) {
    this.name = name;
}

console.log( $.isPlainObject( {} ))                              // true
console.log( $.isPlainObject( new Object ) )                     // true
console.log( $.isPlainObject( Object.create(null) ) )            // true
console.log( $.isPlainObject( Object.assign({a: 1}, {b:  2}) ))  // true
console.log( $.isPlainObject( new Person('yayu') ))              // false
console.log( $.isPlainObject( Object.create({}) ))               // false
```

由此我们可以看到，除了 {} 和 new Object 创建的之外，jQuery 认为 <font color=FF0000>**一个没有原型的对象也是一个纯粹的对象**</font>。

源码实现，有些复杂，略。

##### EmptyObject

jQuery 提供了 isEmptyObject 方法来<font color=FF0000>判断是否是空对象</font>，代码简单，我们直接看源码：

```js
function isEmptyObject( obj ) {
    var name;
    for ( name in obj ) {
        return false;
    }
    return true;
}
```

其实所谓的 isEmptyObject 就是判断是否有属性，<font color=FF0000>for 循环一旦执行，就说明有属性，有属性就会返回 false</font>。

但是根据这个源码我们可以看出 isEmptyObject 实际上判断的并不仅仅是空对象。举个栗子：

```js
console.log( $.isEmptyObject({}) );        // true
console.log( $.isEmptyObject([]) );        // true
console.log( $.isEmptyObject(null) );      // true
console.log( $.isEmptyObject(undefin ed)); // true
console.log( $.isEmptyObject(1) );         // true
console.log( $.isEmptyObject('') );        // true
console.log( $.isEmptyObject(true) );      // true
```

以上都会返回 true。

但是既然 jQuery 是这样写，可能是因为考虑到实际开发中 isEmptyObject 用来判断 {} 和 {a: 1} 是足够的吧。如果真的是只判断 {}，完全可以结合上面写的 type 函数筛选掉不适合的情况。

##### Window 对象

Window 对象作为客户端 JavaScript 的全局对象，它有一个 window 属性指向自身，这点在[《JavaScript深入之变量对象》](https://github.com/mqyqingfeng/Blog/issues/5)中讲到过。我们可以利用这个特性判断是否是 Window 对象。

```js
function isWindow( obj ) {
    return obj != null && obj === obj.window;
}
```

##### isArrayLike

isArrayLike，看名字可能会让我们觉得这是判断类数组对象的，其实不仅仅是这样，<font color=FF0000>jQuery 实现的 isArrayLike，数组和类数组都会返回 true</font>。因为源码比较简单，我们直接看源码：

```js
function isArrayLike(obj) {
  
    // obj 必须有 length属性
    var length = !!obj && "length" in obj && obj.length;
    var typeRes = type(obj);

    // 排除掉函数和 Window 对象
    if (typeRes === "function" || isWindow(obj)) {
        return false;
    }

    return typeRes === "array" || length === 0 ||
        typeof length === "number" && length > 0 && (length - 1) in obj;
}
```

重点分析 return 这一行，使用了或语句，只要一个为 true，结果就返回 true。

**所以如果 isArrayLike 返回 true，至少要满足三个条件之一：**

1. 是数组
2. <font color=FF0000>长度为 0</font>
3. <font color=FF0000>length 属性是大于 0 的数字类型，并且 obj[length - 1] 必须存在</font>

第一个就不说了，看<font color=FF0000>**第二个条件**</font>，为什么长度为 0 就可以直接判断为 true 呢？那我们写个对象：

```js
var obj = {a: 1, b: 2, length: 0}
```

isArrayLike 函数就会返回 true，那这个合理吗？回答合不合理之前，我们先看一个例子：

```js
function a(){
    console.log(isArrayLike(arguments))
}
a();
```

如果我们去掉 length === 0 这个判断，就会打印 false，然而我们都知道 <font color=FF0000>arguments 是一个类数组对象，这里是应该返回 true 的</font>。

所以<font color=FF0000>是不是为了 **放过 “空的 arguments”** 时也放过了一些存在争议的对象呢</font>？

<font color=FF0000>**第三个条件**</font>：length 是数字，并且 length > 0 且 最后一个元素存在。为什么仅仅要求最后一个元素存在呢？

让我们先想下数组是不是可以这样写：

```js
var arr = [,,3]
```

当我们写一个对应的类数组对象就是：

```js
var arrLike = {
    2: 3,
    length: 3
}
```

也就是说当我们<font color=FF0000>**在数组中用逗号直接跳过的时候，我们认为该元素是不存在的**</font>，<font color=FF0000>类数组对象中也就不用写这个元素，但是 **最后一个元素是一定要写的，要不然 length 的长度就不会是最后一个元素的 key 值加 1**</font>。比如数组可以这样写：

```js
var arr = [1,,];
console.log(arr.length) // 2
```

但是类数组对象就只能写成：

```js
var arrLike = {
    0: 1,
    length: 1
}
```

所以符合条件的类数组对象是一定存在最后一个元素的！

##### isElement

isElement 判断是不是 DOM 元素。

```js
isElement = function(obj) {
    return !!(obj && obj.nodeType === 1);
};
```

**注：**只读属性 Node.nodeType 表示的是该节点的类型。详见：[MDN - Node.nodeType](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)

摘自：[JavaScript专题之类型判断(上)  ](https://github.com/mqyqingfeng/Blog/issues/28) 和  [JavaScript专题之类型判断(下)](https://github.com/mqyqingfeng/Blog/issues/30)



### 类数组对象

所谓的类数组对象：拥有一个 length 属性和若干索引属性的对象。

举个例子：

```js
var array = ['name', 'age', 'sex'];

var arrayLike = {
    0: 'name',
    1: 'age',
    2: 'sex',
    length: 3
}
```

即便如此，为什么叫做类数组对象呢？那让我们从读写、获取长度、遍历三个方面看看这两个对象。

##### 读写

```js
console.log(array[0]); // name
console.log(arrayLike[0]); // name

array[0] = 'new name';
arrayLike[0] = 'new name';
```

##### 长度

```js
console.log(array.length); // 3
console.log(arrayLike.length); // 3
```

##### 遍历

```js
for(var i = 0, len = array.length; i < len; i++) { …… }
for(var i = 0, len = arrayLike.length; i < len; i++) { …… }
```

是不是很像？

那类数组对象可以使用数组的方法吗？比如：`arrayLike.push('4');` 然而上述代码会报错：arrayLike.push is not a function。

#### 调用数组方法

如果类数组就是任性的想用数组的方法怎么办呢？

既然无法直接调用，我们可以用 Function.call 间接调用。

```js
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }

Array.prototype.join.call(arrayLike, '&'); // name&age&sex

Array.prototype.slice.call(arrayLike, 0); // ["name", "age", "sex"] // slice可以做到类数组转数组

Array.prototype.map.call(arrayLike, function(item){
  return item.toUpperCase();
});  // ["NAME", "AGE", "SEX"]
```

#### 类数组转数组

在上面的例子中已经提到了一种类数组转数组的方法，再补充三个：

```js
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"] 
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"] 
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"] 
// 4. apply
Array.prototype.concat.apply([], arrayLike)
```

#### Arguments 对象

Arguments 对象只定义在函数体中，包括了函数的参数和其他属性。在函数体中，arguments 指代该函数的 Arguments 对象。

举个例子：

```js
function foo(name, age, sex) { console.log(arguments); }
foo('name', 'age', 'sex')
```

<img src="https://camo.githubusercontent.com/993a101381ec9e9badf6591d841fd7deb53a7a8dde01bf17980cc2aefacc65d4/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f617267756d656e74732e706e67" alt="https://camo.githubusercontent.com/993a101381ec9e9badf6591d841fd7deb53a7a8dde01bf17980cc2aefacc65d4/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f617267756d656e74732e706e67" style="zoom: 67%;" />

我们可以看到除了类数组的 索引属性 和 length 属性之外，还有一个 callee 属性。

#### callee 属性

Arguments 对象的 callee 属性，通过它可以调用函数自身。讲个闭包经典面试题使用 callee 的解决方法：

```js
var data = [];
for (var i = 0; i < 3; i++) {
    (data[i] = function () {
       console.log(arguments.callee.i) 
    }).i = i;
}

data[0](); // 0
data[1](); // 1
data[2](); // 2
```

#### arguments 和对应参数的绑定

```js
function foo(name, age, sex, hobbit) {
    console.log(name, arguments[0]); // name name

    // 改变形参
    name = 'new name';
    console.log(name, arguments[0]); // new name new name

    // 改变arguments
    arguments[1] = 'new age';
    console.log(age, arguments[1]); // new age new age

    // 测试未传入的是否会绑定
    console.log(sex); // undefined
    sex = 'new sex';
    console.log(sex, arguments[2]); // new sex undefined

    arguments[3] = 'new hobbit';
    console.log(hobbit, arguments[3]); // undefined new hobbit
}

foo('name', 'age')
```

传入的参数，实参和 arguments 的值会共享，当没有传入时，实参与 arguments 值不会共享。除此之外，以上是在非严格模式下，如果是在严格模式下，实参和 arguments 是不会共享的。

#### arguments 应用

包括：

1. 参数不定长
2. 函数柯里化
3. 递归调用
4. 函数重载

摘自：[JavaScript深入之类数组对象与arguments](https://github.com/mqyqingfeng/Blog/issues/14)



### Promise

#### 面试题：红绿灯问题

题目：红灯三秒亮一次，绿灯一秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯？（用 Promse 实现）

三个亮灯函数已经存在：

```js
function red(){ console.log('red'); }
function green(){ console.log('green'); }
function yellow(){ console.log('yellow'); }
```

利用 then 和递归实现：

```js
// red green yellow 函数略
var light = function(timmer, cb){
    return new Promise(function(resolve, reject) {
        setTimeout(function() { cb(); resolve(); }, timmer);
    });
};

var step = function() {
    Promise.resolve().then(function(){ return light(3000, red); })
      .then(function(){ return light(2000, green); })
      .then(function(){ return light(1000, yellow); })
      .then(function(){ step(); }); // 注：递归执行
}
step();
```

#### promisify

有的时候，我们<font color=FF0000>需要将 callback 语法的 API 改造成 Promise 语法</font>，为此我们需要一个 promisify 的方法。

因为 callback 语法传参比较明确，最后一个参数传入回调函数，回调函数的第一个参数是一个错误信息，如果没有错误，就是 null，所以我们可以直接写出一个简单的 promisify 方法：

```js
function promisify(original) {
    return function (...args) {
        return new Promise((resolve, reject) => {
            args.push(function callback(err, ...values) {
                if (err) { return reject(err); }
                return resolve(...values)
            });
            original.call(this, ...args);
        });
    };
}
```

完整的可以参考 [es6-promisify](https://github.com/digitaldesignlabs/es6-promisify/blob/master/lib/promisify.js)

#### Promise 的局限性

##### 1. 错误被吃掉

首先我们要理解，什么是错误被吃掉，是指错误信息不被打印吗？并不是，举个例子：

```js
throw new Error('error');
console.log(233333);
```

在<mark>这种情况下，因为 throw error 的缘故，代码被阻断执行，并不会打印 233333</mark>，再举个例子：

```js
const promise = new Promise(null);
console.log(233333);
```

以上代码依然会被阻断执行，这是因为如果通过无效的方式使用 Promise，并且出现了一个错误阻碍了正常 Promise 的构造，结果会得到一个立刻跑出的异常，而不是一个被拒绝的 Promise。

然而再举个例子：

```js
let promise = new Promise(() => { throw new Error('error') });
console.log(2333333);
```

这次会正常的打印 `233333`，说明 <font color=FF0000>**Promise 内部的错误不会影响到 Promise 外部的代码**，而这种情况我们就通常称为 “吃掉错误”</font>。

其实这并不是 Promise 独有的局限性，try..catch 也是这样，同样会捕获一个异常并简单的吃掉错误。

而<font color=FF0000>**正是因为错误被吃掉，Promise 链中的错误很容易被忽略掉**，这也是为什么会一般推荐在 Promise 链的最后添加一个 catch 函数</font>，<font color=FF0000>因为对于一个没有错误处理函数的 Promise 链，**任何错误都会在链中被传播下去，直到你注册了错误处理函数**</font>。

##### 2. 单一值

<font color=FF0000>**Promise 只能有一个完成值或一个拒绝原因**</font>，然而<font color=FF0000>在真实使用的时候，往往需要传递多个值，一般做法都是构造一个对象或数组，然后再传递</font>，then 中获得这个值后，又会进行取值赋值的操作，每次封装和解封都无疑让代码变得笨重。

说真的，并没有什么好的方法，<font color=FF0000>建议使用 ES6 的解构赋值</font>：

```js
Promise.all([Promise.resolve(1), Promise.resolve(2)])
       .then(([x, y]) => { console.log(x, y); });
```

##### 3. 无法取消

<font color=FF0000>**Promise 一旦新建它就会立即执行，无法中途取消**</font>。

##### 4. 无法得知 pending 状态

<font color=FF0000>**当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）**</font>。

摘自：[ES6 系列之我们来聊聊 Promise](https://github.com/mqyqingfeng/Blog/issues/98)

### Generator 的自动执行

##### 关于 generator 实现异步原理的总结

> generator 可以：在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来；所以可以：<font color=FF0000 size=4>**当遇到异步函数执行的时候，将函数执行权转移出去；当异步函数执行完毕时再将执行权给转移回来**</font>。
>
> 因此在 generator 内部对于异步操作的方式，可以以同步的顺序来书写。使用这种方式需要考虑的问题是何时将函数的控制权转移回来，因此需要有一个自动执行 generator 的机制，比如说 co 模块等方式来实现 generator 的自动执行。
>
> 摘自：[「2021」高频前端面试题汇总之JavaScript篇（下）](https://juejin.cn/post/6941194115392634888)

#### 单个异步任务

```js
var fetch = require('node-fetch');

function* gen(){
    var url = 'https://api.github.com/users/github';
    var result = yield fetch(url);
    console.log(result.bio);
}
```

为了获得最终的执行结果，你需要这样做：

```js
var g = gen();         // 注：执行 generator 函数
var result = g.next(); // 注：g.next() 执行到 fetch(url)

// 注：由于返回值 result 为 { value: Promise { <pending> }, done: false }。另外，fetch 返回一个 Promise；所以 result.value 是一个 Promise。于是，再进行 next()
result.value.then(function(data) { return data.json(); })
            .then(function(data) { g.next(data); }
);
```

首先，执行 Generator 函数，获取遍历器对象。然后使用 next 方法，执行异步任务的第一阶段，即 `fetch(url)`。

注意，由于 `fetch(url)` 会返回一个 Promise 对象，所以 result 的值为：

```js
{ value: Promise { <pending> }, done: false }
```

最后我们为这个 Promise 对象添加一个 then 方法，先将其返回的数据格式化 `data.json()`；<font color=FF0000>再调用 g.next，将获得的数据传进去，由此可以执行异步任务的第二阶段，代码执行完毕</font>。

#### 多个异步任务

上节我们只调用了一个接口，那如果我们调用了多个接口，使用了多个 yield，我们岂不是要在 then 函数中不断的嵌套下去……

所以我们来看看执行多个异步任务的情况：

```js
var fetch = require('node-fetch');

function* gen() {
    var r1 = yield fetch('https://api.github.com/users/github');
    var r2 = yield fetch('https://api.github.com/users/github/followers');
    var r3 = yield fetch('https://api.github.com/users/github/repos');

    console.log([r1.bio, r2[0].login, r3[0].full_name].join('\n'));
}
```

为了获得最终的执行结果，你可能要写成：

```js
var g = gen();
var result1 = g.next();

result1.value.then(function(data){ return data.json(); })
             .then(function(data){ return g.next(data).value; }) // 注：第一个 yield 完成，返回值放入 r1
             .then(function(data){ return data.json(); })
             .then(function(data){ return g.next(data).value })  // 注：第二个 yield 完成，返回值放入 r2
             .then(function(data){ return data.json(); })
             .then(function(data){ g.next(data) });              // 注：第三个 yield 完成，返回值放入 r3
```

但我知道你肯定不想写成这样……

其实，利用递归，我们可以这样写：

```js
function run(gen) {
    var g = gen();

    function next(data) {
        var result = g.next(data);
        if (result.done) return; // 注：递归 退出条件
        result.value.then(function(data) { return data.json(); })
                    .then(function(data) { next(data); });
    }
    next();
}

run(gen);
```

其中的关键就是 yield 的时候返回一个 Promise 对象，给这个 Promise 对象添加 then 方法，当异步操作成功时执行 then 中的 onFullfilled 函数，onFullfilled 函数中又去执行 g.next，从而让 Generator 继续执行，然后再返回一个 Promise，再在成功时执行 g.next，然后再返回……

#### 启动器函数

<font color=FF0000>在 **run 这个启动器函数** 中</font>，我们在 then 函数中将数据格式化 `data.json()`；但在更广泛的情况下，比如 yield 直接跟一个 Promise，而非一个 fetch 函数返回的 Promise，因为没有 json 方法，代码就会报错。所以为了更具备通用性，连同这个例子和启动器，我们修改为：

```js
var fetch = require('node-fetch');

function* gen() {
    var r1 = yield fetch('https://api.github.com/users/github');
    // 注：response.json() 返回一个 Promise，详见下面的“注”
    var json1 = yield r1.json();
    var r2 = yield fetch('https://api.github.com/users/github/followers');
    var json2 = yield r2.json();
    var r3 = yield fetch('https://api.github.com/users/github/repos');
    var json3 = yield r3.json();

    console.log([json1.bio, json2[0].login, json3[0].full_name].join('\n'));
}

function run(gen) {
    var g = gen();

    function next(data) {
        var result = g.next(data);
        if (result.done) return;
        result.value.then(function(data) { next(data); });
    }
    next();
}

run(gen);
```

只要 yield 后跟着一个 Promise 对象，我们就可以利用这个 run 函数将 Generator 函数自动执行。**注：**

> Response  mixin 的 json() 方法接收一个 Response 流，并将其读取完成。它 <font color=FF0000>**返回一个 Promise**</font>，Promise 的解析 resolve 结果是将文本体解析为 JSON。
>
> 摘自：[MDN - Response.json()](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/json)

#### 回调函数

yield 后一定要跟着一个 Promise 对象才能保证 Generator 的自动执行吗？如果只是一个回调函数呢？我们来看个例子：

首先我们来模拟一个普通的异步请求：

```js
function fetchData(url, cb) {
    setTimeout(function(){
        cb({ status: 200, data: url })
    }, 1000)
}
```

我们将这种函数改造成：

```js
function fetchData(url) {
    return function(cb){
        setTimeout(function(){
            cb({ status: 200, data: url })
        }, 1000)
    }
}
```

对于这样的 Generator 函数：

```js
function* gen() {
    var r1 = yield fetchData('https://api.github.com/users/github');
    var r2 = yield fetchData('https://api.github.com/users/github/followers');

    console.log([ r1.data, r2.data ].join('\n'));
}
```

如果要获得最终的结果：

```js
var g = gen();
var r1 = g.next();

r1.value(function(data) {
    var r2 = g.next(data);
    r2.value(function(data) { g.next(data); });
});
```

如果写成这样的话，我们会面临跟第一节同样的问题，那就是当使用多个 yield 时，代码会循环嵌套起来……

同样利用递归，所以我们可以将其改造为：

```js
function run(gen) {
    var g = gen();

    function next(data) {
        var result = g.next(data);
        if (result.done) return;
        result.value(next);
    }
    next();
}

run(gen);
```

#### run

由此可以看到 <font color=FF0000>Generator 函数的自动执行需要一种机制，即当异步操作有了结果，能够自动交回执行权</font>。而两种方法可以做到这一点。

1. <font color=FF0000>**回调函数**</font>。将异步操作进行包装，暴露出回调函数，<font color=FF0000 size=4>**在回调函数里面交回执行权**</font>。
2. <font color=FF0000>**Promise 对象**</font>。将异步操作包装成 Promise 对象，<font color=FF0000 size=4>**用 then 方法交回执行权**</font>。

在两种方法中，我们各写了一个 run 启动器函数，那我们能不能将这两种方式结合在一些，写一个通用的 run 函数呢？我们尝试一下：

```js
// 第一版
function run(gen) {
    var gen = gen();

    function next(data) {
        var result = gen.next(data);
        if (result.done) return;
        if (isPromise(result.value)) { // 注：判断 result.value 是否是 Promise
            result.value.then(function(data) { next(data); });
        } else { result.value(next) }
    }
    next()
}

function isPromise(obj) {
    return 'function' == typeof obj.then;
}

module.exports = run;
```

其实实现的很简单，判断 result.value 是否是 Promise，是就添加 then 函数，不是就直接执行。

#### return Promise

我们<font color=FF0000>已经写了一个不错的启动器函数，**支持 yield 后跟回调函数或者 Promise 对象**</font>。

现在有一个问题需要思考，就是我们 <font color=FF0000>如何获得 Generator 函数的返回值</font>呢？又<font color=FF0000>如果 Generator 函数中出现了错误</font>，就比如 fetch 了一个不存在的接口，这个<font color=FF0000>错误该如何捕获</font>呢？

这很容易让人想到 Promise，如果这个启动器函数返回一个 Promise，我们就可以给这个 Promise 对象添加 then 函数，当所有的异步操作执行成功后，我们执行 onFullfilled 函数，如果有任何失败，就执行 onRejected 函数。

```js
// 第二版
function run(gen) {
    var gen = gen();

    return new Promise(function(resolve, reject) {
        function next(data) {
            try { var result = gen.next(data); }
            catch (e) { return reject(e); }

            if (result.done) { return resolve(result.value) };
            var value = toPromise(result.value);
            value.then(function(data) { next(data); }, 
                       function(e) { reject(e) });
        }
        next()
    })
}

function isPromise(obj) {
    return 'function' == typeof obj.then;
}

function toPromise(obj) {
    if (isPromise(obj)) return obj;  // 注：判断是否是 Promise，是则 直接返回
    if ('function' == typeof obj) return thunkToPromise(obj); // 注；不是 Promise，是（回调）函数；则用 Promise 包装
    return obj;
}

function thunkToPromise(fn) {
    return new Promise(function(resolve, reject) {
        fn(function(err, res) {
            if (err) return reject(err);
            resolve(res);
        });
    });
}

module.exports = run;
```

与第一版有很大的不同：

- 首先，我们返回了一个 Promise，当 `result.done` 为 true 的时候，我们将该值 `resolve(result.value)`；如果执行的过程中出现错误，被 catch 住，我们会将原因 `reject(e)`。

- 其次，我们会<font color=FF0000>使用 `thunkToPromise` 将回调函数包装成一个 Promise</font>，然后统一的添加 then 函数。在这里值得注意的是，在 `thunkToPromise` 函数中，我们遵循了 error first 的原则，这意味着当我们处理回调函数的情况时：

```js
// 模拟数据请求
function fetchData(url) {
    return function(cb) {
        setTimeout(function() {
            cb(null, { status: 200, data: url })
        }, 1000)
    }
}
```

#### co

<font color=FF0000>如果我们再将这个启动器函数写的完善一些，我们就**相当于写了一个 [co](https://github.com/tj/co)**</font>（注：根据 [[#async await]] 中的说法，co 是 “自动执行器”）。实际上，上面的代码确实是来自于 co。而 co 是什么？ co 是大神 TJ Holowaychuk 于 2013 年 6 月发布的一个小模块，用于 Generator 函数的自动执行。

如果直接使用 co 模块，这两种不同的例子可以简写为：

```js
var fetch = require('node-fetch');
var co = require('co');

function* gen() {
    var r1 = yield fetch('https://api.github.com/users/github');
    var json1 = yield r1.json();
    var r2 = yield fetch('https://api.github.com/users/github/followers');
    var json2 = yield r2.json();
    var r3 = yield fetch('https://api.github.com/users/github/repos');
    var json3 = yield r3.json();

    console.log([json1.bio, json2[0].login, json3[0].full_name].join('\n'));
}

co(gen);
```

```js
// yield 后是一个回调函数
var co = require('co');

function fetchData(url) {
    return function(cb) {
        setTimeout(function() {
            cb(null, { status: 200, data: url })
        }, 1000)
    }
}

function* gen() {
    var r1 = yield fetchData('https://api.github.com/users/github');
    var r2 = yield fetchData('https://api.github.com/users/github/followers');

    console.log([r1.data, r2.data].join('\n'));
}

co(gen);
```

摘自：[ES6 系列之 Generator 的自动执行](https://github.com/mqyqingfeng/Blog/issues/99)

### async await

ES2017 标准引入了 async 函数，使得异步操作变得更加方便。在异步处理上，async 函数就是 Generator 函数的语法糖。

```js
// 使用 generator
var fetch = require('node-fetch');
var co = require('co');

function* gen() {
    var r1 = yield fetch('https://api.github.com/users/github');
    var json1 = yield r1.json();
    console.log(json1.bio);
}
co(gen);
```

当你使用 async 时：

```js
// 使用 async
var fetch = require('node-fetch');

var fetchData = async function () {
    var r1 = await fetch('https://api.github.com/users/github');
    var json1 = await r1.json();
    console.log(json1.bio);
};
fetchData();
```

其实 <font color=FF0000>**async 函数的实现原理：就是将 Generator 函数 和 自动执行器，包装在一个函数里**</font>。

```js
async function fn(args) { ... }

// 等同于
function fn(args) {
  return spawn(function* () { ... });
}
```

<font color=FF0000>spawn 函数指的是 <font size=4>**自动执行器**</font>，就比如说 <font size=4>**co**</font></font>。再加上 <font color=FF0000 size=4>**async 函数返回一个 Promise 对象**</font>，你也可以理解为 async 函数是基于 Promise 和 Generator 的一层封装。

#### async 与 Promise

严谨的说：<font color=FF0000>async 是一种语法，Promise 是一个内置对象，两者并不具备可比性</font>；更何况 async 函数也返回一个 Promise 对象……

这里主要是展示一些场景，使用 async 会比使用 Promise 更优雅的处理异步流程。

##### 1. 代码更加简洁（注：下面的每个示例都是原先写法 和 使用 async 的写法比较）

```js
/* 示例一 */
function fetch() {
  return ( fetchData().then( () => { return "done" } ); )
}

async function fetch() {
  await fetchData()
  return "done"
};
```

```js
/* 示例二 */
function fetch() {
  return fetchData().then(data => {
    if (data.moreData) {
        return fetchAnotherData(data).then( moreData => { return moreData } )
    } else { return data }
  });
}

async function fetch() {
  const data = await fetchData()
  if (data.moreData) {
    const moreData = await fetchAnotherData(data);
    return moreData
  } else { return data }
};
```

```js
/* 示例三 */
function fetch() {
  return (
    fetchData().then(value1 => { return fetchMoreData(value1) })
    					 .then(value2 => { return fetchMoreData2(value2) })
  )
}

async function fetch() {
  const value1 = await fetchData()
  const value2 = await fetchMoreData(value1)
  return fetchMoreData2(value2)
};
```

##### 2. 错误处理

```js
function fetch() {
  try {
    fetchData()
      .then(result => { const data = JSON.parse(result) })
      .catch((err) => { console.log(err) })
  } catch (err) { console.log(err) }
}
```

在这段代码中，try / catch 能捕获 fetchData() 中的一些 Promise 构造错误，但是不能捕获 JSON.parse 抛出的异常，如果要处理 JSON.parse 抛出的异常，需要添加 catch 函数重复一遍异常处理的逻辑。在实际项目中，错误处理逻辑可能会很复杂，这会导致冗余的代码。

```js
async function fetch() {
  try { const data = JSON.parse(await fetchData()) }
  catch (err) { console.log(err) }
};
```

async / await 的出现使得 try/catch 就可以捕获同步和异步的错误。**注：**关于 async / await 可以使用 [await-to-js](https://github.com/scopsy/await-to-js) 更优雅地捕获异常

##### 3. 调试

```js
const fetchData = () => new Promise((resolve) => setTimeout(resolve, 1000, 1))
const fetchMoreData = (value) => new Promise((resolve) => setTimeout(resolve, 1000, value + 1))
const fetchMoreData2 = (value) => new Promise((resolve) => setTimeout(resolve, 1000, value + 2))

function fetch() {
  return (
    fetchData()
    .then((value1) => { console.log(value1) return fetchMoreData(value1) })
    .then(value2 => { return fetchMoreData2(value2) })
  )
}
const res = fetch();
console.log(res);
```

![https://camo.githubusercontent.com/38c17c920b970173d0b8ba41f26edf9e41cefdf9db6d4c7466333b6b137e1eef/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f4553362f6173796e632f70726f6d6973652e676966](https://s2.loli.net/2022/04/19/MQWNvKT1n9Ryawh.gif)

因为 then 中的代码是异步执行，所以当你打断点的时候，代码不会顺序执行，尤其当你使用 step over 的时候，then 函数会直接进入下一个 then 函数。

```js
const fetchData = () => new Promise((resolve) => setTimeout(resolve, 1000, 1))
const fetchMoreData = () => new Promise((resolve) => setTimeout(resolve, 1000, 2))
const fetchMoreData2 = () => new Promise((resolve) => setTimeout(resolve, 1000, 3))

async function fetch() {
  const value1 = await fetchData()
  const value2 = await fetchMoreData(value1)
  return fetchMoreData2(value2)
};
const res = fetch();
console.log(res);
```

![https://camo.githubusercontent.com/8348dc27d42ca5eff110cbe13a86979871721da5cca45fe99793acf4cd23450a/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f4553362f6173796e632f6173796e632e676966](https://s2.loli.net/2022/04/19/zsL9Ud483SjKkPr.gif)

而使用 async 的时候，则可以像调试同步代码一样调试。

#### async 地狱

async 地狱主要是<mark>指开发者贪图语法上的简洁而让原本可以并行执行的内容变成了顺序执行，从而影响了性能</mark>，但用地狱形容有点夸张了

##### 例子一

举个例子：

```js
(async () => {
  const getList = await getList();
  const getAnotherList = await getAnotherList();
})();
```

`getList()` 和 `getAnotherList()` 其实并没有依赖关系，但是现在的这种写法，虽然简洁，却导致了 `getAnotherList()` 只能在 `getList()` 返回后才会执行，从而导致了多一倍的请求时间。为了解决这个问题，我们可以改成这样：

```js
(async () => {
  const listPromise = getList();
  const anotherListPromise = getAnotherList();
  await listPromise;
  await anotherListPromise;
})();
```

也可以使用 Promise.all()：

```js
(async () => {
  Promise.all([getList(), getAnotherList()]).then(...);
})();
```

##### 例子二

当然上面这个例子比较简单，我们再来扩充一下：

```js
(async () => {
  const listPromise = await getList();
  const anotherListPromise = await getAnotherList();

  // do something

  await submit(listData);
  await submit(anotherListData);
})();
```

因为 await 的特性，整个例子有明显的先后顺序，然而 `getList()` 和 `getAnotherList()` 其实并无依赖，`submit(listData)` 和 `submit(anotherListData)` 也没有依赖关系，那么对于这种例子，我们该怎么改写呢？

**基本分为三个步骤：**

1. **找出依赖关系：**在这里，submit(listData) 需要在 getList() 之后，submit(anotherListData) 需要在 anotherListPromise() 之后。

2. **将互相依赖的语句包裹在 async 函数中**

   ```js
   async function handleList() {
     const listPromise = await getList();
     // ...
     await submit(listData);
   }
   
   async function handleAnotherList() {
     const anotherListPromise = await getAnotherList()
     // ...
     await submit(anotherListData)
   }
   ```

3. **并发执行 async 函数**

   ```js
   async function handleList() {
     const listPromise = await getList();
     // ...
     await submit(listData);
   }
   
   async function handleAnotherList() {
     const anotherListPromise = await getAnotherList()
     // ...
     await submit(anotherListData)
   }
   
   // 方法一
   (async () => {
     const handleListPromise = handleList()
     const handleAnotherListPromise = handleAnotherList()
     await handleListPromise
     await handleAnotherListPromise
   })()
   
   // 方法二
   (async () => {
     Promise.all([handleList(), handleAnotherList()]).then()
   })()
   ```

#### 继发与并发

##### 问题：给定一个 URL 数组，如何实现接口的 继发 和 并发 ？

**async 继发实现：**

```js
// 继发一
async function loadData() {
  var res1 = await fetch(url1);
  var res2 = await fetch(url2);
  var res3 = await fetch(url3);
  return "whew all done";
}
// 继发二
async function loadData(urls) {
  for (const url of urls) {
    const response = await fetch(url);
    console.log(await response.text());
  }
}
```

**async 并发实现：**

```js
// 并发一
async function loadData() {
  var res = await Promise.all([fetch(url1), fetch(url2), fetch(url3)]);
  return "whew all done";
}
// 并发二
async function loadData(urls) {
  // 并发读取 url
  const textPromises = urls.map(async url => {
    const response = await fetch(url);
    return response.text();
  });
  // 按次序输出
  for (const textPromise of textPromises) {
    console.log(await textPromise);
  }
}
```

#### async 错误捕获

尽管我们可以使用 try catch 捕获错误，但是当我们需要捕获多个错误并做不同的处理时，很快 try catch 就会导致代码杂乱，就比如：

```js
async function asyncTask(cb) {
    try {
       const user = await UserModel.findById(1);
       if(!user) return cb('No user found');
    } catch(e) {
        return cb('Unexpected error occurred');
    }

    try {
       const savedTask = await TaskModel({userId: user.id, name: 'Demo Task'});
    } catch(e) {
        return cb('Error occurred while saving task');
    }

    if(user.notificationsEnabled) {
        try {
            await NotificationService.sendNotification(user.id, 'Task Created');
        } catch(e) {
            return cb('Error while sending notification');
        }
    }

    if(savedTask.assignedUser.id !== user.id) {
        try {
            await NotificationService.sendNotification(savedTask.assignedUser.id, 'Task was created for you');
        } catch(e) {
            return cb('Error while sending notification');
        }
    }

    cb(null, savedTask);
}
```

为了简化这种错误的捕获，我们可以给 await 后的 promise 对象添加 catch 函数，为此我们需要写一个 helper：

```js
// to.js
export default function to(promise) {
   return promise.then(data => {
      return [null, data];
   })
   .catch(err => [err]);
}
```

整个错误捕获的代码可以简化为：

```js
import to from './to.js';

async function asyncTask() {
     let err, user, savedTask;

     [err, user] = await to(UserModel.findById(1));
     if(!user) throw new CustomerError('No user found');

     [err, savedTask] = await to(TaskModel({userId: user.id, name: 'Demo Task'}));
     if(err) throw new CustomError('Error occurred while saving task');

    if(user.notificationsEnabled) {
       const [err] = await to(NotificationService.sendNotification(user.id, 'Task Created'));
       if (err) console.error('Just log the error and continue flow');
    }
}
```

#### async 的一些讨论

##### async 会取代 Generator 吗？

Generator 本来是用作生成器，使用 Generator 处理异步请求只是一个比较 hack 的用法。在异步方面，async 可以取代 Generator，但是 <font color=FF0000>async 和 Generator 两个语法本身是用来解决不同的问题的</font>。

##### async 会取代 Promise 吗？

1. async 函数返回一个 Promise 对象
2. 面对复杂的异步流程，Promise 提供的 all 和 race 会更加好用
3. Promise 本身是一个对象，所以可以在代码中任意传递
4. async 的支持率还很低，即使有 Babel，编译后也要增加 1000 行左右。

摘自：[ES6 系列之我们来聊聊 Async](https://github.com/mqyqingfeng/Blog/issues/100)

#### 《 JavaScript 高级程序设计》第四版 中的 async await

async 关键字用于声明异步函数。另外，async 也可以用在类定义中

```js
class Foo {
  async foo() {}
}
```

<font color=FF0000>使用 async 关键字可以让函数具有异步特征，但总体上其代码仍然是同步求值的</font>。而在参数或闭包方面，异步函数仍然具有普通 JavaScript 函数的正常行为。正如下面的例子所示，foo() 函数仍然会在后面的指令之前被求值：

```js
async function foo() { console.log(1); }
foo();
console.log(2);
// 1, 2
```

不过，<font color=FF0000>如果 异步函数使用 return 关键字返回了值（ 如果没有 return 则会返回 undefined ），这 个值会被 Promise.resolve() 包装成一个 Promise 对象</font>。异步函数 始终返回 Promise 对象。在函数外部调用这个函数可以得到它返回的 Promise ：

```js
async function foo() {
  console.log(1);
  return 3;
}
// 给返回的 Promise 添加一个解决处理程序
foo().then(console.log);
console.log(2);
// 1, 2, 3
```

当然，直接返回一个 Promise 对象也是一样的：
```js
async function foo() {
  console.log(1);
  return Promise.resolve(3);
}
// 给返回的 Promise 添加一个解决处理程序
foo().then(console.log);
console.log(2);
// 1, 2, 3
```

异步函数的返回值期待（ 但实际上并不要求 ）一个实现 thenable 接口的对象，但常规的值也可以。 如果返回的是实现 thenable 接口的对象，则这个对象可以由提供给 then() 的处理程序 “解包”（**注：**即 .then(fn) 中的函数（即这里的 fn）可以对 Promise 进行解包，获取 Promise 的内容）。如果不是，则返回值就被当作 resolved 的 Promise。

```js
// 返回一个原始值
async function foo() {  return 'foo'; }
foo().then(val => console.log(val)); // foo。注：如果 typeof val 结果是 string

// 返回一个实现了 thenable 接口的非 Promise 对象
async function baz() {
  const thenable = { 
    then(callback) { callback('baz'); }
  }; 
  return thenable;
}
baz().then(console.log); // baz

// 返回一个 Promise
async function qux() {
  return Promise.resolve('qux');
}
qux().then(console.log); // qux
```

与在 Promise 处理程序中一样，在异步函数中 抛出错误 会返回拒绝的 Promise：

```js
async function foo() {
  console.log(1);
  throw 3; 
}
// 给返回的 Promise 添加一个拒绝处理程序
foo().catch(console.log); // 注：这里 foo() 返回的，依然是一个 Promise
console.log(2);
// 1, 2, 3
```

不过，rejected 的 Promise 的错误不会被异步函数捕获：
```js
async function foo() {
  console.log(1);
  Promise.reject(3);
}
// Attach a rejected handler to the returned promise
foo().catch(console.log);
console.log(2);
// 1, 2, Uncaught (in promise): 3
```

##### await

await 关键字期待（ 但实际上并不要求 ）一个实现 thenable 接口的对象，但常规的值也可以。如果是实现 thenable 接口的对象，则<font color=FF0000>这个对象可以由 await 来 “解包”</font>（**注：**解包 Promise ）。如果不是，则这个值就被当作 resolved 的 Promise

```js
// 等待一个原始值
async function foo() { console.log( await 'foo' ); }
foo(); // foo

// 等待一个实现了 thenable 接口的非 Promise 对象
async function baz() { 
  const thenable = {
    then(callback) { callback('baz'); }
  };
  console.log(await thenable);
}
baz(); // baz

// 等待一个 Promise
async function qux() {
  console.log(await Promise.resolve('qux'));
}
qux(); // qux
```

等待会抛出错误的同步操作，会返回 rejected 的 Promise：

```js
async function foo() {
  console.log(1);
  await (() => { throw 3; })(); 
}
// 给返回的 Promise 添加一个拒绝处理程序
foo().catch(console.log);
console.log(2);
// 1, 2, 3
```

如前面的例子所示，<font color=FF0000>单独的 Promise.reject() 不会被异步函数捕获，而会抛出未捕获错误</font>。不过，对 rejected 的 Promise 使用 await 则会释放 ( unwrap ) 错误值（将 rejected 的 Promise 返回 ）：
```js
async function foo() {
  console.log(1);
  await Promise.reject(3);
  console.log(4); // 这行代码不会执行
}
// 给返回的 Promise 添加一个拒绝处理程序
foo().catch(console.log);
console.log(2);
// 1, 2, 3
```

JavaScript 运行时在碰 到 await 关键字时，会记录在哪里暂停执行。等到 await 右边的值可用了，JavaScript 运行时会向消息队列中推送一个任务，这个任务会恢复异步函数的执行。

另外，TC39 对 await 做了修改，下例中 await Promise.resolve('foo') <font color=FF0000>只会生成一个异步任务</font>，和 await 'bar' 一样；所以先于后者执行。

```js
async function foo() { console.log(await Promise.resolve('foo')); }
async function bar() { console.log(await 'bar'); }
async function baz() { console.log('baz'); }
foo();
bar();
baz();
// baz, foo, bar
```

摘自：《 JavaScript 高级程序设计》第四版 §11.3.1 P348



### 柯里化

#### 定义

在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。

举个例子：

```js
function add(a, b) { return a + b; }

// 执行 add 函数，一次传入两个参数即可
add(1, 2) // 3

// 假设有一个 curry 函数可以做到柯里化
var addCurry = curry(add);
addCurry(1)(2) // 3
```

#### 用途

我们会讲到如何写出这个 curry 函数，并且会将这个 curry 函数写的很强大，但是在编写之前，我们需要知道柯里化到底有什么用？

举个例子：

```js
// 示意而已
function ajax(type, url, data) {
    var xhr = new XMLHttpRequest();
    xhr.open(type, url, true);
    xhr.send(data);
}

// 虽然 ajax 这个函数非常通用，但在重复调用的时候参数冗余
ajax('POST', 'www.test.com', "name=kevin")
ajax('POST', 'www.test2.com', "name=kevin")
ajax('POST', 'www.test3.com', "name=kevin")

// 利用 curry
var ajaxCurry = curry(ajax);

// 以 POST 类型请求数据
var post = ajaxCurry('POST');
post('www.test.com', "name=kevin");

// 以 POST 类型请求来自于 www.test.com 的数据
var postFromTest = post('www.test.com');
postFromTest("name=kevin");
```

想想 jQuery 虽然有 \$.ajax 这样通用的方法，但是也有 \$.get 和 \$.post 的语法糖。(当然 jQuery 底层是否是这样做的，我就没有研究了)。

<font color=FF0000>curry 的这种用途可以理解为：**参数复用**。**本质上是降低通用性，提高适用性**</font>。可是即便如此，是不是依然感觉没什么用呢？

如果我们仅仅是把参数一个一个传进去，意义可能不大，但是如果我们是把柯里化后的函数传给其他函数比如 map 呢？

举个例子：比如我们有这样一段数据：

```js
var person = [{name: 'kevin'}, {name: 'daisy'}]
```

如果我们要获取所有的 name 值，我们可以这样做：

```js
var name = person.map(function (item) {
    return item.name;
})
```

不过如果我们有 curry 函数：

```js
var prop = curry(function (key, obj) {
    return obj[key]
});
var name = person.map(prop('name'))
```

我们为了获取 name 属性还要再编写一个 prop 函数，是不是又麻烦了些？

但是要注意，prop 函数编写一次后，以后可以多次使用，实际上代码从原本的三行精简成了一行，而且你看代码是不是更加易懂了？

`person.map(prop('name'))` 就好像直白的告诉你：person 对象遍历 ( map ) 获取 ( prop ) name 属性。是不是感觉有点意思了呢？

#### 第一版

未来我们会接触到更多有关柯里化的应用，不过那是未来的事情了，现在我们该编写这个 curry 函数了。

一个经常会看到的 curry 函数的实现为：

```js
// 第一版
var curry = function (fn) { // 注：这里只知道使用者会传入 fn，还有其他的参数不知道，通过 arguments 处理。
    var args = [].slice.call(arguments, 1); // 注：截取 1 ~ length-1 的数组，即非 fn 的预设（默认）元素。
    return function() {
        var newArgs = args.concat([].slice.call(arguments)); // 注：将默认参数 和 新传入的实参合并
        return fn.apply(this, newArgs); // 注：通过 apply 调用 fn。另外，经过实验发现，使用 call 和 apply，若（总传入的）实参个数超出 被调用函数形参的个数，多余的实参会被抛弃
    };
};
```

我们可以这样使用：

```js
function add(a, b) { return a + b; }

var addCurry = curry(add, 1, 2);
addCurry() // 3
//或者
var addCurry = curry(add, 1);
addCurry(2) // 3
//或者
var addCurry = curry(add);
addCurry(1, 2) // 3
```

已经有柯里化的感觉了，但是还没有达到要求；不过我们可以把（注：第一版）这个函数用作辅助函数，帮助我们写真正的 curry 函数。

#### 第二版

```js
// 第二版
function sub_curry(fn) { // 注：第一版的代码，作为辅助函数
    var args = [].slice.call(arguments, 1);
    return function() {
        return fn.apply(this, args.concat([].slice.call(arguments)));
    };
}

function curry(fn, length) {
    // 注：function.length 表示形参个数，第一次调用 curry，没传递 length，所以 length 为 fn.length。因为下面有递归调用 curry，所以“可能”会有第二次调用 curry。递归调用时，会传递 length；所以这时候，length 是传来的实参，不是 fn.length。另外，这里传来的 length 也影响了是否继续递归的判断
    length = length || fn.length;

    return function() {
        if (arguments.length < length) { // 注：这里的 length 表示 还能传递多少实参
            // 注：调用 sub_curry 第一个参数就是 fn，所以放在第一个，后面的参数作为“默认参数”
            var combined = [fn].concat([].slice.call(arguments));
            // 注：递归调用 curry 函数。length - arguments.length 作为 curry 函数的形参 length 对应的实参，表示：还可以传
            // 递多少实参
            return curry( sub_curry.apply( this, combined), length - arguments.length );
        } else { // 注：传递了足够的参数，则停止递归；调用 fn
            return fn.apply(this, arguments);
        }
    };
}
```

#### 第二版更易懂的实现

当然了，如果你觉得无法理解，你可以选择下面这种实现方式，可以实现同样的效果：

```js
function curry(fn, args) {
    var length = fn.length;
    args = args || [];

    return function() {
        var _args = args.slice(0),
            arg, i;

        for (i = 0; i < arguments.length; i++) {
            arg = arguments[i];
            _args.push(arg);
        }
        if (_args.length < length) {
            return curry.call(this, fn, _args);
        }
        else {
            return fn.apply(this, _args);
        }
    }
}

var fn = curry(function(a, b, c) {
    console.log([a, b, c]);
});

fn("a", "b", "c") // ["a", "b", "c"]
fn("a", "b")("c") // ["a", "b", "c"]
fn("a")("b")("c") // ["a", "b", "c"]
fn("a")("b", "c") // ["a", "b", "c"]
```

#### 第三版

curry 函数写到这里其实已经很完善了，但是注意这个函数的传参顺序必须是从左到右，根据形参的顺序依次传入，如果我不想根据这个顺序传呢？我们<font color=FF0000>可以创建一个占位符</font>，比如这样（**注：**偏函数中有类似操作）：

```js
var fn = curry(function(a, b, c) {
    console.log([a, b, c]);
});
fn("a", _, "c")("b") // ["a", "b", "c"]
```

我们直接看第三版的代码：

```js
// 第三版
function curry(fn, args, holes) {
    length = fn.length;
    args = args || [];
    holes = holes || [];

    return function() {
        var _args = args.slice(0),
            _holes = holes.slice(0),
            argsLen = args.length,
            holesLen = holes.length,
            arg, i, index = 0;

        for (i = 0; i < arguments.length; i++) {
            arg = arguments[i];
            // 处理类似 fn(1, _, _, 4)(_, 3) 这种情况，index 需要指向 holes 正确的下标
            if (arg === _ && holesLen) {
                index++
                if (index > holesLen) {
                    _args.push(arg);
                    _holes.push(argsLen - 1 + index - holesLen)
                }
            }
            // 处理类似 fn(1)(_) 这种情况
            else if (arg === _) {
                _args.push(arg);
                _holes.push(argsLen + i);
            }
            // 处理类似 fn(_, 2)(1) 这种情况
            else if (holesLen) {
                // fn(_, 2)(_, 3)
                if (index >= holesLen) {
                    _args.push(arg);
                }
                // fn(_, 2)(1) 用参数 1 替换占位符
                else {
                    _args.splice(_holes[index], 1, arg);
                    _holes.splice(index, 1)
                }
            }
            else {
                _args.push(arg);
            }
        }
        if (_holes.length || _args.length < length) {
            return curry.call(this, fn, _args, _holes);
        }
        else {
            return fn.apply(this, _args);
        }
    }
}

var _ = {}; // 注：和偏函数中实现类似
var fn = curry(function(a, b, c, d, e) {
    console.log([a, b, c, d, e]);
});

// 验证 输出全部都是 [1, 2, 3, 4, 5]
fn(1, 2, 3, 4, 5);
fn(_, 2, 3, 4, 5)(1);
fn(1, _, 3, 4, 5)(2);
fn(1, _, 3)(_, 4)(2)(5);
fn(1, _, _, 4)(_, 3)(2)(5);
fn(_, 2)(_, _, 4)(1)(3)(5)
```

#### 评论区中的补充

##### 柯里化的高颜值写法

```js
var curry = fn =>
    judge = (...args) =>
        args.length === fn.length
            ? fn(...args)
            : (arg) => judge(...args, arg)
```

##### 柯里化的原理总结

用闭包把参数保存起来，当参数的数量足够执行函数了，就开始执行函数。

摘自：[JavaScript专题之函数柯里化](https://github.com/mqyqingfeng/Blog/issues/42)

#### 《现代 JS 教程》中的柯里化

柯里化是一种函数的转换  ，它是指将一个函数从可调用的 `f(a, b, c)` 转换为可调用的 `f(a)(b)(c)` 。

柯里化不会调用函数。它只是对函数进行转换。

##### 高级柯里化实现

```js
function curry(func) {

  return function curried(...args) {
    if (args.length >= func.length) { // 注：如果（多次调用的）arguments 合并的长度够了，则运行函数；否则，函数继续等待调用
      return func.apply(this, args); // 注：执行函数
    } else {
      return function(...args2) { // 注：继续等待调用
        // 注：递归。在递归前，将 args 和 args2 合并，并替代 args，用于递归判断
        return curried.apply(this, args.concat(args2));
      }
    }
  }
}
```

当我们运行它时，这里有两个 `if` 执行分支：

1. 如果传入的 `args` 长度与原始函数所定义的（`func.length`）相同或者更长，那么只需要使用 `func.apply` 将调用传递给它即可。
2. 否则，获取一个偏函数：我们目前还没调用 `func`。取而代之的是，返回另一个包装器 `pass`，它将重新应用 `curried`，将之前传入的参数与新的参数一起传入。

然后，如果我们再次调用它，我们将得到一个新的偏函数（如果没有足够的参数），或者最终的结果。

##### 注意 ⚠️

柯里化要求函数具有固定数量的参数。使用 rest 参数的函数，例如 `f(...args)`，不能以这种方式进行柯里化。

摘自：[现代 JS 教程 - 柯里化（Currying）](https://zh.javascript.info/currying-partials)

#### 维基百科中的柯里化

在「计算机科学」中，柯里化（英语：Currying），又译为卡瑞化或加里化，是<font color=FF0000>把 **接受多个参数的函数** 变换成 **接受一个单一参数**（最初函数的第一个参数）**的函数**，并且返回接受余下的参数 而且 <font size=4>**返回结果的新函数**</font> 的技术</font>（**注：**这是核心概念。注意：是 一种样式的函数 转变为 另一种样式函数）。这个技术由克里斯托弗·斯特雷奇以逻辑学家哈斯凯尔·加里命名的，尽管它是 Moses Schönfinkel 和 戈特洛布·弗雷格 发明的。

在「直觉」上，柯里化声称 “如果你固定某些参数，你将得到接受余下参数的一个函数”。所以对于有两个变量的函数 $y^x$，如果固定了 $y=2$，则得到有一个变量的函数 $2^x$（**注：**有点像偏函数）。

在「理论计算机科学」中，柯里化提供了在简单的理论模型中，比如：只接受一个单一参数的 lambda 演算中，研究带有多个参数的函数的方式。

<font color=FF0000>函数柯里化的对偶是 Uncurrying</font>，一种<font color=FF0000>使用匿名单参数函数来实现多参数函数的方法</font>。例如：

```js
var foo = function(a) {
  return function(b) {
    return a * a + b * b;
  }
}
```

这样调用上述函数：`(foo(3))(4)`，或直接`foo(3)(4)`。

摘自：[wikipedia - 柯里化](https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96)



### 偏函数

维基百科中对偏函数 ( Partial application ) 的定义为：

> In computer science, partial application ( or partial function application ) refers to the process of <mark>fixing a number of arguments to a function</mark>, producing another function of smaller arity.

**翻译成中文：**在计算机科学中，局部应用是指 <font color=FF0000>固定一个函数的一些参数</font>，<font color=FF0000>然后产生另一个更小元的函数</font>。

什么是元？<font color=FF0000>元是指函数参数的个数</font>，比如一个带有两个参数的函数被称为二元函数。

举个简单的例子：

```js
function add(a, b) { return a + b; }

// 执行 add 函数，一次传入两个参数即可
add(1, 2) // 3

// 假设有一个 partial 函数可以做到局部应用
var addOne = partial(add, 1);
addOne(2) // 3
```

个人觉得翻译成 “局部应用” 或许更贴切些，以下全部使用“局部应用”。

#### 柯里化与局部应用

如果看过上一篇文章[《JavaScript专题之柯里化》](https://github.com/mqyqingfeng/Blog/issues/42)，实际上你会发现这个例子和柯里化太像了，所以两者到底有什么区别？其实很明显：

- 柯里化是将一个多参数函数转换成多个单参数函数，也就是<font color=FF0000>将一个 n 元函数转换成 n 个一元函数</font>。

- 局部应用则是固定一个函数的一个或者多个参数，也就是<font color=FF0000>将一个 n 元函数转换成一个 n - x 元函数</font>。

如果说两者有什么关系的话，引用 [functional-programming-jargon](https://github.com/hemanth/functional-programming-jargon#partial-application) 中的描述就是：

> Curried functions are automatically partially applied.

#### partial

我们今天的目的是模仿 underscore 写一个 partial 函数，比起 curry 函数，这个显然简单了很多。

也许你在想我们可以直接使用 bind 呐，举个例子：

```js
function add(a, b) { return a + b; }
var addOne = add.bind(null, 1);

addOne(2) // 3
```

<font color=FF0000>然而使用 bind 我们还是改变了 this 指向</font>，我们<font color=FF0000>要写一个不改变 this 指向的方法</font>。

#### 第一版

根据之前的表述，我们可以尝试着写出第一版：

```js
// 第一版
// 似曾相识的代码
function partial(fn) {
    var args = [].slice.call(arguments, 1);
    return function() {
        var newArgs = args.concat([].slice.call(arguments));
        return fn.apply(this, newArgs);
    };
};
```

我们来写个 demo 验证下 this 的指向：

```js
function add(a, b) { return a + b + this.value;}

// var addOne = add.bind(null, 1);
var addOne = partial(add, 1);

var value = 1;
var obj = {
    value: 2,
    addOne: addOne
}
obj.addOne(2); // 使用 bind 时，结果为 4；使用 partial 时，结果为 5
```

#### 第二版

然而<font color=FF0000>正如 curry 函数可以使用占位符</font>（**注：**占位符，即：这个（被占位符占位的）参数先不赋值，接着赋后面参数的值；占位符占位的参数，等返回的函数被调用时，再赋值）一样，我们希望 partial 函数也可以实现这个功能，我们再来写第二版：

```js
// 第二版
var _ = {}; // ⭐️

function partial(fn) {
    // 将 arguments 获取第二个到最后一个元素组成数组
    var args = [].slice.call(arguments, 1);
    return function() {
        var position = 0, len = args.length;
        for(var i = 0; i < len; i++) {
            // 注：这是将之前占位的位置，补齐；positon 用于记录占位符的个数。另外，这里 args 可以访问是因为“闭包”
            args[i] = args[i] === _ ? arguments[position++] : args[i]
        }
        // 注：这一步没看懂，应该是将“最后一个非占位参数”后面的占位符不全。感觉是处理边界情况，不过没有找到是什么边界情况...
        while(position < arguments.length) {
          args.push(arguments[position++]);
        }
        return fn.apply(this, args);
    };
};
```

我们验证一下：

```js
var subtract = function(a, b) { return b - a; };
subFrom20 = partial(subtract, _, 20);
subFrom20(5);
```

##### 写在最后

值得注意的是：underscore 和 lodash 都提供了 partial 函数，但只有 lodash 提供了 curry 函数。

摘自：[JavaScript专题之偏函数](https://github.com/mqyqingfeng/Blog/issues/43)



### 惰性函数

**需求：**我们现在需要写一个 foo 函数，这个函数返回首次调用时的 Date 对象，注意是首次。

##### 解决一：普通方法

```js
var t;
function foo() {
    if (t) return t;
    t = new Date()
    return t;
}
```

问题有两个，一是<font color=FF0000>污染了全局变量</font>，二是<font color=FF0000>每次调用 foo 的时候都需要进行一次判断</font>。

##### 解决二：闭包

我们很容易想到<font color=FF0000>用闭包避免污染全局变量</font>。

```js
var foo = (function() { // 注：使用 IIFE
    var t;
    return function() {
        if (t) return t;
        t = new Date();
        return t;
    }
})();
```

然而<font color=FF0000>还是没有解决调用时都必须进行一次判断的问题</font>。

##### 解决三：函数对象

函数也是一种对象，利用这个特性，我们也可以解决这个问题。

```js
function foo() {
    if (foo.t) return foo.t;
    foo.t = new Date();
    return foo.t;
}
```

依旧没有解决调用时都必须进行一次判断的问题。

#### 解决四：惰性函数

不错，惰性函数就是解决每次都要进行判断的这个问题，解决 <font color=FF0000>**原理很简单，<font size=4>重写函数</font>**</font>。

```js
var foo = function() {
    var t = new Date();
    foo = function() { // 注：这里形成闭包
        return t;      // 注：形成闭包
    };
    return foo();
};
```

#### 更多应用

DOM 事件添加中，为了<mark>兼容现代浏览器和 IE 浏览器</mark>，我们需要对浏览器环境进行一次判断：

```js
// 简化写法
function addEvent (type, el, fn) {
    if (window.addEventListener) {
        el.addEventListener(type, fn, false);
    }
    else if(window.attachEvent){
        el.attachEvent('on' + type, fn);
    }
}
```

问题在于我们每当使用一次 addEvent 时都会进行一次判断。

<font color=FF0000>**利用惰性函数**</font>，我们可以这样做：

```js
function addEvent (type, el, fn) {
    if (window.addEventListener) {
        addEvent = function (type, el, fn) {     // 改写函数
            el.addEventListener(type, fn, false);
        }
    }
    else if(window.attachEvent){
        addEvent = function (type, el, fn) {     // 改写函数
            el.attachEvent('on' + type, fn);
        }
    }
}
```

当然我们也可以使用闭包的形式：

```js
var addEvent = (function(){
    if (window.addEventListener) {
        return function (type, el, fn) {
            el.addEventListener(type, fn, false);
        }
    }
    else if(window.attachEvent){
        return function (type, el, fn) {
            el.attachEvent('on' + type, fn);
        }
    }
})();
```

当我们每次都需要进行条件判断，其实 <font color=FF0000>**只需要判断一次，接下来的使用方式都不会发生改变的时候，想想是否可以考虑使用惰性函数**</font>。

摘自：[JavaScript专题之惰性函数](https://github.com/mqyqingfeng/Blog/issues/44)



### 函数记忆 ( Memoization )

#### 定义

函数记忆是指<font color=FF0000>将上次的计算结果缓存起来，当下次调用时，如果遇到相同的参数，就直接返回缓存中的数据</font>。举个例子：

```js
function add(a, b) { return a + b; }

// 假设 memoize 可以实现函数记忆
var memoizedAdd = memoize(add);

memoizedAdd(1, 2) // 3
memoizedAdd(1, 2) // 相同的参数，第二次调用时，从缓存中取出数据，而非重新计算一次
```

#### 原理

实现这样一个 memoize 函数很简单，<font color=FF0000>原理上只用 **把 参数 和 对应的结果 数据存到一个对象中**</font>。调用时，判断参数对应的数据是否存在，存在就返回对应的结果数据。

#### 第一版

我们来写一版：

```js
// 第一版 (来自《JavaScript权威指南》) 注：《JS权威指南》第七版 §8.4.4 找到了类似代码，不过不完全一样；比如 cache 用 Map 存放 
function memoize(f) {
    var cache = {};
    return function(){
        var key = arguments.length + Array.prototype.join.call(arguments, ",");
        if (key in cache) {
            return cache[key]
        }
        else {
            return cache[key] = f.apply(this, arguments)
        }
    }
}
```

我们来测试一下：

```js
var add = function(a, b, c) { return a + b + c }

var memoizedAdd = memoize(add)

console.time('use memoize')
for(var i = 0; i < 100000; i++) { memoizedAdd(1, 2, 3) }
console.timeEnd('use memoize')

console.time('not use memoize')
for(var i = 0; i < 100000; i++) { add(1, 2, 3) }
console.timeEnd('not use memoize')
```

在 Chrome 中，使用 memoize 大约耗时 60ms，如果我们不使用函数记忆，大约耗时 1.3 ms 左右。

#### 注意

什么，我们使用了看似高大上的函数记忆，结果却更加耗时，这个例子近乎有 60 倍呢！所以，函数记忆也并不是万能的，你看这个简单的场景，其实并不适合用函数记忆。

需要注意的是，<font color=FF0000>函数记忆只是一种编程技巧，本质上是 **牺牲算法的空间复杂度** 以 **换取更优的时间复杂度**</font>，在客户端 JavaScript 中代码的执行时间复杂度往往成为瓶颈，因此在大多数场景下，这种牺牲空间换取时间的做法以提升程序执行效率的做法是非常可取的。

#### 第二版

因为 <font color=FF0000>第一版使用了 join 方法</font>，我们很容易想到<font color=FF0000>当参数是对象的时候，就会自动调用 toString 方法转换成 `[Object object]`，再拼接字符串作为 key 值</font>。我们写个 demo 验证一下这个问题：

```js
var propValue = function(obj){ return obj.value }

var memoizedAdd = memoize(propValue)

console.log(memoizedAdd({value: 1})) // 1
console.log(memoizedAdd({value: 2})) // 1
```

两者都返回了 1，显然是有问题的，所以我们看看 underscore 的 memoize 函数是如何实现的：

```js
// 第二版 (来自 underscore 的实现)
var memoize = function(func, hasher) {
    var memoize = function(key) { // 注：因为作用域，所以函数内还可以定义哥 memoize
        var cache = memoize.cache; // 注：memoize 在下面定义
        var address = '' + (hasher ? hasher.apply(this, arguments) : key); // 注：因为没有传 hasher，这里是有问题的；形参 key 对应的是第一个实参；在下面两次运行中都是 1。正因为此，导致了下面(1, 2, 3) 和 (1, 2, 4) 结果都是 6
        if (!cache[address]) {
            cache[address] = func.apply(this, arguments);
        }
        return cache[address];
    };
    memoize.cache = {};
    return memoize;
};
```

从这个实现可以看出，underscore 默认使用 function 的第一个参数作为 key，所以如果直接使用

```js
var add = function(a, b, c) { return a + b + c }

var memoizedAdd = memoize(add) // 注：在这里的调用中，hasher 为 undefined

memoizedAdd(1, 2, 3) // 6
memoizedAdd(1, 2, 4) // 6
```

肯定是有问题的，如果要支持多参数，我们就需要传入 hasher 函数，自定义存储的 key 值。所以我们考虑使用 JSON.stringify：

```js
var memoizedAdd = memoize(add, function(){
    var args = Array.prototype.slice.call(arguments)
    return JSON.stringify(args)
})

console.log(memoizedAdd(1, 2, 3)) // 6
console.log(memoizedAdd(1, 2, 4)) // 7
```

如果使用 JSON.stringify，参数是对象的问题也可以得到解决，因为存储的是对象序列化后的字符串。

#### 适用场景

这里以 “斐波那契数列” 为例，由于这部分内容在 “记忆化搜索” 和 ”动态规划“ 中是经典示例，被大量提及；所以这部分略。

摘自：[JavaScript专题之函数记忆](https://github.com/mqyqingfeng/Blog/issues/46)



### 递归

**定义：**程序调用自身的编程技巧称为递归 ( recursion )。

##### 阶乘

以阶乘为例：

```js
function factorial(n) {
    if (n == 1) return n;
    return n * factorial(n - 1)
}
console.log(factorial(5)) // 5 * 4 * 3 * 2 * 1 = 120
```

示意图（ 图片来自 www.penjee.com ）

![https://camo.githubusercontent.com/e7f3e971eebd1f8c6e0bd15be013506e516443ed7caeb27dc29c983bf5b1a2e9/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f726563757273696f6e2f666163746f7269616c2e676966](https://camo.githubusercontent.com/e7f3e971eebd1f8c6e0bd15be013506e516443ed7caeb27dc29c983bf5b1a2e9/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f726563757273696f6e2f666163746f7269616c2e676966)

##### 斐波那契数列

在[《JavaScript专题之函数记忆》](https://github.com/mqyqingfeng/Blog/issues/46)中讲到过的斐波那契数列也使用了递归：

```js
function fibonacci(n){
    return n < 2 ? n : fibonacci(n - 1) + fibonacci(n - 2);
}
console.log(fibonacci(5)) // 1 1 2 3 5
```

#### 递归条件

从这两个例子中，我们可以看出：构成递归 需具备 <font color=FF0000>边界条件</font>、<font color=FF0000>递归前进段 </font>和 <font color=FF0000>递归返回段</font>。<mark>当边界条件不满足时，递归前进；当边界条件满足时，递归返回</mark>。阶乘中的 `n == 1` 和 斐波那契数列中的 `n < 2` 都是边界条件。

##### 总结一下递归的特点

1. 子问题须与原始问题为同样的事，且更为简单；
2. 不能无限制地调用本身，须有个出口，化简为非递归状况处理。

了解这些特点可以帮助我们更好的编写递归函数。

#### 执行上下文栈

在[《JavaScript深入之执行上下文栈》](https://github.com/mqyqingfeng/Blog/issues/4)中，我们知道：当执行一个函数的时候，就会创建一个执行上下文，并且压入执行上下文栈，当函数执行完毕的时候，就会将函数的执行上下文从栈中弹出。

试着对阶乘函数分析执行的过程，我们会发现，JavaScript 会不停的创建执行上下文压入执行上下文栈，对于内存而言，维护这么多的执行上下文也是一笔不小的开销呐！那么，我们该如何优化呢？<font color=FF0000>答案就是 <font size=4>**尾调用**</font></font>。

#### 尾调用

尾调用，是指函数内部的最后一个动作是函数调用。该调用的返回值，直接返回给函数。

举个例子：

```js
// 尾调用
function f(x){
    return g(x);
}

// 非尾调用
function f(x){
    return g(x) + 1;
}
```

第二个函数不是尾调用，因为 g(x) 的返回值还需要跟 1 进行计算后，f(x) 才会返回值。**注：**这和尾递归一样。

<font color=FF0000>**两者又有什么区别呢**</font>？答案就是 <font color=FF0000 size=4>**执行上下文栈的变化不一样**</font>。

为了模拟执行上下文栈的行为，让我们定义执行上下文栈是一个数组：

```
ECStack = [];
```

我们模拟下 <font color=FF0000>**第一个尾调用函数** 执行时的执行上下文栈变化</font>：

```js
// 伪代码
ECStack.push(<f> functionContext);
ECStack.pop();

ECStack.push(<g> functionContext);
ECStack.pop();
```

我们再来模拟一下 <font color=FF0000>**第二个非尾调用函数** 执行时的执行上下文栈变化</font>：

```js
ECStack.push(<f> functionContext);
ECStack.push(<g> functionContext);

ECStack.pop();
ECStack.pop();
```

也就说 <font color=FF0000>**尾调用函数执行**</font> 时，虽然也调用了一个函数，但是因为<font color=FF0000>**原来的的函数执行完毕，执行上下文会被弹出**</font>，<font color=FF0000>执行上下文栈中相当于只多压入了一个执行上下文</font>。然而 <font color=FF0000>**非尾调用函数**</font>，就<font color=FF0000>会创建多个执行上下文压入执行上下文栈</font>。

函数调用自身，称为递归。<font color=FF0000 size=4>**如果尾调用自身，就称为尾递归**</font>。

所以我们只用把阶乘函数改造成一个尾递归形式，就可以避免创建那么多的执行上下文。但是我们该怎么做呢？

#### 阶乘函数优化

我们需要做的就是把所有用到的内部变量改写成函数的参数，以阶乘函数为例：

```js
function factorial(n, res) {
    if (n == 1) return res;
    return factorial(n - 1, n * res)
}
console.log(factorial(4, 1)) // 24
```

然而这个很奇怪呐……我们计算 4 的阶乘，结果函数要传入 4 和 1，我就不能只传入一个 4 吗？

这个时候就要用到我们在[《JavaScript专题之偏函数》](https://github.com/mqyqingfeng/Blog/issues/43)中编写的 partial 函数了：

```js
var newFactorial = partial(factorial, _, 1)
newFactorial(4) // 24
```

摘自：[JavaScript专题之递归](https://github.com/mqyqingfeng/Blog/issues/49)

#### 阮一峰文章的尾调用

尾调用不一定出现在函数尾部，只要是最后一步操作即可。

```js
function f(x) {
  if (x > 0) { return m(x) }
  return n(x);
}
```

上面代码中，函数m和n都属于尾调用，因为它们都是函数f的最后一步操作。

##### 尾递归

递归非常耗费内存，因为需要同时保存成千上百个调用记录，很容易发生"栈溢出"错误 ( stack overflow )。但对于尾递归来说，由于只存在一个调用记录，所以永远不会发生"栈溢出"错误。

```js
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}
factorial(5) // 120
```

上面代码是一个阶乘函数，计算 n 的阶乘，最多需要保存 n 个调用记录，（**注：**空间？）复杂度 O(n) 。<font color=FF0000>如果改写成尾递归，只保留一个调用记录，复杂度 O(1) </font>。

总结一下，递归本质上是一种循环操作。<font color=FF0000>纯粹的函数式编程语言没有循环操作命令，所有的循环都用递归实现，这就是为什么尾递归对这些语言极其重要</font>。对于其他支持 “尾调用优化” 的语言（比如 Lua，ES6 ），只需要知道循环可以用递归代替，而一旦使用递归，就最好使用尾递归。

##### 严格模式

<font color=FF0000 size=4>**ES6 的 尾调用优化只在 “严格模式” 下开启**</font>，正常模式是无效的。这是因为在正常模式下，函数内部有（如下）两个变量，可以跟踪函数的调用栈。

- **arguments：**返回调用时函数的参数。
- **func.caller：**返回调用当前函数的那个函数。

<font color=FF0000>尾调用优化发生时，函数的调用栈会改写，因此上面两个变量就会失真</font>。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效。

摘自：[尾调用优化](https://www.ruanyifeng.com/blog/2015/04/tail-call.html)

#### 维基百科中的尾调用

在计算机学里，「尾调用」是指一个函数里的最后一个动作是返回一个函数的调用结果的情形，即最后一步新调用的返回值直接被当前函数的返回结果。此时，该尾部调用位置被称为尾位置。<font color=FF0000 size=4>**尾调用中有一种重要而特殊的情形叫做 尾递归 **</font>。经过适当处理，尾递归形式的函数的运行效率可以被极大地优化。<font color=FF0000>尾调用原则上都可以通过简化函数调用栈的结构而获得性能优化（称为“尾调用消除”），但是 <font size=4>优化尾调用是否方便可行 **取决于运行环境对此类优化的支持程度如何**</font></font>。

摘自：[维基百科 - 尾调用](https://zh.wikipedia.org/wiki/%E5%B0%BE%E8%B0%83%E7%94%A8)

#### 《深入理解 ES6》中的尾调用

ECMAScript 6 关于函数最有趣的变化可能是 <font color=FF0000>**尾调用系统的引擎优化**</font>。

<font color=FF0000>在 **ECMAScript 5 的引擎** 中， 尾调用的实现与其他函数调用的实现类似：**创建一个新的栈帧 ( stack frame )**， 将其推入调用栈来表示函数调用</font>。也就是说，在循环调用中，每一个未用完的栈帧都会被保存在内存中， 当调用栈变得过大时会造成程序问题 。

##### ECMAScript 6 中的尾调用优化

<font color=FF0000>ECMAScript 6 缩减了 **严格模式** 下尾调用栈的大小（非严格模式下不受影响）</font>；<font color=FF0000 size=4>**如果满足以下条件，尾调用不再创建新的栈帧， 而是清除井重用当前栈帧**</font>：

- <font color=FF0000>尾调用不访问当前栈帧的变量 （也就是说 <font size=4>**函数不是一个闭包**</font>）</font>
- 在函数内部， 尾调用是最后一条语句（**注：**感觉这句话不严谨）
- 尾调用的结果作为 <font color=FF0000>函数值返回</font>

以下这段示例代码满足上述的三个条件， 可以被 JavaScript 引擎自动优化 ：

```js
"use strict";

function doSomething() { // 优化
  return doSomethingElse();
}
```

##### 不满足尾调用的情况

如果你定义了一个函数，<font color=FF0000>**在尾调用返回后执行其他操作**，则函数也无法得到优化</font>：

```js
"use strict";

function doSomething() { // 未优化 - 在函数执行并返回之前有额外的操作
  return 1 + doSomethingElse();
}
```

如果把函数调用的结果存储在 再返回这个变量，则可能导致引擎无法优化，就像这样 ：

```js
"use strict";

function doSomething() { // 未优化 - 函数调用未发生在尾部
  var result = doSomethingElse();
  return result;
}
```

由于没有立即返回 doSomethingElse() 函数的值，因此此例中的代码无法被优化。

<font color=FF0000>可能最难避免的情况是 **闭包的使用**，它可以访问作用域中所有变量，因而导致尾调用优化失效</font>。举个例子：

```js
"use strict";

function doSomething() {
  var num = 1,
      func = () => num;

  // 未优化 - 存在闭包
  return func();
}
```

摘自：《深入理解 ES6》- 尾递归优化 P67

#### 《 JavaScript 高级程序设计》第四版中的尾递归

ECMAScript 6 规范新增了一项内存管理优化机制，让 JavaScript 引擎在满足条件时可以重用栈帧。 具体来说，这项优化非常适合“尾调用”，即外部函数的返回值是一个内部函数的返回值

##### 尾调用优化的条件

尾调用优化的条件就是确定外部栈帧真的没有必要存在了。涉及的条件如下：

- 代码在<font color=FF0000>严格模式</font>下执行
- 外部函数的<font color=FF0000>返回值是对尾调用函数的调用</font>
- 尾调用函数<font color=FF0000>返回后不需要执行额外的逻辑</font>
- 尾调用函数<font color=FF0000>不是引用外部函数作用域中自由变量的 **闭包**</font>

下面展示了几个违反上述条件的函数，因此都不符号尾调用优化的要求：

```js
"use strict";

// 无优化：尾调用没有返回
function outerFunction() {
  innerFunction();
}

// 无优化：尾调用没有直接返回
function outerFunction() {
  let innerFunctionResult = innerFunction();
  return innerFunctionResult; 
}

// 无优化：尾调用返回后必须转型为字符串
function outerFunction() {
	return innerFunction().toString();
}

// 无优化：尾调用是一个闭包
function outerFunction() {
	let foo = 'bar';
	function innerFunction() { return foo; }
	return innerFunction();
}
```

下面是几个符合尾调用优化条件的例子：

```js
"use strict";

// 有优化：栈帧销毁前执行参数计算
function outerFunction(a, b) {
	return innerFunction(a + b);
}

// 有优化：初始返回值不涉及栈帧
function outerFunction(a, b) {
  if (a < b) { return a; }
  return innerFunction(a + b);
}

// 有优化：两个内部函数都在尾部
function outerFunction(condition) {
  return condition ? innerFunctionA() : innerFunctionB();
}
```

摘自：《 JavaScript 高级程序设计》第四版 §10.13 P307

#### 一些补充

ES6 尾调用优化 是 ecma 的规范，但是根据 Hax贺师俊 和 死月 的说法：浏览器厂商（包括 Node ），只有 Safari 支持了，其他厂商都没有支持。详见：[尾调用函数是闭包的时候，为什么无法实现优化？ - 知乎](https://www.zhihu.com/question/471431054) &  [DC 的新书《JavaScript 悟道》里面讲了很多尾递归优化，可 TC39 不是已经判其死刑了吗？ - 知乎](https://www.zhihu.com/question/473997712)

尾调用 和 JS 调用栈 可以参见：[JS 调用栈机制与 ES6 尾调用优化介绍](https://juejin.cn/post/6844903847693910029) 以及其中 justjavac 大佬的评论

关于 “尾递归” 更多可以参见：[浅谈尾递归](https://site.douban.com/196781/widget/notes/12161495/note/262014367/) 以及《数据结构与算法分析：C描述》§3.3.3 P61。



### 防抖 和 节流

在前端开发中会遇到一些频繁的事件触发，比如：

1. window 的 resize、scroll
2. mousedown、mousemove
3. keyup、keydown
4. ...

为此，我们举个示例代码来了解事件如何频繁的触发：

```html
<!DOCTYPE html>
<html lang="zh-cmn-Hans">

<head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="IE=edge, chrome=1">
    <title>debounce</title>
    <style>
        #container{
            width: 100%; height: 200px; line-height: 200px; text-align: center; color: #fff; background-color: #444; font-size: 30px;
        }
    </style>
</head>

<body>
    <div id="container"></div>
</body>

<script>
var count = 1;
var container = document.getElementById('container');

function getUserAction() {
    container.innerHTML = count++;
};

container.onmousemove = getUserAction;  
</script>
  
</html>
```

我们来看看效果：

![https://camo.githubusercontent.com/a63c64f8b1b09962064f3d112edfc00ccdc039f625459e9400b3e746f71a0d3d/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f6465626f756e63652f6465626f756e63652e676966](https://s2.loli.net/2022/04/13/tjSTXeVIZsLND5R.gif)

从左边滑到右边就触发了 165 次 getUserAction 函数！

因为这个例子很简单，所以浏览器完全反应的过来，可是如果是复杂的回调函数或是 ajax 请求呢？假设 1 秒触发了 60 次，每个回调就必须在 1000 / 60 = 16.67ms 内完成，否则就会有卡顿出现。

**为了解决这个问题，一般有两种解决方案**

1. debounce 防抖
2. throttle 节流

#### 防抖

防抖的原理就是：你<font color=FF0000>尽管触发事件</font>，但是我 <font color=FF0000>**一定 <font size=4>在事件触发 n 秒后</font> 才执行**</font>，如果你<font color=FF0000>在一个事件触发的 n 秒内又触发了这个事件</font>，那我就<font color=FF0000 size=4>**以新的事件的时间为准，n 秒后才执行**</font>，总之，<font color=FF0000 size=4>**就是要等你触发完事件 n 秒内不再触发事件，我才执行**</font>，真是任性呐!

##### 第一版

根据这段表述，我们可以写第一版的代码：

```js
// 第一版
function debounce(func, wait) {
    var timeout;
    // 注：返回的函数（在 wait 时间范围内）每调用一次，就会被清空掉；如果在 wait 范围之外，则按照 setTimeout 执行
    return function () {
        clearTimeout(timeout)
        timeout = setTimeout(func, wait);
    }
}
```

如果我们要使用它，以最一开始的例子为例：

```js
container.onmousemove = debounce(getUserAction, 1000);
```

现在随你怎么移动，反正你移动完 1000ms 内不再触发，我才执行事件。看看使用效果：

![https://camo.githubusercontent.com/93ec162f14331b5a007f6d01a690226106767acccd6d7b3a98b7b7059ddcbdf0/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f6465626f756e63652f6465626f756e63652d312e676966](https://s2.loli.net/2022/04/13/3RPMkEsclNIbFgT.gif)

顿时就从 165 次降低成了 1 次！

##### this

如果我们在 `getUserAction` 函数中 `console.log(this)`，在不使用 `debounce` 函数的时候，`this` 的值为：`<div id="container"></div>`。但是如果使用我们的 debounce 函数，this 就会指向 Window 对象（**注：**这是 setTimeout 的机制）。所以我们需要将 this 指向正确的对象。修改下代码：

##### 第二版

```js
// 第二版
function debounce(func, wait) {
    var timeout;

    return function () {
        var context = this; // 注：如果下面 setTimeout 用“箭头函数”，就不需要用 context 暂存 this

        clearTimeout(timeout)
        timeout = setTimeout(function(){
            func.apply(context)
        }, wait);
    }
}
```

现在 this 已经可以正确指向了。让我们看下个问题：

##### Event 对象

JavaScript 在事件处理函数中会提供事件对象 Event，我们修改下 getUserAction 函数：

```js
function getUserAction(e) {
    console.log(e);
    container.innerHTML = count++;
};
```

如果我们不使用 debouce 函数，这里会打印 MouseEvent 对象，如图所示：

![https://camo.githubusercontent.com/7cc0af80b9b8ac3805eec37a66f381b8054759b59899c3cdd1a16b6406115a0d/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f6465626f756e63652f6576656e742e706e67](https://s2.loli.net/2022/04/13/ogD9lpitqz5WyfC.png)

但是在我们实现的 debounce 函数中，却只会打印 undefined！所以我们再修改一下代码。

##### 第三版

```js
// 第三版
function debounce(func, wait) {
    var timeout;

    return function () {
        var context = this;
        // 注：保存 arguments，防止丢失。另外，下面用箭头函数，不会出现 arguments 丢失的情况；因为 箭头函数没有 arguments。
        var args = arguments;

        clearTimeout(timeout)
        timeout = setTimeout(function(){
            func.apply(context, args)
        }, wait);
    }
}
```

到此为止，我们修复了两个小问题：

1. this 指向
2. event 对象

##### 立刻执行

这个时候，代码已经很是完善了，但是为了让这个函数更加完善，我们接下来思考一个新的需求。

**这个需求就是：**我<font color=FF0000>不希望非要等到事件停止触发后才执行，我希望立刻执行函数，然后等到停止触发 n 秒后，才可以重新触发执行</font>。

想想这个需求也是很有道理的嘛，那我们加个 immediate 参数判断是否是立刻执行。

##### 第四版

```js
// 第四版
function debounce(func, wait, immediate) {

    var timeout;

    return function () {
        var context = this;
        var args = arguments;

        if (timeout) clearTimeout(timeout);
        if (immediate) {
            // 如果已经执行过，不再执行
            var callNow = !timeout; // timeout 不为 null，则 callnow（立即执行）；感觉 callnow 是一个语义化变量，可去掉
            timeout = setTimeout(function(){
                timeout = null; // 时间到了，timeout 设置为 null；可以继续执行（callnow 为 true），执行 func.apply()
            }, wait)
            if (callNow) func.apply(context, args)
        }
        else { // 注：else 的情况，和之前一样
            timeout = setTimeout(function(){
                func.apply(context, args)
            }, wait);
        }
    }
}
```

##### 返回值

此时注意一点，就是 getUserAction <font color=FF0000>**函数可能是有返回值的**</font>，所以我们也要返回函数的执行结果，但是当 immediate 为 false 的时候，因为使用了 setTimeout ，我们将 func.apply(context, args) 的返回值赋给变量，最后再 return 的时候，值将会一直是 undefined，所以我们只在 immediate 为 true 的时候返回函数的执行结果。

```js
// 第五版
function debounce(func, wait, immediate) {
    var timeout, result;

    return function () {
        var context = this;
        var args = arguments;

        if (timeout) clearTimeout(timeout);
        if (immediate) {
            // 如果已经执行过，不再执行
            var callNow = !timeout;
            timeout = setTimeout(function(){
                timeout = null;
            }, wait)
            if (callNow) result = func.apply(context, args) // 注：result
        }
        else {
            timeout = setTimeout(function(){
                func.apply(context, args)
            }, wait);
        }
        return result;
    }
}
```

##### 取消

最后我们再思考一个小需求，我希望能取消 debounce 函数，比如说我 debounce 的时间间隔是 10 秒钟，immediate 为 true，这样的话，我只有等 10 秒后才能重新触发事件，现在<font color=FF0000>我希望有一个按钮，点击后，取消防抖，这样我再去触发，就可以又立刻执行</font>啦，是不是很开心？

为了这个需求，我们写最后一版的代码：

```js
// 第六版
function debounce(func, wait, immediate) {

    var timeout, result;

    var debounced = function () {
        var context = this;
        var args = arguments;

        if (timeout) clearTimeout(timeout);
        if (immediate) {
            // 如果已经执行过，不再执行
            var callNow = !timeout;
            timeout = setTimeout(function(){
                timeout = null;
            }, wait)
            if (callNow) result = func.apply(context, args)
        }
        else {
            timeout = setTimeout(function(){
                func.apply(context, args)
            }, wait);
        }
        return result;
    };

    // 注：取消按钮相关
    debounced.cancel = function() {
        clearTimeout(timeout);
        timeout = null;
    };

    return debounced;
}
```

那么该如何使用这个 cancel 函数呢？依然是以上面的 demo 为例：

```js
var count = 1;
var container = document.getElementById('container');

function getUserAction(e) {
    container.innerHTML = count++;
};

var setUseAction = debounce(getUserAction, 10000, true);

container.onmousemove = setUseAction;

document.getElementById("button").addEventListener('click', function(){
    setUseAction.cancel();
})
```

演示效果如下：

![https://camo.githubusercontent.com/eec61639e79bfb303fe25a982bdca8a8033ad696f0f4963fed46fe3fdf4a7cce/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f6465626f756e63652f6465626f756e63652d63616e63656c2e676966](https://s2.loli.net/2022/04/15/FwxZN9Cdpgkr3EV.gif)

至此我们已经完整实现了一个 underscore 中的 debounce 函数，恭喜，撒花！

##### 演示代码

相关的代码可以在 [Github 博客仓库](https://github.com/mqyqingfeng/Blog/tree/master/demos/debounce) 中找到

#### 节流

节流的原理很简单：如果你<font color=FF0000>持续触发事件，每隔一段时间，只执行一次事件</font>。

<font color=FF0000>根据首次是否执行以及结束后是否执行，效果有所不同，实现的方式也有所不同</font>。我们用 leading 代表首次是否执行，trailing 代表结束后是否再执行一次。

关于 **节流的实现，有两种主流的实现方式**，<font color=FF0000 size=4>**一种是使用时间戳，一种是设置定时器**</font>。

##### 使用时间戳

让我们来看 **第一种方法：使用时间戳**，<font color=FF0000>当触发事件的时候，我们取出当前的时间戳</font>，<font color=FF0000>然后减去之前的时间戳（最一开始值设为 0 ）</font>，<mark style=background:aqua>如果大于设置的时间周期，就执行函数，然后更新时间戳为当前的时间戳</mark>，<mark>如果小于，就不执行</mark>。

看了这个表述，是不是感觉已经可以写出代码了…… 让我们来写第一版的代码：

```js
// 第一版
function throttle(func, wait) {
    var context, args;
    var previous = 0;

    return function() {
        var now = +new Date(); // 注：加号操作符将 new Date() 转化为 number 类型
        context = this;
        args = arguments;
        if (now - previous > wait) {
            func.apply(context, args);
            previous = now; // 注：以本次的时间戳，作为下一次的 previous
        }
    }
}
```

例子依然是用讲 debounce 中的例子，如果你要使用：

```js
container.onmousemove = throttle(getUserAction, 1000);
```

效果演示如下：

![https://camo.githubusercontent.com/fdee590c44e81ba6ce07627d96500456546fd8a0516867f55cd51da30e11e014/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f7468726f74746c652f7468726f74746c65312e676966](https://s2.loli.net/2022/04/15/lqgy4W9rNsKviOJ.gif)

##### 使用定时器

接下来，我们讲讲第二种实现方式，使用定时器。<font color=FF0000>当触发事件的时候，我们设置一个定时器</font>，<mark style=background:aqua>再触发事件的时候，如果定时器存在，就不执行</mark>，<mark>直到定时器执行，然后执行函数，清空定时器，这样就可以设置下个定时器</mark>。

```js
// 第二版
function throttle(func, wait) {
    var timeout;
    var previous = 0;

    return function() {
        context = this;
        args = arguments; // 注：保存 arguments，使用“闭包”防止其在 setTimeout 中的函数内丢失。
        if (!timeout) { // 注：在定时器任务生效执行前，不允许再创建定时器任务
            timeout = setTimeout(function(){
                timeout = null; // 注：执行后，将timeout 赋值为 null，让下一次 if 内的判断为“真”
                func.apply(context, args)
            }, wait)
        }
    }
}
```

为了让效果更加明显，我们设置 wait 的时间为 3s，效果演示如下：

![https://camo.githubusercontent.com/07970f9ed563d93d960931d6249d6f44565740c641f570e499e18dcd4aefedf2/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f7468726f74746c652f7468726f74746c65322e676966](https://s2.loli.net/2022/04/15/wg73bicBMCAVnxu.gif)

我们可以看到：当鼠标移入的时候，事件不会立刻执行，晃了 3s 后终于执行了一次，此后每 3s 执行一次，当数字显示为 3 的时候，立刻移出鼠标，<font color=FF0000>相当于大约 9.2s 的时候停止触发，**但是依然会在第 12s 的时候执行一次事件**</font>。

##### 所以比较两个方法（时间戳 和 定时器 两种方法）：

1. <mark>第一种 事件会立刻执行</mark>，<mark style="background: aqua">第二种 事件会在 n 秒后第一次执行</mark>
2. <mark>第一种 事件**停止触发后没有办法再执行事件**</mark>，<mark style="background: aqua">第二种 **事件停止触发后依然会再执行一次事件**</mark>

##### 双剑合璧

那我们想要一个什么样的呢？

有人就说了：我想要一个 <mark>有头有尾的</mark>！就是 <mark>鼠标移入能立刻执行，停止触发的时候还能再执行一次</mark>。

所以我们综合两者的优势，然后双剑合璧，写一版代码：

```js
// 第三版
function throttle(func, wait) {
    var timeout, context, args, result;
    var previous = 0;

    var later = function() {
        previous = +new Date();
        timeout = null;
        func.apply(context, args)
    };

    var throttled = function() {
        var now = +new Date(); // 注：获取当前时间，以便判断时间是否超过 wait，可以再次执行
        // 下次触发 func 剩余的时间
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
         // 如果没有剩余的时间了，或者你改了系统时间
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout); // 注：clearTimeout 只会修改 timeout 对象中的属性，timeout 对象依然存在。
                timeout = null;
            }
            previous = now;
            func.apply(context, args);
        } else if (!timeout) {
            // 注：如果时间还有剩余( remaining > 0 )，且 timeout 已经不为“真值”了（比如为 null 了），则重新 setTimeout。另外，这里执行的是上面定义的 later 函数
            timeout = setTimeout(later, remaining);
        }
    };
    return throttled;
}
```

效果演示如下：

![https://camo.githubusercontent.com/778b1f8836deef32ae3f91f2595fa1f198247d18d6a37ecbd69d2d4c92887079/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f7468726f74746c652f7468726f74746c65332e676966](https://camo.githubusercontent.com/778b1f8836deef32ae3f91f2595fa1f198247d18d6a37ecbd69d2d4c92887079/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f7468726f74746c652f7468726f74746c65332e676966)

我们可以看到：鼠标移入，事件立刻执行，晃了 3s，事件再一次执行，当数字变成 3 的时候，也就是 6s 后，我们立刻移出鼠标，停止触发事件，9s 的时候，依然会再执行一次事件。

##### 优化

但是我有时也希望无头有尾，或者有头无尾，这个咋办？

那我们<font color=FF0000>设置个 options 作为第三个参数，然后根据传的值判断到底哪种效果</font>；我们<font color=FF0000>约定：leading: false 表示禁用第一次执行；trailing: false 表示禁用停止触发的回调</font>

```js
// 第四版
function throttle(func, wait, options) {
    var timeout, context, args, result;
    var previous = 0;
    if (!options) options = {};

    var later = function() {
        previous = options.leading === false ? 0 : new Date().getTime();
        timeout = null;
        func.apply(context, args);
        if (!timeout) context = args = null;
    };

    var throttled = function() {
        var now = new Date().getTime(); // 注：获得当前时间时间戳
        if (!previous && options.leading === false) previous = now;
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            func.apply(context, args);
            if (!timeout) context = args = null;
        } else if (!timeout && options.trailing !== false) {
            timeout = setTimeout(later, remaining);
        }
    };
    return throttled;
}
```

##### 取消

在 debounce 的实现中，我们加了一个 cancel 方法，throttle 我们也加个 cancel 方法：

```js
throttled.cancel = function() {
    clearTimeout(timeout); // 复原数据
    previous = 0;
    timeout = null;
}
```

##### 注意

我们要注意 underscore 的实现中有这样一个问题：那就是 <font color=FF0000>`leading：false` 和 `trailing: false` 不能同时设置</font>。

如果同时设置的话，比如当你将鼠标移出的时候，因为 trailing 设置为 false，停止触发的时候不会设置定时器，所以只要再过了设置的时间，再移入的话，就会立刻执行，就违反了 leading: false，bug 就出来了。所以，<font color=FF0000>这个 throttle 只有三种用法</font>：

```js
container.onmousemove = throttle(getUserAction, 1000);
container.onmousemove = throttle(getUserAction, 1000, { leading: false });
container.onmousemove = throttle(getUserAction, 1000, { trailing: false });
```

至此我们已经完整实现了一个 underscore 中的 throttle 函数，恭喜，撒花！

摘自：[JavaScript专题之跟着underscore学防抖](https://github.com/mqyqingfeng/Blog/issues/22)



### 如何实现自己的 underscore ( utility library )

#### 前言

在 [《JavaScript 专题系列》](https://github.com/mqyqingfeng/Blog/issues/53) 中，我们<font color=FF0000>写了很多的功能函数</font>，比如防抖、节流、去重、类型判断、扁平数组、深浅拷贝、查找数组元素、通用遍历、柯里化、函数组合、函数记忆、乱序等，可以我们该<font color=FF0000>如何组织这些函数，形成自己的一个工具函数库</font>呢？这个时候，我们就<font color=FF0000>要借鉴 underscore 是怎么做的了</font>。

#### 自己实现

如果是我们自己去组织这些函数，我们该怎么做呢？我想我会这样做：

```js
(function(){
    var root = this;
    var _ = {};
    root._ = _;

    // 在这里添加自己的方法
    _.reverse = function(string){ return string.split('').reverse().join(''); }
})()

_.reverse('hello'); // 'olleh'
```

我们 <font color=FF0000>将所有的方法添加到一个名为 `_` 的对象上</font>，<font color=FF0000>然后 **将该对象挂载到全局对象上**</font>。

之所以<font color=FF0000>不直接 `window._ = _`</font> 是<font color=FF0000>因为我们写的是一个工具函数库，不仅要求可以运行在浏览器端，还可以运行在诸如 Node 等环境中</font>。

#### Root

然而 underscore 可不会写得如此简单，我们从 `var root = this` 开始说起。

之所以写这一句，是因为我们要<font color=FF0000>通过 this 获得全局对象</font>，然后 <font color=FF0000>将 `_` 对象 挂载上去</font>。

然而<font color=FF0000>在严格模式下，this 返回 undefined，而不是指向 Window</font>，幸运的是 underscore 并没有采用严格模式，可是即便如此，也不能避免：因为在 <font color=FF0000 size=4>**ES6 中模块脚本自动采用严格模式，不管有没有声明 `use strict`**</font>。

如果 this 返回 undefined，代码就会报错，所以我们的思路是<font color=FF0000>对环境进行检测，然后挂载到正确的对象上</font>。我们修改一下代码：

```js
// window 对应 浏览器环境；另外：浏览器中没有定义 global。global 对应的是 Node 环境。
var root = (typeof window == 'object' && window.window == window && window) ||
           (typeof global == 'object' && global.global == global && global);
```

在这段代码中，我们判断了浏览器和 Node 环境，<font color=FF0000 size=4>**可是只有这两个环境吗？那我们来看看 Web Worker**</font>。

#### Web Worker

Web Worker 属于 HTML5 中的内容，引用《JavaScript权威指南》中的话就是：

> 在 Web Worker 标准中，定义了<mark>解决客户端 JavaScript 无法多线程的问题</mark>。<font color=FF0000>其中定义的 “worker” 是指执行代码的并行过程</font>。不过，<font color=FF0000>**Web Worker 处在一个自包含的执行环境中，无法访问 Window 对象和 Document 对象**，和主线程之间的通信业只能通过异步消息传递机制来实现</font>。

为了演示 Web Worker 的效果，我写了一个 demo，[查看代码](https://github.com/mqyqingfeng/Blog/tree/master/demos/web-worker)。

在 Web Worker 中，是无法访问 Window 对象的，所以 `typeof window` 和 `typeof global` 的结果都是 `undefined`，所以最终 root 的值为 false，将一个基本类型的值像对象一样添加属性和方法，自然是会报错的。那么我们该怎么办呢？

<font color=FF0000>虽然在 Web Worker 中不能访问到 Window 对象，但是我们却能**通过 self 访问到 Worker 环境中的全局对象**</font>。我们只是要找全局变量挂载而已，所以完全可以挂到 self 中嘛。

而且在浏览器中，除了 window 属性，我们也可以通过 （**注：**window对象下的）self 属性直接访问到 Winow 对象。

```js
console.log(window.window === window); // true
console.log(window.self === window); // true
```

考虑到使用 self 还可以额外支持 Web Worker，我们直接<mark>将代码改成 self</mark>：

```js
var root = (typeof self == 'object' && self.self == self && self) ||
           (typeof global == 'object' && global.global == global && global);
```

#### Node VM ( virtual machine )

到了这里，依然没完。让你想不到的是，<font color=FF0000>**在 node 的 vm 模块中**</font>（也就是沙盒模块）<font color=FF0000>**runInContext 方法中，不存在 window，也不存在 global 变量**</font>，[查看代码](https://github.com/mqyqingfeng/Blog/blob/master/demos/node-vm/index.js)。

但是我们却<font color=FF0000>可以通过 this 访问到全局对象</font>，所以就有人发起了一个 PR，代码改成了：

```js
var root = (typeof self == 'object' && self.self == self && self) ||
           (typeof global == 'object' && global.global == global && global) ||
           this;
```

#### 微信小程序

到了这里，还是没完，轮到微信小程序登场了。因为<font color=FF0000>在微信小程序中，window 和 global 都是 undefined</font>，加上又强制使用严格模式，this 为 undefined，挂载就会发生错误，所以就有人又发了一个 PR，代码变成了：

```js
var root = (typeof self == 'object' && self.self == self && self) ||
           (typeof global == 'object' && global.global == global && global) ||
           this ||
           {};
```

这就是现在 v1.8.3 的样子。

虽然作者可以直接讲解最终的代码，但是作者更希望带着大家看看这看似普通的代码是如何一步步演变成这样的，也希望告诉大家，代码的健壮性，并非一蹴而就，而是汇集了很多人的经验，考虑到了很多我们意想不到的地方，这也是开源项目的好处吧。

#### 函数对象

现在我们讲第二句 `var _ = {};`。

如果仅仅设置 _ 为一个空对象，我们调用方法的时候，只能使用 `_.reverse('hello')` 的方式，实际上，underscore 也支持类似面向对象的方式调用，即：

```js
_('hello').reverse(); // 'olleh'
```

再举个例子比较下两种调用方式：

```js
// 函数式风格
_.each([1, 2, 3], function(item){ console.log(item) });

// 面向对象风格
_([1, 2, 3]).each(function(item){ console.log(item) });
```

可是该如何实现呢？既然<font color=FF0000>以 `_([1, 2, 3])` 的形式可以执行，就表明 `_` 不是一个字面量对象，而是一个函数</font>

幸运的是，在 JavaScript 中，函数也是一种对象，我们举个例子：

```js
var _ = function() {};
_.value = 1; // 注：上面有讲过，函数看可以挂载元素
_.log = function() { return this.value + 1 }; // 注：这个是挂载函数，这个上面没讲过？

console.log(_.value); // 1
console.log(_.log()); // 2
```

我们完全可以将自定义的函数定义在 `_` 函数上。

目前的写法为：

```js
var root = (typeof self == 'object' && self.self == self && self) ||
           (typeof global == 'object' && global.global == global && global) ||
           this ||
           {};

var _ = function() {}
root._ = _;
```

我们看看 underscore 是如何实现的：

```js
// 注：以 `_(args)` 形式调用，才会执行该函数
var _ = function(obj) {
    if (!(this instanceof _)) return new _(obj);
    // 注：如下面所说：new 之后，this 指向“实例对象”。这时给实例对象添加一个 wrapperd 属性，把 obj 挂载上去
    this._wrapped = obj;
};

_([1, 2, 3]);
```

我们分析下 `_([1, 2, 3])` 的执行过程：

1. <mark>（**注：**如果是第一次判断 this instancof  _  ）</mark>执行 `this instanceof _` ，<mark>（**注：**这时）</mark>this 指向 window ，`window instanceof _` 为 false，`!`操作符取反，所以执行 `new _(obj)` 。<mark>（**注：**执行 new _(obj) 会再次执行 \_函数，即第二次判断 this instanceof \_ ）</mark>
2. `new _(obj)` 中（**注：**第二次执行 「 \_函数」。另外，这里 “中” 改成 “后” 更好理解？），<font color=FF0000>**this 指向实例对象**</font>，`this instanceof _` 为 true，取反后，代码接着执行
3. 执行 `this._wrapped = obj`， 函数执行结束（**注：**给实例对象挂载 \_wrapped 属性）
4. 总结，`_([1, 2, 3])` 返回一个对象，为 `{_wrapped: [1, 2, 3]}`，该对象的原型指向 \_.prototype

<img src="https://s2.loli.net/2022/04/14/kx4c8emJYZo6B5r.png" alt="https://camo.githubusercontent.com/26e79b9d966d98a40deaeb97af5c518094e8849e6403d03429ad3258ddb08122/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f756e64657273636f72652f6e65772d6f626a2e706e67" style="zoom: 50%;" />

然后问题来了，我们是将方法挂载到 _ 函数对象上，并没有挂到函数的原型上呐，所以返回了的实例，其实是无法调用 \_ 函数对象上的方法的！我们写个例子：

```js
(function(){
    var root = (typeof self == 'object' && self.self == self && self) ||
               (typeof global == 'object' && global.global == global && global) ||
               this ||
               {};

    var _ = function(obj) {
        if (!(this instanceof _)) return new _(obj);
        this._wrapped = obj;
    }

    root._ = _;

    _.log = function(){
        console.log(1)
    }
})()

_().log(); // TypeError: _(...).log is not a function
```

确实有这个问题，所以我们还需要一个方法将 \_ 上的方法复制到 `_.prototype` 上，这个方法就是 `_.mixin`。

#### _.functions

为了将 _ 上的方法复制到原型上，首先我们要获得 _ 上的方法，所以我们先写个 `_.functions` 方法。

```js
_.functions = function(obj) {
    var names = [];
    for (var key in obj) {
        if (_.isFunction(obj[key])) names.push(key); // key 是函数名，不是函数本身
    }
    return names.sort();
};
```

isFunction 函数可以参考 [《JavaScript专题之类型判断(下)》](https://github.com/mqyqingfeng/Blog/issues/28)

#### mixin

现在我们可以写 mixin 方法了。

```js
_.mixin = function(obj) {
    _.each(_.functions(obj), function(name) {
        var func = _[name] = obj[name];
        _.prototype[name] = function() {
            var args = [this._wrapped];
            Array.prototype.push.apply(args, arguments);
            return func.apply(_, args);
        };
    });
    return _;
};

_.mixin(_);
```

each 方法可以参考 [《JavaScript专题jQuery通用遍历方法each的实现》](https://github.com/mqyqingfeng/Blog/issues/40)

值得注意的是：因为 `_[name] = obj[name]` 的缘故，我们可以给 underscore 拓展自定义的方法：

```js
_.mixin({
  addOne: function(num) {
    return num + 1;
  }
});

_(2).addOne(); // 3
```

#### 导出

终于到了讲最后一步 `root._ = _`，我们直接看源码：

```js
// 注：exports 是 moudule.exports 的引用
if (typeof exports != 'undefined' && !exports.nodeType) {
    if (typeof module != 'undefined' && !module.nodeType && module.exports) {
        exports = module.exports = _;
    }
    exports._ = _;
} else {
    root._ = _;
}
```

为了支持模块化，我们需要将 _ 在合适的环境中作为模块导出，但是 nodejs 模块的 API 曾经发生过改变，比如在早期版本中：

```js
// add.js
exports.addOne = function(num) {
  return num + 1
}

// index.js
var add = require('./add');
add.addOne(2);
```

在新版本中：

```js
// add.js
module.exports = function(1){
    return num + 1
}

// index.js
var addOne = require('./add.js')
addOne(2)
```

所以我们根据 exports 和 module 是否存在来选择不同的导出方式，那为什么在新版本中，我们还要使用 `exports = module.exports = _` 呢？

这是因为<font color=FF0000>在 nodejs 中，exports 是 module.exports 的一个引用</font>，当你使用了 module.exports = function(){}，实际上覆盖了 module.exports，但是 exports 并未发生改变，为了避免后面再修改 exports 而导致不能正确输出，就写成这样，将两者保持统一。

写个 demo 吧：

```js
// exports 是 module.exports 的一个引用
module.exports.num = '1'

console.log(exports.num) // 1

exports.num = '2'

console.log(module.exports.num) // 2
// addOne.js
module.exports = function(num){
    return num + 1
}

exports.num = '3'

// result.js 中引入 addOne.js
var addOne = require('./addOne.js');

console.log(addOne(1)) // 2
console.log(addOne.num) // undefined
// addOne.js
exports = module.exports = function(num){
    return num + 1
}

exports.num = '3'

// result.js 中引入 addOne.js
var addOne = require('./addOne.js');

console.log(addOne(1)) // 2
console.log(addOne.num) // 3
```

最后<font color=FF0000>为什么要进行一个 exports.nodeType 判断</font>呢？这是因为<font color=FF0000>如果你在 HTML 页面中加入一个 id 为 exports 的元素</font>，比如

```html
<div id="exports"></div>
```

就<font color=FF0000>**会生成一个 window.exports 全局变量**</font>，你可以直接在浏览器命令行中打印该变量。

#### 源码

最终的代码如下，有了这个基本结构，你可以自由添加你需要使用到的函数了：

```js
(function() {
    var root = (typeof self == 'object' && self.self == self && self) ||
        (typeof global == 'object' && global.global == global && global) ||
        this || {};

    var _ = function(obj) {
        if (obj instanceof _) return obj;
        if (!(this instanceof _)) return new _(obj);
        this._wrapped = obj;
    };

    if (typeof exports != 'undefined' && !exports.nodeType) {
        if (typeof module != 'undefined' && !module.nodeType && module.exports) {
            exports = module.exports = _;
        }
        exports._ = _;
    } else {
        root._ = _;
    }

    _.VERSION = '0.1';

    var MAX_ARRAY_INDEX = Math.pow(2, 53) - 1;

    var isArrayLike = function(collection) {
        var length = collection.length;
        return typeof length == 'number' && length >= 0 && length <= MAX_ARRAY_INDEX;
    };

    _.each = function(obj, callback) {
        var length, i = 0;

        if (isArrayLike(obj)) {
            length = obj.length;
            for (; i < length; i++) {
                if (callback.call(obj[i], obj[i], i) === false) {
                    break;
                }
            }
        } else {
            for (i in obj) {
                if (callback.call(obj[i], obj[i], i) === false) {
                    break;
                }
            }
        }

        return obj;
    }

    _.isFunction = function(obj) {
        return typeof obj == 'function' || false;
    };

    _.functions = function(obj) {
        var names = [];
        for (var key in obj) {
            if (_.isFunction(obj[key])) names.push(key);
        }
        return names.sort();
    };

    /**
     * 在 _.mixin(_) 前添加自己定义的方法
     */
    _.reverse = function(string){
        return string.split('').reverse().join('');
    }

    _.mixin = function(obj) {
        _.each(_.functions(obj), function(name) {
            var func = _[name] = obj[name];
            _.prototype[name] = function() {
                var args = [this._wrapped];
                Array.prototype.push.apply(args, arguments);
                return func.apply(_, args);
            };
        });
        return _;
    };

    _.mixin(_);

})()
```

摘自：[underscore 系列之如何写自己的 underscore](https://github.com/mqyqingfeng/Blog/issues/56)
