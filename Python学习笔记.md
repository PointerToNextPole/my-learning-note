# Python 学习笔记



#### 资料

- [Understand Django](https://www.mattlayman.com/understand-django) ：[@100gle](https://twitter.com/1oogle) [推荐](https://x.com/1oogle/status/1773376213731668129) 的开源免费电子书
- Fluent Python / Python Cookbook 这些名声在外的书，自然不用多提
- [怎样才能写出 Pythonic 的代码？ - 知乎](https://www.zhihu.com/question/21408921)
- [Python语言从2.7到3.14的能力变化与演进逻辑](https://mp.weixin.qq.com/s/iRVqDjvfsilxRugG65fJNw)


#### 元组

元组和列表的设计目的是完全不同的。具体来说，列表表示一个同质 ( Homogeneous ) 的容器，而元组表示一个异质 ( Heterogeneous ) 的容器——简单来说，列表在设计上，元素的类型是相同的，而元组的每一个元素类型都是不同的。

```py
lst: list[int] = [1, 2, 3, 4, 5]
tpl: tuple[int, str, float] = (114514, "foo", 42.5)
```

由于元组在设计上是异质的，每一个元素的类型理论上都需要单独指定，因此它理所当然是定长的，不可后续增删改——因为你不可能在编译期未知数量的元素中，为它们每一个都单独指定类型。

当然，由于 Python 通常被认为是“动态类型语言”，你可以说这与你的认知大相径庭——列表明明就可以放任意类型的元素，而元组也经常用来当作一个不可变的列表（即 frozenlist ）用，我谈到的这种来自于静态类型语言的 “教条式认知” 是否过于迂腐？

答案已经体现在 Type hints 的设计中——至少我们看到，在 Python 开发团队大多数人的眼中，列表在设计上是一个同质容器，因此它的泛型定义是 `list[T]`，而元组是一个异质容器，因此它的泛型定义为 `tuple[*Ts]`。

实际上，我们可以将一个 “包含不同类型元素” 的列表理解为包含唯一一种类型，即这其中所有类型的 union 的同质容器：

```py
lst: list[int | str | float] = [1, "foo", 42.5, 5, "bar"]
```

而仅包含一种类型的元组，只不过是把它每个位置上的类型写作相同而已：

```py
tpl: tuple[int, int, int, int, int] = (1, 2, 3, 4, 5)
```

当然，对于<font color=dodgerBlue>**不定长的元组**</font>，我们没办法像这样为每个元素写上类型。Type hints 中有一种特殊的语法解决这个问题，可以把元组类型当作同质类型使：

```py
tpl: tuple[int, ...] = (1, 2, 3, 4, 5)
```

你会发现，元组在 Type hints 上的确可以当作列表使用——但反过来并不成立，因为 `list[T]` 不编码容器的长度。这也导致很多场景下，Type hints 中只能用元组类型处理，比如标注 `*args` 的类型：

```py
def f(*args: *tuple[int, str, float]) -> None:
    print(args)

def g(*args: *tuple[int, ...]) -> None:  # 等价于 *args: int
    print(args)
```

在 Python 3.11 之前，这种语法是不允许的——在那之前，我们只能将 `*args` 的类型标注为同一类型的若干个未知长度的元素，但在一些特定情况下，我们希望更细化它们的类型，这时元组类型就被自然用上了。

当然，在实践上，你会看到很多人为了性能或其他什么原因（比如当作 Hash key）使用元组，而不真正关心所谓的异质和同质容器。当然，这些特性也是可解释的——Hash key 一般要求一个稳定的、哈希值不会改变的值，并且需要是能够自然比较相等性的，元组的不变性也很适合这点。

进一步的，一个定长的异质容器让你想到了其他什么呢？如果你有 C/C++ 的经验，很容易想到一个朴素的 struct ——你会很自然地发觉，一个 `struct MyType { foo: int, bar: str, baz: float }` 和 `tuple[int, str, float]` 是等价的，在非托管语言中，它们在内存上的布局也完全一样。 因此，如果涉及元编程，将 struct 的退化为元组处理是很合适的——例如对于这个叫作 MyType 的 struct，假设我们有一个 `get_fields(value)` 函数，它对于 MyType 类型的值只能返回 `tuple[int, str, float]`，而不可能是别的值。当然，这些都是后话了。

摘自：[Python编程“元组”这个功能是不是多余了？ - Snowflyt的回答 - 知乎](
https://www.zhihu.com/question/1930753510372283154/answer/1934567538018218580)

举个简单的例子，字典的键可以用，我可以确保这个字典的键永远不变。如果键用列表，那列表里面加了一个值，键就变了，就找不到了。

程序设计很多地方都是用来防呆的，程序员永远不可能不犯错，要尽量在语言层面降低程序员犯蠢的概率。

摘自：[Python编程“元组”这个功能是不是多余了？ - 心随风的回答 - 知乎](
https://www.zhihu.com/question/1930753510372283154/answer/1930783103447700834)
#### 海象表达式

// TODO



#### 生成器表达式

#### 列表推导



#### enumerate

> 参考 [怎样才能写出 Pythonic 的代码？ - Snowflyt的回答 - 知乎](https://www.zhihu.com/question/21408921/answer/2557328805) 中的 “使用 emumerate 取代range(len(...))”



#### 函数式 API

##### takewhile



##### dropwhile



#### Future

###### 引入时机

Python 中 Future 的概念主要是通过两个标准库模块引入的：

1. **`concurrent.future` 模块** ：这个模块最早是在 **Python 3.2** 中被引入的。它是作为 [PEP 3148](https://peps.python.org/pep-3148/) 的一部分添加进来的，提供了一个高层级的接口来异步执行可调用对象（使用线程池或进程池）。
2. **`asyncio` 模块（及其 `asyncio.Future`）** ： 这个模块，连同它自己的 `Future` 实现 (`asyncio.Future`)，是在 **Python 3.4** 中作为 [PEP 3156](https://peps.python.org/pep-3156/) 的一部分被引入的。最初它也是一个 provisional (暂定) 包。与 `asyncio` 紧密相关的 `async` 和 `await` 语法则是在 **Python 3.5** 中正式成为关键字，极大地简化了基于 `asyncio` 的异步编程。

因此，可以说 Future 的概念从 **Python 3.2** 开始出现在标准库中，并在 **Python 3.4** 得到了 `asyncio` 框架下的特定实现。

##### 和 JS 中 Promise 的比较

在 Python 的标准库中，**没有**一个直接叫做 `Promise` 的对象，这个术语主要来自 JavaScript。

然而，Python 中的 **`Future` 对象** (存在于 `concurrent.futures` 和 `asyncio` 模块中) 扮演着与 JavaScript 中的 `Promise` **极其相似**的角色。

###### 可以这样理解

1. **核心功能相同**：`Future` 和 `Promise` 都代表一个异步操作的最终结果。它们都是一个**占位符**，表示一个值（或错误）将在未来的某个时间点可用。
2. **状态管理**：两者都有类似的状态，比如“待定”（pending）、“已完成/已履行”（fulfilled/resolved）、“已失败/已拒绝”（failed/rejected）。
3. 结果/错误处理：
   - JavaScript `Promise` 使用 `.then()` 来处理成功的结果，`.catch()` 来处理错误，或者 `.finally()` 来执行无论成功失败都运行的代码。
   - Python `Future` 可以使用 `.result()` 来获取结果（如果未完成会阻塞，如果失败会抛出异常），`.exception()` 来获取异常，以及 `.add_done_callback()` 来附加一个在 Future 完成时（无论成功、失败或取消）被调用的函数。
   - 在 `asyncio` 中，`await` 关键字通常用于等待 `Future` (或 `Task`，它是 `Future` 的子类) 完成，提供了更像同步代码的写法来处理异步结果和异常。

###### 总结

虽然名字不同，但 Python 中的 Future 在概念和功能上是与 JavaScript 中的 Promise 最接近的对应物。如果你熟悉 JavaScript Promises，那么可以将 Python 的 Future 理解为实现类似异步结果管理机制的方式。Python 社区和标准库选择了 Future 这个术语。

摘自：https://g.co/gemini/share/d04df58ba8ba



#### `with` 关键字

with 语句实现原理建立在上下文管理器之上。

上下文管理器是一个实现 **`__enter__`** 和 **`__exit__`** 方法的类。

使用 with 语句确保在嵌套块的末尾调用 `__exit__` 方法。

摘自：[菜鸟教程 - Python3 教程 - Python with 关键字](https://www.runoob.com/python3/python-with.html)



#### GIL



## 工程化相关

#### 包管理器

###### uv / pipenv

> 从一个前端的角度：感觉和 npm 有点相似，但是多了 venv 特性
