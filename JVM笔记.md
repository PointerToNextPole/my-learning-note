# JVM 笔记



## JVM GC

#### 分代收集

堆内存分为新生代和老生代。<font color=dodgerBlue>新生代又分为了 <font size=4>**三部分**</font></font>：<font color=fuchsia><font size=4>**1个 Eden 区**</font> 和 2个 Survivor 区（分别叫 from 和 to ），<font size=4>**默认比例为 8: 1: 1**</font></font>

###### 为什么要设置 Survivor 区？

 Survivor 的存在意义，就是减少被送到老年代的对象，进而减少 Full GC 的发生；<font color=red>Survivor 的预筛选保证，只有经历 16 次 Minor GC 还能在新生代中存活的对象，才会被送到老年代</font>。

###### 为什么要设置两个 Survivor 区？

- **Minor GC** 会清理年轻代的内存
- **Major GC** 是清理老年代
- **Full GC** 是清理整个堆空间—包括年轻代和老年代

<font color=dodgerBlue>为什么一个 Survivor 区不行？假设现在只有一个 survivor 区，我们来模拟一下流程：</font> 

刚刚新建的对象在 Eden 中，<font color=LightSeaGreen>一旦 Eden 满了，触发一次 Minor GC ，Eden 中的存活对象就会被移动到 Survivor 区</font>。这样继续循环下去，下一次 Eden 满了的时候，问题来了：<font color=LightSeaGreen>此时进行 Minor GC，Eden 和 Survivor 各有一些存活对象</font>，如果此时把 Eden 区的存活对象硬放到 Survivor 区，很明显这两部分对象所占有的内存是不连续的，也就导致了内存碎片化。 

<img src="https://s2.loli.net/2022/10/25/tMOE2pWQxiAm3KX.png" alt="一个Survivor区带来碎片化" style="zoom:80%;" />

那么，顺理成章的，应该建立两块 Survivor 区，刚刚新建的对象在 Eden 中，经历一次 Minor GC，Eden 中的存活对象就会被移动到第一块 survivor space S0，Eden 被清空；等 Eden 区再满了，就再触发一次 Minor GC，Eden 和 S0 中的存活对象又会被复制送入第二块survivor space S1（这个过程非常重要，因为这种复制算法保证了 S1 中来自 S0 和 Eden 两部分的存活对象占用连续的内存空间，避免了碎片化的发生）。S0 和 Eden 被清空，然后下一轮 S0 与 S1 交换角色，如此循环往复。<font color=LightSeaGreen>如果对象的复制次数达到 16 次，该对象就会被送到老年代中</font>。

<img src="https://s2.loli.net/2022/10/25/3niDhNFILzJb8qT.png" alt="两块Survivor避免碎片化" style="zoom:80%;" />

述机制最大的好处就是：整个过程中，永远有一个 survivor space 是空的，另一个非空的 survivor space 无碎片。

那么，Survivor 为什么不分更多块呢？比方说分成三个、四个、五个？显然，如果 Survivor 区再细分下去，每一块的空间就会比较小，很容易导致 Survivor 区满，因此，我认为两块 Survivor 区是经过权衡之后的最佳方案。

###### 怎么GC

一般情况下，<font color=fuchsia>新创建的对象都会被分配到 Eden 区</font>（一些大对象特殊处理），这些对象经过第一次 Minor GC 后，如果仍然存活，将会被移到 Survivor 区。

<font color=red>对象在 Survivor 区中每熬过一次 Minor GC，年龄就会增加 1 岁</font>，当它的年龄增加到一定程度时，就会被移动到年老代中（ 👀 即：对象晋升）。

因为年轻代中的对象基本都是朝生夕死的（ 80% 以上），所以在年轻代的垃圾回收算法使用的是复制算法，复制算法的基本思想就是将内存分为两块，每次只用其中一块，当这一块内存用完，就将还活着的对象复制到另外一块上面。复制算法不会产生内存碎片。

在 GC 开始的时候，对象只会存在于 Eden 区和名为 From 的 Survivor 区，Survivor 区 To 是空的。紧接着进行 GC ，Eden 区中所有存活的对象都会被复制到 To ，而在 From 区中，仍存活的对象会根据他们的年龄值来决定去向。年龄达到一定值（年龄阈值，可以通过 `-XX:MaxTenuringThreshold` 来设置）的对象会被移动到年老代中，没有达到阈值的对象会被复制到 To 区域。经过这次 GC 后，Eden 区和 From 区已经被清空。这个时候，From 和 To 会交换他们的角色，也就是新的 To 就是上次 GC前的 From ，新的 From 就是上次 GC 前的 To 。不管怎样，都会保证名为 To 的 Survivor 区域是空的。Minor GC 会一直重复这样的过程，直到 To 区被填满，To 区被填满之后，会将所有对象移动到年老代中。

<img src="https://s2.loli.net/2022/10/25/Zsv3rwCzQbOAl5K.png" alt="young_gc" style="zoom:70%;" />

摘自：[v8 GC机制 ](https://www.cnblogs.com/coderL/p/7941914.html) 👀 本来想做 V8 GC 相关笔记的，也发现和另外看的一篇 V8 GC 不一样，多了 eden space 等，就先摘抄了；结果看到最后发现了 `XX-MaxTenuringThreshold ` 的参数，搜了下，发现是 JVM 的参数... 便将笔记放到了这里。