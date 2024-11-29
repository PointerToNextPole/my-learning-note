# IDEA使用技巧

#### <font color=FF0000>idea官方使用文档：</font>

https://www.jetbrains.com/help/idea/getting-started.html

**⌘ : Command**
**⇧ : Shift**
**⌥ : Option**
**⌃ : Control**
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

- **←**  <font size=4 color=FF0000>**/**</font> **→**：光标在文件夹上时，<font color=FF0000>收起或展开文件</font>

- **⇧ + ⎋**：最小化<mark>非编辑器页面</mark>的其他页面

- **⌥  + ↩︎**

  - 自动创建函数
  - list replace
  - 字符串format或者build
  - 接口实现
  - 单词拼写检查
  - 自动导包

- **⇧ + ⌘ + ⌫** <font size=4 color=FF0000>**/ **</font>**⌃ + ⌘ + ⌫** ：定位到上次编辑的位置 / 返回

- **代码折叠与展开**

  - **⌘ + +** ：展开
  - **⌘ + -** ： 折叠

- idea**<font color=FF0000>不同项目间</font>**的切换

  - **⌥ + ⌘ + `**：切换到上一个idea窗口
  - **⌥ + ⇧ + ⌘ + `**：切换到下一个idea窗口

- 文件操作

  - **F5**：复制文件
  - **F6**：移动文件
  - **⌘ + C** : 复制相对文件名
  - **⇧ + ⌘ + C** : 复制绝对路径的文件名

- 粘贴
  - **⌘ + V**：粘贴（默认带格式化）
  - **⇧ + ⌥ + ⌘ + V**：简单粘贴（粘贴但不格式化）
  - **⇧ + ⌘ + V**：<font color=FF0000>显示剪切板历史</font>（通过数字进行选择）

- **⌘ + F12**：显示当前类的结构

- **⌥ + ⌘ + N**：inline local variable

- 标记书签

  - **F3**：标记一般的书签 / 取消
  - **⌥ + F3**：标记带标志的书签

- **⌥ + ⇧ + F**：标记到收藏

- 跳转到错误

  - **F12**：跳转到下一个高亮错误
  - **⇧ + F12**：跳转到上一个高亮错误

- DeBug

  - **⌘ + F8**：添加断点 / 取消断点
  - **⌃ + ⌥ + D**：进入Debug
  - **F8** ：单步运行
  - **⌥ + ⌘ + R** ：resume，跳到下一个断点，若无下一个断点，则执行到最后
  - **⇧ + ⌘ + F8** : 查看所有断点 /  <font size=4 color=FF0000>条件断点</font>
  - 禁止所有断点  
  - **⌥ + F8**：计算表达式（新版idea自动计算）
  - **⌥ + F9**：运行到光标处
  - 在debug中的variable窗口里想要修改的变量上按下F2修改值
  
- `Ctrl + Alt + S`  <font size=4 color=FF0000>**/**</font> **⌘ + , ** ：进入设置

- `Ctrl + P` <font size=4 color=FF0000>**/**</font> **⌘ + P** ：方法参数提示

- `Ctrl + Shift + N`：打开文件并直接定位到某一行

- `Ctrl + Shift + F12` <font size=4 color=FF0000>**/ **</font> **⇧ + ⌘ + F12**：隐藏所有非编辑器的栏目

- `Alt + Insert`  <font size=4 color=FF0000>**/**</font> **⌘ + N** ：调出Generate，自动生成Getter / Setter / Constructor / ...

- `Ctrl + R`  <font size=4 color=FF0000>**/**</font>  **⌘ + R**：<font color=FF0000>**替换**</font>

- `Ctrl + Shift + Alt + L` <font size=4 color=FF0000>**/ **</font>**⇧ + ⌥ + ⌘ + L** ：<font color=FF0000>**格式化代码**</font>

- **抽取与反抽取**

  - `Ctrl + Alt + V` <font size=4 color=FF0000>**/**</font> **⌥  + ⌘ + V**：<font color=FF0000>**抽取变量（value）**</font>
    - **⌥ + ⌘ + N**：反抽取变量：简化代码，减少变量，将变量融合进原文
  - **⌥  + ⌘ + C**：抽取静态常量（constant）
  - **⌥  + ⌘ + F**：抽取成员变量（field）
  - **⌥  + ⌘ + P**：抽取为参数（parameter）
  - **⌥  + ⌘ + M**：抽取一段代码为函数（method）

- **⌘ + G**  <font size=4 color=FF0000>**/**</font> **⇧+ ⌘ + G**：搜索的下一个/上一个

- **⇧ + ⌘ + F** ：在路径中查找（find in path），可以用于搜索字符串

  <img src="https://s1.ax1x.com/2020/06/19/NMiRfA.png" style="zoom: 50%;" />

- `Shift + Enter`  <font size=4 color=FF0000>**/ **</font>**⇧ + ↩︎** ：相当于光标移动到行尾，并回车

- `Ctrl + E`  <font size=4 color=FF0000>**/ **</font>**⌘ + E** ：**<font color=FF0000>快速查找和打开</font>**<mark>最近<font color=FF0000>**使用过**</font>的文件</mark>

- `Ctrl + Alt + O`  <font size=4 color=FF0000>**/ **</font>**⌃ + ⌥ + O** ：<font color=FF0000>**自动导包，及去除多余的包**</font>

- 修改包含依赖的变量，用于重构

  - `Shift + F6`  <font size=4 color=FF0000>**/**</font> **⇧ + F6** ：<font color=FF0000>**（批量）修改变量名**</font>
  - **⌘ + F6**：<font color=FF0000>**（批量）修改方法签名**</font>

- **⌥ + F7**：**<font color=FF0000>与上面类似，查看哪里调用了该方法（依赖）</font>**

- **⌘ + F12**：显示当前文件结构，比如有哪些<font color=FF0000>成员</font>和<font color=FF0000>方法</font>（与左边栏的Structure似乎功能一样）

- `Ctrl + shift + F12` <font size=4 color=FF0000>**/ **</font>：编辑器全屏 / 编辑器取消全屏

- **⌘ + /**：行注释

- 复制与删除某行

  - `Ctrl + D` <font size=4 color=FF0000> **/ **</font>**⌘ + D**：复制当前光标所在的行，在下一行粘贴；取`D`意为`duplicate`
  - `Ctrl + Y`  <font size=4 color=FF0000>**/  **</font>**⌘ + X** （即剪切）<font size=4 color=FF0000>**/   **</font>**⌘ + ⌫ **：<font color=FF0000>**删除当前光标所在的行**</font>

- `Ctrl + Tab` <font size=4 color=FF0000>**/ **</font> **⌃ + ⇥**

  - 短按：切换为最近编辑的（一个）标签页

  - 长按`Ctrl`，唤出`Switcher`，不断按下`Tab`，切换页面，如下所示：

    <img src="https://s1.ax1x.com/2020/05/11/YG7DoV.png" alt="Switcher" style="zoom:50%;" />
  
- **⌘ + [**   <font size=4 color=FF0000>**/**</font> **⌘ + ]**：与上面类似的，回退到刚刚<font color=FF0000>**浏览**</font>的位置 / 返回

- `Ctrl + Shift + E` <font size=4 color=FF0000>**/ **</font> **⌘ + E** ：**<font color=FF0000>快速查找和打开</font>**<mark>最近<font color=FF0000>**编辑**</font>的文件</mark>，<mark style=background-color:aqua>注意与上一个**浏览**的区别</mark>

- `Ctrl + Shift + 方向键` <font size=4 color=FF0000>**/**</font> **⇧  + ⌘ + 方向键**

  - `Ctrl + Shift + Up / Down`  <font size=4 color=FF0000>**/**</font> **⇧  + ⌘ + ↑ **/ **↓**：改变	代码位置，将选中的代码上下移动
    - 若选中的是<font color=FF0000>**一行代码**</font>，则将该行的代码**<font color=FF0000>以行为单位</font>**上下移动
    - 若选中的<font color=FF0000>**一块代码**</font>，则将该块代码<font color=FF0000>**以块为单位**</font>上下移动
  - `Ctrl + Shift + Left / Right`：类似于`Ctrl + W`，从少到多选中光标所在位置的左 / 右的文本

- `Ctrl + Shift + F`：全局搜索

- **⇧ + ⌘ + U**：快速将选中的字符<font color=FF0000>切换大小写</font>

- **搜索家族**

  <img src="https://resources.jetbrains.com/help/img/idea/2020.1/search_all_window.png" style="zoom:50%;" />

  <mark>（使用 ⇥（制表键）以切换标签）</mark>

  - **两次⇧**：搜索全部（All）
  - **⌘ + O**：搜索类（Class）
  - **⇧ + ⌘ + O**：搜索文件（Files）
  - **⌥ + ⌘ + O**：搜索符号（Symbols），包含<font color=FF0000>**函数和属性**</font>
  - `Ctrl + Shift + A ` <font size=4 color=FF0000>**/ **</font>**⇧ + ⌘ + A**：搜索动作（Actions）

- `Ctrl + W` <font size=4 color=FF0000>**/ **</font>**⌥ + ↑** ：选中当前选中的外层（尤其适合用于编写html代码）

- `Home` / `End`：移动到行头 / 行尾

- `Ctrl + Home` / `Ctrl + End`：移动到整个文档开始 / 结束

- `Alt + *` <font size=4 color=FF0000>**/ **</font> **⌘+ *** 系列，类似的：凡是<font color=FF0000><u>加下划线的字母</u></font>都是`Alt + *` 的快捷键

  - `Alt + 1` <font size=4 color=FF0000>**/ **</font> **⌘ + 1**：<font color=FF0000>**显示右边栏被隐藏的项目列表**</font>
  - `Alt + Direction-Key`系列
    - `Alt + Up / Down`：光标指向当前元素上一层元素（适合用于编写html代码）
    - `Alt + Left / Right`：<font color=FF0000>**切换当前编辑文件为：当前编辑文件列表左边 / 右边的页面**</font>
  - **⎋ **：<font color=FF0000>**光标返回编辑区**</font>

- **⌘ + .** ：收起 / 展开父层代码

- 列操作
  
  - 按下`Alt` <font size=4 color=FF0000>**/**</font> **⌥** ，选中需要列操作的列，开始列操作（在WebStorm中似乎是**⇧ + ⌥**）
  - **⌃ + ⌘ + G**：<font color=FF0000>**选中所有光标所选中的内容**</font><mark>（行操作的灵魂所在）</mark>
  
- <font color=FF0000>**对于HTML文件的IDEA快捷键**</font>

  - `Tab`  <font size=4 color=FF0000>**/ **</font>**⇥**：自动补全。示例：`div.col-lg-9` ，光标在`9`的后面时，按下`Tab` 变成 ： `<div class=col-lg-9></div>`

- <font color=FF0000>**Git相关**</font>

  - annotation，查找这段代码的作者

  - **⌃ + ⌥ + ⇧ + ↓**  <font size=4 color=FF0000>**/ **</font> **⌃ + ⌥ + ⇧ + ↑**  ：移动到所有修改的地方（向上或向下）

  - **⌥ + ⌘ + Z** ：撤销光标所在改动之处（回滚）

    - 光标在文件中未改动的地方，按下**⌥ + ⌘ + Z**，会显示窗口，按下Revert键，会滚到提交状态
    - 光标在文件夹上，按下**⌥ + ⌘ + Z**，会显示窗口，按下Revert键，整个文件会滚到提交状态

    <img src="https://s1.ax1x.com/2020/06/25/NBljHg.png" style="zoom:50%;" />

    <font color=FF0000>这里少一张图片</font>

  - local history：不使用git，记录本地修改历史

    - show history：显示修改
    - put label：加上标签的修改，以显示最近的修改之处

- **idea的关联一切**

  - 与spring关联，明悉项目结构之间的关系
  - 与数据库关联
    - 在我项目的测试中似乎并不能，在写sql语句时，idea做到「完全的」「自动」提示，不过可以参考下面这篇文章（[IDEA sql自动补全/sql自动提示/sql列名提示](https://blog.csdn.net/qq2523208472/article/details/89366264)），可以通过「手动配置」实现提示。

- ⌥ + ⇧ + ⌘ + U：显示类 / maven的依赖图

- ⌃ + H：查看类继承结构

- ⌃ + ⌥ + H：查看方法调用层次

类似的可参考文章：[IntelliJ IDEA自用快捷键 转载](https://www.cnblogs.com/sekai/p/5857286.html)

以及：[IntelliJ IDEA 2019 快捷键终极大全，速度收藏！](https://blog.csdn.net/WantFlyDaCheng/article/details/100078777)<font color=FF0000>**(非常全)**</font> 

[Idea 最全快捷键, 不看后悔系列](https://www.jianshu.com/p/32c804a4c904)



#### <font color=FF0000>Live template</font>（可自定义）

<font color=FF0000>**自带Live template**</font>

- `psvm`：`public static void main(){}`
- `prsf`：`private static final`
- `psf`：`public static final`
- `psfi`：`public static final int`
- `psfs`：``public static final String`

<font color=FF0000>**自定义的Live template**</font>，示例：

```java
//Live template name: main1
public static void main1(){
  $END$  //形成后，光标停留处
}
```

```java
//Live template name: psfi
private static final int $var1$ = $var2$ //$var1$和$var2$都是占位符，对于idea而言是变量，等待使用者自定义
```



#### <font color=FF0000>postfix</font>（不可自定义）

- 对一个数组/容器进行for循环，可使用`arrayName.for `的postfix，可对其进行foreach遍历

- `num.fori` ==> `for(int i = 0; i < num; i++){}`，对于容器一样

- `num.forr` ==> `for(int i = num - 1; i >= 0; i--){}`，对于容器一样

- 对一个变量 / 常量 进行打印，可使用`.sout`的postfix，对其进行打印（`System.out.println()`）

- 对一次循环指定循环的次数，可使用`loopCount.for`的postfix，对其进行循环

  ```java
  for (int i = 0; i < loopCount; i++){
      //TODO
  }
  ```

- `name.field` ==> 在类中自动追加一个名为name的属性
- `name.nn` ==> `name != null`
- `name.return` ==> `return name;`



#### 官方快捷键 for macOS

![](https://s1.ax1x.com/2020/06/25/NDdSHI.jpg)

### 补充

##### Mac 键盘符号和修饰键说明

-  `⌘` Command
- `⇧` Shift
- `⌥` Option
- `⌃` Control
- `↩︎` Return/Enter
- `⌫` Delete
- `⌦` 向前删除键（Fn+Delete）
- `↑` 上箭头
- `↓` 下箭头
- `←` 左箭头
- `→` 右箭头
- `⇞` Page Up（Fn+↑）
- `⇟` Page Down（Fn+↓）
- `Home` Fn + ←
- `End` Fn + →
- `⇥` 右制表符（Tab键）
- `⇤` 左制表符（Shift+Tab）
- `⎋` Escape (Esc)

##### Editing（编辑）

- `⌃Space` 基本的代码补全（补全任何类、方法、变量）
- `⌃⇧Space` 智能代码补全（过滤器方法列表和变量的预期类型）
- `⌘⇧↩` 自动结束代码，行末自动添加分号
- `⌘P` 显示方法的参数信息
- `⌃J, Mid. button click` 快速查看文档
- `⇧F1` 查看外部文档（在某些代码上会触发打开浏览器显示相关文档）
- `⌘+鼠标放在代码上` 显示代码简要信息
- `⌘F1` 在错误或警告处显示具体描述信息
- `⌘N, ⌃↩, ⌃N` 生成代码（getter、setter、构造函数、hashCode/equals,toString）
- `⌃O` 覆盖方法（重写父类方法）
- `⌃I` 实现方法（实现接口中的方法）
- `⌘⌥T` 包围代码（使用if..else, try..catch, for, synchronized等包围选中的代码）
- `⌘/` 注释/取消注释与行注释
- `⌘⌥/` 注释/取消注释与块注释
- `⌥↑` 连续选中代码块
- `⌥↓` 减少当前选中的代码块
- `⌃⇧Q` 显示上下文信息
- `⌥↩` 显示意向动作和快速修复代码
- `⌘⌥L` 格式化代码
- `⌃⌥O` 优化import
- `⌃⌥I` 自动缩进线
- `⇥ / ⇧⇥` 缩进代码 / 反缩进代码
- `⌘X` 剪切当前行或选定的块到剪贴板
- `⌘C` 复制当前行或选定的块到剪贴板
- `⌘V` 从剪贴板粘贴
- `⌘⇧V` 从最近的缓冲区粘贴
- `⌘D` 复制当前行或选定的块
- `⌘⌫` 删除当前行或选定的块的行
- `⌃⇧J` 智能的将代码拼接成一行
- `⌘↩` 智能的拆分拼接的行
- `⇧↩` 开始新的一行
- `⌘⇧U` 大小写切换
- `⌘⇧] / ⌘⇧[` 选择直到代码块结束/开始
- `⌥⌦` 删除到单词的末尾（⌦键为Fn+Delete）
- `⌥⌫` 删除到单词的开头
- `⌘+ / ⌘-` 展开 / 折叠代码块
- `⌘⇧+` 展开所以代码块
- `⌘⇧-` 折叠所有代码块
- `⌘W` 关闭活动的编辑器选项卡

##### Search/Replace（查询/替换）

- `Double ⇧` 查询任何东西
- `⌘F` 文件内查找
- `⌘G` 查找模式下，向下查找
- `⌘⇧G` 查找模式下，向上查找
- `⌘R` 文件内替换
- `⌘⇧F` 全局查找（根据路径）
- `⌘⇧R` 全局替换（根据路径）
- `⌘⇧S` 查询结构（Ultimate Edition 版专用，需要在Keymap中设置）
- `⌘⇧M` 替换结构（Ultimate Edition 版专用，需要在Keymap中设置）

##### Usage Search（使用查询）

- `⌥F7 / ⌘F7` 在文件中查找用法 / 在类中查找用法
- `⌘⇧F7` 在文件中突出显示的用法
- `⌘⌥F7` 显示用法
- `⌘⇧I` 查看定义的类,快速查看

##### Compile and Run（编译和运行）

- `⌘F9` 编译Project
- `⌘⇧F9` 编译选择的文件、包或模块
- `⌃⌥R` 弹出 Run 的可选择菜单
- `⌃⌥D` 弹出 Debug 的可选择菜单
- `⌃R` 运行
- `⌃D` 调试
- `⌃⇧R, ⌃⇧D` 从编辑器运行上下文环境配置

##### Debugging（调试）

- `F8` 进入下一步，如果当前行断点是一个方法，则不进入当前方法体内
- `F7` 进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果该方法体还有方法，则不会进入该内嵌的方法中
- `⇧F7` 智能步入，断点所在行上有多个方法调用，会弹出进入哪个方法
- `⇧F8` 跳出
- `⌥F9` 运行到光标处，如果光标前有其他断点会进入到该断点
- `⌥F8` 计算表达式（可以更改变量值使其生效）
- `⌘⌥R` 恢复程序运行，如果该断点下面代码还有断点则停在下一个断点上
- `⌘F8` 切换断点（若光标当前行有断点则取消断点，没有则加上断点）
- `⌘⇧F8` 查看断点信息

##### Navigation（导航）

- `⌘O` 查找类文件
- `⌘⇧O` 查找所有类型文件、打开文件、打开目录，打开目录需要在输入的内容前面或后面加一个反斜杠`/`
- `⌘⌥O` 前往指定的变量 / 方法
- `⌃← / ⌃→` 左右切换打开的编辑tab页
- `F12` 返回到前一个工具窗口
- `⎋` 从工具窗口进入代码文件窗口
- `⇧⎋` 隐藏当前或最后一个活动的窗口，且光标进入代码文件窗口
- `⌘⇧F4` 关闭活动run/messages/find/… tab
- `⌘L` 在当前文件跳转到某一行的指定处
- `⌘E` 显示最近打开的文件记录列表
- `⌘⌥← / ⌘⌥→` 退回 / 前进到上一个操作的地方
- `⌘⇧⌫` 跳转到最后一个编辑的地方
- `⌥F1` 显示当前文件选择目标弹出层，弹出层中有很多目标可以进行选择(如在代码编辑窗口可以选择显示该文件的Finder)
- `⌘B / ⌘ 鼠标点击` 进入光标所在的方法/变量的接口或是定义处
- `⌘⌥B` 跳转到实现处，在某个调用的方法名上使用会跳到具体的实现处，可以跳过接口
- `⌥ Space, ⌘Y` 快速打开光标所在方法、类的定义
- `⌃⇧B` 跳转到类型声明处
- `⌘U` 前往当前光标所在方法的父类的方法 / 接口定义
- `⌃↓ / ⌃↑` 当前光标跳转到当前文件的前一个/后一个方法名位置
- `⌘] / ⌘[` 移动光标到当前所在代码的花括号开始/结束位置
- `⌘F12` 弹出当前文件结构层，可以在弹出的层上直接输入进行筛选（可用于搜索类中的方法）
- `⌃H` 显示当前类的层次结构
- `⌘⇧H` 显示方法层次结构
- `⌃⌥H` 显示调用层次结构
- `F2 / ⇧F2` 跳转到下一个/上一个突出错误或警告的位置
- `F4 / ⌘↓` 编辑/查看代码源
- `⌥ Home` 显示到当前文件的导航条
- `F3`选中文件/文件夹/代码行，添加/取消书签
- `⌥F3` 选中文件/文件夹/代码行，使用助记符添加/取消书签
- `⌃0...⌃9` 定位到对应数值的书签位置
- `⌘F3` 显示所有书签

##### Refactoring（重构）

- `F5` 复制文件到指定目录
- `F6` 移动文件到指定目录
- `⌘⌫` 在文件上为安全删除文件，弹出确认框
- `⇧F6` 重命名文件
- `⌘F6` 更改签名
- `⌘⌥N` 一致性
- `⌘⌥M` 将选中的代码提取为方法
- `⌘⌥V` 提取变量
- `⌘⌥F` 提取字段
- `⌘⌥C` 提取常量
- `⌘⌥P` 提取参数

##### VCS/Local History（版本控制/本地历史记录）

- `⌘K` 提交代码到版本控制器
- `⌘T` 从版本控制器更新代码
- `⌥⇧C` 查看最近的变更记录
- `⌃C` 快速弹出版本控制器操作面板

##### Live Templates（动态代码模板）

- `⌘⌥J` 弹出模板选择窗口，将选定的代码使用动态模板包住
- `⌘J` 插入自定义动态代码模板

##### General（通用）

- `⌘1...⌘9` 打开相应编号的工具窗口
- `⌘S` 保存所有
- `⌘⌥Y` 同步、刷新
- `⌃⌘F` 切换全屏模式
- `⌘⇧F12` 切换最大化编辑器
- `⌥⇧F` 添加到收藏夹
- `⌥⇧I` 检查当前文件与当前的配置文件
- `§⌃, ⌃`` 快速切换当前的scheme（切换主题、代码样式等）
- `⌘,` 打开IDEA系统设置
- `⌘;` 打开项目结构对话框
- `⇧⌘A` 查找动作（可设置相关选项）
- `⌃⇥` 编辑窗口标签和工具窗口之间切换（如果在切换的过程加按上delete，则是关闭对应选中的窗口）

#### 十一、Other（一些官方文档上没有体现的快捷键）

- `⌘⇧8` 竖编辑模式

摘自：[IntelliJ IDEA For Mac 快捷键](https://www.cnblogs.com/exmyth/p/7600658.html)