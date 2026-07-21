# AI 概念与工具使用笔记



## 通用概念

###### 资料

- [【闪客】大模型已死？上帝视角拆解三年 LLM 架构演进！](https://www.bilibili.com/video/BV1mxR9BiEm8)

#### 召回率

> [!NOTE]
> 
> 这个概念涉及到 “混淆矩阵” ( confusion matrix ) 概念

#### QKV

QKV 即 Query, Key, Value

#### BM25

###### 背景

在人工智能（AI）和自然语言处理（NLP）领域，**BM25**（Best Matching 25）是一个非常经典且极其重要的**信息检索（Information Retrieval）算法**。

尽管近年来基于深度学习的向量检索（Dense Retrieval / Embeddings）大行其道，但 BM25 并没有被淘汰。相反，<font color=red>它在当前最火热的 **RAG（检索增强生成）** 和 **大模型（LLM）应用** 中扮演着不可或缺的“黄金配角”角色</font>。

##### 核心原理

BM25 是一种用来**计算搜索词（Query）与文档（Document）之间相关性得分**的统计算法。它是著名的 TF-IDF 算法的升级版。

<font color=dodgerBlue>当你在搜索框输入一句话时，**BM25 会根据以下三个核心维度来打分**</font>：

- **IDF（逆文档频率 - 词的重要性）** ：如果一个词在所有文档中都出现（比如“的”、“是”），它就不重要，得分很低；<font color=red>如果一个词很罕见</font>（比如“量子纠缠”、“奥本海默”），<font color=red>它就极其重要，得分很高</font>。
  
- **TF（词频 - 且带有“饱和度”）** ：一个词在当前文档里出现的次数越多，文档越相关。但是！**BM25 引入了 “词频饱和” 概念**。在传统的 TF-IDF 中，词出现 100 次的权重是出现 10 次的 10 倍；而在 BM25 中，当词频达到一定程度后，得分的增长会逐渐趋于平缓（饱和）。因为一篇文章提到“苹果” 5 次和 10 次，都足以说明这是一篇关于苹果的文章，100 次并不会让它变得比 10 次相关 20 倍。
  > [!NOTE]
  > 有点类似于归一化
  
- **文档长度归一化（Document Length Normalization）** ：长文章天然包含更多的词。如果一篇文章有一万字，里面刚好出现了几次你的搜索词，它不一定比一篇只有一百字但全篇都在讲你搜索词的文章更相关。BM25 会惩罚那些 “废话连篇” 的长文档，补偿精炼的短文档。

##### BM25 在 AI 与大模型时代的地位

在 LLM（大语言模型）时代，为什么还要了解几十年前发明的 BM25？答案在于 **RAG（检索增强生成）** 架构。

现在最流行的 AI 应用（如 ChatGPT 的联网搜索、企业内部知识库问答）都是先去数据库里“检索”相关资料，然后再喂给大模型去“生成”回答。

在这个“检索”环节，目前有两大流派：

1. **稠密检索（Dense Retrieval / 向量检索）** ：用 AI 模型把文字变成多维向量，计算语义相似度（比如知道“苹果手机”和“iPhone”是同一个东西）。
   
2. **稀疏检索（Sparse Retrieval）** ：这就是 **BM25** 的地盘，基于字词的精确匹配。
   
###### 为什么 AI 时代离不开 BM25？（混合检索 Hybrid Search）

纯粹的向量检索（AI Embeddings）存在致命弱点：**对专有名词、编号、特定术语的敏感度极低**。

- 例子： 如果你搜索 报错代码 ERR_NETWORK_404，向量模型可能会给你返回关于 网络中断提示 ERR_SYS_502 的文档，因为它觉得这两句话“语义上很相似”。但这对于排查 Bug 是灾难性的。
  

而 **BM25 是字面匹配的王者**。只要文档里有 ERR_NETWORK_404 这个精确的词，BM25 就能把它揪出来。

因此，当今业界最顶级的 AI 检索方案是 **混合检索（Hybrid Search） = BM25 + 向量检索**。

- 用向量检索兜底，理解用户的口语化提问和同义词（语义理解）。
  
- 用 BM25 精确打击，确保关键的人名、地名、商品型号、代码不被遗漏（精确匹配）。
  
- 最后使用一个重排模型（Reranker）将两者的结果融合。

##### BM25 的优缺点总结

###### 优势 (Pros)

1. **无需训练（Zero-shot）：** 向量检索需要庞大的算力去训练模型，而 BM25 是基于统计的，开箱即用，不需要任何训练数据，甚至不需要 GPU。
   
2. **极度高效：** 基于底层的“倒排索引”技术（类似字典的目录），在亿级别的数据量下也能在几毫秒内返回结果（Elasticsearch 等搜索引擎的核心算法就是 BM25）。
   
3. **精准打击：** 对特定的专有名词、长串数字、生僻字的匹配效果碾压现代 AI 模型。
   
4. **可解释性强：** 如果一个文档被排在第一名，你能通过公式清晰地算出来是因为哪个词匹配上了，而神经网络/向量检索往往是个“黑盒”。
   
###### 劣势 (Cons)

1. **没有语义理解能力：** 它是“字面派”。如果你搜“自行车”，文档里只有“单车”，BM25 就找不到它（即“词表不匹配”问题 Vocabulary Mismatch）。
   
2. **无法理解语境：** 无法区分“苹果（水果）”和“苹果（公司）”。
   
3. **对错别字敏感：** 搜“泰勒斯威夫特”，如果文档里写的是“泰勒斯韦夫特”，BM25 可能会错过，而 AI 向量通常能兼容这种错误。
   
##### 总结

在 AI 时代，BM25 并没有过时。如果说现代的 AI 向量检索是一个能听懂你言外之意的**“聪明人”**，那么 BM25 就是一个过目不忘、极其严谨的**“档案管理员”**。在构建强大的 AI 应用（尤其是 RAG 系统）时，将两者结合才是当下的最佳实践。

摘自：[AI Studio 回答](https://aistudio.google.com/app/prompts?state=%7B%22ids%22:%5B%221pTFa9xI_7rwJRoGd_m3bp-sL5btpsxAL%22%5D,%22action%22:%22open%22,%22userId%22:%22113986502377656987379%22,%22resourceKeys%22:%7B%7D%7D&usp=sharing)

### RAG

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

   > [!NOTE]
   > 
   这里的算法是：$\cos\theta=\frac{a \cdot b}{| a | \times | b |}$

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


***

#### 其他笔记

###### 如何评估 RAG

不能只看最终答案“读起来是否不错”，而要分别评估检索和生成。

检索层常用指标：

- `Recall@K`：正确资料是否出现在前 K 条结果中。
- `Precision@K`：前 K 条结果中有多少真正相关。
- `MRR`：第一个正确结果排得是否足够靠前。
- `nDCG`：整体排序质量是否合理。

生成层常关注：

- 答案是否正确。
- 是否忠实于提供的资料。
- 是否回答了用户真正的问题。
- 引用是否支持对应结论。
- 资料不足时是否正确拒答。
- 是否泄露无权访问的信息。

工程上应建立一套带标准答案和标准证据的测试集，并持续做回归测试。只测几十个“演示效果很好”的问题，通常无法代表真实质量。

###### 常见失败模式

RAG 失败通常并不是“模型不够强”，而是链路中的某一环出了问题：

- 正确文档没有进入知识库。
- PDF 或表格解析错误。
- 切块破坏了语义。
- 查询改写改变了原意。
- 检索只找到表面相关内容。
- 重排模型排除了正确证据。
- 旧版本文档排名高于新版本。
- 上下文过长，模型忽略关键证据。
- 模型拥有证据却仍然编造。
- 权限过滤在检索后才执行，造成数据泄露。
- 文档中的恶意提示影响模型行为。
- 用户的问题本来就需要计算、数据库查询或实时 API，而不是文档检索。

因此，调试 RAG 时应保存完整链路：原始问题、改写结果、召回内容、排序分数、最终上下文、模型输出和引用映射。

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

- **云端 Agent**（Web 应用、API 服务、移动端）：Skills + MCP 是最优解。<font color=red>没有本地环境，**必须走远程协议**</font>。而且云端 Agent 通常需要对接多个第三方服务，MCP 的标准化在这里价值巨大——一个 Server 写好，Claude、Codex、Cursor 全都能用。

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




### 杂项工具

- [edge-tts](https://github.com/rany2/edge-tts) ：免费的 tts 工具
- [hyperframes](https://github.com/heygen-com/hyperframes) ：AI 视频生成工具

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

  要控制在压缩期间保留的内容，请在 CLAUDE.md 中添加 “Compact Instructions” 部分 <font color=dodgerBlue>**或**</font> <font color=red>**使用焦点运行 `/compact`（如 `/compact focus on the API changes`）**</font>。

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

> [!TIP] 相关历史
> 
> 2025 年 5 月，Sourcegraph 旗下的 AMP 率先提议统一标准，建议用 `AGENT.md`（单数），并注册了 agent.md 域名。随后 OpenAI 宣布买下了 agents.md 域名，提议用 `AGENTS.md`（复数），理由是多个 Agent 会共用同一份配置。<font color=lightSeaGreen>AMP 随即主动让步对齐，将 agent.md 重定向到 agents.md</font>。
> 
> 摘自：[一个文件让 AI Coding 效率翻倍：AGENTS.md 实践指南](https://mp.weixin.qq.com/s/fBBBSfQajYjYtngZAitZCA)

Claude Code 读取 `CLAUDE.md`，而不是 `AGENTS.md`。<font color=dodgerBlue>**如果你的存储库已经为其他编码代理使用 `AGENTS.md`**</font>，<font color=red>创建一个导入它的 `CLAUDE.md`</font>，<font color=red>这样两个工具都可以读取相同的指令而无需重复</font>。你也可以在导入下方添加 Claude 特定的指令。Claude 在会话开始时加载导入的文件，然后附加其余部分：

```md
@AGENTS.md

## Claude Code

对 `src/billing/` 下的更改使用 Plan Mode。
```

> [!TIP]
> 
> Claude Code 虽然仍用 CLAUDE.md，但内容完全通用，一个软链接即可兼容：`ln -s AGENTS.md CLAUDE.md`。
> 
> 摘自：[一个文件让 AI Coding 效率翻倍：AGENTS.md 实践指南](https://mp.weixin.qq.com/s/fBBBSfQajYjYtngZAitZCA)

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

#### 选择权限模式

控制 Claude 在编辑文件或运行命令前是否询问。<font color=lightSeaGreen>在 CLI 中使用 Shift+Tab 循环切换模式</font>，或在 VS Code、Desktop 和 claude.ai 中使用模式选择器。

当 Claude 想要编辑文件、运行 shell 命令或发出网络请求时，它会暂停并要求您批准该操作。<font color=lightSeaGreen>权限模式控制暂停发生的频率</font>。您选择的模式塑造了会话的流程：默认模式让您在操作进行时审查每个操作，而更宽松的模式让 Claude 在更长的不间断时间内工作，然后报告完成情况。为敏感工作选择更多监督，或在您信任方向时选择更少中断。

##### 可用模式

每种模式在便利性和监督之间做出不同的权衡。下表显示了在每种模式中 Claude 无需权限提示即可执行的操作。

|模式|无需询问即可运行的操作|最适合|
|---|---|---|
|`default`|仅读取|入门、敏感工作|
|[`acceptEdits`](https://code.claude.com/docs/zh-CN/permission-modes#auto-approve-file-edits-with-acceptedits-mode)|读取、文件编辑和常见文件系统命令（`mkdir`、`touch`、`mv`、`cp` 等）|迭代您正在审查的代码|
|[`plan`](https://code.claude.com/docs/zh-CN/permission-modes#analyze-before-you-edit-with-plan-mode)|仅读取|在更改代码库前进行探索|
|[`auto`](https://code.claude.com/docs/zh-CN/permission-modes#eliminate-prompts-with-auto-mode)|所有操作，带后台安全检查|长时间任务、减少提示疲劳|
|[`dontAsk`](https://code.claude.com/docs/zh-CN/permission-modes#allow-only-pre-approved-tools-with-dontask-mode)|仅预先批准的工具|锁定的 CI 和脚本|
|[`bypassPermissions`](https://code.claude.com/docs/zh-CN/permission-modes#skip-all-checks-with-bypasspermissions-mode)|所有操作，带后台安全检查|仅隔离容器和 VM|

在除 `bypassPermissions` 外的每种模式中，对[受保护路径](https://code.claude.com/docs/zh-CN/permission-modes#protected-paths)的写入永远不会自动批准，保护仓库状态和 Claude 自己的配置免受意外破坏。模式设置基线。

在顶部分层[权限规则](https://code.claude.com/docs/zh-CN/permissions#manage-permissions)以在除 `bypassPermissions` 外的任何模式中预先批准或阻止特定工具，`bypassPermissions` 完全跳过权限层。

##### 切换权限模式

您可以在会话期间、启动时或作为持久默认设置切换模式。模式通过这些控制设置，而不是通过在聊天中询问 Claude。选择下面的您的界面以查看如何更改它。

###### CLI

**在会话期间**：按 `Shift+Tab` 循环切换 `default` → `acceptEdits` → `plan`。当前模式显示在状态栏中。并非每种模式都在默认循环中：

- `auto`：当您的账户满足 [auto mode 要求](https://code.claude.com/docs/zh-CN/permission-modes#eliminate-prompts-with-auto-mode)时出现；循环到 auto 会显示一个选择加入提示，直到您接受它，或选择**不，不再询问**以从循环中移除 auto
- `bypassPermissions`：在您使用 `--permission-mode bypassPermissions`、`--dangerously-skip-permissions` 或 `--allow-dangerously-skip-permissions` 启动后出现；`--allow-` 变体将模式添加到循环中而不激活它
- `dontAsk`：永远不会出现在循环中；使用 `--permission-mode dontAsk` 设置它

启用的可选模式在 `plan` 之后插入，`bypassPermissions` 优先，`auto` 最后。如果您同时启用了两者，您将在前往 `auto` 的途中循环通过 `bypassPermissions`。

**启动时**：将模式作为标志传递。

```sh
claude --permission-mode plan
```

**作为默认设置**：在[设置](https://code.claude.com/docs/zh-CN/settings#settings-files)中设置 `defaultMode`。

```json
{
  "permissions": {
    "defaultMode": "acceptEdits"
  }
}
```

相同的 `--permission-mode` 标志适用于 `-p` [非交互式运行](https://code.claude.com/docs/zh-CN/headless)。

#####  使用 acceptEdits mode 自动批准文件编辑

`acceptEdits` mode 让 Claude 在您的工作目录中创建和编辑文件而无需提示。状态栏显示 `⏵⏵ accept edits on` 当此模式处于活动状态时。

除了文件编辑外，`acceptEdits` mode 自动批准常见的文件系统 Bash 命令：`mkdir`、`touch`、`rm`、`rmdir`、`mv`、`cp` 和 `sed`。这些命令在以安全环境变量（如 `LANG=C` 或 `NO_COLOR=1`）或进程包装器（如 `timeout`、`nice` 或 `nohup`）为前缀时也会自动批准。与文件编辑一样，自动批准仅适用于您的工作目录或 `additionalDirectories` 内的路径。该范围外的路径、对[受保护路径](https://code.claude.com/docs/zh-CN/permission-modes#protected-paths)的写入和所有其他 Bash 命令仍然会提示。

当启用 [PowerShell tool](https://code.claude.com/docs/zh-CN/tools-reference#powershell-tool) 时，`acceptEdits` mode 也会自动批准 `Set-Content`、`Add-Content`、`Clear-Content` 和 `Remove-Item` 在范围内的路径上，以及它们的常见别名。相同的范围和受保护路径规则适用。

当您想在编辑器中或通过 `git diff` 之后审查更改而不是逐个批准每个编辑时，使用 `acceptEdits`。从默认模式按 `Shift+Tab` 一次进入它，或直接启动它：

```sh
claude --permission-mode acceptEdits
```

再次按 `Shift+Tab` 离开 plan mode 而不批准计划。

###### 审查并批准计划

当计划准备好时，Claude 呈现它并询问如何继续。从该提示您可以：

- 批准并在 auto mode 中启动
- 批准并接受编辑
- 批准并手动审查每个编辑
- 继续规划并提供反馈
- 使用 [Ultraplan](https://code.claude.com/docs/zh-CN/ultraplan) 进行基于浏览器的审查进行细化

批准计划会退出 plan mode 并将会话切换到每个批准选项描述的权限模式，因此 Claude 开始编辑。要再次规划，使用 `Shift+Tab` 循环回到 plan mode，或在下一个提示前加上 `/plan`。

按 `Ctrl+G` 在您的默认文本编辑器中打开提议的计划并在 Claude 继续之前直接编辑它。当启用 [`showClearContextOnPlanAccept`](https://code.claude.com/docs/zh-CN/settings#available-settings) 时，每个批准选项也提供首先清除规划上下文的选项。

接受计划也会根据计划内容自动命名会话，除非您已经使用 `--name` 或 `/rename` 设置了名称。

###### 将 plan mode 设置为默认值

要使 plan mode 成为项目的默认值，请在 `.claude/settings.json` 中设置 `defaultMode`：

```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

#### 常见工作流程

###### 按计划运行 Claude

假设您想让 Claude 自动定期处理任务，如每天早上审查开放的 PR、每周审计依赖项或在夜间检查 CI 失败。根据您希望任务运行的位置选择调度选项：

|选项|运行位置|最适合|
|---|---|---|
|[Routines](https://code.claude.com/docs/zh-CN/routines)|Anthropic 管理的基础设施|即使您的计算机关闭也应该运行的任务。也可以在 API 调用或 GitHub 事件上触发，除了计划。在 [claude.ai/code/routines](https://claude.ai/code/routines) 配置。|
|[桌面计划任务](https://code.claude.com/docs/zh-CN/desktop-scheduled-tasks)|您的机器，通过桌面应用|需要直接访问本地文件、工具或未提交更改的任务。|
|[GitHub Actions](https://code.claude.com/docs/zh-CN/github-actions)|您的 CI 管道|与存储库事件（如打开的 PR）相关的任务，或应该与工作流配置一起存在的 cron 计划。|
|[`/loop`](https://code.claude.com/docs/zh-CN/scheduled-tasks)|当前 CLI 会话|会话打开时的快速轮询。任务在您开始新对话时停止；`--resume` 和 `--continue` 恢复未过期的任务。|

> [!TIP]
> 
> 为计划任务编写提示时，明确说明成功是什么样的以及如何处理结果。任务自主运行，所以它不能提出澄清问题。例如：“审查标记为 `needs-review` 的开放 PR，对任何问题留下内联评论，并在 `#eng-reviews` Slack 频道中发布摘要。“

###### 编辑前规划

对于您想在接触磁盘前审查的更改，切换到 plan mode。Claude 读取文件并提出计划，但在您批准前不进行任何编辑。

```sh
claude --permission-mode plan
```

您也可以在会话中按 `Shift+Tab` 切换到 plan mode。有关批准流程和在文本编辑器中编辑计划，请参阅 [Plan Mode](https://code.claude.com/docs/zh-CN/permission-modes#analyze-before-you-edit-with-plan-mode)。

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



#### 交互模式

###### 使用 `!` 前缀的 Bash 模式

通过在输入前加上 `!` 来直接运行 bash 命令，无需通过 Claude：

```txt
! npm test
! git status
! ls -la
```

Bash 模式：

- 将命令及其输出添加到对话上下文
- 显示实时进度和输出
- 支持相同的 `Ctrl+B` 后台运行长时间运行的命令
- 不需要 Claude 解释或批准命令
- 支持基于历史的自动完成：键入部分命令并按 **Tab** 以从当前项目中的上一个 `!` 命令完成
- 使用 `Escape`、`Backspace` 或在空提示上使用 `Ctrl+U` 退出
- 将以 `!` 开头的文本粘贴到空提示中会自动进入 bash 模式，与键入的 `!` 行为相匹配

这对于快速 shell 操作同时保持对话上下文很有用。




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

#### 《你不知道的 Claude Code：架构、治理与工程实践》笔记

最直接的理解方式，是<font color=dodgerBlue>把 Claude Code 拆成六层来看</font>：

|层|职责|
|---|---|
|`CLAUDE.md` / rules / memory|长期上下文，告诉 Claude “是什么”|
|`Tools` / `MCP`|动作能力，告诉 Claude “能做什么”|
|`Skills`|按需加载的方法论，告诉 Claude “怎么做”|
|`Hooks`|强制执行某些行为，不依赖 Claude 自己判断|
|`Subagents`|隔离上下文的工作者，负责受控自治|
|`Verifiers`|<font color=red>验证闭环，让输出可验、可回滚、可审计</font>|

##### 它底层是怎么运行的

![Agent Loop](https://files.seeusercontent.com/2026/04/15/n1hR/agentic-loop.svg)

Claude Code 跑的是一个反复循环的代理过程：

```txt
收集上下文 → 采取行动 → 验证结果 → [完成 or 回到收集]
   ↑           ↓
CLAUDE.md  Hooks / 权限 / 沙箱
Skills     Tools / MCP
Memory
```

###### 五个诊断层面

| 层面                   | 核心问题                       | 主要载体                              |
| :--------------------- | :----------------------------- | :------------------------------------ |
| `Context surface`      | 哪些信息常驻，哪些按需加载     | `CLAUDE.md`、rules、memory、skills    |
| `Action surface`       | Claude 当前具备哪些动作能力    | built-in tools、MCP、plugins          |
| `Control surface`      | 哪些动作必须被约束、阻断或审计 | permissions、sandbox、hooks           |
| `Isolation surface`    | 哪些任务需要隔离上下文和权限   | subagents、worktrees、forked sessions |
| `Verification surface` | 如何判断任务完成且结果可信     | tests、lint、screenshots、logs、CI    |

##### 概念边界：搞清楚六个概念，别混用

| 概念              | 运行时角色            | 解决什么                               | 典型误用                          |
| :---------------- | :-------------------- | :------------------------------------- | :-------------------------------- |
| `CLAUDE.md`       | 项目级持久契约        | 每次会话都必须成立的命令、边界、禁止项 | 写成团队知识库                    |
| `.claude/rules/*` | 路径或语言相关规则    | 目录、文件类型或语言级局部规范         | 所有规则都堆到根 `CLAUDE.md`      |
| `Built-in Tools`  | 内置能力              | 读文件、改文件、跑命令、搜索           | 把所有集成都塞进 shell            |
| `MCP`             | 外部能力接入协议      | 让 Claude 访问 GitHub、Sentry、数据库  | 接太多 server，工具定义挤爆上下文 |
| `Plugin`          | 打包分发层            | 把 Skills/Hooks/MCP 一起分发           | 把 plugin 当成运行时 primitive    |
| `Skill`           | 按需加载的知识/工作流 | 给 Claude 一个方法包                   | skill 既像百科全书又像部署脚本    |
| `Hook`            | 强制执行规则的拦截层  | 在生命周期事件前后执行规则             | 用 hook 替代所有模型判断          |
| `Subagent`        | 隔离上下文的工作单元  | 并行研究、限制工具与权限               | 无边界 fan-out，治理失控          |

<font color=dodgerBlue>简单记</font>：给 Claude 新动作能力用 Tool/MCP，给它一套工作方法用 Skill，需要隔离执行环境用 Subagent，<font color=red>要强制约束和审计用 Hook</font>，<font color=lightSeaGreen>跨项目分发用 Plugin</font>。

##### 上下文工程：最重要的系统约束

Claude Code 的 200K 上下文并非全部可用：

```txt
200K 总上下文
├── 固定开销 (~15-20K)
│   ├── 系统指令: ~2K
│   ├── 所有启用的 Skill 描述符: ~1-5K
│   ├── MCP Server 工具定义: ~10-20K  ← 最大隐形杀手
│   └── LSP 状态: ~2-5K
│
├── 半固定 (~5-10K)
│   ├── CLAUDE.md: ~2-5K
│   └── Memory: ~1-2K
│
└── 动态可用 (~160-180K)
    ├── 对话历史
    ├── 文件内容
    └── 工具调用结果
```

![img](https://files.seeusercontent.com/2026/05/11/nS3p/formatwebp.jpg)

一个典型 MCP Server（如 GitHub）包含 20-30 个<font color=red>工具定义</font>，每个约 200 tokens，合计 **4,000-6,000 tokens**。接 5 个 Server，光这部分固定开销就到了 **25,000 tokens（12.5%）**。我第一次算出这个数字的时候，真没想到有这么多，在要读大量代码的场景，这 12.5% 真的很关键。

###### 推荐的上下文分层

```txt
始终常驻    → CLAUDE.md：项目契约 / 构建命令 / 禁止事项
按路径加载  → rules：语言 / 目录 / 文件类型特定规则
按需加载    → Skills：工作流 / 领域知识
隔离加载    → Subagents：大量探索 / 并行研究
不进上下文  → Hooks：确定性脚本 / 审计 / 阻断
```

###### 上下文最佳实践

- 保持 `CLAUDE.md` 短、硬、可执行，优先写命令、约束、架构边界。Anthropic 官方自己的 CLAUDE.md 大约只有 2.5K tokens，可以参考

- 把大型参考文档拆到 Skills 的 supporting files，不要塞进 SKILL.md 正文

- 使用 `.claude/rules/` 做路径/语言规则，不让根 `CLAUDE.md` 承担所有差异

- <font color=red>长会话主动用 `/context` 观察消耗</font>，不要等系统自动压缩后再补救

  <img src="https://files.seeusercontent.com/2026/05/11/eI6k/formatwebp-20260511112439535.jpg" alt="`/context` 命令输出，可以看到各来源的 token 占比" style="zoom:50%;" />

- 任务切换优先 `/clear`，同一任务进入新阶段用 `/compact`

- <font color=fuchsia>**把 Compact Instructions 写进 CLAUDE.md**</font>，<font color=red>压缩后必须保留什么由你控制，不由算法猜</font>

###### Tool Output 噪声：另一个隐形上下文杀手

前面算的是 MCP 工具定义的固定开销，但动态部分同样有个坑容易被忽视：Tool Output。<font color=dodgerBlue>`cargo test` 一次完整输出动辄几千行，`git log`、`find`、`grep` 在稍大的仓库里也能轻松塞满屏幕</font>。这些输出 Claude 并不需要全看，但只要它出现在上下文里，就是实实在在的 token 消耗，同样会挤掉对话历史和文件内容的空间。

后来看到 [RTK（Rust Token Killer）](https://www.rtk-ai.app/) 这个思路觉得挺对的，它做的事很简单：在命令输出到 Claude 之前自动过滤，只留决策需要的核心信息。比如 `cargo test`：

```txt
# Claude 看到的原始输出
running 262 tests
test auth::test_login ... ok
...（几千行）

# 走 RTK 之后
✓ cargo test: 262 passed (1 suite, 0.08s)
```

Claude 真正需要知道的就是「过了还是挂了，挂在哪里」，其他都是噪声。它通过 Hook 透明重写命令，对 Claude Code 来说完全无感。RTK 干的就是这件事，只是覆盖面更广，不用每条命令自己加 `| head -30`，项目[开源在 GitHub](https://github.com/rtk-ai/rtk)。

###### 压缩机制的陷阱

<img src="https://files.seeusercontent.com/2026/04/16/5Xwz/session-continuity.svg" alt="Session Continuity" />

默认压缩算法按 ”可重新读取” 判断，早期的 Tool Output 和文件内容会被优先删掉，顺带把**架构决策和约束理由**也一起扔了。两小时后再改，可能根本不记得两小时前定了什么，莫名其妙的 Bug 就是这么来的。

解决方案就是在 CLAUDE.md 里写明：

```md
## Compact Instructions

When compressing, preserve in priority order:

1. Architecture decisions (NEVER summarize)
2. Modified files and their key changes
3. Current verification status (pass/fail)
4. Open TODOs and rollback notes
5. Tool outputs (can delete, keep pass/fail only)
```

<font color=dodgerBlue>除了写 Compact Instructions，**还有一种更主动的方案**</font> ：<font color=red>在开新会话前，先让 Claude 写一份 HANDOFF.md，把当前进度、尝试过什么、哪些走通了、哪些是死路、下一步该做什么写清楚</font>。下一个 Claude 实例只读这个文件就能接着做，不依赖压缩算法的摘要质量：

> 在 HANDOFF.md 里写清楚现在的进展。解释你试了什么、什么有效、什么没用，让下一个拿到新鲜上下文的 agent 只看这个文件就能继续完成任务。

写完后快速扫一眼，有缺漏直接让它补，然后开新会话，把 HANDOFF.md 的路径发过去就行。

###### Plan Mode 的工程价值

![img](https://files.seeusercontent.com/2026/05/11/u1Xw/formatwebp-20260511114909863.jpg)

Plan Mode 的核心是把探索和执行拆开，探索阶段不动文件，确认方案后再执行：

- 探索阶段以只读操作为主
- Claude 可以先澄清目标和边界，再提交具体方案
- 执行成本在计划确认之后才发生

![img](https://files.seeusercontent.com/2026/05/11/1lKh/formatwebp-20260511114939221.jpg)

对于复杂重构、迁移、跨模块改动，这样做比 “急着出代码” 有用多了，在错误假设上越跑越偏的情况会少很多。按两下 `Shift+Tab` 进入 Plan Mode，<font color=red>**进阶玩法是开一个 Claude 写计划，再开一个 Codex 以 ”高级工程师” 身份审这个计划，让 AI 审 AI，效果很好**</font>。

##### Skills 设计：不是模板库，是用的时候才加载的工作流

Skill 官方描述是 “按需加载的知识与工作流”，描述符常驻上下文，完整内容按需加载。

###### 一个好 Skill 应该满足什么

- 描述要让模型知道 ”何时该用我”，而不是 “我是干什么的”，这两个差很多
- 有完整步骤、输入、输出和停止条件，别写了个开头没有结尾
- 正文只放导航和核心约束，大资料拆到 supporting files 里
- <font color=lightSeaGreen>有副作用的 Skill 要显式设置 `disable-model-invocation: true`</font>，不然 Claude 会自己决定要不要跑

###### Skill 怎么做到”按需加载”

Claude Code 团队在内部设计中反复强调 “progressive disclosure”，意思不是让模型一次性看到所有信息，而是先获得索引和导航，再按需拉取细节：

- `SKILL.md` 负责定义任务语义、边界和执行骨架
- <font color=red>supporting files 负责提供领域细节</font>
- 脚本负责确定性收集上下文或证据

一个比较稳定的结构长这样：

```txt
.claude/skills/
└── incident-triage/
    ├── SKILL.md
    ├── runbook.md
    ├── examples.md
    └── scripts/
        └── collect-context.sh
```

###### 描述符写短点，每个 Skill 都在偷你的上下文空间

每个启用的 Skill，描述符常驻上下文。优化前后差距很大：

```yml
# 低效（~45 tokens）
description: |
  This skill helps you review code changes in Rust projects.
  It checks for common issues like unsafe code, error handling...
  Use this when you want to ensure code quality before merging.

# 高效（~9 tokens）
description: Use for PR reviews with focus on correctness.
```

还有一个很重要的 `disable-auto-invoke` 使用策略：

- 高频（>1 次/会话）→ 保持 auto-invoke，优化描述符
- 低频（<1 次/会话）→ disable-auto-invoke，手动触发，描述符完全脱离上下文
- 极低频（<1 次/月）→ 移除 Skill，改为 AGENTS.md 中的文档

###### Skills 反模式

- 描述过短：`description: help with backend`（任何后端工作都能触发，哈哈）
- 正文过长：几百行工作手册全塞进 SKILL.md 正文
- 一个 Skill 覆盖 review、deploy、debug、docs、incident 五件事
- 有副作用的 Skill 允许模型自动调用

##### 工具设计：怎么让 Claude 少选错

我后面越用越觉得，<font color=lightSeaGreen>**给 Claude 的工具和给人写的 API 不是一回事**。给 agent 的工具，目标是让它用对</font>。

###### 好工具 vs 坏工具

| 维度     | 好工具                                       | 坏工具                        |
| :------- | :------------------------------------------- | :---------------------------- |
| 名称     | `jira_issue_get`, `sentry_errors_search`     | `query`, `fetch`, `do_action` |
| 参数     | `issue_key`, `project_id`, `response_format` | `id`, `name`, `target`        |
| 返回     | 与下一步决策直接相关的信息                   | 一堆 UUID、内部字段、原始噪声 |
| 规模     | 单一目标，边界清楚                           | 多个动作混杂，副作用不透明    |
| 成本     | 默认输出受控、可截断                         | 默认返回过大上下文            |
| 错误信息 | 包含修正建议                                 | 仅返回 opaque error code      |

<font color=dodgerBlue>几个实用设计原则：</font>

- 名称前缀按系统或资源分层：`github_pr_*`、`jira_issue_*`
- 对大响应支持 `response_format: concise / detailed`
- 错误响应要教模型如何修正，不要只抛 opaque error code
- 能合并成高层任务工具时，不要暴露过多底层碎片工具，避免 `list_all_*` 让模型自行筛选

###### 从 Claude Code 内部工具演进学到的

![img](https://files.seeusercontent.com/2026/05/11/e9Ym/aMfLwM.png)

我看到 Claude Code 团队内部工具的这段演进时，感觉还挺有意思。<font color=dodgerBlue>像这种需要在任务中途停下来问用户的场景，他们前后试了三种做法</font>：

- **第一版**：给已有工具（如 Bash）加一个 `question` 参数，让 Claude 在调用工具时顺带提问。结果 Claude 大多数时候直接忽略这个参数，继续往下跑，根本不停下来问。
- **第二版**：要求 Claude 在输出里写特定 markdown 格式，外层解析到这个格式就暂停。问题是没有强制约束，Claude 经常 “忘了” 按格式写，提问逻辑非常脆弱。
- **第三版**：做成独立的 `AskUserQuestion` 工具。<font color=red>Claude 想提问就必须显式调用它</font>，调用即暂停，没有歧义。比前两版靠谱多了。

下面这张图刚好能解释，为什么第三版明显更稳：

![img](https://files.seeusercontent.com/2026/05/11/laB3/JHqSz5.png)

左边（markdown 自由输出）太松，模型格式随意、外层解析脆弱；右边（ExitPlanTool 参数）太死，等到退出计划阶段提问已经太晚；`AskUserQuestion` 独立工具落在中间，结构化且随时可调用，是这三者里最稳定的设计。

说白了，既然你就是要 Claude 停下来问一句，那就直接给它一个专门的工具。加个 flag 或者约定一段输出格式，很多时候它一顺手就略过去了。

**Todo 工具的演进**：

![Updating with Capabilities - Tasks & Todos](https://files.seeusercontent.com/2026/05/11/4zeF/formatwebp-20260511123306134.jpg)

早期用 TodoWrite 工具 + 每 5 轮插入提醒让 Claude 记住任务。随着模型变强，这个工具反而成了限制，Todo 提醒让 Claude 认为必须严格遵循，无法灵活修改计划。挺有意思的教训：当初加这个工具是因为模型不够强，模型变强之后它反而变成了枷锁。值得过段时间回来检查一下，当初加的限制还成不成立。

**搜索工具的演进**：最初用 RAG 向量数据库，虽然快但需要索引、不同环境脆弱，最重要的是 **Claude 不喜欢用**。<font color=red>改成 Grep 工具让 Claude 自己搜索后，好用很多</font>。<font color=dodgerBlue>后来又发现一个顺带的好处</font>：Claude 读 Skill 文件，Skill 文件又引用其他文件，模型会递归读取，<font color=red>按需发现信息，**不需要提前塞进去**</font>，这个模式后来被叫做 ”渐进式披露”。

###### 什么时候不该再加 Tool

- 本地 shell 可以可靠完成的事情
- 模型只需要静态知识，不需要真正与外部交互
- 需求更适合 Skill 的工作流约束，而不是 Tool 的动作能力
- 还没验证过工具描述、schema 和返回格式能被模型稳定使用

##### Hooks：在 Claude 执行操作前后，强制插入你自己的逻辑

Hooks 很容易被当成 “自动运行的脚本”，但我自己用下来，觉得它更像是<font color=red>把一些不能交给 Claude 临场发挥的事情，重新收回到确定性的流程里</font>。

###### 当前支持的 Hook 点

![Hooks 配置界面](https://files.seeusercontent.com/2026/05/11/3Qnz/formatwebp-20260511123300004.jpg)

###### 适合 vs 不适合放到 Hooks 的

<font color=dodgerBlue>**适合**</font>：阻断修改受保护文件、Edit 后自动格式化 / lint / 轻量校验、SessionStart 后注入动态上下文（Git 分支、环境变量）、任务完成后推送通知。

<font color=dodgerBlue>**不适合**</font>：需要读大量上下文的复杂语义判断、长时间运行的业务流程、需要多步推理和权衡的决策，这些该在 Skill 或 Subagent 里。

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "pattern": "*.rs",
        "hooks": [
          {
            "type": "command",
            "command": "cargo check 2>&1 | head -30",
            "statusMessage": "Running cargo check..."
          }
        ]
      }
    ],
    "Notification": [
      {
        "type": "command",
        "command": "osascript -e 'display notification \"Task completed\" with title \"Claude Code\"'"
      }
    ]
  }
}
```

###### Hooks：越早发现错误，越省时间

![Hooks 在执行过程中的介入点](https://files.seeusercontent.com/2026/05/11/L6sw/formatwebp-20260511131017505.jpg)

在 100 次编辑的会话中，每次节省 30-60 秒，累积节省 1-2 小时，还挺可观的。**注意限制输出长度**（`| head -30`），避免 Hook 输出反而污染上下文。如果不想在每条命令后面手动加截断，可以看看第 3 节提到的 RTK，它把这件事系统化了。

###### Hooks + Skills + CLAUDE.md 三层叠加

- `CLAUDE.md`：声明 “提交前必须通过测试和 lint ”
- `Skill`：告诉 Claude 在什么顺序下运行测试、如何看失败、如何修复
- `Hook`：对关键路径执行硬性校验，必要时阻断

##### Subagents：派一个独立的 Claude 去干一件具体的事

Subagent 就是从主对话派出去的一个独立 Claude 实例，有自己的上下文窗口，只用你指定的工具，干完汇报结果。我用下来觉得它最大的价值不是 “并行”，而是隔离——扫代码库、跑测试、做审查这类会产生大量输出的事，塞进主线程很快就把有效上下文挤没了，交给 Subagent 做，主线程只拿一个摘要，干净很多。

<font color=dodgerBlue>Claude Code 内置了三个</font>：**Explore**（只读扫库，默认跑 Haiku 省成本）、**Plan**（规划调研）、**General-purpose**（通用），<font color=lightSeaGreen>也可以自定义</font>。

###### 配置时要显式约束

- `tools` / `disallowedTools`：限定能用什么工具，别给和主线程一样宽的权限
- `model`：<font color=lightSeaGreen>**探索任务用 Haiku / Sonnet**，重要审查用 Opus</font>
- <font color=red>`maxTurns`：防止跑飞</font>
- <font color=red>`isolation: worktree`：需要动文件时隔离文件系统</font>

另一个实用细节：长时间运行的 bash 命令可以按 `Ctrl+B` 移到后台，Claude 之后会用 BashOutput 工具查看结果，不会阻塞主线程继续工作。subagent 同理，直接告诉它「在后台跑」就行。

###### 几个常见反模式

- 子代理权限和主线程一样宽，隔离没有意义
- 输出格式不固定，主线程拿到没法用
- 子任务之间强依赖，频繁要共享中间状态，这种情况用 Subagent 不合适

##### Prompt Caching：Claude Code 内部架构的核心

<font color=red>Claude Code 的整个架构都是围绕 Prompt 缓存构建的</font>，高命中率不光省钱，速率限制也会松很多，Anthropic 甚至会对命中率跑告警，太低直接宣布 SEV。

###### 为缓存设计的 Prompt Layout

<img src="https://files.seeusercontent.com/2026/05/11/kH8o/formatwebp-20260511134025270.jpg" alt="Lay Out Your Prompt for Caching" style="zoom:40%;" />

<font color=red>Prompt 缓存是按 **前缀匹配** 工作的</font>，从请求开头到每个 `cache_control` 断点之前的内容都会被缓存。<font color=dodgerBlue>**所以这里的顺序很重要**</font>：

```txt
Claude Code 的 Prompt 顺序：
1. System Prompt → 静态，锁定
2. Tool Definitions → 静态，锁定
3. Chat History → 动态，在后面
4. 当前用户输入 → 最后
```

**破坏缓存的常见陷阱：**

- 在静态系统 Prompt 中放入带时间戳的内容（让它每次都变）
- 非确定性地打乱工具定义顺序
- 会话中途增删工具

<font color=dodgerBlue>像当前时间这种动态信息怎么办？</font> <font color=red>**别去动系统 Prompt，放到下一条消息里传进去就行**</font>。Claude Code 自己也是这么做的，用户消息里加 `<system-reminder>` 标签，系统 Prompt 不动，缓存也就不会被打坏。

###### 会话中途不要切换模型

Prompt 缓存是模型唯一的。假如你已经和 Opus 对话了 100K tokens，想问个简单问题，**切换到 Haiku 实际上比继续用 Opus 更贵**，因为要为 Haiku 重建整个缓存。确实需要切换的话，用 Subagent 交接：Opus 准备一条” 交接消息” 给另一个模型，说明需要完成的任务就行。

###### Compaction 的实际实现

![Forking Context — Compaction](https://cdn.fliggy.com/upic/0M2eXx.png?x-oss-process=image/auto-orient,1/resize,w_2000/format,webp)

上图是 Compaction（上下文压缩）的执行流程：左边是上下文快满时的状态，中间是 Claude Code 开一个 fork 调用，把完整对话历史喂给模型，<font color=dodgerBlue>加一句 “Summarize this conversation”</font> ，这一步命中缓存所以只需 1/10 的价格，右边是压缩完之后，原来几十轮对话被替换成一段 ~20k tokens 的摘要，System + Tools 还在，再挂上之前用到的文件引用，腾出空间继续新的轮次。

直觉上 Plan Mode 应该切换成只读工具集，但这会破坏缓存。实际实现是：`EnterPlanMode` 是模型可以自己调用的工具，检测到复杂问题时自主进入 plan mode，工具集不变，缓存不受影响。

##### 验证闭环：没有 Verifier 就没有工程上的 Agent

「Claude 说完成了」其实没啥用，你得能知道它做没做对、出了问题能退回来、过程还能查，这才算数。

###### Verifier 的层级

- 最低层：命令退出码、lint、typecheck、unit test
- 中间层：集成测试、截图对比、contract test、smoke test
- 更高层：生产日志验证、监控指标、人工审查清单

###### 在 Prompt、Skill 和 CLAUDE.md 中显式定义验证

```md
## Verification

For backend changes:

- Run `make test` and `make lint`
- For API changes, update contract tests under `tests/contracts/`

For UI changes:

- Capture before/after screenshots if visual

Definition of done:

- All tests pass
- Lint passes
- No TODO left behind unless explicitly tracked
```

写任务 Prompt 或 Skill 的时候，最好把验收标准提前说清楚。哪些命令跑完算完成，失败了先查什么，截图和日志看到什么才算过，这些越早讲明白，后面越省事。

##### 高频命令的工程意义

###### 会话连续性与并行

```sh
claude --continue               # 恢复当前目录最近会话，隔天接着做
claude --resume                 # 打开选择器恢复历史会话
claude --continue --fork    # 从已有会话分叉，同一起点不同方案
claude --worktree              # 创建隔离 git worktree
claude -p "prompt"            # 非交互模式，接入 CI / pre-commit / 脚本
claude -p --output-format json  # 结构化输出，便于脚本消费
```

###### 几个不常见但很好用的命令

- **`/simplify`**：对刚改完的代码做三维检查，代码复用、质量和效率，发现问题直接修掉。特别适合改完一段逻辑后立刻跑一遍，代替手动 review。

- **`/rewind`**：不是 “撤销”，而是<font color=red>回到某个会话 checkpoint 重新总结</font>。适合：<font color=red>**Claude 已沿错误路径探索太久；想保留前半段共识但丢掉后半段失败**</font>。

- **`/btw`**：<font color=red>在不打断主任务的前提下快速问一个侧问题</font>，适合 “两个命令有什么区别” 这类单轮旁路问答，不适合需要读仓库或调用工具的问题。

- **`claude -p --output-format stream-json`**：实时 JSON 事件流，适合长任务监控、增量处理、流式集成到自己的工具。

- **`/insight`**：<font color=fuchsia>让 Claude 分析当前会话，提炼出哪些内容值得沉淀到 CLAUDE.md</font>。用法是会话做了一段之后跑一次，它会指出 “这个约定你们反复提到，但没有写进契约” 之类的盲点，是迭代优化 CLAUDE.md 的好手段。

- **双击 ESC 回溯**：按两次 ESC 可以回到上一条输入重新编辑，不用重新手打。Claude 走偏了、或者上一句话没说清楚，双击 ESC 修改后重发，比重新开会话省事得多。

- **对话历史都在本地**：<font color=fuchsia>所有会话记录存放在 `~/.claude/projects/` 下</font>，文件夹名按项目路径命名（斜杠变横杠），每个会话是一个 `.jsonl` 文件。<font color=dodgerBlue>想找某个话题的历史</font>，直接 <font color=red>`grep -rl "关键词" ~/.claude/projects/` 就能定位</font>，或者直接 <font color=red>告诉 Claude「帮我搜一下之前关于 X 的讨论」，它会自己去翻</font>。

##### 如何写一个好的 CLAUDE.md

`CLAUDE.md` 在我看来更像是你和 Claude 之间的协作契约，不是团队文档，也不是知识库，<font color=red>里面只放那些每次会话都得成立的事</font>。

我自己的建议其实很简单，<font color=red>一开始甚至可以什么都不写</font>。<font color=lightSeaGreen>先用起来</font>，<font color=red>等你发现自己老是在重复同一件事，再把它补进去</font>。加法也不复杂，<font color=red>**输入 `#` 可以把当前对话里的内容直接追加进 CLAUDE.md**</font>，或者直接告诉 Claude「把这条加到项目的 CLAUDE.md 里」，它会知道该改哪个文件。

###### 应该放什么

- 怎么 build、怎么 test、怎么跑（最核心）
- 关键目录结构与模块边界
- 代码风格和命名约束
- 那些不明显的环境坑
- 绝对不能干的事（NEVER 列表）
- 压缩时必须保留的信息（Compact Instructions）

###### 不该放什么

- 大段背景介绍
- 完整 API 文档
- 空泛原则，如 “写高质量代码”
- Claude 通过读仓库即可推断的显然信息
- 大量背景资料和低频任务知识（这些放到 Skills）

###### 高质量模板

```md
# Project Contract

## Build And Test

- Install: `pnpm install`
- Dev: `pnpm dev`
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`
- Lint: `pnpm lint`

## Architecture Boundaries

- HTTP handlers live in `src/http/handlers/`
- Domain logic lives in `src/domain/`
- Do not put persistence logic in handlers
- Shared types live in `src/contracts/`

## Coding Conventions

- Prefer pure functions in domain layer
- Do not introduce new global state without explicit justification
- Reuse existing error types from `src/errors/`

## Safety Rails

## NEVER

- Modify `.env`, lockfiles, or CI secrets without explicit approval
- Remove feature flags without searching all call sites
- Commit without running tests

## ALWAYS

- Show diff before committing
- Update CHANGELOG for user-facing changes

## Verification

- Backend changes: `make test` + `make lint`
- API changes: update contract tests under `tests/contracts/`
- UI changes: capture before/after screenshots

## Compact Instructions

Preserve:

1. Architecture decisions (NEVER summarize)
2. Modified files and key changes
3. Current verification status (pass/fail commands)
4. Open risks, TODOs, rollback notes
```

###### 让 Claude 维护自己的 CLAUDE.md

我最喜欢的一个技巧：每次纠正 Claude 的错误后，让它自己更新 CLAUDE.md：

> “Update your CLAUDE.md so you don’t make that mistake again.”

Claude 在给自己补这类规则时其实还挺好用，用久了确实越来越少犯同样的错。不过也要定期 review，时间一长总会有些条目慢慢过时，当初有用的限制现在未必还适合。

##### 最近自己折腾中得到的新经验

###### “环境透明” 比你想象中重要

Claude Code 调用的都是真实的 shell、git、package manager 和本地配置。<font color=red>这里面只要有一层不透明，它就只能开始猜，一猜可靠性就掉</font>。这不是 Claude Code 特有的问题，很多 agent 都一样。

所以我后来很快就在 terminal 里加了个 `doctor` 命令，把环境状态、依赖和配置情况先统一收上来，输出一份结构化的健康报告。Claude Code 开始做事前先跑一次 doctor，确实能省掉很多 “环境没搞清楚就开干” 的问题。

> [!NOTE]
>
> 即：`claude doctor` 命令

另外我还发现，假如 CLI 本身就有 `init`、`config`、`reset` 这类语义清楚的子命令，Claude Code 用起来会稳不少，比让它自己去猜配置文件怎么摆要靠谱。先把状态收敛住，再暴露编辑入口，顺序一反过来就很容易乱。

###### 混合语言项目的 Hooks 实践

两套语言、两套检查，其实挺适合用 Hooks 按文件类型分别触发：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "pattern": "*.rs",
        "hooks": [{
          "type": "command",
          "command": "cargo check 2>&1 | head -30",
          "statusMessage": "Checking Rust..."
        }]
      },
      {
        "matcher": "Edit",
        "pattern": "*.lua",
        "hooks": [{
          "type": "command",
          "command": "luajit -b $FILE /dev/null 2>&1 | head -10",
          "statusMessage": "Checking Lua syntax..."
        }]
      }
    ]
  }
}
```

每次编辑完立刻知道有没有编译错误，比 “跑了一堆才发现最开始就挂了” 舒服得多。

###### 完整的工程化布局参考

如果你想给自己项目配一套比较完整的 Claude Code 工程布局，可以参考这个结构，不用全做，按需裁剪：

```txt
Project/
├── CLAUDE.md
├── .claude/
│   ├── rules/
│   │   ├── core.md
│   │   ├── config.md
│   │   └── release.md
│   ├── skills/
│   │   ├── runtime-diagnosis/     # 统一收集日志、状态和依赖
│   │   ├── config-migration/      # 配置迁移回滚防污
│   │   ├── release-check/         # 发布前校验、smoke test
│   │   └── incident-triage/       # 线上故障分诊
│   ├── agents/
│   │   ├── reviewer.md
│   │   └── explorer.md
│   └── settings.json
└── docs/
    └── ai/
        ├── architecture.md
        └── release-runbook.md
```

全局约束（`CLAUDE.md`）、路径约束（`rules`）、工作流（`skills`）和架构细节各归各位，Claude Code 跑起来会稳很多。<font color=dodgerBlue>假如你同时维护多个项目</font>，可以把稳定的个人基线放在 `~/.claude/`，各项目的差异放在项目级 `.claude/`，通过同步脚本分发，不同项目之间就不会互相污染了。

##### 常见反模式

| 反模式                 | 症状                                                       | 修复                                                         |
| :--------------------- | :--------------------------------------------------------- | :----------------------------------------------------------- |
| CLAUDE.md 当 wiki      | 每次加载污染上下文，关键指令被稀释                         | 只保留契约，资料拆到 Skills 和 rules                         |
| Skill 大杂烩           | 描述无法稳定触发，工作流冲突                               | 一个 Skill 只做一类事，副作用显式控制                        |
| 工具太多描述模糊       | 选错工具，schema 挤爆上下文                                | 合并重叠工具，做明确 namespacing                             |
| 没有验证闭环           | Claude 只能觉得自己完成了                                  | <font color=red>给每类任务绑定 verifier</font>               |
| 过度自治               | 多 agent 并行无边界，出错难止损                            | 角色 / 权限 / worktree 最小化，明确 maxTurns                 |
| 上下文不做切分         | 研究、实现、审查全堆在主线程，有效上下文被稀释             | 任务切换 `/clear`，阶段切换 `/compact`，重型探索交给 subagent（Explore → Main 模式） |
| 自治范围过宽但治理不足 | 多 agent、外部工具全开，但缺乏权限边界和结果回收边界       | <font color=red>permissions + sandbox + hooks + subagent 组合边界</font> |
| 已批准命令堆积不清理   | `settings.json` 里残留 `rm -rf` 等危险操作，一旦触发不可逆 | 定期审查 `.claude/settings.json` 的 `allowedTools` 列表      |

##### 配置健康检查

基于文章里的六层框架，我把这套检查整理成了一个开源 Skill 项目 [`tw93/waza`](https://github.com/tw93/waza)，可以一键检查你的 Claude Code 配置现在处于什么状态。

```sh
claude plugin marketplace add tw93/waza
claude plugin install health@waza
```

装好之后在任意会话里跑 `/health`，它会自动识别项目复杂度，对 `CLAUDE.md`、`rules`、`skills`、`hooks`、`allowedTools` 和实际行为模式各跑一遍检查，输出一份优先级报告：需要立刻修 / 结构性问题 / 可以慢慢做。

想知道自己的配置离这些原则差多远，跑一次 `/health` 是最快的方式。



## OpenAI & Codex

###### 资料

- [Codex 配置文档](https://developers.openai.com/codex/config-reference)

#### CLI 命令

> [!NOTE]
> 如下内容均是运行 `codex -h` 的输出，并截取了 Commands 一段的内容

| 命令             | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| `exec`           | Run Codex non-interactively [aliases: e]                     |
| `review`         | Run a code review non-interactively                          |
| `login`          | Manage login                                                 |
| `logout`         | Remove stored authentication credentials                     |
| `mcp`            | Manage external MCP servers for Codex                        |
| `plugin`         | Manage Codex plugins                                         |
| `mcp-server`     | Start Codex as an MCP server (stdio)                         |
| `app-server`     | [experimental] Run the app server or related tooling         |
| `remote-control` | [experimental] Manage the app-server daemon with remote control enabled |
| `app`            | Launch the Codex desktop app (opens the app installer if missing) |
| `completion`     | Generate shell completion scripts                            |
| `update`         | Update Codex to the latest version                           |
| `doctor`         | Diagnose local Codex installation, config, auth, and runtime health |
| `sandbox`        | Run commands within a Codex-provided sandbox                 |
| `debug`          | Debugging tools                                              |
| `apply`          | Apply the latest diff produced by Codex agent as a `git apply` to your local working tree [aliases: a] |
| `resume`         | Resume a previous interactive session (picker by default; use --last to continue the most recent) |
| `archive`        | Archive a saved session by id or session name                |
| `delete`         | Permanently delete a saved session by id or session name     |
| `unarchive`      | Unarchive a saved session by id or session name              |
| `fork`           | Fork a previous interactive session (picker by default; use --last to fork the most recent) |
| `cloud`          | [EXPERIMENTAL] Browse tasks from Codex Cloud and apply changes locally |
| `exec-server`    | [EXPERIMENTAL] Run the standalone exec-server service        |
| `features`       | Inspect feature flags                                        |
| `help`           | Print this message or the help of the given subcommand(s)    |



#### 一些配置

###### 默认允许联网

```toml
[sandbox_workspace_write]
network_access = true
```

###### 开启 goal 模式

```toml
[features]
goals = true
```

此外，根据 [@Lonely__MH 的推特动态](https://x.com/Lonely__MH/status/2050601931165532632) 还可以使用如下命令：

```sh
codex features enable goals
```

###### 开启 vim 模式

可以使用 `/vim` 切换 vim 模式开启状态

#### Plugins

##### Chrome 插件

在 Codex 和 Chrome 插件连接完成后，在 Codex 输入框中输入 `@chrome`，触发插件的运行

#### 使用经验

##### 内置 Browser 只能打开一个标签时，如何汇总多个页面的批注

在 Codex `26.715.31925` 中，同一任务只能添加一个内置 Browser 标签。这个限制会破坏原先的多页面批注工作流：无法同时打开多个页面，分别添加视觉批注后，再让同一个任务统一修改。

两个看似可行的替代方案都不完全等价：`@Chrome` 虽然能同时操作多个标签，但没有内置 Browser 的页面批注能力；拆成多个任务则会分散上下文，不便于最后统一处理所有页面。

当前比较可靠的临时办法，是在同一任务中逐页“固化”批注：

1. 在页面 A 添加全部批注。
2. 发送一条消息：`仅记录当前页面的这些批注，暂时不要修改，等待我继续标注其他页面。页面：/xxx，视口：desktop。`
3. 用同一个 Browser 标签导航到页面 B，继续批注并发送同样的记录消息。
4. 全部完成后发送：`现在综合本任务前面所有页面的批注统一修改，并逐页验证。`

批注一旦随消息发送，就会进入当前任务的历史上下文，不再依赖原 Browser 标签继续存在。因此，后续即使在同一个标签中切换页面，Codex 仍能综合前面已经提交的批注。

> [!warning] 局限
> 这种方法必须逐页发送批注，无法像以前那样把多个页面保留在多个标签中来回比较。目前也没有公开开关可以恢复同一任务中的多个内置 Browser 标签。

##### 如何在一个 session 中引用另一个 session 中的内容

> [!NOTE]
> 这个问题，我是直接在 codex 中问的 gpt5.5xh，codex 告诉我不是叫会话 session，而是叫 thread；但是找不到要复制的 thread ID，只能找到 session ID ；但是会话的链接( deeplink ) 的格式是 `codex://threads/<thread-session-uuid>`；总感觉 codex 内部也将 session 和 thread 混淆了。不过，和 codex 沟通时，还是需要将其称为 thread ID 而非 session ID

###### 可以用自然语言来描述

```md
请找到标题/内容包含“登录重构”的那个 session，读取最近几轮，然后基于它继续分析当前问题。
```

###### 也可以通过 session ID 来描述

```md
请读取 session 0199... 的最近 20 轮，把关键结论总结成当前上下文，然后继续做 X。
```

***

> [!NOTE]
> 
> 下面是 codex 回答中的补充

找 thread 的方式：

- 用 `Cmd+G` 搜索历史会话，按标题、内容片段或分支名找。
- 打开目标会话后可用 `/status` 查看当前 thread ID。
- 如果只是想打开另一个会话，可以用深链：`codex://threads/<thread-session-uuid>`。
- 如果想“以另一个会话为基础开新会话”，用 fork：它会保留已完成的历史 transcript。
- 如果想让当前会话操作另一个会话，也可以直接说“继续/发送消息到那个 thread”。

##### 如何在不移动项目的情况下，让一个 workspace 读到另一个 workspace 中的内容

关键是把“另一个项目目录”加入当前 thread 的可读边界。

推荐做成 **项目 A 可写，项目 B 只读**。在项目 A 里放一个 `.codex/config.toml`，例如：

```toml
default_permissions = "project_a_with_b_read"

[permissions.project_a_with_b_read]
description = "Edit project A, read project B as reference."
extends = ":workspace"

[permissions.project_a_with_b_read.filesystem]
"$HOME/path/to/project-b" = "read"
```

然后重开项目 A 的 thread，提示里直接写绝对路径或文件：

```md
请参考 `$HOME/path/to/project-b/src/foo.ts` 的实现，修改当前项目里的对应逻辑。
```

如果只是偶尔看一次，也可以直接在当前 thread 里给绝对路径；Codex 可能会因为超出当前 workspace 而请求批准。长期使用就别靠一次性批准，配置 profile 更稳。


#### subagent

###### Custom agents

Codex ships with built-in agents:

- `default`: general-purpose fallback agent.
- `worker`: execution-focused agent for implementation and fixes.
- `explorer`: read-heavy codebase exploration agent.

To define your own custom agents, <font color=red>add standalone TOML files under `~/.codex/agents/` for personal agents</font> or <font color=red>`.codex/agents/` for project-scoped agents</font>.

Each file defines one custom agent. Codex loads these files as configuration layers for spawned sessions, so custom agents can override the same settings as a normal Codex session config. That can feel heavier than a dedicated agent manifest, and the format may evolve as authoring and sharing mature.

Every standalone custom agent file <font color=dodgerBlue>must define</font>:

- `name`
- `description`
- `developer_instructions`

Optional fields such as `nickname_candidates`, `model`, `model_reasoning_effort`, `sandbox_mode`, `mcp_servers`, and `skills.config` inherit from the parent session when you omit them.

摘自：[OpenAI Developers - Subagents # Custom agents](https://developers.openai.com/codex/subagents#custom-agents)

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


## Agent

#### 《你不知道的 Agent：原理、架构与工程实践》笔记

这篇文章主要讲 Agent 架构里几块最影响工程效果的内容，包括控制流、上下文工程、工具设计、记忆、多 Agent 组织、评测、追踪和安全，最后再用 OpenClaw 的实现把这些设计原则串起来看一遍。

##### Agent Loop 的基本运转方式

Agent Loop 的核心实现逻辑抽象后其实不到 20 行代码：

```ts
const messages: MessageParam[] = [{ role: "user", content: userInput }];

while (true) {
  const response = await client.messages.create({
    model: "claude-opus-4-6",
    max_tokens: 8096,
    tools: toolDefinitions,
    messages,
  });

  if (response.stop_reason === "tool_use") {
    const toolResults = await Promise.all(
      response.content
        .filter((b) => b.type === "tool_use")
        .map(async (b) => ({
          type: "tool_result" as const,
          tool_use_id: b.id,
          content: await executeTool(b.name, b.input),
        }))
    );
    messages.push({ role: "assistant", content: response.content });
    messages.push({ role: "user", content: toolResults });
  } else {
    return response.content.find((b) => b.type === "text")?.text ?? "";
  }
}
```

对应的控制流如下，感知 -> 决策 -> 行动 -> 反馈四个阶段不断循环，直到模型返回纯文本为止：

![Agent Loop 控制流](https://cdn.tw93.fun/pic/react_loop_control_flow_en29.svg)

看过不少 Agent 实现和官方 SDK，结构都差不多，循环本身相当稳定，从最小实现一路扩展到支持子 Agent、上下文压缩和 Skills 加载，主循环基本没有变化，新增能力通常都是叠加在循环外部，而不是改动循环内部。

<font color=dodgerBlue>新能力基本只通过三种方式接入</font>：<font color=red>扩展工具集和 handler、调整系统提示结构、把状态外化</font>到文件或数据库，不应该让循环体本身变成一个巨大的状态机，模型负责推理，外部系统负责状态和边界，一旦这个分工确定下来，核心循环逻辑就很少需要频繁调整了。

###### Workflow 和 Agent 有什么区别

<font color=dodgerBlue>Anthropic 对这两类系统有一个直接区分</font>：**执行路径** <font color=red>由代码预先写死的是 Workflow</font>，<font color=red>由 LLM 动态决定下一步的是 Agent</font>，<font color=red>**核心区别在于控制权掌握在谁手里**</font>，现实中很多标着 Agent 的产品，深入看其实更接近 Workflow。

| 维度       | Workflow                       | Agent                                         |
| :--------- | :----------------------------- | :-------------------------------------------- |
| 控制权     | 代码预定义，同输入必走同一路径 | LLM 动态决策，可能需要评测验证                |
| 执行方式   | 工具顺序固定，错误走预设分支   | 工具按需选择，模型可尝试自我修复              |
| 状态与记忆 | 显式状态机，节点跳转清晰       | 隐式上下文，状态在对话历史中累积              |
| 维护成本   | 改流程需修改代码并重新部署     | 调整系统提示即可，无需重新部署                |
| 可观测性   | 日志定位节点，延迟可预估       | 需完整执行记录理解决策链，轮数不固定          |
| 人机协作   | 人在预设节点介入               | <font color=red>人在任意轮次介入或接管</font> |
| 适用场景   | 流程固定、输入边界清晰         | 需要中间推理与灵活判断                        |

放在一张图里看，会更直观：

![Workflow 与 Agent 对比](https://gw.alipayobjects.com/zos/k/dh/workflow_vs_agent.svg)

###### 五种常见控制模式

<font color=dodgerBlue>大多数 AI 系统拆开看，其实都是这五种模式的组合</font>，很多场景并不需要完整的 Agent 自主权，把其中几种模式搭起来就够了。

1. **提示链 Prompt Chaining**：任务拆成顺序步骤，每步 LLM 处理上一步的输出，中间可加代码检查点，适合生成后翻译、先写大纲再写正文这类线性流程。
2. **路由 Routing**：<font color=red>对输入分类，定向到对应的专用处理流程</font>，简单问题走轻量模型，复杂问题走强模型，技术咨询和账单查询走不同逻辑。
3. **并行 Parallelization**：两种变体：分段法把任务拆成独立子任务并发跑，投票法把同一任务跑多次取共识，适合高风险决策或需要多视角的场景。
4. **编排器-工作者 Orchestrator-Workers**：中央 LLM 动态分解任务，委派给工作者 LLM，综合结果，nanobot 的 `spawn` 工具和 learn-claude-code 的子 Agent 模式都是这个原型。
5. **评估器-优化器 Evaluator-Optimizer**：生成器产出，评估器给反馈，循环直到达标，适合翻译、创意写作这类质量标准难以用代码精确定义的任务。

![五种常见控制模式](https://gw.alipayobjects.com/zos/k/dw/five_agent_patterns.svg)

###### 什么场景选哪种模式

选型主要看两件事：任务确定性和验证能不能自动化。

| 场景                                   | 选什么                                    |
| :------------------------------------- | :---------------------------------------- |
| 流程固定 + 验收可代码判定              | Workflow / Prompt Chaining，不必上 Agent  |
| 输入可分类到不同分支                   | Routing                                   |
| 需要中间推理 + 验收清晰                | 单 Agent ReAct Loop                       |
| 任务可拆 + 子任务可并行 + 结论只需摘要 | Orchestrator-Workers，主 ReAct + 子 Agent |
| 质量标准难以代码化（翻译、创意）       | Evaluator-Optimizer                       |
| 高风险决策 + 需要多视角                | Parallelization 投票                      |

主 Agent 选 ReAct Loop，配上显式任务图；子 Agent 只带最小提示（Tooling、Workspace、Runtime），不带 Skills 和 Memory，避免权限外泄，也避免破坏隔离。多 Agent 不是默认选项，先把单 Agent 上限跑出来再扩展，协调开销经常超过并行收益。

摘自：[你不知道的 Agent：原理、架构与工程实践](https://tw93.fun/2026-03-21/agent.html)



### 三元 《吃透 AI Agent 开发》笔记

> [!NOTE]
> 
> 🔗 https://sitor.ai/

#### 搞定 Agent 六大支柱：今天出个 Manus 明天出个 OpenClaw，你到底应该学什么？

##### 一次 Agent 调用背后发生了什么

假设你在终端里跟 Claude Code 说：

```
帮我把 src/utils.ts 里的 formatDate 函数重构一下，用 dayjs 替换 moment
```

然后你按下回车。从这一刻开始，到 Claude Code 跟你说"搞定了"，中间发生了什么？

###### 第一件事：它得决定先做什么

<font color=lightSeaGreen>Claude Code 不会上来就改代码</font>。它会先"想"一下：我需要先读一下 `utils.ts` 看看 formatDate 长什么样，然后看看 `package.json` 里有没有 `dayjs`，没有的话要装一下，然后再改代码，改完跑一下测试……

<font color=fuchsia>这个"想一步、做一步、看一步"的循环，就是 **Agent Loop**</font>。<font color=fuchsia>它是 Agent 的心跳</font>——没有这个循环，AI 就只是一个问答机器。

###### 第二件事：它得能读文件、跑命令

光"想"没用，得有手。Claude Code 需要调用 Read 工具读文件、调用 Bash 工具跑 `pnpm add dayjs`、调用 Edit 工具改代码。

<font color=red>这些工具的定义、调度、权限管理、并发控制，组成了 **Tool System**</font>。它是 Agent 的手脚——没有工具，Agent 就是个只会说不会做的嘴炮。

###### 第三件事：它得记住前面做了什么

假设这个重构涉及 15 个文件，Claude Code 改到第 10 个文件的时候，它需要记住前面 9 个文件改了什么、用了什么模式、有没有遇到什么坑。但上下文窗口是有限的——前面的信息太多了，塞不下了怎么办？

这就是 **Context Engineering**，上下文工程。它是 Agent 的大脑供血系统——决定 Agent 能"看到"多少、"记住"多少。<font color=red>这块我认为是整个 Agent 开发里最被低估、也最有含金量的部分</font>。

###### 第四件事：它得记住你是谁

你上次跟 Claude Code 说过"这个项目用 pnpm 不要用 npm"，它会记住。下次你开一个新的会话，它还能记得。

这是 **Memory 系统**——跨会话的长期记忆。和 Context Engineering 不同，Context 管的是单次会话内的信息，Memory 管的是跨会话的持久化。

###### 第五件事：复杂任务，一个 Agent 搞不定

如果你让 Claude Code 探索一个几万行的代码库，它可能会 fork 出一个子 Agent，让子 Agent 在干净的上下文里去探索，探索完把结果压缩成一两千 token 的摘要传回来。这样主 Agent 的上下文不会被撑爆。

这就是 **Multi-Agent**——不是为了"分角色"搞什么 PM + 设计师 + 开发者的 cosplay，而是为了分隔上下文。

###### 第六件事：权限、Hook、重试、优雅退出……

Agent 要跑 `rm -rf` 的时候，谁来拦？API 挂了怎么重试？用户按了 Ctrl+C 怎么优雅退出？模型陷入死循环怎么检测？

这些"包裹在模型外面"的工程系统，就是 **Harness Engineering**。它是 Agent 的骨架——模型再强，没有一个好的骨架，也跑不起来。

##### 六大支柱

你看，就这一次看似简单的工具调用，背后牵扯了六个大的系统：

<img src="https://files.seeusercontent.com/2026/05/26/y4wA/agent-course-six-pillars.png" alt="六大支柱全景图" style="zoom:50%;" />

|支柱|一句话|人体类比|
|---|---|---|
|Agent Loop|想一步、做一步、看一步的循环|心跳|
|Tool System|读文件、跑命令、调 API|手脚|
|Context Engineering|管理有限的上下文窗口|大脑供血|
|Memory|跨会话的长期记忆|长期记忆|
|Multi-Agent|拆任务、分上下文|团队协作|
|Harness Engineering|权限、重试、Hook、生命周期|骨架|

**记住这六个词。** 后面不管出什么新的 Agent 产品、新的框架、新的论文，你都可以用这六个维度去拆解它。它在 Agent Loop 上做了什么创新？Context Engineering 怎么处理的？工具系统是什么设计？

这就是为什么我说"学 Agent 不要追产品、热点，要学底层问题"。产品会过时，但问题不会。

##### 六大支柱，每个都有多深？

###### Agent Loop：不只是个 while 循环

最简单的 Agent Loop 就是一个 `while (true) { think → act → observe }` 循环。10 行代码就能写出来。

但实际跑起来你会发现一堆问题：

- <font color=dodgerBlue>模型说到一半被截断了怎么办？</font>Claude Code 的做法是 3 次递进恢复——第一次把 max_tokens 设成 8192 希望它能说完，不行就升到 64K，再不行就注入一条"你的输出被截断了，请继续"的消息。超过 3 次就放弃。

- <font color=dodgerBlue>Agent 陷入死循环怎么办？</font>OpenClaw 设计了 4 种检测器：同一个工具反复调用（Generic Repeat）、无进展的轮询（Known Poll）、两个工具交替调用（Ping-Pong）、全局熔断（30 次重复强制停止）。不是一种检测就够了，实际场景中循环的模式五花八门。

- <font color=dodgerBlue>API 挂了怎么办？</font>Claude Code 有三层重试：内层 API 级重试（最多 10 次指数退避）、中层流式失败降级为非流式、外层快速模式失败降级为标准模式。OpenClaw 更夸张——重试预算最多 160 次，还有各种模型 Provider 相互兜底，具体我们先不展开了，后面的课程会展开。

就一个循环——Agent Loop，展开来讲可以讲很多篇。

###### Tool System：工具越多越不准

你可能觉得工具系统就是定义几个函数让模型调嘛，有什么难的。

实际上，工具数量一上去，问题就来了。10 个工具以内，模型选择准确率 > 95%；30 个工具，降到 ~85%；50 个以上，不做特殊处理就是灾难。

怎么解决？<font color=red>Claude Code 用了一个叫 **Deferred Tool Loading** 的设计——初始 prompt 里只放工具名和一句话描述，不放完整的 JSON Schema</font>。模型需要用某个工具的时候，先通过 ToolSearch 按需加载完整定义。效果：初始 prompt 瘦身 30-40%，缓存命中率大幅提升。

Manus 走了另一条路——干脆只保留不到 20 个原子工具，复杂操作全部通过写脚本放 sandbox 里跑。工具定义只占 3K token，而不是 30K。

还有一个容易忽略的问题：**工具定义一旦改了，后面所有的 KV Cache 全部失效。** 这意味着你不能动态增删工具——看起来省了 token，实际上缓存失效的成本可能是 10 倍。Manus 的做法是 "Mask Don't Remove"：不删工具定义，只标记哪些不可用，并通过比较巧妙的方式来限制模型的 Action Space，后续我们也会好好拆解。

###### Context Engineering：最被低估的护城河

这一块我认为是整个 Agent 开发里技术含量最高的部分。

一个数据帮你感受一下：一个典型的 Agent 任务，50 次工具调用，每次平均 2000 token 的结果，光工具输出就是 10 万 token。再加上 system prompt、工具定义、对话历史——200K 的窗口说满就满。

而且上下文不是越长越好。<font color=red>学术界有一个叫 "Lost in the Middle" 的现象：**模型对上下文头部和尾部的信息注意力最强，中间的内容会被逐渐忽略**</font>。你往上下文里塞的东西越多，模型对中间信息的"记忆力"就越差。

所以上下文管理的核心不是"怎么塞更多"，而是**怎么让模型在有限的注意力预算内看到最关键的信息**。

我们在课程里会用五个维度来拆解 Context Engineering：

|维度|做什么|
|---|---|
|**Offload（卸载）**|把信息搬到上下文之外——文件系统、sandbox、脚本|
|**Reduce（压缩）**|就地缩小（Compaction）或用摘要替换（Summarization）|
|**Retrieve（检索）**|从外部按需取回——RAG、文件读取、ToolSearch|
|**Isolate（隔离）**|拆成多个独立上下文——Multi-Agent|
|**Cache（缓存）**|复用已有计算结果——KV Cache、Prompt Cache|

有意思的是，行业对这五个维度并没有共识。Manus 重度使用 Offload 和 Cache，但不太鼓励压缩——因为信息丢失风险太大。**没有银弹，每个维度都有 trade-off。**

还有一个点：**压缩不等于删除。** 好的上下文管理是可恢复的——信息不是真丢了，只是暂时移出了模型的视野。

###### Memory：简单方案反而更好

跨会话记忆听起来应该很复杂——向量数据库、语义检索、embedding 模型……

但 Claude Code 用的方案简单到让人意外：就是一个 `MEMORY.md` 文件。

模型自己用 Write 工具把值得记住的信息写进去，下次会话开始的时候读回来。没有向量数据库，没有 embedding，就是纯文本文件。

<font color=dodgerBlue>反过来，OpenClaw 走的是比较重的路线</font>——SQLite + 向量混合检索，BM25 关键词权重 30% + 向量权重 70%，还有 MMR 去重和时间衰减。

哪个更好？取决于你的场景。Claude Code 面对的是单用户、记忆量不大的场景，文件方案简单可调试，人类可直接编辑。OpenClaw 面对的是多用户、大量记忆的场景，需要语义搜索能力。

**没有最好的方案，只有最合适的方案。** 这个判断力，比学会某个具体技术更重要。

###### Multi-Agent：不是分角色，是分上下文

市面上很多 Multi-Agent 的教程都在教你搞"角色扮演"——一个 Agent 当 PM，一个当开发，一个当测试。

但真的，业界用的 Multi-Agent，核心价值根本不在这里。

Manus 的联合创始人 Peak 说了一句很精辟的话："人类按角色组织是因为认知限制。LLM 不一定有这些限制。"

**子 Agent 的主要目标不是按角色分工，而是隔离上下文。** 让每个子 Agent 在干净的窗口里工作，不被其他任务的信息污染。

Claude Code 的子 Agent 设计就是这个思路——Explore Agent 去探索代码库，在自己的上下文里可以读几万 token 的文件，最后只给父 Agent 返回一两千 token 的摘要。父 Agent 的上下文保持干净。

还有一个更硬核的场景：Claude Code 的 Worktree 隔离。子 Agent 在一个独立的 git worktree 里工作，文件系统都是隔离的。做完了再 merge 回来。这才是生产级的 Multi-Agent。

###### Harness Engineering：模型外面那层壳

最后这个，可能是最不性感但最重要的。

Harness 翻译过程可以叫“壳”，也可以叫环境，它的设计可能比模型选择的影响更大。

举个具体的例子。OpenClaw 今年爆出了 CVSS 8.8 的高危 RCE 漏洞，3 万多个实例受影响，800 多个恶意 Skill 混进了 ClawHub 市场。你猜猜是啥 原因？默认没开认证。

这不是模型的问题，是 Harness 的问题。权限系统设计得不够严格，一个漏洞就能让整个 Agent 变成攻击工具。

Claude Code 在这方面做得很细：6 种权限模式、Bash 命令专用风险分类器、危险操作模式检测（`rm -rf /`、fork bomb、sudo）、连续被拒会自动降级为手动确认。还有 14 种 Hook 类型让用户能在不改源码的情况下定制行为。

这些东西虽然看起来不够酷炫，但没有它们，Agent 就是一个随时可能失控的定时炸弹。

##### 学的顺序很重要

这六个支柱不是平行的，它们之间有依赖关系。我建议的学习路径是从内到外：

![image-20260526111534839](https://files.seeusercontent.com/2026/05/26/Ovj6/image-20260526111534839.png)

1. **Agent Loop（心跳）**：先理解 Agent 的最小可运行单元
2. **Tool System（手脚）**：Agent 能"做事"了，你才能观察后面的问题
3. **Context Engineering（供血）**：Agent 跑起来之后，上下文爆了怎么办？这是深水区的入口。
4. **Memory（记忆）**：单次会话、跨会话，上下文记忆怎么来管理？
5. **Multi-Agent（协作）**：一个 Agent 不够用，怎么拆成多个？
6. **Harness Engineering（骨架）**：上面都搞定了，怎么包成一个生产级系统？

每一层都建立在前一层的基础上。你不理解 Agent Loop，就没法理解为什么 Context Engineering 这么重要；你不理解 Context Engineering，就不理解为什么 Multi-Agent 的核心价值是"分上下文"而不是"分角色"。



#### 从 ChatBot 到 Agent：一个 while 循环，凭什么让 AI 从"能聊天"变成"能干活"？

![image-20260526121832895](https://files.seeusercontent.com/2026/05/26/pD4f/image-20260526121832895.png)

本质区别在一个词：**谁掌控循环。**

- ChatBot：人掌控循环。人不说话，AI 不动。
- Copilot：人掌控循环，AI 有建议权。你不按 Tab，建议就被丢弃。
- Agent：AI 掌控循环，人有否决权。AI 自己决定下一步做什么，人只在关键节点审批（比如要跑 `rm -rf` 的时候）。

Anthropic 在 [官方文章](https://www.anthropic.com/engineering/building-effective-agents) 里有一个更精确的说法：**Workflow 是 LLM 在预定义的代码路径中被编排，Agent 是 LLM 自己决定流程和工具使用。** 关键词是"自己决定"——模型不是被动执行你的指令，而是自主规划和行动。

> At Anthropic, we categorize all these variations as **agentic systems**, but draw an important architectural distinction between **workflows** and **agents**:
>
> - **Workflows** are systems where LLMs and tools are orchestrated through predefined code paths.
> - **Agents**, on the other hand, are systems where LLMs dynamically direct their own processes and tool usage, maintaining control over how they accomplish tasks.

##### Agent 的最小模型：while(true)

如果让你用代码来表达 Agent 的核心逻辑，最简单的版本长这样：

```js
while (true) {
  const response = await llm.chat(messages)     // 想：让模型决定下一步

  if (response.toolCalls.length === 0) {
    break  // 模型认为任务完成了，没有工具要调
  }

  for (const toolCall of response.toolCalls) {
    const result = await executeTool(toolCall)  // 做：执行工具
    messages.push(result)                       // 看：把结果加入上下文
  }
}
```

这就是 <font color=fuchsia>ReAct 模式——Reasoning（想）+ Acting（做），加上 Observation（看结果）</font>。2022 年的论文提出的概念，到现在已经成了所有 Agent 产品的标准范式

> [!NOTE]
>
> 搜了下，论文是 [ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)

没有那么多花里胡哨的东西，就这么一个小的认知模型，已经跑在了如今无数的 Agent 产品了。

##### 从 10 行到上千行：真实的 Agent Loop 有多复杂

###### 一轮循环里到底发生了什么

简单版是"想-做-看"三步，Claude Code 的实际流程是这样的：

<img src="https://files.seeusercontent.com/2026/05/26/r4sM/agent-course-loop-iteration.png" alt="Claude Code 单轮循环流程图" style="zoom:40%;" />

- **第一步：准备上下文**

  在调 API 之前，<font color=red>先检查上下文是不是快爆了</font>。如果快到上限了，触发压缩——<font color=lightSeaGreen>先试轻量级的 Snip（删掉老消息），不行就 Microcompact（局部摘要），再不行就 Auto-compact（全局摘要）</font>。

- **第二步：调模型 API**

  把消息发给模型 API，流式接收响应。这里有一个很聪明的设计——**模型还在说话的时候，已经识别出来的工具调用就开始执行了。**

  为什么？想象一下，模型说"我要同时读 3 个文件"，如果你等它把 3 个工具调用全说完、再一个个去执行，用户就得干等着。但如果模型刚说出第一个"读文件"的指令，你就立刻去读了，等模型说完第三个的时候，第一个可能已经读完了。

  这就是"边说边执行"——流式的不只是文字，工具执行也是流式的。

  当然这里有一个前提：<font color=red>**只有不冲突的操作才能并发**</font>。 读文件可以同时读 3 个（只读不冲突），但改文件必须一个个来（两个修改同时写一个文件就乱了）。这个判断——哪些操作可以并发、哪些必须串行——是 Agent 工具系统里一个很重要的设计点。

- **第三步：决定是否继续**

  模型响应回来了。现在要判断：循环要不要继续？

  这个判断比你想的复杂得多。不只是"有没有工具调用"这一个条件，Claude Code 有 **7 种退出路径**：
  
  |退出原因|什么时候触发|
  |---|---|
  |`completed`|模型没有调用工具，认为任务完成了|
  |`aborted_streaming`|流式传输过程中被中断|
  |`aborted_tools`|工具执行过程中被中断|
  |`hook_stopped`|Hook 阻止了继续执行|
  |`max_turns`|超过了最大轮次限制|
  |`blocking_limit`|上下文太长，API 拒绝了|
  |`prompt_too_long`|压缩后还是太长，无法恢复|

  这些退出条件不需要你背，你只需要知道是什么时机就可以，留个印象即可。

  每一种退出都需要不同的处理。比如 `aborted_streaming` 发生的时候，模型可能已经说了一半——已经到达的文字要保留（因为用户已经看到了），但还没执行完的工具调用要丢弃。

- **第四步：执行工具，收集结果**

  如果决定继续，就执行所有工具调用，收集结果，把结果塞回消息列表，准备下一轮。

  这一步也不简单——工具可能执行失败，失败了怎么给模型反馈？错误信息写得好不好直接影响模型能不能自我纠正。

- **第五步：处理附加任务**

  工具执行完了，但在开始下一轮之前，还有一堆"杂活"：消费排队的命令附件、检查 Memory 预取结果、检查 Skill 发现结果、记录已消费的命令……

  然后构建下一轮的 State，回到第一步。

###### 状态追踪：Agent 需要"记住"的不止是对话

简单版的 Agent 只维护一个消息列表。但真实的 Agent 需要追踪的东西多得多：

- **现在是第几轮了？** 用来判断是不是该停了（保险丝）
- **上一轮为什么选择了继续？** 是正常流转、还是从错误中恢复、还是压缩重试？这个信息对调试极其重要——当 Agent 行为异常的时候，你可以一轮一轮追溯，看看它到底是哪一步的判断出了问题
- **压缩执行到哪了？** 是否触发过紧急压缩、压缩后 token 降了多少
- **输出被截断了几次？** 模型说到一半被截断，第一次恢复、第二次恢复、第三次还截断就放弃
- **有没有挂着的异步任务？** 比如后台在生成工具摘要

这些状态组合在一起，才能让 Agent Loop 在各种异常场景下做出正确的决策。这也是为什么真实的 Agent Loop 会比你想象的复杂——不是核心逻辑复杂，而是**异常处理**复杂。

###### 实时反馈：Agent 不能是"黑箱"

还有一个很重要的设计理念：<font color=red>**Agent 在执行过程中，必须实时把中间结果"吐"出来给用户看**</font>。

Agent 跑一个复杂任务可能要几分钟。如果用户看到的是一个空白屏幕转圈圈，然后几分钟后突然蹦出结果——这个体验是不可接受的。

所以，<font color=lightSeaGreen>好的 Agent Loop 不是"跑完再说"，而是"边跑边说"</font>。<font color=red>模型在思考什么、调了什么工具、工具返回了什么——每一步都实时展示给用户</font>。你在 Claude Code 里看到的那种"模型一边说话、工具一边执行"的效果，就是这个设计的体现。

这个机制在技术上是通过 **Generator（生成器）** 实现的——函数可以在执行过程中不断"吐出"中间结果，而不是等全部跑完才返回一个最终结果。

##### 手术直播：一个真实任务的完整 trace

下面这张图是一个真实场景的完整 trace——让 Agent 给 fetchUser 函数加上重试机制，从头到尾 7 轮，每一轮的 Think / Act / Observe 都在里面。

![Agent 执行 Trace 流程图](https://files.seeusercontent.com/2026/05/26/cs6S/agent-course-agent-trace.png)

##### 最小可运行版本

理解了这些之后，我们来写一个 "麻雀虽小五脏俱全" 的 Agent。不是 Claude Code 那么复杂，但核心的设计决策都在：

```ts
import { generateText } from 'ai'
import { anthropic } from '@ai-sdk/anthropic'

const tools = {
  read_file: { /* ... */ },
  write_file: { /* ... */ },
  run_command: { /* ... */ },
}

async function agent(task: string) {
  const messages = [{ role: 'user', content: task }]
  let turnCount = 0
  const maxTurns = 30  // 保险丝：防止无限循环

  while (true) {
    // 保险丝检查
    if (++turnCount > maxTurns) {
      console.log('超过最大轮次，停止')
      break
    }

    // 调模型
    const result = await generateText({
      model: anthropic('claude-sonnet-4-6'),
      messages,
      tools,
      maxSteps: 1,  // 每次只走一步，循环由我们控制
    })

    // 没有工具调用 = 任务完成
    if (result.toolCalls.length === 0) {
      console.log('Agent:', result.text)
      break
    }

    // 执行工具，收集结果
    for (const call of result.toolCalls) {
      console.log(`调用工具: ${call.toolName}(${JSON.stringify(call.args)})`)
      const toolResult = await executeTool(call)
      messages.push(
        { role: 'assistant', content: result.text, toolCalls: [call] },
        { role: 'tool', content: toolResult, toolCallId: call.toolCallId }
      )
    }
  }
}
```

30 行左右。能跑，但离生产级差了十万八千里。它缺了什么？

- 没有流式响应（等模型说完才展示）
- 没有并发工具执行（一个个跑）
- 没有上下文压缩（跑多了就爆）
- 没有重试（API 挂了就挂了）
- 没有权限检查（模型说跑什么就跑什么）
- 没有死循环检测（模型发疯了没人拦）



#### 做 Agent 开发，有些大模型本身的底层机制，你不得不了解

##### 你的中文 prompt，到底被拆成了什么？

<font color=lightSeaGreen>大部分大模型用的分词算法叫 **BPE（Byte Pair Encoding）**</font>。简单理解就是：模型在训练之前，先统计大量文本中哪些字符组合出现频率最高，把高频组合合并成一个 token。英文只有 26 个字母，高频组合容易合并，一个词基本一个 token；中文有几万个汉字，字符集大得多，BPE 合并效率天然低一些。

这直接导致了一个实际问题：**同样的语义内容，中文比英文消耗更多的 token。**

在早期的 GPT-2 / GPT-3 时代，1 个中文汉字大约要 2-3 个 token，效率很低。现在的主流模型已经大幅优化了中文分词，但不同模型差异不小：Claude 和 GPT 系列大约 **1 个汉字 ≈ 1~1.5 个 token**，国产模型（Qwen、DeepSeek）做得更好，很多常见词可以整词编码。<font color=red>整体来看，**同样语义的一句话，中文 token 数比英文多 30%-60%**</font>。

为什么？根本原因在 UTF-8 编码：一个英文字母只占 **1 字节**，而一个中文汉字占 **3 字节**。BPE 是在字节层面做合并的，英文 1 字节起步，大量常见词（`the`、`function`、`learning`）都能被合并成单个 token，压缩率极高；中文 3 字节起步，BPE 光把 3 个字节拼回一个汉字就已经消耗合并预算了，留给词级别合并（比如把"机器"+"学习"合成一个 token）的空间天然更少。**这是编码层面的结构性差距，不会因为模型升级而消失。**

这也是为什么 <font color=lightSeaGreen>OpenClaw 在做 token 估算的时候，会加一个 **20% 的安全边距**（`SAFETY_MARGIN = 1.2`）</font>。不是拍脑袋来的，就是因为中文这类多字节字符的 token 估算容易低估。

##### 模型是怎么"说话"的：一个字一个字蹦出来

这里有一个非常重要的事实，很多人不知道：**大模型不是"想好了再说"的，它是"边说边想"的。**

什么意思呢？模型生成文本的过程，是一个 token 接一个 token 往后蹦的。它先生成第 1 个 token，然后把第 1 个 token 加入输入，再生成第 2 个 token，然后把前 2 个 token 加入输入，再生成第 3 个……如此循环，直到生成一个"结束"标记。

这个过程叫 <font color=red>**自回归生成（Autoregressive Generation）**</font>。

![自回归生成过程示意图](https://files.seeusercontent.com/2026/05/26/wuC2/autoregressive.png)

这恰恰解释了你在用 ChatGPT 或者 Claude 的时候看到的那个"打字机效果"——文字一个一个蹦出来。这不是为了好看做的动画效果，这就是模型真实的生成过程。

**对 Agent 开发来说，这个机制有三个直接的影响：**

- **第一，流式响应是天然的。**

  因为模型本来就是一个 token 一个 token 生成的，所以你不需要等它全部说完再展示给用户。生成一个就发一个，这就是 SSE（Server-Sent Events）流式响应。

- **第二，工具调用的 JSON 要"攒完"才能解析。**

  模型在调用工具的时候，会输出一个 JSON 格式的参数。但<font color=fuchsia>因为是一个 token 一个 token 蹦出来的，你收到的不是一个完整的 JSON，而是一堆 JSON 碎片</font>：

  ```txt
  {"na        ← 第一批 token
  me": "rea   ← 第二批
  d_file",    ← 第三批
  "path":     ← 第四批
  "/src/in    ← 第五批
  dex.ts"}    ← 最后一批，JSON 才完整
  ```

  <font color=lightSeaGreen>**你得把这些碎片攒起来，等最后一个 `}` 到了，才能解析这个 JSON，才能真正去执行工具**</font>。<font color=red>Claude Code 的 `StreamingToolExecutor` 就是干这个事的</font>。

- **第三，response prefill 能"操控"模型的行为。**

  <font color=red>**这是做 Agent 最有用的技巧之一**</font>。既然模型是基于前面所有 token 来决定下一个 token 的，那如果我提前替模型把前几个 token 写好呢？

  比如我想让模型只能调用浏览器相关的工具，我可以在 assistant 消息里预先填入：

  ```json
  {"name": "browser_
  ```

  <font color=red>模型看到这个前缀，就只能在 `browser_` 开头的工具名里选了</font>。它没法回头改已经"说过"的字，只能顺着往下说。

  > [!NOTE]
  >
  > 某种程度上消除幻觉和消除其他错误的可能，也是一种“剪枝”
  
  Manus 就是<font color=red>用这个方法来控制 Agent 在不同阶段只能用特定工具的</font>——不需要删除任何工具定义，只需要引导模型的"开头"。

##### Attention 和 KV Cache：为什么上下文不是越长越好

接下来聊聊 Transformer 架构里最核心的东西：Attention（注意力机制）。

这就要聊到 Attention 里面最核心的三个角色了：**Query、Key、Value**。

###### Q、K、V：搜索引擎的比喻

Q、K、V 的逻辑跟你用搜索引擎一模一样。

你在 Google 搜索的时候，发生了什么？

1. 你输入一个**搜索词**——"KV Cache 是什么"
2. Google 拿你的搜索词去**匹配**所有网页的标签和关键词
3. 匹配度高的网页，把它的**内容**返回给你

Attention 做的事情完全一样：

- **Query（查询）**：当前正在生成的 token 会问一个问题——"我需要什么信息来决定下一个词？"
- **Key（索引）**：前面每一个 token 都有一个标签——"我包含什么类型的信息？"
- **Value（内容）**：前面每一个 token 的实际内容——"这是我的具体信息。"

生成新 token 的时候，模型拿当前 token 的 Query，去和前面所有 token 的 Key 做匹配。匹配度高的，就把对应的 Value 拿过来，加权混合，作为生成下一个 token 的依据。

![Q/K/V 注意力机制示意图](https://files.seeusercontent.com/2026/05/26/O1cb/qkv-attention.png)

举个具体的例子。假设上下文是"小明昨天去了北京，今天他去了"，现在要生成下一个 token：

- 当前 token 的 Query 大概在问："谁去了哪里？"
- "小明"这个 token 的 Key 标签是"人名"，匹配度高 → 它的 Value 被拉过来
- "北京"的 Key 是"地名"，匹配度也挺高 → Value 也被拉过来
- "昨天"的 Key 是"时间词"，跟当前问题关系不大 → Value 的权重就低

最终模型综合了高权重的 Value 信息，可能生成"上海"（另一个城市）。

**这就是 Attention 的全部了。** 没有什么神秘的，就是一个"查询-匹配-取值"的过程，只不过<font color=red>这个过程是可以被训练的——模型通过大量数据学会了怎么生成好的 Query、Key 和 Value</font>。

现在你理解了为什么叫"注意力"了吧？Query 和 Key 的匹配度，就是"注意力权重"。<font color=fuchsia>模型对某个 token 的注意力越高，那个 token 的 Value 在最终结果里的占比就越大</font>。

<font color=red>问题在于：每个新 token 都要拿自己的 Query 去和前面**所有** token 的 Key 做匹配</font>。n 个 token 就要做 n 次匹配，而上下文里每个 token 都可能成为"当前 token"，所以总共是 n × n = **n²** 的计算量。10 个 token 要做 100 次，100 个 token 要做 10000 次，10000 个 token 要做 1 亿次。

> [!NOTE]
>
> 这里复杂度是 $O(n^{2})$ 无疑。不过，按照等差数列的计算方式，应该 $\frac{1}{2} \cdot n^{2}$ 吧？
>
> 问了下 Gemini ，得到如下回复 https://gemini.google.com/share/963ebb92cd56 ；虽然肯定了我的说法，但是还是

更关键的是，随着上下文越来越长，每个 token 能分到的"注意力预算"就越少。上下文里塞的东西越多，模型对每一条信息的"记忆力"就越差。这不是我说的，Chroma 团队专门做过研究，叫 **Context Rot**——上下文腐烂。

> [!NOTE]
>
> 让人想到 “信息熵”。问了下 Gemini in Chrome，得到如下回答 https://gemini.google.com/share/d86578d78c25

###### KV Cache：已经算过的 Key 和 Value，别再算了

好，理解了 Q、K、V 之后，KV Cache 就很好理解了。

刚才说模型每生成一个新 token，都要拿 Query 去匹配前面所有 token 的 Key 和 Value。但你想想，如果模型已经生成了 100 个 token，现在要生成第 101 个，它需要用到前 100 个 token 的 Key 和 Value。等到要生成第 102 个的时候，前 101 个 token 的 Key 和 Value 又要用一遍——其中 100 个跟刚才是一模一样的。

<font color=dodgerBlue>重新计算一遍？太浪费了。</font>

所以，<font color=fuchsia>模型会把之前算过的每个 token 的 Key 和 Value 缓存起来，这个缓存就叫 **KV Cache**（Key-Value Cache）</font>。现在你知道这个名字的由来了——缓存的就是 Attention 里面的 K 和 V。

![KV Cache 工作原理示意图](https://files.seeusercontent.com/2026/05/26/a7Ag/kv-cache.png)

<font color=fuchsia>**但 KV Cache 有一个关键限制：它是基于"前缀匹配"的。**</font>

什么意思？缓存只有在前缀完全一样的时候才能复用。

举个例子。你的 Agent 第一轮对话，发给模型的内容是：

```txt
[System Prompt] + [用户消息1] + [模型回复1]
```

第二轮对话，发的是：

```txt
[System Prompt] + [用户消息1] + [模型回复1] + [用户消息2]
```

因为前面的部分完全一样，只是末尾加了新内容，所以前面所有 token 的 KV Cache 都能复用。模型只需要计算新增的 `[用户消息2]` 的部分。

但如果你做了一件事——比如在 System Prompt 的开头加了个时间戳：

```txt
当前时间：2026-04-02 08:00:00    ← 这个每次都变！
[其余的 System Prompt]
[用户消息1]
[模型回复1]
[用户消息2]
```

完蛋。第一个 token 就不一样了，后面所有的 KV Cache 全部作废，需要从头重新计算。

**这不是理论上的问题。Anthropic 的 API，缓存命中和缓存未命中的价格差 10 倍。** 一次工具定义变动，一次调用就贵 10 倍。

Manus 的首席科学家 Peak 说：KV Cache 命中率是"生产阶段最重要的单一指标"。

##### 模型是怎么"选词"的：从打分到概率

> [!NOTE]
>
> TL;DR ：下面三步就是：打分（Logits）、归一化（Softmax）、抽样（Sampling）

最后一个概念，也是理解"约束解码"的关键。

模型每生成一个 token，内部到底发生了什么？

简单来说，分三步：

###### 第一步：给所有候选词打分（Logits）

模型的词汇表里有几万个 token（ Claude 大概有 10 万多个）。每生成一个新 token，模型会给词汇表里的**每一个** token 打一个原始分数，表示"根据前面的上下文，这个 token 接下来出现的可能性有多大"。

这些原始分数叫 **logits**。

比如在"今天天气真"这个上下文后面，"好"这个 token 可能得分 5.2，"不"得分 3.1，"的"得分 1.8，而"跑步"可能只有 -2.3。

###### 第二步：把分数变成概率（Softmax）

原始分数不好直接用——有正有负，也不知道总和是多少。所以需要用一个叫 **softmax** 的函数，把这些分数转换成概率分布：所有候选词的概率加起来等于 1。

> [!NOTE]
>
> 用作归一化的 softmax 公式为：
>
> 对于输入向量 $z$，其包含 $K$ 个实数元素 $(z_1, z_2, \dots, z_K)$。第 $i$ 个元素的 Softmax 归一化计算公式为
> $$
> S(z_i)=\frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}
> $$

softmax 做的事情很简单：**分数越高的词，概率越大；分数越低的词，概率越小。而且这个差距会被放大——高分词的概率会比它的原始分数暗示的还要高。**

经过 softmax 之后，"好"可能变成 65% 的概率，"不"15%，"的"8%，"跑步"0.01%。

![Logits → Softmax → 采样 示意图](https://files.seeusercontent.com/2026/05/27/dH2e/logits-softmax-v2.png)

###### 第三步：根据概率采样（Sampling）

拿到概率分布后，模型从里面"抽签"选一个 token。概率高的只是更容易被选中，但不是确定的——这就是为什么你问模型同一个问题，有时候得到不同的回答。反过来思考，如果每次都拿概率最高了，那模型的输出基本就是那些 token 序列，就跟复读机一样，没啥智能可言了。

这里提一个你肯定用过的参数：**temperature**。

说个冷知识，<font color=red>temperature 调的是 softmax 输出的概率分布的"尖锐程度"</font>：

- **temperature 接近 0**：概率分布变得非常尖锐，高的特别高，低的也很低，最高分的那个 token 几乎 100% 被选中。模型输出变得确定、保守。
- **temperature = 1**：<font color=red>正常的概率分布，有一定的随机性</font>。
- **temperature > 1**：概率分布变得更平坦（或者更平均），低分 token 也有机会被选中。<font color=lightSeaGreen>模型输出变得更有创意，但也更容易胡说八道</font>。

做 Agent 的时候，一般用比较低的 temperature（0 或者接近 0），因为你希望模型做可靠的决策，而不是来一段"创意写作"。

###### 为什么这些对 Agent 开发很重要？

因为理解了这个"打分 → 概率 → 采样"的过程，你就能理解两个在 Agent 里非常关键的技术：

- **Logit Mask（约束解码）**

  既然模型是从概率分布里"抽签"的，那如果我在抽签之前，直接把某些 token 的概率设为 0 呢？

  比如我只希望模型输出 "yes" 或 "no"，那我<font color=lightSeaGreen>就把除了 "yes" 和 "no" 之外的所有 token 的 logit 设为负无穷（经过 softmax 后概率就是 0 了）</font>。模型只能在 "yes" 和 "no" 之间选。

  这就叫 **logit mask**，也叫约束解码。

  Manus 在多 Agent 架构里用这个技术来确保子 Agent 的输出格式严格可控——不管模型"想"说什么，logit mask 保证它只能按规定的格式输出。

- **Structured Output（结构化输出）**

  同理，当你让模型输出一个 JSON 格式的工具调用时，模型怎么保证输出的 JSON 是合法的？

  一种方式就是在每一步生成的时候，用 logit mask 只允许当前上下文下语法合法的 token。比如刚输出了 `{"name":` 之后，下一个 token 只能是引号 `"` 开头的字符串。

  这就是为什么现在的模型能比较可靠地输出结构化的工具调用参数——不是它"理解"了 JSON 语法，而是在解码阶段被约束了。

##### 串一下

我们来把这四个概念串起来，看看一次 Agent 的工具调用到底经历了什么：

1. **Tokenize**：你的 prompt（system prompt + 对话历史 + 用户指令）被拆成一串 token 数字

2. **Attention 计算**：模型回看所有 token，给每个 token 分配注意力权重，综合所有信息

3. **KV Cache**：前面轮次已经算过的 token 直接复用缓存，只算新增的部分

4. **生成 token**：模型输出第一个 token 的 logits → softmax → 采样，得到第一个 token

5. <font color=fuchsia>**自回归循环**</font>：把新 token 加入上下文，回到第 2 步，重复直到输出完整的工具调用 JSON

   > [!NOTE]
   >
   > 应该是第一次在笔记上记录这个词语，之前好像有听过；这个词听起来有点唬人，但是想了下也就是再迭代一轮输出下一个 token

6. **约束解码**（如果用了的话）：在第 4 步的采样阶段，通过 logit mask 限制模型只能输出合法的 token

整个过程就像一个打字员，一边打字一边翻阅桌上的资料，每打一个字都要参考前面所有已经打过的字和桌上的资料。而我们做 Agent 开发的工作，很大程度上就是在管理那张桌子——**桌上放什么资料、放多少、怎么放，直接决定了这个打字员的工作质量。**

##### 一张表总结

| 概念                 | 一句话解释                          | 对 Agent 开发的影响                                          |
| -------------------- | ----------------------------------- | ------------------------------------------------------------ |
| **Token 化**         | 文本被拆成模型能理解的小块          | 中文 token 估算要加安全边距                                  |
| **自回归生成**       | 一个 token 一个 token 往后蹦        | 流式响应天然支持；工具 JSON 要攒完才能解析；prefill 能控制行为 |
| **Attention**        | 每个 token 都要回看所有前面的 token | 上下文越长，注意力越分散，模型越容易忘事                     |
| **KV Cache**         | 缓存已算过的注意力结果              | 前缀不能变，不然缓存全废，成本翻10倍                         |
| **Logits + Softmax** | 给所有候选词打分，转成概率          | temperature 控制确定性                                       |
| **Logit Mask**       | 把不想要的选项概率设为0             | 约束解码，控制 Agent 输出格式                                |



#### 2026 年了，你的 Agent 架构还停留在 LangChain 时代吗？

##### LangChain 到底是什么？

LangChain 的核心设计理念是：把 LLM 应用开发中的常见步骤，封装成可组合的"链条"（Chain）。

##### LangChain 出了什么问题？

###### 第一个问题："链条"模型不适合 Agent

LangChain 的核心概念是"Chain"——链条。A → B → C → D，线性的、预定义的流程。你在写代码的时候就已经决定了每一步做什么。

<font color=lightSeaGreen>对于 RAG（检索增强生成）这类场景，Chain 模型是合适的</font>。因为<font color=red>流程确实是固定的：加载文档 → 切分 → 向量化 → 检索 → 生成</font>。每次执行的步骤都一样，只是输入不同。

但 Agent 不是这样工作的。

Agent 的核心是 `while(true) { think → act → observe }`。它是一个**循环**，不是一条链。它不知道自己要执行几步，不知道下一步该做什么——这些都是模型实时决定的。

**Chain 是确定性的：写代码时就定好了路线。** **Agent 是非确定性的：运行时才知道下一步做什么。**

用一个为链条设计的框架去构建循环驱动的系统，就像用 Coze 工作流去搭一个游戏的逻辑——能跑，但想想就非常难受。

> [!TIP]
>
> *LangChain 团队也意识到了这个问题，后来搞了 LangGraph——用图和状态机替代链条来做 Agent 编排。*

###### 第二个问题：抽象层太多了

LangChain 想帮你做很多事情，所以它建了很多抽象层。

这在简单场景下感觉很爽——几行代码就搞定了。但一旦你的需求超出了框架提供的标准路径，问题就来了。

> [!NOTE]
>
> 这里感觉并非重点内容，略。总结下来就是不方便自定义，也不方便调试

###### 第三个问题：原型到生产的鸿沟

用 LangChain 搞一个 demo 确实快——半天就能跑起来一个还不错的 Agent 原型。但当你想把它上生产的时候，你会发现框架的设计目标（降低入门门槛）和生产的需求（精细控制）本身就是矛盾的。

> [!NOTE]
>
> 这里感觉并非重点内容，略。

##### LangGraph：方向对了，但依然比较重

###### LangGraph 解决了什么问题

LangGraph 的核心思路是：**用"图"替代"链"。**

Chain 是线性的——A → B → C → D，走完就结束。但 Agent 需要循环、需要分支、需要根据结果决定下一步。图（Graph）天然支持这些。

LangGraph 把 Agent 的执行流程建模成一个**状态机**：

- **节点（Node）** ：一个动作。比如"调模型"是一个节点，"执行工具"是一个节点，"检查结果"也是一个节点。
- **边（Edge）** ：节点之间的路由。可以是无条件的（A 执行完永远走 B），也可以是有条件的（根据 A 的结果决定走 B 还是 C）。
- **状态（State）** ：在图上流转的数据。每个节点可以读取状态、修改状态，传给下一个节点。

核心就三步：**定义节点（做什么）→ 定义边（怎么连）→ 编译运行。**

`model → tool → model → tool → ...` 这就形成了一个循环。跟 Chain 的 A → B → C → 结束 不同，图天然支持"走回去"这个操作。

<img src="https://files.seeusercontent.com/2026/05/27/o6Ty/agent-course-langgraph-flow.png" alt="LangGraph 执行流程图" style="zoom: 40%;" />

而且 LangGraph 还提供了一些很实用的能力：

- **人工审核**：工具要执行危险操作时，暂停图的执行，等人确认了再继续
- **子图**：把一段复杂逻辑封装成子图，嵌套在主图里，跟函数调用一个道理
- **检查点**：保存执行到某一步的完整状态，挂了可以从断点恢复

###### 那它的问题是什么？

- **概念抽象过重，复杂场景下反而增加心智负担与编写阻碍**

  > [!NOTE]
  >
  > 作者并没有给出第一个问题一个总结或标题，我的感觉是 “自定义困难”，不过我还是用 Gemini in Chrome 让他总结了下：https://gemini.google.com/share/f2ae3725ed9c

  LangGraph 的方向没问题——用图替代链来建模 Agent，思路是对的。

  但你有没有注意到一件事？上面那段 LangGraph 代码做的事情，用纯代码写其实就是这样：

  对比一下等价的纯代码：

  ```js
  while (true) {
    const response = await llm.chat(messages)
    if (!response.toolCalls) break          // 没有工具调用，结束
    const result = await executeTool(response.toolCalls)
    messages.push(response, result)         // 把结果加回去，继续循环
  }
  ```

  十行代码不到，逻辑一目了然。你不需要理解什么是 Node、Edge、State、Conditional Edge、Checkpoint 。

  当然，<font color=dodgerBlue>简单场景下两者差不多。但当逻辑变复杂的时候</font>——比如你<font color=red>需要流式输出、需要并发执行多个工具、需要在工具执行中途中断——框架的概念反而会碍事</font>。因为你得先想"这个需求在图的模型里怎么表达"，而不是直接想"代码怎么写"。

  > [!NOTE]
  >
  > 根据这里的内容，感觉 LangGraph 和 LangChain 存在一样的问题

- **第二，它跟 LangChain 生态绑定。**

  LangGraph 虽然可以独立使用，但它的很多功能——LangSmith 可观测性、LangServe 部署、预置的 ReAct Agent 模板——都和 LangChain 生态深度绑定。一旦用了 LangGraph，你很容易把整个 LangChain 生态都带进来。

- **第三，它解决的不是最难的问题。**

  图编排帮你定义了"模型节点完了走工具节点，工具节点完了回模型节点"——但这个路由逻辑本身就是一个 if 判断的事。真正难的是每个节点内部：流式输出怎么做平滑、工具并发怎么控制安全性、上下文快爆了怎么压缩。这些 LangGraph 管不了，因为<font color=red>它在编排层，不在执行层</font>。

##### 那什么时候适合用 LangGraph？

说了这么多，不是说 LangGraph 没用。它有自己适合的场景，如果你要开发企业级的 Agent 应用，又不需要做深层的定制，那直接团队统一用 LangGraph 即可。

如果你需要的是——精细的上下文控制、流式工具执行、自定义压缩策略、极致的性能——这些 Agent 开发里最硬核的部分，LangGraph 帮不了你太多。因为这些东西不在"图怎么编排"这个层面，而在每个节点内部的实现细节里。

> [!TIP]
>
> *我做一个类比：LangGraph 帮你设计了城市的交通网络（哪些路口连哪些路口），但真正难的是每条路上的红绿灯怎么调度、出了车祸怎么应急、高峰期怎么分流。后者才是 Agent 工程的核心挑战。*

![Chain vs Graph vs While Loop 对比](https://files.seeusercontent.com/2026/05/27/W9qu/agent-course-langgraph-vs-loop-v.png)

##### 那真正做 Agent 产品的人在用什么？

###### Claude Code：纯手写，零框架

Claude Code 的 Agent 核心逻辑，没用任何外部 AI 框架。

它的整个 Agent Loop，即 `while(true)` 循环是用纯 TypeScript 手写的 async generator。

它唯一的 AI 相关依赖是 `@anthropic-ai/sdk`——Anthropic 自家的 API SDK。这个 SDK 做的事情很简单：帮你封装 HTTP 请求，处理流式响应的底层细节。它不管你怎么编排 Agent，不管你怎么管理上下文，不管你怎么调度工具。

工具系统也是 Claude Code 自己写的。每个工具一个独立的 TypeScript 文件，用 Zod 做参数校验。

流式执行的机制也是自己写的。模型还在输出的时候，就已经开始并发执行安全的工具了。

上下文管理同样是自己写的。三层压缩策略，每一层的触发时机和压缩方式都是针对 Agent 场景专门设计的。

**为什么要这么做？** 因为 Agent 的核心逻辑太需要精细控制了。

流式工具执行需要判断哪些工具可以并发、哪些必须串行——Read 可以并发跑，但 Edit 必须排队，因为两个 Edit 同时改一个文件会冲突。这个判断逻辑跟工具类型、输入参数、当前上下文都有关。有哪个通用框架能帮你做这个决策？在 LangChain 里面很难实现。

上下文压缩需要在恰好正确的时机触发——太早了会丢关键信息，太晚了模型就"爆了"。压缩时保留什么、丢弃什么，完全取决于具体业务。

###### OpenClaw：加一层薄薄的框架，核心自研

OpenClaw 的选择稍有不同。它没有完全从零开始，而是用了一个叫 [`pi-agent-core`](https://github.com/earendil-works/pi/tree/main/packages/agent)（也叫 `Pi`） 的开源库作为底层。

但注意，这不是 LangChain 那种大而全的框架。`pi-agent-core` 提供的是最基础的东西：消息格式定义、工具接口抽象、会话管理的骨架。相当于给你搭了个毛坯房，里面怎么装修完全由你决定。

而 OpenClaw 真正花功夫的地方——上下文引擎、工具策略系统、Memory 搜索——全是自研的。

特别值得说的是它的**工具策略系统**。OpenClaw 的工具不是简单地注册了就能用，它有一套多层策略过滤，确保每个 Agent 只能调用它被允许调用的工具。

Memory 也是一样。OpenClaw 用的是向量搜索 + BM25 文本搜索的混合方案，支持多种 embedding 提供商，用 SQLite 做向量存储，还有记忆随时间衰减的策略。这些都是高度贴合自身业务的设计，不可能从通用框架里"配置"出来。

###### 一个共同的规律

看完这两个产品，你会发现一个规律：

**越是做到生产级别的 Agent 产品，在核心逻辑上越倾向于自研。**

不是因为他们有"非我发明不用"的技术洁癖，而是因为 Agent 的核心——循环控制、上下文管理、工具编排——这些东西太贴合具体场景了，通用框架的抽象反而成了障碍。

![框架层次对比图](https://files.seeusercontent.com/2026/05/27/Vxv9/agent-course-framework-layers.png)

##### 那框架就完全没用了吗？

###### 该用框架的场景

- **某些标准化应用。** 你要做的是一个比较标准的 RAG 应用，或者一个简单的工具调用 Agent，需求不太会偏离框架的标准路径。

  > [!NOTE]
  >
  > 这里存在一个问题：如何获知“框架的标准路径” ？感觉还是要深入理解框架，以及其边界才行

- **团队快速对齐。** 团队里大部分人没做过 Agent 开发，框架提供的结构和约定能帮大家快速上手。

###### 不该用框架的场景

- **上下文需要精细控制。** Agent 跑了 50 轮，上下文快爆了，你需要决定压缩什么、保留什么。这种决策高度依赖业务，框架帮不了你。
- **性能敏感。** 框架的抽象层带来的额外开销，在延迟敏感的场景下不可接受。
- **工具编排逻辑复杂。** 并发控制、权限管理、错误恢复——这些一旦复杂起来，框架的标准接口就不够用了。
- **你需要能快速定位问题。** 生产环境出了 bug，你得能在几分钟内找到根因。Agent 逻辑埋在五层框架抽象里的话，这基本不可能，用框架就等于是灾难。

有一条很粗暴但也很实际的判断标准：**你在用框架省力地解决问题，还是在跟框架搏斗？如果是后者，那就该切了。**

##### Vercel AI SDK：它不是框架

**Vercel AI SDK 不是 Agent 框架，它是 API 适配层。**

什么叫 API 适配层？就是帮你抹平不同模型提供商之间的 API 差异。

Vercel AI SDK 帮你做的就是这件事：<font color=red>统一的接口，底下自动适配不同的提供商</font>。你换模型只需要改一行代码，Agent 的核心逻辑完全不用动。

除此之外，<font color=dodgerBlue>它 **还做了** 两件有价值的事</font>：<font color=red>处理流式响应的底层细节（SSE 解析、流转发）</font>，以及提供前端 UI 组件（React / Svelte / Vue 的流式聊天 hooks）。当然前端这个我也不推荐大家用它的 UI 组件，不如自己来处理 SSE 前后端通信，这部分同理也是非常核心的部分，自研带来的扩展性长期而言收益很大。

**注意它不做什么：** 不帮你编排 Agent 循环，不帮你管理上下文，不帮你调度工具，不帮你做 Memory。

> [!TIP]
>
> 作者在教程中引用了 Vercel 的话，但是没找到一模一样的句子，只找到高度类似的话：
>
> > Building AI agents might seem like a new thing that calls for new abstractions, but it is just regular programming. Use if statements, loops, or switches, whatever fits. Don’t overthink the structure.
> >
> > 摘自：[Vercel blog - The no-nonsense approach to AI agent development](https://vercel.com/blog/the-no-nonsense-approach-to-ai-agent-development)
>
> ---
>
> 另外，博客中还有点其他有价值的东西，这里做下补充：
>
> > To recap, agents are useful when:
> >
> > - The task is difficult to automate with traditional code
> > - Prompting the model manually shows signs of success
> > - You scope the agent narrowly and build on [solid software practices](https://ai-sdk.dev/docs/foundations/agents)
> > - You optimize both with hands-on intuition and structured evaluation

**API 适配层和 Agent 框架，是两个完全不同的东西。** 前者是有价值的标准化（帮你屏蔽差异），后者往往是过度抽象（帮你隐藏了你不该被隐藏的逻辑）。

这也是为什么 Vercel AI SDK 能跟"自研 Agent 核心逻辑"完美共存——它管 API 层，你管 Agent 层，各司其职。

##### 小结

这篇的核心观点：

**LangChain 是 AI 领域的 jQuery。** 它在 API 还不成熟、开发者还不熟悉 LLM 的年代降低了入门门槛，功不可没。但随着模型 API 越来越好用、开发者经验越来越丰富，它的核心价值在下降。

**生产级 Agent 的核心逻辑不适合用通用框架。** Claude Code 和 OpenClaw 都用行动证明了这一点。<font color=red>Agent 的循环控制、上下文管理、工具编排太贴合具体场景，通用抽象反而是障碍</font>。

**API 适配层 ≠ Agent 框架。** Vercel AI SDK 做前者，LangChain 试图做后者。前者是有价值的标准化，后者容易变成过度抽象。

**学原理比学框架重要。** 框架每年都在变，LangChain 自己都从 Chain 进化到了 Graph。但 Agent Loop 的核心——think、act、observe——从 ReAct 论文提出到现在，一直没变过。



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



#### 《AI 编程的 30 条最佳实践》笔记

> [!NOTE]
>
> 文章写的有点过碎了，让人感觉 AI 成分过高；这里只摘录其中对自己有用的部分。

###### 创建测试检查清单

在项目根目录创建 `TEST_CHECKLIST.md`：

```md
# 测试检查清单 

## 功能测试 
- [ ] 所有按钮可点击 
- [ ] 表单验证正常 
- [ ] 数据正确保存 
- [ ] 错误提示友好 

## 性能测试 
- [ ] 页面加载 < 2 秒 
- [ ] 列表渲染流畅（60fps）
- [ ] 内存无泄漏 

## 兼容性测试 
- [ ] Chrome 最新版
- [ ] Safari 最新版
- [ ] 移动端 Safari
- [ ] 移动端 Chrome 

## 安全测试 
- [ ] 输入验证和清理 
- [ ] SQL 注入防护 
- [ ] XSS 防护 
- [ ] CSRF 防护
```




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
