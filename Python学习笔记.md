# Python 学习笔记



#### 资料

- [Understand Django](https://www.mattlayman.com/understand-django) ：[@100gle](https://twitter.com/1oogle) [推荐](https://x.com/1oogle/status/1773376213731668129) 的开源免费电子书
- Fluent Python / Python Cookbook 这些名声在外的书，自然不用多提
- [怎样才能写出 Pythonic 的代码？ - 知乎](https://www.zhihu.com/question/21408921)
- [你见过哪些令你瞠目结舌的 Python 代码技巧？ - 知乎](https://www.zhihu.com/question/37904398)
- [Python语言从2.7到3.14的能力变化与演进逻辑](https://mp.weixin.qq.com/s/iRVqDjvfsilxRugG65fJNw)

#### `python -m`

`python3 -m` 是 Python 中一个非常强大且常用的命令行选项。简单来说，它的作用是 **将库中的模块（module）当作脚本（script）来运行**。其中的 `-m` 代表 **module**（模块）。

 - JSON美化/校验：`python3 -m json.tool data.json`
 - Base64编解码：`echo "Hi" | python3 -m base64` ；解码 `-d`
 - 代码微基准：`python3-m timeit '[i for i in range(1000)]'`
 - 性能分析：`python3-m cProfile -s time your_script.py`
 - ZIP压缩/解压：`python3-m zfile -c out.zip a.txt b.txt` ；解压 `-e`
 - Python之禅：`python3 -m this`
 - 反重力彩蛋：`python3 -m antigravity`
 - 终端日历：`python3 -m calendar 2026`

学习自：[实用好玩的python](https://bilibili.com/video/BV1pnFdzjEa3)
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

当然，<font color=red>在实践上，你会看到很多人为了性能或其他什么原因（比如当作 Hash key）使用元组，而不真正关心所谓的异质和同质容器</font>。当然，这些特性也是可解释的——Hash key 一般要求一个稳定的、哈希值不会改变的值，并且需要是能够自然比较相等性的，元组的不变性也很适合这点。

进一步的，一个定长的异质容器让你想到了其他什么呢？如果你有 C/C++ 的经验，很容易想到一个朴素的 struct ——你会很自然地发觉，一个 `struct MyType { foo: int, bar: str, baz: float }` 和 `tuple[int, str, float]` 是等价的，在非托管语言中，它们在内存上的布局也完全一样。 因此，如果涉及元编程，将 struct 的退化为元组处理是很合适的——例如对于这个叫作 MyType 的 struct，假设我们有一个 `get_fields(value)` 函数，它对于 MyType 类型的值只能返回 `tuple[int, str, float]`，而不可能是别的值。当然，这些都是后话了。

> 💡 上面提到了 “非托管语言” ，可以看下 [原生语言和托管语言的本质区别是什么？ - 知乎](
https://www.zhihu.com/question/294040278)

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

#### mypy / myright




#### Pydantic

> [!NOTE]
>
> Pydantic 的作用和 TypeScript 生态中的 [zod](https://github.com/colinhacks/zod) 是一致的。见 [[#Pydantic 与 Zod 的共同点]]

###### 核心优势

- **对 IDE 极其友好：** 因为使用纯 Python 类型，PyCharm、VS Code 等 IDE 可以提供完美的自动补全、类型检查和重构支持。
- **速度极快（尤其是 V2）：** <font color=lightSeaGreen>Pydantic V2 的核心验证逻辑使用 **Rust** 重写（pydantic-core），性能比 V1 提升了 5 到 50 倍</font>，是目前最快的 Python 验证库之一。
- **支持复杂结构：** 轻松处理嵌套字典、列表、泛型以及自定义数据类型。
- **JSON Schema 生成：** <font color=red>可以一键将 Python 模型导出为标准的 JSON Schema</font>，方便与其他系统集成。

###### Pydantic 在 Python 生态中的地位

Pydantic 已经成为了现代 Python 后端和 AI 开发的基石：

1. **FastAPI 的灵魂：** <font color=red>著名 Web 框架 FastAPI 完全基于 Pydantic 构建。路由请求的参数校验、JSON 响应的序列化、OpenAPI/Swagger 文档的自动生成，全部由 Pydantic 在底层完成</font>。
2. **AI 与大语言模型（LLM）：** 在目前火热的 AI 开发中（如 LangChain, LlamaIndex, Instructor），Pydantic 被广泛用于**定义结构化输出**。你可以定义一个 Pydantic 模型，强制 ChatGPT 返回符合该模型结构的 JSON 数据。
3. **SQLModel：** 同为 FastAPI 作者开发的 ORM 库，它将 Pydantic 模型与 SQLAlchemy 模型完美融合，实现了“一个模型类，既是数据库表，也是数据验证类”。

###### Pydantic 与 Zod 的共同点

在 TypeScript 和 Python 中，类型系统（TS Types / Interfaces 和 Python Type Hints）都有一个致命的弱点：**它们只存在于静态检查阶段，在运行时（Runtime）会完全消失或不生效。**
当你从网络请求（API、数据库、JSON）接收到未知数据时，TS 的 as User 或 Python 的 user: User = data 仅仅是“掩耳盗铃”，并不能保证数据真的是这个结构。

**Pydantic 和 Zod 都是为了打破这个屏障而生的：**

- **运行时验证 (Runtime Validation)：** 确保传入的数据真正符合预期的结构。
- **解析与转换 (Parse, don't validate)：** 它们不仅仅是检查报错，还会主动做数据转换（Coercion）。比如，Zod 接收到 "123" 可以通过 z.coerce.number() 转成数字，而 Pydantic 默认就会把 "123" 转为 int，把 ISO 字符串转为 Date/datetime 对象。
- **类型推导 (Type Inference)：** 让验证逻辑与静态类型系统完美契合，做到“一次定义，运行时和静态检查双丰收”。

##### Pydantic 和 Zod 语法与设计哲学的差异（Type-First vs Schema-First）

虽然目的相同，但受限于 Python 和 TS 语言特性的不同，它们的 API 设计方向正好是**相反的**：

###### Zod: Schema-First（先写 Schema，再推导类型）

因为 TypeScript 的类型在运行时完全不存在（被擦除了），Zod 无法读取你的 TS interface。因此，你必须先用 Zod 提供的方法（纯 JS 代码）构建一个 Schema，然后再通过 `z.infer` 把静态类型“反向提取”出来。

###### Pydantic: Type-First（先写类型，自动生成 Schema）

Python 的类型提示（Type Hints）在运行时是保留在类属性里的（可以通过 __annotations__ 拿到）。因此，Pydantic 允许你**直接使用 Python 原生的类型来定义模型**，Pydantic 在底层自动为你生成验证 Schema。

##### 生态位映射 (Ecosystem Mapping)

如果你是一个全栈开发者，你会发现它们在前后端栈中的位置惊人地对称：

| 场景                 | TypeScript 生态 (Zod)                                   | Python 生态 (Pydantic)                                       |
| -------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| **全栈/API 框架**    | **tRPC** (用 Zod 定义输入输出)                          | **FastAPI** (用 Pydantic 定义请求响应)                       |
| **环境变量管理**     | **T3 Env** (基于 Zod 验证 .env)                         | **Pydantic Settings** (解析和验证 .env)                      |
| **表单验证**         | **React Hook Form** + Zod Resolver                      | **WTForms** (传统) / FastAPI Form (底层也是 Pydantic)        |
| **大语言模型 (LLM)** | **Vercel AI SDK** / LangChain JS (用 Zod 强制输出 JSON) | **Instructor** / LangChain / OpenAI SDK (用 Pydantic 强制输出 JSON) |

随着 AI 的爆发，Pydantic 和 Zod 现在都是做 Prompt Engineering（让 LLM 输出结构化数据）的绝对主力工具。

摘自：[AI Studio - Python 数据验证库 Pydantic 介绍](https://aistudio.google.com/app/prompts?state=%7B%22ids%22:%5B%2210bio29qSFopYrVdC24n9Hgf7ILoOTH7Z%22%5D,%22action%22:%22open%22,%22userId%22:%22113986502377656987379%22,%22resourceKeys%22:%7B%7D%7D&usp=sharing)



#### pyproject.toml



#### SQLAlchemy 与 Alembic

可以看下 [AI Studio - SQLAlchemy 与 Alembic 简介](https://aistudio.google.com/app/prompts?state=%7B%22ids%22:%5B%221R2pAR0elF2lx6GlJ7KsGHFRrEAaqHUzg%22%5D,%22action%22:%22open%22,%22userId%22:%22113986502377656987379%22,%22resourceKeys%22:%7B%7D%7D&usp=sharing)



### uv 

###### uv 和 npm 命令对比

- `uv add --dev` 类似于 `npm add -D`
- `uv tool install` 类似于 `npm add -g`
- `uvx` 类似于 `npx`

学习自：[AI Studio Gemini 回答](https://aistudio.google.com/app/prompts?state=%7B%22ids%22:%5B%2212OYmRGYJIgep89yHGAF2bnxMmXgUeffQ%22%5D,%22action%22:%22open%22,%22userId%22:%22113986502377656987379%22,%22resourceKeys%22:%7B%7D%7D&usp=sharing)

##### uv 和 npm / pnpm 的对比

如果硬要用前端生态来对标，uv **绝不仅仅等于 pnpm**。  

**uv = pnpm (包管理) + nvm / fnm (Node版本管理) + npx (工具执行) + tsc 的初始化速度。**

以下是 uv 真正超出 pnpm 范畴的核心点：

##### Python 解释器（运行时）管理

在前端，pnpm 只管装包，不管你电脑上装的是 Node 18 还是 Node 20（那是 nvm 或 fnm 的工作）。  

但 **uv 直接接管了 Python 版本管理**。如果你的项目需要 Python 3.12，你<font color=red>甚至不需要提前安装 Python，直接运行</font>：

```sh
uv run script.py
```

`uv` 会在底层自动下载 Python 3.12，创建隔离环境并运行。这相当于 `pnpm` 发现你本地没装 Node，自动帮你下载一个 Node 运行项目。

##### 终结 Python 生态的“碎片化”

前端的包管理虽然经历过 npm -> yarn -> pnpm 的演进，但核心一直比较统一（就是 package.json）。  
而 Python 过去十年的包管理极其混乱：

- 装包用 pip
    
- 锁定版本用 pip-tools
    
- 管理虚拟环境用 virtualenv
    
- 管理多版本 Python 用 pyenv
    
- 现代项目管理用 Poetry 或 Pipenv
    
- 全局安装命令行工具用 pipx
    

**uv 的出现是为了“大一统”。** 它的目标是用一个极速的 Rust 编译的单一二进制文件，替代上面所有的工具（类似 Rust 生态里的 Cargo）。

###### 全局工具执行（类似 npx）

前端有非常方便的 `npx` ，可以不安装包直接运行（比如 `npx create-react-app` ）。  
Python 过去需要用 `pipx` ，而 `uv` 原生支持了 uvx（或者 `uv tool run` ）。

##### 解决跨平台编译难题

前端的依赖 95% 是纯 JavaScript 代码，最多包含一点 WebAssembly 或 node-gyp 编译的 C++ 扩展。  

Python 的包（特别是 AI、数据科学领域的包如 PyTorch, NumPy）包含了大量复杂的 C/C++/Fortran 底层代码。uv 在处理这些预编译二进制包（Wheels）的解析树时，面临的图计算复杂度远超前端，但它的速度依然快得惊人。


