#  linux与macOS备忘录



## <font color=FF0000>macOS快捷键</font>

#### <font color=FF0000>基础键</font>

**⌘ : Command** <font color=FF0000>**Option键在键盘映射中相当于windows中的win键 / meta键**</font>
**⇧ : Shift**
**⌥ : Option** <font color=FF0000>**Option键在键盘映射中相当于windows中的Alt键**</font>
**⌃ : Control** <font color=FF0000>**Control键在键盘映射中相当于windows中的Ctrl键**</font>
**↩︎ / ⌅ : Return / Enter**
**⇪ : caps lock**
**⎋ : Escape (Esc)**
**␣ : Space**
**⌫ : Delete**
**⌦ : 向前删除键（Fn+Delete）**
**↑ : 上箭头**
**↓ : 下箭头**
**← : 左箭头**
**→ : 右箭头**
**⇞ : Page Up（Fn+↑）**
**⇟ :  Page Down（Fn+↓）**
**↖ / Home : ( Fn + ←)**
**↘ / End : (Fn + →)**
**⇥ : 右制表符（Tab键）**
**⇤ : 左制表符（Shift+Tab）**
**⏏︎ : eject 介质推出键**

#### <font color=FF0000>快捷键</font>

**⌘ + Z 撤销**
**⇧ + ⌘ + Z 反撤销**

**␣ 向下翻页**
**⇧ + ␣ 向上翻页**

**⌘ + ← 移动到光标所在行首**
**⌘ + → 移动到光标所在行尾**
**⌘ + ⌫: 删除至行首**

**⌥ + ← 移动到光标所在前一个单词**
**⌥ + → 移动到光标所在后一个单词**
**⌥ + ⌫  删除一个单词**

**⌥ + F3 显示dock**
**⌘ + ` 同一应用间切换界面**

**⇧ + ⌘ + G (Finder中使用)前往路径**

**⌃ + ⌘ + F 最大化应用/ 取消**

**⌃ + ⌘ + ␣  打开“字符检视器”**

**⇧ + ⌘ + ␣ + 4 截取特定窗口**

**输入法相关**
**⌃ + ⇧ + ⌘ + C 简体转换为繁体（似乎无效…）**
**⌃ + ⌥ + ⇧ + ⌘ + C 繁体转换为简体**
**⌃ + ⌥ + ␣ 切换输入法**

**特殊字符**
**· ⇧ + ⌥ + 9 / 英文输入法下⎋下面的一个键    打出欧美姓名中的圆点 ·**
**· ⇧ + ⌥ + K 打出**

#### **在浏览器之类的app选中文本：**

**1. 三指选中当前词语**
**2. ⇧ + ⌥ + ← / →  ：向前向后以词为单位选中文本**
    **⇧ + ⌥ + ↑ / ↓ ：向上向下以行为单位选中文本**



#### **⌘ + ␣ **

- **短按：**spotlight
- **长按：**呼出 / 关闭 Siri



#### **⌃ + 方向键**

- **⌃ + ←：相当于四指滑动的左滑，前往左侧的页面**
- **⌃ + →：相当于四指滑动的右滑，前往右侧的页面**
- **⌃ + ↑：相当于四指滑动的上滑，显示所有页面，以及当前页面所有应用**
- **⌃ + ↓：相当于四指滑动的下滑，显示当前页面所有的此应用**



#### **Chrome相关**

**· ⌥ + ⌘ + I：打开/ 关闭 Developer Tools**
**· ⌥ + ⌘ + J：打开 / 关闭 Developer Tools 并进入控制台**
**· ⇧ + ⌘ + C： 打开 检查元素**



**更多快捷键，请参看设置中的键盘选项：**

<img src="https://s1.ax1x.com/2020/09/02/wpQmKP.png" style="zoom:45%;" />



### <font color=FF0000>Mac命令行使用</font>

##### `cd`命令，在命令行中进入查找输出的地址，如：

```bash
#方法一
cd `brew --prefix go`
#方法二
cd $(brew --prefix go)
```

**创建文件夹并进入该文件夹**

```sh
mkdir folder-name && cd $_
```

这里的 `&&` 和 `$_`下面都有解释。

**类似的：`open`命令**

```bash
open `brew --prefix go`    #打开软件
open folder                #在访达中打开文件夹
open folder/fileName.filetype     #打开默认的打开方式打开文件（如pdf）
```



#### <font color=FF0000>Homebrew使用</font>

- Homebrew是使用ruby开发的Mac的软件包管理器
- Homebrew 安装软件时将会<font color=FF0000>自动下载各种依赖</font>，这一点非常方便。
- 命令列表：

|     操作     | 命令 |
| :----------: | :--: |
| <font color=FF0000>更新Homebrew</font> |  `brew update`  |
| 查看Homebrew版本，及brew/cask状况 | `brew -v` |
|   <font color=FF0000>更新**所有**安装过的</font>软件包   | `brew upgrade` |
| <font color=FF0000>更新**指定**的</font>软件包 | `brew upgrade wget` |
| <font color=FF0000>查找</font>软件包 | `brew search wget` |
| <font color=FF0000>安装</font>软件包 | `brew install wget` |
| <font color=FF0000>卸载</font>软件包 | `brew uninstall wget` |
| <font color=FF0000>列出已安装</font>的软件包 | `brew list` |
| <font color=FF0000>查看</font>软件包<font color=FF0000>信息</font> | `brew info wget` |
|   <font color=FF0000>列出</font>软件包的<font color=FF0000>依赖关系</font>   | `brew deps wget` |
| 查看安装的扩展列表 | `brew tap` |
| <font color=FF0000>列出可以更新的</font>软件包 | `brew outdated` |
| <font color=FF0000>Homebrew不会自动移除旧版本的软件包</font>，移除旧版本软件包 | `brew cleanup` |
| 查看哪些软件包要被清除 | `brew cleanup -n` |
| 清除n天前的缓存（默认n=14） | `brew cleanup --prune n` |
| 查看Homebrew安装位置 | `brew --prefix` |
| 查看软件包在Homebrew中安装的位置 | `brew --prefix wget` |



#### 切换到root用户模式

```bash
sudo su
sudo -i  #进入root目录
su root #存疑

# 退出管理员模式
exit
```



#### 打开外接存储设备

进入根目录，进入Volume文件夹，这里存放所有外接存储设备



#### echo命令

```bash
# >> 表示在文件末尾追加命令
echo 'add content'>>filename

# > 表示删除原有内容，并添加
echo 'add content'>filename
```



#### 删除文件与文件夹

- 删除文件

```bash
rm -f fileName  # -f表示强行删除
```

- 删除文件夹

```bash
rm -rf folderName # -r表示递归，-f表示强行删除
```



#### 管道与`|`

**管道定义：**在类Unix操作系统（以及一些其他借用了这个设计的操作系统，如Windows）中，管道（英语：Pipeline）是<mark>一系列将标准输入输出<font color=FF0000>**链接**</font>起来的进程</mark>，**<mark>其中每一个<font color=FF0000>进程的输出被直接作为下一个进程的输入</font></mark>**。 <font color=FF0000>每一个链接都由匿名管道实现</font>[来源请求]。管道中的组成元素也被称作过滤程序。

`|`**的用法：**这个特殊的`|`字符告诉命令行解释器（Shell）**<font color=FF0000>将前一个命令的输出通过“管道”导入到接下来的一行命令作为输入</font>**。

摘自：[维基百科 - 管道 (Unix)]([https://zh.wikipedia.org/wiki/%E7%AE%A1%E9%81%93_(Unix)](https://zh.wikipedia.org/wiki/管道_(Unix))



#### `&`, `;` `&&` ,  `||`, `()`, `{}`

- **&** 命令<font color=FF0000>**同时**执行</font>

  ```sh
  command1 & command2 & command3   
  ```

-  **;**   <font color=FF0000>**不管**前面命令执行成功没有，后面的命令**继续执行**</font>

   ```sh
   command1; command2; command3
   ```

-  **&&** <font color=FF0000>**只有**前面命令执行成功，后面命令**才继续执行**</font>

   ```sh
   command1 && command2 [&& command3 ...]
   ```

   - 命令之间使用 && 连接，实现逻辑与的功能。

   - 只有在 && 左边的命令返回真（命令返回值 $? == 0），&& 右边的命令才会被执行。

   - 只要有一个命令返回假（命令返回值 $? == 1），后面的命令就不会被执行。

- **||**  如果<font color=FF0000>前面的命令**没有执行成功**</font>，<font color=FF0000>后面的命令**就开始执行**</font>

  ```sh
  command1 || command2 [|| command3 ...]
  ```

  - 命令之间使用 || 连接，实现逻辑或的功能。

  - 只有在 || 左边的命令返回假（命令返回值 $? == 1），|| 右边的命令才会被执行。这和 c 语言中的逻辑或语法功能相同，即实现短路逻辑或操作。

  - 只要有一个命令返回真（命令返回值 $? == 0），后面的命令就不会被执行。

- **()**  为了在当前shell中执行一组命令，可以用命令分隔符(即",")隔开每一个命令，并把所有的命令用圆括号()括起来。

  ```sh
  (command1; command2; command3 [; ...] )
  ```

  - 一条命令需要独占一个物理行，如果需要将多条命令放在同一行，命令之间使用命令分隔符`;`分隔。执行的效果等同于多个独立的命令单独执行的效果。
- <font color=FF0000>`()` 表示在当前 shell 中**将多个命令作为一个整体执行**</font>。<mark>需要注意的是，使用 `()` 括起来的命令在执行前面都不会切换当前工作目录，也就是说命令组合都是在当前工作目录下被执行的，尽管命令中有切换目录的命令</mark>。
  
- 命令组合常和命令执行控制结合起来使用
  
- **{}**  如果使用`{}`来代替`()`，那么相应的命令将在子shell而不是当前shell中作为一个整体被执行，只有在{}中所有命令的输出作为一个整体被重定向时，其中的命令才被放到子shell中执行，否则在当前shell执行。

  ```bash
  { command1; command2; command3 [; ...] }
  ```

  <font color=FF0000>**注意：**在使用{}时，{}与命令之间必须使用一个空格</font>

摘自：[Linux 命令行 &&与||](https://www.jianshu.com/p/25b0d6c9dc9f)

**//todo**

**花括号扩展（Brace expansion）**



#### Shell特殊变量： $0, $#, $*, $@, $?, $$, $!, $_和命令行参数$n

变量名只能包含数字、字母和下划线，因为<font color=FF0000>某些包含其他字符的变量有特殊含义，这样的变量被称为**特殊变量**</font>。

- **$0 ：** <font color=FF0000>**获取**</font>当前执行<font color=FF0000>脚本的**文件名**包括**路径**</font>
  
  - **dirname $0 ：**  <font color=FF0000>只取</font>当前执行脚本的<font color=FF0000>路径</font>
  - **basename $0 ：**  <font color=FF0000>只取</font>当前执行脚本<font color=FF0000>文件名</font>
  
- **$# ：**获取执行命令行(脚本)<font color=FF0000>参数的总个数</font>

- **$@ ：** 获取这个执行程序的<font color=FF0000>所有参数</font>

- **$* ：** 获取当前shell 的所有参数（注意与$@区别）

- **$! ：** 获取<font color=FF0000>**上一个**执行命令的**PID**</font>

- **$$ ：** 获取<font color=FF0000>**当前**shell的**PID**</font>

- **$_ ：** 获取<font color=FF0000>在此**之前执行**的命令或者脚本的**最后一个参数**</font>

- **$? ：** 获取<font color=FF0000>**上一个**命令的**退出状态**</font>

  所谓<font color=FF0000>退出状态</font>，就是<font color=FF0000>上一个命令执行后的返回结果</font>。退出时**返回值的表示含义如下**：

  - **0**： 表示运行成功
  - **2** ：权限拒绝
  - **1~125**： 表示运行失败，脚本命令、系统命令或者参数传递错误
  - **126** ：找到了该命令，但是无法执行
  - **127** ：未找到要运行的命令
  - **128** ：命令被系统强制结束

- **$n ：** <font color=FF0000>传递给脚本或函数的参数</font>。<mark>n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2</mark>。

**其中：$* 和 $@ 的区别**

$* 和 $@ 都表示传递给函数或脚本的所有参数，不被双引号`" "`包含时，都以"$1" "$2" … "$n" 的形式输出所有参数。

但是当它们被双引号" "包含时，<font color=FF0000>**"$*"** </font>会将<font color=FF0000>所有的参数作为一个整体</font>，以<font color=FF0000>"$1 $2 … $n"的形式输出所有参数</font>；<font color=FF0000>**"$@"** </font>会将<font color=FF0000>各个参数分开</font>，以<font color=FF0000>"$1" "$2" … "$n" 的形式输出所有参数</font>。

摘自：[Shell特殊变量： $0, $#, $*, $@, $?, $$, $!, $_和命令行参数$n](https://blog.csdn.net/w746805370/article/details/51044352)

**补充：!$**

>  !$ refers to the last argument from the previous bash command.  	
>
> **翻译：!$表示：上一条bash命令的最后一个参数**

摘自：[stack overflow - What does ./!$ mean in Linux?](https://stackoverflow.com/questions/38515790/what-does-mean-in-linux)



#### grep命令

**grep**（**<font color=FF0000>global</font>** search **<font color=FF0000>regular expression(RE)</font>** and **<font color=FF0000>print</font>** out the line，全面搜索正则表达式并把行打印出来）是一种强大的<font color=FF0000>文本搜索工具</font>，它<font color=FF0000>能使用正则表达式搜索文本，并把匹配的行打印出来</font>。

摘自：[linux命令大全 - grep命令](https://man.linuxde.net/grep)

##### **命令行参数**

```bash
-a / --text                                   #不要忽略二进制的数据。
-A<显示行数> / --after-context=<显示行数>        #除了显示符合范本样式的那一列之外，并显示该行之后的内容。
-b / --byte-offset                            #在显示符合样式的那一行之前，标示出该行第一个字符的编号。
-B<显示行数> / --before-context=<显示行数>       #除了显示符合样式的那一行之外，并显示该行之前的内容。
-c / --count                                  #计算符合样式的列数。
-C<显示行数> / --context=<显示行数>/-<显示行数>   #除了显示符合样式的那一行之外，并显示该行之前后的内容。
-d <动作> / --directories=<动作>               #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
-e<范本样式> / --regexp=<范本样式>               #指定字符串做为查找文件内容的样式。
-E / --extended-regexp                        #将样式为延伸的正则表达式来使用。
-f<规则文件> / --file=<规则文件>                 #指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
-F / --fixed-regexp                           #将样式视为固定字符串的列表。
-G / --basic-regexp                           #将样式视为普通的表示法来使用。
-h / --no-filename                            #在显示符合样式的那一行之前，不标示该行所属的文件名称。
-H / --with-filename                          #在显示符合样式的那一行之前，表示该行所属的文件名称。
-i / --ignore-case                            #忽略字符大小写的差别。
-l / --file-with- matches                     #列出文件内容符合指定的样式的文件名称。
-L / --files-without-match                    #列出文件内容不符合指定的样式的文件名称。
-n / --line-number                            #在显示符合样式的那一行之前，标示出该行的列数编号。
-o / --only-matching                          #只显示匹配PATTERN 部分。
-q / --quiet/--silent                         #不显示任何信息。
-r / --recursive                              #此参数的效果和指定"-d recurse"参数相同。
-s / --no-messages                            #不显示错误信息。
-v / --revert-match                           #显示不包含匹配文本的所有行。
-V / --version                                #显示版本信息。                
-w / --word-regexp                            #只显示全字符合的列。
-x --line-regexp                              #只显示全列符合的列。
-y                                            #此参数的效果和指定"-i"参数相同。
```

摘自：[RUNOOB - Linux grep 命令](https://www.runoob.com/linux/linux-comm-grep.html)



#### ps命令

ps命令是Process Status的缩写，用来<font color=FF0000>列出系统中当前运行的那些进程</font>。ps命令列出的是当前那些进程的快照，就是执行ps命令的那个时刻的那些进程。ps 为我们提供了进程的<font color=FF0000>一次性</font>的查看，它所提供的查看结果并不动态连续的；如果<font color=FF0000>想要动态的显示进程信息</font>，就可以使用top命令。

要对进程进行监测和控制，首先必须要了解当前进程的情况，也就是需要查看当前进程，而 ps 命令就是最基本同时也是非常强大的进程查看命令。<font color=FF0000>使用该命令可以确定：有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多的资源等等</font>。总之大部分信息都是可以通过执行该命令得到的。

**linux上进程有5种状态:** 

|                           状态名称                           |                    状态码                     |
| :----------------------------------------------------------: | :-------------------------------------------: |
|               运行(正在运行或在运行队列中等待)               |        R 运行 runnable (on run queue)         |
|     中断(休眠中, 受阻, 在等待某个条件的形成或接受到信号)     |                S 中断 sleeping                |
| 不可中断(收到信号不唤醒和不可运行, 进程必须等待直到有中断发生) | D 不可中断 uninterruptible sleep (usually IO) |
| 僵死(进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放) |      Z 僵死 a defunct (”zombie”) process      |
| 停止(进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行) |           T 停止 traced or stopped            |

**命令参数：**

```bash
a             #显示所有进程
-a            #显示同一终端下的所有程序
-A            #显示所有进程
c             #显示进程的真实名称
-N            #反向选择
-e            #等于“-A”
e             #显示环境变量
f             #显示程序间的关系
-H            #显示树状结构
r             #显示当前终端的进程
T             #显示当前终端的所有程序
u             #指定用户的所有进程
-au           #显示较详细的资讯
-aux          #显示所有包含其他使用者的行程 
-C            #<命令> 列出指定命令的状况
--lines       #<行数> 每页显示的行数
--width       #<字符数> 每页显示的字符数
--help        #显示帮助信息
--version     #显示版本显示
```

摘自：[每天一个linux命令（41）：ps命令](https://www.cnblogs.com/peida/archive/2012/12/19/2824418.html)

`ps [options]`输出示例：

<img src="https://s1.ax1x.com/2020/08/06/ageYqI.png" style="zoom: 50%;" />

- USER: 行程拥有者
- PID: pid
- %CPU: 占用的 CPU 使用率
- %MEM: 占用的记忆体使用率
- VSZ: 占用的虚拟记忆体大小
- RSS: 占用的记忆体大小
- TTY: 终端的次要装置号码 (minor device number of tty)
- STAT: 该行程的状态（上面的 R S D Z T）
- START: 行程开始时间
- TIME: 执行的时间
- COMMAND:所执行的指令

摘自：[RUNOOB - Linux ps命令](https://www.runoob.com/linux/linux-comm-ps.html)



#### killall

Linux系统中的killall命令用于杀死指定名字的进程（kill processes by name）。我们可以使用kill命令杀死指定进程PID的进程，如果要找到我们需要杀死的进程，我们还需要在之前使用ps等命令再配合grep来查找进程，而killall把这两个过程合二为一，是一个很好用的命令。

**命令格式：**

```sh
killall [参数] [进程名]
```

**命令参数：**

```sh
-Z         只杀死拥有scontext 的进程
-e         要求匹配进程名称
-I         忽略小写
-g         杀死进程组而不是进程
-i         交互模式，杀死进程前先询问用户
-l         列出所有的已知信号名称
-q         不输出警告信息
-s         发送指定的信号
-v         报告信号是否成功发送
-w         等待进程死亡
--help     显示帮助信息
--version  显示版本显示
```

摘自：[每天一个linux命令（43）：killall命令](https://www.cnblogs.com/peida/archive/2012/12/21/2827366.html)



#### ./configure、make、make install

- `./configure` 是用来<font color=FF0000>检测你的安装平台的目标特征</font>的。比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本。另外，这一步<font color=FF0000>一般用来生成 Makefile，为下一步的编译做准备</font>

- `make` 是用来<font color=FF0000>编译</font>的，它<font color=FF0000>从Makefile中读取指令</font>，<font color=FF0000>然后编译</font>。

- `make install`是用来安装的，它也<font color=FF0000>从Makefile中读取指令</font>，<font color=FF0000>安装到指定的位置</font>

摘自：[Linux 命令详解（三）./configure、make、make install 命令](https://www.cnblogs.com/tinywan/p/7230039.html)



#### ssh命令

- 以用户名userName连接主机IP命令如下：

  ```bash
  ssh userName@hostIP
  ```

- 如果本地用户名与远程用户名一致，登录时可以省略用户名。

  ```bash
  ssh hostIP
  ```

- SSH的<font color=FF0000>默认端口是22</font>，也就是说，你的登录请求会送进远程主机的22端口。使用<font color=FF0000>p参数</font>，可以修改这个端口。

  ```bash
  ssh -p 2222 userName@hostIP
  ```

摘自：[ssh用法及命令](https://blog.csdn.net/pipisorry/article/details/52269785)（后面还有更深层次的内容，有空看完）



#### tail命令

tail 命令<mark>将指定的文件的<font color=FF0000>**最后部分输出（tail）**</font>到标准设备，一般是终端</mark>。即：将把某个文件的最后几行显示到终端上，<mark>如果<font color=FF0000>该文件有更新，tail会自动刷新</font>，确保你看到最新的文件内容</mark>。

**命令格式：**

```bash
tail [ -f ] [ -c Number | -n Number | -m Number| -b Number | -k Number ] [ File ]
```

**参数说明：**

- -f         等同于`--follow=descriptor`，该参数用于<font color=FF0000>**监视File文件增长（非常重要）**</font>。
- -F        等同于`--follow=name --retry`，根据文件名进行追踪，并保持重试，即该文件被删除或改名后，如果再次创建相同的文件名，会继续追踪
- -c        Number 从 Number 字节位置读取指定文件。
- -n       Number 从 Number 行位置读取指定文件。
- -m      Number 从 Number 多字节字符位置读取指定文件，比如你的文件如果包含中文字，如果指定-c参数，可能导致截断，但使用-m则会避免该问题。
- -b       Number 从 Number 表示的512字节块位置读取指定文件。
- -k       Number 从 Number 表示的1KB块位置读取指定文件。
- File    指定操作的目标文件名

摘自：[玩转Linux命令 tail命令详解](https://www.jianshu.com/p/ee44fe0c5bae)

**另外：tailf**命令等同于`tail -f -n 10`（貌似tail -f或-F默认也是打印最后10行，然后追踪文件），<mark>与tail -f不同的是：如果文件不增长，它不会去访问磁盘文件</mark>，所以tailf特别适合那些便携机上跟踪日志文件，因为它减少了磁盘访问，可以省电



#### more命令

more命令，<mark>功能类似 cat</mark> ，cat命令是整个文件的内容从上到下显示在屏幕上。 <mark>more会以<font color=FF0000>一页一页的显示</font>方便使用者逐页阅读，而最基本的指令就是<font color=FF0000>按空白键（space）就往下一页显示</font></mark>，<font color=FF0000>按 b 键就会往回（back）一页显示</font>，而且还有搜寻字串的功能 。more命令从前向后读取文件，因此在启动时就加载整个文件。

#### <font color=FF0000>另外，值得注意的是：space显示下一页，b显示上一页是在linux中一个较为常见的用法，比如man命令也是这样</font>

**命令格式：**

```bash
more [-dlfpcsu] [-num] [+/pattern] [+linenum] [file...]
```

**命令参数：**

```bash
+num        #从第n行开始显示
-num        #定义屏幕大小为n行
+/pattern   #在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示 
-c          #从顶部清屏，然后显示
-d          #提示“Press space to continue，’q’ to quit（按空格键继续，按q键退出）”，禁用响铃功能
-f 		      #计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）
-l          #忽略Ctrl+l（换页）字符
-p          #通过清除窗口而不是滚屏来对文件进行换页，与-c选项相似
-s          #把连续的多个空行显示为一行
-u          #把文件内容中的下画线去掉
```

**常用操作命令：**

- Enter      向下n行，需要定义。默认为1行
- ⌃ + F       向下滚动一屏
- 空格键    向下滚动一屏
- ⌃ + B      返回上一屏
- =             输出当前行的行号
- :f             输出文件名和当前行的行号
- V             调用vi编辑器
- !命令      调用Shell，并执行命令 
- q             退出more

摘自：[每天一个linux命令(12)：more命令](https://www.cnblogs.com/peida/archive/2012/11/02/2750588.html)



#### curl和wget

curl由于可自定义各种请求参数所以在模拟web请求方面更擅长，wget由于支持ftp和Recursive所以在下载文件方面更擅长。类比的话curl是浏览器，而wget是迅雷。

摘自：[curl和wget的区别和使用](https://www.cnblogs.com/lsdb/p/7171779.html)



#### Linux下不同的文件类型有不同的颜色

- <font style=color:blue>蓝色</font>：表示目录;
- <font style=color:green>绿色</font>：表示可执行文件，可执行的程序;
- <font style=color:red>红色</font>：表示压缩文件或包文件;
- <font style=color:activeborder>浅蓝色</font>：表示链接文件;
- <font style=color:grey>灰色</font>：表示其它文件;
- 红色闪烁：表示链接的文件有问题了
- <font style=color:yellow>黄色</font>：表示设备文件

摘自：[linux下chmod +x的意思？为什么要进行chmod +x](https://blog.csdn.net/u012106306/article/details/80436911)



### Linux权限详解

Linux 下文件的权限类型一般包括<font color=FF0000>读，写，执行</font>。对应字母为<font color=FF0000> r、w、x</font>。

Linux 下<font color=FF0000>权限的粒度</font>有 <font color=FF0000>拥有者 、群组 、其它组</font> 三种。每个文件都可以针对三个粒度，设置不同的 r/ w/ x (读/ 写/ 执行) 权限。通常情况下，一个文件只能归属于一个用户和组， 如果其它的用户想有这个文件的权限，则可以将该用户加入具备权限的群组，一个用户可以同时归属于多个组。

##### **chmod命令语法**

```bash
chmod [可选项] <mode> <file...>  # 更改文件权限
```

<font color=FF0000>**TL;DR**</font>

参数说明：

> **[可选项]**
>
> ```bash
>   -c, --changes          like verbose but report only when a change is made (若该档案权限确实已经更改，才显示其更改动作)
>   -f, --silent, --quiet  suppress most error messages  （若该档案权限无法被更改也不要显示错误讯息）
>   -v, --verbose          output a diagnostic for every file processed（显示权限变更的详细资料）
>   --no-preserve-root  do not treat '/' specially (the default)
>   --preserve-root    fail to operate recursively on '/'
>   --reference=RFILE  use RFILE's mode instead of MODE values
>   -R, --recursive        change files and directories recursively （以递归的方式对目前目录下的所有档案与子目录进行相同的权限变更)
>   --help		显示此帮助信息
>   --version		显示版本信息
> ```
>
> **[mode]**  权限设定字串，详细格式如下 ：
>
> ```bash
> [ugoa...] [[+-=][rwxX]...] [,...]，
> ```
>
> 其中
> [ugoa...]
>
>     - u 表示该档案的拥有者
>     - g 表示与该档案的拥有者属于同一个群体(group)者，
>     - o 表示其他以外的人，
>     - a 表示所有（包含上面三者）且：<font color=FF0000>chmod a+x 等价于 chmod +x</font>。
>
> [+-=]
>
> - `+` 表示增加权限
> - `-`  表示取消权限
> - `=` 表示唯一设定权限。
>
> [rwxX]
>
> - r 表示可读取
> - w 表示可写入
> - x 表示可执行
> - X 表示只有当该档案是个子目录或者该档案已经被设定过为可执行。
>
> [file...]
>     文件列表（单个或者多个文件、文件夹）

##### 数字权限使用格式

**语法：**

```bash
chmod [1-7][1-7][1-7] file
# 三个数字分别对应u g o
# 即：以上命令等价于：chmod u=权限,g=权限,o=权限 file...
```

**解释：**

linux中规定 数字<font color=FF0000> 4 、2 和 1 表示读、写、执行权限</font>，即<font color=FF0000> r=4，w=2，x=1</font> 。此时其他的权限组合也可以用其他的八进制数字表示出来，如：

rwx = 4 + 2 + 1 = 7  若要同时设置 rwx (可读写运行） 权限则将该权限位 设置 为 4 + 2 + 1 = 7

rw = 4 + 2 = 6          若要同时设置 rw- （可读写不可运行）权限则将该权限位 设置 为 4 + 2 = 6

rx = 4 +1 = 5.           若要同时设置 r-x （可读可运行不可写）权限则将该权限位 设置 为 4 +1 = 5

摘自：[Linux权限详解（chmod、600、644、666、700、711、755、777、4755、6755、7755）](https://blog.csdn.net/u013197629/article/details/73608613)，另外，还有该文章还有更多内容。



#### which命令

which指令会在环境变量$PATH设置的目录里查找符合条件的文件

**参数：**

- n<文件名长度> 　指定文件名长度，指定的长度必须大于或等于所有文件中最长的文件名。
- -p<文件名长度> 　与-n参数相同，但此处的<文件名长度>包括了文件的路径。
- -w 　指定输出时栏位的宽度。
- -V 　显示版本信息。



#### lsof命令

lsof（list open files）是一个查看当前系统文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，该文件描述符提供了大量关于这个应用程序本身的信息。

**参数**

- -a 列出打开文件存在的进程
- -c<进程名> 列出指定进程所打开的文件
- -g 列出GID号进程详情
- -d<文件号> 列出占用该文件号的进程
- +d<目录> 列出目录下被打开的文件
- +D<目录> 递归列出目录下被打开的文件
- -n<目录> 列出使用NFS的文件
- -i<条件> 列出符合条件的进程。（4、6、协议、:端口、 @ip ）
- -p<进程号> 列出指定进程号所打开的文件
- -u 列出UID号进程详情
- -h 显示帮助信息
- -v 显示版本信息

***



## **Bash**

#### 变量类型

运行shell时，会同时存在**三种变量**：

- **局部变量** 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- **环境变量** 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
- **shell变量** shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行



#### 单引号和双引号的区别

单引号将剥夺其中的所有字符的特殊含义，而双引号中的'$'（参数替换）和'\`'（命令替换）是例外。所以，两者基本上没有什么区别，除非在内容中遇到了参数替换符 $ 和命令替换符 '\`'。所以下面的结果：

单引号

```bash
num=3
echo ‘$num’  # 结果：$num
```

双引号

```bash
echo “$num”  # 结果：3
```



#### Bash获取字符串长度

```bash
string="abcd"
echo ${#string} #输出 4
```



#### Bash提取子字符串

以下实例从字符串第 **2** 个字符开始截取 **4** 个字符：

```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```



#### Bash查找子字符串

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)，脚本中' **`**' 是反引号，而不是单引号 ' **'** '

```bash
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```



#### 获取数组的长度

获取数组长度的方法与获取字符串长度的方法相同，例如：

```bash
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```



#### <font color=FF0000>Bash的注释</font>

行内注释（Inline comments）：

```bash
# your comments
```

注释块（Block comments）：

- 方法一：

  ```bash
  :<<!
  your comments
  ...
  !
  ```

  类似的：

  ```bash
  :<<EOF
  your comments
  ...
  EOF
  ```

  ```bash
  :<<'
  your comments
  ...
  '
  ```

- 方法二：

  ```bash
  if false; then
  your comments
  fi
  ```

- 方法三：

  ```bash
  && {
  your comments
  }
  ```


摘自：[shell脚本注释方法](https://www.cnblogs.com/Braveliu/p/10855771.html)



### <font color=FF0000>iTerm2使用</font>

#### **快捷键**

- 新建标签：⌘ + t

- 关闭标签：⌘ + w

- 切换标签：⌘ + 数字 ⌘ + 左右方向键

- 切换全屏：⌘ + enter

- 查找：⌘ + f

- 分屏
  - 垂直分屏：⌘ + d
  - 水平分屏：⌘ +⇧ + d

- 切换屏幕：⌘ + ⌥ + 方向键 ⌘ + [ 或 ⌘ + ]

- 查看历史命令：⌘ + ;

  <img src="https://i.loli.net/2020/08/14/3HbJQrVvmDOESKA.png" style="zoom: 45%;" />

- 查看剪贴板历史：⌘ +⇧ + h

- **到行首：⌃ + a**

- **到行尾：⌃ + e**

- 前进后退：⌃ + f/b (相当于左右方向键)

- 上一条命令：⌃ + p

- 搜索命令历史：⌃ + r

  <img src="https://i.loli.net/2020/08/14/NyGkMtgEbeIR96J.png" style="zoom: 45%;" />

- **删除家族**

  - 删除当前光标的字符：⌃ + d
  - 删除光标之前的字符：⌃ + h
  - **<font color=FF0000>删除光标之前的单词：⌃ + w</font>**
  - 删除到文本末尾：⌃ + k
  - **<font color=FF0000>清除当前行：⌃ + u</font>**

- 交换光标处文本：⌃ + t

- 清屏：⌘ + r / ⌃ + l

- ⌘ + 数字在各 tab 标签直接来回切换

- 选择即复制 + 鼠标中键粘贴，这个很实用

- ⌘ + f 所查找的内容会被自动复制

- ⌘ + d 横着分屏 / ⌘ +⇧ + d 竖着分屏

- ⌘ + r = clear，而且只是换到新一屏，不会想 clear 一样创建一个空屏

- 输入开头命令后 按 ⌘ + ; 会自动列出输入过的命令

- ⌘ +⇧ + h 会列出剪切板历史

- ⌘ + 1 / 2 左右 tab 之间来回切换，这个在 前面 已经介绍过了

- ⌘← / ⌘→ 到一行命令最左边/最右边 ，这个功能同 C+a / C+e

- ⌥← / ⌥→ 按单词前移/后移，相当与 C+f / C+b，其实这个功能在Iterm中已经预定义好了，⌥f  / ⌥b，看个人习惯了

- ⌘+d：垂直分割；

- ⌘+shift+d：水平分割

- **自动完成**：输入打头几个字母，然后输入**⌘+;** iterm2将自动列出之前输入过的类似命令。

- **自动完成**输入打头几个字母，然后输入**⌘+;** iterm2将自动列出之前输入过的类似命令。

- **全屏切换**

  ⌘+enter进入与返回全屏模式

  Exposé所有Tab

  ⌘+⌥+e,并且可以搜索

以上摘自：[iTerm 2 常用快捷键](https://www.cnblogs.com/ckmiao/p/6279195.html)
