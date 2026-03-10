# AI 概念与工具使用笔记


## RAG

##### LLM 是涌现性智能

现在的 LLM 存在的问题之一是幻觉问题 ( hallucination )。因为 LLM 带来的巨大讨论量，直接让 hallucination 这个小众词汇成为 2023 年度剑桥词典的年度热词。回到幻觉问题本身：因为 <font color=red>LLM 本身是基于从大量数据中训练出来的概率模型来一个个生成 token</font>，也就是<font color=red>它并没有逻辑和事实基线</font>，所以我们说 <font color=fuchsia>LLM 的智能是**涌现性的智能**，是 **基于概率产生** 的 “伪智能”，不是底层基于逻辑和推理能力 “真智能”</font> 。但这对应用层无所谓，因为这个涌现性的智能已经足够强大，在应用层已经表现出了足够使用的逻辑和推理能力。

#### RAG 的原理

RAG 的基本流程

1. 用户输入提问
2. 检索：<font color=red>根据用户提问对 **向量数据库** 进行相似性检测，查找与回答用户问题最相关的内容</font>
3. 增强：根据检索的结果，生成 prompt 。一般都会涉及 “仅依赖下述信息源来回答问题” 这种限制 LLM 参考信息源的语句，来减少幻想，让回答更加聚焦
4. 生成：将增强后的 prompt 传递给 LLM ，返回数据给用户

RAG 是哪里有问题解决哪里，既然大模型无法获得最新和内部的数据集，那我们就使用外挂的向量数据库为 llm 提供最新和内部的数据库。既然大模型有幻想问题，我们就将回答问题所需要的信息和知识编码到上下文中，强制大模型只参考这些内容进行回答。

RAG 更底层的逻辑，也是我们对待 llm 正确的态度：<font color=red>LLM 是**逻辑推理引擎**，而不是信息引擎</font>。所以，由外挂的向量数据库提供最有效的知识，然后由 llm 根据知识进行推理，提供有价值的回复。

#### RAG 的流程

在理解了 RAG 的基本原理后，我们快速和宏观的介绍如何打造一个 RAG chat bot 的全流程。

1. **加载数据**

   因为想要根据用户的提问进行语意检索，我们<font color=red>需要将数据集放到向量数据库中</font>，所以<font color=lightSeaGreen>需要将不同的数据源加载进来</font>。这里涉及到多种数据源，例如 pdf、code、现存数据库、云数据库等等。

2. **切分数据**

   GPT-3.5 Turbo 的上下文窗口是 16k ，GPT-4 Turbo 上下文窗口是 128k ，而<font color=lightSeaGreen>很多数据源都很容易比这个大</font>。更何况，<font color=lightSeaGreen>用户的提问经常涉及多个数据源</font>，所以<font color=red>需要对数据集进行语意化的切分</font>，根据内容的特点和目标大模型的特点、上下文窗口等，对数据源进行合适的切分。

   > 👀 这里单位 “k” 表示 “千”

   这里听起来比较容易，但<font color=lightSeaGreen>考虑到数据源的多种多样和自然语言的特点</font>，<font color=red>切分函数的选择和参数的设定是非常难以控制的</font>。理论上希望每个文档块都是语意相关，并且相互独立的。

3. **嵌入 ( embedding )**

   这部分对没有机器学习相关背景的同学不容易理解。这里<font color=dodgerBlue>用最简单的 **词袋 ( words bag ) 模型** 来描述一下最简单的 embedding 过程</font>，让大家更具象化的理解这个。

   a. 词袋模型就是最简化的情况：把一篇 句子/文章 中的单词提前出来，就像放到一个袋子里一样，认为单词之间是独立的，并不关心词与词之间的上下文关系。

   b. 假设有十篇英语文章，<font color=red>可以把每个文章拆分成单词，并且还原成最初的状态</font>（例如 did、does => do），然后<font color=red>统计每个词出现的次数</font>。 简化一下假设最后结果就是

   ```
   第一篇文章: 
   apple: 10, phone:12
   
   第二篇文章:
   apple: 8, android: 10, phone: 18
   
   第三篇文章:
   banana: 6, juice: 10
   ```

   c. 尝试构建一个向量，也就是一个数组，每个位置有一个值，代表每个单词在这个文章中出现的次数

   ```
   变量
   [apple, banana, phone, android, juice]
   ```

   那每篇文章，都能用一个变量来表示

   ```
   第一篇文章: [10, 0, 12, 0, 0]
   第二篇文章: [8, 0, 18, 10, 0]
   第三篇文章: [0, 6, 0, 0, 10]
   ```

   d. <font color=red>这样我们就能把一篇文章用一个向量来表示了</font>，然后可以用最简单的余弦定理去计算两个向量之间的夹角，以此确定两个向量的距离。

   > 💡 这里的算法是：$\cos\theta=\frac{a \cdot b}{| a | \times | b |}$

   e. 这样，我们就有了通过向量和向量之间的余弦夹角的，来衡量文章之间相似度的能力，是不是很简单。

   当然，这是最最最简单的 embedding 原理，不过是<font color=lightSeaGreen>所有的 embedding 和相似性搜索都是类似的原理</font>。

   回到 RAG 流程中，我们<font color=red>**将切分后的每一个文档块使用 embedding 算法转换成一个向量，存储到向量数据库中 ( vector store ) 中**</font>。这样，每一个原始数据都有一个对应的向量，可以用来检索。

4. **检索数据**

   当所有需要的数据都存储到向量数据库中后，我们<font color=red>就可以把用户的提问也 embedding 成向量，用这个向量去向量数据库中进行检索</font>，<font color=lightSeaGreen>找到相似性最高的几个文档块，返回</font>。

   在这里，使用什么算法去计算向量之间的距离也是需要选择的，这个我们会在对应的章节进行介绍。

5. **增强 prompt**

   在有了跟用户提问最相关的文档块后，我们根据文档块去构建 prompt 。一般格式都类似于

   ```
   你是一个 xxx 的聊天机器人，你的任务是根据给定的文档回答用户问题，并且回答时仅根据给定的文档，尽可能回答
   用户问题。如果你不知道，你可以回答“我不知道”。
   
   这是文档:
   {docs}
   
   用户的提问是:
   {question}
   ```

6. **生成**

   然后就是将组装好的 prompt 传递给 chatbot 进行生成回答。

学习与摘抄自：[从前端到 AI：LangChain.js 入门和实战 - 5. 检索增强生成（RAG）原理和流程](https://juejin.cn/book/7347579913702293567/section/7351410645298135091)



## 通用概念



#### 召回率

> 💡 这个概念涉及到 “混淆矩阵” ( confusion matrix ) 概念





## 工具使用

### MCP



#### 概念

可以看下 [编译 | 对话 MCP 创始人：MCP 的起源、挑战与未来 - 段小草的文章 - 知乎](https://zhuanlan.zhihu.com/p/1892538540031705247) ，其中虽然感觉文章有些过长，没耐心看完，不过看的部分还是可以发现一些有用的信息：

- MCP 借鉴自 LSP。

- 针对 $M \times N$ 的场景（个人的理解是：存在多个系统，同时每个系统都暴露出多个独立的接口与其他系统通讯。也可以简单类比为数据库表中 “多对多” 的关系），通过一个协议进行通讯是很好的策略

  > 很快你就发现，**这显然是一个 $M \times N$ 的问题**。你有多个应用和多个你想构建的集成，用协议来解决这个问题再合适不过了

- 和 LSP 一样，使用 JSON-RPC 实现通讯；不过，使用方式存在差异



### A2A / Agent2Agent

###### 文档

https://google.github.io/A2A/

###### A2A 是什么？

Agent2Agent 开放协议，支持不同厂商或框架开发的智能体安全通信、信息交换与行动协同，解决大规模多智能体部署的挑战，赋予开发者跨平台构建互操作智能体的能力。

###### A2A 和 MCP 是什么关系？

谷歌自己说，A2A 和 MCP 是互补的，也确实如此。MCP 解决了大模型如何使用工具的问题，属于赋予单个 Agent 更多能力的通用协议；A2A 瞄准的是不同厂商、不同开发者、不同技术栈之间多个 Agent 的通信和协作问题。

换句话说，**MCP 让开发者更容易构建单个 Agent，A2A 让这些 Agent 之间可以协作交互。**

摘自：[谷歌发布的 A2A 协议是什么？会给 Agent 的发展带来什么影响？ - 段小草的回答 - 知乎](https://www.zhihu.com/question/1893443983843255220/answer/1893814587071111612)



#### 一些技巧

##### 带有权限地使用 npx 执行

当用 npx 执行任意 npm 包时，你实际上给予了它在你的电脑上执行任意 nodejs 代码的权限，潜在风险包括但不限于读取文件系统（比如读取私钥）；这是非常危险的。

permissions 是 Node.js 的一个新特性，当传入 `--permission` flag 时，Node 会默认禁用读写文件系统、派生进程等权限，使用这些权限需要手动声明。

所以，可以使用如下命令：

```bash
npx --node-options="--permission --allow-fs-read=$(npm config get cache)" package-name
```

`--allow-fs-read` 声明了仅可以访问 npm cache 目录（如果不加这个的话 npx 本身执行脚本会报错），但同时把权限限定在了这一个目录，防止了 npm 包读取文件系统任意文件的风险

学习自：[twitter - @wong2_x 的帖子](https://x.com/wong2_x/status/1907331129809723862)


## Claude & Claude Code

##### 资料

- [Claude Code 官方中文文档](https://code.claude.com/docs/zh-CN/)

#### AnyRouter 教程

###### 使用方法

```sh
# 启动交互模式
claude

# 以初始查询启动
claude "解释这个项目"

# 运行单个命令并退出
claude -p "这个函数做什么？"

# 处理管道内容
cat logs.txt | claude -p "分析这些错误"
```

###### 恢复对话

```sh
# 立即恢复最近的对话
claude --continue

# 选择特定对话
claude --resume
```

###### 图像处理

Claude Code 可以处理图像信息，支持多种输入方式：

- 将图像拖放到 Claude Code 窗口中
- 复制图像并使用 cmd+v 粘贴到 CLI 中
- 提供图像路径

###### 记忆系统

Claude Code 通过 `CLAUDE.md` 文件存储重要的项目信息、约定和常用命令。

```sh
# 初始化记忆文件
/init

# 快速添加记忆
# 始终使用描述性变量名
```

###### 压缩上下文

使用压缩功能节省开销：

```sh
/compact [instructions]
```

###### Git 集成

Claude Code 支持使用自然语言操作 Git：

```sh
> 提交我的更改
> 创建一个 pr
> 哪个提交在去年十二月添加了 markdown 测试？
> 在 main 分支上变基并解决任何合并冲突
```


### 官方文档笔记

#### 斜杠命令

在交互式会话中使用斜杠命令控制 Claude 的行为。

##### 内置斜杠命令

|命令|用途|
|---|---|
|`/add-dir`|添加额外的工作目录|
|`/agents`|管理用于专门任务的自定义 AI 子代理|
|`/bashes`|列出和管理后台任务|
|`/bug`|报告错误（将对话发送给 Anthropic）|
|`/clear`|清除对话历史|
|`/compact [instructions]`|压缩对话，可选择性地提供焦点说明|
|`/config`|打开设置界面（配置选项卡）|
|`/context`|将当前上下文使用情况可视化为彩色网格|
|`/cost`|显示令牌使用统计。有关订阅特定的详细信息，请参阅[成本跟踪指南](https://code.claude.com/docs/zh-CN/costs#using-the-cost-command)。|
|`/doctor`|检查您的 Claude Code 安装的健康状况|
|`/exit`|退出 REPL|
|`/export [filename]`|将当前对话导出到文件或剪贴板|
|`/help`|获取使用帮助|
|`/hooks`|管理工具事件的钩子配置|
|`/ide`|管理 IDE 集成并显示状态|
|`/init`|使用 `CLAUDE.md` 指南初始化项目|
|`/install-github-app`|为存储库设置 Claude GitHub Actions|
|`/login`|切换 Anthropic 账户|
|`/logout`|从您的 Anthropic 账户登出|
|`/mcp`|管理 MCP 服务器连接和 OAuth 身份验证|
|`/memory`|编辑 `CLAUDE.md` 内存文件|
|`/model`|选择或更改 AI 模型|
|`/output-style [style]`|直接设置输出样式或从选择菜单中选择|
|`/permissions`|查看或更新[权限](https://code.claude.com/docs/zh-CN/iam#configuring-permissions)|
|`/plan`|直接从提示进入计划模式|
|`/plugin`|管理 Claude Code 插件|
|`/pr-comments`|查看拉取请求注释|
|`/privacy-settings`|查看和更新您的隐私设置|
|`/release-notes`|查看发布说明|
|`/rename <name>`|重命名当前会话以便于识别|
|`/remote-env`|配置远程会话环境（claude.ai 订阅者）|
|`/resume [session]`|按 ID 或名称恢复对话，或打开会话选择器|
|`/review`|请求代码审查|
|`/rewind`|回退对话和/或代码|
|`/sandbox`|启用沙箱化 bash 工具，具有文件系统和网络隔离，用于更安全、更自主的执行|
|`/security-review`|对当前分支上的待处理更改完成安全审查|
|`/stats`|可视化每日使用情况、会话历史、连胜记录和模型偏好|
|`/status`|打开设置界面（状态选项卡），显示版本、模型、账户和连接性|
|`/statusline`|设置 Claude Code 的状态行 UI|
|`/teleport`|按会话 ID 从 claude.ai 恢复远程会话，或打开选择器（claude.ai 订阅者）|
|`/terminal-setup`|为换行安装 Shift+Enter 键绑定（VS Code、Alacritty、Zed、Warp）|
|`/theme`|更改颜色主题|
|`/todos`|列出当前 TODO 项目|
|`/usage`|仅适用于订阅计划：显示计划使用限制和速率限制状态|
|`/vim`|进入 vim 模式以在插入和命令模式之间交替|

##### 自定义斜杠命令

自定义斜杠命令允许您将经常使用的提示定义为 Markdown 文件，Claude Code 可以执行这些文件。命令按范围（特定于项目或个人）组织，并通过目录结构支持命名空间。

> [!TIP]
> 斜杠命令自动完成在您的输入中的任何位置都有效，而不仅仅在开头。在任何位置键入 `/` 以查看可用命令。

###### 语法

```sh
/<command-name> [arguments]
```

###### 参数

|参数|描述|
|---|---|
|`<command-name>`|从 Markdown 文件名派生的名称（不包括 `.md` 扩展名）|
|`[arguments]`|传递给命令的可选参数|


## Prompt

##### 一些资料

- [让AI聪明45%的心理学Prompt技巧](https://mp.weixin.qq.com/s/rA1EzPK5Gk0yNNBUXNAlRg)
- [RIPER-5](https://github.com/NeekChaw/RIPER-5)

#### amp prompt 指南

 For the best results, follow these guidelines:

- <font color=red>**Be explicit with what you want**</font>. Instead of “can you do X?”, try “do X.”
- Keep it short, keep it focused. Break very large tasks up into smaller sub-tasks, one per thread. <font color=red>Do not ask the agent to write database migrations in the same thread as it previously changed CSS for a documentation page</font>.
- Don’t try to make the model guess. If you know something about how to achieve what you want the agent to do — which files to look at, which commands to run — put it in your prompt.
- <font color=dodgerBlue>If you want the model to not write any code</font>, but only to research and plan, say so: “Only plan how to implement this. Do NOT write any code.”
- Use [`AGENTS.md` files](https://ampcode.com/manual#AGENTS.md) to guide Amp on how to run your tests and build steps and to avoid common mistakes.
- <font color=red>Abandon threads if they accumulated too much noise</font>. Sometimes things go wrong and failed attempts with error messages clutter up the context window. In those cases, it’s often best to start with a new thread and a clean context window.
- Tell the agent how to best review its work: what command or test to run, what URL to open, which logs to read. Feedback helps agents as much as it helps us.

<font color=red>**The first prompt in the thread carries a lot of weight**</font>. It sets the direction for the rest of the conversation. We encourage you to be deliberate with it. That’s why we use Cmd/Ctrl+Enter to submit a message in Amp — it’s a reminder to put effort into a prompt.

<font color=dodgerBlue>Here are some examples of prompts we’ve used with Amp:</font>

- “Make `observeThreadGuidanceFiles` return `Omit<ResolvedGuidanceFile, 'content'>[]` and remove that field from its return value, and update the tests. Note that it is omitted because this is used in places that do not need the file contents, and this saves on data transferred over the view API.” ([See Thread](https://ampcode.com/threads/T-9219191b-346b-418a-b521-7dc54fcf7f56))
- “Run `<build command>` and fix all the errors”
- “Look at `<local development server url>` to see this UI component. Then change it so that it looks more minimal. Frequently check your work by screenshotting the URL”
- “Run git blame on the file I have open and figure out who added that new title”
- “Convert these 5 files to use Tailwind, <font color=red>use one subagent per file</font>”
- “Take a look at `git diff` — someone helped me build a debug tool to edit a Thread directly in JSON. Please analyze the code and see how it works and how it can be improved. […]” ([See Thread](https://ampcode.com/threads/T-39dc399d-08cc-4b10-ab17-e6bac8badea7))
- “Check `git diff --staged` and remove the debug statements someone added” ([See Thread](https://ampcode.com/threads/T-66beb0de-7f02-4241-a25e-50c0dc811788))
- “Find the commit that added this using git log, look at the whole commit, then help me change this feature”
- “<font color=red>Explain the relationship between class AutoScroller and ViewUpdater using a diagram</font>”
- “Run `psql` and rewire all the `threads` in the databaser to my user (email starts with thorsten)” ([See Thread](https://ampcode.com/threads/T-f810ef79-ba0e-4338-87c6-dbbb9085400a))

摘自：[amp - manual # How to Prompt](https://ampcode.com/manual#how-to-prompt)

### Plan mode

#### 《不要随便用 Plan Mode》阅读笔记

> 摘自：[不要随便用 Plan Mode](https://mp.weixin.qq.com/s/IhNsksFFZH57_cTzmPCF7Q)

##### 总结

**Plan Mode 应该是「先讨论 → 追问 → 再确定 plan」的流程。**

不要上来就让 AI 写 plan。而是先跟它聊：

1. 让 AI 先理解现状——读代码、分析架构、搞清楚现在是怎么运作的
   
2. <font color=red>把你的初步想法抛出来，**问它怎么看，有没有问题**</font>
   
3. <font color=red>**让 AI 提出备选方案，你来做决策**</font>
   
4. <font color=fuchsia>关键决策确认完了，再让它写 plan</font>
   

这个顺序的区别在于：**关键决策是你和 AI 一起做的，而不是 AI 自己猜的。**

<font color=red>plan 只是对已经确认的决策的执行步骤梳理</font>，而不是从零开始做所有决策。

##### 落地 SOP 与 Prompt 模版

###### Step 1：先让 AI 摸底（不要用 Plan Mode）

用普通的聊天模式。让 AI 读代码、分析现状：

```
请你先分析一下 [相关目录/文件] 的现有架构。  
我想做 [简述你的目标]。  
你觉得现有架构能不能支持？有什么需要注意的地方？
```

这一步的目的是：**让 AI 和你对现状达成共识。**

###### Step 2：抛出想法，征求意见

```
我的初步想法是 [你的方案]。
你觉得这个方案怎么样？
如果有问题，你有什么备选方案？
```

注意，<font color=lightSeaGreen>**一定要问"你觉得怎么样"和"有没有备选方案"**</font>。这不是客气，而是在**主动邀请 AI 暴露你没想到的问题**。

> [!TIP]
> 这里可以参考下 [[#《Agent 越用越翻车，怎么破局？答案藏在经典管理学里》笔记#警惕 Agent 的锚定效应]] 中的内容：多让 AI 出几个（2 - 3 个）方案，并分析各个方案的优劣，这样可以避免被 AI 牵着走

###### Step 3：纠偏 + 补充约束

AI 的分析里肯定有一些理解偏差或者遗漏。这时候你需要**精准地纠正**。

```
你说的 A 方案不太合适，因为 [你的原因]。  
另外你漏掉了一个点：[补充信息]。  
基于这些，你再想想呢？
```

这一步可能会来回几轮，完全正常。**每一轮都在缩小分歧、补充上下文。**

###### Step 4：逐个确认关键决策

AI 应该会提出几个需要你决定的技术选型或设计问题。逐个回答，别含糊。

```
关于 [决策点 A]，我选方案一，因为 [原因]。  
关于 [决策点 B]，我的想法是 [具体说明]。
```

**每个决策都要有明确的结论。** 不要说"都行"或者"你决定"——那等于又把决策权扔回给 AI 了。

###### Step 5：现在可以写 plan 了

所有关键决策确认完毕后，切到 Plan Mode：

```
好，基于我们刚才讨论确认的方案，  
请你写一个详细的执行 plan。
```

这时候出来的 plan，质量会比你直接让 AI 写高出一个档次。因为决策层面已经没有歧义了，AI 只需要专注于梳理执行步骤。

###### Step 6：快速确认，直接执行

看一眼 plan，大概率不需要大改。确认后让 AI 开干就行。

**整个过程的时间分配大概是这样的一个比例：**

- 讨论 + 决策：70% 的时间
  
- 写 plan + 确认：10% 的时间
  
- 执行：20% 的时间
  

**看起来讨论花了更多时间，但总时间反而更短。** 因为你<font color=red>省掉了"plan 写错 → 执行出错 → 回头改 plan → 重新执行"的循环</font>。

##### 什么时候可以直接用 Plan Mode？

说了这么多"不要直接用 Plan Mode"，也不是说它完全没用。有些场景直接开 plan 是没问题的：

- **改动范围非常明确的任务**：比如"把这个组件从 class 组件改成函数组件"，没什么决策空间，直接 plan 就行
  
- **你对方案已经非常确定的任务**：如果你脑子里已经有完整的方案，只需要 AI 执行，那直接写个详细的 prompt 开 plan 也可以
  
- **小范围的修改**：改几个文件的事情，plan 不 plan 的其实无所谓
  

但凡涉及**架构决策、多方案选择、改动面较广**的任务，请一定先讨论再 plan。这个顺序的调整，成本几乎为零，但收益巨大。

#### 《Agent 越用越翻车，怎么破局？答案藏在经典管理学里》笔记

> 摘自：[Agent 越用越翻车，怎么破局？答案藏在经典管理学里](https://mp.weixin.qq.com/s/N1WgUN8ddXrw0p0o5IiykA)

##### 不同的任务，管法完全不同

Agent 管理没有银弹。

这是我踩了无数坑之后得出的第一个结论。很多人用 Agent 的方式就是一刀切——不管什么任务，都扔给它一个 prompt，然后等结果。简单任务还好，复杂任务就翻车了。

Andy Grove 在《High Output Management》里提了一个概念，叫 **TRM——Task-Relevant Maturity**，任务相关成熟度。他的意思是，不存在什么"最优管理风格"，你怎么管一个人，完全取决于他在**这个具体任务**上的成熟度。

> [!NOTE]
> 
> 这里稍微有点啰嗦，而且稍显常识，所以作下总结。
> 
> 总结来说就是：简单的任务直接交给 AI 去做即可，越复杂的任务越需要干预，指导 AI 去做。另外，这部分内容，可以结合 [[#《不要随便用 Plan Mode》阅读笔记#落地 SOP 与 Prompt 模版]] 阅读
> 
> 另外，下面的内容做了下精简，详见原文

我把 Agent 任务大致分成三档：

- 高 TRM 任务：放手让它干
- 中 TRM 任务：指导 + 点拨
- 低 TRM 任务：你来主导，Agent 来执行

##### 警惕 Agent 的锚定效应

<font color=dodgerBlue>Daniel Kahneman 在《思考，快与慢》里详细讲过 **锚定效应**</font>：当你面对一个不确定的问题时，最先接触到的信息会变成你心里的"锚"，你后续所有的判断都会不自觉地围绕这个锚来调整。

> 👀 类似的，也有 “价格锚定”

锚定效应在跟 AI Agent 打交道的时候简直无处不在。

你向 Agent 抛了一个复杂问题，它返回的第一个回答——功能架构、技术选型——会立刻变成你心中的参考点。后面即使你隐约觉得不太对，心理上也倾向于在原方案上"微调"，而不是推倒重来。

###### 怎么破？

**第一，让 Agent 给你多个方案。** 这是最简单也最有效的方法。不要只要一个答案，要两到三个备选：

```markdown
给我 2-3 个可行的方案，分别说说各自的优缺点和适用场景
```

有了多个方案，你就不会被单一输出锚定了。你的角色是评估和取舍，而不是被动接受。

**第二，在看 Agent 输出之前，先自己想一想。** 哪怕只是花两分钟列几个关键决策点，形成你自己的"锚"。这样你看到 Agent 的输出时，心里是有参照系的，不会被它牵着走。

##### 善于苏格拉底式提问

Agent 输出有一个特点：**它永远都很自信。**

不管对错，<font color=lightSeaGreen>Agent 说话的语气都很笃定</font>。"我建议使用工厂模式来解决这个问题"、"这个实现是线程安全的"、"这样修改不会影响现有功能"。

<font color=lightSeaGreen>**这种自信的语气会触发你大脑的一个机制——Kahneman 叫它"认知放松"（cognitive ease）**</font>。当信息看起来连贯、有条理、语气坚定的时候，你的系统 1（快速直觉思维）会自动判定"这个靠谱"，然后你的系统 2（慢速理性思维）就懒得再去验证了。你想想平时是不是被它的语气给蛊惑了？

<font color=red>但 Agent 输出的语气自信不代表内容正确。它可能正在用一种特别自信的口吻，给你挖一个特别大的坑</font>。

怎么解决？很简单——**苏格拉底式提问**。

对 Agent 的每个关键结论追问：

- "这个方案在什么情况下会失败？"
    
- "你搞的这个改动有没有什么潜在的风险？"
    
- "有没有你没考虑到的边界情况？"
    
- "如果数据量增长 10 倍，这个设计还 hold 得住吗？"
    

这些问题的威力在于：它们强制 Agent 从另一个角度审视自己的输出。你会发现，<font color=dodgerBlue>同一个 Agent</font>，当<font color=red>你让它"论证这个方案好"的时候，它说得头头是道</font>；当<font color=red>你让它"找这个方案的问题"的时候，它也能找出一堆隐患</font>。

前几天我让 Agent 写了一个文件上传的接口，它写完之后我问了一句："这个实现有什么安全风险？"

它自己就列出来了：没有限制文件大小、没有校验文件类型、可能有 XSS 攻击... <font color=lightSeaGreen>这些问题如果我不问，它是不会主动告诉我的</font>。

从这个角度看，**会提问这个能力在 Agent 时代真的是一个巨大的杠杆。** 你不需要是某个领域的专家，几个简单的反向问题，就能让你拿到远超你自身认知水平的洞察。

Kahneman 还讲过一个概念叫 **WYSIATI——What You See Is All There Is**，意思是<font color=red>你的大脑会根据手头有限的信息编出一个"最好的故事"，但完全不会去想那些缺失的信息</font>。

Agent 也一样。它给你的输出看起来"完整"，但你根本不知道它遗漏了什么。它不会主动跟你说"我其实还有些东西没考虑到"。

所以，养成一个习惯：每次拿到 Agent 的关键产出，追问一句——"**你漏掉了什么？**"。

##### 避免 Agent 的过度设计

如果你经常 review Agent 的代码，你大概率会发现这些情况：

- 明明一个现成的库几行代码就能搞定的事，它偏要从零造轮子，洋洋洒洒写了几百行
    
- 同样的逻辑，它在不同地方重复写了好几遍，也不知道抽个公共函数
    
- 为了一些根本不可能出现的边界情况，写了一大堆防御代码
    
- 一个一次性操作，它也要给你封装成一个工具类，加配置项、加扩展点
    

不要奇怪，AI Agent 天生就有这个毛病。

Brooks 在《人月神话》里讲过一个概念叫**第二系统效应**——<font color=lightSeaGreen>**设计者在做第二个系统的时候，会把上次忍住没加的功能全塞进去，导致过度设计**</font>。Agent 更极端，因为代码生成的边际成本为零，加功能、堆代码量对 Agent 来说太容易了。你让它加功能，它绝不会跟你说"这个不该做"。

这也是 AI Coding 最大的坑之一。

功能以前所未有的速度堆积，维护负担指数级增长。只凭感觉编程的人，短期可能能撸出来一个像样的产品，但打开代码仓库一看，一大堆技术债，大量的垃圾代码，维护起来效率极低。

###### 那怎么来约束呢？

**第一，在 prompt 里明确要求。** 简单粗暴但有效：

```markdown
最小化实现，不要过度设计，只做我要求的改动
```

或者在 CLAUDE.md 里写上规则：

```markdown
Avoid over-engineering. Only make changes that are directly requested or clearly necessary.  
``` 

**第二，做完一个改动之后，让 Agent 自己清理。** 这个技巧我最近用得很多：

```markdown
你觉得现在哪些代码是 over-engineering 的？给我清理和优化一下
```

Agent 其实很擅长做"减法"，前提是你要明确告诉它去做。

Agent 过度设计，本质上不是 Agent 的错，是你没有给它足够的约束。

##### 找到你系统的瓶颈

最后一个点，也是我认为最重要的一个。

Goldratt 在《目标》这本书里讲了一个特别核心的思想：

> 每个复杂系统都由多个相互关联的活动组成，其中一个活动对整个系统构成约束——就是"链条中最薄弱的环节"。

换句话说：

- 整个系统的产出，只能在约束环节得到改善时才能提升
    
- 花时间优化非约束环节，不会带来什么显著收益
    
- 链条的强度由最弱的环节决定
    

你现在想一想，你日常用 Agent 工作的时候，整个工作流的瓶颈在哪？

| 环节        | 有没有可能是瓶颈？ | 怎么判断                     |
| --------- | --------- | ------------------------ |
| 任务分解      | 有可能       | 你是不是花大量时间在想怎么拆任务？        |
| Prompt 编写 | 有可能       | 是不是每次都从零开始写 prompt？      |
| Agent 执行  | 一般不是      | Agent 通常秒级/分钟级就完成了       |
| **人类审查**  | **最常见**   | **Agent 的产出是不是在排队等你审查？** |
| 反馈迭代      | 有可能       | 是不是花太多时间在来回修改上？          |

说实话，在大多数人的工作流中，**人类审查就是那个瓶颈**。Agent 几分钟就能产出大量代码，但你的审查速度是有限的。

但我接下来要说的，不是让你提升自己审查的速度。

我真正想说的是：<font color=red>**当你的 review 已经成为整个流程的瓶颈时，再去唤起更多的 Agent 其实没有任何意义。**</font>

Goldratt 在《目标》里讲了一个特别好的例子。Alex 的工厂花大价钱装了机器人，某些部门的生产率提高了 36%。听起来很厉害对吧？但实际上工厂一分钱都没多赚。为什么？因为机器人装在了非瓶颈环节。非瓶颈环节的效率提升，只是增加了库存，但最终出货量一点没变。

这就是经典的**局部最优但全局恶化**。

映射到 Agent 管理上：你同时开了 5 个 Claude Code 会话并行处理任务，Agent 产出了一大堆代码，但你审查不过来。API 费用上去了，待审代码在那堆着，但最终交付的高质量产出没有增加，而且 Agent 生成的代码之间可能还会冲突。

你的精力有限。当你只能一次跟进两个并行任务的时候，给你 20 个任务不会提升你的效率，只会让你焦头烂额。

那怎么办？**围绕瓶颈来优化**。

如果你的瓶颈是人的审查这一步，那就：

- 不要让 Agent 产出超过你能审查的量——宁可 Agent 等你，不要你追不上 Agent
    
- 用自动化测试减少人工审查的负担（CI/CD、lint、typecheck 这些配一次，永久生效）
    
- 提升 prompt 质量，让 Agent 一次性输出更可靠的代码，减少返工
    
- 高 TRM 任务直接跳过审查，中 TRM 任务只审关键节点，把精力留给低 TRM 任务
    

如果你的瓶颈是 prompt 编写——每次都从零开始写 prompt，效率当然低。那就把常用的 prompt 封装成 Skill 或者模板，一次投入，反复使用。

如果你的瓶颈是任务分解——你花大量时间在想怎么拆任务。那就让 Agent 先帮你拆，你来审核和调整，利用 Plan Mode 做这件事。

**总而言之，找到瓶颈之后，尽可能优化瓶颈本身。你去优化瓶颈以外的因素，没有任何收益。**

##### 写在最后

真正的瓶颈早就不是模型，那些天天说模型能力不行的、动不动降智的，其实有很多时候是自己成为了整个工作流的瓶颈，比如 prompt 上下文没有控制好、妄想一个 prompt 能完成一个复杂任务、最开始的方案路线就有问题等等。

当自己本身成为瓶颈，模型再怎么优化都只是杯水车薪。

Agent 加速了编码——但编码从来就不是瓶颈。设计、判断、审查、决策，这些才是真正难的部分。

如果你的 Agent 工作流一直不太顺，先别急着换工具、换模型。先检查你自己的系统和流程——prompt 设计合不合理？任务拆分清不清晰？瓶颈在哪？

管理 Agent 的最高境界，不是让 Agent 做更多的事，而是让 Agent 做**正确的事**，在**正确的时机**得到**正确的干预**。