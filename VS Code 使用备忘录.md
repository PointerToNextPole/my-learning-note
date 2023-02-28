# VS Code 使用备忘录



#### 一些资料

官方文档：https://code.visualstudio.com/docs

有效果 gif 的快捷键教程：https://www.vscheatsheet.com



#### 一些设置

##### 进入 settings.json 方法

按下 ⇧ + ⌘ + P ，会显示搜索框；在输入框中输入 `settings.json` ，会显示如下结果：

<img src="https://s2.loli.net/2022/05/18/bd3k7BDFAqoCc92.png" alt="image-20220518215940563" style="zoom:50%;" />

选择第一个即可进入



##### 函数添加 “参数名提示” 功能

打开 settings.json 配置文件（方法见 [[#进入 settings.json 方法]]），添加如下配置；即可应用。

```json
"javascript.inlayHints.parameterNames.enabled": "all",
"typescript.inlayHints.parameterNames.enabled": "all",
```



##### “粘性滚动” 设置

```json
"editor.experimental.stickyScroll.enabled": true
```

样式如下：

<img src="https://s2.loli.net/2022/08/06/XwtKEZvqNJOUIpP.png" alt="image-20220806194800985" style="zoom:45%;" />

另外，发现用来写 markdown 效果出奇的好：

<img src="https://s2.loli.net/2022/09/05/1I2spWkfXBeZ4VH.png" alt="image-20220905005539528" style="zoom:50%;" />



#### 快捷键

- **⇧ + ⌥ + F** ：代码格式化。注意：如果代码检测有错误，将不会执行格式化

- **⇧ + ⌘ + L** ：批量重命名

  > 👀 不知道为什么这个方法偶尔会失灵... 😳 
  >
  > 💡 不过，在 www.vscheatsheet.com 中找到了更好的解决方案：光标聚焦使用 `F2` 会出现如下效果：
  >
  > <img src="https://s2.loli.net/2022/09/16/j9Dd7elL3YZg4vc.png" alt="image-20220916203504239" style="zoom:50%;" />

- **⌃ + Space** ：智能建议（只能是短按，长按和 Mac 自带切切换输入法相冲突），光标悬浮也可以产生同样的效果

- **⌘ + P** ：搜索文件，并快速跳转。有点类似于 IDEA 中的 “double space”

- **⌘ + K  ➡ ⌘ + S** ：键盘快捷键列表，效果如下：

  <img src="https://s2.loli.net/2022/09/16/7IYL5C1qSB4Xj8f.png" alt="image-20220916205802024" style="zoom:40%;" />

- **⌘ + J** / **⌃ + `** ：打开 VSC 内置的 Terminal

- **⌘ + B** ：展示 / 隐藏 活动栏 ( Activity Bar ) 



#### User and Workspace Settings

You can <font color=dodgerBlue>configure Visual Studio Code to your liking through its various settings</font>. Nearly every part of VS Code's editor, <font color=red>user interface</font>, and functional behavior has options you can modify.

> 👀 之所以 “user interface” 加了重点，是因为 `git clone` 并用 vsc 打开 [ractive](https://github.com/ractivejs/ractive) 时，发现 UI 样式和日常使用的 vsc 样式不一样，于是找到了 `.vscode/settings.json` 文件，找到了修改 vsc UI 的相关配置。
>
> 💡 `.vscode` 文件夹可以放置 `settings.json` 和 `launch.json` 文件。

<font color=dodgerBlue>**VS Code provides several different scopes for settings**</font>. When you open a workspace, you will see at least the following two scopes:

- **User Settings** - Settings that <font color=red>apply globally</font> to any instance of VS Code you open.
- **Workspace Settings** - <font color=red>Settings **stored inside your workspace**</font> and <font color=red>only apply when the workspace is opened</font>.





#### VS Code 工具

https://github.com/viatsko/awesome-vscode

