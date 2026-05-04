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



#### 《MCP 真的会被 Skills + CLI 干掉吗？聊聊我的观察》笔记

Skills + CLI 能干掉 MCP 的前提是什么？是你的 Agent 跑在本地。

本地环境有文件系统、有 Shell、有包管理器。模型读一个 SKILL.md 知道该怎么做，然后直接 Bash 调 ffmpeg、kubectl、git，不需要中间加一层协议。这条路确实比 MCP 更直接。

但现实是，<font color=red>**越来越多的 Agent 跑在云上**</font>。

Web 应用里嵌的 Agent、移动端 Agent、API 服务型 Agent——这些场景下没有本地文件系统，没有 Shell 可以调。你能让一个跑在浏览器里的 Agent 去执行 CLI 命令吗？一般情况下是做不到的。

而<font color=dodgerBlue>这些 Agent 需要连接的是什么？</font> <font color=red>数据库、内部系统、数据分析平台、Stripe 平台等等</font>。<font color=red>全都在云上，**全都需要认证，全都需要标准化的远程协议**</font>。

**MCP 就是干这个的。**

##### MCP 不是在萎缩，是在加速

Anthropic 官方的数据显示：MCP SDK 月下载量已经突破 3 亿，年初还只有 1 亿，4 个月涨了 3 倍。Anthropic 目录里有 200 多个 MCP Server，每天有几百万人在用。

<font color=dodgerBlue>**而且之前大家吐槽 MCP 最多的 Token 占用问题，也在被快速解决**</font>：

- **Tool Search** ：工具多了不全塞上下文，按需搜索加载，Token 占用降了 85%。
- **Code Orchestration** ：Cloudflare 2500 个 API 端点只暴露 2 个工具（搜索 + 执行），Agent 自己写代码调用，整套工具定义只占 1000 token。

说了这么多，我真正想表达的是，MCP 的工程问题不是无解的，社区在持续优化，而且速度很快。

##### MCP 正在长出 CLI 给不了的能力

<font color=dodgerBlue>除了修复已有的问题，**MCP 还在长出一些全新的能力**</font>，这些能力是 Skills + CLI 从架构上就做不到的。

- **MCP Apps** ：<font color=red>**工具可以返回交互式 UI**。图表、表单、仪表盘，直接在对话里渲染出来</font>。你让 Agent 查一个数据，它不是给你吐一堆文字，而是直接画一张图表出来。<font color=red>CLI 能返回的只有文本，这种富交互体验它给不了</font>。<font color=lightSeaGreen>Anthropic 说，接入了 MCP Apps 的 Server，用户留存率明显更高</font>。

- **Elicitation** ：<font color=red>工具执行到一半可以暂停，主动向用户要信息</font>。比如 Agent 在帮你订酒店，执行到付款环节，MCP Server 可以弹出一个表单让你确认价格和房型，或者直接跳转到支付页面完成认证。这种 “中途交互” 的能力，CLI 的同步执行模式是做不到的。

- **OAuth 和 Vault** ：MCP 最新的协议版本标准化了 OAuth 客户端注册（CIMD），<font color=red>用户首次授权更快，重复授权更少</font>。Managed Agents 里还加了 Vault 机制，注册一次 OAuth token，后面所有 Session 自动注入和刷新。这整套认证体系，CLI 世界里完全没有对应物——CLI 的认证靠的是磁盘上的 credential 文件，换个环境就废掉了。

你看，MCP 不是在原地等着被取代，它在往"富交互"和"企业级安全"的方向进化。这些方向上，CLI 是打不过 MCP 的。

##### 不同场景，不同选择

不同的场景对应的选择不同：

- **本地 Agent**（Claude Code、OpenClaw 这种）：Skills + CLI 确实是更优解。有完整的文件系统和 Shell，不需要加一层协议。这个场景下 CLI 的简洁性是实打实的优势。

- **云端 Agent**（Web 应用、API 服务、移动端）：Skills + MCP 是最优解。<font color=red>没有本地环境，**必须走远程协议**</font>。而且云端 Agent 通常需要对接多个第三方服务，MCP 的标准化在这里价值巨大——一个 Server 写好，Claude、CodeX、Cursor 全都能用。

摘自：[MCP 真的会被 Skills + CLI 干掉吗？聊聊我的观察](https://mp.weixin.qq.com/s/krmpMWJ9eeZ0_Yj55UZBtQ)



### Skills

###### 参考资料

- [值得安装的 7 个 Skills，让你的 AI 助手脱胎换骨](https://mp.weixin.qq.com/s/ASlpgAXeIkvj83aY77E5rw)



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
- [你不知道的 Claude Code：架构、治理与工程实践](https://tw93.fun/2026-03-12/claude.html)

###### Cheat Sheet

<img src="https://files.seeusercontent.com/2026/03/29/3oEo/8u2gs9i7d8rg1.jpeg" alt="r/vibecoders_ - Useful Claude Code Cheat sheet!" style="zoom:55%;" />

该内容随 Claude Code 更新而更新，查看最新的内容可查看 🔗：[Claude Code Cheat Sheet](https://cc.storyfox.cz/)



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
- 复制图像并使用 cmd + v 粘贴到 CLI 中
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



#### 《你不知道的 AI Coding：非技术人的上手、场景与实战》笔记

Claude Code 还有个 **Routines** 功能，能把一段工作流存到云端，按定时、Webhook 或 GitHub 事件自动触发；感兴趣可以看[官方文档](https://code.claude.com/docs/en/routines)。

##### CLAUDE.md：先把项目背景写清楚

<font color=dodgerBlue>写得好不好，三件事最关键</font>。<font color=red>写得短一点，150 行以内为佳，写太长会挤压后续对话的空间</font>。语气直接，用命令式，别写 “我们团队比较喜欢” 这种软话，“所有注释用中文” 比 “团队偏好中文注释” 有效太多。每条都能判断，“代码质量要高” 没用，“函数超过 50 行必须拆分” 才能落地。

> [!NOTE]
>
> 这里有点类似于 [[#amp prompt 指南]] 中的第一条

<font color=dodgerBlue>四条最值钱的规则，直接拿去用</font>：先问清楚再动手、简单优先、只动该动的、做完要验证。展开就是：别让它猜你的意图，目标说清再写；能两行解决的不写两百行，拒绝过度设计；不要顺手重构没让它改的代码；跑通构建和测试才算完，没通过别说完成。

<font color=dodgerBlue>**下面这份模板，你改一改项目背景就能直接用：**</font>

```md
# 项目背景
这是一个面向运营同学的客户看板，技术栈 Node.js + Next.js，
前端用 React，数据库 PostgreSQL，部署在 Vercel。
产品经理是 Alice，设计是 Bob，后端是我自己。

# 工作规范
- 所有注释用中文，变量函数用英文。
- 改动前先说明你打算改什么，确认后再动手。
- 新功能先写实现，不主动加测试，除非我明确要求。
- 数据库表名用下划线分隔，比如 user_profile。

# 禁止项
- 不要主动重构我没提到的文件。
- 不要删除任何文件，除非我明确说删掉。
- 不要在没确认前直接执行 npm install 装新依赖。

# 压缩时保留
长对话被自动压缩时，按优先级保留：
1. 架构决策和它背后的理由
2. 改过哪些文件、改了什么
3. 当前进展状态
4. 还没做完的 TODO
```

<font color=red>**最后这段 ”压缩时保留” 看着不起眼，长会话能不能稳就靠它**</font>。Claude Code 的上下文用到一定程度会自动压缩，决策的理由通常是第一个被丢的。比如你之前说过 “这里要用 POST 不用 GET，因为数据量大”，压缩之后可能只剩 “用 POST” 三个字，理由没了。下次再问相关问题，它可能给你一个完全不同的方案，前后矛盾。把这一段写进去，长会话就不会前后打架。

上面这些不一定要自己从头写。装好 Claude Code 之后，<font color=red>直接说 “读一下我这个项目，帮我生成一份 CLAUDE.md” ，它会扫一遍代码、技术栈、目录结构，**给你一份草稿**，你只要改一改人名和团队偏好</font>。装依赖、配 alias、改 `~/.claude/settings.json` 这些事也一样，告诉它要什么效果让它自己去试，比你查文档快得多。配置类的活能交就交，省下来的精力放到真正要判断的事情上。

##### 需求描述越精确，它越少分叉跑偏

> [!NOTE]
>
> 这里作者举例并分析 yetone 的 [voice-input-src - README_CN.md](https://github.com/yetone/voice-input-src/blob/master/README_CN.md) ，这里就不摘抄了

业务场景的需求，光描述功能还不够。开头先把问题写清楚，要解决什么、给谁用、怎么算做对了，别一上来就列功能清单。比如说我们要写一个国际门票频道页时，第一句话就是 “国际门票目前没有独立入口，用户只能搜索找到，非热门城市曝光极低” ，这两句话决定了它后面碰到 “热门城市展示几个”、“筛选要不要做 ‘最近浏览’ ” 这类问题时的判断方向。

接下来要给它划范围。Claude Code 很积极，你说做一个列表页它顺手就给你加上收藏、分享、埋点。明确写出 “不做登录态、不做分享、不做 SEO，下一期再说” ，它就不会越界。异常情况要单独列出来，接口超时怎么办、数据为空展示什么、图片挂了用什么兜底，这些不写它要么不处理，要么猜个你不满意的方案。

<font color=red>**验收标准必须给数字**</font>，”页面要快” 没用，”首屏 1.5 秒内” 才能判断；”布局正常” 没用，”在 375 和 1440 两个宽度下不错位” 才能验收。

写需求的时候，别用 ”待定”、“后续再看”、“TBD”。Claude Code 碰到这些会自己猜着填，猜的往往不是你要的。哪怕写 “这一版先硬编码，下版再做配置化”，也比空着强。

##### 你也可以自己写一个 Skill

Claude Code 启动时只读 frontmatter，也就是描述触发条件的约 100 个字，真正调用时才加载完整内容，所以你装几十个 Skill 启动也不会变慢。



##### 长会话怎么办

Claude Code 的上下文容量是固定的，跑久了早期内容会被挤出去。

![img](https://files.seeusercontent.com/2026/04/30/eyN5/image.png)

**长任务结束前让它写交接笔记**，直接对它说：”把当前进度写成一份 `HANDOFF.md`，包括做了什么、试过什么没成功、下一步该做什么。” 第二天打开新会话，把这个文件给它，就能接着干，不依赖任何压缩算法。

##### 用熟之后的几个小习惯

**截图比文字快**，要描述一个界面问题或者想参考某个设计风格，直接丢图比写一段话准多了，布局、颜色、层级都带进来了，让它少猜。

<font color=dodgerBlue>**Memory 跨项目记住你的偏好**</font>，`CLAUDE.md` 是项目级的每个项目都得单独写一份，<font color=red>**Memory 是用户级的跨所有项目和会话都生效**</font>。<font color=lightSeaGreen>直接对它说 ”***记住*** 我喜欢先看方案再执行”、”***记住*** 回我中文”</font>，<font color=red>它会写进 `~/.claude/memory/`</font>，以后任何项目打开都记得。常交代的背景信息都可以沉淀进去，省得每次重复说。



### 官方文档笔记



> [!IMPORTANT]
>
> 官方笔记变化很快，经常一个文档刷新之后，会发现刷新后变动不少；所以，这里的记录仅供参考，还是要以最新官方文档为准



#### 概述

##### 你可以做什么

以下是你可以使用 Claude Code 的一些方式：

###### 自动化你一直在推迟的工作

Claude Code 处理那些占用你一整天的繁琐任务：为未测试的代码编写测试、修复项目中的 lint 错误、解决合并冲突、更新依赖项和编写发布说明。

```sh
claude "write tests for the auth module, run them, and fix any failures"
```

> [!NOTE]
>
> 这里的 prompt ，是相当简洁而抽象的；没有指定路径，同时用简洁的语言让 Claude 去写测试，并修复。有种炫技的感觉？不过，作为一个 TUI 应用，相较于 GUI 没有那么方便，鼓励并能处理简洁的 prompt 也是合理的。

###### 构建功能和修复错误

用简单的语言描述你想要的内容。Claude Code 规划方法、跨多个文件编写代码，并验证其工作。

对于错误，粘贴错误消息或描述症状。Claude Code 通过你的代码库追踪问题、识别根本原因并实施修复。查看[常见工作流](https://code.claude.com/docs/zh-CN/common-workflows)了解更多示例。

###### 创建提交和拉取请求

Claude Code 直接与 git 配合工作。它暂存更改、编写提交消息、创建分支并打开拉取请求。

```sh
claude "commit my changes with a descriptive message"
```

在 CI 中，你可以使用 [GitHub Actions](https://code.claude.com/docs/zh-CN/github-actions) 或 [GitLab CI/CD](https://code.claude.com/docs/zh-CN/gitlab-ci-cd) 自动化代码审查和问题分类。

###### 使用 MCP 连接你的工具

[Model Context Protocol (MCP)](https://code.claude.com/docs/zh-CN/mcp) 是一个开放标准，用于将 AI 工具连接到外部数据源。使用 MCP，Claude Code 可以读取 Google Drive 中的设计文档、更新 Jira 中的工单、从 Slack 拉取数据，或使用你自己的自定义工具。

###### 使用说明、skills 和 hooks 进行自定义

[`CLAUDE.md`](https://code.claude.com/docs/zh-CN/memory) 是一个 markdown 文件，你可以将其添加到项目根目录，<font color=red>Claude Code 会在每个会话开始时读取它</font>。使用它来<font color=red>**设置编码标准、架构决策、首选库和审查清单**</font>。Claude 还会在工作时构建[自动内存](https://code.claude.com/docs/zh-CN/memory#auto-memory)，保存学习内容，如构建命令和调试见解，跨会话使用，无需你编写任何内容。

创建[自定义命令](https://code.claude.com/docs/zh-CN/skills)来打包你的团队可以共享的可重复工作流，如 `/review-pr` 或 `/deploy-staging`。

<font color=red>[Hooks](https://code.claude.com/docs/zh-CN/hooks) 让你在 Claude Code 操作之前或之后运行 shell 命令</font>，如在每次文件编辑后自动格式化或在提交前运行 lint。

###### 运行代理团队并构建自定义代理

<font color=red>**生成[多个 Claude Code 代理](https://code.claude.com/docs/zh-CN/sub-agents)，同时处理任务的不同部分**</font>。主导代理协调工作、分配子任务并合并结果。

对于完全自定义的工作流，[Agent SDK](https://platform.claude.com/docs/en/agent-sdk/overview) 让你构建由 Claude Code 的工具和功能驱动的自己的代理，完全控制编排、工具访问和权限。

###### 使用 CLI 进行管道、脚本和自动化

Claude Code 是可组合的，遵循 Unix 哲学。将日志管道传入其中、在 CI 中运行它，或将其与其他工具链接：

```sh
# 分析最近的日志输出
tail -200 app.log | claude -p "Slack me if you see any anomalies"

# 在 CI 中自动化翻译
claude -p "translate new strings into French and raise a PR for review"

# 跨文件的批量操作
git diff main --name-only | claude -p "review these changed files for security issues"
```

查看 [CLI 参考](https://code.claude.com/docs/zh-CN/cli-reference)了解完整的命令和标志集。

###### 安排定期任务

<font color=red>按计划运行 Claude 以自动化重复的工作：早晨 PR 审查、夜间 CI 失败分析、每周依赖项审计或在 PR 合并后同步文档</font>。

- [云计划任务](https://code.claude.com/docs/zh-CN/web-scheduled-tasks)在 Anthropic 管理的基础设施上运行，因此即使你的计算机关闭，它们也会继续运行。从网络、桌面应用或<font color=lightSeaGreen>通过在 CLI 中运行 `/schedule` 来创建它们</font>。
- [桌面计划任务](https://code.claude.com/docs/zh-CN/desktop#schedule-recurring-tasks)在你的机器上运行，可直接访问你的本地文件和工具
- [`/loop`](https://code.claude.com/docs/zh-CN/scheduled-tasks) 在 CLI 会话中重复提示以进行快速轮询

###### 从任何地方工作

会话不受限于单一界面。当你的上下文改变时，在环境之间移动工作：

- 离开你的办公桌，使用[远程控制](https://code.claude.com/docs/zh-CN/remote-control)从你的手机或任何浏览器继续工作
- <font color=red>**向 [Dispatch](https://code.claude.com/docs/zh-CN/desktop#sessions-from-dispatch) 发送来自你手机的任务，并打开它创建的桌面会话**</font>
- 在[网络](https://code.claude.com/docs/zh-CN/claude-code-on-the-web)或 [iOS 应用](https://apps.apple.com/app/claude-by-anthropic/id6473753684)上启动长时间运行的任务，然后使用 `/teleport` 将其拉入你的终端
- 使用 `/desktop` 将终端会话交给[桌面应用](https://code.claude.com/docs/zh-CN/desktop)进行视觉差异审查
- 从团队聊天路由任务：在 [Slack](https://code.claude.com/docs/zh-CN/slack) 中提及 `@Claude` 并附上错误报告，获得拉取请求



#### 快速开始

##### 在 Claude Code 中使用 Git

Claude Code 使 Git 操作变得对话式：

```md
我更改了哪些文件？
```

```md
用描述性消息提交我的更改
```

您也可以提示更复杂的 Git 操作：

```md
创建一个名为 feature/quickstart 的新分支
```

```md
显示我最后的 5 次提交
```

```md
帮我解决合并冲突
```

##### 修复错误或添加功能

Claude 擅长调试和功能实现。用自然语言描述您想要的内容：

```md
向用户注册表单添加输入验证
```

或修复现有问题：

```md
有一个错误，用户可以提交空表单 - 修复它
```

Claude Code 将：

- 定位相关代码
- 理解上下文
- 实现解决方案
- 如果可用，运行测试

> [!TIP]
>
> 像与有帮助的同事交谈一样与 Claude 交谈。描述您想要实现的目标，它将帮助您实现。

#####  初学者专业提示

有关更多信息，请参阅[最佳实践](https://code.claude.com/docs/zh-CN/best-practices)和[常见工作流](https://code.claude.com/docs/zh-CN/common-workflows)。

###### 对您的请求要具体

不要说：‘修复错误’尝试：‘修复登录错误，用户输入错误凭证后看到空白屏幕’

###### 使用分步说明

将复杂任务分解为步骤：

```md
1. 为用户配置文件创建新的数据库表
2. 创建 API 端点以获取和更新用户配置文件
3. 构建允许用户查看和编辑其信息的网页
```

###### 让 Claude 先探索

在进行更改之前，让 Claude 理解您的代码：

```md
分析数据库架构
```

```md
构建一个仪表板，显示英国客户最常退货的产品
```

###### 使用快捷方式节省时间

- 按 `?` 查看所有可用的快捷键
- 使用 Tab 进行命令补全
- 按 ↑ 查看命令历史
- 输入 `/` 查看所有命令和 skills

#### Claude Code 如何工作

##### 代理循环

<font color=dodgerBlue>**当您给 Claude 一个任务时，它会经历三个阶段**</font>：<font color=fuchsia>**收集上下文**、**采取行动**和**验证结果**</font>。这些阶段相互融合。Claude 始终使用工具，无论是搜索文件以了解您的代码、编辑以进行更改，还是运行测试以检查其工作。

![代理循环：您的提示导致 Claude 收集上下文、采取行动、验证结果，并重复直到任务完成。您可以在任何时刻中断。](https://files.seeusercontent.com/2026/04/15/n1hR/agentic-loop.svg)

循环会根据您的要求进行调整。关于您代码库的问题可能只需要收集上下文。<font color=fuchsia>错误修复会循环通过所有三个阶段**多次**</font>。重构可能涉及广泛的验证。Claude 根据从前一步学到的内容决定每一步需要什么，将数十个操作链接在一起并沿途进行纠正。

<font color=fuchsia>**您也是这个循环的一部分**。您可以在任何时刻中断以引导 Claude 朝不同的方向发展、提供额外的上下文或要求它尝试不同的方法</font>。Claude 自主工作但对您的输入保持响应。

<font color=dodgerBlue>代理循环由两个组件驱动</font>：[模型](https://code.claude.com/docs/zh-CN/how-claude-code-works#models)进行推理和[工具](https://code.claude.com/docs/zh-CN/how-claude-code-works#tools)采取行动。<font color=red>Claude Code 充当 Claude 周围的**代理框架**：它提供工具、上下文管理和执行环境，将语言模型转变为能够进行编码的代理</font>。

###### 工具

工具是使 Claude Code 成为代理的原因。没有工具，Claude 只能用文本回应。有了工具，Claude 可以采取行动：读取您的代码、编辑文件、运行命令、搜索网络并与外部服务交互。每个工具使用都会返回信息，反馈到循环中，告知 Claude 的下一个决定。

<font color=dodgerBlue>内置工具通常分为五个类别，每个类别代表不同类型的代理能力</font>。

| 类别         | Claude 可以做什么                                            |
| :----------- | :----------------------------------------------------------- |
| **文件操作** | 读取文件、编辑代码、创建新文件、重命名和重新组织             |
| **搜索**     | 按模式查找文件、使用正则表达式搜索内容、探索代码库           |
| **执行**     | 运行 shell 命令、启动服务器、运行测试、使用 git              |
| **网络**     | 搜索网络、获取文档、查找错误消息                             |
| **代码智能** | 编辑后查看类型错误和警告、跳转到定义、查找引用（需要[代码智能插件](https://code.claude.com/docs/zh-CN/discover-plugins#code-intelligence)） |

这些是主要功能。Claude 还有用于生成 subagents、询问您问题和其他编排任务的工具。

Claude 根据您的提示和沿途学到的内容选择使用哪些工具。当您说”修复失败的测试”时，Claude 可能会：

1. 运行测试套件以查看失败的内容
2. 读取错误输出
3. 搜索相关的源文件
4. 读取这些文件以理解代码
5. 编辑文件以修复问题
6. 再次运行测试以验证

每个工具使用都给 Claude 新的信息，告知下一步。这就是代理循环的实际应用。

**扩展基本功能：** 内置工具是基础。您可以<font color=red>使用 **[skills](https://code.claude.com/docs/zh-CN/skills) 扩展 Claude 知道的内容**、使用 **[MCP](https://code.claude.com/docs/zh-CN/mcp) 连接到外部服务**、使用 [hooks](https://code.claude.com/docs/zh-CN/hooks) 自动化工作流，以及将任务卸载给 [subagents](https://code.claude.com/docs/zh-CN/sub-agents)</font>。这些扩展形成了核心代理循环之上的一层。

###### Claude 可以访问什么

本指南重点关注终端。当您在目录中运行 `claude` 时，Claude Code 可以访问：

- **您的项目。** 您目录和子目录中的文件，以及其他地方有您许可的文件。
- **您的终端。** <font color=red>您可以运行的任何命令：构建工具、git、包管理器、系统实用程序、脚本</font>。<font color=fuchsia>如果您可以从命令行做到，Claude 也可以</font>。
- **您的 git 状态。** 当前分支、未提交的更改和最近的提交历史。
- **您的 [CLAUDE.md](https://code.claude.com/docs/zh-CN/memory)。** 一个 markdown 文件，您可以在其中存储项目特定的说明、约定和 Claude 应该在每个会话中了解的上下文。
- **[自动内存](https://code.claude.com/docs/zh-CN/memory#auto-memory)。** Claude 在您工作时自动保存的学习内容，如项目模式和您的偏好。<font color=fuchsia>MEMORY.md 的前 200 行或 25KB（以先到者为准）在每个会话开始时加载</font>。
- **您配置的扩展。** 用于外部服务的 [MCP servers](https://code.claude.com/docs/zh-CN/mcp)、<font color=fuchsia>**用于工作流的 [skills](https://code.claude.com/docs/zh-CN/skills)**</font>、用于委派工作的 [subagents](https://code.claude.com/docs/zh-CN/sub-agents) 和用于浏览器交互的 [Claude in Chrome](https://code.claude.com/docs/zh-CN/chrome)。

因为 Claude 看到您的整个项目，它可以跨越它工作。当您要求 Claude “修复身份验证错误” 时，它搜索相关文件、读取多个文件以理解上下文、跨它们进行协调编辑、运行测试以验证修复，并在您要求时提交更改。这与只看到当前文件的内联代码助手不同。

##### 使用会话

Claude Code 在您工作时将您的对话保存在本地。每条消息、工具使用和结果都被存储，<font color=red>这使得[回退](https://code.claude.com/docs/zh-CN/how-claude-code-works#undo-changes-with-checkpoints)、[恢复和分叉](https://code.claude.com/docs/zh-CN/how-claude-code-works#resume-or-fork-sessions)会话成为可能</font>。在 Claude 进行代码更改之前，它还会对受影响的文件进行快照，以便您在需要时可以恢复。

**会话是独立的。** 每个新会话都以新的上下文窗口开始，没有来自以前会话的对话历史。<font color=fuchsia>Claude 可以使用 [自动内存](https://code.claude.com/docs/zh-CN/memory#auto-memory) 跨会话保持学习</font>，您可以在 [CLAUDE.md](https://code.claude.com/docs/zh-CN/memory) 中添加您自己的持久说明。

###### 跨分支工作

每个 Claude Code 对话都是一个与您当前目录相关的会话。当您恢复时，您只会看到来自该目录的会话。

Claude 看到您当前分支的文件。<font color=dodgerBlue>当您切换分支时</font>，Claude 看到新分支的文件，但您的对话历史保持不变。<font color=red>**Claude 记得您讨论过的内容，即使在切换后也是如此**</font>。

由于会话与目录相关，您可以通过使用 [git worktrees](https://code.claude.com/docs/zh-CN/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees) 运行并行 Claude 会话，这为各个分支创建单独的目录。

###### 恢复或分叉会话

当您使用 `claude --continue` 或 `claude --resume` 恢复会话时，您使用相同的会话 ID 从中断处继续。新消息附加到现有对话。<font color=lightSeaGreen>您的完整对话历史被恢复</font>，但<font color=red>会话范围的权限不会</font>。您<font color=red>需要重新批准这些</font>。

![会话连续性：恢复继续相同的会话，分叉创建一个具有新 ID 的新分支。](https://files.seeusercontent.com/2026/04/16/5Xwz/session-continuity.svg)

<font color=red>**要分支并尝试不同的方法而不影响原始会话**，请使用 `--fork-session` 标志</font>：

```sh
claude --continue --fork-session
```

这会创建一个新的会话 ID，同时保留到该点的对话历史。原始会话保持不变。与恢复一样，分叉的会话不继承会话范围的权限。

**在多个终端中的相同会话**：如果您在多个终端中恢复相同的会话，两个终端都会写入相同的会话文件。来自两者的消息会交错，就像两个人在同一个笔记本中写字一样。没有任何内容损坏，但对话变得混乱。每个终端在会话期间只看到自己的消息，但如果您稍后恢复该会话，您会看到所有内容交错。对于从相同起点的并行工作，使用 `--fork-session` 为每个终端提供自己的干净会话。

###### 上下文窗口

Claude 的上下文窗口保存您的对话历史、文件内容、命令输出、[CLAUDE.md](https://code.claude.com/docs/zh-CN/memory)、[自动内存](https://code.claude.com/docs/zh-CN/memory#auto-memory)、加载的 skills 和系统说明。当您工作时，上下文填满。Claude 自动压缩，但对话早期的说明可能会丢失。将持久规则放在 CLAUDE.md 中，并 <font color=fuchsia>**运行 `/context` 以查看什么在占用空间**</font>。

- **当上下文填满时**

  Claude Code 在您接近限制时自动管理上下文。它首先清除较旧的工具输出，然后在需要时总结对话。您的请求和关键代码片段被保留；对话早期的详细说明可能会丢失。将持久规则放在 CLAUDE.md 中，而不是依赖对话历史。

  要控制在压缩期间保留的内容，请在 CLAUDE.md 中添加 “Compact Instructions” 部分 <font color=dodgerBlue>**或**</font> <font color=<font color=red>>**使用焦点运行 `/compact`（如 `/compact focus on the API changes`）**</font>。

  运行 `/context` 以查看什么在占用空间。MCP 工具定义默认被延迟，并通过[工具搜索](https://code.claude.com/docs/zh-CN/mcp#scale-with-mcp-tool-search)按需加载，因此只有工具名称消耗上下文，直到 Claude 使用特定工具。运行 `/mcp` 以检查每个服务器的成本。 

- **使用 skills 和 subagents 管理上下文**

  除了压缩，您可以使用其他功能来控制什么加载到上下文中。

  [Skills](https://code.claude.com/docs/zh-CN/skills) 按需加载。Claude 在会话开始时看到 skill 描述，但完整内容仅在使用 skill 时加载。<font color=lightSeaGreen>对于您手动调用的 skills，设置 `disable-model-invocation: true` 以将描述保留在上下文之外，直到您需要它们</font>。

  [Subagents](https://code.claude.com/docs/zh-CN/sub-agents) 获得自己的新上下文，完全独立于您的主对话。他们的工作不会使您的上下文膨胀。完成后，他们返回一个摘要。这种隔离是为什么 subagents 有助于长会话。

##### 使用检查点和权限保持安全

Claude 有两个安全机制：检查点让您撤销文件更改，权限控制 Claude 可以在不询问的情况下做什么。

###### 使用检查点撤销更改

**每个文件编辑都是可逆的。** <font color=red>**在 Claude 编辑任何文件之前，它会对当前内容进行快照**</font>。<font color=dodgerBlue>如果出现问题</font>，<font color=red>按两次 `Esc` 以回退到之前的状态，或要求 Claude 撤销</font>。

检查点是会话本地的，独立于 git。它们仅涵盖文件更改。影响远程系统的操作（数据库、API、部署）无法进行检查点，这就是为什么 Claude 在运行具有外部副作用的命令之前询问。

###### 控制 Claude 可以做什么

按 `Shift+Tab` 循环通过权限模式：

- **默认**：Claude 在文件编辑和 shell 命令之前询问

- **自动接受编辑**：Claude edits files and runs <font color=red>common filesystem commands</font> like `mkdir` and `mv` without asking, still asks for other commands

  > [!NOTE]
  >
  > 中文版翻译过差看不懂，这里放了英文版

- **Plan Mode**：Claude 仅使用只读工具，创建您可以在执行前批准的计划

- **自动模式**：Claude 使用后台安全检查评估所有操作。目前是研究预览

您也可以在 `.claude/settings.json` 中允许特定命令，以便 Claude 不会每次都询问。这对于受信任的命令（如 `npm test` 或 `git status`）很有用。设置可以从组织范围的策略范围到个人偏好。有关详细信息，请参阅[权限](https://code.claude.com/docs/zh-CN/permissions)。

##### 有效使用 Claude Code

这些提示可以帮助您从 Claude Code 获得更好的结果。

###### 向 Claude Code 寻求帮助

Claude Code 可以教您如何使用它。提出问题，如 我如何设置 hooks？“或”构建我的 CLAUDE.md 的最佳方式是什么？“，Claude 会解释。内置命令也会指导您完成设置：

- `/init` 引导您为项目创建 CLAUDE.md
- `/agents` 帮助您配置自定义 subagents
- `/doctor` 诊断您的安装的常见问题

###### 预先具体

您的初始提示越精确，您需要的更正就越少。参考特定文件、提及约束并指出示例模式。

```md
结账流程对于持有过期卡的用户来说已损坏。
检查 src/payments/ 中的问题，特别是令牌刷新。
首先编写一个失败的测试，然后修复它。
```

模糊的提示有效，但您会花更多时间引导。像上面这样的具体提示通常在第一次尝试时就成功。

###### 给 Claude 一些东西来验证

Claude 在能够检查自己的工作时表现更好。包括测试用例、粘贴预期 UI 的屏幕截图或定义您想要的输出。

```md
实现 validateEmail。测试用例：'user@example.com' → true，
'invalid' → false，'user@.com' → false。之后运行测试。
```

对于视觉工作，粘贴设计的屏幕截图并要求 Claude 将其实现与其进行比较。

###### 在实现之前探索

对于复杂的问题，将研究与编码分开。使用 plan mode（按 `Shift+Tab` 两次）首先分析代码库：

```md
读取 src/auth/ 并理解我们如何处理会话。
然后为添加 OAuth 支持创建一个计划。
```

审查计划，通过对话细化它，然后让 Claude 实现。这种两阶段方法比直接跳到代码产生更好的结果。

###### 委派，不要指示

想象委派给一个有能力的同事。提供上下文和方向，然后相信 Claude 会弄清楚细节：

```md
结账流程对于持有过期卡的用户来说已损坏。
相关代码在 src/payments/ 中。您可以调查并修复它吗？
```

您不需要指定要读取哪些文件或运行什么命令。Claude 会弄清楚。



#### 扩展 Claude Code

##### 概述

扩展插入代理循环的不同部分：

- **[CLAUDE.md](https://code.claude.com/docs/zh-CN/memory)** 添加 Claude 每个会话都能看到的持久上下文
- **[Skills](https://code.claude.com/docs/zh-CN/skills)** <font color=red>添加可重用的知识和可调用的工作流</font>
- **[MCP](https://code.claude.com/docs/zh-CN/mcp)** 将 Claude <font color=red>连接到外部服务和工具</font>
- **[Subagents](https://code.claude.com/docs/zh-CN/sub-agents)** 在隔离的上下文中运行自己的循环，返回摘要
- **[Agent teams](https://code.claude.com/docs/zh-CN/agent-teams)** 协调多个独立会话，具有共享任务和点对点消息传递
- **[Hooks](https://code.claude.com/docs/zh-CN/hooks)** 完全在循环外作为确定性脚本运行
- **[Plugins](https://code.claude.com/docs/zh-CN/plugins)** 和 **[marketplaces](https://code.claude.com/docs/zh-CN/plugin-marketplaces)** 打包和分发这些功能

[Skills](https://code.claude.com/docs/zh-CN/skills) 是最灵活的扩展。Skill 是一个包含知识、工作流或说明的 markdown 文件。您可以使用 `/deploy` 之类的命令调用 skills，或者 Claude 可以在相关时自动加载它们。Skills 可以在您当前的对话中运行，也可以通过 subagents 在隔离的上下文中运行

##### 将功能与您的目标相匹配

功能范围从 Claude 每个会话都能看到的始终开启的上下文，到您或 Claude 可以调用的按需功能，再到在特定事件上运行的后台自动化。下表显示了可用的功能以及何时使用每个功能。

| 功能                                                         | 作用                                | 何时使用                                   | 示例                                                      |
| :----------------------------------------------------------- | :---------------------------------- | :----------------------------------------- | :-------------------------------------------------------- |
| **CLAUDE.md**                                                | 每次对话加载的持久上下文            | 项目约定、“始终执行 X” 规则                | “使用 pnpm，而不是 npm。提交前运行测试。“                 |
| **Skill**                                                    | Claude 可以使用的说明、知识和工作流 | 可重用内容、参考文档、可重复的任务         | `/deploy` 运行您的部署清单；包含端点模式的 API 文档 skill |
| **Subagent**                                                 | 返回摘要结果的隔离执行上下文        | 上下文隔离、并行任务、专门的工作者         | 读取许多文件但仅返回关键发现的研究任务                    |
| **[Agent teams](https://code.claude.com/docs/zh-CN/agent-teams)** | 协调多个独立的 Claude Code 会话     | 并行研究、新功能开发、使用竞争假设进行调试 | 生成审查者同时检查安全性、性能和测试                      |
| **MCP**                                                      | 连接到外部服务                      | 外部数据或操作                             | 查询您的数据库、发布到 Slack、控制浏览器                  |
| **Hook**                                                     | 在事件上运行的确定性脚本            | 可预测的自动化，不涉及 LLM                 | 每次文件编辑后运行 ESLint                                 |

<font color=red>**[Plugins](https://code.claude.com/docs/zh-CN/plugins)** 是 **打包层**</font>。<font color=fuchsia>Plugin 将 skills、hooks、subagents 和 MCP servers 捆绑到单个可安装单元中</font>。<font color=red>Plugin skills 是命名空间的（如 `/my-plugin:review`）</font>，因此多个 plugins 可以共存。当您想在多个存储库中重用相同的设置或通过 **[marketplace](https://code.claude.com/docs/zh-CN/plugin-marketplaces)** 分发给他人时，使用 plugins。

###### 比较相似的功能

某些功能可能看起来相似。以下是如何区分它们。

- **Skill vs Subagent**

  Skills 和 subagents 解决不同的问题：

  - **Skills** 是可重用的内容，您<font color=red>可以将其加载到任何上下文中</font>
  - **Subagents** 是与您的主对话分开运行的隔离工作者

  | 方面         | Skill                      | Subagent                                                    |
  | :----------- | :------------------------- | :---------------------------------------------------------- |
  | **它是什么** | 可重用的说明、知识或工作流 | 具有自己上下文的隔离工作者 ( workers )                      |
  | **关键优势** | 在上下文之间共享内容       | <font color=red>上下文隔离</font>。工作单独进行，仅返回摘要 |
  | **最适合**   | 参考材料、可调用的工作流   | 读取许多文件的任务、并行工作、专门的工作者                  |

  **Skills 可以是参考或操作。** 参考 skills 提供 Claude 在整个会话中使用的知识（如您的 API 风格指南）。操作 skills 告诉 Claude 执行特定操作（如运行您的部署工作流的 `/deploy`）。

  **当您需要上下文隔离或上下文窗口变满时，使用 subagent**。Subagent 可能读取数十个文件或运行广泛的搜索，但您的主对话仅接收摘要。由于 subagent 工作不消耗您的主上下文，当您不需要中间工作保持可见时，这也很有用。自定义 subagents 可以有自己的说明并可以预加载 skills。

  <font color=red>**它们可以结合**</font>。 <font color=red>Subagent 可以预加载特定的 skills（`skills:` 字段）</font>。<font color=red>**Skill 可以使用 `context: fork` 在隔离的上下文中运行**</font>。有关详细信息，请参阅 [Skills](https://code.claude.com/docs/zh-CN/skills)。

- **CLAUDE.md vs Skill**

  两者都存储说明，但它们的加载方式和用途不同。

  | 方面               | CLAUDE.md                                    | Skill                                        |
  | :----------------- | :------------------------------------------- | :------------------------------------------- |
  | **加载**           | 每个会话，自动                               | 按需                                         |
  | **可以包含文件**   | 是，<font color=red>使用 `@path` 导入</font> | 是，<font color=red>使用 `@path` 导入</font> |
  | **可以触发工作流** | 否                                           | 是，使用 `/<name>`                           |
  | **最适合**         | <font color=fuchsia>”始终执行 X” 规则</font> | 参考材料、可调用的工作流                     |

  **如果 Claude 应该始终知道它，请将其放在 CLAUDE.md 中**：编码约定、构建命令、项目结构、“永远不要执行 X” 规则。

  **如果它是 Claude 有时需要的参考材料（API 文档、风格指南）或您使用 `/<name>` 触发的工作流（部署、审查、发布），请将其放在 skill 中**。

  **经验法则：** 保持 CLAUDE.md 在 200 行以下。如果它在增长，将参考内容移到 skills 或拆分为 [`.claude/rules/`](https://code.claude.com/docs/zh-CN/memory#organize-rules-with-clauderules) 文件。

- **CLAUDE.md vs Rules vs Skills**

  所有三者都存储说明，但它们的加载方式不同：

  | 方面       | CLAUDE.md          | `.claude/rules/`                                          | Skill                    |
  | :--------- | :----------------- | :-------------------------------------------------------- | :----------------------- |
  | **加载**   | 每个会话           | <font color=red>**每个会话**</font>，或当打开匹配的文件时 | 按需，当调用或相关时     |
  | **范围**   | 整个项目           | 可以限定到文件路径                                        | 特定于任务               |
  | **最适合** | 核心约定和构建命令 | 特定于语言或目录的指南                                    | 参考材料、可重复的工作流 |

  **对于每个会话需要的说明，使用 CLAUDE.md**：构建命令、测试约定、项目架构。

  <font color=red>**使用 rules 来保持 CLAUDE.md 专注**</font>。<font color=red>带有 [`paths` frontmatter](https://code.claude.com/docs/zh-CN/memory#path-specific-rules) 的 rules</font> 仅在 Claude 处理匹配文件时加载，节省上下文。

  **对于 Claude 有时只需要的内容，使用 skills**，如 API 文档或您使用 `/<name>` 触发的部署清单。

- **Subagent vs Agent team**

  两者都并行化工作，但它们在架构上不同：

  - **Subagents** 在您的会话内运行并将结果报告回您的主上下文
  - **Agent teams** 是 <font color=red>相互通信的独立 Claude Code 会话</font>

  | 方面         | Subagent                                  | Agent team                                        |
  | :----------- | :---------------------------------------- | :------------------------------------------------ |
  | **上下文**   | 自己的上下文窗口；结果返回给调用者        | 自己的上下文窗口；完全独立                        |
  | **通信**     | 仅向主代理报告结果                        | 队友直接相互发送消息                              |
  | **协调**     | <font color=red>主代理管理所有工作</font> | <font color=red>具有自我协调的共享任务列表</font> |
  | **最适合**   | 仅结果重要的专注任务                      | 需要讨论和协作的复杂工作                          |
  | **令牌成本** | 较低：结果摘要返回到主上下文              | 较高：每个队友是一个单独的 Claude 实例            |

  **当您需要一个快速、专注的工作者时，使用 subagent**：研究一个问题、验证一个声明、审查一个文件。Subagent 完成工作并返回摘要。您的主对话保持清洁。

  **当队友需要共享发现、相互质疑和独立协调时，使用 agent team**。Agent teams 最适合具有竞争假设的研究、并行代码审查以及每个队友拥有单独部分的新功能开发。

  **过渡点：** 如果您运行并行 subagents 但遇到上下文限制，或者您的 subagents 需要相互通信，agent teams 是自然的下一步。

- **MCP vs Skill**

  MCP 将 Claude 连接到外部服务。Skills 扩展 Claude 的知识，包括如何有效地使用这些服务。

  | 方面         | MCP                                | Skill                                  |
  | :----------- | :--------------------------------- | :------------------------------------- |
  | **它是什么** | 连接到外部服务的协议               | 知识、工作流和参考材料                 |
  | **提供**     | 工具和数据访问                     | 知识、工作流、参考材料                 |
  | **示例**     | Slack 集成、数据库查询、浏览器控制 | 代码审查清单、部署工作流、API 风格指南 |

  这些解决不同的问题，可以很好地协同工作：

  **MCP** 给予 Claude 与外部系统交互的能力。没有 MCP，Claude 无法查询您的数据库或发布到 Slack。

  **Skills** 给予 Claude 关于如何有效使用这些工具的知识，以及您可以使用 `/<name>` 触发的工作流。Skill 可能包括您团队的数据库架构和查询模式，或带有您团队消息格式规则的 `/post-to-slack` 工作流。

  示例：<font color=red>MCP 服务器将 Claude 连接到您的数据库</font>。Skill 教导 Claude 您的数据模型、常见查询模式以及用于不同任务的表。

###### 了解功能如何分层

功能可以在多个级别定义：用户范围、每个项目、通过 plugins 或通过托管策略。您还可以在子目录中嵌套 CLAUDE.md 文件或在 monorepo 的特定包中放置 skills。<font color=dodgerBlue>当相同的功能存在于多个级别时，**以下是它们的分层方式**</font>：

- **CLAUDE.md 文件** 是累加的：所有级别同时向 Claude 的上下文贡献内容。<font color=red>来自您的工作目录及以上的文件在启动时加载</font>；子目录在您在其中工作时加载。当说明冲突时，Claude 使用判断来协调它们，更具体的说明通常优先。有关详细信息，请参阅 [CLAUDE.md 文件如何加载](https://code.claude.com/docs/zh-CN/memory#how-claudemd-files-load)。

  > [!NOTE]
  >
  > 上面的 “工作目录” 相关令人困惑，这里放一下英文版：
  >
  > > Files from your working directory and above load at launch; subdirectories load as you work in them.
  >
  > 在 Gemini in Google 中问了下 Gemini ，得到如下[回复](https://gemini.google.com/share/a18010f1839f)。
  >
  > 根据回答，可以大致理解 ( tldr )：工作目录是指启动 Claude Code 时，所在的文件夹，Claude Code 会读取该文件夹下的 CLAUDE.md；也会读取所有父级目录的 CLAUDE.md

- **Skills 和 subagents** 按名称覆盖：当相同的名称存在于多个级别时，一个定义根据优先级获胜（对于 skills 为托管 > 用户 > 项目；对于 subagents 为托管 > CLI 标志 > 项目 > 用户 > plugin）。Plugin skills 是 [命名空间的](https://code.claude.com/docs/zh-CN/plugins#add-skills-to-your-plugin) 以避免冲突。有关详细信息，请参阅 [skill 发现](https://code.claude.com/docs/zh-CN/skills#where-skills-live) 和 [subagent 范围](https://code.claude.com/docs/zh-CN/sub-agents#choose-the-subagent-scope)。

  > [!NOTE]
  >
  > 担心误解，放一下英文版
  >
  > > **Skills and subagents** override by name: when the same name exists at multiple levels, one definition wins based on priority (managed > user > project ***for skills***; managed > CLI flag > project > user > plugin ***for subagents***).
  >
  > 感觉 “for skills” 和 “for subagents” 前面可以加一个逗号？

- **MCP 服务器** 按名称覆盖：本地 > 项目 > 用户。有关详细信息，请参阅 [MCP 范围](https://code.claude.com/docs/zh-CN/mcp#scope-hierarchy-and-precedence)。

- **Hooks** 合并：所有注册的 hooks 为其匹配的事件触发，无论来源如何。有关详细信息，请参阅 [hooks](https://code.claude.com/docs/zh-CN/hooks)。

###### 组合功能

每个扩展解决不同的问题：CLAUDE.md 处理始终开启的上下文，<font color=red>skills 处理按需知识和**工作流**</font>，MCP 处理外部连接，subagents 处理隔离，hooks 处理自动化。真实的设置根据您的工作流组合它们。

例如，您可能使用 CLAUDE.md 处理项目约定、使用 skill 处理部署工作流、使用 MCP 连接到数据库、使用 hook 在每次编辑后运行 linting。每个功能处理它最擅长的事情。

| 模式                   | 工作原理                                                    | 示例                                                         |
| :--------------------- | :---------------------------------------------------------- | :----------------------------------------------------------- |
| **Skill + MCP**        | MCP 提供连接；skill 教导 Claude 如何很好地使用它            | MCP 连接到您的数据库，skill 记录您的架构和查询模式           |
| **Skill + Subagent**   | <font color=red>**Skill 为并行工作生成 subagents**</font>   | `/audit` skill 启动在隔离上下文中工作的安全性、性能和风格 subagents |
| **CLAUDE.md + Skills** | CLAUDE.md 保存始终开启的规则；skills 保存按需加载的参考材料 | CLAUDE.md 说 “遵循我们的 API 约定”，skill 包含完整的 API 风格指南 |
| **Hook + MCP**         | <font color=red>Hook 通过 MCP 触发外部操作</font>           | 编辑后 hook 在 Claude 修改关键文件时发送 Slack 通知          |

##### 了解上下文成本

###### 按功能的上下文成本

每个功能都有不同的加载策略和上下文成本：

| 功能           | 何时加载          | 加载内容                       | 上下文成本                   |
| :------------- | :---------------- | :----------------------------- | :--------------------------- |
| **CLAUDE.md**  | 会话开始          | 完整内容                       | 每个请求                     |
| **Skills**     | 会话开始 + 使用时 | 启动时的描述，使用时的完整内容 | 低（每个请求的描述）*        |
| **MCP 服务器** | 会话开始          | 所有工具定义和 JSON 架构       | 每个请求                     |
| **Subagents**  | 生成时            | 具有指定 skills 的新鲜上下文   | 与主会话隔离                 |
| **Hooks**      | 触发时            | 无（外部运行）                 | 零，除非 hook 返回额外上下文 |

*<font color=dodgerBlue>默认情况下</font>，<font color=red>skill 描述在**会话开始时加载**</font>，以便 Claude 可以决定何时使用它们。<font color=red>在 skill 的 frontmatter 中设置 `disable-model-invocation: true` 以将其完全隐藏在 Claude 中，直到您手动调用它</font>。这将 skills 的上下文成本降低到零，您只需自己触发这些 skills。

###### 了解功能如何加载

每个功能在会话的不同点加载。下面的选项卡解释了每个功能何时加载以及什么进入上下文。

![上下文加载：CLAUDE.md 和 MCP 在会话开始时加载并保留在每个请求中。Skills 在启动时加载描述，在调用时加载完整内容。Subagents 获得隔离的上下文。Hooks 外部运行。](https://mintcdn.com/claude-code/6yTCYq1p37ZB8-CQ/images/context-loading.svg?w=2500&fit=max&auto=format&n=6yTCYq1p37ZB8-CQ&q=85&s=7807709604d9851e7cba2c604422901c)

- **CLAUDE.md**

  **何时：** 会话开始

  **加载内容：** 所有 CLAUDE.md 文件的完整内容（托管、用户和项目级别）。

  **继承：** Claude 从您的工作目录读取 CLAUDE.md 文件直到根目录，并在访问这些文件时发现子目录中的嵌套文件。有关详细信息，请参阅 [CLAUDE.md 文件如何加载](https://code.claude.com/docs/zh-CN/memory#how-claudemd-files-load)。

  > [!TIP]
  >
  > <font color=red>保持 CLAUDE.md 在 200 行以下</font>。将参考材料移到 skills，这些 skills 按需加载。

- **Skills**

  Skills 是 Claude 工具包中的额外功能。它们可以是参考材料（如 API 风格指南）或可调用的工作流，您<font color=red>可以使用 `/<name>` 触发（如 `/deploy`）</font>。<font color=dodgerBlue>**Claude Code 附带 [捆绑的 skills](https://code.claude.com/docs/zh-CN/skills#bundled-skills)**</font>，<font color=fuchsia>如 `/simplify`、`/batch` 和 `/debug`</font>，可以开箱即用。您也可以创建自己的。Claude 在适当时使用 skills，或者您可以直接调用一个。

  **何时：** 取决于 skill 的配置。默认情况下，描述在会话开始时加载，完整内容在使用时加载。对于仅用户 skills（`disable-model-invocation: true`），在您调用它们之前不加载任何内容。

  **加载内容：** 对于模型可调用的 skills，Claude 在每个请求中看到名称和描述。当您使用 `/<name>` 调用 skill 或 Claude 自动加载它时，完整内容加载到您的对话中。

  **Claude 如何选择 skills：** Claude 将您的任务与 skill 描述相匹配，以决定哪些相关。<font color=dodgerBlue>如果描述模糊或重叠</font>，<font color=red>Claude 可能加载错误的 skill 或错过会有帮助的 skill</font>。要告诉 Claude 使用特定的 skill，请使用 `/<name>` 调用它。带有 `disable-model-invocation: true` 的 Skills 对 Claude 不可见，直到您调用它们。

  **上下文成本：** 低，直到使用。仅用户 skills 在调用前成本为零。

  **在 subagents 中：** Skills 在 subagents 中的工作方式不同。不是按需加载，而是传递给 subagent 的 skills 在启动时完全预加载到其上下文中。<font color=<font color=red>>**Subagents 不从主会话继承 skills；您必须明确指定它们**</font>。

  > [!TIP]
  >
  > 对于有副作用的 skills，使用 `disable-model-invocation: true`。这节省上下文并确保只有您触发它们。

- **MCP 服务器**

  **何时：** 会话开始。

  **加载内容：** 来自连接的服务器的所有工具定义和 JSON 架构。

  **上下文成本：** <font color=red>[工具搜索](https://code.claude.com/docs/zh-CN/mcp#scale-with-mcp-tool-search)（默认启用）将 MCP 工具加载到上下文的 10%</font>，并延迟其余部分直到需要。

  **可靠性说明：** <font color=red>MCP 连接可能**在会话中途无声地失败**</font>。如果服务器断开连接，其工具会无警告地消失。Claude 可能尝试使用不再存在的工具。<font color=dodgerBlue>如果您注意到 Claude 无法使用它之前可以访问的 MCP 工具</font>，<font color=red>请使用 `/mcp` 检查连接</font>。

  > [!TIP]
  >
  > 运行 `/mcp` 查看每个服务器的令牌成本。断开您未主动使用的服务器。

- **Subagents**

  **何时：** 按需，当您或 Claude 为任务生成一个时。

  **加载内容：** 新鲜、隔离的上下文，包含：

  - 系统提示（与父级共享以提高缓存效率）
  - agent 的 `skills:` 字段中列出的 skills 的完整内容
  - CLAUDE.md 和 git 状态（从父级继承）
  - 主 agent 在提示中传递的任何上下文

  **上下文成本：** 与主会话隔离。Subagents 不继承您的对话历史或调用的 skills。

  > [!TIP]
  >
  > 对于不需要您完整对话上下文的工作，使用 subagents。它们的隔离防止膨胀您的主会话。

- **Hooks**

  **何时：** 触发时。Hooks 在特定的生命周期事件上触发，如工具执行、会话边界、提示提交、权限请求和压缩。有关完整列表，请参阅 [Hooks](https://code.claude.com/docs/zh-CN/hooks)。

  **加载内容：** 默认情况下无。Hooks 作为外部脚本运行。

  **上下文成本：** 零，除非 hook 返回作为消息添加到您的对话中的输出。

  > [!TIP]
  >
  > Hooks 非常适合不需要影响 Claude 上下文的副作用（linting、logging）。



#### 探索 `.claude` 目录

Claude Code 读取 CLAUDE.md、settings.json、hooks、skills、commands、subagents、rules 和自动内存的位置。<font color=red>探索项目中的 `.claude` 目录和主目录中的 `~/.claude`</font>。

Claude Code 从您的项目目录和主目录中的 `~/.claude` 读取指令、设置、skills、subagents 和内存。将项目文件提交到 git 以与您的团队共享；`~/.claude` 中的文件是个人配置，适用于您的所有项目。

大多数用户只编辑 `CLAUDE.md` 和 `settings.json`。<font color=lightSeaGreen>目录的其余部分是可选的</font>：根据需要添加 skills、rules 或 subagents。

##### 探索目录

###### Project

- **CLAUDE.md**

  Project-specific instructions that <font color=red>**shape** how Claude works in this repository</font>. Put your conventions, common commands, and architectural context here so Claude operates with the same assumptions your team does.

  > [!TIP]
  > <font color=red>Target under 200 lines</font>. Longer files still load in full but may reduce adherence
  >
  > - CLAUDE.md loads into every session. If something only matters for specific tasks, move it to a [skill](https://code.claude.com/docs/en/skills) or a path-scoped [rule](https://code.claude.com/docs/en/memory#organize-rules-with-claude/rules/) so it loads only when needed
  > - List the commands you run most, like build, test, and format, so Claude knows them without you spelling them out each time
  > - Run `/memory` to open and edit CLAUDE.md from within a session
  >
  > - Also works at `.claude/CLAUDE.md` if you prefer to keep the project root clean

- **`.mcp.json`**

  > [!TIP]
  >
  > **When it loads**
  >
  > <font color=red>Servers connect when the session begins</font>. Tool schemas are deferred by default and load on demand via [tool search](https://code.claude.com/docs/en/mcp#scale-with-mcp-tool-search)

  This file holds the project-scoped <font color=red>servers your whole team uses</font>. Personal servers you want to keep to yourself go in `~/.claude.json` instead.

  > [!TIP]
  >
  > Use environment variable references for secrets: `${GITHUB_TOKEN}`

- **`.worktreeinclude`**

  Gitignored files to copy into new worktrees

  > [!TIP]
  >
  > **When it loads**
  >
  > Read when Claude creates a git worktree via `--worktree`, the `EnterWorktree` tool, or subagent `isolation: worktree`

  <font color=red>Lists gitignored files to copy from your main repository into each new worktree</font>. Worktrees are fresh checkouts, so untracked files like `.env` are missing by default. <font color=red>Patterns here use `.gitignore` syntax</font>. <font color=red>**Only files that match a pattern and are also gitignored get copied**</font>, so tracked files are never duplicated.

  > [!TIP]
  >
  > - Lives at the project root, not inside `.claude/`
  >
  > - Git-only: if you configure a [WorktreeCreate hook](https://code.claude.com/docs/en/hooks#worktreecreate) for a different VCS, this file is not read. Copy files inside your hook script instead
  >
  > - Also applies to parallel sessions in the [desktop app](https://code.claude.com/docs/en/desktop#work-in-parallel-with-sessions)

  This example copies your local environment files and a secrets config into every worktree Claude creates. Comments start with # and blank lines are ignored, same as `.gitignore`.

  ```bash
  # Local environment
  .env
  .env.local
  
  # API credentials
  config/secrets.json
  ```

- **`./claude`**

  Everything Claude Code reads that is specific to this project. If you use git, commit most files here so your team shares them; a few, like `settings.local.json` , are automatically gitignored. Each file badge shows which.

  - **`settings.json`**

    > [!NOTE]
    >
    > 如副标题所说：是配置 “Permissions, hooks, and configuration” 的

    Settings that Claude Code applies directly. **Permissions** control which commands and tools Claude can use; **hooks** run your scripts at specific points in a session. Unlike CLAUDE.md, which Claude reads as guidance, these are enforced whether Claude follows them or not.

    **Common keys**

    - [permissions](https://code.claude.com/docs/en/permissions): allow, deny, or prompt before Claude uses specific tools or commands

    - [hooks](https://code.claude.com/docs/en/hooks): run your own scripts on events like before a tool call or after a file edit

    - [statusLine](https://code.claude.com/docs/en/statusline): customize the line shown at the bottom while Claude works

    - [model](https://code.claude.com/docs/en/settings#available-settings): pick a default model for this project

    - [env](https://code.claude.com/docs/en/settings#environment-variables): environment variables set in every session

    - [outputStyle](https://code.claude.com/docs/en/output-styles): select a custom system-prompt style from `output-styles/`

    > [!TIP]
    >
    > - Bash permission patterns support wildcards: `Bash(npm test *)` matches any command starting with `npm test`
    >
    > - Array settings like `permissions.allow` combine across all scopes; scalar settings like `model` use the most specific value

    This example allows `npm test` and `npm run` commands without prompting, blocks `rm -rf`, and runs Prettier on files after Claude edits or writes them.

    ```json
    {
      "permissions": {
        "allow": [
          "Bash(npm test *)",
          "Bash(npm run *)"
        ],
        "deny": [
          "Bash(rm -rf *)"
        ]
      },
      "hooks": {
        "PostToolUse": [{
          "matcher": "Edit|Write",
          "hooks": [{
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }]
        }]
      }
    }
    ```

  - **`settings.local.json`**

    > [!TIP]
    >
    > **When it loads**
    > <font color=red>**Highest** of the user-editable settings files</font>; CLI flags and managed settings still take precedence
    >
    > > [!NOTE]
    > >
    > > 译为：**用户可编辑 **的设置文件中级别最高；命令行标志 和 受管设置 <font color=red>**仍然具有** 更高优先级</font>

    Personal settings that take precedence over the project defaults. Same JSON format as `settings.json`, but <font color=red>not committed</font>. Use this when you need different permissions or defaults than the team config.

  - **`rules/`**

    > [!TIP]
    >
    > **When it loads**
    >
    > <font color=dodgerBlue>Rules without `paths:`</font> load at session start. <font color=dodgerBlue>Rules with `paths:`</font> <font color=red>load **when a matching file enters context**</font>

    <font color=red>Project instructions split into topic files that can load conditionally based on file paths.</font> A rule without `paths:` frontmatter loads at session start like CLAUDE.md; a rule with `paths:` loads only when Claude reads a matching file.

    > [!NOTE]
    >
    > 这段就是在复述 “when it loads” 处的内容。重点是第一句 “项目说明按主题拆分为多个文件，可根据文件路径有条件地加载。”

    Like CLAUDE.md, rules are guidance Claude reads, not configuration Claude Code enforces. For guaranteed behavior use [hooks](https://code.claude.com/docs/en/hooks) or [permissions](https://code.claude.com/docs/en/permissions).

    > [!TIP]
    >
    > - Use `paths:` frontmatter with globs to scope rules to directories or file types
    >
    > - Subdirectories work: `.claude/rules/frontend/react.md` is discovered automatically
    >
    > - <font color=dodgerBlue>When CLAUDE.md approaches 200 lines</font>, <font color=red>start splitting into rules</font>

    - **`testing.md`**

      > [!TIP]
      >
      > **When it loads**
      >
      > Loaded when Claude reads a file matching the `paths:` globs below

      An example rule that only loads when Claude is working on test files. The `paths:` globs in the frontmatter define which files trigger it; here, anything ending in `.test.ts` or `.test.tsx`. <font color=dodgerBlue>For other files</font>, this rule is not loaded into context.

      > [!NOTE]
      >
      > 参考下面的示例，感觉这里已经说的很清楚了。需要匹配的文件要通过 `paths:` 来配置

      ```md
      ---
      paths:
        - "**/*.test.ts"
        - "**/*.test.tsx"
      ---
      
      # Testing Rules
      
      - Use descriptive test names: "should [expected] when [condition]"
      - Mock external dependencies, not internal modules
      - Clean up side effects in afterEach
      ```

  - **`skills/`**

    > [!TIP]
    >
    > **When it loads**
    >
    > Invoked with `/skill-name` or when Claude matches the task to a skill

    Each skill is a folder with a SKILL.md file plus any supporting files it needs. <font color=dodgerBlue>By default</font>, <font color=lightSeaGreen>both you and Claude can invoke a skill</font>. Use frontmatter to control that: `disable-model-invocation: true` for user-only workflows like `/deploy`, or `user-invocable: false` to hide from the `/` menu while Claude can still invoke it.

    > [!TIP]
    >
    > - <font color=red>Skills accept arguments</font>: `/deploy staging` passes "staging" as `$ARGUMENTS`. <font color=red>Use `$0`, `$1`, and so on for positional access</font>
    >
    > - The `description` frontmatter determines when Claude auto-invokes the skill
    >
    > - Bundle reference docs alongside SKILL.md. Claude knows the skill directory path and can read supporting files when you mention them

  - **`commands/`**

    > [!warning]
    >
    > Commands and skills are now the same mechanism. For new workflows, use [skills/](https://code.claude.com/docs/en/skills) instead: same `/name` invocation, plus you can bundle supporting files.

  - **`output-styles/`**

    > [!TIP]
    >
    > Project-scoped output styles, <font color=red>if your team shares any</font>

    > [!TIP]
    >
    > **When it loads**
    >
    > Applied at session start when selected via the outputStyle setting

    <font color=dodgerBlue>Output styles are usually personal</font>, so <font color=red>most live in `~/.claude/output-styles/`</font>. Put one here if your team shares a style, like a review mode everyone uses. See [the Global tab](https://code.claude.com/docs/zh-CN/claude-directory#ce-global-output-styles) for the full explanation and example.

  - **`agents/`**

    > [!TIP]
    >
    > Specialized subagents with their own context window

    > [!TIP]
    >
    > **When it loads**
    >
    > Runs in its own context window when you or Claude invoke it

    Each markdown file defines a subagent with its own system prompt, tool access, and optionally its own model. Subagents run in a fresh context window, keeping the main conversation clean. <font color=red>Useful for parallel work or isolated tasks</font>.

    > [!TIP]
    >
    > - Each agent gets a fresh context window, separate from your main session
    >
    > - Restrict tool access per agent with the <font color=red>`tools:` frontmatter field</font>
    >
    > - Type @ and pick an agent from the autocomplete to delegate directly

    - `code-reviewer.md`

      > [!TIP]
      >
      > **When it loads**
      >
      > Claude spawns it for review tasks, or you @-mention it from the autocomplete

      An example subagent restricted to read-only tools. The `description` frontmatter tells Claude when to delegate to it automatically; <font color=red>`tools:` limits it to Read, Grep, and Glob</font> so it can inspect code but never edit. The body becomes the subagent's system prompt.

      ```md
      ---
      name: code-reviewer
      description: Reviews code for correctness, security, and maintainability
      tools: Read, Grep, Glob
      ---
      
      You are a senior code reviewer. Review for:
      
      1. Correctness: logic errors, edge cases, null handling
      2. Security: injection, auth bypass, data exposure
      3. Maintainability: naming, complexity, duplication
      
      Every finding must include a concrete fix.
      ```

  - **`agent-memory/`**

    > [!TIP]
    >
    > Subagent persistent memory, separate from your main session auto memory

    > [!TIP]
    >
    > **When it loads**
    >
    > First 200 lines (capped at 25KB) of MEMORY.md loaded into the subagent system prompt when it runs

    Subagents with `memory: project` in their frontmatter get a dedicated memory directory here. <font color=dodgerBlue>This is distinct from your [main session auto memory](https://code.claude.com/docs/en/memory#auto-memory) at `~/.claude/projects/`</font>: <font color=red>each subagent reads and writes its own MEMORY.md, not yours</font>.

    > [!TIP]
    >
    > - Only created for subagents that set the `memory:` frontmatter field
    >
    > - This directory holds project-scoped subagent memory, meant to be shared with your team. <font color=red>To keep memory out of version control use `memory: local`</font>, which writes to `.claude/agent-memory-local/` instead. <font color=dodgerBlue>For cross-project</font> memory use `memory: user`, which writes to `~/.claude/agent-memory/`
    >
    > - The main session auto memory is a different feature; see `~/.claude/projects/` in the Global tab

    - **`<agent-name>/MEMORY.md`**

      > [!TIP]
      >
      > <font color=red>The subagent writes and maintains this file **automatically**</font>

      > [!TIP]
      >
      > **When it loads**
      >
      > Loaded into the subagent system prompt when the subagent starts

      Works the same as your [main auto memory](https://code.claude.com/docs/en/memory#auto-memory): <font color=red>the subagent creates and updates this file itself</font>. You do not write it. The subagent reads it at the start of each task and writes back what it learns.

      ```md
      # code-reviewer memory
      
      ## Patterns seen
      - Project uses custom Result<T, E> type, not exceptions
      - Auth middleware expects Bearer token in Authorization header
      - Tests use factory functions in test/factories/
      
      ## Recurring issues
      - Missing null checks on API responses (src/api/*)
      - Unhandled promise rejections in background jobs
      ```

###### Global

- **`.claude.json`**

  > [!TIP]
  >
  > App state and UI preferences

  > [!TIP]
  >
  > **When it loads**
  >
  > Read at session start for your preferences and MCP servers. Claude Code writes back to it when you change settings in `/config` or approve trust prompts

  <font color=red>Holds state that does not belong in `settings.json`</font> : theme, OAuth session, per-project trust decisions, your personal MCP servers, and UI toggles. Mostly managed through `/config` rather than editing directly.

  > [!TIP]
  >
  > - IDE toggles like `autoConnectIde` and `externalEditorContext` live here, not in `settings.json`
  >
  > - The `projects` key tracks per-project state like trust-dialog acceptance and last-session metrics. Permission rules you approve in-session go to `.claude/settings.local.json` instead
  >
  > - MCP servers here are yours only: user scope applies across all projects, local scope is per-project but not committed. <font color=red>Team-shared servers go in `.mcp.json` at the project root instead</font>

  ```json
  {
    "autoConnectIde": true,
    "externalEditorContext": true,
    "mcpServers": {
      "my-tools": {
        "command": "npx",
        "args": ["-y", "@example/mcp-server"]
      }
    }
  }
  ```

- **`.claude/`**

  > [!TIP]
  >
  > Your personal configuration across all projects

  The global counterpart to your project `.claude/` directory. Files here apply to every project you work in and are never committed to any repository.

  - **`CLAUDE.md`**

    Your global instruction file. Loaded alongside the project CLAUDE.md at session start, so both are in context together. <font color=dodgerBlue>When instructions conflict</font>, <font color=red>project-level instructions take priority</font>. <font color=dodgerBlue>Keep this to preferences that apply everywhere</font>: <font color=red>response style, commit format, personal conventions</font>.

    > [!TIP]
    >
    > - <font color=red>**Keep it short since it loads into context for every project**</font>, alongside that project's own CLAUDE.md
    > - <font color=red>Good for response style, commit format, and personal conventions</font>

    ```md
    # Global preferences
    
    - Keep explanations concise
    - Use conventional commit format
    - Show the terminal command to verify changes
    - Prefer composition over inheritance
    ```

  - **`settings.json`**

    <font color=red>Same keys as project `settings.json`</font>: permissions, hooks, model, environment variables, and the rest. <font color=dodgerBlue>Put settings here that you want in every project</font>, <font color=red>like permissions you always allow, a **preferred model**, or a notification hook that runs regardless of which project you're in</font>.

    Settings follow a precedence order: <font color=lightSeaGreen>project `settings.json` overrides any matching keys you set here</font>. <font color=dodgerBlue>**This is different from CLAUDE.md**</font>, where global and project files are both loaded into context rather than merged key by key.

    ```json
    {
      "permissions": {
        "allow": [
          "Bash(git log *)",
          "Bash(git diff *)"
        ]
      }
    }
    ```

  - **`keybindings.json`**

    > [!TIP]
    >
    > Custom keyboard shortcuts

    Rebind keyboard shortcuts in the interactive CLI. Run `/keybindings` to create or open this file with a schema reference. <font color=lightSeaGreen>Ctrl+C, Ctrl+D, Ctrl+M, and Caps Lock are reserved and cannot be rebound</font>.

    This example binds `Ctrl+E` to open your external editor and unbinds `Ctrl+U` by setting it to `null`. The `context` field scopes bindings to a specific part of the CLI, here the main chat input.

    ```json
    {
      "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
      "$docs": "https://code.claude.com/docs/en/keybindings",
      "bindings": [
        {
          "context": "Chat",
          "bindings": {
            "ctrl+e": "chat:externalEditor",
            "ctrl+u": null
          }
        }
      ]
    }
    ```

  - **`themes/`**

    > [!TIP]
    >
    > Custom color themes

    Each `.json` file defines a custom color theme: a built-in `base` preset plus an `overrides` map of color tokens. Create one interactively with `/theme` or write the JSON by hand. Selecting a custom theme stores `custom:<slug>` as your theme preference.

    ```json
    {
      "name": "Dracula",
      "base": "dark",
      "overrides": {
        "claude": "#bd93f9",
        "error": "#ff5555",
        "success": "#50fa7b"
      }
    }
    ```

  - **`projects/`**

    > [!TIP]
    >
    > Auto memory: Claude's notes to itself, per project

    > [!TIP]
    >
    > **When it loads**
    >
    > MEMORY.md loaded at session start; topic files read on demand

    <font color=red>Auto memory lets Claude accumulate knowledge across sessions without you writing anything</font>. Claude saves notes as it works: build commands, debugging insights, architecture notes. <font color=red>Each project gets its own memory directory keyed by the repository path</font>.

    > [!TIP]
    >
    > - On by default. Toggle with `/memory` or `autoMemoryEnabled` in settings
    >
    > - MEMORY.md is the index loaded each session. The first 200 lines, or 25KB, whichever comes first, are read
    >
    > - Topic files like debugging.md are read on demand, not at startup
    >
    > - These are plain markdown. Edit or delete them anytime

  - **`rules/`**

  - **`skills/`**

  - **`commands/`**

  - **`output-styles/`**

    > [!TIP]
    >
    > Custom system-prompt sections that adjust how Claude works

    > [!TIP]
    >
    > **When it loads**
    >
    > Applied at session start when selected via the outputStyle setting

    Each markdown file defines an output style: a section appended to the system prompt that, by default, also drops the built-in software-engineering task instructions. Use this to adapt Claude Code for uses beyond coding, or to add teaching or review modes.

    Select a built-in or custom style with `/config` or the `outputStyle` key in settings. Styles here are available in every project; project-level styles with the same name take precedence.

    > [!TIP]
    >
    > - Built-in styles Explanatory and Learning are included with Claude Code; custom styles go here
    >
    > - Set `keep-coding-instructions: true` in frontmatter to keep the default task instructions alongside your additions
    >
    > - Changes take effect on the next session since the system prompt is fixed at startup for caching

  - **`agents/`**

  - **`agent-memory/`**

    > [!TIP]
    >
    > Persistent memory for subagents with `memory: user`

    > [!TIP]
    >
    > **When it loads**
    >
    > Loaded into the subagent system prompt when the subagent starts

    Subagents with `memory: user` in their frontmatter store knowledge here that persists across all projects. For project-scoped subagent memory, see `.claude/agent-memory/` instead.

##### 未显示的内容

浏览器涵盖您创作和编辑的文件。一些相关文件位于其他位置：

| 文件                    | 位置                     | 用途                                                         |
| :---------------------- | :----------------------- | :----------------------------------------------------------- |
| `managed-settings.json` | 系统级别，因操作系统而异 | 企业强制执行的设置，您无法覆盖。请参阅[服务器管理的设置](https://code.claude.com/docs/zh-CN/server-managed-settings)。 |
| `CLAUDE.local.md`       | 项目根目录               | 您对此项目的私人偏好，与 CLAUDE.md 一起加载。手动创建它并将其添加到 `.gitignore`。 |
| 已安装的 plugins        | `~/.claude/plugins`      | 克隆的市场、已安装的 plugin 版本和每个 plugin 的数据，由 `claude plugin` 命令管理。孤立版本在 plugin 更新或卸载后 7 天被删除。请参阅 [plugin 缓存](https://code.claude.com/docs/zh-CN/plugins-reference#plugin-caching-and-file-resolution)。 |

`~/.claude` 还保存 Claude Code 在您工作时写入的数据：记录、提示历史、文件快照、缓存和日志。请参阅下面的[应用数据](https://code.claude.com/docs/zh-CN/claude-directory#application-data)。

##### 选择正确的文件

> [!important]
>
> 可以参考一二

不同类型的自定义位于不同的文件中。<font color=dodgerBlue>**使用此表找到更改应该放在哪里**</font>。

| 您想要                            | 编辑                                     | 范围       | 参考                                                         |
| :-------------------------------- | :--------------------------------------- | :--------- | :----------------------------------------------------------- |
| 为 Claude 提供项目上下文和约定    | `CLAUDE.md`                              | 项目或全局 | [内存](https://code.claude.com/docs/zh-CN/memory)            |
| 允许或阻止特定工具调用            | `settings.json` `permissions` 或 `hooks` | 项目或全局 | [权限](https://code.claude.com/docs/zh-CN/permissions)、[Hooks](https://code.claude.com/docs/zh-CN/hooks) |
| 在工具调用前后运行脚本            | `settings.json` `hooks`                  | 项目或全局 | [Hooks](https://code.claude.com/docs/zh-CN/hooks)            |
| 为会话设置环境变量                | `settings.json` `env`                    | 项目或全局 | [设置](https://code.claude.com/docs/zh-CN/settings#available-settings) |
| 将个人覆盖保留在 git 之外         | `settings.local.json`                    | 仅项目     | [设置范围](https://code.claude.com/docs/zh-CN/settings#settings-files) |
| 添加使用 `/name` 调用的提示或功能 | `skills/<name>/SKILL.md`                 | 项目或全局 | [Skills](https://code.claude.com/docs/zh-CN/skills)          |
| 定义具有自己工具的专门 subagent   | `agents/*.md`                            | 项目或全局 | [Subagents](https://code.claude.com/docs/zh-CN/sub-agents)   |
| 通过 MCP 连接外部工具             | `.mcp.json`                              | 仅项目     | [MCP](https://code.claude.com/docs/zh-CN/mcp)                |
| 更改 Claude 格式化响应的方式      | `output-styles/*.md`                     | 项目或全局 | [输出样式](https://code.claude.com/docs/zh-CN/output-styles) |

##### 应用数据

<font color=red>除了您创作的配置外，`~/.claude` 还保存 Claude Code 在会话期间写入的数据</font>。这些文件是纯文本。通过工具传递的任何内容都会在磁盘上的记录中：文件内容、命令输出、粘贴的文本。

###### 自动清理

下面路径中的文件在启动时被删除，一旦它们的年龄超过 [`cleanupPeriodDays`](https://code.claude.com/docs/zh-CN/settings#available-settings)。默认值为 30 天。

> [!NOTE]
>
> Files in the paths below are deleted on startup once they’re older than [`cleanupPeriodDays`](https://code.claude.com/docs/en/settings#available-settings)

| `~/.claude/` 下的路径                        | 内容                                                         |
| :------------------------------------------- | :----------------------------------------------------------- |
| `projects/<project>/<session>.jsonl`         | 完整的对话记录：每条消息、工具调用和工具结果                 |
| `projects/<project>/<session>/tool-results/` | 大型工具输出溢出到单独的文件                                 |
| `file-history/<session>/`                    | Claude 更改的文件的编辑前快照，用于[检查点恢复](https://code.claude.com/docs/zh-CN/checkpointing) |
| `plans/`                                     | 在[计划模式](https://code.claude.com/docs/zh-CN/permission-modes#analyze-before-you-edit-with-plan-mode)期间写入的计划文件 |
| `debug/`                                     | 每个会话的调试日志，仅在您使用 `--debug` 启动或运行 `/debug` 时写入 |
| `paste-cache/`、`image-cache/`               | <font color=lightSeaGreen>大型粘贴和附加图像的内容</font>    |
| `session-env/`                               | 每个会话的环境元数据                                         |
| `tasks/`                                     | 由任务工具写入的每个会话的任务列表                           |
| `shell-snapshots/`                           | 由 Bash 工具使用的捕获的 shell 环境。在正常退出时删除。扫描清理任何在崩溃后留下的内容。 |
| `backups/`                                   | 在配置迁移前获取的 `~/.claude.json` 的时间戳副本             |

###### 保留直到您删除它们

以下路径不受自动清理覆盖，并无限期保留。

| `~/.claude/` 下的路径 | 内容                                                       |
| :-------------------- | :--------------------------------------------------------- |
| `history.jsonl`       | 您输入的每个提示，带有时间戳和项目路径。用于向上箭头回忆。 |
| `stats-cache.json`    | 由 `/usage` 显示的聚合令牌和成本计数                       |
| `todos/`              | 旧版每个会话的任务列表。不再由当前版本写入；可以安全删除。 |

其他小缓存和锁定文件根据您使用的功能而出现，可以安全删除。

###### 清除本地数据

<font color=red>运行 `claude project purge` 以删除 Claude Code 为一个项目保存的状态</font>：

- `projects/` 下的记录和自动内存
- 每个会话的 `tasks/`、`debug/` 和 `file-history/` 条目
- `history.jsonl` 中的匹配提示行
- `~/.claude.json` 中的项目条目

该命令打印完整的删除计划，并在删除任何内容之前要求确认。

<font color=dodgerBlue>预览计划而不删除任何内容</font>：

```sh
claude project purge ~/work/my-repo --dry-run
```

<font color=dodgerBlue>通过单个确认提示删除</font>：

```sh
claude project purge ~/work/my-repo
```

省略路径以从交互式列表中选择项目。

跳过确认提示以在脚本中使用：

```sh
claude project purge ~/work/my-repo --yes
```

传递 `--all` 而不是路径以一次清除所有项目的状态，这会直接删除 `history.jsonl` 而不是过滤它。传递 `-i` 以逐项逐步执行删除计划。

该命令不理会 `shell-snapshots/` 和 `backups/`，因为这些不是项目范围的，并在计划输出中警告它们。如果没有状态与给定路径匹配，它以状态 1 退出。

您也可以手动删除上面的任何应用数据路径。新会话不受影响。下表显示您对过去会话失去的内容。

| 删除                                                         | 您失去                           |
| :----------------------------------------------------------- | :------------------------------- |
| `~/.claude/projects/`                                        | 恢复、继续和倒回过去的会话       |
| `~/.claude/history.jsonl`                                    | 向上箭头提示回忆                 |
| `~/.claude/file-history/`                                    | 过去会话的检查点恢复             |
| `~/.claude/stats-cache.json`                                 | 由 `/usage` 显示的历史总计       |
| `~/.claude/debug/`、`~/.claude/plans/`、`~/.claude/paste-cache/`、`~/.claude/image-cache/`、`~/.claude/session-env/`、`~/.claude/tasks/`、`~/.claude/shell-snapshots/`、`~/.claude/backups/` | 没有面向用户的内容               |
| `~/.claude/todos/`                                           | 没有。旧版目录不由当前版本写入。 |

不要删除 `~/.claude.json`、`~/.claude/settings.json` 或 `~/.claude/plugins/`：这些保存您的身份验证、偏好和已安装的 plugins。



#### Explore the context window

<font color=dodgerBlue>Claude Code’s context window holds everything Claude knows about your session</font>: <font color=red>your instructions, the files it reads, its own responses, and content that never appears in your terminal</font>. The timeline below walks through what loads and when. See [the written breakdown](https://code.claude.com/docs/en/context-window#what-the-timeline-shows) for the same content as a list.

> [!TIP]
>
> **Key takeaway**
>
> A lot loads before you type anything. CLAUDE.md, memory, skills, and MCP tools are all in context before your first prompt.
>
> > [!NOTE]
> >
> > 这里也可以结合一下 [[#Prompt Cache 到底是什么]]

- **System prompt**

  <font color=red>Core instructions for behavior, tool use, and response formatting</font>. Always loaded first. You never see it.

- **Auto memory (MEMORY.md)**

  Claude's notes to itself from previous sessions: build commands it learned, patterns it noticed, mistakes to avoid. The first 200 lines or 25KB, whichever comes first, are loaded into the conversation context.

- **Environment info**

  <font color=lightSeaGreen>Working directory, platform, shell, OS version, and whether this is a git repo</font>. Git branch, status, and recent commits load as a separate block at the very end of the system prompt.

- **MCP tools (deferred)**

  MCP tool names listed so Claude knows what is available. <font color=dodgerBlue>By default</font>, full schemas stay deferred and Claude loads specific ones on demand via tool search when a task needs them. Set `ENABLE_TOOL_SEARCH=auto` to load schemas upfront when they fit within 10% of the context window, or `ENABLE_TOOL_SEARCH=false` to load everything.

- **Skill descriptions**

  One-line descriptions of available skills so Claude knows what it can invoke. Full skill content loads only when Claude actually uses one. Skills with `disable-model-invocation: true` are not in this list. They stay completely out of context until you invoke them with `/name`. <font color=dodgerBlue>**Unlike** the rest of the startup content</font>, <font color=red>this listing is not re-injected after `/compact`</font>. Only skills you actually invoked get preserved.

- **`~/.claude/CLAUDE.md`**

  Your global preferences. Applies to every project. Loaded alongside project instructions at the start of every conversation.

- **`Project CLAUDE.md`**

  Project conventions, build commands, architecture notes. <font color=red>The most important file you can create</font>. Lives in your project root, so your whole team gets the same instructions.

##### What the timeline shows

The session walks through a realistic flow with representative token counts:

- **Before you type anything**: CLAUDE.md, auto memory, MCP tool names, and skill descriptions all load into context. Your own setup may add more here, like an [output style](https://code.claude.com/docs/en/output-styles) or text from [`--append-system-prompt`](https://code.claude.com/docs/en/cli-reference), which both go into the system prompt the same way.
- **As Claude works**: each file read adds to context, [path-scoped rules](https://code.claude.com/docs/en/memory#path-specific-rules) load automatically alongside matching files, and a [PostToolUse hook](https://code.claude.com/docs/en/hooks-guide) fires after each edit.
- **The follow-up prompt**: a [subagent](https://code.claude.com/docs/en/sub-agents) handles the research in its own separate context window, so the large file reads stay out of yours. Only the summary and a small metadata trailer come back.
- **At the end**: `/compact` replaces the conversation with a structured summary. Most startup content reloads automatically; <font color=dodgerBlue>the table below shows what happens to each mechanism</font>.

##### What survives compaction

When a long session compacts, Claude Code summarizes the conversation history to fit the context window. What happens to your instructions depends on how they were loaded:

| Mechanism                                                    | After compaction                                             |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| System prompt and output style                               | Unchanged; not part of message history                       |
| <font color=lightSeaGreen>Project-root CLAUDE.md</font> and unscoped rules | Re-injected from disk                                        |
| Auto memory                                                  | Re-injected from disk                                        |
| Rules with `paths:` frontmatter                              | Lost until a matching file is read again                     |
| <font color=red>Nested CLAUDE.md</font> in subdirectories    | <font color=red>Lost until a file in that subdirectory is read again</font> |
| Invoked skill bodies                                         | Re-injected, capped at 5,000 tokens per skill and 25,000 tokens total; oldest dropped first |
| Hooks                                                        | Not applicable; <font color=red>hooks run as code, not context</font> |

Path-scoped rules and nested CLAUDE.md files load into message history when their trigger file is read, so compaction summarizes them away with everything else. They reload the next time Claude reads a matching file. <font color=dodgerBlue>If a rule must persist across compaction</font>, <font color=red>drop the `paths:` frontmatter or move it to the project-root CLAUDE.md</font>.

Skill bodies are re-injected after compaction, but <font color=red>large skills are truncated to fit the per-skill cap</font>, and the oldest invoked skills are dropped once the total budget is exceeded. <font color=red>**Truncation keeps the start of the file**, so put the most important instructions near the top of `SKILL.md`</font>.

##### Check your own session

The visualization uses representative numbers. <font color=dodgerBlue>**To see your actual context usage at any point**</font>, <font color=red>run **`/context`** for a live breakdown by category with optimization suggestions</font>. Run **`/memory`** to check which CLAUDE.md and auto memory files loaded at startup.



#### Claude 如何记住你的项目

使用 CLAUDE.md 文件为 Claude 提供持久指令，并让 Claude 通过自动记忆功能自动积累学习内容。

每个 Claude Code 会话都从一个全新的上下文窗口开始。<font color=dodgerBlue>两种机制可以跨会话传递知识</font>：

- **CLAUDE.md 文件**：你编写的指令，为 Claude 提供持久上下文
- **自动记忆**：Claude 根据你的更正和偏好自己编写的笔记

##### CLAUDE.md 与自动记忆

Claude Code 有两个互补的记忆系统。两者都在每次对话开始时加载。Claude 将它们视为上下文，而不是强制配置。你的指令越具体和简洁，Claude 遵循它们的一致性就越高。

|              | CLAUDE.md 文件             | 自动记忆                              |
| :----------- | :------------------------- | :------------------------------------ |
| **谁编写**   | 你                         | Claude                                |
| **包含内容** | 指令和规则                 | 学习和模式                            |
| **范围**     | 项目、用户或组织           | 每个工作树                            |
| **加载到**   | 每个会话                   | 每个会话（前 200 行或 25KB）          |
| **用于**     | 编码标准、工作流、项目架构 | 构建命令、调试见解、Claude 发现的偏好 |

当你想指导 Claude 的行为时，使用 CLAUDE.md 文件。自动记忆让 Claude 从你的更正中学习，无需手动操作。

subagents 也可以维护自己的自动记忆。有关详细信息，请参阅 [subagent 配置](https://code.claude.com/docs/zh-CN/sub-agents#enable-persistent-memory)。

##### CLAUDE.md 文件

CLAUDE.md 文件是 markdown 文件，为项目、你的个人工作流或整个组织为 Claude 提供持久指令。你用纯文本编写这些文件；Claude 在每个会话开始时读取它们。

###### 何时添加到 CLAUDE.md

将 CLAUDE.md 视为你写下你本来会重新解释的内容的地方。在以下情况下添加到它：

- Claude 第二次犯同样的错误
- 代码审查发现 Claude 应该了解这个代码库的内容
- 你在聊天中输入的相同更正或澄清是你上个会话输入的
- 新队友需要相同的上下文才能提高生产力

将其保持为 Claude 应该在每个会话中保持的事实：构建命令、约定、项目布局、“总是做 X” 规则。<font color=red>如果一个条目是多步骤过程或仅对代码库的一部分重要，将其移到 [skill](https://code.claude.com/docs/zh-CN/skills) 或 [路径范围规则](https://code.claude.com/docs/zh-CN/memory#organize-rules-with-claude/rules/) 中</font>。[扩展概述](https://code.claude.com/docs/zh-CN/features-overview#build-your-setup-over-time)涵盖何时使用每种机制。

##### 选择 CLAUDE.md 文件的位置

CLAUDE.md 文件可以位于多个位置，每个位置有不同的范围。更具体的位置优先于更广泛的位置。

| 范围         | 位置                                                       | 目的                                  | 用例示例                         | 共享对象                 |
| :----------- | :--------------------------------------------------------- | :------------------------------------ | :------------------------------- | :----------------------- |
| **托管策略** | macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md` | 由 IT/DevOps 管理的组织范围指令       | 公司编码标准、安全策略、合规要求 | 组织中的所有用户         |
| **项目指令** | `./CLAUDE.md` 或 `./.claude/CLAUDE.md`                     | 项目的团队共享指令                    | 项目架构、编码标准、常见工作流   | 通过源代码控制的团队成员 |
| **用户指令** | `~/.claude/CLAUDE.md`                                      | 所有项目的个人偏好                    | 代码样式偏好、个人工具快捷方式   | 仅你（所有项目）         |
| **本地指令** | `./CLAUDE.local.md`                                        | 个人项目特定偏好；添加到 `.gitignore` | 你的沙箱 URL、首选测试数据       | 仅你（当前项目）         |

工作目录上方目录层次结构中的 `CLAUDE.md` 和 `CLAUDE.local.md` 文件在启动时完整加载。子目录中的文件在 Claude 读取这些目录中的文件时按需加载。有关完整的解析顺序，请参阅 [CLAUDE.md 文件如何加载](https://code.claude.com/docs/zh-CN/memory#how-claude-md-files-load)。

对于大型项目，你可以使用 [项目规则](https://code.claude.com/docs/zh-CN/memory#organize-rules-with-claude/rules/) 将指令分解为特定主题的文件。规则让你将指令范围限定到特定文件类型或子目录。

###### 编写有效的指令

CLAUDE.md 文件在每个会话开始时加载到上下文窗口中，与你的对话一起消耗令牌。[上下文窗口可视化](https://code.claude.com/docs/en/context-window)显示 CLAUDE.md 相对于其余启动上下文的加载位置。因为它们是上下文而不是强制配置，你编写指令的方式会影响 Claude 遵循它们的可靠性。具体、简洁、结构良好的指令效果最好。

**大小**：每个 CLAUDE.md 文件目标在 200 行以下。较长的文件消耗更多上下文并降低遵守度。如果你的指令变得很大，使用 [路径范围规则](https://code.claude.com/docs/zh-CN/memory#path-specific-rules) 以便指令仅在 Claude 处理匹配文件时加载。你也可以将内容分割成 [导入](https://code.claude.com/docs/zh-CN/memory#import-additional-files) 以便组织，尽管导入的文件仍然加载并在启动时进入上下文窗口。

**结构**：<font color=red>使用 markdown 标题和项目符号来分组相关指令</font>。Claude 扫描结构的方式与读者相同：<font color=red>有组织的部分比密集段落更容易遵循</font>。

**具体性**：编写具体到足以验证的指令。例如：

- “使用 2 空格缩进”而不是”正确格式化代码”
- “在提交前运行 `npm test`”而不是”测试你的更改”
- “API 处理程序位于 `src/api/handlers/`”而不是”保持文件有组织”

**一致性**：<font color=dodgerBlue>如果两条规则相互矛盾</font>，<font color=red>Claude 可能会任意选择一条</font>。定期审查你的 CLAUDE.md 文件、子目录中的嵌套 CLAUDE.md 文件和 [`.claude/rules/`](https://code.claude.com/docs/zh-CN/memory#organize-rules-with-claude/rules/) 以删除过时或冲突的指令。在 monorepos 中，使用 [`claudeMdExcludes`](https://code.claude.com/docs/zh-CN/memory#exclude-specific-claude-md-files) 跳过与你的工作无关的其他团队的 CLAUDE.md 文件。

###### 导入其他文件

CLAUDE.md 文件可以使用 `@path/to/import` 语法导入其他文件。导入的文件在启动时展开并加载到上下文中，与引用它们的 CLAUDE.md 一起。

允许相对路径和绝对路径。相对路径相对于包含导入的文件解析，而不是工作目录。导入的文件可以递归导入其他文件，最大深度为五跳。

要引入 README、package.json 和工作流指南，在你的 CLAUDE.md 中的任何地方使用 `@` 语法引用它们：

```md
有关项目概述，请参阅 @README，有关此项目的可用 npm 命令，请参阅 @package.json。

# 其他指令
- git 工作流 @docs/git-instructions.md
```

对于你不想签入 ( check into ) 版本控制的私人项目偏好，在项目根目录创建 `CLAUDE.local.md`。它与 `CLAUDE.md` 一起加载并以相同方式处理。将 `CLAUDE.local.md` 添加到你的 `.gitignore` 以便它不被提交；运行 `/init` 并选择个人选项会为你做这个。

如果你在同一存储库的多个 git worktrees 中工作，一个被 gitignore 的 `CLAUDE.local.md` 仅存在于你创建它的 worktree 中。要在 worktrees 中共享个人指令，改为从你的主目录导入文件：

```md
# 个人偏好
- @~/.claude/my-project-instructions.md
```

###### AGENTS.md

Claude Code 读取 `CLAUDE.md`，而不是 `AGENTS.md`。<font color=dodgerBlue>**如果你的存储库已经为其他编码代理使用 `AGENTS.md`**</font>，<font color=red>创建一个导入它的 `CLAUDE.md`</font>，<font color=red>这样两个工具都可以读取相同的指令而无需重复</font>。你也可以在导入下方添加 Claude 特定的指令。Claude 在会话开始时加载导入的文件，然后附加其余部分：

```md
@AGENTS.md

## Claude Code

对 `src/billing/` 下的更改使用 Plan Mode。
```

###### CLAUDE.md 文件如何加载

Claude Code 通过从当前工作目录向上遍历目录树来读取 CLAUDE.md 文件，检查沿途的每个目录是否有 `CLAUDE.md` 和 `CLAUDE.local.md` 文件。这意味着如果你在 `foo/bar/` 中运行 Claude Code，它会从 `foo/bar/CLAUDE.md`、`foo/CLAUDE.md` 和沿途的任何 `CLAUDE.local.md` 文件加载指令。

所有发现的文件被连接到上下文中，而不是相互覆盖。在目录树中，内容从文件系统根目录向下排序到你的工作目录。对于 `foo/bar/` 示例，`foo/CLAUDE.md` 在上下文中出现在 `foo/bar/CLAUDE.md` 之前，因此更接近你启动 Claude 的位置的指令最后被读取。在每个目录中，`CLAUDE.local.md` 在 `CLAUDE.md` 之后附加，因此你的个人笔记是 Claude 在该级别读取的最后内容。

Claude 还在当前工作目录下的子目录中发现 `CLAUDE.md` 和 `CLAUDE.local.md` 文件。它们不是在启动时加载，而是在 Claude 读取这些子目录中的文件时包含。

如果你在一个大型 monorepo 中工作，其他团队的 CLAUDE.md 文件被拾取，使用 [`claudeMdExcludes`](https://code.claude.com/docs/zh-CN/memory#exclude-specific-claude-md-files) 跳过它们。

块级 HTML 注释（`<!-- maintainer notes -->`）在 CLAUDE.md 文件中在内容注入到 Claude 的上下文之前被剥离。使用它们为人类维护者留下笔记，而不在它们上花费上下文令牌。代码块内的注释被保留。当你直接用 Read 工具打开 CLAUDE.md 文件时，注释保持可见。

- 从其他目录加载

  `--add-dir` 标志使 Claude 可以访问主工作目录外的其他目录。默认情况下，不加载这些目录中的 CLAUDE.md 文件。要也从其他目录加载记忆文件，设置 `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` 环境变量：

  ```sh
  CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1 claude --add-dir ../shared-config
  ```

  这会从其他目录加载 `CLAUDE.md`、`.claude/CLAUDE.md`、`.claude/rules/*.md` 和 `CLAUDE.local.md`。如果你从 [`--setting-sources`](https://code.claude.com/docs/zh-CN/cli-reference) 中排除 `local`，`CLAUDE.local.md` 会被跳过。

###### 使用 `.claude/rules/` 组织规则

对于较大的项目，你可以使用 `.claude/rules/` 目录将指令组织到多个文件中。这使指令保持模块化并更容易让团队维护。规则也可以 [范围限定到特定文件路径](https://code.claude.com/docs/zh-CN/memory#path-specific-rules)，因此它们仅在 Claude 处理匹配文件时加载到上下文中，减少噪音并节省上下文空间。

- **设置规则**

  在你的项目的 `.claude/rules/` 目录中放置 markdown 文件。每个文件应涵盖一个主题，具有描述性文件名，如 `testing.md` 或 `api-design.md`。所有 `.md` 文件都被递归发现，因此你可以将规则组织到子目录中，如 `frontend/` 或 `backend/`：

  ```txt
  your-project/
  ├── .claude/
  │   ├── CLAUDE.md           # 主项目指令
  │   └── rules/
  │       ├── code-style.md   # 代码样式指南
  │       ├── testing.md      # 测试约定
  │       └── security.md     # 安全要求
  ```

  没有 [`paths` frontmatter](https://code.claude.com/docs/zh-CN/memory#path-specific-rules) 的规则在启动时加载，优先级与 `.claude/CLAUDE.md` 相同。

- **特定路径的规则**

  规则可以使用带有 `paths` 字段的 YAML frontmatter 范围限定到特定文件。这些条件规则仅在 Claude 处理与指定模式匹配的文件时适用。

  ```md
  ---
  paths:
    - "src/api/**/*.ts"
  ---
  
  # API 开发规则
  
  - 所有 API 端点必须包括输入验证
  - 使用标准错误响应格式
  - 包括 OpenAPI 文档注释
  ```

  <font color=red>没有 `paths` 字段的规则无条件加载并适用于所有文件</font>。路径范围规则在 Claude 读取与模式匹配的文件时触发，而不是在每次工具使用时。

  在 `paths` 字段中 <font color=red>**使用 glob 模式按扩展名、目录或任何组合匹配文件**</font>：

  | 模式                   | 匹配                             |
  | :--------------------- | :------------------------------- |
  | `**/*.ts`              | 任何目录中的所有 TypeScript 文件 |
  | `src/**/*`             | `src/` 目录下的所有文件          |
  | `*.md`                 | 项目根目录中的 Markdown 文件     |
  | `src/components/*.tsx` | 特定目录中的 React 组件          |

  你可以指定多个模式并使用大括号扩展在一个模式中匹配多个扩展名：

  ```md
  ---
  paths:
    - "src/**/*.{ts,tsx}"
    - "lib/**/*.ts"
    - "tests/**/*.test.ts"
  ---
  ```

- **使用符号链接跨项目共享规则**

  `.claude/rules/` 目录支持符号链接，因此你可以维护一组共享规则并将它们链接到多个项目中。符号链接被解析并正常加载，循环符号链接被检测并优雅处理。

  此示例链接共享目录和单个文件：

  ```sh
  ln -s ~/shared-claude-rules .claude/rules/shared
  ln -s ~/company-standards/security.md .claude/rules/security.md
  ```

- **用户级规则**

  `~/.claude/rules/` 中的个人规则适用于你机器上的每个项目。使用它们来处理不是项目特定的偏好：

  ```txt
  ~/.claude/rules/
  ├── preferences.md    # 你的个人编码偏好
  └── workflows.md      # 你的首选工作流
  ```

  用户级规则在项目规则之前加载，给予项目规则更高的优先级。

##### 自动记忆

自动记忆让 Claude 跨会话积累知识，无需你编写任何内容。Claude 在工作时为自己保存笔记：构建命令、调试见解、架构笔记、代码样式偏好和工作流习惯。Claude 不会每个会话都保存内容。它根据信息在未来对话中是否有用来决定什么值得记住。

###### 启用或禁用自动记忆

自动记忆默认开启。要切换它，在会话中打开 `/memory` 并使用自动记忆切换，或在你的项目设置中设置 `autoMemoryEnabled`：

```json
{
  "autoMemoryEnabled": false
}
```

要通过环境变量禁用自动记忆，设置 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`。

###### 存储位置

每个项目在 `~/.claude/projects/<project>/memory/` 获得自己的记忆目录。`<project>` 路径来自 git 存储库，因此同一存储库中的所有 worktrees 和子目录共享一个自动记忆目录。在 git 存储库外，改用项目根目录。

要将自动记忆存储在不同位置，在你的用户或本地设置中设置 `autoMemoryDirectory`：

```json
{
  "autoMemoryDirectory": "~/my-custom-memory-dir"
}
```

此设置从策略、本地和用户设置接受。它不从项目设置（`.claude/settings.json`）接受，以防止共享项目将自动记忆写入重定向到敏感位置。

目录包含一个 `MEMORY.md` 入口点和可选的主题文件：

```txt
~/.claude/projects/<project>/memory/
├── MEMORY.md          # 简洁索引，加载到每个会话
├── debugging.md       # 关于调试模式的详细笔记
├── api-conventions.md # API 设计决策
└── ...                # Claude 创建的任何其他主题文件
```

<font color=red>`MEMORY.md` 充当记忆目录的索引</font>。Claude 在你的会话中读取和写入此目录中的文件，使用 `MEMORY.md` 跟踪存储的内容。

<font color=red>**自动记忆是机器本地的**</font>。同一 git 存储库中的所有 worktrees 和子目录共享一个自动记忆目录。<font color=red>文件不在机器或云环境之间共享</font>。

###### 它如何工作

`MEMORY.md` 的前 200 行或前 25KB（以先到者为准）在每次对话开始时加载。第 200 行之外的内容在会话开始时不加载。Claude 通过将详细笔记移到单独的主题文件中来保持 `MEMORY.md` 简洁。

此限制仅适用于 `MEMORY.md`。<font color=red>CLAUDE.md 文件无论长度如何都完整加载</font>，尽管较短的文件产生更好的遵守度。

主题文件如 `debugging.md` 或 `patterns.md` 在启动时不加载。Claude 在需要信息时使用其标准文件工具按需读取它们。

Claude 在你的会话中读取和写入记忆文件。<font color=red>当你在 Claude Code 界面中看到 “Writing memory” 或 “Recalled memory” 时，Claude 正在主动更新或读取 `~/.claude/projects/<project>/memory/`</font>。

###### 审计和编辑你的记忆

自动记忆文件是纯 markdown，你可以随时编辑或删除。运行 [`/memory`](https://code.claude.com/docs/zh-CN/memory#view-and-edit-with-memory) 从会话中浏览和打开记忆文件。

##### 使用 `/memory` 查看和编辑

`/memory` 命令列出在你当前会话中加载的所有 `CLAUDE.md` 、`CLAUDE.local.md` 和规则文件，让你切换自动记忆开或关，并提供打开自动记忆文件夹的链接。选择任何文件在你的编辑器中打开它。

当你要求 Claude 记住某些内容时，如 “总是使用 pnpm，而不是 npm” 或 ”记住 API 测试需要本地 Redis 实例”，Claude 将其保存到自动记忆。要改为添加指令到 CLAUDE.md，直接要求 Claude，如”将其添加到 CLAUDE.md”，或通过 `/memory` 自己编辑文件。



#### Claude Code 最佳实践

##### 先探索，再规划，最后编码

让 Claude 直接跳到编码可能会产生解决错误问题的代码。使用 [Plan Mode](https://code.claude.com/docs/zh-CN/common-workflows#use-plan-mode-for-safe-code-analysis) 将探索与执行分开。

推荐的工作流有四个阶段：

1. **探索**
   <font color=red>进入 Plan Mode</font>。Claude 读取文件并回答问题，不进行任何更改。
   ```md
   read /src/auth and understand how we handle sessions and login.
   also look at how we manage environment variables for secrets.
   ```
2. **规划**
   要求 Claude 创建详细的实现计划。
   ```md
   I want to add Google OAuth. What files need to change?
   What's the session flow? Create a plan.
   ```
   按 `Ctrl+G` 在文本编辑器中打开计划进行直接编辑，然后 Claude 继续。
3. **实现**
   <font color=red>切换回 Normal Mode 并让 Claude 编码</font>，根据其计划进行验证。
   ```md
   implement the OAuth flow from your plan. write tests for the
   callback handler, run the test suite and fix any failures.
   ```
4. **提交**
   要求 Claude 使用描述性消息进行提交并创建 PR。
   ```md
   commit with a descriptive message and open a PR
   ```

##### 在提示中提供具体的上下文

> [!TIP]
> 你的指令越精确，你需要的更正就越少。

Claude 可以推断意图，但它不能读心术。引用特定文件、提及约束，并指出示例模式。

| 策略                              | 之前                                   | 之后                                                                                                                  |
| ------------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **限定任务范围。** 指定哪个文件、什么场景和测试偏好。   | _“为 foo.py 添加测试"_                    | _"为 foo.py 编写测试，涵盖用户已注销的边界情况。避免 mock。“_                                                                             |
| **指向来源。** 指导 Claude 到可以回答问题的来源。 | _“为什么 ExecutionFactory 有这样奇怪的 api？"_ | _"查看 ExecutionFactory 的 git 历史并总结其 api 是如何形成的”_                                                                     |
| **参考现有模式。** 指向代码库中的模式。          | _“添加日历小部件"_                          | _"查看主页上现有小部件的实现方式以了解模式。HotDogWidget.php 是一个很好的例子。按照模式实现一个新的日历小部件，让用户选择月份并向前/向后分页以选择年份。从头开始构建，除了代码库中已使用的库外，不使用其他库。“_ |
| **描述症状。** 提供症状、可能的位置以及”修复”的样子。  | _“修复登录错误"_                           | _"用户报告会话超时后登录失败。检查 src/auth/ 中的身份验证流程，特别是 token 刷新。编写一个失败的测试来重现问题，然后修复它”_                                           |

当你在探索并能够改正方向时，模糊的提示可能很有用。像 `"你会改进这个文件的什么？"` 这样的提示可以表面你不会想到要问的东西。

##### 编写有效的 CLAUDE.md

| ✅ 包括                 | ❌ 排除                    |
| -------------------- | ----------------------- |
| Claude 无法猜测的 Bash 命令 | Claude 可以通过读取代码弄清楚的任何东西 |
| 与默认值不同的代码风格规则        | Claude 已经知道的标准语言约定      |
| 测试指令和首选测试运行器         | 详细的 API 文档（改为链接到文档）     |
| 存储库礼仪（分支命名、PR 约定）    | 经常变化的信息                 |
| 特定于你的项目的架构决策         | 长解释或教程                  |
| 开发者环境怪癖（必需的环境变量）     | 自明的实践，如”编写干净的代码”        |
| 常见陷阱或非显而易见的行为        | 文件逐个描述代码库               |

你可以通过添加强调（例如”IMPORTANT”或”YOU MUST”）来调整指令以改进遵守。将文件检入 git，以便你的团队可以贡献。该文件随时间增加价值。

CLAUDE.md 文件可以使用 `@path/to/import` 语法导入其他文件：

```md
See @README.md for project overview and @package.json for available npm commands.

# Additional Instructions
- Git workflow: @docs/git-instructions.md
- Personal overrides: @~/.claude/my-project-instructions.md
```

你可以在多个位置放置 CLAUDE.md 文件：

- **主文件夹（`~/.claude/CLAUDE.md`）**：适用于所有 Claude 会话
- **项目根目录（`./CLAUDE.md`）**：检入 git 以与你的团队共享
- **父目录**：对于 monorepos 有用，其中 `root/CLAUDE.md` 和 `root/foo/CLAUDE.md` 都会自动拉入
- **子目录**：当处理这些目录中的文件时，Claude 按需拉入子 CLAUDE.md 文件

##### 配置权限

> [!TIP]
> 使用 [auto mode](https://code.claude.com/docs/zh-CN/permission-modes#eliminate-prompts-with-auto-mode) 让分类器处理批准，使用 `/permissions` 来允许列表特定命令，或使用 `/sandbox` 进行操作系统级隔离。每种方式都减少中断，同时让你保持控制。

默认情况下，Claude Code 请求可能修改你的系统的操作的权限：文件写入、Bash 命令、MCP 工具等。这是安全的但繁琐。在第十次批准后，你不是真的在审查，你只是点击通过。有三种方式来减少这些中断：

- **Auto mode**：一个单独的分类器模型审查命令并仅阻止看起来有风险的东西：范围升级、未知基础设施或由敌对内容驱动的操作。最适合当你信任任务的总体方向但不想点击通过每一步时
- **权限允许列表**：允许你知道是安全的特定工具，如 `npm run lint` 或 `git commit`
- **沙箱**：启用操作系统级隔离，限制文件系统和网络访问，允许 Claude 在定义的边界内更自由地工作

阅读更多关于 [权限模式](https://code.claude.com/docs/zh-CN/permission-modes)、[权限规则](https://code.claude.com/docs/zh-CN/permissions) 和 [沙箱](https://code.claude.com/docs/zh-CN/sandboxing)。

##### 创建自定义 subagents

> [!TIP]
> 在 `.claude/agents/` 中定义专门的助手，Claude 可以委托给它们来<font color=red>处理隔离的任务</font>。

[Subagents](https://code.claude.com/docs/zh-CN/sub-agents) 在自己的 context 中运行，拥有自己的一组允许的工具。它们<font color=red>对于读取许多文件或需要专门关注而不会使你的主对话混乱的任务很有用</font>。

##### 有效沟通

你与 Claude Code 沟通的方式显著影响结果的质量。

###### 提出代码库问题

> [!TIP]
> 问 Claude 你会问资深工程师的问题。

当加入新代码库时，使用 Claude Code 进行学习和探索。你<font color=red>可以问 Claude 你会问另一个工程师的相同类型的问题</font>：

- 日志如何工作？
- 我如何创建新的 API 端点？
- `foo.rs` 第 134 行的 `async move { ... }` 做什么？
- `CustomerOnboardingFlowImpl` 处理哪些边界情况？
- 为什么这段代码在第 333 行调用 `foo()` 而不是 `bar()`？

以这种方式使用 Claude Code 是一个有效的入职工作流，改进了加入时间并减少了对其他工程师的负担。<font color=red>无需特殊提示：直接提问</font>。

##### 让 Claude 采访你

> [!TIP]
> 对于更大的功能，<font color=fuchsia>**让 Claude 先采访你**</font>。从最小的提示开始，要求 Claude 使用 `AskUserQuestion` 工具采访你。

Claude 会问你可能还没有考虑过的东西，包括技术实现、UI/UX、边界情况和权衡。

>[!NOTE]
>下面的 prompt ，无论是 “interview me ...” 、“Ask about ...”、“Keep interviewing until ...” 还是 “then write a complete spec to SPEC.md”，很有参考意义

```md
I want to build [brief description]. Interview me in detail using the AskUserQuestion tool.

Ask about technical implementation, UI/UX, edge cases, concerns, and tradeoffs. Don't ask obvious questions, dig into the hard parts I might not have considered.

Keep interviewing until we've covered everything, then write a complete spec to SPEC.md.
```

<font color=fuchsia>一旦规范完成，**启动新会话来执行它**</font>。新会话有干净的 context，完全专注于实现，你有一个书面规范可以参考。

##### 管理你的会话

对话是持久的和可逆的。利用这一点！

######  尽早且经常改正方向

> [!TIP]
> 一旦你注意到 Claude 偏离轨道，立即改正它。

最好的结果来自紧密的反馈循环。虽然 Claude 有时会在第一次尝试时完美地解决问题，但快速改正它通常会更快地产生更好的解决方案。

- **`Esc`**：使用 `Esc` 键在中途停止 Claude。Context 被保留，所以你可以重定向。
- **`Esc + Esc` 或 `/rewind`**：按 `Esc` 两次或运行 `/rewind` 来打开 rewind 菜单并恢复之前的对话和代码状态，或从选定的消息进行总结。
- **`"撤销那个"`**：让 Claude 恢复其更改。
  > [!NOTE]
  >英文版本是 “**`"Undo that"`**: have Claude revert its changes.”
- **`/clear`**：<font color=fuchsia>**在不相关的任务之间重置 context**</font>。长会话与无关的 context 可能会降低性能。

如果你在一个会话中对同一问题改正了 Claude 两次以上，context 就充满了失败的方法。运行 `/clear` 并使用更具体的提示重新开始，该提示包含你学到的东西。干净的会话与更好的提示几乎总是优于长会话与累积的改正。

###### 积极管理 context

> [!TIP]
> 在不相关的任务之间频繁运行 `/clear` 来重置 context。

Claude Code 在你接近 context 限制时自动压缩对话历史，这保留了重要的代码和决策，同时释放空间。在长会话中，Claude 的 context window 可能会充满无关的对话、文件内容和命令。这可能会降低性能，有时会分散 Claude 的注意力。

- 在任务之间频繁使用 `/clear` 来完全重置 context window
- 当自动压缩触发时，Claude 总结最重要的东西，包括代码模式、文件状态和关键决策
- <font color=fuchsia>为了更多控制，运行 `/compact <instructions>`，如 `/compact Focus on the API changes`</font>
- 要仅压缩对话的一部分，使用 `Esc + Esc` 或 `/rewind`，选择消息检查点，并选择 **从这里总结**。这会压缩从该点开始的消息，同时保持早期 context 完整。
- 在 CLAUDE.md 中使用像 `"When compacting, always preserve the full list of modified files and any test commands"` 这样的指令来自定义压缩行为，以确保关键 context 在总结中存活
- <font color=fuchsia>对于 **不需要留在 context 中的快速问题**，使用 [`/btw`](https://code.claude.com/docs/zh-CN/interactive-mode#side-questions-with-btw)</font>。答案出现在可关闭的覆盖层中，永远不会进入对话历史，所以你可以检查细节而不增加 context。

###### 使用 subagents 进行调查

> [!TIP]
> 使用 `"use subagents to investigate X"` 委托研究。它们在单独的 context 中探索，为实现保持你的主对话干净。

由于 context 是你的基本约束，subagents 是可用的最强大的工具之一。当 Claude 研究代码库时，它读取许多文件，所有这些都消耗你的 context。Subagents 在单独的 context windows 中运行并报告摘要：

```md
Use subagents to investigate how our authentication system handles token
refresh, and whether we have any existing OAuth utilities I should reuse.
```

subagent 探索代码库、读取相关文件并报告发现，所有这些都不会使你的主对话混乱。你也可以在 Claude 实现某些东西后使用 subagents 进行验证：

```md
use a subagent to review this code for edge cases
```

###### 使用检查点进行 Rewind

> [!TIP]
> Claude 进行的<font color=fuchsia>每个操作都会创建一个检查点</font>。你可以将对话、代码或两者恢复到任何之前的检查点。

Claude 在更改前自动检查点。双击 `Escape` 或运行 `/rewind` 来打开 rewind 菜单。你可以仅恢复对话、仅恢复代码、恢复两者或从选定的消息进行总结。有关详细信息，请参阅 [Checkpointing](https://code.claude.com/docs/zh-CN/checkpointing)。

与其仔细规划每一步，你可以告诉 Claude 尝试一些冒险的事情。如果不起作用，rewind 并尝试不同的方法。检查点在会话中持续，所以你可以关闭你的终端并稍后仍然 rewind。

> [!WARNING]
> 检查点仅跟踪 Claude 进行的更改，不跟踪外部进程。这不是 git 的替代品。

###### 恢复对话

> [!TIP]
> 运行 `claude --continue` 来继续你离开的地方，或 `--resume` 来从最近的会话中选择。

Claude Code 在本地保存对话。当任务跨越多个会话时，你不必重新解释 context：

```sh
claude --continue    # Resume the most recent conversation
claude --resume      # Select from recent conversations
```

<font color=red>**使用 `/rename` 给会话起描述性名称**</font>，如 `"oauth-migration"` 或 `"debugging-memory-leak"`，以便你稍后可以找到它们。像对待分支一样对待会话：不同的工作流可以有单独的、持久的 context。

##### 自动化和扩展

一旦你对一个 Claude 有效，通过并行会话、非交互模式和扇出模式来增加你的输出。到目前为止，一切都假设一个人、一个 Claude 和一个对话。但 Claude Code 水平扩展。本部分中的技术展示了你如何能做更多。

###### 运行非交互模式

> [!TIP]
> 在 CI、pre-commit hooks 或脚本中使用 `claude -p "prompt"`。添加 `--output-format stream-json` 用于流式 JSON 输出。

使用 `claude -p "your prompt"`，你可以非交互地运行 Claude，不需要会话。非交互模式是你将 Claude 集成到 CI 管道、pre-commit hooks 或任何自动化工作流中的方式。<font color=red>输出格式让你以编程方式解析结果：纯文本、JSON 或流式 JSON</font>。

```md
# One-off queries
claude -p "Explain what this project does"

# Structured output for scripts
claude -p "List all API endpoints" --output-format json

# Streaming for real-time processing
claude -p "Analyze this log file" --output-format stream-json
```

###### 跨文件扇出

> [!TIP]
> 循环遍历任务，为每个调用 `claude -p`。使用 `--allowedTools` 来限定批量操作的权限。

对于大型迁移或分析，你可以跨许多并行 Claude 调用分配工作：

1. **生成任务列表**

   让 Claude 列出所有需要迁移的文件（例如，`list all 2,000 Python files that need migrating`）

2. **编写脚本来循环遍历列表**
   ```sh
   for file in $(cat files.txt); do
     claude -p "Migrate $file from React to Vue. Return OK or FAIL." \
       --allowedTools "Edit,Bash(git commit *)"
   done

3. **在几个文件上测试，然后大规模运行**

   根据前 2-3 个文件出错的情况精化你的提示，然后在完整集合上运行。`--allowedTools` 标志限制 Claude 能做什么，这在你无人值守运行时很重要。

你也可以将 Claude 集成到现有的数据/处理管道中：

```sh
claude -p "<your prompt>" --output-format json | your_command
```

在开发期间使用 `--verbose` 进行调试，在生产中关闭它。

###### 使用 auto mode 自主运行

为了不间断的执行和后台安全检查，使用 [auto mode](https://code.claude.com/docs/zh-CN/permission-modes#eliminate-prompts-with-auto-mode)。分类器模型在命令运行前审查它们，阻止范围升级、未知基础设施和由敌对内容驱动的操作，同时让常规工作无提示进行。

```sh
claude --permission-mode auto -p "fix all lint errors"
```

对于使用 `-p` 标志的非交互运行，如果分类器重复阻止操作，auto mode 会中止，因为没有用户可以回退到。请参阅 [auto mode 何时回退](https://code.claude.com/docs/zh-CN/permission-modes#when-auto-mode-falls-back) 了解阈值。



#### 通过 MCP 将 Claude Code 连接到工具

Claude Code 可以通过 [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction)（一个用于 AI 工具集成的开源标准）连接到数百个外部工具和数据源。MCP 服务器为 Claude Code 提供对您的工具、数据库和 API 的访问权限。

##### 使用 MCP 可以做什么

连接 MCP 服务器后，您可以要求 Claude Code：

- **从问题跟踪器实现功能**：“添加 **JIRA** 问题 ENG-4521 中描述的功能，并在 **GitHub** 上创建 PR。”
- **分析监控数据**：“检查 **Sentry** 和 **Statsig** 以检查 ENG-4521 中描述的功能的使用情况。”
- **查询数据库**：“根据我们的 **PostgreSQL** 数据库，查找使用功能 ENG-4521 的 10 个随机用户的电子邮件。”
- **集成设计**：“根据在 **Slack** 中发布的新 **Figma** 设计更新我们的标准电子邮件模板”
- **自动化工作流**：“创建 **Gmail** 草稿，邀请这 10 个用户参加关于新功能的反馈会议。”
- **对外部事件做出反应**：MCP 服务器也可以充当[频道](https://code.claude.com/docs/zh-CN/channels)，将消息推送到您的会话中，因此当您不在时，Claude 可以对 **Telegram** 消息、**Discord** 聊天或 webhook 事件做出反应。

#####  流行的 MCP 服务器

[这里](https://code.claude.com/docs/zh-CN/mcp#%E6%B5%81%E8%A1%8C%E7%9A%84-mcp-%E6%9C%8D%E5%8A%A1%E5%99%A8) 有一些 可以连接到 Claude Code 的常用 MCP 服务器，考虑到有点多，同时时效性较强，变化很快，所以，没必要记录；这里略，详见链接，这里作为速查表。

##### 安装 MCP 服务器

MCP 服务器可以根据您的需求以三种不同的方式进行配置：

###### 选项 1：添加远程 HTTP 服务器

HTTP 服务器是连接到远程 MCP 服务器的<font color=red>**推荐选项**</font>。这是<font color=red>云服务 **最广泛支持** 的传输方式</font>。

```sh
# 基本语法
claude mcp add --transport http <name> <url>

# 真实示例：连接到 Notion
claude mcp add --transport http notion https://mcp.notion.com/mcp

# 带有 Bearer 令牌的示例
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "Authorization: Bearer your-token"
```

###### 选项 2：添加远程 SSE 服务器

> [!warning]
>
> <font color=red>**SSE (Server-Sent Events) 传输已弃用**</font>。请在可用的地方使用 HTTP 服务器。

```sh
# 基本语法
claude mcp add --transport sse <name> <url>

# 真实示例：连接到 Asana
claude mcp add --transport sse asana https://mcp.asana.com/sse

# 带有身份验证标头的示例
claude mcp add --transport sse private-api https://api.company.com/sse \
  --header "X-API-Key: your-key-here"
```

###### 选项 3：添加本地 stdio 服务器

Stdio 服务器作为您机器上的本地进程运行。它们非常适合需要直接系统访问或自定义脚本的工具。

```sh
# 基本语法
claude mcp add [options] <name> -- <command> [args...]

# 真实示例：添加 Airtable 服务器
claude mcp add --transport stdio --env AIRTABLE_API_KEY=YOUR_KEY airtable \
  -- npx -y airtable-mcp-server
```

> [!important]
>
> **重要：选项顺序**
>
> <font color=red>**所有选项**</font>（`--transport`、`--env`、`--scope`、`--header`）<font color=red>必须在服务器名称**之前**</font>。然后 `--`（双破折号）将服务器名称与传递给 MCP 服务器的命令和参数分开。例如：
>
> - `claude mcp add --transport stdio myserver -- npx server` → 运行 `npx server`
> - `claude mcp add --transport stdio --env KEY=value myserver -- python server.py --port 8080` → 运行 `python server.py --port 8080`，环境中有 `KEY=value`
>
> 这可以防止 Claude 的标志与服务器标志之间的冲突。

###### 管理您的服务器

配置后，您可以使用这些命令管理您的 MCP 服务器：

```sh
# 列出所有配置的服务器
claude mcp list

# 获取特定服务器的详细信息
claude mcp get github

# 删除服务器
claude mcp remove github

# （在 Claude Code 中）检查服务器状态
/mcp
```

######  动态工具更新

Claude Code 支持 MCP `list_changed` 通知，允许 MCP 服务器动态更新其可用工具、提示和资源，而无需您断开连接并重新连接。当 MCP 服务器发送 `list_changed` 通知时，Claude Code 会自动刷新来自该服务器的可用功能。

> [!NOTE]
>
> 感觉可以简单概括为：“无感升级” 和 “热更新”

###### 使用频道推送消息

MCP 服务器也可以直接将消息推送到您的会话中，以便 Claude 可以对外部事件（如 CI 结果、监控警报或聊天消息）做出反应。要启用此功能，您的服务器声明 `claude/channel` 功能，并在启动时使用 `--channels` 标志选择加入。请参阅[频道](https://code.claude.com/docs/zh-CN/channels)以使用官方支持的频道，或[频道参考](https://code.claude.com/docs/zh-CN/channels-reference)以构建您自己的频道。

###### 使用频道推送消息

MCP 服务器也可以直接将消息推送到您的会话中，以便 Claude 可以对外部事件（如 CI 结果、监控警报或聊天消息）做出反应。要启用此功能，您的服务器声明 `claude/channel` 功能，并在启动时使用 `--channels` 标志选择加入。请参阅[频道](https://code.claude.com/docs/zh-CN/channels)以使用官方支持的频道，或[频道参考](https://code.claude.com/docs/zh-CN/channels-reference)以构建您自己的频道。

> [!TIP]
>
> 提示：
>
> - 使用 `--scope` 标志指定配置的存储位置：
>
>   - `local`（默认）：<font color=red>**仅** **在当前项目中** **对您可用**</font>（在较旧版本中称为 `project`）
>
>     > [!NOTE]
>     >
>     > 辅助理解，类似于 `.env.local`
>
>   - `project`：通过 `.mcp.json` 文件与项目中的每个人共享
>
>     > [!NOTE]
>     >
>     > 原文为：
>     >
>     > > `project`: Shared with everyone in the project via `.mcp.json` file
>     >
>     > 不过，这还是没有说清楚，project 的具体范围。问了下 Gemini，得到 [回复](https://gemini.google.com/share/dbda0774ae7e)，就是当前项目
>
>   - `user`：在所有项目中对您可用（在较旧版本中称为 `global`）
>
> - 使用 `--env` 标志设置环境变量（例如，`--env KEY=value`）
>
> - 使用 MCP_TIMEOUT 环境变量配置 MCP 服务器启动超时（例如，`MCP_TIMEOUT=10000 claude` 设置 10 秒超时）
>
> - 当 MCP 工具输出超过 10,000 个令牌时，Claude Code 将显示警告。要增加此限制，请设置 `MAX_MCP_OUTPUT_TOKENS` 环境变量（例如，`MAX_MCP_OUTPUT_TOKENS=50000`）
>
> - <font color=red>使用 `/mcp` 对需要 OAuth 2.0 身份验证的远程服务器进行身份验证</font>

###### 插件提供的 MCP 服务器

[插件](https://code.claude.com/docs/zh-CN/plugins)可以捆绑 MCP 服务器，在启用插件时自动提供工具和集成。插件 MCP 服务器的工作方式与用户配置的服务器相同。<font color=dodgerBlue>**插件 MCP 服务器的工作原理**</font>：

- <font color=red>插件在 **插件根目录的 `.mcp.json`** 中或在 `plugin.json` 中内联定义 MCP 服务器</font>
- 启用插件时，其 MCP 服务器会自动启动
- 插件 MCP 工具与手动配置的 MCP 工具一起出现
- 插件服务器通过插件安装进行管理（不是 `/mcp` 命令）

**示例插件 MCP 配置**：

<font color=dodgerBlue>在插件根目录的 `.mcp.json` 中：</font>

```json
{
  "mcpServers": {
    "database-tools": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/db-server",
      "args": ["--config", "${CLAUDE_PLUGIN_ROOT}/config.json"],
      "env": {
        "DB_URL": "${DB_URL}"
      }
    }
  }
}
```

或 <font color=dodgerBlue>在 `plugin.json` 中内联</font>：

```json
{
  "name": "my-plugin",
  "mcpServers": {
    "plugin-api": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/api-server",
      "args": ["--port", "8080"]
    }
  }
}
```

**插件 MCP 功能**：

- **自动生命周期**：在会话启动时，启用的插件的服务器会自动连接。如果您在会话期间启用或禁用插件，请运行 `/reload-plugins` 以连接或断开其 MCP 服务器
- **环境变量**：对插件相对路径使用 `${CLAUDE_PLUGIN_ROOT}`，对[持久状态](https://code.claude.com/docs/zh-CN/plugins-reference#persistent-data-directory)使用 `${CLAUDE_PLUGIN_DATA}`，该状态在插件更新后仍然存在
- **用户环境访问**：访问与手动配置的服务器相同的环境变量
- **多种传输类型**：支持 stdio、SSE 和 HTTP 传输（传输支持可能因服务器而异）

**查看插件 MCP 服务器**：

```sh
# 在 Claude Code 中，查看所有 MCP 服务器，包括插件服务器
/mcp
```

插件服务器在列表中出现，并带有指示它们来自插件的指示符。

<font color=dodgerBlue>**插件 MCP 服务器的优势**：</font>

- **捆绑分发**：工具和服务器打包在一起
- **自动设置**：无需手动 MCP 配置
- **团队一致性**：安装插件时每个人都获得相同的工具

有关使用插件捆绑 MCP 服务器的详细信息，请参阅[插件组件参考](https://code.claude.com/docs/zh-CN/plugins-reference#mcp-servers)。

##### MCP 安装范围

MCP 服务器可以在三个不同的范围级别进行配置。您选择的范围控制服务器在哪些项目中加载以及配置是否与您的团队共享。

| 范围 | 加载位置     | 与团队共享                | 存储位置                   |
| :--- | :----------- | :------------------------ | :------------------------- |
| 本地 | 仅当前项目   | <font color=red>否</font> | `~/.claude.json`           |
| 项目 | 仅当前项目   | 是，通过版本控制          | 项目根目录中的 `.mcp.json` |
| 用户 | 您的所有项目 | 否                        | `~/.claude.json`           |

###### 本地范围

<font color=red>**本地范围** 是默认范围</font>。本地范围的服务器仅在您添加它的项目中加载，并对您保持私密。Claude Code 将其存储在 `~/.claude.json` 中该项目的路径下，因此相同的服务器不会出现在您的其他项目中。对个人开发服务器、实验配置或包含您不想在版本控制中的凭据的服务器使用本地范围。

> [!caution]
>
> MCP 服务器的”本地范围”术语与一般本地设置不同。MCP 本地范围的服务器存储在 `~/.claude.json`（您的主目录）中，而一般本地设置使用 `.claude/settings.local.json`（在项目目录中）。有关设置文件位置的详细信息，请参阅[设置](https://code.claude.com/docs/zh-CN/settings#settings-files)。

```sh
# 添加本地范围的服务器（默认）
claude mcp add --transport http stripe https://mcp.stripe.com

# 显式指定本地范围
claude mcp add --transport http stripe --scope local https://mcp.stripe.com
```

从 `/path/to/your/project` 运行时，该命令将服务器写入 `~/.claude.json` 中您当前项目的条目。下面的示例显示结果：

```json
{
  "projects": {
    "/path/to/your/project": {
      "mcpServers": {
        "stripe": {
          "type": "http",
          "url": "https://mcp.stripe.com"
        }
      }
    }
  }
}
```

###### 项目范围

项目范围的服务器通过在项目根目录中存储配置在 `.mcp.json` 文件中来启用团队协作。此文件设计为检入版本控制，确保所有团队成员都可以访问相同的 MCP 工具和服务。添加项目范围的服务器时，Claude Code 会自动创建或更新此文件，使用适当的配置结构。

```sh
# 添加项目范围的服务器
claude mcp add --transport http paypal --scope project https://mcp.paypal.com/mcp
```

生成的 `.mcp.json` 文件遵循标准化格式：

```json
{
  "mcpServers": {
    "shared-server": {
      "command": "/path/to/server",
      "args": [],
      "env": {}
    }
  }
}
```

出于安全原因，Claude Code 在使用来自 `.mcp.json` 文件的项目范围的服务器之前会提示批准。如果您需要重置这些批准选择，请使用 `claude mcp reset-project-choices` 命令。

###### 用户范围

用户范围的服务器存储在 `~/.claude.json` 中，并提供跨项目可访问性，使其在您机器上的所有项目中可用，同时对您的用户帐户保持私密。此范围适用于个人实用程序服务器、开发工具或您在不同项目中经常使用的服务。

```sh
# 添加用户服务器
claude mcp add --transport http hubspot --scope user https://mcp.hubspot.com/anthropic
```

###### 范围层次结构和优先级

当具有相同名称的服务器在多个位置定义时，Claude Code 连接到它一次，使用来自最高优先级源的定义：

1. 本地范围
2. 项目范围
3. 用户范围
4. [插件提供的服务器](https://code.claude.com/docs/zh-CN/plugins)
5. [claude.ai 连接器](https://code.claude.com/docs/zh-CN/mcp#use-mcp-servers-from-claude-ai)

三个范围按名称匹配重复项。插件和连接器按端点匹配，因此指向与上述服务器相同的 URL 或命令的连接器被视为重复项。

###### `.mcp.json` 中的环境变量扩展

Claude Code 支持 `.mcp.json` 文件中的环境变量扩展，允许团队共享配置，同时为特定于机器的路径和 API 密钥等敏感值保持灵活性。

**支持的语法：**

- `${VAR}` - 扩展为环境变量 `VAR` 的值
- `${VAR:-default}` - 如果设置了 `VAR`，则扩展为 `VAR`；否则使用（默认值） `default`

**扩展位置：** <font color=dodgerBlue>环境变量可以在以下位置扩展</font>：

- `command` - 服务器可执行文件路径
- `args` - 命令行参数
- `env` - 传递给服务器的环境变量
- `url` - 对于 HTTP 服务器类型
- `headers` - 对于 HTTP 服务器身份验证

**带有变量扩展的示例：**

```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}"
      }
    }
  }
}
```

如果未设置所需的环境变量且没有默认值，Claude Code 将无法解析配置。

##### 从 JSON 配置添加 MCP 服务器

如果您有 MCP 服务器的 JSON 配置，您可以直接添加它：

1. **从 JSON 添加 MCP 服务器**

   ```sh
   # 基本语法
   claude mcp add-json <name> '<json>'
   
   # 示例：添加带有 JSON 配置的 HTTP 服务器
   claude mcp add-json weather-api '{"type":"http","url":"https://api.weather.com/mcp","headers":{"Authorization":"Bearer token"}}'
   
   # 示例：添加带有 JSON 配置的 stdio 服务器
   claude mcp add-json local-weather '{"type":"stdio","command":"/path/to/weather-cli","args":["--api-key","abc123"],"env":{"CACHE_DIR":"/tmp"}}'
   
   # 示例：添加带有预配置 OAuth 凭据的 HTTP 服务器
   claude mcp add-json my-server '{"type":"http","url":"https://mcp.example.com/mcp","oauth":{"clientId":"your-client-id","callbackPort":8080}}' --client-secret
   ```

2. **验证服务器已添加**

   ```sh
   claude mcp get weather-api
   ```

> [!TIP]
>
> 提示：
>
> - 确保 JSON 在您的 shell 中正确转义
> - JSON 必须符合 MCP 服务器配置架构
> - 您可以使用 `--scope user` 将服务器添加到您的用户配置而不是项目特定的配置

##### 响应 MCP 引发请求

> [!NOTE]
>
> 原文是：Respond to MCP **elicitation** requests

MCP 服务器可以在任务中途使用 **引发**（ elicitation ）来请求您的结构化输入。当服务器需要无法自行获取的信息时，Claude Code 会显示交互式对话框并将您的响应传递回服务器。您无需进行任何配置：当服务器请求时，引发对话框会自动出现。<font color=dodgerBlue>服务器可以通过两种方式请求输入</font>：

- **表单模式**：Claude Code 显示一个对话框，其中包含服务器定义的表单字段（例如，用户名和密码提示）。填写字段并提交。
- **URL 模式**：Claude Code 打开浏览器 URL 以进行身份验证或批准。在浏览器中完成流程，然后在 CLI 中确认。

要自动响应引发请求而不显示对话框，请使用 [`Elicitation` hook](https://code.claude.com/docs/zh-CN/hooks#Elicitation)。如果您正在构建使用引发的 MCP 服务器，请参阅 [MCP 引发规范](https://modelcontextprotocol.io/docs/learn/client-concepts#elicitation)以了解协议详细信息和架构示例。

##### 使用 MCP 资源

MCP 服务器可以公开资源，您可以使用 `@` 提及来引用，类似于您引用文件的方式。

###### 引用 MCP 资源

1. **列出可用资源**

   <font color=red>在您的提示中键入 `@` 以查看来自所有连接的 MCP 服务器的可用资源</font>。资源与文件一起出现在自动完成菜单中。

2. **引用特定资源**

   <font color=red>**使用格式 `@server:protocol://resource/path` 来引用资源**</font>：

   ```md
   Can you analyze @github:issue://123 and suggest a fix?
   ```

   ```md
   Please review the API documentation at @docs:file://api/authentication
   ```

3. **多个资源引用**

   您可以在单个提示中引用多个资源：

   ```md
   Compare @postgres:schema://users with @docs:file://database/user-model
   ```

> [!TIP]
>
> 提示：
>
> - 资源在引用时会自动获取并作为附件包含
> - 资源路径在 `@` 提及自动完成中可进行模糊搜索
> - Claude Code 在服务器支持时自动提供列出和读取 MCP 资源的工具
> - 资源可以包含 MCP 服务器提供的任何类型的内容（文本、JSON、结构化数据等）

##### 使用 MCP 工具搜索进行扩展

工具搜索 ( Tool Search ) 通过延迟工具定义直到 Claude 需要它们来保持 MCP 上下文使用低。仅工具名称在会话启动时加载，因此添加更多 MCP 服务器对您的上下文窗口的影响最小。

###### 工作原理

<font color=red>工具搜索**默认启用**</font>。<font color=red>MCP 工具被延迟而不是预先加载到上下文中</font>，Claude 使用搜索工具在任务需要时发现相关的工具。<font color=red>仅 Claude 实际使用的工具进入上下文</font>。从您的角度来看，MCP 工具的工作方式与之前完全相同。<font color=lightSeaGreen>如果您更喜欢基于阈值的加载，请设置 `ENABLE_TOOL_SEARCH=auto` 以在工具适合上下文窗口的 10% 内时预先加载架构，仅延迟溢出部分</font>。有关所有选项，请参阅[配置工具搜索](https://code.claude.com/docs/zh-CN/mcp#configure-tool-search)。

##### 将 MCP 提示用作命令

MCP 服务器可以公开在 Claude Code 中作为命令可用的提示。

###### 执行 MCP 提示

1. **发现可用的提示**

   键入 `/` 以查看所有可用的命令，包括来自 MCP 服务器的命令。MCP 提示以 `/mcp__servername__promptname` 的格式出现。

2. **执行不带参数的提示**

   ```md
   /mcp__github__list_prs
   ```

3. **执行带参数的提示**

   许多提示接受参数。在命令后面用空格分隔传递它们：

   ```md
   /mcp__github__pr_review 456
   ```

   ```md
   /mcp__jira__create_issue "Bug in login flow" high
   ```

> [!TIP]
>
> 提示：
>
> - MCP 提示从连接的服务器动态发现
> - 参数根据提示的定义参数进行解析
> - 提示结果直接注入到对话中
> - 服务器和提示名称被规范化（空格变为下划线）



#### 通过市场发现和安装预构建插件

##### 官方 Anthropic 市场

官方 Anthropic 市场（`claude-plugins-official`）在您启动 Claude Code 时自动可用。运行 `/plugin` 并转到 **发现** 选项卡 以浏览可用内容，或在 [claude.com/plugins](https://claude.com/plugins) 查看目录。

要从官方市场安装插件，请使用 `/plugin install <name>@claude-plugins-official`。例如，要安装 GitHub 集成：

```md
/plugin install github@claude-plugins-official
```

##### 添加市场

使用 `/plugin marketplace add` 命令从不同来源添加市场。

> [!TIP]
>
> **快捷方式**：您可以使用 `/plugin market` 代替 `/plugin marketplace`，以及使用 `rm` 代替 `remove`。

- **GitHub 存储库**：`owner/repo` 格式（例如，`anthropics/claude-code`）
- **Git URL**：任何 git 存储库 URL（GitLab、Bitbucket、自托管）
- **本地路径**：目录或 `marketplace.json` 文件的直接路径
- **远程 URL**：托管 `marketplace.json` 文件的直接 URL

######  从 GitHub 添加

使用 `owner/repo` 格式添加包含 `.claude-plugin/marketplace.json` 文件的 GitHub 存储库，其中 `owner` 是 GitHub 用户名或组织，`repo` 是存储库名称。

例如，`anthropics/claude-code` 指的是由 `anthropics` 拥有的 `claude-code` 存储库：

```md
/plugin marketplace add anthropics/claude-code
```

###### 从其他 Git 主机添加

通过提供完整 URL 添加任何 git 存储库。这适用于任何 Git 主机，包括 GitLab、Bitbucket 和自托管服务器：使用 HTTPS：

```md
/plugin marketplace add https://gitlab.com/company/plugins.git
```

> [!NOTE]
>
> 相当于 GitHub 是默认域名

使用 SSH：

```md
/plugin marketplace add git@gitlab.com:company/plugins.git
```

<font color=dodgerBlue>要添加特定分支或标签，请在 `#` 后附加 ref</font>：

```md
/plugin marketplace add https://gitlab.com/company/plugins.git#v1.0.0
```

###### 从本地路径添加

添加包含 `.claude-plugin/marketplace.json` 文件的本地目录：

```md
/plugin marketplace add ./my-marketplace
```

您也可以添加 `marketplace.json` 文件的直接路径：

```md
/plugin marketplace add ./path/to/marketplace.json
```

###### 从远程 URL 添加

通过 URL 添加远程 `marketplace.json` 文件：

```md
/plugin marketplace add https://example.com/marketplace.json
```

##### 安装插件

添加市场后，您可以直接安装插件（默认安装到用户范围）：

```md
/plugin install plugin-name@marketplace-name
```

要选择不同的[安装范围](https://code.claude.com/docs/zh-CN/settings#configuration-scopes)，请使用交互式 UI：运行 `/plugin`，转到**发现**选项卡，然后在插件上按 **Enter**。您将看到以下选项：

- **用户范围**（默认）：在所有项目中为自己安装
- **项目范围**：为此存储库上的所有协作者安装（添加到 `.claude/settings.json`）
- **本地范围**：仅在此存储库中为自己安装（不与协作者共享）

您也可能看到具有**托管**范围的插件——这些由管理员通过[托管设置](https://code.claude.com/docs/zh-CN/settings#settings-files)安装，无法修改。运行 `/plugin` 并转到**已安装**选项卡以查看按范围分组的插件。

##### 管理已安装的插件

运行 `/plugin` 并转到**已安装**选项卡以查看、启用、禁用或卸载您的插件。键入以按插件名称或描述筛选列表。您也可以使用直接命令管理插件。<font color=red>禁用插件</font>而不卸载：

```md
/plugin disable plugin-name@marketplace-name
```

<font color=red>重新启用</font>已禁用的插件：

```md
/plugin enable plugin-name@marketplace-name
```

完全<font color=red>删除</font>插件：

```md
/plugin uninstall plugin-name@marketplace-name
```

<font color=red>`--scope` 选项允许您使用 CLI 命令针对特定范围</font>：

```md
claude plugin install formatter@your-org --scope project
claude plugin uninstall formatter@your-org --scope project
```

##### 应用插件更改而不重启

当您在会话期间安装、启用或禁用插件时，运行 `/reload-plugins` 以在不重启的情况下获取所有更改：

```md
/reload-plugins
```

Claude Code 重新加载所有活跃插件，并显示重新加载的命令、skills、agents、hooks、插件 MCP servers 和插件 LSP servers 的计数。

##### 管理市场

您可以通过交互式 `/plugin` 界面或 CLI 命令管理市场。

###### 使用交互式界面

运行 `/plugin` 并转到**市场**选项卡以：

- 查看所有已添加的市场及其来源和状态
- 添加新市场
- 更新市场列表以获取最新插件
- 删除您不再需要的市场

###### 使用 CLI 命令

您也可以使用直接命令管理市场。列出所有配置的市场：

```md
/plugin marketplace list
```

刷新市场的插件列表：

```md
/plugin marketplace update marketplace-name
```

删除市场：

```md
/plugin marketplace remove marketplace-name
```

> [!warning]
>
> 删除市场将卸载您从中安装的任何插件。



#### 创建插件

##### 何时使用插件与独立配置

<font color=dodgerBlue>Claude Code **支持两种方式**来添加自定义 skills、agents 和 hooks</font>：

| 方法                                                 | Skill 名称           | 最适合                                             |
| :--------------------------------------------------- | :------------------- | :------------------------------------------------- |
| **独立**（`.claude/` 目录）                          | `/hello`             | 个人工作流、项目特定的自定义、快速实验             |
| **插件**（包含 `.claude-plugin/plugin.json` 的目录） | `/plugin-name:hello` | 与团队成员共享、分发到社区、版本化发布、跨项目重用 |

###### 在以下情况下使用独立配置

- 你正在为单个项目自定义 Claude Code
- 配置是个人的，不需要共享
- 你在打包 skills 或 hooks 之前进行实验
- 你想要简短的 skill 名称，如 `/hello` 或 `/deploy`

###### 在以下情况下使用插件

- 你想与团队或社区共享功能
- 你需要在多个项目中使用相同的 skills/agents
- 你想要版本控制和轻松更新扩展
- 你通过市场分发
- 你 <font color=red>可以接受命名空间化的 skills，如 `/my-plugin:hello`</font>（命名空间可防止插件之间的冲突）

> [!TIP]
>
> 从 `.claude/` 中的独立配置开始进行快速迭代，然后在准备好共享时[转换为插件](https://code.claude.com/docs/zh-CN/plugins#convert-existing-configurations-to-plugins)。

#####  快速开始

###### 创建你的第一个插件

1. **创建插件目录**

   每个插件都位于其自己的目录中，包含清单和你的 skills、agents 或 hooks。现在创建一个：

   ```sh
   mkdir my-first-plugin
   ```

2. **创建插件清单**

   位于 `.claude-plugin/plugin.json` 的清单文件定义了你的插件的身份：其名称、描述和版本。Claude Code 使用此元数据在插件管理器中显示你的插件。

   在你的插件文件夹内创建 `.claude-plugin` 目录：

   ```sh
   mkdir my-first-plugin/.claude-plugin
   ```

   然后使用以下内容创建 `my-first-plugin/.claude-plugin/plugin.json`：

   > [!NOTE]
   >
   > 感觉和 package.json 没什么区别

   ```json
   {
     "name": "my-first-plugin",
     "description": "A greeting plugin to learn the basics",
     "version": "1.0.0",
     "author": {
       "name": "Your Name"
     }
   }
   ```

   | 字段          | 目的                                                         |
   | :------------ | :----------------------------------------------------------- |
   | `name`        | 唯一标识符和 skill 命名空间。Skills 以此为前缀（例如 `/my-first-plugin:hello`）。 |
   | `description` | 在浏览或安装插件时在插件管理器中显示。                       |
   | `version`     | <font color=lightSeaGreen>可选</font>。如果设置，用户仅在你更新此字段时接收更新。如果省略且你的插件通过 git 分发，则使用提交 SHA，每个提交都计为新版本。请参阅[版本管理](https://code.claude.com/docs/zh-CN/plugins-reference#version-management)。 |
   | `author`      | 可选。有助于归属。                                           |

   有关 `homepage`、`repository` 和 `license` 等其他字段，请参阅[完整清单架构](https://code.claude.com/docs/zh-CN/plugins-reference#plugin-manifest-schema)。

3. **添加 skill**

   Skills 位于 `skills/` 目录中。<font color=red>每个 skill 是一个包含 `SKILL.md` 文件的文件夹</font>。文件夹名称成为 skill 名称，以插件的命名空间为前缀（在名为 `my-first-plugin` 的插件中的 `hello/` 创建 `/my-first-plugin:hello`）。在你的插件文件夹中创建一个 skill 目录：

   ```sh
   mkdir -p my-first-plugin/skills/hello
   ```

   然后使用以下内容创建 `my-first-plugin/skills/hello/SKILL.md`：

   ```md
   ---
   description: Greet the user with a friendly message
   disable-model-invocation: true
   ---
   
   Greet the user warmly and ask how you can help them today.
   ```

4. **测试你的插件**

   使用 `--plugin-dir` 标志运行 Claude Code 以加载你的插件：

   > [!NOTE]
   >
   > 关于 `--plugin-dir` 可以看下 [[#在本地测试你的插件]]

   ```sh
   claude --plugin-dir ./my-first-plugin
   ```

   Claude Code 启动后，尝试你的新 skill：

   ```md
   /my-first-plugin:hello
   ```

   你将看到 Claude 用问候语回应。运行 `/help` 以查看你的 skill 在插件命名空间下列出。

   > [!TIP]
   >
   > **为什么要命名空间？** 插件 skills 总是命名空间化的（如 `/my-first-plugin:hello`），以防止多个插件具有相同名称的 skills 时发生冲突。要更改命名空间前缀，请更新 `plugin.json` 中的 `name` 字段。

5. **添加 skill 参数**

   通过接受用户输入使你的 skill 动态化。`$ARGUMENTS` 占位符捕获用户在 skill 名称后提供的任何文本。更新你的 `SKILL.md` 文件：

   ```md
   ---
   description: Greet the user with a personalized message
   ---
   
   # Hello Skill
   
   Greet the user named "$ARGUMENTS" warmly and ask how you can help them today. Make the greeting personal and encouraging.
   ```

   运行 `/reload-plugins` 以获取更改，然后尝试使用你的名字的 skill：

   ```
   /my-first-plugin:hello Alex
   ```

   Claude 将按名字问候你。有关向 skills 传递参数的更多信息，请参阅 [Skills](https://code.claude.com/docs/zh-CN/skills#pass-arguments-to-skills)。

<font color=dodgerBlue>你已成功创建并测试了一个包含以下关键组件的插件</font>：

- **插件清单**（`.claude-plugin/plugin.json`）：描述你的插件的元数据
- **Skills 目录**（`skills/`）：包含你的自定义 skills
- **Skill 参数**（`$ARGUMENTS`）：捕获用户输入以实现动态行为

> [!TIP]
>
> `--plugin-dir` 标志对开发和测试很有用。当你准备好与他人共享你的插件时，请参阅[创建和分发插件市场](https://code.claude.com/docs/zh-CN/plugin-marketplaces)

##### 插件结构概览

你已创建了一个带有 skill 的插件，但 <font color=dodgerBlue>插件可以包含更多内容</font>：<font color=fuchsia>**自定义 agents、hooks、MCP servers、LSP servers 和后台监视器**</font>。

> [!warning]
>
> **常见错误**：<font color=red>不要将 `commands/`、`agents/`、`skills/` 或 `hooks/` 放在 `.claude-plugin/` 目录内</font>。<font color=red>**只有 `plugin.json` 应该在 `.claude-plugin/` 内**</font>。所有其他目录必须在插件根级别。

| 目录              | 位置   | 目的                                                         |
| :---------------- | :----- | :----------------------------------------------------------- |
| `.claude-plugin/` | 插件根 | 包含 `plugin.json` 清单（如果组件使用默认位置，则可选）      |
| `skills/`         | 插件根 | <font color=red>Skills 作为 `<name>/SKILL.md` 目录</font>    |
| `commands/`       | 插件根 | Skills 作为平面 Markdown 文件。为新插件使用 `skills/`        |
| `agents/`         | 插件根 | 自定义 agent 定义                                            |
| `hooks/`          | 插件根 | `hooks.json` 中的事件处理程序                                |
| `.mcp.json`       | 插件根 | MCP server 配置                                              |
| `.lsp.json`       | 插件根 | 用于代码智能的 LSP server 配置                               |
| `monitors/`       | 插件根 | `monitors.json` 中的后台监视器配置                           |
| `bin/`            | 插件根 | 在启用插件时添加到 Bash tool 的 `PATH` 的可执行文件          |
| `settings.json`   | 插件根 | 启用插件时应用的默认[设置](https://code.claude.com/docs/zh-CN/settings) |

##### 开发更复杂的插件

一旦你对基本插件感到满意，你可以创建更复杂的扩展。

###### 向你的插件添加 Skills

插件可以包含 [Agent Skills](https://code.claude.com/docs/zh-CN/skills) 以扩展 Claude 的功能。Skills 是模型调用的：Claude 根据任务上下文自动使用它们。在你的插件根目录添加一个 `skills/` 目录，其中包含包含 `SKILL.md` 文件的 Skill 文件夹：

```txt
my-plugin/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    └── code-review/
        └── SKILL.md
```

每个 `SKILL.md` 包含 YAML frontmatter 和说明。<font color=red>包含一个 `description`，以便 Claude 知道何时使用该 skill</font>：

```md
---
description: Reviews code for best practices and potential issues. Use when reviewing code, checking PRs, or analyzing code quality.
---

When reviewing code, check for:
1. Code organization and structure
2. Error handling
3. Security concerns
4. Test coverage
```

###### 向你的插件添加 LSP servers

> [!TIP]
>
> <font color=red>对于 TypeScript、Python 和 Rust 等常见语言，请从官方市场安装预构建的 LSP 插件</font>。仅当你需要支持尚未涵盖的语言时，才创建自定义 LSP 插件。

LSP（Language Server Protocol）插件为 Claude 提供实时代码智能。如果你需要支持没有官方 LSP 插件的语言，你可以通过向你的插件 <font color=red>添加 `.lsp.json` 文件</font> 来创建自己的：

```json
{
  "go": {
    "command": "gopls",
    "args": ["serve"],
    "extensionToLanguage": {
      ".go": "go"
    }
  }
}
```

安装你的插件的用户必须在其机器上安装语言服务器二进制文件。

有关完整的 LSP 配置选项，请参阅 [LSP servers](https://code.claude.com/docs/zh-CN/plugins-reference#lsp-servers)。

###### 向你的插件添加后台监视器

<font color=red>后台监视器让你的插件在后台监视日志、文件或外部状态，并 **在事件到达时通知 Claude**</font>。<font color=red>Claude Code 在插件处于活动状态时自动启动每个监视器</font>，因此你无需指示 Claude 启动监视。

在插件根目录添加一个 `monitors/monitors.json` 文件，其中包含监视器条目数组：

```json
[
  {
    "name": "error-log",
    "command": "tail -F ./logs/error.log",
    "description": "Application error log"
  }
]
```

来自 `command` 的每个 stdout 行( stdout line )在会话期间作为通知传递给 Claude。有关完整的架构，包括 `when` 触发器和变量替换，请参阅 [Monitors](https://code.claude.com/docs/zh-CN/plugins-reference#monitors)。

###### 使用你的插件提供默认设置

插件可以在插件根目录包含一个 `settings.json` 文件，以 <font color=red>在启用插件时应用默认配置</font>。目前仅支持 `agent` 和 `subagentStatusLine` 键。

设置 `agent` 激活插件的[自定义 agents](https://code.claude.com/docs/zh-CN/sub-agents) 之一作为主线程，应用其系统提示、工具限制和模型。这让插件在启用时通过改变 Claude Code 的默认行为方式。

```json
{
  "agent": "security-reviewer"
}
```

此示例激活在插件的 `agents/` 目录中定义的 `security-reviewer` agent。<font color=red>来自 `settings.json` 的设置优先于在 `plugin.json` 中声明的 `settings`</font>。未知键被静默忽略。

###### 组织复杂的插件

对于具有许多组件的插件，按功能组织你的目录结构。有关完整的目录布局和组织模式，请参阅 [Plugin directory structure](https://code.claude.com/docs/zh-CN/plugins-reference#plugin-directory-structure)。

###### 在本地测试你的插件

<font color=red>使用 `--plugin-dir` 标志在开发期间测试插件。这会直接加载你的插件，无需安装</font>。

```sh
claude --plugin-dir ./my-plugin
```

<font color=red>当 `--plugin-dir` 插件与已安装的市场插件同名时，本地副本在该会话中优先</font>。这让你可以测试已安装的插件的更改，而无需先卸载它。由托管设置强制启用的市场插件是唯一的例外，无法被覆盖。

当你对插件进行更改时，运行 `/reload-plugins` 以获取更新，无需重新启动。这会重新加载 plugins、skills、agents、hooks、插件 MCP servers 和插件 LSP servers。测试你的插件组件：

- 使用 `/plugin-name:skill-name` 尝试你的 skills
- 检查 agents 是否出现在 `/agents` 中
- 验证 hooks 是否按预期工作

> [!TIP]
>
> 你可以通过多次指定标志来一次加载多个插件：
>
> ```sh
> claude --plugin-dir ./plugin-one --plugin-dir ./plugin-two
> ```

###### 共享你的插件

当你的插件准备好共享时：

1. **添加文档**：包含一个 `README.md`，其中包含安装和使用说明
2. **选择版本控制策略**：决定是设置显式 `version` 还是依赖 git 提交 SHA。请参阅 [version management](https://code.claude.com/docs/zh-CN/plugins-reference#version-management)
3. **创建或使用市场**：通过 [plugin marketplaces](https://code.claude.com/docs/zh-CN/plugin-marketplaces) 分发以供安装
4. **与他人测试**：在更广泛分发之前让团队成员测试插件



#### 使用 skills 扩展 Claude

Claude Code skills 遵循 [Agent Skills](https://agentskills.io/) 开放标准，该标准适用于多个 AI 工具。Claude Code 使用额外功能扩展了该标准，如[调用控制](https://code.claude.com/docs/zh-CN/skills#control-who-invokes-a-skill)、[subagent 执行](https://code.claude.com/docs/zh-CN/skills#run-skills-in-a-subagent)和[动态上下文注入](https://code.claude.com/docs/zh-CN/skills#inject-dynamic-context)。

##### 捆绑 skills

Claude Code 包括一组捆绑 skills，在每个会话中都可用，包括 `/simplify`、`/batch`、`/debug`、`/loop` 和 `/claude-api`。<font color=dodgerBlue>**与大多数内置命令不同**</font>，<font color=fuchsia>内置命令 **直接执行固定逻辑**，捆绑 skills 是基于提示的：它们为 Claude 提供详细的剧本，让它使用其工具来编排工作</font>。你调用捆绑 skills 的方式与调用任何其他 skill 相同，输入 `/` 后跟 skill 名称。

> [!NOTE]
>
> 所以说：“捆绑 skills” ( Bundled skills ) 和 “内置命令” 是两个东西

捆绑 skills 在[命令参考](https://code.claude.com/docs/zh-CN/commands)中与内置命令一起列出，在”目的”列中标记为 **Skill**。

##### 入门

###### 创建你的第一个 skill

此示例创建一个 skill，教 Claude 使用视觉图表和类比来解释代码。由于它使用默认 frontmatter，Claude 可以在你询问某事如何工作时自动加载它，或者你可以使用 `/explain-code` 直接调用它。

1. **创建 skill 目录**

   在你的个人 skills 文件夹中为 skill 创建一个目录。个人 skills 在你的所有项目中都可用。

   ```sh
   mkdir -p ~/.claude/skills/explain-code
   ```

2. **编写 SKILL.md**

   每个 skill 都需要一个 `SKILL.md` 文件，<font color=dodgerBlue>包含两部分</font>：YAML frontmatter（在 `---` 标记之间）告诉 Claude 何时使用该 skill，以及包含 Claude 在调用该 skill 时遵循的说明的 markdown 内容。`name` 字段变成 `/slash-command`，`description` 帮助 Claude 决定何时自动加载它。

   创建 `~/.claude/skills/explain-code/SKILL.md`：

   ```md
   ---
   name: explain-code
   description: Explains code with visual diagrams and analogies. Use when explaining how code works, teaching about a codebase, or when the user asks "how does this work?"
   ---
   
   When explaining code, always include:
   
   1. **Start with an analogy**: Compare the code to something from everyday life
   2. **Draw a diagram**: Use ASCII art to show the flow, structure, or relationships
   3. **Walk through the code**: Explain step-by-step what happens
   4. **Highlight a gotcha**: What's a common mistake or misconception?
   
   Keep explanations conversational. For complex concepts, use multiple analogies.
   ```

3. **测试 skill**

   <font color=dodgerBlue>你可以通过两种方式测试它</font>：

   **让 Claude 自动调用它**，通过询问与描述匹配的内容：
   
   ```md
   How does this code work?
   ```

   **或直接使用 skill 名称调用它**：
   
   ```md
   /explain-code src/auth/login.ts
   ```
   
   无论哪种方式，Claude 都应该在其解释中包含类比和 ASCII 图表。
   
   > [!NOTE]
   >
   > 因为 skill.md 中有提及 “ Compare the code to something” 和 “Use ASCII art to show”

###### Skills 的位置

你存储 skill 的位置决定了谁可以使用它：

| 位置 | 路径                                                         | 适用于               |
| :--- | :----------------------------------------------------------- | :------------------- |
| 企业 | 请参阅[托管设置](https://code.claude.com/docs/zh-CN/settings#settings-files) | 你的组织中的所有用户 |
| 个人 | `~/.claude/skills/<skill-name>/SKILL.md`                     | 你的所有项目         |
| 项目 | `.claude/skills/<skill-name>/SKILL.md`                       | 仅此项目             |
| 插件 | `<plugin>/skills/<skill-name>/SKILL.md`                      | 启用插件的位置       |

<font color=dodgerBlue>当 skills 在各个级别共享相同的名称时</font>，<font color=red>**企业覆盖个人**，**个人覆盖项目**</font>。插件 skills 使用 `plugin-name:skill-name` 命名空间，因此它们不能与其他级别冲突。如果你在 `.claude/commands/` 中有文件，它们的工作方式相同，但如果 skill 和命令共享相同的名称，skill 优先。

- **实时变更检测**

  Claude Code 监视 skill 目录的文件变更。

  - 在 `~/.claude/skills/`、项目 `.claude/skills/` 或 `--add-dir` 目录内的 `.claude/skills/` 中 <font color=red>添加、编辑或删除 skill</font> 会在当前会话中生效，<font color=red>无需重新启动</font>。


  - 创建在会话启动时 <font color=red>***不存在的顶级 skills 目录***</font> <font color=red>需要重新启动 Claude Code</font>，以便可以监视新目录。


- **从嵌套目录自动发现**

  当你在子目录中处理文件时，Claude Code 会自动从嵌套的 `.claude/skills/` 目录中发现 skills。<font color=dodgerBlue>例如</font>，<font color=red>如果你正在编辑 `packages/frontend/` 中的文件，Claude Code 也会在 `packages/frontend/.claude/skills/` 中查找 skills</font>。这支持 monorepo 设置，其中包有自己的 skills。

  <font color=dodgerBlue>**每个 skill 都是一个以 `SKILL.md` 作为入口点的目录：**</font>

  ```txt
  my-skill/
  ├── SKILL.md           # 主要说明（必需）
  ├── template.md        # Claude 要填写的模板
  ├── examples/
  │   └── sample.md      # 显示预期格式的示例输出
  └── scripts/
      └── validate.sh    # Claude 可以执行的脚本
  ```

  `SKILL.md` 包含主要说明，是必需的。其他文件是可选的，让你构建更强大的 skills：Claude 要填写的模板、显示预期格式的示例输出、Claude 可以执行的脚本或详细的参考文档。从你的 `SKILL.md` 中引用支持文件，以便 Claude 知道每个文件包含什么以及何时加载它。有关更多详细信息，请参阅[添加支持文件](https://code.claude.com/docs/zh-CN/skills#add-supporting-files)。

- 来自其他目录的 skills

  <font color=red>`--add-dir` 标志 [授予文件访问权限](https://code.claude.com/docs/zh-CN/permissions#additional-directories-grant-file-access-not-configuration) 而不是配置发现</font>，但 <font color=red>skills 是一个例外</font>：添加目录中的 `.claude/skills/` 会自动加载。请参阅 [实时变更检测](https://code.claude.com/docs/zh-CN/skills#live-change-detection) 了解编辑如何在会话期间被拾取。

  其他 `.claude/` 配置（如 subagents、命令和输出样式）不会从其他目录加载。有关加载和不加载的完整列表以及跨项目共享配置的推荐方式，请参阅[例外表](https://code.claude.com/docs/zh-CN/permissions#additional-directories-grant-file-access-not-configuration)。

  > [!caution]
  >
  > 来自 `--add-dir` 目录的 CLAUDE.md 文件默认不加载。要加载它们，请设置 `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1`。请参阅[从其他目录加载](https://code.claude.com/docs/zh-CN/memory#load-from-additional-directories)。

##### 配置 skills

###### 添加支持文件

Skills 可以在其目录中包含多个文件。这使 `SKILL.md` 专注于要点，同时让 Claude 仅在需要时访问详细的参考资料。大型参考文档、API 规范或示例集合不需要在每次 skill 运行时加载到上下文中。

```txt
my-skill/
├── SKILL.md (required - overview and navigation)
├── reference.md (detailed API docs - loaded when needed)
├── examples.md (usage examples - loaded when needed)
└── scripts/
    └── helper.py (utility script - executed, not loaded)
```

从 `SKILL.md` 中引用支持文件，以便 Claude 知道每个文件包含什么以及何时加载它：

```md
## Additional resources

- For complete API details, see [reference.md](reference.md)
- For usage examples, see [examples.md](examples.md)
```

> [!TIP]
>
> 将 `SKILL.md` 保持在 500 行以下。将详细的参考资料移到单独的文件中。

###### 控制谁调用 skill

默认情况下，你和 Claude 都可以调用任何 skill。你可以输入 `/skill-name` 直接调用它，Claude 可以在与你的对话相关时自动加载它。两个 frontmatter 字段让你限制这一点：

- **`disable-model-invocation: true`**：只有你可以调用该 skill。<font color=red>用于有副作用的工作流或你想控制时间的工作流，如 `/commit`、`/deploy` 或 `/send-slack-message`</font>。你不希望 Claude 因为你的代码看起来准备好了就决定部署。

  > [!NOTE]
  >
  > 即：只能手动调用，不能自动触发

- **`user-invocable: false`**：<font color=red>只有 Claude 可以调用该 skill</font>。用于不可作为命令操作的背景知识。`legacy-system-context` skill 解释了旧系统的工作原理。Claude 在相关时应该知道这一点，但 `/legacy-system-context` 对用户来说不是一个有意义的操作。

###### 为 skill 预先批准工具

`allowed-tools` 字段<font color=red>在 skill 处于活动状态时授予对列出的工具的权限</font>，因此 <font color=red>Claude 可以使用它们而无需提示你获得批准</font>。它不限制哪些工具可用：每个工具仍然可调用，你的 [权限设置](https://code.claude.com/docs/zh-CN/permissions) 仍然管理不在列表中的工具。

此 skill 让 Claude 在你调用它时运行 git 命令而无需每次使用批准：

```md
---
name: commit
description: Stage and commit the current changes
disable-model-invocation: true
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
```

要阻止 skill 使用某些工具，请在你的[权限设置](https://code.claude.com/docs/zh-CN/permissions)中添加拒绝规则。




### 其他笔记

#### 《CLAUDE.md 写不好，效率至少下降一半》笔记

##### CLAUDE.md 是什么

CLAUDE.md 是一个 Markdown 文件，Claude Code 在每次会话开始的时候会自动读取它。

CLAUDE.md 是专门写给 Claude 看的——告诉它你的项目长什么样、代码风格是什么、常用命令有哪些、有哪些坑需要注意。

**核心逻辑就一句话：Claude 每次启动都是失忆的，CLAUDE.md 是唯一能让它"记住"你的东西。**

Claude 团队会把 CLAUDE.md 提交到 git 里，每次 Claude 犯了错就加一条，相当于一个不断进化的"错题本"。

##### 四个存放的位置

1. **全局级别** ：`~/.claude/CLAUDE.md`。

2. **项目根目录** ：`./CLAUDE.md`。

3. **子目录** ：`./src/api/CLAUDE.md`。适合 monorepo 场景。比如前端目录有自己的规范，后端目录有自己的规范，可以各自维护。

4. **本地私有** ：`CLAUDE.local.md`。个人偏好，加到 `.gitignore` 里，不提交。

这四个层级是可以叠加的。Claude 会按顺序全部读取，从全局到具体。

##### 编写 CLAUDE.md 注意点

###### 别写得太长

研究表明，前沿大模型大概能可靠地遵循 150-200 条指令。<font color=lightSeaGreen>Claude Code 自身的系统提示已经占了大约 50 条，留给你的空间也就 100-150 条</font>。超了之后，**所有指令的执行质量都会下降**——不是新加的不管用，是连之前写的也开始不稳了。

**经验：控制在 300 行以内。**

而且有一个很多人不知道的细节：Claude Code 的系统提示里其实有一行字，大意是 “CLAUDE.md 的内容可能与当前任务相关也可能不相关” 。也就是说，**Claude 会主动过滤它认为跟当前任务无关的内容**。写得太多太杂，不但浪费 token，还可能让真正重要的规则被"过滤"掉。

###### 别把它当 Linter 用

"用 2 个空格缩进"、"行尾不要分号"、"import 按字母排序"……

说实话，这是在用大炮打蚊子。代码格式化这种事，ESLint、Prettier、Biome 这些工具干起来又快又准，而且是确定性的。LLM 做这种事既贵又慢，还不一定靠谱。

**正确的做法是：让 Claude 写代码，linter 自动格式化，Claude 根据 linter 的报错来修正。** 用 hooks 把这个流程自动化，比写一堆代码风格规则管用多了。

###### 别嵌入大段代码或文档

有人在 CLAUDE.md 里用 `@` 引用了好几个大文档，结果每次启动的时候这些文件全部被嵌入到上下文里，几千甚至上万 token 就这么没了。

更好的做法是：在 CLAUDE.md 里只写一句指引，告诉 Claude **什么时候去读哪个文件**。

> [!TIP]
> 即：渐进式披露 ( progressive disclosure )

```md
# 参考文档  
  
- 遇到认证相关的问题，先看 @docs/auth-patterns.md  
- 数据库操作的规范在 @docs/database-conventions.md  
- API 设计的约定参考 @docs/api-guidelines.md
```

这样 Claude 只在需要的时候才去读，不会每次都加载所有内容。核心思路就是：不要一股脑把所有信息都塞给它，告诉它去哪找就行了。

###### 不要只写“不要做什么”

"不要用 --force 参数"、"不要直接操作数据库"、"不要修改 migrations 目录"……

光说不要做什么，不说应该做什么，Claude 有时候会卡住——它觉得必须用某个方法，但你又不让它用，它就不知道该怎么办了。

**每个"不要"后面都应该跟一个"应该"。** 比如："不要用 --force 推送，应该用 --force-with-lease" 或者 "不要直接操作数据库，应该通过 Prisma migration 来修改 schema"。

###### `/init` 生成后还要持续打磨

`/init` 确实很方便，一行命令就能生成一个基本的 CLAUDE.md。但它只是一个起点。

<font color=red>自动生成的内容通常包含太多无关信息</font>，同时又缺少真正重要的架构决策和业务上下文。这些东西只有你自己知道。

**CLAUDE.md 应该是手动精心打磨的，就像对待一段关键的基础设施代码一样。**

##### 一个好的 CLAUDE.md 应该长什么样

###### 核心框架：WHAT、WHY、HOW

HumanLayer 的工程团队提出了一个非常清晰的框架：

- **WHAT**：你的项目是什么？用了什么技术栈？目录结构长什么样？
  
- **WHY**：每个模块的职责是什么？为什么用这个方案而不是那个？
  
- **HOW**：怎么跑测试？怎么构建？怎么部署？用什么包管理器？
  

  <font color=red>**围绕这三个维度组织内容，基本上就够了**</font>。

###### 推荐的骨架

以下的骨架是我觉得比较精简并且有足够信息量的：

```md
# 项目名：简短描述  
  
这是一个 Next.js 14 项目，使用 App Router + TypeScript。  
  
## 技术栈  
  
- 前端：React 18 + TypeScript + Tailwind CSS  
- 状态管理：Zustand  
- 后端：Next.js API Routes + Prisma + PostgreSQL  
- 包管理器：pnpm  
  
## 常用命令  
  
-`pnpm dev`：启动开发服务器  
-`pnpm test`：运行测试（优先跑单个测试文件，不要跑全量）  
-`pnpm lint`：代码检查  
-`pnpm typecheck`：类型检查（改完代码之后务必跑一次）  
  
## 项目结构  
  
-`/app` - 页面和路由  
-`/components/ui` - 通用 UI 组件  
-`/lib` - 工具函数和共享逻辑  
-`/prisma` - 数据库 schema 和 migrations  
  
## 代码规范  
  
- 使用 ES Modules（import/export），不用 CommonJS  
- 组件用函数式写法，不用 class  
- 尽量用 named export，避免 default export  
  
## 重要约定  
  
- 所有 API 路由必须做错误处理  
- 环境变量在 .env.local 中，不要提交到 git  
- PR 标题用英文，格式：`feat: xxx` / `fix: xxx`  
  
## 注意事项  
  
- Prisma 的 migration 不要手动修改生成的 SQL 文件  
- 遇到认证相关的问题，先看 @docs/auth-patterns.md  
- 测试用 vitest，mock 数据放在 **fixtures** 目录下
```

<font color=dodgerBlue>注意几个要点：</font>

1. **开头一行话说清楚项目是什么**。
   
2. **命令要写精确的语法**——比如 Claude 猜不到你用 pnpm 还是 npm。
   > [!CAUTION]
   > 
   > 好像也可以通过 lockfile 推导出来？
   
3. **只写 Claude 猜不到的东西**——TypeScript、React 的基本语法不用教它。
   
4. **架构决策要写"为什么"**——不只是告诉它目录结构，还要说清楚每个目录的职责。

##### 进阶技巧

###### 用 @imports 保持主文件精简

CLAUDE.md 支持 `@path/to/file` 语法来引用其他文件：

```md
# 项目概览

...

# 详细文档

- Git 工作流：@docs/git-workflow.md
- API 设计规范：@docs/api-conventions.md
- 个人偏好：@~/.claude/my-preferences.md
```

这样主文件保持简洁，详细的内容按需加载。

也可以用 `.claude/rules/` 目录来组织模块化的规则：

```txt
.claude/  
├── CLAUDE.md  
└── rules/  
    ├── code-style.md  
    ├── testing.md  
    └── security.md
```

<font color=red>放在 `.claude/rules/` 里的 Markdown 文件会跟主文件一起自动加载，不需要手动 import</font>。

更强大的是，这些规则文件支持 YAML frontmatter 的 `paths` 字段，可以用 glob 匹配来限定作用范围：

```md
---
paths: src/auth/**/*  
---
  
# 认证模块规则

- 所有认证相关的接口必须做 rate limiting
- Token 刷新逻辑不要手动实现，用 @lib/auth-utils.ts 里的工具函数
```

这样只有当 Claude 处理 `src/auth/` 下面的文件时，这些规则才会生效。非常适合大型项目里不同模块有不同约定的场景。

###### 用强调语法提升关键规则的权重

如果某条规则 Claude 老是不遵守，可以试试加重语气：

```md
IMPORTANT: 所有数据库操作必须通过 Prisma ORM，绝对不要写原生 SQL。  
  
YOU MUST: 改完代码之后跑一次 typecheck。
```

Anthropic 官方文档明确说了，<font color=red>`IMPORTANT`、`YOU MUST` 这类关键词red可以提升模型对特定指令的遵循程度</font>。当然，不要滥用，每条都加"IMPORTANT"就等于没加。

> [!TIP]
> 找了一下来源：见 [Claude Code doc - Best Practices for Claude Code # Write an effective CLAUDE.md](https://code.claude.com/docs/en/best-practices#write-an-effective-claude-md)
> 
> > You can tune instructions by adding emphasis (e.g., “IMPORTANT” or “YOU MUST”) to improve adherence. Check CLAUDE.md into git so your team can contribute. The file compounds in value over time.

###### 自定义压缩策略

Claude Code 在上下文快满的时候会自动压缩对话历史。你<font color=fuchsia>**可以在 CLAUDE.md 里指定压缩时要保留什么**</font>：

```md
## 压缩策略  
  
压缩对话的时候，务必保留：  
  
- 所有修改过的文件列表  
- 测试命令和测试结果  
- 当前的实现计划和进度
```

这样即使对话被压缩了，关键信息也不会丢。

###### 定期让 Claude 帮你优化

<font color=red>**每隔几周，可以让 Claude 自己来审查和优化你的 CLAUDE.md**</font> ：

```md
帮我看看当前的 CLAUDE.md，有没有过时的内容？有没有冗余的规则？
有没有表述不清楚的地方？帮我优化一下。
```

它会基于项目的当前状态给出修改建议，删掉过时的、合并重复的、改清楚含糊的。

###### 结合 hooks 实现硬性约束

CLAUDE.md 里的规则本质上是 “建议性” 的——Claude 大概率会遵守，但不是 100%。如果某个规则必须严格执行，比如 “每次编辑文件后跑 lint” ，那就用 hooks。

Hooks 是在 Claude 工作流特定节点自动执行的脚本，是确定性的，保证会执行。

你可以直接让 Claude 帮你写 hooks：

```md
写一个 hook，每次编辑文件之后自动跑 eslint、e2e 测试用例
```

**CLAUDE.md 负责"软约束"（建议和偏好），hooks 负责"硬约束"（必须执行的规则）。** 两者配合使用效果最好。

##### CLAUDE.md 不只是给 Claude Code 用的

现在各家 AI 编程工具都有类似的配置文件：

- Claude Code → `CLAUDE.md`
  
- Cursor → `.cursor/rules`
  
- GitHub Copilot → `.github/copilot-instructions.md`
  
- Open Code → `OPENCODE.md`
  
- Gemini CLI → `GEMINI.md`

如果你的团队里有人用 Cursor，有人用 Claude Code，可以考虑维护一个 `AGENTS.md` 作为通用版本，然后各个工具的配置文件从它派生。这样不用维护多份重复的规则。比如在以上所有的配置文件里面都写上：

```md
Include:  
@./AGENTS.md
```

然后，把具体的内容全放到 `AGENT.md` 里面，这样就免得为不同的 Agent 维护不同的文档了。



## Cursor

#### Debug Mode

Debug Mode 最适合用于：

- **你能复现但搞不清原因的 bug**：当你知道有问题，但光看代码看不出原因时
- **竞争条件和时序问题**：依赖执行顺序或异步行为的问题
- **性能问题和内存泄漏**：需要运行时分析才能理解的问题
- **曾经能用但现在失效的回归问题**：当你需要追踪到底改了什么时

### 工具

#### 浏览器

##### 使用场景

###### Web 开发工作流

Browser 可以集成到 Web 开发工作流中，与 Figma、Linear 等工具协同使用。请查看 [Web 开发实用手册](https://cursor.com/for/web-development)，了解如何将 Browser 与设计系统、项目管理工具和组件库配合使用的完整指南。
###### 无障碍改进

Agent 可以审计并改进网页无障碍情况，以满足 WCAG 合规标准。

```md
@browser Check color contrast ratios, verify semantic HTML and ARIA labels, test keyboard navigation, and identify missing alt text
```

###### 自动化测试

Agent 可以执行完整的测试套件，并捕获截图用于视觉回归测试。

```md
@browser 使用测试数据填写表单，依次点击完成各项工作流，测试响应式设计，验证错误信息，并监控控制台中的 JavaScript 错误
```

###### 从设计到代码

Agent 可以将设计稿转换为可运行的代码，并支持响应式布局。

```md
@browser 分析这个设计稿，提取配色和字体，并生成像素级精确还原的 HTML 和 CSS 代码
```

###### 根据截图调整 UI 设计

Agent 可以通过识别视觉差异并更新组件样式来优化现有界面。

```md
@browser 对比当前 UI 与这张设计截图，并调整间距、颜色和字体排版以使其匹配
```


#### 语义与 Agent 搜索

查找代码最快的方法是精确匹配：函数名、变量名、错误字符串或正则表达式模式。当你引用特定符号时，<font color=red>Agent 会自动使用 grep</font>。
##### 语义搜索

当你不知道确切名称时，语义搜索会按含义查找代码。比如问 “where do we handle authentication?”，Agent 仍然可以定位到 `middleware/session.ts`，<font color=lightSeaGreen>即使文件里从未出现过单词 “authentication”</font>。

这是因为 Cursor 会[为你的代码库建立索引](https://cursor.com/blog/secure-codebase-indexing)，<font color=red>使用自定义的嵌入模型把代码转成**可搜索向量**</font>。对 Cursor [语义搜索](https://cursor.com/blog/semsearch) 的研究表明，将它和 grep 结合使用，在回答代码库相关问题时，准确率比单独使用 grep 提升了 12.5%。这种提升在包含 1,000+ 文件的代码库中最为明显。
###### 索引的工作原理

Cursor 会将你的代码拆分为有意义的片段 (函数、类、逻辑块) ，将每个片段转换为捕捉其语义的向量嵌入，并将结果存储在向量数据库中。你进行搜索时，查询会使用相同的模型转换为向量，并与已存储的嵌入进行匹配。

当你打开一个工作区时，会自动开始建立索引。索引完成 80% 后即可使用语义搜索。索引通过每 5 分钟自动同步一次保持最新状态，并且只处理已更改的文件。

###### 配置

在 **Cursor Settings > Indexing** 中检查索引状态或触发重新索引。

<font color=red>Cursor 会为除 [忽略文件](https://cursor.com/docs/reference/ignore-file) (`.gitignore`、`.cursorignore`) 之外的所有文件建立索引</font>。忽略大型生成文件或内容文件有助于提升搜索准确性。
##### 提升搜索结果的技巧

- **先具体，再泛化。** 如果你知道函数名，就直接写出来。如果你在阅读不熟悉的代码，就描述它的行为。
- **先了解，再改动。** 先让 Agent 展示现有模式，再让它添加新的逻辑。这样可以避免产生重复实现或破坏既有约定。
- **引用具体代码。** 像“find all callers of `processOrder`”这样的指令会给 Agent 一个精确目标。像“find the order code”这样的指令会让 Agent 只能猜你的真实意图。


#### 工作树

使用 `/worktree` 进行一次隔离运行，使用 `/best-of-n` 在隔离工作树中对同一任务比较多个模型。

工作树是一个独立的 Git 检出，位于主检出目录旁边。Cursor 用它来隔离本地智能体的工作，同时仍基于同一个代码仓库进行操作。

##### 使用 `/worktree` 进行一次隔离运行

当你希望 Cursor 在单独的检出副本中继续处理这段聊天的其余工作时，使用 `/worktree` 启动任务。

- 让实验性修改远离主检出
- <font color=red>在不影响当前分支的情况下运行安装、构建和测试</font>
- <font color=red>进行高风险重构，并保留简单的清理路径</font>
  
```md
/worktree fix the failing auth tests and update the login copy
```

在很多情况下，你应该可以直接在工作树中提交/推送。你可以直接让智能体来做：

```md
Commit and push these changes, then open a PR
```

不过，<font color=red>如果你想将更改带回主检出中进行测试，使用 `/apply-worktree`</font>。<font color=red>完成这个独立检出后，使用 `/delete-worktree`</font>。

如果你想查看代码仓库中的所有工作树，请运行：

```sh
git worktree list
```

##### 使用 `/best-of-n` 比较多个模型

`/best-of-n` 会同时在多个模型上运行同一个任务。每次运行都会获得各自的工作树，因此这些候选结果彼此隔离，也与您的主检出隔离。

```md
/best-of-n sonnet, gpt, composer fix the flaky logout test
```

<font color=dodgerBlue>在你想要执行以下操作时使用它：</font>

- <font color=red>在同一个提示词上比较不同模型</font>
- <font color=red>针对棘手的更改尝试多种方法</font>
- 在应用任何内容之前选出最佳结果

`/best-of-n` 只比较各次运行。它不会帮你将更改合并回你的主检出。<font color=lightSeaGreen>选出最佳结果后</font>，你可以直接在工作树中 commit/push，或使用 `/apply-worktree` 将更改带入你的主检出。

##### worktree 设置如何运作？

你可以使用 `.cursor/worktrees.json` 自定义工作树设置。Cursor 按以下顺序查找该文件：

1. 在工作树路径中
2. 在项目根路径中

### 规则

<font color=dodgerBlue>Cursor 支持四种类型的规则：</font>

- **项目规则**：存储在 `.cursor/rules` 中，纳入版本控制，并限定在你的代码库范围内。
- **用户规则**：在你的 Cursor 环境中全局生效。由 Agent (Chat) 使用。
- **团队规则**：从仪表盘管理的团队范围规则。适用于 Team 和 [Enterprise](https://cursor.com/docs/enterprise) 方案。
- **AGENTS.md**：采用 markdown 格式的 Agent 指令。是 `.cursor/rules` 的简单替代方案。

###### 规则如何工作

大语言模型在各次补全之间不会保留记忆。规则在提示层面提供持久、可复用的上下文。

应用规则时，其<font color=red>内容会被加入到模型上下文的开头</font>。这为 AI 在生成代码、理解修改或协助处理工作流程时提供一致的指导。

##### 项目规则

项目规则存放在 `.cursor/rules` 目录中的 Markdown 文件里，并 <font color=red>**纳入版本控制**</font>。它们可以通过路径模式限定作用范围、手动触发，或按相关性自动应用。

<font color=dodgerBlue>使用项目规则可以：</font>

- 固化与你代码库相关的领域知识
- 自动化项目特定的工作流或模板
- 统一风格或架构决策

###### 规则文件结构

每条规则对应一个 Markdown 文件，文件名可自定义。Cursor 支持 `.md` 和 `.mdc` 扩展名。使用带有 frontmatter 的 `.mdc` 文件来指定 `description` 和 `globs`，以便更精细地控制规则的生效条件。

> [!NOTE]
> 这里的 frontmatter （前言）就是死 [[#规则结构]] 中的代码块最前面的部分，用于描述该文档的使用规则的标识，如下面所说是 “元数据”，也如 HTML 中的 TDK

```txt
.cursor/rules/
  react-patterns.mdc       # 带有前置元数据的规则（description, globs）
  api-guidelines.md        # 简单的 markdown 规则
  frontend/                # 在文件夹中组织规则
    components.md
```

###### 规则结构

每条规则都是一个带有 frontmatter 元数据和内容的 markdown 文件。通过类型下拉菜单控制规则的应用方式，该菜单会更改 `description`、`globs`、`alwaysApply` 属性。

| Rule Type                 | 描述                                                       |
| ------------------------- | -------------------------------------------------------- |
| `Always Apply`            | 应用于每个聊天会话                                                |
| `Apply Intelligently`     | 当 Agent 根据描述判断其相关时应用                                     |
| `Apply to Specific Files` | <font color=red>当文件匹配指定模式时应用</font>                      |
| `Apply Manually`          | <font color=red>**在聊天中被 @ 提及时应用 (例如 `@my-rule`)**</font> |
```md
---
globs:
alwaysApply: false
---

- Use our internal RPC pattern when defining services
- Always use snake_case for service names.

@service-template.ts
```

###### 创建规则

有两种方式可以创建规则：

- **在对话中使用 `/create-rule`**：在 Agent 中输入 `/create-rule`，然后描述你的需求。Agent 会生成带有正确 frontmatter 的规则文件，并将其保存到 `.cursor/rules`。
- **在设置中创建**：打开 `Cursor Settings > Rules, Commands`，然后点击 `+ Add Rule`。这会在 `.cursor/rules` 中创建一个新的规则文件。在设置中可以查看所有规则及其状态。

##### 最佳实践

好的规则应当聚焦、可操作且范围明确。

- 将规则控制在 500 行以内
- 将大型规则拆分为<font color=red>多个 **可组合的** 规则</font>
- 提供具体示例或引用相关文件
- 避免模糊的指导，像撰写清晰的内部文档那样来写规则
- 在聊天中重复提示时复用规则
- 引用文件而不是复制其内容——这样可以让规则更简短，并避免在代码变更后规则变得陈旧

###### 编写规则时应避免的事项

- **整份照搬风格指南**：请改用 linter。Agent 已经了解常见的风格规范。
- **把每一个可能的命令都写进文档**：Agent 了解常用工具，比如 npm、git 和 pytest。
- <font color=fuchsia>**为极少出现的边缘情况添加说明**：让规则只聚焦在你经常使用的模式上</font>。
- **重复你代码库中已有的内容**：引用规范示例，而不是复制代码。

把你的规则提交到 Git，这样整个团队都能受益。当你看到 Agent 出错时，就更新对应的规则。你甚至可以在 GitHub issue 或 PR 上标记 `@cursor`，让 Agent 帮你更新规则。

##### 规则文件格式

每条规则都是一个包含 frontmatter 元数据和正文内容的 Markdown 文件。frontmatter 元数据用于控制规则的应用方式，正文则定义规则本身。

```md
---
description: "This rule provides standards for frontend components and API validation"
alwaysApply: false
---

...rest of the rule content
```

如果 `alwaysApply` 为 true，则该规则会应用于每个聊天会话。否则，会将该规则的描述展示给 Cursor Agent，由其决定是否应当应用该规则。

##### 导入规则

可以从外部来源导入规则，以复用现有配置或引入其他工具的规则。

###### 远程规则 (通过 GitHub)

> [!WARNING]
> 2026/4/7，在当前的 Cursor Version: 3.0.12，这里入口找不到，应该是文档更新不及时

从你有访问权限的任何 GitHub 仓库 (公共或私有) 直接导入规则。

1. 打开 **Cursor Settings → Rules, Commands**
2. 点击 `+ Add Rule` (位于 `Project Rules` 旁) ，然后选择 Remote Rule (GitHub)
3. 粘贴包含该规则的 GitHub 仓库 URL
4. Cursor 会拉取该规则并将其同步到你的项目中

导入的规则会与其源仓库保持同步，因此远程规则的更新会自动体现在你的项目中。

##### AGENTS.md

`AGENTS.md` 是一个用于定义 agent 指令的简单 markdown 文件。将它<font color=red>放在项目根目录中，可作为 `.cursor/rules` 的一种替代方式</font>，适用于简单直接的用例。

与 Project Rules 不同，`AGENTS.md` 是不带元数据或复杂配置的纯 markdown 文件。对于只需要简单、易读指令且不想引入结构化规则开销的项目来说，它非常合适。

Cursor 支持在项目根目录和子目录中使用 `AGENTS.md`。

```md
# Project Instructions

## Code Style

- Use TypeScript for all new files
- Prefer functional components in React
- Use snake_case for database columns

## Architecture

- Follow the repository pattern
- Keep business logic in service layers
```

###### 改进

嵌套 AGENTS.md 支持

现在已支持在子目录中使用嵌套的 `AGENTS.md` 文件。你可以在项目的任意子目录下放置 `AGENTS.md` 文件，在处理该目录及其子目录中的文件时，这些文件会自动生效。

这样可以根据你当前正在处理的代码库区域，对 agent 指令进行更精细的控制：

```txt
project/
  AGENTS.md              # 全局指令
  frontend/
    AGENTS.md            # 前端专用指令
    components/
      AGENTS.md          # 组件专用指令
  backend/
    AGENTS.md            # 后端专用指令
```

来自嵌套 `AGENTS.md` 文件的指令会与父目录中的指令合并，更具体的指令具有更高的优先级。

##### 用户规则

用户规则是在 **Cursor Settings → Rules** 中定义的全局首选项，适用于所有项目。它们供 Agent (Chat) 使用，非常适合用来设定偏好的沟通风格或编码规范：

```md
Please reply in a concise style. Avoid unnecessary repetition or filler language.
```



### Agent Skills

Agent Skills 是一个用于<font color=red>为 AI 智能体扩展专门能力的开放标准</font>。<font color=red>**Skills 将特定领域的知识和工作流封装起来**</font>，智能体可以调用这些 Skills 来执行特定任务。

##### 什么是技能？

技能是一个 <font color=red>**可移植**、**支持版本控制**</font> 的包，用于让 Agent 学会如何执行特定领域的任务。技能可以包含脚本、模板和参考资料，Agent 可以使用其工具对这些内容进行操作。

- **可移植**：技能适用于任何支持 Agent Skills 标准的 Agent。
- **受版本控制**：技能以文件形式存储，<font color=red>**可以在你的代码仓库中追踪其变更**</font>，或<font color=red>通过 GitHub 仓库链接进行安装</font>。
- **可操作**：技能可以包含脚本、模板和参考资料，Agent 使用其工具对这些内容进行处理。
- **渐进式**：技能按需加载资源，使上下文使用更加高效。

##### 技能的工作原理

Cursor 启动时，会自动从技能目录中发现并加载技能，并将它们提供给 Agent 使用。Agent 会看到所有可用技能，并根据当前上下文决定何时调用它们。

你<font color=red>也可以在 Agent 对话中 **输入 `/` 并搜索技能名称** 来手动调用技能</font>。

##### 技能目录

技能会自动从以下位置加载：

|位置|作用域|
|---|---|
|`.agents/skills/`|项目级|
|`.cursor/skills/`|项目级|
|`~/.cursor/skills/`|用户级（全局）|

为了兼容性，Cursor 还会从 Claude 和 Codex 目录加载技能：`.claude/skills/`、`.codex/skills/`、`~/.claude/skills/` 和 `~/.codex/skills/`。

<font color=red>每个技能应为一个包含 `SKILL.md` 文件的文件夹</font>：

```txt
.agents/
└── skills/
    └── my-skill/
        └── SKILL.md
```

技能还可以包含脚本、<font color=red>参考文件</font> 和 <font color=red>资源</font> 等可选目录：

```txt
.agents/
└── skills/
    └── deploy-app/
        ├── SKILL.md
        ├── scripts/
        │   ├── deploy.sh
        │   └── validate.py
        ├── references/
        │   └── REFERENCE.md
        └── assets/
            └── config-template.json
```

##### SKILL.md 文件格式

每个 Skill 都在带有 YAML 前置信息（frontmatter）的 `SKILL.md` 文件中定义：

```md
---
name: my-skill
description: 简要描述此技能的功能及使用时机。
---

# 我的技能

为 Agent 提供的详细指令。

## 使用时机

- 在以下情况使用此技能...
- 此技能适用于...

## 指令

- 为 Agent 提供的分步指导
- 特定领域的约定
- 最佳实践和模式
- 如需向用户澄清需求,请使用提问工具
```

###### Frontmatter 字段

|字段|必填|说明|
|---|---|---|
|`name`|Yes|技能标识符。仅限小写字母、数字和连字符。必须与父文件夹名称一致。|
|`description`|Yes|描述技能的作用及其使用场景。由代理用于判断相关性。|
|`license`|No|许可证名称或对随附许可证文件的引用。|
|`compatibility`|No|运行环境要求（系统软件包、网络访问等）。|
|`metadata`|No|用于额外元数据的任意键值映射。|
|`disable-model-invocation`|No|当为 `true` 时，该技能仅会在通过 `/skill-name` 显式调用时才会被使用。代理不会基于上下文自动调用它。|
##### 禁用自动调用

默认情况下，当 agent 判断某个 skill 相关时，会自动应用该 skill。将 `disable-model-invocation` 设为 `true`，可以让该 skill 的行为类似传统的斜杠命令（slash command），<font color=red>只有当你在聊天中显式输入 `/skill-name` 时，才会被包含进上下文</font>。

> [!NOTE]
>
> 先看的 Cursor 文档，这部分看得云里雾里，之后看的 Claude Code 文档，才看明白了。关于 `disable-model-invocation` 可以看下 [[#了解上下文成本]] 中 “*” 部分的补充内容

##### 在技能中包含脚本

技能可以包含 `scripts/` 目录，内含可由代理运行的可执行代码。在 `SKILL.md` 文件中使用相对于技能根目录的相对路径引用这些脚本。

```md
---
name: deploy-app
description: 将应用部署到预发布或生产环境。在部署代码时使用,或当用户提及部署、发布或环境时使用。
---

# Deploy App

Deploy the application using the provided scripts.

## Usage

Run the deployment script: `scripts/deploy.sh <environment>`

Where `<environment>` is either `staging` or `production`.

## Pre-deployment Validation

Before deploying, run the validation script: `python scripts/validate.py`
```

当技能被调用时，agent 会读取这些指令并执行引用的脚本。脚本可以使用任何语言编写，例如 Bash、Python、JavaScript，或 agent 实现所支持的任何其他可执行格式。

> [!TIP]
> 脚本应是自包含的，提供有用的错误信息，并能优雅地处理各种边界情况。

##### 可选目录

Skills 支持以下可选目录：

|Directory|Purpose|
|---|---|
|`scripts/`|Agents 可以运行的可执行代码|
|`references/`|按需加载的附加文档|
|`assets/`|模板、图片或数据文件等静态资源|

请让主 `SKILL.md` 文件保持简洁，将详细参考资料放在单独的文件中。这样可以更高效地利用上下文，因为 Agents 会按需逐步加载资源——只在需要时才加载。

### Subagents

<font color=red>子代理是 **专门化的 AI 助手**，Cursor 的代理可以将任务委派给它们</font>。<font color=red>每个子代理都在自己的上下文窗口中运行，处理特定类型的工作，并**将结果返回给父代理**</font>。使用子代理可以拆解复杂任务、**并行开展工作**，并在主对话中保留上下文。

> [!NOTE]
> 分治思想

你可以在编辑器、CLI 和 [Cloud Agents](https://cursor.com/docs/cloud-agent) 中使用子代理。

- **上下文隔离**：<font color=red>**每个子代理都有自己的上下文窗口**</font>。长时间的研究或探索任务不会占用主对话中的空间。
- **并行执行**：同时启动多个子代理。无需等待依次完成，就可以处理代码库的不同部分。
- **Specialized expertise**：通过自定义提示词、工具访问权限和模型来配置子代理，以处理特定领域的任务。
- **Reusability**：定义自定义子代理，并在多个项目中使用它们。

##### 子代理的工作原理

当 <font color=red>Agent **遇到复杂任务时**，它可以 **自动启动一个子代理**</font>。<font color=fuchsia>子代理会**接收一个包含所有必要上下文** 的提示，自主运行</font>，并在完成后返回一条包含其结果的最终消息。

<font color=fuchsia>子代理在全新的上下文中启动</font>。<font color=red>父代理会在提示中加入相关信息</font>，因为<font color=fuchsia>子代理无法访问先前的会话历史</font>。

###### 前台 vs 后台

<font color=dodgerBlue>子代理可以通过两种模式运行：</font>

| 模式     | 行为                                                  | 最适合                                        |
| ------ | --------------------------------------------------- | ------------------------------------------ |
| **前台** | <font color=fuchsia>会 **阻塞直到子代理完成**，并立即返回结果</font>。 | 你需要输出结果的串行任务。                              |
| **后台** | 会立即返回。子代理会独立工作。                                     | <font color=fuchsia>长时间运行的任务或并行工作流</font>。 |
> [!NOTE]
> 看到下面，在 [[#配置字段]] 看到了 `is_background` 配置，看起来应该是通过该配置设置的前台还是后台
##### 内置子代理

<font color=red>**Cursor 包含三个内置子代理**</font>，可自动处理上下文密集型的操作。这些子代理是基于对代理对话的分析设计出来的，当时这些对话会触及上下文窗口限制。

|子代理|作用|为什么它是子代理|
|---|---|---|
|**Explore**|搜索并分析代码库|代码库探索会产生大量中间输出，导致主上下文膨胀。它使用更快的模型来并行运行大量搜索。|
|**Bash**|运行一系列 shell 命令|命令输出通常很冗长。将其隔离可以让父代理专注于决策，而不是日志。|
|**Browser**|通过 MCP 工具控制浏览器|浏览器交互会产生嘈杂的 DOM 快照和截图。子代理会将这些过滤为相关结果。|

##### 为什么需要这些子代理

这三类操作有共同特点：它们会生成嘈杂的中间输出，受益于专门的提示词和工具，而且可能消耗大量上下文。<font color=dodgerBlue>将它们作为子代理运行可以解决几个问题</font>：

- **上下文隔离** — 中间输出保留在子代理中。父级智能体只能看到最终摘要。
- **模型灵活性** — explore 子代理默认使用更快的模型。这样就能在一次主智能体搜索所需的时间内并行运行 10 次搜索。
- **专门配置** — 每个子代理都有针对其特定任务调优的提示词和工具访问权限。
- **成本效率** — 更快的模型成本更低。将 token 消耗高的工作隔离到采用合适模型选择的子代理中，可以降低总体成本。

<font color=red>你**不需要配置这些子代理**。**智能体会在适当时自动使用它们**</font>。

##### 何时使用子代理

| 适合使用子代理的情况...                          | 适合使用技能的情况...                                        |
| -------------------------------------- | --------------------------------------------------- |
| 你需要为长时间的研究任务隔离上下文                      | <font color=red>任务是单一用途的</font> (生成 changelog、格式化等) |
| <font color=red>**需要并行运行多个工作流**</font> | 你想要一个快速、可重复的操作                                      |
| 任务在多个步骤中需要专业领域的专长                      | 任务可以一次性完成                                           |
| 你希望对工作进行独立验证                           | 你不需要单独的上下文窗口                                        |
##### 快速开始

> [!NOTE]
> 这里看得有点迷，感觉可以理解为：Cursor 内置了三个 subAgent，但是用户也可以自定义自己的 subAgent

Agent 会在合适的时候自动使用子代理。你也可以通过向 Agent 提示来创建自定义子代理：

```md
在 `.cursor/agents/verifier.md` 中创建一个子代理文件，在文件开头使用 YAML frontmatter（name、description），后面写入具体提示内容。verifier 子代理应当验证已完成的工作，检查实现是否正常工作、运行测试，并报告通过的部分与未完成的部分。
```

若需获得更精细的控制，可以在项目目录或用户目录中手动创建自定义子代理。

##### 自定义子代理

定义自定义子代理，用于固化专业知识、落实团队规范或自动化处理重复性流程。

###### 文件位置

|类型|位置|适用范围|
|---|---|---|
|**项目子代理**|`.cursor/agents/`|仅限当前项目|
||`.claude/agents/`|仅限当前项目 (与 Claude 兼容)|
||`.codex/agents/`|仅限当前项目 (与 Codex 兼容)|
|**用户子代理**|`~/.cursor/agents/`|当前用户的所有项目|
||`~/.claude/agents/`|当前用户的所有项目 (与 Claude 兼容)|
||`~/.codex/agents/`|当前用户的所有项目 (与 Codex 兼容)|

当名称冲突时，项目子代理优先。当多个位置包含同名子代理时，`.cursor/` 的优先级高于 `.claude/` 或 `.codex/`。

###### 文件格式

<font color=lightSeaGreen>每个子代理都是一个带有 YAML frontmatter 的 Markdown 文件</font>：

```md
---
name: security-auditor
description: 安全专家。在实现身份验证、支付或处理敏感数据时使用。
model: inherit
readonly: true
---

您是一位负责审计代码漏洞的安全专家。

调用时：
1. 识别安全敏感的代码路径
2. 检查常见漏洞（注入、XSS、身份验证绕过）
3. 验证密钥未被硬编码
4. 审查输入验证和清理

按严重程度报告发现：
- 严重（部署前必须修复）
- 高（尽快修复）
- 中（尽可能解决）
```

###### 配置字段

|字段|类型|必填|默认值|描述|
|---|---|---|---|---|
|`name`|string|否|根据文件名生成|显示名称和标识符。使用小写字母和连字符。|
|`description`|string|否|—|显示在 Task 工具提示中的简短描述。代理会读取此项以决定是否委派。|
|`model`|string|否|`inherit`|要使用的模型：`fast`、`inherit` 或特定模型 ID。参见 [模型配置](https://cursor.com/cn/docs/subagents#model-configuration)。|
|`readonly`|boolean|否|`false`|如果为 `true`，子代理将以受限写入权限运行（不能编辑文件，也不能运行会更改状态的 shell 命令）。|
|`is_background`|boolean|否|`false`|如果为 `true`，子代理将在后台运行，不会阻塞父级。|

###### 模型配置

`model` 字段用于控制子代理使用哪个模型。共有三个选项：

| 值         | 行为                                                                                                           |
| --------- | ------------------------------------------------------------------------------------------------------------ |
| `inherit` | <font color=red>使用与父代理相同的模型。**这是默认值**</font>。                                                                |
| `fast`    | 使用更小、更快且针对速度和成本优化的模型。适合搜索、验证或执行测试等高频任务。                                                                      |
| 特定模型 ID   | 使用你指定的确切模型，例如 `claude-4-sonnet` 或 `gpt-5-mini`。可用 ID 请参见 [模型参考](https://cursor.com/docs/models-and-pricing)。 |

当子代理需要与父代理相同的推理能力时，选择 `inherit`。如果任务中速度和成本比深度更重要，则选择 `fast`。如果你需要某个特定模型的能力，而不受父代理所用模型影响，则使用特定模型 ID。

##### 使用子代理

###### 自动委派

<font color=dodgerBlue>智能体会根据以下因素主动委派任务：</font>

- 任务的复杂度和范围
- 你在项目中为子代理编写的自定义描述
- 当前上下文和可用工具

在 description 字段中包含诸如 “use proactively” 或 “always use for” 之类的短语，可以促进自动委派。

###### 显式调用

在提示中使用 `/name` 语法来调用指定的子代理：

```md
> /verifier confirm the auth flow is complete
> /debugger investigate this error
> /security-auditor review the payment module
```

你也可以在对话中自然提到子代理，从而调用它们：

```md
> Use the verifier subagent to confirm the auth flow is complete
> Have the debugger subagent investigate this error
> Run the security-auditor subagent on the payment module
```

###### 并行执行

并发启动多个子代理，以获得最大吞吐量：

```md
> Review the API changes and update the documentation in parallel
```

智能体会在一条消息中发送多个 Task 工具调用，因此子代理会同时运行。

###### 恢复子代理

恢复子代理可以继续之前的对话。这对于跨越多次调用的长时运行任务很有用。

<font color=lightSeaGreen>每次子代理执行都会返回一个 agent ID</font>。传入该 ID 即可在保留完整上下文的情况下恢复该子代理：

```md
> Resume agent abc123 and analyze the remaining test failures
```

##### 常见模式

###### 验证代理

验证代理会独立验证声称已完成的工作是否真的已经完成。这解决了一个常见问题：AI 会将任务标记为已完成，但实际实现却不完整或存在故障。

此模式适用于：

- 在将工单标记为完成之前，验证功能是否端到端正常工作
- 发现只实现了一部分的功能
- 确保测试确实通过 (而不只是存在测试文件)

###### Orchestrator 模式

对于复杂工作流，父级 Agent 可以按顺序协调多个专业子 Agent：

1. **Planner** 分析需求并制定技术方案
2. **Implementer** 根据方案实现功能
3. **Verifier** 确认实现满足需求

每次交接都会包含结构化输出，让下一个 Agent 拥有清晰的上下文。

##### 子代理示例

> [!NOTE]
> 看下来，感觉和一般的 rules.md 的风格很接近
###### 调试器

```md
---
name: debugger
description: Debugging specialist for errors and test failures. Use when encountering issues.
---

You are an expert debugger specializing in root cause analysis.

When invoked:
1. Capture error message and stack trace
2. Identify reproduction steps
3. Isolate the failure location
4. Implement minimal fix
5. Verify solution works

For each issue, provide:
- Root cause explanation
- Evidence supporting the diagnosis
- Specific code fix
- Testing approach

Focus on fixing the underlying issue, not symptoms.
```

```md
在 .cursor/agents/debugger.md 中创建一个子代理文件，在 YAML frontmatter 中填写 name 和 description。debugger 子代理应专注于根本原因分析：捕获堆栈跟踪、确定复现步骤、隔离故障、实施最小修复，并验证方案有效性。
```

##### 测试运行器

```md
---
name: test-runner
description: Test automation expert. Use proactively to run tests and fix failures.
---

You are a test automation expert.

When you see code changes, proactively run appropriate tests.

If tests fail:
1. Analyze the failure output
2. Identify the root cause
3. Fix the issue while preserving test intent
4. Re-run to verify

Report test results with:
- Number of tests passed/failed
- Summary of any failures
- Changes made to fix issues
```

```md
在 .cursor/agents/test-runner.md 中创建一个子代理文件，其 YAML frontmatter 中包含 name 和 description（描述中需提到“Use proactively”）。test-runner 子代理应在检测到代码变更时主动运行测试，分析失败原因，在保持测试意图的前提下修复问题，并报告结果。
```

##### 最佳实践

- **编写专注的子代理** — <font color=red>每个子代理应只负责一件事、**职责清晰**</font>。避免泛泛的「helper」代理。
- **投入精力写好描述** — <font color=red>`description` 字段决定 Agent 何时委派给你的子代理</font>。花时间打磨它。<font color=fuchsia>通过编写不同的提示并检查是否触发了正确的子代理来测试</font>。
- **保持提示简洁** — 冗长、啰嗦的提示会削弱聚焦度。要具体、直接。
- **将子代理纳入版本控制** — 把 `.cursor/agents/` 提交到代码仓库，让整个团队都能受益。
- **从 Agent 生成的代理开始** — 先让 Agent 帮你起草初始配置，再进行自定义。
- **使用 hooks 处理文件输出** — 如果你需要子代理生成结构化输出文件，可以考虑使用 [hooks](https://cursor.com/docs/hooks) 来统一处理并保存这些结果。

###### 需要避免的反模式

>[!WARNING]
> **不要创建几十个通用型子代理。** 拥有 50+ 个带有 “helps with coding” 这类模糊说明的子代理是低效的。Agent 不知道该在什么时候使用它们，而你也会浪费时间维护它们。

- **描述含糊** — “Use for general tasks” 不会给 Agent 任何关于何时进行委派的信号。要具体一些：比如 “Use when implementing authentication flows with OAuth providers.”
- **提示过长** — 2,000 字的提示不会让子代理更聪明，只会让它更慢、更难维护。
- **重复 slash commands** — 如果任务是单一用途且不需要上下文隔离，请改用 [slash command](https://cursor.com/help/customization/rules)。
- **子代理过多** — 从 2–3 个聚焦明确的子代理开始。只有在你有清晰且彼此不同的用例时，再增加更多。

##### 管理子代理

##### 创建子代理

<font color=dodgerBlue>创建子代理最简单的方法</font>，就是<font color=red>让 Agent 为你创建一个</font>：

```md
Create a subagent file at .cursor/agents/security-reviewer.md with YAML frontmatter containing name and description. The security-reviewer subagent should check code for common vulnerabilities like injection, XSS, and hardcoded secrets.
```

你也可以通过在 `.cursor/agents/` (项目) 或 `~/.cursor/agents/` (用户) 中添加 Markdown 文件来手动创建子代理。

###### 查看子代理

智能体会在其可用工具中包含所有自定义子代理。你可以通过检查项目中的 `.cursor/agents/` 目录来查看配置了哪些子代理。

##### 性能与成本

子代理有其取舍。理解这些取舍有助于你决定何时使用它们。

| 优点    | 代价                        |
| ----- | ------------------------- |
| 上下文隔离 | 启动开销 (每个子代理都要收集自己的上下文)    |
| 并行执行  | 更高的 token 使用量 (多个上下文同时运行) |
| 专项聚焦  | 延迟 (对于简单任务，可能比主代理更慢)      |

###### Token 和成本注意事项

- **子 Agent 独立消耗 Token** — 每个子 Agent 都有自己的上下文窗口和 Token 使用量。并行运行 5 个子 Agent，大约会使用单个 Agent 的 5 倍 Token。
- **评估额外开销** — 对于快速、简单的任务，主 Agent 往往更快。子 Agent 更适合复杂、长时间运行或需要并行的工作。
- **子 Agent 可能更慢** — 它的优势在于上下文隔离，而不是速度。<font color=red>执行简单任务的子 Agent 可能比主 Agent 更慢，**因为它需要从零开始**</font>。

##### FAQ

- **子代理可以启动其他子代理吗？**
  可以。自 Cursor 2.5 起，<font color=red>子代理可以启动子级子代理</font>，形成协调工作的树状结构。 嵌套启动需要当前模式下具有 Task 工具访问权限，并且 hooks 或工具策略可能会阻止生成。
  > [!NOTE]
  > 有点类似于 子进程 fork
- **如何查看子代理在做什么？**
  后台子代理会将输出写入 `~/.cursor/subagents/`。父代理 可以读取这些文件来检查进度。
- **如果子代理失败会发生什么？**
  子代理会向父代理返回错误状态。父代理可以 重试、在补充额外上下文后恢复，或以不同方式处理该失败。
- **我可以在子代理中使用 MCP 工具吗？**
  是的。<font color=red>**子代理会继承父代理的所有工具**</font>，包括来自已配置服务器的 MCP 工具。
- **如何调试行为异常的子代理？**
  检查子代理的 description 和提示。确保这些说明 具体且没有歧义。你也可以通过显式调用子代理并给它一个简单任务 来测试它。

### 钩子

钩子 <font color=red>让你能够使用自定义脚本来观察、控制和扩展 agent 循环</font>。钩子 <font color=red>**是生成的进程**，通过 stdio 使用 JSON 进行双向通信</font>。它们会在 agent 循环的定义阶段之前或之后运行，并且可以观察、阻止或修改行为。

借助 钩子，你可以：

- 在编辑后运行格式化工具
- 为事件添加分析
- 扫描 PII 或机密信息
- 为高风险操作设置门禁 (例如 SQL 写入)
- 控制 subagent (Task 工具) 的执行
- 在会话开始时注入上下文

#### Agent 和 Tab 支持

钩子同时适用于 **Cursor Agent** (Cmd+K/Agent Chat) 和 **Cursor Tab** (行内补全) ，但它们使用不同的 钩子事件：

**Agent (Cmd+K/Agent Chat)** 使用标准钩子：

- `sessionStart` / `sessionEnd` - 会话生命周期管理
- `preToolUse` / `postToolUse` / `postToolUseFailure` - 通用工具使用钩子 (对所有工具都会触发)
- `subagentStart` / `subagentStop` - subagent (Task 工具) 生命周期
- `beforeShellExecution` / `afterShellExecution` - 控制 shell 命令
- `beforeMCPExecution` / `afterMCPExecution` - 控制 MCP 工具使用
- `beforeReadFile` / `afterFileEdit` - 控制文件访问和编辑
- `beforeSubmitPrompt` - 在提交前校验 prompt
- `preCompact` - 监听上下文窗口压缩
- `stop` - 处理 Agent 完成
- `afterAgentResponse` / `afterAgentThought` - 跟踪 Agent 响应

**Tab (行内补全)** 使用专门的钩子：

- `beforeTabFileRead` - 控制 Tab 补全的文件访问
- `afterTabFileEdit` - 对 Tab 编辑进行后处理

这些独立的钩子允许为自动化的 Tab 操作和用户驱动的 Agent 操作配置不同策略。

#### 快速开始

先创建一个 `hooks.json` 文件。你可以在项目级路径 (`<project>/.cursor/hooks.json`) 或主目录 (`~/.cursor/hooks.json`) 下创建。项目级钩子只对该项目生效，主目录中的钩子全局生效。

###### 用户 hooks (`~/.cursor/`)

对于全局生效的用户级钩子，在 `~/.cursor/hooks.json` 中创建：

```json
{
  "version": 1,
  "hooks": {
    "afterFileEdit": [{ "command": "./hooks/format.sh" }]
  }
}
```

在 `~/.cursor/hooks/format.sh` 中创建你的钩子脚本：

```sh
#!/bin/bash
# 读取输入，执行处理，然后以 0 退出
cat > /dev/null
exit 0
```

将脚本设为可执行：

```sh
chmod +x ~/.cursor/hooks/format.sh
```

Cursor 会监视钩子配置文件并自动重新加载。现在每次编辑文件后都会执行你的钩子。

###### 项目 hooks (`.cursor/`)

对于仅对某个代码仓库生效的项目级钩子，在 `<project>/.cursor/hooks.json` 中创建：

```json
{
  "version": 1,
  "hooks": {
    "afterFileEdit": [{ "command": ".cursor/hooks/format.sh" }]
  }
}
```

注意：项目级钩子从**项目根目录**运行，因此要使用 `.cursor/hooks/format.sh` (而不是 `./hooks/format.sh`) 。

在 `<project>/.cursor/hooks/format.sh` 中创建你的钩子脚本：

> [!NOTE]
> 这部分内容和 [[#用户 hooks (`~/.cursor/`)]] 一样，略

##### Hook 类型

Hook 提供两种执行方式：命令驱动 (默认) 和 Prompt 驱动 (由 LLM 评估) 。

> [!NOTE]
> 即：默认的 “命令驱动” 无需指定 `type`

###### 基于命令的 Hook

命令类 Hook 会执行 Shell 脚本，这些脚本从 stdin 接收 JSON 输入，并向 stdout 输出 JSON。

```json
{
  "hooks": {
    "beforeShellExecution": [
      {
        "command": "./scripts/approve-network.sh",
        "timeout": 30,
        "matcher": "curl|wget|nc"
      }
    ]
  }
}
```

**退出码行为：**

- 退出码 `0` - Hook 成功，使用 JSON 输出
- 退出码 `2` - 阻止该操作 (等同于返回 `permission: "deny"`)
- 其他退出码 - Hook 失败，但操作继续执行 (默认失败放行)

###### 基于提示的 Hook

提示 Hook 使用 LLM 来判断自然语言条件。它们适用于在不编写自定义脚本的情况下执行策略。

```json
{
  "hooks": {
    "beforeShellExecution": [
      {
        "type": "prompt",
        "prompt": "Does this command look safe to execute? Only allow read-only operations.",
        "timeout": 10
      }
    ]
  }
}
```

**功能：**

- 返回结构化的 `{ ok: boolean, reason?: string }` 响应
- 使用快速模型进行快速评估
- `$ARGUMENTS` 占位符会自动替换为 hook 输入的 JSON
- 如果缺少 `$ARGUMENTS`，会自动追加 hook 输入
- 可选的 `model` 字段可用于覆盖默认的 LLM 模型

##### 配置

在 `hooks.json` 文件中定义 hooks。配置可以存在于多个级别。来自每个来源的所有匹配 hooks 都会运行；当响应发生冲突时，合并时优先采用优先级更高的配置源：

```txt
~/.cursor/
├── hooks.json
└── hooks/
    ├── audit.sh
    └── block-git.sh
```



### MCP

##### 什么是 MCP？

[模型上下文协议 (MCP)](https://modelcontextprotocol.io/introduction) 使 Cursor <font color=red>能够 **连接到外部工具和数据源**</font>。

###### 为什么使用 MCP？

MCP 可将 Cursor 连接到外部系统和数据。无需反复解释您的项目结构，直接与您的工具集成即可。

您可以使用任何能够输出到 `stdout` 或提供 HTTP 端点的语言来编写 MCP 服务器，例如 Python、JavaScript、Go 等。

在 [Cursor Marketplace](https://cursor.com/marketplace) 浏览官方插件。社区插件和 MCP 服务器请浏览 [cursor.directory](https://cursor.directory/)。

###### 工作原理

MCP 服务器通过该协议提供能力，将 Cursor 连接到外部工具或数据源。

<font color=dodgerBlue>Cursor 支持三种传输方式：</font>

| 传输方式                  | 执行环境  | 部署          | 用户  | 输入          | 认证    |
| --------------------- | ----- | ----------- | --- | ----------- | ----- |
| **`stdio`**           | 本地    | 由 Cursor 管理 | 单用户 | Shell 命令    | 手动    |
| **`SSE`**             | 本地/远程 | 部署为服务器      | 多用户 | SSE 端点 URL  | OAuth |
| **`Streamable HTTP`** | 本地/远程 | 部署为服务器      | 多用户 | HTTP 端点 URL | OAuth |

###### 协议和扩展支持

<font color=dodgerBlue>Cursor 支持以下 MCP 协议能力和扩展：</font>

|功能|支持情况|描述|
|---|---|---|
|**工具**|支持|供 AI 模型调用的函数|
|**Prompts**|支持|面向用户的模板化消息和工作流|
|**Resources**|支持|可读取和引用的结构化数据源|
|**Roots**|支持|由服务器发起、用于确定 URI 或文件系统边界的查询|
|**Elicitation**|支持|由服务器发起、向用户请求更多信息的请求|
|**应用 (扩展)**|支持|由 MCP 工具返回的交互式 UI 视图|

###### MCP 应用

Cursor 支持 [MCP 应用扩展](https://modelcontextprotocol.io/extensions/apps/overview)。MCP 工具除了标准工具输出外，还可以返回交互式 UI。

MCP 应用<font color=red>**遵循渐进增强原则**</font>。如果宿主无法渲染应用 UI，同一个工具仍然可以通过常规的 MCP 响应正常工作。

##### 常见问题

###### MCP 服务器有什么作用？

MCP 服务器可将 Cursor 连接到 Google Drive、Notion 等外部工具和 其他服务，把文档和需求纳入你的编码工作流。

###### 我可以将 MCP 服务器用于敏感数据吗？

可以，但请遵循安全最佳实践：

- 对机密信息使用环境变量，切勿将其硬编码
- 使用 `stdio` 传输方式在本地运行敏感服务器
- 将 API 密钥权限限制在最低必要范围内
- 连接敏感系统前先评审服务器代码
- 考虑在隔离环境中运行服务器



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




## Spec Driven Dev ( SDD )

### OpenSpec

Now tell your AI: `/opsx:propose <what-you-want-to-build>`

If you want the expanded workflow (`/opsx:new`, `/opsx:continue`, `/opsx:ff`, `/opsx:verify`, `/opsx:sync`, `/opsx:bulk-archive`, `/opsx:onboard`), select it with `openspec config profile` and apply with `openspec update`.

###### Why OpenSpec?

AI coding assistants are <font color=lightSeaGreen>powerful but unpredictable</font> when requirements live only in chat history. OpenSpec <font color=red>adds a lightweight spec layer</font> so you <font color=fuchsia>agree on what to build before any code is written</font>.

- **Agree before you build** — <font color=red>human and AI align on specs</font> before code gets written
- **Stay organized** — each change gets its own folder with proposal, specs, design, and tasks
- **Work fluidly** — update any artifact anytime, no rigid phase gates
- **Use your tools** — works with 20+ AI assistants via slash commands

###### OpenSpec philosophy

we serve a wide variety of users across different coding agents, models, and use cases. Changes should work well for everyone.


#### Getting Started

##### How It Works

###### Default quick path (core profile)

```
/opsx:propose ──► /opsx:apply ──► /opsx:archive
```

###### Expanded path (custom workflow selection):

```
/opsx:new ──► /opsx:ff or /opsx:continue ──► /opsx:apply ──► /opsx:verify ──► /opsx:archive
```

The default global profile is `core`, which includes `propose`, `explore`, `apply`, and `archive`. <font color=red>You can enable the expanded workflow commands with `openspec config profile` and then `openspec update`</font>.

##### What OpenSpec Creates

After running `openspec init`, your project has this structure:

```txt
openspec/
├── specs/              # Source of truth (your system's behavior)
│   └── <domain>/
│       └── spec.md
├── changes/            # Proposed updates (one folder per change)
│   └── <change-name>/
│       ├── proposal.md
│       ├── design.md
│       ├── tasks.md
│       └── specs/      # Delta specs (what's changing)
│           └── <domain>/
│               └── spec.md
└── config.yaml         # Project configuration (optional)
```

###### Two key directories

- **`specs/`** - The source of truth. <font color=red>These specs describe how your system currently behaves</font>. **Organized by domain** (e.g., `specs/auth/`, `specs/payments/`).
  
- **`changes/`** - Proposed modifications. Each change gets its own folder with all related artifacts. <font color=red>**When a change is complete, its specs merge into the main `specs/` directory**</font>.

##### Understanding Artifacts

Each change folder contains artifacts that guide the work:

| Artifact      | Purpose                                                                        |
| ------------- | ------------------------------------------------------------------------------ |
| `proposal.md` | The "why" and "what" - captures intent, scope, and approach                    |
| `specs/`      | <font color=red>Delta specs</font> showing ADDED/MODIFIED/REMOVED requirements |
| `design.md`   | The "how" - technical approach and architecture decisions                      |
| `tasks.md`    | Implementation checklist with checkboxes                                       |
> [!TIP]
> Delta 即：差异
###### Artifacts build on each other

```txt
proposal ──► specs ──► design ──► tasks ──► implement
   ▲           ▲          ▲                    │
   └───────────┴──────────┴────────────────────┘
            update as you learn
```

You can always go back and refine earlier artifacts as you learn more during implementation.

##### How Delta Specs Work

<font color=red>Delta specs are the **key concept in OpenSpec**</font>. They show what's changing relative to your current specs.
###### The Format

Delta specs use sections to indicate the type of change:

> [!NOTE]
> 注意下面的 “ADDED Requirements” 、“MODIFIED Requirements”、“REMOVED Requirements”

```md
# Delta for Auth

## ADDED Requirements

### Requirement: Two-Factor Authentication
The system MUST require a second factor during login.

#### Scenario: OTP required
- GIVEN a user with 2FA enabled
- WHEN the user submits valid credentials
- THEN an OTP challenge is presented

## MODIFIED Requirements

### Requirement: Session Timeout
The system SHALL expire sessions after 30 minutes of inactivity.
(Previously: 60 minutes)

#### Scenario: Idle timeout
- GIVEN an authenticated session
- WHEN 30 minutes pass without activity
- THEN the session is invalidated

## REMOVED Requirements

### Requirement: Remember Me
(Deprecated in favor of 2FA)
```

###### What Happens on Archive

When you archive a change:

1. **ADDED** requirements are appended to the main spec
2. **MODIFIED** requirements replace the existing version
3. **REMOVED** requirements are deleted from the main spec

The change folder moves to `openspec/changes/archive/` for audit history.



## Harness Engineering

###### 资料

- [最近爆火的 Harness Engineering 到底是啥？一期讲透！](https://www.bilibili.com/video/BV1Zk9FBwELs)

Martin Fowler 在 2026 年关于 AI 编程的[论述](https://martinfowler.com/articles/harness-engineering.html)中指出：**Agent = Model + Harness**



## 其他笔记

#### 《怎么来选 AI 中转站？我最看重的是 Cache》笔记

##### 输入与输出的成本

Manus 官方公布过一组数据：**在 Agent 运行过程中，输入和输出的 token 占比大概是 100:1。**

100:1 什么概念？你的 Agent 每生成 1 个 token 的回答，背后有 100 个 token 的输入在支撑。

<font color=dodgerBlue>为什么输入会这么爆炸？</font>

其实很简单。Agent 是多轮对话的，每一轮都要把完整的对话历史发给模型。第一轮你发了 5000 token，第二轮就变成了 8000，第三轮 12000……到了第二十轮，输入可能已经膨胀到 10 万 token 了。而每一轮的输出可能就几百 token。

这意味着什么？**你的 API 账单里，90% 以上的钱都花在了输入 token 上。**

但如果有 Prompt Cache，cache 命中的输入 token 价格只有原来的 **1/10**。

##### Prompt Cache 到底是什么

很多人听过 KV Cache，但搞不清楚它和 Prompt Cache 的关系。简单说一下。

**KV Cache 是模型推理层的东西。** 大模型做推理的时候，要对整个输入做 Attention 计算——算出每个 token 和其他所有 token 的关系。这个计算非常昂贵。<font color=dodgerBlue>KV Cache 做的事情是</font>：<font color=fuchsia>如果你这次发的输入和上次有一段相同的前缀，那这段前缀的计算结果可以直接复用，不用重新算</font>。

**Prompt Cache 是 API 层的东西。** 它是 API 提供商基于 KV Cache 提供的一个计费优化——<font color=red>如果你的请求里有一段内容之前发过，API 不对这段内容收全价，而是收一个大幅折扣的"缓存读取"价格</font>。

<font color=fuchsia>核心原理就一句话：**前缀匹配。**</font> 你的输入从第一个字符开始，和上一次请求的前缀一模一样的部分，就能命中缓存。哪怕中间差了一个字符，从那个字符往后全部 cache miss。

<font color=fuchsia>**这就是为什么在做 Agent 的时候，system prompt 要放在最前面，而且不能变**</font>——因为它是所有请求共享的前缀，一变就全废了。

各家 API 的缓存折扣大概是这样的：

| 提供商             | 缓存命中折扣 | 缓存方式         |
| :----------------- | :----------- | :--------------- |
| Anthropic (Claude) | 90% off      | 显式标记         |
| DeepSeek           | 90% off      | 自动             |
| Gemini             | 90% off      | 隐式、显式都可以 |
| 通义千问           | ~90% off     | 显式标记         |

可以看到，<font color=red>大部分主流模型的缓存折扣都在 90% 左右</font>。省下来的钱，在高频 Agent 场景下是非常可观的。

##### 为什么很多中转站的 Prompt Cache 基本无效？

道理都懂了，但问题来了——**不是所有中转站都能让你享受到 Prompt Cache 的红利。**

<font color=dodgerBlue>有两个关键因素会让缓存彻底失效</font>。

###### 第一个：API Key 轮转

大部分中转站的商业模式是这样的：它背后有一堆 API Key，你的请求进来之后，通过负载均衡轮转分配到不同的 Key 上。这一次用 Key A，下一次用 Key B，再下一次用 Key C。

Prompt Cache 的前提是什么？**同一个账号、同一个 API Key。** 你第一轮请求走了 Key A，写入了缓存。第二轮请求被轮转到 Key B 了——Key B 上根本没有你之前的缓存，直接 miss。

第三轮又轮到 Key C，还是 miss。

你以为自己在享受缓存，实际上每一轮都是全价。

###### 第二个：缓存时间窗口

就算中转站没有做 Key 轮转，Prompt Cache 也有一个时间限制。<font color=red>Anthropic 的默认 TTL 是 5 分钟</font>——也就是说，<font color=red>如果你两次请求的间隔超过 5 分钟，缓存就过期了</font>。

Agent 跑起来的时候，两次请求之间一般就几秒钟，5 分钟完全够用。但如果中转站的调度延迟比较高，或者中间有排队机制，实际的请求间隔可能被拉长，缓存命中率就会下降。

所以你在选中转站的时候，关键要看的不是它的价格倍率多低——倍率再低，cache 命中不了，实际成本可能比倍率高但 cache 能命中的平台还贵。

摘自：[怎么来选 AI 中转站？我最看重的是 Cache](https://mp.weixin.qq.com/s/CenuJzCuW4DmUl3hCj7L6Q)