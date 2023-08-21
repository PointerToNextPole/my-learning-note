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
"editor.stickyScroll.enabled": true
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



#### User and Workspace Settings

You can <font color=dodgerBlue>configure Visual Studio Code to your liking through its various settings</font>. Nearly every part of VS Code's editor, <font color=red>user interface</font>, and functional behavior has options you can modify.

> 👀 之所以 “user interface” 加了重点，是因为 `git clone` 并用 vsc 打开 [ractive](https://github.com/ractivejs/ractive) 时，发现 UI 样式和日常使用的 vsc 样式不一样，于是找到了 `.vscode/settings.json` 文件，找到了修改 vsc UI 的相关配置。
>
> 💡**一些补充**
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





#### VS Code 工具

https://github.com/viatsko/awesome-vscode

