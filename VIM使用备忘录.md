# VIM使用备忘录



**换行：**`ESC + O`，同时不仅仅在光标位于当前行的末尾才生效，在当前行的任意位置都生效

##### 删除

- `dd` ：删除光标所在的<font color=FF0000>一行</font>
- `x` ：删除当前光标下的字符
- `dw` ：删除光标之后的单词剩余部分
- `d$` ：删除光标之后的该行剩余部分

##### 剪切

- `dd` ：删除光标所在的<font color=FF0000>一行</font>
- `n + dd` ：删除<font color=FF0000>光标所在行及以下的n行</font>

##### 复制

- `y` ：复制

##### 粘贴

- `p` ：粘贴在光标所在的行
- `P` ：粘贴复制到当前行的上一行

##### 查找

- **左斜杠 `/` ：从光标所在<font color=FF0000>位置向文件尾</font>搜索**
  
  示例：`／hello`

- **问号 `?` ：从光标所在<font color=FF0000>位置向文件头</font>搜索**
  
  示例：`?hello`

##### 撤销

- `u` ： 撤销上一步的操作

##### 光标移动

- `0` ：移动到行首
- `$` (⇧ + 4 ) ：移动到行尾
- ⇧ + L：将光标移动到文件的末尾

##### 切换大小写

- `u` ：（在选中的情况下）小写

- `U` ：（在选中的情况下）大写

##### 退出

- `ZZ` ：命令模式下保存当前文件所做的修改后退出vi

##### 注释

- `"` 作为注释

##### 将文字大小写切换

- **`~`** : 将光标选中的文字进行大小写切换
- `num~` ：将光标位置开始的 num 个字母进行大小写切换



#### VIM 常用的模式

- normal 模式
- insert 模式
- 分屏模式
- viusal 模式
- 命令模式（在 normal 模式下，键入`:` ）

##### normal 模式

- `:w` 写入保存

**进入 vim，默认进入的是 <font color=FF0000>normal模式</font>，在 normal 模式下进入 <font color=FF0000>Insert 模式</font>**

- i (insert)                          在当前<font color=FF0000>光标之前写入</font>
- a (append)                      在当前<font color=00FF00>光标之后写入</font>
- o (open a line below)    在<font color=0000FF>下一行写入</font>
- I                                        在<font color=FF0000>该行最前面写入</font>
- A                                       在<font color=00FF00>该行最后面写入</font>
- O                                      在<font color=0000FF>该行上一行写入</font>

##### VIM有分屏模式

- vs ( vertical split )     竖分屏
- sp ( split )                   横分屏
- q                                 退出分屏模式（退出顺序类似栈）
- 输入`set nu`[mber]  来给<font color=FF0000>代码加上行号</font>

##### VIM的全局替换示例

`% s/java/python/g`

解释如下:

- /         用于分隔

- %        表示全部文件

- s           表示替换操作

- java      被替换的字符串

- python   替换的字符串

- g        表示全局替换

**visual模式（可视化模式）**

visual 模式一般用于<font color=FF0000>块状选择文本，从而进行批量操作</font>。进入方式：在 normal 模式下

- 键入 `v`            进入 visual 选择，使用<font color=FF0000>方向键（上下左右），或者 hjkl</font> 以批量选中文本
- 键入 `V`           进行<font color=FF0000>行选择</font>，使用<font color=FF0000>方向键（上下），或者 jk </font>以批量选中多行
- 键入 `⌃ + v`    进行<font color=FF0000>方块选择</font>（实际用法与 v 类似）

**VIM在insert模式下的快速纠错<font color=FF0000>（同时这些命令在终端下也可以使用！）</font>**

- `⌃ + h`        删除<font color=FF0000>上一个字符</font>
- `⌃ + w`        删除<font color=FF0000>上一个单词</font>
- `⌃ + u`        当<font color=FF0000>光标在当前行的最后，则删除当前行</font>；否则，<font color=FF0000>删除当前光标之前的内容</font>

<font color=FF0000>**在终端下还有一些快捷键**</font>（这个应该是 emacs 的快捷键了...）

- `⌃ + a`        光标移动到<font color=FF0000>当前行的开头</font>
- `⌃ + e`         光标移动到<font color=FF0000>当前行的结尾</font>
- `⌃ + f`         <font color=FF0000>光标前移一个单位</font>
- `⌃ + b`        <font color=FF0000>光标后移一个单位</font>



#### 光标移动

**快速从<font color=FF0000>normal切换到insert</font>（在不使用⎋的情况下）**

- 可以使用 `⌃ + c` 或者  `⌃ + [` 以替代 **⎋**，其中<font color=FF0000>不推荐使用 `⌃ + c`</font>，因为这可能会影响 VIM 的插件
- 可以在<font color=FF0000>normal模式</font>下键入 `gi` 以快速跳转到上次编辑的地方继续编辑

##### VIM 在normal模式下的光标移动

<img src="https://s1.ax1x.com/2020/06/20/NlEYBq.png" style="zoom:50%;" />

##### 在单词之间移动

- w/W   移动到下一个**<font color=FF0000>word</font> / <font color=0000FF>WORD</font>**开头
- e/E     移动到下一个**<font color=FF0000>word</font> / <font color=0000FF>WORD</font>**结尾
- b/B     回到上一个**<font color=FF0000>word</font> / <font color=0000FF>WORD</font>**开头

其中：**<font color=FF0000>word </font>** 指的是以 **非空白符分割** 的单词，**<font color=0000FF>WORD</font>**是以 **空白符分割** 的单词

##### 行级移动

- `nG` / `:n` ：移动光标到当前文件的第n行

##### 行间搜索移动

- 使用 `f{char}` 可以移动到 `char` <font color=FF0000>字符上</font>，使用 `t{char}` 移动到 `char` 的<font color=FF0000>前一个字符</font>
- 如果该行包含多个该字符，可使用分号 ( ` ;`  ) 搜索下一个，逗号 ( ` , ` ) 搜索上一个
- 使用`F{char}`表示反过来（如：当前光标在行尾）<font color=FF0000>搜索前面的字符</font>

##### Vim水平（行内）移动

- 键入 `0` 移动到<font color=FF0000>行首第一个字符</font>**（绝对的第一个字符，即文本框的右边）**
- 键入`^` / ⇧ + 6 移动到<font color=FF0000>行首第一个非空白字符</font>（<font color=LightSeaGreen>建议使用这个，不建议使用 `0`</font> ）
- 键入` $` /  ⇧ + 4 移动到<font color=FF0000>行尾部</font>
- 键入 `g_` 移动到<font color=FF0000>行尾非空白符</font>

##### Vim 垂直（行间）移动

> 不常用

- 使用` (` /` ) `在句子间移动，光标移动到新的一行开头
- 使用` {` / ` } `在段落间移动 ，光标移动到<font color=FF0000>空白行</font>的开头

##### Vim 页面移动

- `gg` <font color=FF0000>**移动到文件的开头**</font> / `G` <font color=FF0000>**移动到文件的结尾**</font>；键入 `⌃ + o` 以快速返回
- `H`    跳转到屏幕的开头 ( Head )
- `M`   跳转到屏幕的中间 ( Middle )
- `L`     跳转到屏幕的结尾 ( Lower )
- `⌃ + u`   向上翻页 ( upward )
- `⌃ + f`    向下翻页 ( forward ) 。👀 在 emacs 中 ，这个快捷键是向后移动一个字符
- `zz`          把屏幕设为中间



#### 增删改查

##### 删除

###### 在 normal 模式下

- `x`       快速删除一个<font color=FF0000>字符</font>

- `dw`     快速删除 ( delete ) 一个<font color=FF0000>单词</font>，默认为 `daw`
  - `daw`         删除一个单词，并包含周围的空格 ( delete around word )
  - `diw`         删除一个单词，不包含周围的空格 ( delete inner word )
  
- `d + num` / `x + num` 都可以搭配数字来执行多次

- `dd`         删除当前行

- `dt*`       从光标处删除到`*`字符处，<font color=FF0000>\*字符不删</font>（delete to *）

- `d0`         从光标处删除到行首<font color=FF0000>（光标处的字符不删）</font>

- `d$`         从光标处删除到行尾

- `num + x` / `num + dd` 删除光标所在及后面的共 num 个字符 / 删除光标所在行及后面的共 num 行（同样的，可以在 Visual 模式下进行删除）

#### 快速修改

- `r`  / `R`    替换一个字符 ( replace ) / 替换一个字符并将光标移动到下一个字符上
- `c`     配合文本对象，进行快速修改 ( change )
  - `C` 删除整行，并进入插入模式
  - `caw`  与 `daw` 类似：删除一个单词，包含周围的空格，并进入插入模式
  - `ciw`  与 `diw` 类似：删除一个单词，不包含周围的空格，并进入插入模式
  - `ct*`  与 `dt*` 类似：从光标处删除到 `*` 字符处，<font color=FF0000>\*字符不删</font>，并进入插入模式
- `s`  / `S`  / `num + s` 删除当前字符，并进入插入模式 ( substitute ) / 删除整行，并进入插入模式 / 删除光标所在及后面共 num 个字符，之后进入插入模式 

#### 查找

- 使用 `/` 或者 `?` 进行前向或者反向搜索

- 使用 `n` / `N` 跳转到<font color=FF0000>下</font>一个或者<font color=FF0000>上</font>一个匹配

- 使用`*` / `#` 进行当前单词的<font color=FF0000>前向</font>/ <font color=FF0000>后向</font>匹配

- 在搜索过程中，键入 `:set hls` ( set highlight search ) ，以使得搜索结果高亮

  > 💡 值得补充的是：搜索结果的高亮如果不设置的话，将会一直存在，相当影响阅读，可以使用 `:noh` 或 `nohls`，再或者 `:nohlsearch` 使得高亮消除
  >
  > 学习自 Copilot Chat ，截图如下：
  >
  > <img src="https://s2.loli.net/2024/02/24/rBfNTzmuIECZDVH.png" alt="image-20240224153548696" style="zoom:50%;" />

- 键入 `:set incsearch` ，进行增量搜索（在输入搜索字符的<font color=FF0000>字符输入过程中</font>，只要找到匹配的，就高亮）

#### 搜索替换

- `substitute` 查找文本并替换文本，支持正则表达式

- 示例如下：`: [range]s[ubstitute]/{pattern}/{string}/[flags]`
  
  - `range`  操作范围，比如：`10, 20` 表示 10-20 行，`%` 表示全部
  - `pattern` 搜索模式
  - `string`  要替换后的文本
  - `flags` 替换的标志位
    - `g` ( global ) 表示全局范围内执行
    - `c` ( confirm ) 表示确认，可以确认或拒绝修改
    - `n` ( number ) 报告匹配到的次数而不替换，可以用来查询匹配次数



### 多文件操作

##### 多文件操作相关的概念

- Buffer    打开的一个文件的内存缓冲区
- window  Buffer 的可视化的分割区域，一个缓冲区可以分割成多个窗口，每个窗口也可以打开不同的缓冲区
- Tab         可以组织窗口的一个工作区，可以容纳一系列窗口的

<img src="https://s1.ax1x.com/2020/06/20/NlBTQe.png" style="zoom:70%;" />

##### Buffer 间切换

- `:ls`   列出当前所有缓冲区（显示打开的文件）
- `:e file_name` 打开 ( edit ) 文件
- `:b`家族
  - `:b n` 跳转到第n个缓冲区
  - `:bprevious`
  - `:bnext`
  - `:bfist`
  - `:blast`
  - `:b buffer_name `

##### 窗口分割

- `<⌃ + w> + s` / `:sp` 水平分割，`:sp` 更方便
- `<⌃ + w> + v` / `:vs` 垂直分割，`:vs` 更方便
- `:sp + filename` / `:vs + filename`  打开 filename 文件，并通过水平分割 / 垂直分割显示
- `:q` 退出分屏

补充：`tabnew + filename`  在新的标签页打开 filename

##### 切换窗口

- `<⌃ + w> + w`  在窗口间循环切换
- `<⌃ + w> + h` / H  切换到左边的窗口 / 将右边的窗口切换到左边
- `<⌃ + w> + j`  切换到下面的窗口
- `<⌃ + w> + k`  切换到上面的窗口
- `<⌃ + w> + l / L`  切换到右边的窗口 / 将左边的窗口切换到右边

##### 重排窗口大小

- `<⌃ + w> + =`   所有的窗口等宽、等高
- `<⌃ + w> + _`   最大化活动窗口的高度
- `<⌃ + w> + |`  最大化活动窗口的宽度
- `<⌃ + w> + _`  把活动窗口的高度设为[N]行
- `[N] + <⌃ + w> + |`  把活动窗口的宽度设为[N]行
- 更多的：通过 `:h window-resize` 以查看文档

##### Tab（标签页）操作

- `:tabe[dit] {filename}`  在新标签页中打开 `{filename}` ，这里的[]中的内容表示可省略，可用⇥键补全**<font color=FF0000>（下同）</font>**
- `<⌃ + w> + T`                        把当前窗口移动到一个新标签页
- `:tabc[lose]`                      关闭当前标签页及其中的所有窗口
- `:tabo[nly]`                        只保留活动标签页，关闭所有其他标签页

##### 标签切换操作

|      Ex命令      | 普通模式命令 |          用途           |
| :--------------: | :----------: | :---------------------: |
| `:tabn[ext] {N}` |   `{N}gt`    | 切换到序号为{N}的标签页 |
|   `:tabn[ext]`   |     `gt`     |    切换到下一标签页     |
| `:tabp[revious]` |     `gT`     |    切换到上一标签页     |



#### VIM 的 Text Object（文本对象）

操作文本对象，比如：`dw`（删除一个单词），`diw`

语法：`[number]<command>[text object]`

- number 表示次数
- command  命令，如：v(isual)，d(elete)，c(hange)，y(ank) 复制
  - i(nner)，a(round)
- text object 要操作的文本对象，比如 w(ord)，s(entence)，p(aragaph)



### VIM 复制粘贴和寄存器的使用

##### normal 模式的复制粘贴

- `y`  复制 ( yank )

- `p`  粘贴 ( put )

- `d*` 剪切 ( dd / diw / daw )

- 使用 v(isual) 命令选中复制的地方，复制与粘贴

- 对于文本对象 ( text object ) ：`yiw  ` 复制一个单词，`yy ` <font color=FF0000>复制一行</font>

##### 在 insert 模式中使用复制粘贴

- 一般人使用 `⌘ + v` ，不过会有问题：如果在 vim 中设置了 autoindent（自动支持锁进），在粘贴 Python 代码时会出现缩进错乱。对此解决方法：
  - 可以使用 `:set paste `在按下 `i` 进入 insert( paste ) 模式，使用 `⌘ + v` ；并使用 `:set nopaste` 使之失效，不过这样做有些麻烦。
  - `"+p`（这里是用了系统剪切板，具体：见下面 VIM 的寄存器）
  - `:set clipboard=unnamed` ，再键入 `p` ，让你直接复制粘贴系统剪切板的内容

##### VIM 的寄存器

- vim 操作的是寄存器而非系统剪切板，这和其他编辑器不同
- 默认我们使用 `d` 删除或者 `y` 复制的内容都放到了“无名寄存器”中。同时，对于两个相邻的字符颠倒可使用 `d` 删除（光标之下的自负）`p` 粘贴（到光标之后的位置）以修正
- vim 不通过单一粘贴板进行剪切、复制和粘贴，而是使用多组寄存器
  - 通过 `"{register}` 前缀可以指定寄存器，若不指定，则默认使用无名寄存器
  - 有名字的寄存器 a-z；另外，`""` 表示无名寄存器，缺省使用；`""p` 其实就等同于 `p`
  - 除了上面的寄存器，vim 还有一些其他常见的寄存器，如下：
    - 复制专用寄存器 `"0` ：使用 `y` 复制文本的同时会拷贝到`复制寄存器0`
    - 系统剪切板 `"+` ：可以在复制前加上 `"+` 以复制到系统剪切板
    - 其他剪切板，如：`"%当前文件名` ，`".上次插入的文本`
  - 使用寄存器示例：
    - 使用 `"ayiw` 复制一个单词到寄存器a 中
    - 使用 `"bdd` 删除当前行到寄存器b 中
  - `:reg reg_name` 查询寄存器中的内容
  - `:echo has("clipboard") ` 查看当前 vim 是否支持系统剪切板，若输出`1`则支持



#### VIM宏 ( macro )

宏可以看作是一系列命令的集合，我们可以使用宏「录制」一系列操作，然后用于回放。宏可以非常方便地把一系列的命令用在多文本上

- vim使用 `q` 来录制，同时也是使用 `q` 来结束录制
- 使用 `q{register}` 选择要保存的寄存器，来把录制的命令保存其中
- 使用 `@{register}` 回放寄存器中保存的一系列命令（键入`:normal @{register}`）



#### VIM 补全

##### 最常见的三种补全类型

- `⌃ + n` 和 `⌃ + p` 补全单词，其中 `⌃ + n` 不仅激活补全，也在多个候补项时有查看下一个 ( next ) 的功能；而 `⌃ + p` 在多个候补项时有查看上一个 ( pervious ) 的功能
- `⌃ + x`    `⌃ + f`  补全文件名/ 文件路径，同样的：`⌃ + n` 和 `⌃ + p` 有查看下一个 / 上一个功能
- `⌃ + x`  `⌃ + o` 补全代码（全能补全），需要开启文件类型检查（ `:filetype on`，`:set filetype` ），安装插件



#### 给 VIM 更换配色

- 使用 `:colorscheme` 显示当前的主题配色，默认是 default
- 用 `:colorscheme <⌃+d>` 可以显示所有的配色
- 有中意的配色后，用 `:colorscheme 配色名` 就可以修改配色

##### 杂项

在 normal模式 下，键入 `:help + 命令` 以获取相关的帮助文档

在 normal模式 下键入：`:syntax on ` 使得文本<font color=FF0000>语法高亮</font>



#### VIM 配置

编写 VIM 配置文件：*nix 用户新建一个隐藏文件：`vim ~/.vimrc`

在 vim 配置文件中，配置什么？

- 常用配置，比如 `:set nu` 设置行号，`colorscheme hybird` 设置主题

- 常用的 vim 映射（即：自定义快捷键），
  
  - 比如 noremap \<leader>w :w\<cr> 保存文件，示例：
    
    ```vimscript
    " 设置','为leader键
    " 在insert模式下键入 ,+w 即等价于 Esc+w
    let mapleader=','  
    inoremap <leader>w <Esc>:w<cr>
    ```
  
  - 基本映射是指在 <font color=FF0000>normal模式</font>下的映射，当然还有其他模式的映射，示例：
    
    使用 map 就可以实现映射。比如：
    
    - `:map - x`  按下 `-` 就会删除字符（实现`x`的功能），若要取消该 map 映射，只需要输入命令 `:unmap -`
    - `:map <space> viw` 按下 **␣** 时就会选中整个单词
    - `:map <c-d> `    使用 `⌃+d` 执行 `dd` 删除一行 
  
  - normal / visual / insert / command 模式都可以定义映射，用nmap / vamp / imap / cmap 定义映射只在normal / visual / insert / command 模式下分别有效，示例：
    
    `:vmap \ U` 在 visual 模式下将选中的文本设置为大写
  
  - vim 映射分为递归映射（ 如 A 映射为 B，B 映射为 C，C 映射为 A，这样映射将无法解释）和非递归映射，nmap / vamp / imap / cmap 的非递归映射分别对应为：n<font color=FF0000>no</font><font color=0000FF>re</font>map / v<font color=FF0000>no</font><font color=0000FF>re</font>amp / i<font color=FF0000>no</font><font color=0000FF>re</font>map / c<font color=FF0000>no</font><font color=0000FF>re</font>map 其中<font color=FF0000>no</font>表示非，<font color=0000FF>re</font>表示递归

- 自定义的 vimscript 函数 和 插件的配置

##### `.vimrc` 配置参考

```vimscript
syntax on	" 自动语法高亮
set number " 显示行号
set cindent
set smartindent " 开启新行时使用智能自动缩进
set showmatch " 插入括号时，短暂地跳转到匹配的对应括号
set ruler " 打开状态栏标尺
:set mouse=a "在vim所有模式下开鼠标，复制文档就可以不包含行号，但是复制到剪切板功能失效
```

学习自：[让MacOS的终端靓起来，给MacOS终端CLI添加颜色](https://zhuanlan.zhihu.com/p/60880207)



### VIM插件

vim 有很多插件管理器，常见的有：vim-plug，Vundle，Pathogen，Dein.Vim，volt等



### 需求和解决方法

将某一行代码上移一行：`dd + k + p`

- dd：将删除该行并将其添加到默认寄存器
- k：将向上移动一条线（j将向下移动一条线）
- P：将粘贴在当前行之上

学习自：[vi - 在Vim中上下移动整行](https://www.itranslater.com/qa/details/2109078928726950912)
