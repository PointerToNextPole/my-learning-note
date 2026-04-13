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
  > [!TIP]
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

注意几个要点：

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

Cursor 支持以下 MCP 协议能力和扩展：

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