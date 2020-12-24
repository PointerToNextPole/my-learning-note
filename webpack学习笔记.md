# webpack学习笔记



#### webpack存在的必要：

webpack是一种构建工具工具。那，为什么需要构建或者说编译呢？因为像es6、less及sass、模板语法、vue指令及jsx在浏览器中是无法直接执行的，必须经过构建这一个操作才能保证项目运行，所以前端构建打包很重要。除了这些，前端构建还能解决一些web应用性能问题，比如：依赖打包、资源嵌入、文件压缩及hash指纹等。具体的我不再展开，总之前端构建工程化已经是趋势。



- **webpack解析ES6**

  需要掌握一个新的概念：loaders，所谓loaders就是说把原本webpack不支持加载的文件或者文件内容通过loaders进行加载解析，实现应用的目的。这里讲解es6解析，原生支持js解析，但是不能解析es6，需要babel-loader ，而babel-loader 又依赖babel。

- **webpack加载css、less等样式文件**

  css-loader用于加载css文件并生成commonjs对象，style-loader用于将样式通过style标签插入到head中

- **webpack加载图片**

  图片在webpack中的打包使用file-loader或url-loader

摘自：[一小时的时间，上手 Webpack](https://zhuanlan.zhihu.com/p/114286243)