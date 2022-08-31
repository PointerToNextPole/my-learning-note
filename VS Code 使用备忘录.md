# VS Code 使用备忘录



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



#### 快捷键

- **⇧ + ⌥ + F** ：代码格式化。不过，如果代码被检测出错误，将不会执行
- **⇧ + ⌘ + L** ：批量重命名
- **⌃ + Space** ：智能建议（只能是短按，长按和 Mac 自带切切换输入法相冲突），光标悬浮也可以产生同样的效果



#### VS Code 工具

https://github.com/viatsko/awesome-vscode

