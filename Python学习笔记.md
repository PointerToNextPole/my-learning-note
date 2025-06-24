# Python 学习笔记



#### 资料

- [Understand Django](https://www.mattlayman.com/understand-django) ：[@100gle](https://twitter.com/1oogle) [推荐](https://x.com/1oogle/status/1773376213731668129) 的开源免费电子书
- Fluent Python / Python Cookbook 这些名声在外的书，自然不用多提
- [怎样才能写出 Pythonic 的代码？ - 知乎](https://www.zhihu.com/question/21408921)



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



## 工程化相关

#### 包管理器

###### uv / pipenv

> 从一个前端的角度：感觉和 npm 有点相似，但是多了 venv 特性
