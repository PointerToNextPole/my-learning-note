# webpack学习笔记



#### webpack存在的必要：

webpack是一种构建工具工具。那，为什么需要构建或者说编译呢？因为像es6、less及sass、模板语法、vue指令及jsx在浏览器中是无法直接执行的，必须经过构建这一个操作才能保证项目运行，所以前端构建打包很重要。除了这些，前端构建还能解决一些web应用性能问题，比如：依赖打包、资源嵌入、文件压缩及hash指纹等。具体的我不再展开，总之前端构建工程化已经是趋势。



- **webpack解析ES6**

  需要掌握一个新的概念：loaders，所谓loaders就是说把原本webpack不支持加载的文件或者文件内容通过loaders进行加载解析，实现应用的目的。<mark>这里讲解es6解析，原生支持js解析，但是不能解析es6，需要babel-loader ，而babel-loader 又依赖babel</mark>。

- **webpack加载css、less等样式文件**

  css-loader用于加载css文件并生成commonjs对象，style-loader用于将样式通过style标签插入到head中

- **webpack加载图片**

  图片在webpack中的打包使用file-loader或url-loader

摘自：[一小时的时间，上手 Webpack](https://zhuanlan.zhihu.com/p/114286243)



> ### 学习慕课网课程《从基础到实战 手把手带你掌握新版Webpack4.0》做的笔记

**`npx webpack entryFile`**命令的作用是：使用webpack工具去解析一个项目，这时候项目的文件夹下会产生一个**`dist`**目录，在该目录下会产生一个**`main.js`**的文件（main.js是webpack默认配置定义的打包名称，关于webpack配置见下面）。



webpack本质上是一个模块打包工具



<font color=FF0000>**CommonJS是一种模块引入规范**</font>，其中引入和导出的语法如下

```js
//引入
var module = require('modulePath')

//导出
module.exports = module
```



webpack是基于Node开发的模块打包工具，所以本质上是由Node实现的。同时，由于更高版本的webpack，会使用高版本Node的一些特性；可以大大提高webpack打包的速率。

通常不建议使用全局安装包（**`npm install -g package`**），比如webpack；因为这样对于依赖不同版本包的不同项目，会造成依赖冲突，所以推荐使用在项目中通过**`-D`**形式安装。以webpack为例，如果没有全局安装webpack，这时使用**`webpack -v`**命令是没有结果的，这时可以使用**`npx webpack -v`**



webpack的（默认）配置文件的名称为：webpack.config.js。即：即使用户不自定义配置文件，webpack也会使用默认的配置文件



#### webpack.config.js配置文件的格式

webpack.config.js使用CommonJS的语法，语法格式如下：

```js
const path = require('path')

module.exports = {
  entry: './src/index.js', //项目打包，开始打包的文件入口，实际上相当于entry: {main: './src/index.js'},的简写
  output: {
    filename: 'bundle.js', //项目打包输出文件名称，默认为main.js,
    //项目打包输出文件夹路径，即打包输出的文件在path/filename下。__dirname的意思是webpack.config.js所在了文件夹路径。这行代码			的意思是输出文件为__dirname/'bundle'/bundle.js
    path: path.resolve(__dirname, 'bundle'),
  }
  //打包的模式，打包出的文件将会被压缩（变成min格式），除了production外，还可以选择development选项，这时代码不会被压缩
  mode: 'production',
}
```



**`npx webpack --config myConfig.js`：**webpack通过myConfig.js（自定义的）配置文件打包，而不是默认使用`webpack.config.js`



如果习惯于Vue或React的npm run命令，可以在package.json下的scripts项设置

```json
//package.json文件夹下
"scripts": {
  "bundle": "webpack"
}
```

它的意思是：当你执行bundle（自定义的，即也可以换成start）命令，它实际执行的是webpack打包。这时输入**`npm run bundle`**一样可以起到**`npmx webpack`**打包的效果。这里涉及到的是：npm scripts

//TODO 去读 https://webpack.js.org/guides/getting-started/



#### Loader

webpack默认只会对js文件打包，如果想要对其他类型的文件打包，需要在**`webpack.config.js`**文件中进行设置：

以打包jpg图片为例：

```js
module: {
  rules: [{
    test: /\.jpg$/, //jpg文件格式
    use: {
  		loader: 'file-loader'  //file-loader需要npm install安装
  	}
  }]
}
```

<mark>同样，如果需要打包vue格式的文件，使用loader就需要使用vue-loader</mark>

**设置打包后图片的名字，**在webpack.config.js文件中设置：

```js
module: {
  rules: [{
    test: /\.(jpg|png|gif)$/, //jpg文件格式
    use: {
  		loader: 'file-loader',  //file-loader需要npm install安装
      options: {
      	//打包生成后的名字和之前的一样，这里的ext表示后缀。更多设置见：https://webpack.docschina.org/loaders/file-loader/#placeholders
      	name: '[name].[ext]', 
      	outputPath: 'images/', //把图片打包到images文件夹下
    	}
  	}
  }]
}
```

也可以使用**url-loader**，可以实现file-loader一样的功能；不过，不会将图片打包到dist文件夹下；url-loader会把图片转变成Base64的文字，写入到打包出的js文件中<mark>（类似于C++中的内联）</mark>。不过这样会带来问题：如果图片很大，该js文件将会因此很大，请求js文件的时间将会很长，于是页面将会很长时间内没有反应。于是需要进行一个新的设置**`limit`**：

```js
    use: {
  		loader: 'url-loader',
      options: {
      	name: '[name].[ext]', 
      	outputPath: 'images/',
        limit: 2048, //这里的单位是字节，如果文件超过2048字节，那么该文件将不会内联到js文件中
    	}
  	}
```

//TODO 去看[file-loader](https://webpack.js.org/loaders/file-loader/) 和同页面的 url-loader



#### 对于样式文件（CSS）的打包

```js
module: {
  rules: {
    test: /\.(css|scss)$/,
    use: ['style-loader', 'css-loader', 'sass-loader', 'postcss-loader']
  }
}
```

- **css-loader：**会分析所有CSS文件的关系，最后将这些CSS文件合并为一个CSS文件
  - **sass-loader：**如果需要打包到文件中包含scss / sass，则需要使用sass-loader
- **style-loader：**在得到css-loader输出的内容后，会把生成的css文件挂载到页面的header部分
- **postcss-loader：**见下面。

##### <font color=FF0000>**在webpack中，loader的使用是有先后顺序的：从下到上，从右到左。**</font>



如果想要针对不同的浏览器进行适配（需要写前缀，如**`webkit-`**），可以使用**`postcss-loader`**，

想要对postcss-loader进行配置，需要添加并配置postcss.config.js配置文件，可以使用autoprefixer插件

```js
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}
```

如果想要对css-loader增加配置项，则需要将字符串'css-loader'改成对象形式：

```js
use: [
  'style-loader', 
  {
    loader: 'css-loader',
    options: {
      //由于loader的使用是有先后顺序的：从下到上，从右到左；且如果SCSS文件中出现嵌套引用，由于已经执行到了css-loader部分，将无法				再使用sass-loader，这时候就需要使用importLoader，使之再调用一遍
      importLoaders: 2,
      modules: true, //使用模块化的CSS
    }
  }, 
  'sass-loader',
	'postcss-loader'
]
```

如果使用模块化CSS，那么在文件中导入CSS模块时，需要将**`import 'cssPath'`**，改为**`import style from 'cssPath'`**。如果想要使用模块CSS中的样式，要使用**`style.foo`**



**webpack打包字体**（eot / ttf / svg 格式），可以使用file-loader

```js
{
  test: /\.(eot|ttf|svg)/,
  use: {
    loader: 'url-loader',
    options: {
      loader: 'file-loader',
    }
  }
}
```



//TODO 去看webpack官方文档 https://webpack.js.org/guides/asset-management/  [css-loader](https://webpack.js.org/loaders/css-loader/)  以及同页面的 scss-loader style-loader postcss-loader



#### 使用plugins插件

**使用plugin的作用：**plugin可以在webpack运行到某个时刻的时候，帮你做一些事情



每次打包项目时，入口的html文件（index.js）都要<font color=FF0000>手动生成</font>，很麻烦；这时就需要**`htmlWebpackPlugin`**。代码示例如下：

```js
var HtmlWebpackPlugin = require('html-webpack-plugin');
var path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  plugins: [new HtmlWebpackPlugin({
    template: 'src/index.html', //这里可以配置一个模版，自动生成的index.html会根据这个模版来生成
  })]
};
```

**htmlWebpackPlugin的作用：** htmlWebpackPlugin会在打包结束后，自动生成一个html文件， 并把打包生成的js自动引入到这个html文件中。该插件在打包后执行



在打包之后生成了打包后输出的js文件，如果这时修改了配置文件（webpack.config.js）中的打包输出js的文件名称，再进行打包，这时会出现两个打包输出的js文件（分别是配置修改前生成的js文件，和配置修改后生成的js文件）。可以使用 **`clean-webpack-plugin`** 在生成新的js文件时，清除掉旧的js文件。代码示例如下：

```js
plugins: [
  new CleanWebpackPlugin(['dist']), //指定作用的文件夹
]
```

补充：该插件在打包前执行



#### entry和output的配置

如果想要对一个项目<font color=FF0000>同时</font>（在一次打包）生成两个js文件，可以进行如下配置

```js
module.exports = {
  entry: {
   main: './src/index.js',
   sub: './src/index.js',
  },
  output: {
    filename: '[name].js', //这里如果只制定一个的话会报错，所以可以使用这种方式 
    path: path.resolve(__dirname, 'bundle') 
  }
}
```

如果想要在打包输出文件前添加一个前缀（比如一个网址路径），可以在output中进行如下配置

```js
output: {
  publicPath: 'http://cdn.com.cn',
}
```

这时在所有的输出文件会变成：**`http://cdn.com.cn/bundle.js`**



//TODO 阅读output相关：https://webpack.js.org/configuration/output/  https://webpack.js.org/guides/output-management/  和HtmlWebpackPlugin相关：https://webpack.js.org/plugins/html-webpack-plugin/



#### SourceMap配置

sourceMap它是一个<font color=FF0000>映射关系</font>，他知道dist目录（打包后的输出）下main. js文件的代码行，与对应的是src目录（未打包的源代码）下代码中代码行的对应关系；便于在打包代码出现错误时候，可以指向源代码的<font color=FF0000>某一行某一列（即精确到字符）</font>出现的问题，<font color=FF0000>所以使用sourceMap打包速度会变慢（尤其是大型项目）</font>。

如何启用sourceMap？只需要使用**`devtool: source-map`**即可。同时，启用sourceMap后，在dist文件夹下会出现一个`main.js.map`的映射对应关系文件，这个文件实际上是一个<mark>VLQ的编码集合</mark>

**`devtool: 'inline-source-map'`**：main.js.map文件将不会存在在dist文件夹中，不过main.js.map的内容实际上会被包含在打包输出的js文件中

**`devtool: cheap-source-map`**：如果不加cheap，将会精确到源码的<font color=FF0000>某一行某一列（即精确到字符）</font>，而使用cheap，将会精确到代码的某一行，不会精确到某个字符，这样大大提高了打包效率。同时，cheap还有一个功能：如果添加了cheap，那么打包时只会管业务代码的映射，对于其他的代码（比如loader、第三方模块）将不会关心。

**`devtool: cheap-module-source-map`**： 如果想要对于上面所说的其他的非业务代码（比如loader、第三方模块）也会做映射，可以加上**`module`**

**`detvool: 'eval'`**：<font color=FF0000>是执行效率最快的</font>，虽然它同样可以定位错误的源代码；不过，不是记录对应关系；而是将错误直接记录在打包输出的js文件中，不过对于复杂的项目，使用eval可能提示错误并不全面。

**`devtool: 'none'`** ：表示省略 devtool 选项 - 不生成 sourceMap。

##### <font color=FF0000>**最佳实践：**</font>

- 在<font color=FF0000>开发环境</font>中，建议使用`cheap-module-eval-source-map`，这样提示出的错误是比较全的，同时打包速度也很快
- 在生产环境中，建议使用`cheap-module-source-map`，这样提示效果会更好一些

  

#### webpack-dev-server

webpack-dev-server可以用来实现<font color=FF0000>热部署</font>，即修改源代码，不需要在输入npm run * 或者 npx webpack * 命令，即可自动完成打包。

**方法有三种：**

- 在package.json的npm scripts中进行如下设置：

  ```js
  "scripts": {
    "watch": "webpack --watch"
  }
  ```

  然后输入`npm run watch`以使用。

- **使用webpack-dev-server（<font color=FF0000>最推荐</font>）**

  上面的已经简单实现了，不过这还不够好，希望可以实现自动打包、自动打开浏览器、在代码改变后自动刷新浏览器、还可以模拟服务器上的特性。示例如下：

  ```js
  //在webpack.config.js配置文件中，与entry同一层级
  devServer：{
    contentBase: './dist', //服务器要启在哪一个文件夹下
    open: true, //自动打开浏览器，并访问配置的url
    port: 8080, //设置端口号
  }
    
  //在package.json配置文件中的scripts下：
  "scripts": {
    "start": "webpack-dev-server",
  }
  ```

  另外：如其名，webpack-dev-server只在development环境中需要被使用（配置），production环境不需要使用（配置）

  <font color=FF0000>**启用模拟服务器的作用：使用http协议，而不是file协议，file协议无法使用ajax等web服务**</font>

- 自己手动实现webpack-dev-server，在package.json配置文件中的scripts（如下示例）。然后自己使用node编写server.js代码（非常复杂，不推荐）

  ```json
  "scripts": {
    "server": "node server.js"
  }
  ```



//TODO 阅读：https://webpack.js.org/api/cli/  https://webpack.js.org/api/node/  https://webpack.js.org/guides/development/  https://webpack.js.org/configuration/devtool/  https://webpack.js.org/configuration/dev-server/



#### Hot Module ReplaceMent（HMR） 热模块更新 

使用webpack-dev-server之后将不会生成dist目录，原因是：webpack-dev-server虽然也会对项目进行打包，但是<font color=FF0000>打包的结果会放在内存中</font>

```js
//webpack.config.js 文件下
devServer: {
  hot: true, //开启Hot Module ReplaceMent（HMR） 热模块更新 功能
  hotOnly: true, //即便是HMR的功能没有生效，也不会让浏览器自动刷新
}
```

开启HMR功能还需要使用HotModuleReplacementPlugin插件，引入HotModuleReplacementPlugin插件的方法如下：

```js
plugins: [
  new webpack.HotModuleReplacementPlugin()
]
```

热模块更新的作用：

- 方便调试CSS代码
- 开启局部刷新

局部刷新的方法：

```js
if(module.hot) {
  moduele.hot.accept('./number', () => {
    foo()  //	进行需要局部刷新的代码
  })
}
```



//TODO 阅读：https://webpack.js.org/guides/hot-module-replacement/  https://webpack.js.org/api/hot-module-replacement/（HMR除了accept方法外还有什么方法） https://webpack.js.org/concepts/hot-module-replacement/



#### 使用Babel处理ES6语法的代码

```js
//webpack.config.js文件下
module: {
  rules: {
    test: /\.js%/,
    exclude: /node_modules/, //对于node_modules文件夹下的js文件不翻译
    loader: "babel-loader",
    options: {
      "presets": [["@babel/preset-env", {
        targets: {
          chrome: "67", //只针对chrome 67以下的浏览器
        }
        useBuiltIns: "usage"
      }]]
    }
  }
}
```

**babel/polyfill：**用于对于一些<font color=FF0000>更低版本</font>的浏览器，提供ES6支持。在 **`import "@babel/polyfill"`** 时默认将所有的适配内容都放入输出的js文件中，这样会导致js文件臃肿，所以可以使用**`useBuiltIns: "usage"`**（见上面）以设置放入输出js文件的，只有业务代码中被使用的那部分（<font color=FF0000>**按需打包**</font>）。不过，babel/polyfill会污染全局环境，所以只推荐在业务代码中使用，而如果写的是框架，则建议使用**`@babel/plugin-transform-runtime`**

babel的配置文件是**`.babelrc`**



webpack打包React需要安装、使用babel/preset-react

```js
options: {
      "presets": [
        ["@babel/preset-env", {
        	targets: {
          	chrome: "67", //只针对chrome 67以下的浏览器
        	}
        	useBuiltIns: "usage"
      		}
        ],
        "@babel/preset-react"
      ]
    }
```

同样presets调用的顺序也是从下到上，从右到左



#### Tree Shaking

对于ES Module引入时候，默认会将业务代码中所有ES6代码打包引入，而不管这个代码块是否被调用。这样就没有做到<font color=FF0000>按需使用</font>，而这时可以使用<font color=FF0000>**Tree Shaking**，实现想要的效果</font>。Tree Shaking<font color=FF0000>只支持</font>ES Module代码的引入。代码使用如下：

```js
//webpack.config.js
optimization: {
  usedExports: true
}

//package.json，最外层
{
  "sideEffects": ['@babel/polly-fill'], //即使没有用到，也不希望被忽略的模块@babel/polly-fill。如果没有可以直接写false
}
```

另外：即使不使用的那些代码，在<font color=FF0000>开发环境</font>的打包中，那些不用的代码将不会被删掉，而是告知你只使用了哪些代码，便于你开发。而在生产环境的打包中，将会直接删掉那些不用的代码。

<font color=FF0000>**补充：**</font>在生产环境的打包中，Tree Shaking是自动生效的，即：你不需要写**`optimization: { usedExports: true }`**，不过sideEffects还是要写的。



#### Development模式和Production模式的区分打包

由于Development和Production是两种模式，所以不建议它们共享一个webpack.config.js，在打包时还需要分别修改配置，容易造成遗漏和BUG，所以建议两种模式分别使用两个不同的配置文件，比如：`webpack.dev.conf.js`和`webpack.prod.conf.js`

相对应的需要在package.json分别配置dev环境和prod环境的npm scripts

```json
"scripts": {
  "dev": "webpack-dev-server --config webpack.dev.conf.js", //后面的--config webpack.dev.conf.js表示选择哪个配置文件
  "build": "webpack --config webpack.prod.conf.js ", //build是指打包上线的代码，即production
}
```

由于`webpack.dev.conf.js`和`webpack.prod.conf.js`中存在很多相同的配置（代码），可以添加一个`webpack.common.conf.js`存放公共代码，把公用代码放入其中。同时，想要合并两个配置文件，需要安装插件：`webpack-merge`，且dev / prod的配置中也需要进行修改，以`webpack.dev.conf.js`为例：

```js
const commonConfig = require('./webpack.common.conf.js')

//dev的配置
const devConfig = {
  //...
}

module.exports = merge(commonConfig, devConfig)
```

另外，可以新建一个build文件夹，将`webpack.dev.conf.js`、`webpack.prod.conf.js`以及`webpack.common.conf.js`三个配置文件放入其中。不过这时package.json需要修改对应的配置文件的路径

```json
"scripts": {
  "dev": "webpack-dev-server --config ./build/webpack.dev.conf.js", //后面的--config webpack.dev.conf.js表示选择哪个配置文件
  "build": "webpack --config ./build/webpack.prod.conf.js ", //build是指打包上线的代码，即production
}
```



#### Code Splitting 代码分割

