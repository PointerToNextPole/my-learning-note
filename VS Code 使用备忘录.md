# VS Code 使用备忘录


#### 资料

官方文档：https://code.visualstudio.com/docs

有效果 gif 的快捷键教程：https://www.vscheatsheet.com

[以 VSCode 为例，VSCode 团队成员吕鹏带你了解如何参与开源项目](https://www.bilibili.com/video/BV1dw411F7eR) 很有意思的介绍了编写和调试 vsc 的一些技巧



#### 一些设置

##### 进入 `settings.json` 的方法

按下 ⇧ + ⌘ + P ，会显示搜索框；在输入框中输入 `settings.json` ，会显示如下结果：

<img src="https://s2.loli.net/2022/05/18/bd3k7BDFAqoCc92.png" alt="image-20220518215940563" style="zoom:50%;" />

选择第一个即可进入



##### 函数添加 “参数名提示” 功能

打开 settings.json 配置文件（方法见 [[#进入 settings.json 方法]]），添加如下配置；即可应用。

```json
"javascript.inlayHints.parameterNames.enabled": "all",
"typescript.inlayHints.parameterNames.enabled": "all",
```



##### 编辑器的 “粘性滚动” 设置

```json
"editor.stickyScroll.enabled": true
```

样式如下：

<img src="https://s2.loli.net/2022/08/06/XwtKEZvqNJOUIpP.png" alt="image-20220806194800985" style="zoom:45%;" />

另外，发现用来写 markdown 效果出奇的好：

<img src="https://s2.loli.net/2022/09/05/1I2spWkfXBeZ4VH.png" alt="image-20220905005539528" style="zoom:50%;" />

##### 工作台的粘性滚动

```json
"workbench.tree.enableStickyScroll": true,
```

效果如下：

<img src="https://s2.loli.net/2024/01/07/GJFDMsvkpX74bHm.png" alt="image-20240107234054700" style="zoom:50%;" />

##### 设置 gitlens 的日期格式

```json
{
  "gitlens.defaultDateFormat": "YYYY-MM-DD HH:mm:ss"
}
```

学习自：https://x.com/wyh236/status/1930535877035360291

#### 快捷键

这里只说笔者在用的 Mac 版

官方给出的 [Mac 版快捷键 CheatSheet](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf) ，且该文档可搜索

- **⇧ + ⌥ + F** ：代码格式化。注意：如果代码检测有错误，将不会执行格式化

- **fn + F2** ：重命名

  光标聚焦使用 `F2` 会出现如下效果：

  <img src="https://s2.loli.net/2022/09/16/j9Dd7elL3YZg4vc.png" alt="image-20220916203504239" style="zoom:50%;" />

  另外，使用 `F2` 也是 VS Code 文档中指定的重命名的方法，详见 [VS Code - LANGUAGES - JavaScript # rename](https://code.visualstudio.com/Docs/languages/javascript#_rename)

  > 👀 似乎也可以使用 ⇧ + ⌘ + L ，但是不知道为什么这个方法偶尔会失灵... 😳 
  >

- **⌃ + Space** ：智能建议（只能是短按，长按和 Mac 自带切切换输入法相冲突），光标悬浮也可以产生同样的效果

- **⌘ + P** ：搜索文件，并快速跳转。有点类似于 IDEA 中的 “double space”

- **⌘ + K  ➡ ⌘ + S** ：键盘快捷键列表，效果如下：

  <img src="https://s2.loli.net/2022/09/16/7IYL5C1qSB4Xj8f.png" alt="image-20220916205802024" style="zoom:40%;" />

- **⌘ + J** / **⌃ + `** ：打开 VSC 内置的 Terminal

- **⌘ + B** ：展示 / 隐藏 活动栏 ( Activity Bar ) 

  > 👀 **⌘ + B** 有点类似于 WebStorm 的 **⌘ + 1** ，可以很好的让 “活动栏” 隐藏，从而在分屏写代码时显示更多（宽）的内容，让屏幕空间得到很好的利用。
  >
  > 下面也顺带介绍一下 **⌘ + B** 对应的 “活动栏” 中的 “活动”的快捷键

- **⇧ + ⌘ + E** ：打开项目

- **⇧ + ⌘ + F** ：打开全局搜索

- **⇧ + ⌘ + G** ：打开 Git 同步相关

- **⇧ + ⌘ + D** ：打开 debug 相关

- **⇧ + ⌘ + X** ：打开插件相关

- **⇧ + ⌘ + P** ：手动输入与选中，以调整设置 ( **Command Palette** with **Preferences** )

  如下示例是：输入 “activity”，筛选配置，以修改 “活动栏” 显示配置（下面第一个选项）

  <img src="https://s2.loli.net/2023/03/01/hVgvbAmTul3UCco.png" alt="image-20230301014242242" style="zoom:55%;" />
  
- **fn + F12** ：前往定义处

- **⇧ + ⌥ + fn + F12** ：在边栏 ( sidebar ) 中查看所有该变量、函数 被使用（引用 reference ）的地方

  <img src="https://s2.loli.net/2024/03/24/m3pLzEdDy6ocZxR.png" alt="image-20240324152723589" style="zoom:40%;" />

- **⌥ + fn + F12** ：以弹窗形式查看所有该变量、函数被引用的地方。和上面一条、在边栏显示，功能一样

  <img src="https://s2.loli.net/2024/03/24/ti5gw319rlAfJCE.png" alt="image-20240324165233811" style="zoom:50%;" />

- **⇧ + ⌘ + Space** ：对光标处的代码，手动触发 Signature help

- **⇧ + ⌃ + ⌘ + →** / **⇧ + ⌃ + ⌘ + ←** ：智能选择 ( Smart Select )，类似于 JetBrains 家的 **⌥ + ↑** / **⌥ + ↑**
  > zed 也有类似的快捷键 `editor: select larger syntax node` **⇧ + ⌃ + →** / **⇧ + ⌃ + ←**

  > 👀 学习自：https://g.co/gemini/share/e07222bba6a1

- **⌥ + ↑** / **⌥ + ↓** ：选中的代码行，上移和下移
  **⇧ + ⌥ + ↑** / **⇧ + ⌥ + ↓**  ：复制并上移和下移
  > 👀 值得注意的是：JetBrains 家的上移和下移就是 **⇧ + ⌥ + ↑** / **⇧ + ⌥ + ↓** ；只能说：可以用来辅助记忆，但是别记混了


#### `code` 命令

在一次让 codex 帮我分析当前的 100+ 个 vs code 插件中是否存在过时、冗余插件时，codex 使用了 `code` 命令，让我发现 `code` 命令不仅仅能用于指定 vs code 打开文件，还有其他功能；比如：`code --list-extensions --show-versions` 和 `code --uninstall-extension <identifier>`
作用分别是列出当前所安装的插件，和卸载指定到插件（对应安装的是 `--install-extension` ）
###### `tldr code` 结果

<img src="https://files.seeusercontent.com/2026/05/23/7Ayr/image-20260523190423563.png" style="zoom: 50%;" />

#### User and Workspace Settings

You can <font color=dodgerBlue>configure Visual Studio Code to your liking through its various settings</font>. Nearly every part of VS Code's editor, <font color=red>user interface</font>, and functional behavior has options you can modify.

> [!NOTE]
> 
> 之所以 “user interface” 加了重点，是因为 `git clone` 并用 vsc 打开 [ractive](https://github.com/ractivejs/ractive) 时，发现 UI 样式和日常使用的 vsc 样式不一样，于是找到了 `.vscode/settings.json` 文件，找到了修改 vsc UI 的相关配置。

> [!TIP]
>
>  `.vscode` 文件夹可以放置 `settings.json` 和 `launch.json` ，以及 [[#Workspace settings]] 中提及的 task 对应的 `tasks.json` 。
>
> 也可在 `.vscode` 中添加 `extensions.json` ，作为项目 ( workspace ) 中推荐使用的插件；官方文档介绍：[VS Code Doc - Extension Marketplace # Workspace recommended extensions](https://code.visualstudio.com/docs/editor/extension-marketplace#_workspace-recommended-extensions) 。相关讨论：[Cory House 在 23/4/11 的推文](https://twitter.com/housecor/status/1645459301249458177)

<font color=dodgerBlue>**VS Code provides several different scopes for settings**</font>. When you open a workspace, you will see at least the following two scopes:

- **User Settings** - Settings that <font color=red>apply globally</font> to any instance of VS Code you open.
- **Workspace Settings** - <font color=red>Settings **stored inside your workspace**</font> and <font color=red>only apply when the workspace is opened</font>.

##### Settings editor filters

<font color=dodgerBlue>The Settings editor Search bar **has several filters** to make it easier to manage your settings</font>. To the right of the Search bar is a filter button with a funnel icon that provides some options to easily add a filter to the Search bar.

> 👀 如下图，其中 `@modified` 是一个比较重要的选项，下面会细说
>
> <img src="https://s2.loli.net/2023/03/01/afYp63MzOD1urXc.png" alt="image-20230301145738879" style="zoom:50%;" />

###### Modified settings

<font color=dodgerBlue>To **check which settings you have configured**</font>, <font color=red>there is a `@modified` filter in the Search bar</font>. A <font color=red>setting shows up under this filter if its value differs from the default value</font>, or if its value is explicitly set in the respective settings JSON file. <font color=LightSeaGreen>This filter can be useful if you have forgotten whether you configured a setting</font>, or if the editor is not behaving as you expect because you accidentally configured a setting.

<img src="https://s2.loli.net/2023/03/01/oZnmQyF8MDjc9bu.png" alt="image-20230301154545097" style="zoom:45%;" />

###### Other filters

There are several other handy filters to help with searching through settings.

<img src="https://s2.loli.net/2023/03/01/TtPeIwALOvHzXs1.png" alt="image-20230301154901956" style="zoom:45%;" />

<font color=dodgerBlue>**Here are some of the filters available:**</font>

- `@ext` - Settings specific to an extension. You provide the extension ID such as `@ext:ms-python.python`.

- `@feature` - Settings specific（特性） to a **Features** subgroup. For example, <font color=LightSeaGreen>`@feature:explorer` shows settings of the File Explorer</font>.

- `@id` - Find a setting based on the setting ID. For example, `@id:workbench.activityBar.visible`.

- `@lang` - Apply a language filter based on a language ID. For example, `@lang:typescript`. See [Language-specific editor settings](https://code.visualstudio.com/docs/getstarted/settings#_language-specific-editor-settings) for more details.

  > 👀 这里的 lang 是指 编程语言，而不是 UI 展示的自然语言（一开始搞错了...）

- `@tag` - Settings specific to a system of VS Code. For example, `@tag:workspaceTrust` for settings related to [Workspace Trust](https://code.visualstudio.com/docs/editor/workspace-trust), or `@tag:accessibility` for settings related to accessibility.

##### settings.json

The Settings editor is the UI that lets you review and modify setting values that are stored in a `settings.json` file. You can review and edit this file directly by opening it in the editor with the **Preferences: Open Settings (JSON)** command（👀 如下图）. <font color=red>Settings are written as JSON by specifying the setting ID and value</font>.

<img src="https://s2.loli.net/2023/03/01/1DfncWNFOyvPatK.png" alt="image-20230301160339769" style="zoom:50%;" />

<font color=red>Some settings can only be edited in `settings.json`</font> such as **`Workbench: Color Customizations`** and show a **Edit in settings.json** link in the Settings editor.

<img src="https://s2.loli.net/2023/03/01/xHjDRe7Olha1tNm.png" alt="image-20230301160843819" style="zoom:49%;" />

If you prefer to always work directly with `settings.json`, you can set `"workbench.settings.editor": "json"` so that **File** > **Preferences** > **Settings** and the keybinding ⌘, always opens the `settings.json`file and not the Setting editor UI.

###### Settings file locations

Depending on your platform, the user settings file is located here:

- **Windows** `%APPDATA%\Code\User\settings.json`
- **macOS** `$HOME/Library/Application\ Support/Code/User/settings.json`
- **Linux** `$HOME/.config/Code/User/settings.json`

###### Reset all settings

While you can reset settings individually via the Settings editor **Reset Setting** command, you can reset all changed settings by <font color=red>opening `settings.json` and **deleting the entries between the braces `{}`**</font>. Be careful since <font color=LightSeaGreen>there will be no way to recover your previous setting values</font>.

##### Workspace settings

<font color=red>Workspace settings are **specific to a project** and **can be shared across developers on a project**</font>. <font color=red>Workspace settings override user settings</font>.

> **Note**: A VS Code "workspace" is usually just your project root folder. <font color=red>Workspace settings as well as [debugging](https://code.visualstudio.com/docs/editor/debugging)</font> ( 👀 `launch.json` ) <font color=red>and [task](https://code.visualstudio.com/docs/editor/tasks)</font> ( 👀 `tasks.json` ) <font color=red>configurations are **stored at the root in a `.vscode` folder**</font>. You can also have more than one root folder in a VS Code workspace through a feature called [Multi-root workspaces](https://code.visualstudio.com/docs/editor/multi-root-workspaces). You can learn more in the [What is a VS Code "workspace"?](https://code.visualstudio.com/docs/editor/workspaces) article.

You can edit via the Settings editor **Workspace** tab or open that tab directly with the **Preferences: Open Workspace Settings** command.

<img src="https://s2.loli.net/2023/03/01/lQ6JGgv7ZdefmPa.png" alt="image-20230301170924843" style="zoom:48%;" />

All features of the Settings editor such as settings groups, search, and filtering behave the same for Workspace settings. <font color=dodgerBlue>Not all User settings are available as Workspace settings</font>. For example, application-wide settings related to updates and security can not be overridden by Workspace settings.

###### Workspace settings.json location

Similar to User Settings, Workspace Settings are also stored in a `settings.json` file, which you can edit directly via the **Preferences: Open Workspace Settings (JSON)** command.

The workspace settings file is located under the `.vscode` folder in your root folder.

<img src="https://s2.loli.net/2023/03/01/DvhtNgO6a2mzPZW.png" alt="image-20230301173050960" style="zoom: 45%;" />

> **Note:** For a [Multi-root Workspace](https://code.visualstudio.com/docs/editor/multi-root-workspaces#_settings), workspace settings are located inside the workspace configuration file.

##### Language specific editor settings

<font color=dodgerBlue>**One way**</font> to customize language-specific settings is by opening the Settings editor, pressing on the filter button, and selecting the language option to add a language filter. <font color=dodgerBlue>Alternatively</font>, one <font color=red>can directly type a language filter of the form **`@lang:languageId`** into the search widget</font>. The settings that show up will be configurable for that specific language, and will show the setting value specific to that language, if applicable.

When modifying a setting while there is a language filter in place, <font color=LightSeaGreen>the setting will be configured in the given scope for that language</font>. For example, <font color=dodgerBlue>when modifying the user-scope `diffEditor.codeLens` setting while there is a `@lang:css` filter in the search widget</font>, the Settings editor will save the new value to the CSS-specific section of the user settings file.

<img src="https://s2.loli.net/2023/03/01/jgCvFO7ziwXhGpJ.png" alt="settings-css-example" style="zoom:38%;" />

> **Note:** <font color=dodgerBlue>If you enter **more than one language** filter in the search widget</font>, the current behavior is that only the first language filter will be used.

<font color=dodgerBlue>**Another way**</font> to customize your editor by language is by running the global command **`Preferences: Configure Language Specific Settings`** ( command ID: `workbench.action.configureLanguageBasedSettings` ) from the **Command Palette** (⇧⌘P) which opens the language picker. Select the language you want. Then, the Settings editor opens with a language filter for the selected language, which allows you to modify language-specific settings for that language. Though, if you have the `workbench.settings.editor` setting set to `json`, then the `settings.json` file opens with a new language entry where you can add applicable settings.

<img src="https://s2.loli.net/2023/03/01/7yGgf3ZVYMzUSvK.png" alt="image-20230301180224655" style="zoom:48%;" />

Now you can start editing settings specifically for that language:

<img src="https://s2.loli.net/2023/03/08/xYv3VyMpWEnCHGj.png" alt="image-20230308233334111" style="zoom:45%;" />

Or, if `workbench.settings.editor` is set to `json` , now you can start adding language-specific settings to your user settings:

<img src="https://s2.loli.net/2023/03/08/olbanBLGyiQEF6N.png" alt="image-20230308233731085" style="zoom:45%;" />

If you <font color=dodgerBlue>have a file open</font> and you <font color=dodgerBlue>want to customize the editor for this file type</font>, select the Language Mode in the Status Bar to the bottom-right of the VS Code window. This opens the Language Mode picker with an option **Configure 'language_name' language based settings**. Selecting this opens your user `settings.json` with the language entry where you can add applicable settings.

<img src="https://s2.loli.net/2023/03/09/K7SedvfNJkGqWZr.png" alt="image-20230309000715804" style="zoom:50%;" />

You can <font color=lightSeaGreen>use IntelliSense in `settings.json` to help you find language-specific settings</font>. All editor settings and some non-editor settings are supported. Some languages have default language-specific settings already set, which you can review in `defaultSettings.json` by running the **Preferences: Open Default Settings** command.

###### Multiple language-specific editor settings

You <font color=dodgerBlue>can configure language specific editor settings for multiple languages at once</font>. The following example shows how you can customize settings for `javascript` and `typescript` languages together in your `settings.json` file:

```json
"[javascript][typescript]": {
  "editor.maxTokenizationLineLength": 2500
}
```

##### Settings precedence

> 👀 设置优先级

Configurations can be overridden at multiple levels by the different setting scopes. In the following list, <font color=dodgerBlue>**later scopes override earlier scopes**</font>:

- Default settings - This scope represents the default unconfigured setting values.
- User settings - Apply globally to all VS Code instances.
- Remote settings - Apply to a remote machine opened by a user.
- Workspace settings - Apply to the open folder or workspace.
- Workspace Folder settings - Apply to a specific folder of a [multi-root workspace](https://code.visualstudio.com/docs/editor/multi-root-workspaces).
- Language-specific default settings - These are language-specific default values that can be contributed by extensions.
- Language-specific user settings - Same as User settings, but specific to a language.
- Language-specific remote settings - Same as Remote settings, but specific to a language.
- Language-specific workspace settings - Same as Workspace settings, but specific to a language.
- Language-specific workspace folder settings - Same as Workspace Folder settings, but specific to a language.
- Policy settings - Set by the system administrator, <font color=red>these values always override other setting values</font>.

<font color=dodgerBlue>**Setting values can be of various types:**</font>

- String - `"files.autoSave": "afterDelay"`
- Boolean - `"editor.minimap.enabled": true`
- Number - `"files.autoSaveDelay": 1000`
- Array - `"editor.rulers": []`
- Object - `"search.exclude": { "**/node_modules": true, "**/bower_components": true }`

<font color=red>Values with primitive types and Array types are overridden</font>, meaning a configured value in a scope that takes precedence over another scope is used instead of the value in the other scope. But, <font color=fuchsia>**values with Object types are merged**</font>.

> 👀 准确的说：对象类型的值出现重复：配置出现冲突，高优先级的配置替换优先级低低；没有冲突的，则合并为一个新的对象。
>
> 原文这有示例与讲解，这里略。

###### Note about multiple language specific settings

If you are using multiple language-specific settings, <font color=dodgerBlue>be aware that</font> language-specific settings are merged and <font color=fuchsia>precedence is set based on the full language string</font> (for example `"[typescript][javascript]"`) and <font color=fuchsia>not the individual language IDs</font> (`typescript` and `javascript`). This means that for example, a <font color=red>**`"[typescript][javascript]"` workspace setting will not override a `"[javascript]"` user setting**</font>.



### VS Code 工具

https://github.com/viatsko/awesome-vscode



### VS Code 中 GitHub Copilot 使用

[Github Copilot 备忘清单 & github-copilot cheatsheet & Quick Reference](https://wangchujiang.com/reference/docs/github-copilot.html)

##### 快捷键

- **⌃ + ⌘ + I** ：调出 Copilot Chat 页面

- **⇧ + ⌘ + I** ：调出编辑器顶部的快捷 Copilot Chat 弹窗

  <img src="https://s2.loli.net/2024/03/20/G9jgOWypDI5NVZa.png" alt="image-20240320101701473" style="zoom:50%;" />

  > If you want to ask Copilot a quick question and don't want to start a full Chat view session or open inline chat in your editor, you can use the Quick Chat dropdown. To open Quick Chat, you can run **Chat: Open Quick Chat** in the Command Palette, or <font color=red>use the ⇧⌘I keyboard shortcut</font>.
  >
  > 摘自：[Quick Chat](https://code.visualstudio.com/docs/copilot/overview#_quick-chat)

- **⌘ + I** ：在文件中，调出行内 Copilot Chat 弹窗

  <img src="https://s2.loli.net/2024/03/20/2TD7p9zJ3GWgaiw.png" alt="image-20240320103036665" style="zoom: 45%;" />

  > In any file, you can press ⌘I on your keyboard to bring up Copilot inline chat.
  >
  > If you have code selected in the editor, Copilot scopes your question to the selection.
  >
  > 摘自：[vs code doc - copilot # inline chat](https://code.visualstudio.com/docs/copilot/overview#_inline-chat)

##### Copilot Chat 页面中的指令

To make your interactions more efficient, you can use *slash commands* as a shorthand for common questions. For example, use the `/help` to get help about GitHub Copilot. Have you started working on a new code base, then try <font color=fuchsia>entering `/explain` to let Copilot help you understand the code</font>.

> 💡 补充
>
> <img src="https://s2.loli.net/2024/03/20/X8VpIWtnRPyuHTc.png" alt="image-20240320095705061" style="zoom:50%;" />



##### Chat smart actions

To make it easier to use Copilot Chat features, <font color=dodgerBlue>there is a Copilot menu group **in the editor context menu**</font>. <font color=red>**Right-click in the editor** and navigate to Copilot to see the available options</font>:

<img src="https://s2.loli.net/2025/01/23/7ElI1ei4nGkRqzQ.png" alt="image-20240320104704108" style="zoom:45%;" />

You can apply these smart actions on the current file or a selection in the file. Choosing an action brings up the Chat view or inline chat, depending on the action. For example, <font color=red>selecting **Generate Docs** for a function opens the inline chat</font> with a proposed documentation comment:

<img src="https://s2.loli.net/2024/03/20/dLzsrZWK51wftgb.png" alt="Inline chat /doc results adding JSDoc comment for a TypeScript function" style="zoom:45%;" />

##### Improving your developer experience

In addition to inline completions and chat, GitHub Copilot can help with other development tasks and workflows. For example, Copilot can help with writing commit messages, fixing errors, and finding commands.

When Copilot can help with a task or workflow, VS Code displays a **sparkle** icon. Hovering over the sparkle icon describes the Copilot action.

<img src="https://s2.loli.net/2024/03/20/ZX3QVxr2EaUsqjd.png" alt="Sparkle icon in an input box" style="zoom:60%;" />

###### Generate Git commit messages

Copilot can help you write GitHub commit messages. In the Source Control message input box, select the sparkle button at the right and Copilot creates a commit message based on your pending changes.

<img src="https://s2.loli.net/2024/03/20/Ys7k1nKEZcNUFbi.png" alt="Hover over Source Control input box sparkle buttons shows Generate Commit Message" style="zoom:48%;" />

If you're using the [GitHub Pull Request and Issues](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) extension, there is a sparkle button to fill in both the title and description in the Pull Request **Create** view.

###### Terminal Quick Fixes

When a command fails to run in the terminal, Copilot displays a sparkle in the gutter that offers a Quick Fix to explain what happened.

<img src="https://s2.loli.net/2024/03/20/5FSejkAnWvwU8f6.png" alt="Terminal command failure shows sparkle with Explain using Copilot Quick Fix" style="zoom:45%;" />

Selecting **Explain using Copilot** populates Quick Chat with the `@terminal #terminalLastCommand`agent and variable to help correct the last terminal command error.

<img src="https://s2.loli.net/2024/03/20/ihoYFIJvx4zjSnG.png" alt="Quick Chat with @terminal #terminalLastCommand and Copilot's answer" style="zoom:48%;" />

###### Command Palette help

When you're looking for a command in the Command Palette (⇧⌘P), you can run **Ask GitHub Copilot**with your search term to help you find the relevant command.

<img src="https://s2.loli.net/2024/03/20/5xtHekQYLCJ1zTj.png" alt="Command Palette with Ask GitHub Copilot selected to search for &quot;hide editor overview&quot;" style="zoom:50%;" />

The **Ask GitHub Copilot** command opens the Chat view and input your search term.

<img src="https://s2.loli.net/2024/03/20/JCSaqD3Q4wnhurH.png" alt="Chat view with answer to &quot;hide editor overview&quot;" style="zoom:50%;" />

摘自：[VS Code doc - GitHub Copilot in VS Code](https://code.visualstudio.com/docs/copilot/overview#_inline-chat)



## VS Code 语言配置



### JavaScript



#### JavaScript in Visual Studio Code

##### JavaScript projects (jsconfig.json)

A [jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig) file defines a JavaScript project in VS Code. <font color=lightSeaGreen>While `jsconfig.json` files are not required</font>, <font color=dodgerBlue>you will want to create one in cases such as</font>:

- <font color=red>If not all JavaScript files in your workspace should be considered part of a single JavaScript project</font>. `jsconfig.json` files let you exclude some files from showing up in IntelliSense.

  <img src="https://s2.loli.net/2024/03/24/V6L9MBTlyedJvKc.png" alt="image-20240324155030326" style="zoom:50%;" />

- <font color=red>To ensure that a **subset of JavaScript files in your workspace** is treated as a single project</font>. This is useful if you are working with legacy code that uses implicit globals dependencies instead of `imports` for dependencies.

- <font color=dodgerBlue>If your workspace contains more than one project context</font>, <font color=lightSeaGreen>such as front-end and back-end JavaScript code</font>. For multi-project workspaces, <font color=red>create a `jsconfig.json` at the root folder of each project</font>.

- You are using the TypeScript compiler to down-level compile JavaScript source code.

To define a basic JavaScript project, <font color=red>add a `jsconfig.json` at the root of your **workspace**</font>:

> ⚠️ 注意这里的用词，是 “root of your workspace”

```json
{
  "compilerOptions": {
    "module": "CommonJS",
    "target": "ES6"
  },
  "exclude": ["node_modules"]
}
```



#### Working with JavaScript

This topic describes some of the advanced JavaScript features supported by Visual Studio Code. <font color=dodgerBlue>Using the **TypeScript language service**</font>, <font color=red>VS Code can **provide smart completions (IntelliSense) as well as type checking for JavaScript**</font>.

##### IntelliSense

Visual Studio Code's JavaScript [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) <font color=red>provides intelligent code completion, parameter info, references search, and many other advanced language features</font>. <font color=lightSeaGreen>Our JavaScript IntelliSense is powered by the [JavaScript language service](https://github.com/microsoft/TypeScript/wiki/JavaScript-Language-Service-in-Visual-Studio) **developed by the TypeScript team**</font>. While IntelliSense should just work for most JavaScript projects without any configuration, <font color=lightSeaGreen>you can make IntelliSense even more useful with [JSDoc](https://code.visualstudio.com/docs/languages/javascript#_jsdoc-support) or by configuring a `jsconfig.json` project</font>.

For the details of how JavaScript IntelliSense works, including being based on type inference, JSDoc annotations, TypeScript declarations, and mixing JavaScript and TypeScript projects, see the [JavaScript language service documentation](https://github.com/microsoft/TypeScript/wiki/JavaScript-Language-Service-in-Visual-Studio).

When type inference does not provide the desired information, type information may be provided explicitly with JSDoc annotations. This document describes the [JSDoc annotations](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html#supported-jsdoc) currently supported.

In addition to objects, methods, and properties, the JavaScript IntelliSense window also provides basic word completion for the symbols in your file.

##### JavaScript projects (jsconfig.json)

> 👀 这里不少的内容，和 [[#jsconfig.json]] 中的内容有所重复，所以，这里只摘抄不重复且值得摘抄的部分

For common setups, a `jsconfig.json` file is not required, however, <font color=dodgerBlue>there are situations when you will want to add a `jsconfig.json`</font>.

- Not all files should be in your JavaScript project (for example, you want to exclude some files from showing IntelliSense). <font color=lightSeaGreen>This situation is common with front-end and back-end code</font>.
- <font color=lightSeaGreen>**Your workspace contains more than one project context**</font>. In this situation, you should add a `jsconfig.json` file at the root folder for each project.
- <font color=red>You are using the TypeScript compiler to down-level compile JavaScript source code</font>.

###### Migrating to TypeScript

<font color=lightSeaGreen>It is possible to have mixed TypeScript and JavaScript projects</font>. <font color=dodgerBlue>To start migrating to TypeScript</font>, <font color=red>rename your `jsconfig.json` file to `tsconfig.json`</font> and <font color=red>set the `allowJs` property to `true`</font>. For more information, see [Migrating from JavaScript](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html) .

##### Type checking JavaScript

<font color=red>VS Code allows you to leverage some of TypeScript's advanced type checking and error reporting functionality in regular JavaScript files</font>. This is a great way to catch common programming mistakes. These type checks also enable some exciting Quick Fixes for JavaScript, including **Add missing import** and **Add missing property**.

<img src="https://code.visualstudio.com/assets/docs/nodejs/working-with-javascript/checkjs-example.gif" alt="Using type checking and Quick Fixes in a JavaScript file" style="zoom:85%;" />

TypeScript can infer types in `.js` files same as in `.ts` files. When types cannot be inferred, they can be specified using JSDoc comments. You can read more about how TypeScript uses JSDoc for JavaScript type checking in [Type Checking JavaScript Files](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html) .

Type checking of JavaScript is optional and opt-in. Existing JavaScript validation tools such as ESLint can be used alongside the new built-in type checking functionality.

<font color=dodgerBlue>You can get started with type checking a few different ways depending on your needs</font>.

###### Per file

<font color=red>The easiest way</font> to enable type checking in a JavaScript file is by adding `// @ts-check` to the top of a file.

```ts
// @ts-check
let itsAsEasyAs = 'abc';
itsAsEasyAs = 123; // Error: Type '123' is not assignable to type 'string'
```

Using `// @ts-check` is a good approach <font color=red>if you just want to try **type checking in a few files** but not yet enable it for an entire codebase</font>.

###### Using a setting

To enable type checking for all JavaScript files without changing any code, just add `"js/ts.implicitProjectConfig.checkJs": true` to your workspace or user settings. This enables type checking for any JavaScript file that is not part of a `jsconfig.json` or `tsconfig.json` project.

<font color=red>You can opt individual files out of type checking with a `// @ts-nocheck` comment at the top of the file</font>:

> 👀 相当于加了一个逃生舱

```ts
// @ts-nocheck
let easy = 'abc';
easy = 123; // no error
```

You can <font color=dodgerBlue>also</font> disable individual errors in a JavaScript file using a `// @ts-ignore` comment on the line before the error:

```
let easy = 'abc';
// @ts-ignore
easy = 123; // no error
```

###### Using jsconfig or tsconfig

To enable type checking for JavaScript files that are part of a `jsconfig.json` or `tsconfig.json`, <font color=red>add `"checkJs": true` to the project's compiler options</font>:

`jsconfig.json`:

```json
{
  "compilerOptions": {
    "checkJs": true
  },
  "exclude": ["node_modules", "**/node_modules/*"]
}
```

`tsconfig.json`:

```
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true
  },
  "exclude": ["node_modules", "**/node_modules/*"]
}
```

This enables type checking for all JavaScript files in the project. You can use `// @ts-nocheck` to disable type checking per file.

<font color=lightSeaGreen>JavaScript type checking requires TypeScript 2.3</font>. If you are unsure what version of TypeScript is currently active in your workspace, run the **TypeScript: Select TypeScript Version** command to check. You must have a `.js/.ts` file open in the editor to run this command. If you open a TypeScript file, the version appears in the lower right corner.

##### Global variables and type checking

Let's say that you are working in legacy JavaScript code that uses global variables or non-standard DOM APIs:

```js
window.onload = function() {
  if (window.webkitNotifications.requestPermission() === CAN_NOTIFY) {
    window.webkitNotifications.createNotification(null, 'Woof!', '🐶').show();
  } else {
    alert('Could not notify');
  }
};
```

<font color=dodgerBlue>If you try to use `// @ts-check` with the above code</font>, you'll <font color=dodgerBlue>see a number of errors about the use of global variables</font>:

1. `Line 2` - `Property 'webkitNotifications' does not exist on type 'Window'.`
2. `Line 2` - `Cannot find name 'CAN_NOTIFY'.`
3. `Line 3` - `Property 'webkitNotifications' does not exist on type 'Window'.`

<font color=dodgerBlue>If you want to continue using `// @ts-check` but are confident that these are not actual issues with your application</font>, <font color=red>you have to **let TypeScript know about these global variables**</font>.

To start, [create a `jsconfig.json`](https://code.visualstudio.com/docs/nodejs/working-with-javascript#_javascript-projects-jsconfigjson) at the root of your project:

```
{
  "compilerOptions": {},
  "exclude": ["node_modules", "**/node_modules/*"]
}
```

Then reload VS Code to make sure the change is applied. The presence of a `jsconfig.json` lets TypeScript know that your Javascript files are part of a larger project.

<font color=dodgerBlue>Now create a `globals.d.ts` file somewhere your workspace:</font>

```ts
interface Window {
  webkitNotifications: any;
}

declare var CAN_NOTIFY: number;
```

`d.ts` files are type declarations. In this case, <font color=red>`globals.d.ts` lets TypeScript know that a global `CAN_NOTIFY` exists and that a `webkitNotifications` property exists on `window`</font>. You can read more about writing `d.ts` in the [TypeScript documentation](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html). `d.ts` files do not change how JavaScript is evaluated, they are used only for providing better JavaScript language support.

##### Using tasks

###### Using the TypeScript compiler

<font color=lightSeaGreen>One of the key features of TypeScript is the ability to use the latest JavaScript language features, and emit code that can execute in JavaScript runtimes that don't yet understand those newer features</font>. With JavaScript using the same language service, it too can now take advantage of this same feature.

The TypeScript compiler `tsc` can down-level compile JavaScript files from ES6 to another language level. <font color=red>Configure the `jsconfig.json` with the desired options and then use the `–p` argument to make `tsc` use your `jsconfig.json` file</font>, for example `tsc -p jsconfig.json` to down-level compile.

Read more about the compiler options for down level compilation in the [jsconfig documentation](https://code.visualstudio.com/docs/languages/jsconfig#_jsconfig-options).

###### Running Babel

The Babel transpiler turns ES6 files into readable ES5 JavaScript with Source Maps. <font color=red>You can easily integrate **Babel** into your workflow by adding the configuration below to your `tasks.json` file</font> (<font color=lightSeaGreen>located under the workspace's `.vscode`folder</font>). The `group` setting makes this task the default **Task: Run Build Task** gesture. `isBackground` tells VS Code to keep running this task in the background. To learn more, go to [Tasks](https://code.visualstudio.com/docs/editor/tasks).

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "watch",
      "command": "${workspaceFolder}/node_modules/.bin/babel",
      "args": ["src", "--out-dir", "lib", "-w", "--source-maps"],
      "type": "shell",
      "group": { "kind": "build", "isDefault": true },
      "isBackground": true
    }
  ]
}
```

Once you have added this, you can start **Babel** with the ⇧⌘B (**Run Build Task**) command and it will compile all files from the `src` directory into the `lib` directory.

> **Tip:** For help with Babel CLI, see the instructions in [Using Babel](https://babeljs.io/docs/setup/#installation). The example above uses the CLI option.

##### Disable JavaScript support

If you prefer to use JavaScript language features supported by other JavaScript language tools such as [Flow](https://flow.org/), <font color=dodgerBlue>you can disable VS Code's built-in JavaScript support</font>. <font color=red>You do this by disabling the built-in TypeScript language extension **TypeScript and JavaScript Language Features** (vscode.typescript-language-features) which also provides the JavaScript language support</font>.

To disable JavaScript/TypeScript support, go to the Extensions view (⇧⌘X) and filter on built-in extensions (**Show Built-in Extensions** in the **...** **More Actions** dropdown), then type 'typescript'. Select the **TypeScript and JavaScript Language Features** extension and press the **Disable** button. VS Code built-in extensions cannot be uninstalled, only disabled, and can be re-enabled at any time.

> 👀 注意下图的筛选条件 `@builtin typescript`

![TypeScript and JavaScript Language Features extension](https://s2.loli.net/2024/03/17/BbH1x3NIWdPfye2.png)



#### jsconfig.json

##### What is jsconfig.json?

<font color=lightSeaGreen>The presence of `jsconfig.json` file in a directory indicates that the directory is the root of a JavaScript Project</font>. The `jsconfig.json` file <font color=fuchsia>specifies the root files and **the options for the features provided by the [JavaScript language service](https://github.com/microsoft/TypeScript/wiki/JavaScript-Language-Service-in-Visual-Studio)**</font> （⭐️ 这里说明了 `jsconfig.json` 配置的意义，即配置 JS LSP service 的行为）.

> **Tip:** <font color=red>`jsconfig.json` **is a descendant of [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)**</font> , which is a configuration file for TypeScript. <font color=fuchsia>`jsconfig.json` is `tsconfig.json` with `"allowJs"` attribute set to `true`</font>.

> 💡 上面说了 `jsconfig.json` 和 `tsconfig.json` 的关系，下面是补充：
>
> ![image-20240317154454259](https://s2.loli.net/2024/03/17/JizYKmnvua9hIrO.png)

##### Why do I need a jsconfig.json file?

Visual Studio Code's JavaScript support can <font color=dodgerBlue>run in two different modes</font>:

- **File Scope - no jsconfig.json**: <font color=dodgerBlue>In this mode</font>, <font color=red>JavaScript files opened in Visual Studio Code are treated as independent units</font>. As long as a file `a.js` doesn't reference a file `b.ts` explicitly (either using `import` or **CommonJS** [modules](https://wiki.commonjs.org/wiki/Modules/1.0)), there is no common project context between the two files.
- **Explicit Project - with jsconfig.json**: A JavaScript project is defined via a `jsconfig.json` file. The presence of such a file in a directory indicates that the directory is the root of a JavaScript project. The file itself can optionally list the files belonging to the project, the files to be excluded from the project, as well as compiler options (see below).

<font color=red>The JavaScript experience is improved</font> <font color=lightSeaGreen>when you have a `jsconfig.json` file in your workspace</font> that defines the project context. For this reason, we offer a hint to create a `jsconfig.json` file when you open a JavaScript file in a fresh workspace.

###### Location of jsconfig.json

We define this part of our code, the client side of our website, as a JavaScript project by creating a `jsconfig.json` file. <font color=lightSeaGreen>Place the file at the root of your JavaScript code as shown below</font>.

![jsconfig setup](https://s2.loli.net/2024/03/17/4sRK2MZFbScQGDX.png)

<font color=dodgerBlue>In more complex projects</font>, you may have more than one `jsconfig.json` file defined inside a workspace. You will want to <font color=dodgerBlue>do this so that the code in one project is not suggested</font> as IntelliSense to code in another project. Illustrated below is a project with a `client` and `server` folder, showing two separate JavaScript projects.

> 👀 上面提了一下 IntelliSense ，可以看下 [[#Working with JavaScript#IntelliSense]] ，里面有说 “<font color=lightSeaGreen>you can make IntelliSense even more useful with JSDoc or **by configuring a `jsconfig.json`** project</font>”

![multiple jsconfigs](https://s2.loli.net/2024/03/17/dFJDpQAg2nLhM97.png)

##### jsconfig Options

Below are `jsconfig` `"compilerOptions"` to configure the JavaScript language support.

> **Tip:** <font color=lightSeaGreen>Do not be confused by `compilerOptions`</font> , since <font color=red>no actual compilation is required for JavaScript</font>. <font color=lightSeaGreen>This attribute **exists because `jsconfig.json` is a descendant of `tsconfig.json`** , which is used for compiling TypeScript</font>.

| Option                         | Description                                                  |
| :----------------------------- | :----------------------------------------------------------- |
| `noLib`                        | Do not include the default library file (lib.d.ts)           |
| `target`                       | Specifies which default library (lib.d.ts) to use. The values are "ES3", "ES5", "ES6", "ES2015", "ES2016", "ES2017", "ES2018", "ES2019", "ES2020", "ES2021", "ES2022", "ES2023", "ESNext". |
| `module`                       | Specifies the module system, when generating module code. The values are "AMD", "CommonJS", "ES2015", "ES2020", "ES2022", "ES6", "Node16", "NodeNext", "ESNext", "None", "System", "UMD". |
| `moduleResolution`             | Specifies how modules are resolved for imports. The values are "Node", "Classic", "Node16", "NodeNext", "Bundler". |
| `checkJs`                      | Enable type checking on JavaScript files.                    |
| `experimentalDecorators`       | Enables experimental support for proposed ES decorators.     |
| `allowSyntheticDefaultImports` | Allow default imports from modules with no default export. This does not affect code emit, just type checking. |
| `baseUrl`                      | Base directory to resolve non-relative module names.         |
| `paths`                        | Specify path mapping to be computed relative to baseUrl option. |

You can read more about the available `compilerOptions` in the [TypeScript compilerOptions documentation](https://www.typescriptlang.org/tsconfig#compilerOptions).

##### Using webpack aliases

For IntelliSense to work with webpack aliases, you need to specify the `paths` keys with a [glob pattern](https://code.visualstudio.com/docs/editor/glob-patterns).

<font color=dodgerBlue>For example, for alias 'ClientApp':</font>

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "ClientApp/*": ["./ClientApp/*"]
    }
  }
}
```

and then to use the alias

```js
import Something from 'ClientApp/foo';
```

##### Best Practices

<font color=dodgerBlue>**Whenever possible**</font>, <font color=red>you should **exclude folders with JavaScript files that are not part of the source code for your project**</font>.

> **Tip:** <font color=dodgerBlue>If you do **not** have a `jsconfig.json` in your workspace</font>, <font color=red>VS Code will by default exclude the `node_modules` folder</font>.

<font color=dodgerBlue>Below is a table</font>, mapping common project components to their installation folders that <font color=dodgerBlue>are recommended to exclude</font>:

| Component                       | folder to exclude                               |
| :------------------------------ | :---------------------------------------------- |
| `node`                          | exclude the `node_modules` folder               |
| `webpack`, `webpack-dev-server` | exclude the content folder, for example `dist`. |
| `bower`                         | exclude the `bower_components` folder           |
| `ember`                         | exclude the `tmp` and `temp` folders            |
| `jspm`                          | exclude the `jspm_packages` folder              |

When your JavaScript project is growing too large and performance slows, it is often because of library folders like `node_modules`. <font color=lightSeaGreen>If VS Code detects that your project is growing too large, it will prompt you to edit the `exclude` list</font>.

摘自：[VS Code doc - languages - jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig)
