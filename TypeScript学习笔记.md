# TypeScript 学习笔记



#### 一些资料

###### 官方资料

[The TypeScript release notes](https://devblogs.microsoft.com/typescript/) covers a each new features in-depth.

###### 网络书籍

- [阮一峰 - TypeScript 教程](https://wangdoc.com/typescript/)
- [TypeScript入门教程](https://ts.xcatliu.com)
- [深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/)
- [Exploring TypeScript (TS 5.8 Edition) ](https://exploringjs.com/ts/book/index.html) 以及 “前作” Tackling TypeScript

###### 中文文档 / 手册

- www.tslang.cn

- https://typescript.bootcss.com

- [冴羽的 TS 教程](https://yayujs.com/)

###### CheatSheet

- [Quick Reference - TypeScript 备忘清单](https://wangchujiang.com/reference/docs/typescript.html)



## 《TypeScript入门教程》笔记

链接 🔗：[《TypeScript入门教程》](https://ts.xcatliu.com)

#### TS 介绍

##### TypeScript 是静态语言

类型系统<font color=FF0000> 按照「类型检查的时机」来分类，可以分为动态类型和静态类型</font>。

- **动态类型** 是指在<font color=FF0000> 运行时才会进行类型检查</font>，这种语言的类型错误往往会导致运行时错误。
- **静态类型** 是指<font color=FF0000> 编译阶段就能确定每个变量的类型</font>，这种语言的类型错误往往会导致语法错误。

<font color=LightSeaGreen>JavaScript 是一门解释型语言，没有编译阶段，所以它是动态类型。</font>

<font color=fuchsia>**TypeScript 在运行前需要先编译为 JavaScript，而在编译阶段就会进行类型检查，所以 TypeScript 是静态类型**</font>

##### TypeScript 是弱类型

<font color=FF0000>类型系统按照「是否允许隐式类型转换」来分类</font>，可以分为强类型和弱类型

TypeScript 是完全兼容 JavaScript 的，它不会修改 JavaScript 运行时的特性，所以它们都是弱类型。

##### 各种语言强弱 / 静动态

![](https://i.loli.net/2021/08/31/mvDCBdjIHUfTO8V.png)

##### TS的类型和编译

在 TypeScript 中，我们使用 `:` 指定变量的类型，`:` 的前后有没有空格都可以。

**TypeScript 只会在编译时对类型进行静态检查，如果发现有错误，编译的时候就会报错**。

TypeScript 编译的时候即使报错了，还是会生成编译结果，我们仍然可以使用这个编译之后的文件。

如果要在报错的时候终止 js 文件的生成，可以在 `tsconfig.json` 中配置 `noEmitOnError` 即可



#### 特殊类型

##### 空值

JavaScript 没有空值 Void 的概念，在 TypeScript 中，可以用 void 表示没有任何返回值的函数：

```ts
function alertName(): void {
    alert('My name is Tom');
}
```

声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null（只在 tsconfig中  `--strictNullChecks` 未指定时）：

```ts
let unusable: void = undefined;
```

##### Null 和 Undefined

在 TypeScript 中，可以使用 null 和 undefined 来定义这两个原始数据类型：

```ts
let u: undefined = undefined;
let n: null = null;
```

与 void 的区别是：<font color=FF0000>undefined 和 null 是所有类型的子类型</font>。<font color=FF0000>也就是说 undefined 类型的变量，可以赋值给 number 类型的变量</font>：

```ts
// 这样不会报错
let num: number = undefined;
// 这样也不会报错
let u: undefined;
let num: number = u;
```

而 void 类型的变量不能赋值给 number 类型的变量：

```ts
let u: void;
let num: number = u;
// Type 'void' is not assignable to type 'number'.
```

##### 任意值

**任意值（Any）用来<font color=FF0000> 表示允许赋值为任意类型</font>。**

如果是一个普通类型，在赋值过程中改变类型是不被允许的。<font color=FF0000> 如果是 any 类型，则允许被赋值为任意类型</font>。

```ts
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```

**任意值的属性和方法**

<font color=FF0000> 在任意值上访问任何属性都是允许的</font>：

```ts
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
```

<font color=FF0000> 也允许调用任何方法</font>：

```ts
let anyThing: any = 'Tom';
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```

可以认为，声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值

<font color=FF0000> **变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型**：</font>

```ts
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```

等价于

```ts
let something: any;
something = 'seven';
something = 7;

something.setName('Tom');
```



#### 类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查**

```ts
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```



#### 联合类型

联合类型 ( Union Types ) 表示<font color=FF0000> 取值可以为多种类型中的一种</font>。

##### 示例

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = true;

// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
//   Type 'boolean' is not assignable to type 'number'.
```

联合类型使用 `|` 分隔每个类型。

这里 ` let myFavoriteNumber: string | number ` 的含义是，<font color=FF0000> 允许 `myFavoriteNumber` 的类型是 `string` 或者 `number` ，但是不能是其他类型</font>。

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，<font color=FF0000> 我们只能访问此联合类型的所有类型里共有的属性或方法</font>。

```ts
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型



#### 对象的类型——接口

在 TypeScript 中，我们使用<font color=FF0000> 接口（Interfaces）来定义对象的类型</font>。

##### 什么是接口

在面向对象语言中，接口 ( Interfaces ) 是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类 ( classes ) 去实现 ( implement )。

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状 ( Shape )」进行描述。

<font color=red>**接口一般首字母大写**</font>

##### 示例

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

上面的例子中，我们定义了一个接口 `Person` ，接着定义了一个变量 tom，它的类型是 `Person` 。<font color=FF0000>这样，我们就约束了 tom 的 **形状必须和接口 `Person` 一致**</font>。定义的变量比接口少了一些属性是不允许的，多一些属性也是不允许的。

##### 可选属性

有时我们希望不要完全匹配一个形状，那么可以用可选属性：

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

可选属性的含义是该属性可以不存在。这时**仍然不允许添加未定义的属性**

##### 任意属性

> 💡 更专业的名称叫作 “索引签名” ( [Index Signatures](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures) )，也可参考 [[#TS 类型系统中的类型#接口]] 中的 “可索引签名”

有时候我们<font color=dodgerblue> **希望一个接口允许有任意的属性，可以使用如下方式**</font>：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

<font color=fuchsia> 使用 `[propName: string]` 定义了任意属性取 string 类型的值</font>。

需要注意的是，<font color=red> **一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**：</font>

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

上例中，任意属性的值允许是 string，但是可选属性 age 的值却是 number，number 不是 string 的子属性，所以报错了。

另外，在报错信息中可以看出，此时 `{ name: 'Tom', age: 25, gender: 'male' }` 的类型被推断成了 `{ [x: string]: string | number; name: string; age: number; gender: string; }` ，这是联合类型和接口的结合。

一个接口中只能定义一个任意属性。<font color=FF0000> 如果接口中有多个类型的属性，则可以在任意属性中使用联合类型</font>：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

<font color=dodgerblue> **补充：**</font>类似的属性在对象中可以定义多个。不是一个接口中的可选属性，在对象中只能定义一次；而是可以定义成多个

> 👀 官方文档补充
>
> > An index signature property type must be either ‘string’ or ‘number’. （注：也就是说不能是 `symbol` 类型？）

##### 只读属性

有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 readonly 定义只读属性

```ts
interface Person {
    readonly id: number;
  	// ...
}
```

**注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**



#### 数组的类型

在 TypeScript 中，数组类型有多种定义方式，比较灵活。

##### 「类型 + 方括号」表示法

<font color=FF0000> 最简单的方法是使用「类型 + 方括号」来表示数组</font>：

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

数组的项中**不允许**出现其他的类型：

```ts
let fibonacci: number[] = [1, '1', 2, 3, 5];
// Type 'string' is not assignable to type 'number'.
```

##### 数组泛型

我们也<font color=FF0000> 可以使用数组泛型 ( Array Generic ) `Array<elemType>` 来表示数组</font>

```ts
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

##### 用接口表示数组

<font color=FF0000> 接口也可以用来描述数组</font>：

```ts
interface NumberArray {
	[index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

##### 类数组

类数组 ( Array-like Object ) 不是数组类型

##### any 在数组中的应用

一个比较常见的做法是，用 any 表示数组中允许出现任意类型：

```ts
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```



#### 函数的类型

##### 函数声明

在 JavaScript 中，<font color=FF0000> 有两种常见的定义函数的方式——函数声明（Function Declaration）和函数表达式（Function Expression）</font>：

```js
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中<font color=FF0000> **函数声明的类型定义**</font>较简单：

```ts
function sum(x: number, y: number): number {
    return x + y;
}
```

注意，**输入多余的（或者少于要求的）参数，是不被允许的**

##### 函数表达式

如果要我们现在写一个对<font color=FF0000> 函数表达式（Function Expression）</font>的定义，可能会写成这样：

```ts
let mySum = function (x: number, y: number): number {
    return x + y;
};
```

这是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的 mySum，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给 mySum 添加类型，则应该是这样：

```ts
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

**注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>。**

在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

在 ES6 中，`=>` 叫做箭头函数，应用十分广泛，可以参考 [ES6 中的箭头函数](http://es6.ruanyifeng.com/#docs/function#箭头函数)

##### 用接口定义函数的形状

我们也可以使用接口的方式来定义一个函数需要符合的形状：

```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。

##### 可选参数

<font color=FF0000> 用 `?` 表示可选的参数，需要注意的是，可选参数必须接在必需参数后面。换句话说，**可选参数后面不允许再出现必需参数了**</font>

```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

##### 参数默认值

在 ES6 中，我们允许给函数的参数添加默认值，<font color=FF0000> TypeScript 会将添加了**默认值的参数识别为可选参数** </font>：

```ts
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

<font color=FF0000> 此时就不受「可选参数必须接在必需参数后面」的限制了</font>

**剩余参数**

ES6 中，<font color=FF0000> 可以使用 `...rest` 的方式获取函数中的剩余参数（ rest 参数）</font>。items 是一个数组。所以我们可以用数组的类型来定义它：

```ts
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}
let a = [];
push(a, 1, 2, 3);
```

<font color=FF0000> 注意，`rest` 参数只能是最后一个参数</font>

##### 重载

**重载允许一个函数接受不同数量或类型的参数时，作出不同的处理**

比如，我们需要实现一个函数 reverse，输入数字 123 的时候，输出反转的数字 321，输入字符串 'hello' 的时候，输出反转的字符串 'olleh'。

利用联合类型，我们可以这么实现：

```ts
function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

**然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。**

这时，我们可以使用重载定义多个 reverse 的函数类型：

```ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```



### 声明文件

##### 总结

- `declare var` ： 声明全局变量
- `declare function` ：声明全局方法
- `declare class` ：声明全局类
- `declare enum` ：声明全局枚举类型
- `declare namespace` ：声明（含有子属性的）全局对象
- `interface` 和 `type` ：声明全局类型
- `export` ：导出变量
- `export namespace` ：导出（含有子属性的）对象
- `export default` ：ES6 默认导出
- `export =` ：<font color=LightSeaGreen>commonjs 导出模块</font>
- `export as namespace` ：<font color=LightSeaGreen>UMD 库声明全局变量</font>
- `declare global` ：扩展全局变量
- `declare module` ：扩展模块
- `/// <reference /> ` ：三斜线指令

##### 书写声明语句

以通过在 html 的 `<script>` 引入并使用 jQuery 为例：通常有 `$` 或 `jQuery` 两种获取一个元素的方法：

```ts
$('foo')
jQuery('#foo')
```

但是如果使用 ts， tsc 并不知道 `$` 或 `jQuery` 是什么，这继续需要通过 `declare var` 来定义它的类型：

```ts
declare var jQuery: (selector: string) => any;
jQuery('#foo')
```

上例中，<font color=red>`declare var` 并没有真的定义一个变量，只是定义了全局变量 `jQuery` 的类型</font>，<font color=red>仅仅会用于编译时的检查，在编译结果中会被删除</font>。它编译结果是：

```ts
jQuery('#foo');
```

##### 书写声明文件

通常我们会把声明语句放到一个单独的文件中，比如 `jQuery.d.ts` ；这就是声明文件：

```ts
// src/jQuery.d.ts
declare var jQuery: (selector: string) => any;
```

```ts
// src/index.ts
jQuery('#foo');
```

声明文件必须以 `.d.ts` 为后缀。

<font color=dodgerBlue>一般来说，ts 会解析项目中所有的 `*.ts` 文件，当然也包含以 `.d.ts` 结尾的文件</font>；所以<font color=LightSeaGreen>将 `jQuery.d.ts` 放到项目中时，其他所有 `*.ts` 文件就都可以获得 `jQuery` 的类型定义了</font>。

假如仍然无法解析，那么可以检查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，确保其包含了 `jQuery.d.ts` 文件。

> 💡 `files`、`include` 和 `exclude` 的配置及之间的关系，可以参考 [[#files、include 和 exclude]] 中的内容

###### 第三方声明文件

当然，jQuery 的声明文件不需要我们定义了，社区已经帮我们定义好了：[jQuery in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/jquery/index.d.ts)。

> 💡 点击链接就是 GitHub DefinelyTyped repo 中的一个  `.d.ts` 文件

可以直接下载下来使用（这个 `.d.ts` 文件），但是<font color=red>更推荐的是：使用 `@types` 统一管理第三方库的声明文件</font>。

<font color=LightSeaGreen>`@types` 的使用方式很简单，直接用 npm 安装对应的声明模块即可</font>，以 jQuery 举例：

```bash
npm install @types/jquery --save-dev
```

#### 书写声明文件

当一个第三方库没有提供声明文件时，使用者就需要自己书写声明文件。

<font color=dodgerBlue>**库的使用场景主要有以下几种**</font>：

- **全局变量**：通过 `<script>` 标签引入第三方库，注入全局变量
- **npm 包**：通过 `import foo from 'foo'` 导入，符合 ES6 模块规范
- **UMD 库**：<font color=red>既可以通过 `<script>` 标签引入，又可以通过 `import` 导入</font>
- **直接扩展全局变量**：通过 `<script>` 标签引入后，改变一个全局变量的结构
- **在 npm 包或 UMD 库中扩展全局变量**：引用 npm 包或 UMD 库后，改变一个全局变量的结构
- **模块插件**：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构

##### 全局变量

全局变量是最简单的一种场景，先前的例子就是通过 `<script>` 标签引入 jQuery，注入全局变量 `$` 和 `jQuery`。

使用全局变量的声明文件时，<font color=dodgerBlue>如果是以 `npm install @types/xxx --save-dev` 安装</font>的，则<font color=red>**不需要任何配置**</font>。如果是<font color=dodgerBlue>将声明文件直接存放于当前项目中</font>，则<font color=red>建议和其他源码一起放到 `src` 目录下（或者对应的源码目录下）</font>：

```
/path/to/project
├── src
|  ├── index.ts
|  └── jQuery.d.ts
└── tsconfig.json
```

<font color=dodgerBlue>**全局变量的声明文件主要有以下几种语法：**</font>

- `declare var` ：声明全局变量
- `declare function` ：声明全局方法
- `declare class` ：声明全局类
- `declare enum` ：声明全局枚举类型
- `declare namespace` ：声明（含有子属性的）<font color=red>全局对象</font>
- `interface` 和 `type` ：声明全局类型

###### `declare var`

在所有的声明语句中，`declare var` 是最简单的，如之前所学，它能够用来定义一个全局变量的类型。与其类似的，还有 `declare let` 和 `declare const`，<font color=red>**使用 `let` 与使用 `var` 没有什么区别**</font>：

```ts
// src/jQuery.d.ts
declare let jQuery: (selector: string) => any;
```

```ts
// src/index.ts
jQuery('#foo');
// 使用 declare let 定义的 jQuery 类型，允许修改这个全局变量
jQuery = function(selector) {
    return document.querySelector(selector);
};
```

而当使用 `const` 定义时，表示此时的全局变量是一个常量，不允许再去修改它的值了：

```ts
// src/jQuery.d.ts
declare const jQuery: (selector: string) => any;

jQuery('#foo');
// 使用 declare const 定义的 jQuery 类型，禁止修改这个全局变量
jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: Cannot assign to 'jQuery' because it is a constant or a read-only property.
```

一般来说，全局变量都是禁止修改的常量，所以大部分情况都应该使用 `const` 而不是 `var` 或 `let`。

需要注意的是，声明语句中只能定义类型，切勿在声明语句中定义具体的实现：

```ts
// src/jQuery.d.ts
declare const jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: An implementation cannot be declared in ambient contexts.
```

###### `declare function`

`declare function` 用来定义全局函数的类型。jQuery 其实就是一个函数，所以也可以用 `function` 来定义：

```ts
// src/jQuery.d.ts
declare function jQuery(selector: string): any;
```

```ts
// src/index.ts
jQuery('#foo');
```

<font color=red>在函数类型的声明语句中，函数重载也是支持的</font>：

```ts
// src/jQuery.d.ts
declare function jQuery(selector: string): any;
declare function jQuery(domReadyCallback: () => any): any;
```

```ts
// src/index.ts
jQuery('#foo');
jQuery(function() {
    alert('Dom Ready!');
});
```

###### declare class

当全局变量是一个类的时候，我们用 `declare class` 来定义它的类型：

```ts
// src/Animal.d.ts
declare class Animal {
    name: string;
    constructor(name: string);
    sayHi(): string;
}
```

```ts
// src/index.ts
let cat = new Animal('Tom');
```

同样的，<font color=LightSeaGreen>`declare class` 语句也只能用来定义类型，不能用来定义具体的实现</font>；如定义 `sayHi` 方法的具体实现则会报错：

```ts
// src/Animal.d.ts
declare class Animal {
    name: string;
    constructor(name: string);
    sayHi() {
        return `My name is ${this.name}`;
    };
    // ERROR: An implementation cannot be declared in ambient contexts.
}
```

###### `declare enum`

使用 `declare enum` 定义的枚举类型也称作外部枚举（Ambient Enums），举例如下：

```ts
// src/Directions.d.ts
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
```

```ts
// src/index.ts
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

与其他全局变量的类型声明一致，`declare enum` 仅用来定义类型，而不是具体的值。

`Directions.d.ts` 仅仅会用于编译时的检查，声明文件里的内容在编译结果中会被删除。它编译结果是：

```js
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

其中 `Directions` 是由第三方库定义好的全局变量。

###### `declare namespace`

<font color=dodgerBlue>`namespace` 是 ts 早期时为了解决模块化而创造的关键字，中文称为命名空间</font>。由于历史遗留原因，在早期还没有 ES6 的时候，ts 提供了一种模块化方案，使用 `module` 关键字表示内部模块。但由于后来 ES6 也使用了 `module` 关键字，ts 为了兼容 ES6，使用 `namespace` 替代了自己的 `module`，更名为命名空间。

<font color=LightSeaGreen>**随着 ES6 的广泛应用，现在已经不建议再使用 ts 中的 `namespace`**</font>，而<font color=LightSeaGreen>**推荐使用 ES6 的模块化方案**</font>了，故我们不再需要学习 `namespace` 的使用了。

<font color=dodgerBlue>`namespace` 被淘汰了</font>，但是<font color=red>在声明文件中，`declare namespace` 还是比较常用的</font>，它 <font color=fuchsia>**用来表示全局变量是一个对象，包含很多子属性**</font>。

比如 `jQuery` 是一个全局变量，它是一个对象，提供了一个 `jQuery.ajax` 方法可以调用，那么我们就应该<font color=red>使用 `declare namespace jQuery` 来声明这个拥有多个子属性的全局变量</font>。

```ts
// src/jQuery.d.ts
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

```ts
// src/index.ts
jQuery.ajax('/api/get_something');
```

注意，<font color=red>在 `declare namespace` 内部，**直接使用 `function ajax` 来声明函数**，而不是使用 `declare function ajax`</font> 。类似的，<font color=LightSeaGreen>也可以使用 `const` , `class` , `enum` 等语句</font>：

```ts
// src/jQuery.d.ts
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
    const version: number;
    class Event {
        blur(eventType: EventType): void
    }
    enum EventType {
        CustomClick
    }
}
```

```ts
// src/index.ts
jQuery.ajax('/api/get_something');
console.log(jQuery.version);
const e = new jQuery.Event();
e.blur(jQuery.EventType.CustomClick);
```

###### 嵌套的命名空间

<font color=dodgerBlue>如果对象拥有深层的层级</font>，则<font color=red>需要用嵌套的 `namespace` 来声明深层的属性的类型</font>：

```ts
// src/jQuery.d.ts
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
    namespace fn { // 👀
        function extend(object: any): void;
    }
}
```

```ts
// src/index.ts
jQuery.ajax('/api/get_something');
jQuery.fn.extend({
    check: function() {
        return this.each(function() {
            this.checked = true;
        });
    }
});
```

假如 `jQuery` 下仅有 `fn` 这一个属性（没有 `ajax` 等其他属性或方法），则可以不需要嵌套 `namespace`：

```ts
// src/jQuery.d.ts
declare namespace jQuery.fn { // 👀 jQuery.fn
    function extend(object: any): void;
}
```

```ts
// src/index.ts
jQuery.fn.extend({
    check: function() {
        return this.each(function() {
            this.checked = true;
        });
    }
});
```

> 👀 虽然但是，从代码可维护性的角度来考虑，感觉默认情况下嵌套 namespace 是更值得推荐的。

###### `interface` 和 `type`

除了全局变量之外，可能有一些类型我们也希望能暴露出来。在类型声明文件中，我们可以直接使用 `interface` 或 `type` 来声明一个全局的接口或类型：

```ts
// src/jQuery.d.ts
interface AjaxSettings {
    method?: 'GET' | 'POST'
    data?: any;
}
declare namespace jQuery {
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

这样的话，在其他文件中也可以使用这个接口或类型了：

```ts
// src/index.ts
let settings: AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/post_something', settings);
```

`type` 与 `interface` 类似，不再赘述。

###### 防止命名冲突

<font color=red>**暴露在最外层的 `interface` 或 `type` 会作为全局类型作用于整个项目中**</font>，我们<font color=fuchsia>应该尽可能的减少全局变量或全局类型的数量；故最好将他们放到 `namespace` 下</font>：

```ts
// src/jQuery.d.ts
declare namespace jQuery {
    interface AjaxSettings {
        method?: 'GET' | 'POST'
        data?: any;
    }
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

注意，在使用这个 `interface` 的时候，也应该加上 `jQuery` 前缀：

```ts
// src/index.ts
let settings: jQuery.AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/post_something', settings);
```

###### 声明合并

假如 jQuery 既是一个函数，可以直接被调用 `jQuery('#foo')`；同时也是一个对象，拥有子属性 `jQuery.ajax()`（事实确实如此），那么我们可以组合多个声明语句，它们会不冲突的合并起来：

```ts
// src/jQuery.d.ts
declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

```ts
// src/index.ts
jQuery('#foo');
jQuery.ajax('/api/get_something');
```

##### npm 包

一般我们通过 `import foo from 'foo'` 导入一个 npm 包，这是符合 ES6 模块规范的。

在尝试给一个 npm 包创建声明文件之前，需要先看看它的声明文件是否已经存在。<font color=dodgerBlue>一般来说，npm 包的 **声明文件** 可能存在于两个地方</font>：

1. **与该 npm 包绑定在一起**。<font color=red>判断依据是 `package.json` 中有 `types` 字段</font>（👀 形如 `@types/repoName`），<font color=red>或者有一个 `index.d.ts` 声明文件</font>。**这种模式不需要额外安装其他包**，是<font color=red>**最为推荐的**</font>，所以以后我们自己创建 npm 包的时候，最好也将声明文件与 npm 包绑定在一起。
2. **发布到 `@types` 里**。我们<font color=LightSeaGreen>只需要尝试安装一下对应的 `@types` 包就知道是否存在该声明文件</font>，安装命令是 `npm install @types/foo --save-dev`。这种模式一般是由于 npm 包的维护者没有提供声明文件，所以只能由其他人将声明文件发布到 `@types` 里了。

<font color=LightSeaGreen>假如以上两种方式都没有找到对应的声明文件，**那么就需要自己为它写声明文件了**</font>。由于是通过 `import` 语句导入的模块，所以声明文件存放的位置也有所约束，<font color=dodgerBlue>**一般有两种方案**</font>：

1. **创建一个 `node_modules/@types/foo/index.d.ts` 文件**，存放 `foo` 模块的声明文件。<font color=LightSeaGreen>这种方式不需要额外的配置</font>，但是 <font color=red>`node_modules` 目录不稳定，代码也没有被保存到仓库中，无法回溯版本，有不小心被删除的风险</font>，故<font color=red>不太建议用这种方案</font>，一般只用作临时测试。
2. <font color=red>**创建一个 `types` 目录，专门用来管理自己写的声明文件**，将 `foo` 的声明文件放到 `types/foo/index.d.ts` 中</font>。<font color=fuchsia>这种方式**需要配置下 `tsconfig.json` 中的 `paths` 和 `baseUrl` 字段**</font>。

目录结构：

```
/path/to/project
├── src
|  └── index.ts
├── types
|  └── foo
|     └── index.d.ts
└── tsconfig.json
```

`tsconfig.json` 内容：

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "baseUrl": "./",
        "paths": {
            "*": ["types/*"]
        }
    }
}
```

配置之后，通过 `import` 导入 `foo` 的时候，也会去 `types` 目录下寻找对应的模块的声明文件了。

⚠️ 注意： `module` 配置可以有很多种选项，不同的选项会影响模块的导入导出模式。<font color=dodgerBlue>**这里我们使用了 `commonjs` 这个最常用的选项，后面的教程也都默认使用的这个选项**</font>。

<font color=dodgerBlue>npm 包的声明文件主要有以下几种语法：</font>

- `export` ：导出变量
- `export namespace`  ：导出（含有子属性的）对象
- `export default` ：ES6 默认导出
- `export =` ：commonjs 导出模块

<font color=dodgerBlue>npm 包的声明文件与全局变量的声明文件有很大区别</font>：<font color=dodgerBlue>**在 npm 包的声明文件中**</font>，<font color=red>**使用 `declare` 不再会声明一个全局变量**，**只会在当前文件中声明一个局部变量**</font>。<font color=fuchsia>只有在声明文件中使用 `export` 导出，然后在使用方 `import` 导入后，才会应用到这些类型声明</font>。

`export` 的语法与普通的 ts 中的语法类似，区别仅在于声明文件中禁止定义具体的实现：

```ts
// types/foo/index.d.ts
export const name: string;
export function getName(): string;
export class Animal {
    constructor(name: string);
    sayHi(): string;
}
export enum Directions { Up, Down, Left, Right }
export interface Options {
    data: any;
}
```

对应的导入和使用模块：

```ts
// src/index.ts
import { name, getName, Animal, Directions, Options } from 'foo';

console.log(name);
let myName = getName();
let cat = new Animal('Tom');
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
let options: Options = {
    data: { name: 'foo' }
};
```

###### 混用 `declare` 和 `export`

<font color=red>也可以使用 `declare` 先声明多个变量，**最后再用 `export` 一次性导出**</font>。上例的声明文件可以等价的改写为：

```ts
// types/foo/index.d.ts
declare const name: string;
declare function getName(): string;
declare class Animal {
    constructor(name: string);
    sayHi(): string;
}
declare enum Directions { Up, Down, Left, Right }
interface Options { data: any; }

export { name, getName, Animal, Directions, Options };
```

注意，<font color=fuchsia>**与全局变量的声明文件类似，`interface` 前是不需要 `declare` 的**</font>。

###### `export namespace`

与 `declare namespace` 类似，`export namespace` 用来导出一个拥有子属性的对象：

```ts
// types/foo/index.d.ts
export namespace foo {
    const name: string;
    namespace bar {
        function baz(): string;
    }
}
```

```ts
// src/index.ts
import { foo } from 'foo';

console.log(foo.name);
foo.bar.baz();
```

###### `export default`

在 ES6 模块系统中，使用 `export default` 可导出一个默认值，使用方可以用 `import foo from 'foo'` 导入默认值。

在类型声明文件中，`export default` 用来导出默认值的类型：

```ts
// types/foo/index.d.ts
export default function foo(): string;
```

```ts
// src/index.ts
import foo from 'foo';

foo();
```

⚠️ 注意：<font color=fuchsia size=4>**只有 `function`、`class` 和 `interface` 可直接默认导出**</font>，<font color=red>**其他的变量需要先定义，再默认导出**</font>

```ts
// types/foo/index.d.ts
export default enum Directions {
// ERROR: Expression expected.
    Up,
    Down,
    Left,
    Right
}
```

<font color=dodgerBlue>上例 `export default enum` 是错误的语法</font>，需要使用 `declare enum` 定义出来，再使用 `export default` 导出

```ts
// types/foo/index.d.ts
declare enum Directions { Up, Down, Left, Right }

export default Directions;
```

针对这种默认导出，我们<font color=red>**一般会将导出语句放在整个声明文件的最前面**</font>：

```ts
// types/foo/index.d.ts
export default Directions;

declare enum Directions { Up, Down, Left, Right }
```

###### `export =`

在 commonjs 规范中，我们用以下方式来导出一个模块：

```js
// 整体导出
module.exports = foo;
// 单个导出
exports.bar = bar;
```

在 ts 中，<font color=dodgerBlue>**针对这种模块导出，有多种方式可以导入**</font>：

第一种方式是 `const ... = require-expression`

```ts
// 整体导入
const foo = require('foo');
// 单个导入
const bar = require('foo').bar;
```

第二种方式是 `import ... from`，注意针对整体导出，需要使用 `import * as` 来导入：

```ts
// 整体导入
import * as foo from 'foo';
// 单个导入
import { bar } from 'foo';
```

第三种方式是 `import ... = require`，<font color=red>**这也是 ts 官方推荐的方式**</font>：

```ts
// 整体导入
import foo = require('foo');
// 单个导入
import bar = foo.bar;
```

对于这种使用 commonjs 规范的库，假如要为它写类型声明文件的话，就需要使用到 `export =` 这种语法了：

```ts
// types/foo/index.d.ts
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

⚠️ 需要注意的是：<font color=fuchsia>上例中使用了 `export =` 之后，就不能再单个导出 `export { bar }` 了</font>。所以我们通过声明合并，使用 `declare namespace foo` 来将 `bar` 合并到 `foo` 里。

准确地讲，`export =` 不仅可以用在声明文件中，也可以用在普通的 ts 文件中。实际上，<font color=red>`import ... = require` 和 `export =` 都是 ts 为了兼容 AMD 规范和 commonjs 规范而创立的新语法</font>，由于并不常用也不推荐使用，所以这里就不详细介绍了，感兴趣的可以看[官方文档](https://www.typescriptlang.org/docs/handbook/modules.html#export--and-import--require)。

由于很多第三方库是 commonjs 规范的，所以声明文件也就不得不用到 `export =` 这种语法了。但是还是需要再强调下，<font color=red>相比与 `export =`，我们更推荐使用 ES6 标准的 `export default` 和 `export`</font>。

##### UMD 库

<font color=LightSeaGreen>既可以通过 `<script>` 标签引入，又可以通过 `import` 导入的库，称为 UMD 库</font>。相比于 npm 包的类型声明文件，我们<font color=dodgerBlue>需要额外声明一个全局变量</font>，<font color=red>为了实现这种方式，ts 提供了一个新语法 `export as namespace`</font>。

###### `export as namespace`

一般使用 `export as namespace` 时，<font color=red>都是先有了 npm 包的声明文件</font>，<font color=red>**再基于它添加一条 `export as namespace` 语句，即可将声明好的一个变量声明为全局变量**</font>，举例如下：

```ts
// types/foo/index.d.ts
export as namespace foo;
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

当然它也可以与 `export default` 一起使用：

```ts
// types/foo/index.d.ts
export as namespace foo;
export default foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

##### 直接扩展全局变量

有的第三方库扩展了一个全局变量，<font color=dodgerBlue>可是此全局变量的类型却没有相应的更新过来，就会导致 ts 编译错误，此时就需要扩展全局变量的类型</font>。比如扩展 `String` 类型：

```ts
interface String {
    prependHello(): string;
}

'foo'.prependHello();
```

<font color=red>**通过声明合并**，使用 `interface String` 即可给 `String` 添加属性或方法</font>。

也可以使用 `declare namespace` 给已有的命名空间添加类型声明：

```ts
// types/jquery-plugin/index.d.ts
declare namespace JQuery {
    interface CustomOptions {
        bar: string;
    }
}

interface JQueryStatic {
    foo(options: JQuery.CustomOptions): string;
}
```

```ts
// src/index.ts
jQuery.foo({
    bar: ''
});
```

##### 在 npm 包或 UMD 库中扩展全局变量

如之前所说：<font color=lightSeaGreen>对于一个 npm 包或者 UMD 库的声明文件，只有 `export` 导出的类型声明才能被导入</font>。所以<font color=dodgerBlue>对于 npm 包或 UMD 库，如果导入此库之后会扩展全局变量</font>，则<font color=dodgerBlue>需要使用另一种语法在声明文件中扩展全局变量的类型</font>，那<font color=red>就是 `declare global`</font>。

###### `declare global`

使用 `declare global` 可以在 npm 包或者 UMD 库的声明文件中扩展全局变量的类型：

```ts
// types/foo/index.d.ts
declare global {
    interface String {
        prependHello(): string;
    }
}

export {};
```

```ts
// src/index.ts
'bar'.prependHello();
```

⚠️ 注意：<font color=red>**即使此声明文件不需要导出任何东西，仍然需要导出一个空对象**</font>，用来<font color=red>**告诉编译器这是一个模块的声明文件**，而不是一个全局变量的声明文件</font>。

##### 模块插件

<font color=lightSeaGreen>**有时通过 `import` 导入一个模块插件，可以改变另一个原有模块的结构**</font>。<font color=dodgerBlue>如果原有模块已经有了类型声明文件，而插件模块没有类型声明文件</font>，就会导致类型不完整，缺少插件部分的类型。ts <font color=red>提供了一个语法 `declare module`</font>，它<font color=red>可以用来扩展原有模块的类型</font>。

###### `declare module`

<font color=dodgerBlue>如果需要扩展原有模块</font>，<font color=red>需要在类型声明文件中先引用原有模块，**再使用 `declare module` 扩展原有模块**</font>：

```ts
// types/moment-plugin/index.d.ts
import * as moment from 'moment'; // 👀 引入

declare module 'moment' { // 👀 使用 declare module 导出
    export function foo(): moment.CalendarKey;
}
```

```ts
// src/index.ts
import * as moment from 'moment';
import 'moment-plugin'; // 👀 使用时，需要将 original module 和 add-on 同时导入

moment.foo();
```

`declare module` 也可用于在一个文件中一次性声明多个模块的类型：

```ts
// types/foo-bar.d.ts
declare module 'foo' {
    export interface Foo {
        foo: string;
    }
}

declare module 'bar' {
    export function bar(): string;
}
```

```ts
// src/index.ts
import { Foo } from 'foo';
import * as bar from 'bar';

let f: Foo;
bar.bar();
```

##### 声明文件中的依赖

<font color=dodgerBlue>一个声明文件有时会依赖另一个声明文件中的类型</font>，比如前面的 `declare module` 的例子( [[#`declare module`]] )，就在声明文件中导入了 `moment`，并使用了 `moment.CalendarKey`这个类型：

```ts
// types/moment-plugin/index.d.ts
import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}
```

<font color=dodgerBlue>除了 可以在声明文件中通过 `import` 导入另一个声明文件中的类型之外，还有一个语法也可以用来导入另一个声明文件</font>，那<font color=red>就是三斜线指令</font>。

###### 三斜线指令

<font color=dodgerBlue>**与 `namespace` 类似**</font>，<font color=red>三斜线指令也是 **ts 在早期版本中** 为了 <font size=4>**描述模块之间的依赖关系**</font> 而创造的语法</font>。<font color=lightSeaGreen>随着 ES6 的广泛应用，现在已经不建议再使用 ts 中的三斜线指令来声明模块之间的依赖关系了</font>。

但是在声明文件中，它还是有一定的用武之地。

类似于声明文件中的 `import`，它可以用来导入另一个声明文件。与 `import` 的区别是，<font color=dodgerBlue>当且仅当在以下几个场景下，我们才需要使用三斜线指令替代 `import`</font>：

- 当我们在**书写**一个全局变量的声明文件时
- 当我们需要**依赖**一个全局变量的声明文件时

###### 三斜线指令 - 书写一个全局变量的声明文件

这些场景听上去很拗口，但实际上很好理解：<font color=red>**在全局变量的声明文件中，是不允许出现 `import`, `export` 关键字的**</font>。<font color=LightSeaGreen>一旦出现，**就会被视为一个 npm 包或 UMD 库**，**不再是全局变量的声明文件**了</font>。故当在书写一个全局变量的声明文件时，如果需要引用另一个库的类型，那么就必须用三斜线指令了：

```ts
// types/jquery-plugin/index.d.ts 👀 在 d.ts 文件中

/// <reference types="jquery" /> // 👀 引入 jquery
declare function foo(options: JQuery.AjaxSettings): string; // 👀 使用 jquery
```

```ts
// src/index.ts
foo({});
```

三斜线指令的语法如上，`///` 后面使用 xml 的格式添加了对 `jquery` 类型的依赖，这样就可以在声明文件中使用 `JQuery.AjaxSettings` 类型了。

⚠️ 注意：<font color=red>三斜线指令 **必须放在文件的最顶端**，**三斜线指令的前面只允许出现单行或多行注释**</font>。

###### 三斜线指令 - 依赖一个全局变量的声明文件

在另一个场景下，<font color=red>如果 **需要依赖** 一个全局变量的声明文件</font>，由于全局变量不支持通过 `import` 导入，当然也就必须使用三斜线指令来引入了：

```ts
// types/node-plugin/index.d.ts

/// <reference types="node" />
export function foo(p: NodeJS.Process): string;
```

```ts
// src/index.ts
import { foo } from 'node-plugin';

foo(global.process);
```

在上面的例子中，通过三斜线指引入了 `node` 的类型，然后在声明文件中使用了 `NodeJS.Process` 这个类型。最后在使用到 `foo` 的时候，传入了 `node` 中的全局变量 `process`。

<font color=dodgerBlue>由于引入的 `node` 中的类型都是全局变量的类型，是没有办法通过 `import` 来导入的</font>，所以这种场景下也只能通过三斜线指令来引入了。

以上两种使用场景下，都是由于需要书写或需要依赖全局变量的声明文件，所以必须使用三斜线指令。在其他的一些不是必要使用三斜线指令的情况下，就都需要使用 `import` 来导入。

###### 三斜线指令 - 拆分声明文件

<font color=LightSeaGreen>全局变量的声明文件太大时，可以通过拆分为多个文件，然后在一个入口文件中将它们一一引入，来提高代码的可维护性</font>。比如 `jQuery` 的声明文件就是这样的：

```ts
// node_modules/@types/jquery/index.d.ts

/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />
export = jQuery;
```

其中用到了 <font color=dodgerBlue>`types` 和 `path` 两种不同的指令</font>，它们的<font color=dodgerBlue>区别是</font>：<font color=red>**`types` 用于声明对另一个库的依赖**</font>，而 <font color=red>**`path` 用于声明对另一个文件的依赖**</font>。

上例中，`sizzle` 是与 `jquery` 平行的另一个库，所以需要使用 `types="sizzle"` 来声明对它的依赖。而其他的三斜线指令就是将 `jquery` 的声明拆分到不同的文件中了，然后在这个入口文件中使用 `path="foo"` 将它们一一引入。

###### 其他三斜线指令

除了这两种三斜线指令之外，还有其他的三斜线指令，比如 `/// <reference no-default-lib="true"/> ` , `/// <amd-module />` 等，但它们都是废弃的语法，故这里就不介绍了，详情可见[官网](http://www.typescriptlang.org/docs/handbook/triple-slash-directives.html)。

##### 自动生成声明文件

如果库的源码本身就是由 ts 写的，那么在使用 `tsc` 脚本将 ts 编译为 js 的时候，添加 `declaration` 选项，就可以同时也生成 `.d.ts` 声明文件了。

<font color=red>可以在命令行中添加 `--declaration`（简写 `-d`），或者在 `tsconfig.json` 中添加 `declaration` 选项</font>。以 `tsconfig.json` 为例：

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "outDir": "lib",
        "declaration": true,
    }
}
```

上例中我们添加了 `outDir` 选项，将 ts 文件的编译结果输出到 `lib` 目录下，然后添加了 `declaration` 选项，设置为 `true`，表示将会由 ts 文件自动生成 `.d.ts` 声明文件，也会输出到 `lib` 目录下。

运行 `tsc` 之后，目录结构如下：

```autoit
/path/to/project
├── lib
|  ├── bar
|  |  ├── index.d.ts
|  |  └── index.js
|  ├── index.d.ts
|  └── index.js
├── src
|  ├── bar
|  |  └── index.ts
|  └── index.ts
├── package.json
└── tsconfig.json
```

在这个例子中，`src` 目录下有两个 ts 文件，分别是 `src/index.ts` 和 `src/bar/index.ts`，它们被编译到 `lib` 目录下的同时，也会生成对应的两个声明文件 `lib/index.d.ts` 和 `lib/bar/index.d.ts`。它们的内容分别是：

```ts
// src/index.ts
export * from './bar';

export default function foo() {
    return 'foo';
}
```

```ts
// src/bar/index.ts
export function bar() {
    return 'bar';
}
```

```ts
// lib/index.d.ts
export * from './bar';
export default function foo(): string;
```

```ts
// lib/bar/index.d.ts
export declare function bar(): string;
```

可见，自动生成的声明文件基本保持了源码的结构，而将具体实现去掉了，生成了对应的类型声明。

使用 `tsc` 自动生成声明文件时，每个 ts 文件都会对应一个 `.d.ts` 声明文件。这样的好处是，使用方不仅可以在使用 `import foo from 'foo'` 导入默认的模块时获得类型提示，还可以在使用 `import bar from 'foo/lib/bar'` 导入一个子模块时，也获得对应的类型提示。

除了 `declaration` 选项之外，还有几个选项也与自动生成声明文件有关，这里只简单列举出来，不做详细演示了：

- `declarationDir` 设置生成 `.d.ts` 文件的目录
- `declarationMap` 对每个 `.d.ts` 文件，都生成对应的 `.d.ts.map`（sourcemap）文件
- `emitDeclarationOnly` 仅生成 `.d.ts` 文件，不生成 `.js` 文件

#### 发布声明文件

为一个库写好了声明文件之后，下一步就是将它发布出去了。<font color=dodgerBlue>有两种方案</font>：

1. 将声明文件和源码放在一起
2. 将声明文件发布到 `@types` 下

<font color=red>这两种方案中优先选择第一种方案</font>。<font color=LightSeaGreen>保持声明文件与源码在一起，使用时就不需要额外增加单独的声明文件库的依赖了</font>，而且<font color=LightSeaGreen>**也能保证声明文件的版本与源码的版本保持一致**</font>。

<font color=dodgerBlue>仅当我们在给别人的仓库添加类型声明文件，但原作者不愿意合并 pull request 时</font>，<font color=LightSeaGreen>才需要使用第二种方案，将声明文件发布到 `@types` 下</font>。

##### 将声明文件和源码放在一起

<font color=dodgerBlue>如果声明文件是通过 `tsc` 自动生成的</font>，<font color=LightSeaGreen>无需做任何其他配置，只需把编译好的文件也发布到 npm 上</font>，使用方便可获取类型提示了。

<font color=dodgerBlue>**如果是手动写的声明文件**</font>，那么<font color=dodgerBlue>需要满足以下条件 <font size=4>**之一**</font>，才能被正确的识别</font>：

- 在 `package.json` 中 `types` 或 `typings` 字段指定一个类型声明文件地址
- 在项目根目录下，编写一个 `index.d.ts` 文件
- 针对入口文件（ `package.json` 中的 `main` 字段指定的入口文件），编写一个同名不同后缀的 `.d.ts` 文件

第一种方式是给 `package.json` 中的 `types` 或 `typings` 字段指定一个类型声明文件地址。比如：

```json
{
    "name": "foo",
    "version": "1.0.0",
    "main": "lib/index.js",
    "types": "foo.d.ts",
}
```

<font color=dodgerBlue>**指定了 `types` 为 `foo.d.ts` 之后**</font>，<font color=LightSeaGreen>导入此库的时候，就会去找 `foo.d.ts` 作为此库的类型声明文件了</font>。

`typings` 与 `types` 一样，只是另一种写法。

<font color=dodgerBlue>**如果没有指定 `types` 或 `typings`**</font>，那么就<font color=LightSeaGreen>会在根目录下寻找 `index.d.ts` 文件</font>，将它视为此库的类型声明文件。

<font color=dodgerBlue>**如果没有找到 `index.d.ts` 文件**</font>，那么就<font color=LightSeaGreen>会寻找入口文件</font>（ <font color=red>`package.json` 中的 `main` 字段指定的入口文件</font>）是否存在对应同名不同后缀的 `.d.ts` 文件。

比如 `package.json` 是这样时：

```json
{
    "name": "foo",
    "version": "1.0.0",
    "main": "lib/index.js"
}
```

就会先识别 `package.json` 中是否存在 `types` 或 `typings` 字段。发现不存在，那么就会寻找是否存在 `index.d.ts` 文件。如果还是不存在，那么就会寻找是否存在 `lib/index.d.ts` 文件。<font color=dodgerBlue>**如果 `lib/index.d.ts` 都不存在的话**</font>，就<font color=red>会被认为是一个没有提供类型声明文件的库了</font>。

有的库为了支持导入子模块，比如 `import bar from 'foo/lib/bar'`，就需要额外再编写一个类型声明文件 `lib/bar.d.ts` 或者 `lib/bar/index.d.ts`，这与自动生成声明文件类似，一个库中同时包含了多个类型声明文件。

##### 将声明文件发布到 `@types` 下

如果我们是在给别人的仓库添加类型声明文件，但原作者不愿意合并 pull request，那么就需要将声明文件发布到 `@types` 下。

与普通的 npm 模块不同，`@types` 是统一由 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 管理的。<font color=LightSeaGreen>要将声明文件发布到 `@types` 下，就需要给 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 创建一个 pull-request</font>，其中包含了类型声明文件，测试代码，以及 `tsconfig.json` 等。

<font color=LightSeaGreen>pull-request 需要符合它们的规范，并且通过测试，才能被合并</font>；稍后就会被自动发布到 `@types` 下。

在 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 中创建一个新的类型声明，需要用到一些工具，[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 的文档中已经有了[详细的介绍](https://github.com/DefinitelyTyped/DefinitelyTyped#create-a-new-package)，这里就不赘述了，以官方文档为准。

> ⚠️ 值得注意的是：[ts handbook - module](https://www.typescriptlang.org/docs/handbook/modules.html) 介绍的很多的方法，比如 `export =`  在 [ts handbook V2 - module](https://www.typescriptlang.org/docs/handbook/2/modules.html) 都被省略了；原因想必是很多兼容的方法，现在都已经过时了，所以就不再介绍



## 《TypeScript 编程》笔记



#### TS Compiler 处理

##### 一般编译器处理流程

That text is parsed by a special program called a compiler, which <font color=red>transforms it into an abstract syntax tree (AST)</font>, a data structure that <font color=LightSeaGreen>ignores things like whitespace, comments, and where you stand on the tabs versus spaces debate</font>. The compiler then <font color=fuchsia>converts that AST to a lower-level representation called **bytecode**</font>. You <font color=fuchsia>can feed that bytecode into another program called a runtime to evaluate it and get a result</font>. So when you run a program, what you’re really doing is telling the runtime to evaluate the bytecode generated by the compiler from the AST parsed from your source code. The details vary, but for most languages this is an accurate high-level view.

<font color=dodgerBlue>Once again, the steps are:</font>

1. “Program is parsed into an AST.

2. AST is compiled to bytecode.

3. Bytecode is evaluated by the runtime.”

##### TSC 处理流程

<font color=dodgerBlue>**Where TypeScript is special is**</font> that <font color=red>instead of compiling straight to bytecode, TypeScript compiles to… JavaScript code!</font> You then run that JavaScript code like you normally would—in your browser, or with NodeJS, or by hand with a paper and pen (for anyone reading this after the machine uprising has begun).

At this point you may be thinking: “Wait! In the last chapter you said TypeScript makes my code safer! When does that happen?”

Great question. <font color=dodgerBlue>I actually skipped over a crucial step</font>: <font color=red>after the TypeScript Compiler generates an AST for your program</font>—but <font color=fuchsia>**before it emits code—it typechecks your code**</font>.

> **Typechecker** : A special program that verifies that your code is typesafe.

This typechecking is the magic behind TypeScript. It’s how TypeScript makes sure that your program works as you expect, that there aren’t obvious mistakes, and that the cute barista（咖啡馆服务生） across the street really will call you back when they said they would. (Don’t worry, they’re probably just busy.)

So if we include typechecking and JavaScript emission, the process of compiling TypeScript now looks roughly like Figure 2-1:

<img src="https://s2.loli.net/2022/09/19/Iokr6JHzVub8nAh.png" alt="Image from Programming TypeScript Making Your JavaScript Applications Scale (Boris Cherny), page 26" style="zoom:70%;" />

摘自：Chapter2 - The Compiler



#### TS 类型结构关系图

<img src="https://s2.loli.net/2022/05/07/AQ2daqsipyXJcYL.png" alt="图片来自 Programming TypeScript Making Your JavaScript Applications Scale (Boris Cherny) (z-lib.org)，第 38 页" style="zoom:43%;" />

摘自：Chapter 3 ( 👀 overview at the begin of chapter)



#### 协变 & 逆变

在 TypeScript 中，每个复杂类型的成员都会进行协变，包括 对象、类、数组 和 <font color=FF0000>函数的返回类型</font>。不过有个例外：<font color=FF0000>**函数的参数类型进行逆变**</font>。

如果 函数A的 参数数量 小于或等于 函数B的参数数量，而且**满足下述条件**，那么 <font color=FF0000>**函数A 是 函数B 的子类型**</font>：

1. 函数A 的 <font color=FF0000>this 类型</font>未指定，或者 >: 函数B 的 this 类型

2. 函数A 的 <font color=FF0000>各个参数的类型</font> >: 函数B 的相应参数。

3. 函数A 的 <font color=FF0000>返回类型</font> <: 函数B 的返回类型。

其中：

- A <: B 指 “A类型 是 B类型的 子类型，或者为 同种类型 ”
- A >: B 指 “ A类型 是 B类型的 超类型，或者为 同种类型 ”。



## 《深入理解 TypeScript 》笔记

🔗 链接：[深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/)

#### infer

// todo



#### 类型断言

TypeScript <font color=red>允许你覆盖它的推断，并且能以你任何你想要的方式分析它</font>，<font color=dodgerBlue>这种机制被称为「类型断言」</font>。TypeScript <font color=fuchsia>类型断言用来告诉编译器你比它更了解这个类型，并且它不应该再发出错误</font>。

类型断言的一个常见用例是当你从 JavaScript 迁移到 TypeScript 时：

```ts
const foo = {};
foo.bar = 123;     // Error: 'bar' 属性不存在于 ‘{}’
foo.bas = 'hello'; // Error: 'bas' 属性不存在于 '{}'
```

<font color=red>这里的代码发出了错误警告，因为 `foo` 的类型推断为 `{}` ，即没有属性的对象</font>。因此，你不能在它的属性上添加 `bar` 或 `bas`，<font color=dodgerBlue>你可以通过 **类型断言** 来避免此问题</font>：

```ts
interface Foo {
  bar: number;
  bas: string;
}

const foo = {} as Foo; // 👀
foo.bar = 123;
foo.bas = 'hello';
```

##### `as foo` 与 `<foo>`

最初的断言语法如下所示：

```ts
let foo: any;
let bar = <string>foo; // 现在 bar 的类型是 'string'
```

然而，<font color=red>当你在 JSX 中使用 `<foo>` 的断言语法时，这会与 JSX 的语法存在歧义</font>：

```jsx
let foo = <string>bar;</string>;
```

因此，<font color=fuchsia>为了一致性，我们建议你使用 `as foo` 的语法来为类型断言</font>。

> 💡 这的注意的是：在 TSX 中也是可以使用泛型的，`<T>` 的语法同样与 TSX 语法会引发歧义，所以可以使用 `<T,>` 来纠正泛型（转义？）。代码示例如下：
>
> ```tsx
> const fn = <T,>(param: T) => {
>   console.log(params)
> }
> ```
>
> 不过，经过实验发现：通过这种方法依然不能在 tsx 文件中 “纠正断言”
>
> <img src="https://s2.loli.net/2024/10/25/nBxur8LmVOtIeEH.png" alt="image-20241025100744069" style="zoom:50%;" />
>
> 学习自：学习自：[小满zs - react最新教程 - React基础篇(tsx语法全解)](https://www.bilibili.com/video/BV1mcpPeMETt&p=3&t=406)

##### 类型断言与类型转换

<font color=dodgerBlue>它之所以不被称为「类型转换」</font>，是因为<font color=red>转换通常意味着某种 **运行时** 的支持</font>（👀 这个概念没有听过 ... ）。但是，<font color=fuchsia>类型断言 **纯粹是一个编译时语法**</font>，同时，<font color=red>它也是一种为编译器提供关于如何分析代码的方法</font>。

##### 类型断言被认为是有害的

<font color=red>在很多情景下，断言能让你更容易的从遗留项目中迁移</font>（甚至将其他代码粘贴复制到你的项目中），然而，<font color=dodgerBlue>你应该小心谨慎的使用断言</font>。让我们用最初的代码作为示例，如果你没有按约定添加属性，TypeScript 编译器并不会对此发出错误警告：

```ts
interface Foo {
  bar: number;
  bas: string;
}

const foo = {} as Foo;
// ahhh, 忘记了什么？
```

另外一个常见的想法是使用类型断言来提供代码的提示：

```ts
interface Foo {
  bar: number;
  bas: string;
}

const foo = <Foo>{
  // 编译器将会提供关于 Foo 属性的代码提示，但是开发人员也很容易忘记添加所有的属性
  // 同样，⚠️ **如果 Foo 被重构，这段代码也可能被破坏**（例如，一个新的属性被添加）。
};
```

这也会存在一个同样的问题，如果你忘记了某个属性，编译器同样也不会发出错误警告。使用一种更好的方式：

```ts
interface Foo {
  bar: number;
  bas: string;
}

const foo: Foo = {
  // 编译器将会提供 Foo 属性的代码提示
};
```

在某些情景下，你可能需要创建一个临时的变量，但至少，你不会使用一个承诺（可能是假的），而是依靠类型推断来检查你的代码。

##### 双重断言

<font color=dodgerBlue>类型断言，尽管我们已经证明了它并不是那么安全，但它也还是有用武之地</font>。如下一个非常实用的例子所示，当使用者了解传入参数更具体的类型时，类型断言能按预期工作：

```ts
function handler(event: Event) {
  const mouseEvent = event as MouseEvent;
}
```

然而，<font color=red>如下例子中的代码将会报错，尽管使用者已经使用了类型断言</font>：

```ts
function handler(event: Event) {
  const element = event as HTMLElement; // Error: 'Event' 和 'HTMLElement' 中的任何一个都不能赋值给另外一个
}
```

<font color=dodgerBlue>如果你仍然想使用那个类型</font>，你<font color=fuchsia>可以使用双重断言</font>。<font color=fuchsia>首先断言成兼容所有类型的 `any`</font> ，编译器将不会报错：

```ts
function handler(event: Event) {
  const element = (event as any) as HTMLElement; // ok
}
```

###### TypeScript 是怎么确定单个断言是否足够

<font color=fuchsia>当 `S` 类型是 `T` 类型的子集，或者 `T` 类型是 `S` 类型的子集时，`S` 能被成功断言成 `T`</font> 。这是为了在进行类型断言时提供额外的安全性，完全毫无根据的断言是危险的，如果你想这么做，你可以使用 `any` 。



#### 类型保护

<font color=fuchsia>类型保护允许你 **使用更小范围下的对象类型**</font>。

##### typeof

TypeScript 熟知 JavaScript 中 `instanceof` 和 `typeof` 运算符的用法。<font color=red>如果你在一个条件块中使用这些，TypeScript 将会推导出在条件块中的的变量类型</font>。如下例所示，TypeScript 将会辨别 `string` 上是否存在特定的函数，以及是否发生了拼写错误：

```ts
function doSome(x: number | string) {
  if (typeof x === 'string') {
    // 在这个块中，TypeScript 知道 `x` 的类型必须是 `string`
    console.log(x.subtr(1)); // Error: 'subtr' 方法并没有存在于 `string` 上
    console.log(x.substr(1)); // ok
  }
  x.substr(1); // Error: 无法保证 `x` 是 `string` 类型
}
```

##### instanceof

<font color=dodgerBlue>这有一个关于 `class` 和 `instanceof` 的例子：</font>

```ts
class Foo { foo = 123; common = '123'; }
class Bar { bar = 123; common = '123'; }

function doStuff(arg: Foo | Bar) {
  if (arg instanceof Foo) { // 👀 使用了 instanceof
    console.log(arg.foo);   // ok
    console.log(arg.bar);   // Error
  }
  if (arg instanceof Bar) {
    console.log(arg.foo);   // Error
    console.log(arg.bar);   // ok
  }
}

doStuff(new Foo());
doStuff(new Bar());
```

<font color=fuchsia>TypeScript 甚至能够理解 `else`</font> 。当你使用 `if` 来缩小类型时，TypeScript 知道在其他块中的类型并不是 `if` 中的类型：

```ts
class Foo { foo = 123; }
class Bar { bar = 123; }

function doStuff(arg: Foo | Bar) {
  if (arg instanceof Foo) {
    console.log(arg.foo); // ok
    console.log(arg.bar); // Error
  } else {                // 👀 使用 else
    // 这个块中，一定是 'Bar'
    console.log(arg.foo); // Error
    console.log(arg.bar); // ok
  }
}

doStuff(new Foo());
doStuff(new Bar());
```

##### in

<font color=LightSeaGreen>`in` 操作符可以安全的检查一个对象上是否存在一个属性</font>，<font color=red>它通常也被作为 **类型保护** 使用</font>：

```ts
interface A { x: number; }
interface B { y: string; }

function doStuff(q: A | B) {
  if('x' in q) { // 👀
    // q: A
  }
  else {
    // q: B
  }
}
```

##### 字面量类型保护

当你在联合类型里使用字面量类型时，你可以检查它们是否有区别：

```ts
type Foo = {
  kind: 'foo'; // 字面量类型 👀
  foo: number;
};
type Bar = {
  kind: 'bar'; // 字面量类型 👀
  bar: number;
};

function doStuff(arg: Foo | Bar) {
  if (arg.kind === 'foo') { // 👀 使用字面量
    console.log(arg.foo);   // ok
    console.log(arg.bar);   // Error
  } else {
    // 一定是 Bar
    console.log(arg.foo);   // Error
    console.log(arg.bar);   // ok
  }
}
```

##### 使用定义的类型保护

JavaScript 并没有内置非常丰富的、运行时的自我检查机制。<font color=LightSeaGreen>当你在使用普通的 JavaScript 对象时</font>（使用结构类型，更有益处），你甚至无法访问 `instanceof` 和 `typeof` 。<font color=fuchsia>在这种情景下，你可以创建 ***用户自定义的类型保护函数***</font>，这仅仅<font color=fuchsia>是一个返回值为类似于 `someArgumentName is SomeType` 的函数</font>，如下：

> 👀  `is` 有点类似于 Py 中的 `is` 关键字。另外，根据 [TypeScript类型系统（5/5）-类型编程篇 - Ethan Ruan的文章 - 知乎](https://zhuanlan.zhihu.com/p/145679056) 中的说法：`is` 是 “断言返回布尔类型”；参考下面的使用，确实是这样。

```ts
// 仅仅是一个 interface
interface Foo { foo: number; common: string; }
interface Bar { bar: number; common: string; }

// **用户自己定义的类型保护！**
function isFoo(arg: Foo | Bar): arg is Foo { // 👀 arg is Foo，返回值是布尔类型
  return (arg as Foo).foo !== undefined;
}

// 用户自己定义的类型保护使用用例：
function doStuff(arg: Foo | Bar) {
  if (isFoo(arg)) {       // 👀 自定义的类型保护返回布尔类型
    console.log(arg.foo); // ok
    console.log(arg.bar); // Error
  } else {
    console.log(arg.foo); // Error
    console.log(arg.bar); // ok
  }
}

doStuff({ foo: 123, common: '123' });
doStuff({ bar: 123, common: '123' });
```

关于上面这段代码有点没看懂，于是问了下 GPT，得到如下回复：

<img src="https://s2.loli.net/2024/06/02/jEAK4VOeauhnHoT.png" alt="image-20240602134907863" style="zoom:48%;" />

其中第二段，很好的解答了这个问题：

> `arg is Foo` 告诉 TypeScript 编译器：“如果函数 `isFoo` 返回 `true`，则参数 `arg` 应被视为 `Foo` 类型；如果返回 `false`，则 `arg` 不应被视为 `Foo` 类型”。这样<font color=red>**在编译期间** TypeScript 就能根据 `isFoo` 函数的返回结果，推断出 `arg` 的确切类型，从而能在代码中安全地访问 `arg` 的相应属性和方法</font>。



## 神光《 TypeScript 类型体操通关秘籍》笔记

🔗 链接：[TypeScript 类型体操通关秘籍](https://juejin.cn/book/7047524421182947366/section)

> 👀 除了本小册的内容，神光还写了一些文章比如 [这几个 TypeScript 类型，90% 的人说不出原因](https://juejin.cn/post/7066745410194243597) 、[我读 Typescript 源码的秘诀都在这里了](https://juejin.cn/post/7015567717876908063) 也很不错，且并 **不记得** 小册上包含这些这些内容（也可能是自己还没有读到... ），建议阅读和做笔记。



#### 类型是什么

类型具体点来说就是指 <font color=FF0000>number、boolean、string 等**基础类型**</font> 和 <font color=FF0000>Object、Function 等**复合类型**</font>，它们<font color=FF0000>是编程语言提供的对不同内容的抽象</font>：

- **不同类型变量占据的 <font color=FF0000>内存大小不同</font>** ：boolean 类型的变量只会分配 1 个字节的内存，而 <font color=FF0000>**number 类型的变量则会分配 8 个字节的内存**</font>（👀 因为 JS / TS 中数字类型，不区分 int、float、double 等，所以 number 被分配了 8 个字节 ），<font color=lightSeaGreen>给变量声明了不同的类型就代表了会占据不同的内存空间</font>。
- **不同类型变量 <font color=FF0000>可做的操作不同</font>** ：number 类型可以做加减乘除等运算，boolean 就不可以；<font color=lightSeaGreen>复合类型中不同类型的对象可用的方法不同</font>，比如 Date 和 RegExp，变量的类型不同代表可以对该变量做的操作就不同。

我们  <font color=FF0000>知道了什么是类型，那自然可以想到类型和所做的操作要匹配才行，<font size=4>**这就是为什么要做类型检查**</font></font>。



#### 类型检查

<font color=FF0000>**如果能保证对某种类型只做该类型允许的操作，这就叫做 「类型安全」**</font>。比如你对 boolean 做加减乘除，这就是类型不安全，你对 Date 对象调用 exec 方法，这就是类型不安全。反之，就是类型安全。所以，<font color=FF0000>**类型检查是为了保证类型安全的**</font>。

<img src="https://s2.loli.net/2022/04/30/VkhC3MlB8J2Uxpo.png" alt="img" style="zoom:55%;" />

##### 动态类型检查 & 静态类型检查

类型检查可以 **在运行时** 做，也可以 **运行之前的编译期** 做。这是两种不同的类型，前者叫做 **动态类型检查**，后者叫做 **静态类型检查**。两种类型检查各有优缺点。

- **动态类型检查** ：<font color=FF0000>**在源码中不保留类型信息**，对某个变量赋什么值、做什么操作都是允许的，写代码很灵活</font>。但这也埋下了类型不安全的隐患，比如对 string 做了乘除，对 Date 对象调用了 exec 方法，这些都是运行时才能检查出来的错误。

  其中，最常见的错误应该是 `null is not an object` 、`undefined is not a function` 之类的了，写代码时没发现类型不匹配，到了运行的时候才发现，就会有很多这种报错。

  <font color=lightSeaGreen>所以，动态类型虽然代码写起来简单，但代码中很容易藏着一些类型不匹配的隐患</font>。

  <img src="https://s2.loli.net/2022/04/30/nfgwztehkpbEX7C.png" alt="img" style="zoom:55%;" />

- **静态类型检查：**<font color=FF0000>**在源码中保留类型信息**，声明变量要指定类型，对变量做的操作要和类型匹配，有专门的编译器在编译期间做检查</font>。

  静态类型给写代码增加了一些难度，因为你 **除了要考虑代码要表达的逻辑之外**，<font color=FF0000>还要考虑 **类型逻辑**：变量是什么类型的、是不是匹配、要不要做类型转换等</font>。

  不过，静态类型也消除了类型不安全的隐患，因为在编译期间就做了类型检查，就不会出现对 string 做了乘除，调用了 Date 的 exec 方法这类问题。

  <font color=lightSeaGreen>所以，静态类型虽然代码写起来要考虑的问题多一些，会复杂一些，但是却消除了代码中潜藏类型不安全问题的可能</font>。

  <img src="https://s2.loli.net/2022/04/30/XVKUm1fPwirLMnh.png" alt="img" style="zoom:55%;" />



#### TS 给 JS 添加的「静态类型系统」

<font color=FF0000>**TypeScript 给 JavaScript 添加了一套「静态类型系统」**，从动态类型语言变成了静态类型语言</font>，可以在编译期间做类型检查，提前发现一些类型安全问题。

<img src="https://s2.loli.net/2022/04/30/gL815ZfivRCYjut.png" alt="img" style="zoom:55%;" />

而且，因为代码中添加了静态类型，也就可以配合编辑器来实现更好的提示、重构等，这是额外的好处。

<img src="https://s2.loli.net/2022/05/07/NY4Hiws8VWA362L.png" alt="image-20220507011037158" style="zoom:50%;" />



#### 类型体操 & 支持类型编程的类型系统

<font color=FF0000 size=4>**类型系统**</font> 不止 TypeScript 有，别的语言 Java、C++ 等都有，<font color=FF0000>为什么 TypeScript 的类型编程被叫做「类型体操」，而其他语言没有呢？</font>

<font color=FF0000>TypeScript 给 JavaScript 增加了一套静态类型系统，**通过 TS Compiler 编译为 JS**</font> （ 👀 即 TSC ），<font color=FF0000 size=4>**编译的过程做类型检查**</font>。它并没有改变 JavaScript 的语法，只是在 JS 的基础上添加了类型语法，所以被叫做 JavaScript 的超集。

##### 类型系统的分类

静态类型编程语言都有自己的类型系统，从简单到复杂可以分为 3 类：

- **简单类型系统**：变量、函数、类等都可以声明类型，编译器会基于声明的类型做类型检查，类型不匹配时会报错。这是最基础的类型系统，能保证类型安全，但有些死板。

  比如一个 add 函数既可以做整数加法、又可以做浮点数加法，却需要声明两个函数（**注：**下文原文中代码块标注的语言是 C，而直到 C11，C 都没有支持「函数重载」；所以应该是 cpp ）：

  ```cpp
  int add(int a, int b) {
      return a + b;
  }
  
  double add(double a, double b) {
      return a + b;
  }
  ```

  这个问题的解决思路很容易想到：<font color=FF0000>如果 **类型能传参数** 就好了，传入 int 就是整数加法，传入 double 就是浮点数加法</font>。所以，就有了第二种类型系统。

- **<font color=FF0000>支持泛型</font>的类型系统**：泛型的英文是 Generic Type，通用的类型；它可以代表任何一种类型，也叫做<font color=FF0000 size=4>**「类型参数」**</font>（👀 这个说法下面有更多提到）

  它给类型系统增加了一些灵活性，在整体比较固定，部分变量的类型有变化的情况下，可以减少很多重复代码。比如上面的 add 函数，有了泛型之后就可以这样写：

  ```cpp
  T add<T>(T a, T b) {
      return a + b;
  }
  
  add(1, 2);
  add(1.111, 2.2222);
  ```

  Java 就是这种类型系统。如果你看过 Java 代码，你会发现泛型用的特别多，这确实是一个很好的增加类型系统灵活性的特性。

  但是，<font color=FF0000 size=4>这种类型系统的灵活性对于 JavaScript 来说还不够，**因为 JavaScript 太过灵活了**</font>。比如，<font color=FF0000>在 Java 里，对象都是由类 new 出来的，你不能凭空创建对象</font>；但是 <font color=FF0000>**JavaScript 却可以，它支持对象字面量**</font>。

  那如果是一个 **返回对象某个属性值** 的函数，类型该怎么写呢？

  ```typescript
  function getPropValue<T>(obj: T, key): key对应的属性值类型 {
      return obj[key];
  }
  ```

  <font color=FF0000>好像拿到了 T，也不能拿到它的属性和属性值，如果能对类型参数 T 做一些逻辑处理就好了</font>。所以，就有了第三种类型系统。

- **<font color=FF0000>支持类型编程</font>的类型系统**

  <font color=LightSeaGreen>在 Java 里面，**拿到了对象的类型就能找到它的类，进一步拿到各种信息**，所以类型系统支持泛型就足够了</font>。但 <font color=FF0000>在 JavaScript 里面，对象可以字面量的方式创建，还可以灵活的增删属性，**拿到对象并不能确定什么，所以要支持对传入的类型参数做进一步的处理**</font>。

  <font color=red>**对传入的类型参数（泛型）做各种逻辑运算，产生新的类型，这就是类型编程。**</font>

  比如上面那个 `getProps` 的函数，类型可以这样写：

  ```ts
  function getPropValue<
      T extends object, // 注：类型参数 T 必须是对象。
      Key extends keyof T // 注：类型参数 Key 必须是 T 的成员
  >(obj: T, key: Key): T[Key] {
      return obj[key];
  }
  ```

  这里的 `keyof T`、`T[Key]` 就是对 “ 类型参数 T ” 的类型运算。

  > 💡 这里 `keyof` 操作符 会 <font color=FF0000>**提取出 “对象类型” 里的所有 key** 然后 <font size=4>**得到一个「联合类型」**</font></font>。
  >
  > 学习自：[TypeScript 类型编程](https://segmentfault.com/a/1190000040247980)
  >
  > 💡 另外，相当值得注意的是：在很多情况下，在 vscode 中 `keyof MyType`  的类型预览也只是 `keyof MyType` ，<font color=red>**需要加上 `& {}` 才能在预览中见到真正的类型**</font>。示例如下：
  >
  > <img src="https://s2.loli.net/2024/10/26/8EI1Qrjv5z2ZcGb.png" alt="image-20241026235154113" style="zoom:50%;" />
  >
  > 上面的代码的含义是：取类型中所有属性名字，获得对应字面量类型的联合类型。
  >
  > > ⚠️ 24/11/18 补充：在工作中发现把 `keyof` 的语法忘得一干二净，以至于在有对应场景的时候，完全想不起来。另外，对上面的内容做一下扩展：
  > >
  > > ```ts
  > > type GetKeysUnion<T extends Record<string, any>> = keyof T & {}
  > > ```
  >
  > 除了上面的用法，还有另一个种用法，虽然看起没什么，但是确实第一次见...
  >
  > ```ts
  > interface Foo {
  >   propA: number;
  >   propB: boolean;
  >   propC: string;
  > }
  > 
  > type PropTypeUnion = Foo[keyof Foo];
  > ```
  >
  > 作用是：取所有属性的类型联合，效果如下：
  >
  > <img src="https://s2.loli.net/2024/10/26/OM1AoGE4yhUwdna.png" alt="image-20241026235915983" style="zoom:50%;" />
  >
  > 对上面的写法做一个扩展：
  >
  > ```ts
  > type GetKeysUnion<T extends Record<string, any>> = T[keyof T]
  > ```
  >
  > **官网文档截图**
  >
  > <img src="https://s2.loli.net/2023/03/15/82OxcZy5wQvmUYf.png" alt="image-20230315153748488" style="zoom:45%;" />
  >
  > 文档地址：[TS Doc - Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)
  
  <font color=LightSeaGreen>TypeScript 的类型系统就是第三种，支持对类型参数做各种逻辑处理，可以写很复杂的类型逻辑</font>。

##### TS 的类型系统

<font color=FF0000>**TypeScript 的类型系统是<font size=4>「图灵完备」的</font>，也就是能描述各种可计算逻辑**</font>。<font color=LightSeaGreen>简单点来理解就是 循环、条件 等各种 JS 里面有的语法它都有，JS 能写的逻辑它都能写</font>。

“对类型参数的编程” 是 TypeScript 类型系统最强大的部分，可以实现各种复杂的类型计算逻辑，是它的优点。但同时也被认为是它的缺点，因为除了业务逻辑外还要写很多类型逻辑。不过，我倒是觉得这种复杂度是不可避免的，因为 JS 本身足够灵活，要准确定义类型那类型系统必然也要设计的足够灵活。

<font color=FF0000>是不是感觉 TypeScript 类型系统挺复杂的？确实，不然大家也不会把 TS 的类型编程戏称为「**类型体操**」了</font>。

> ##### 💡 补充
>
> > TypeScript 的类型系统乍一看挺唬人的，东西贼多，但是你往深了看，其实最主要就是<font color=red>在标注一段代码在运行时的行为，然后让一段代码推导另一段代码</font>。
> >
> > 摘自：[TypeScript 的类型系统是否太过复杂？ - Hugo的回答 - 知乎](https://www.zhihu.com/question/348443486/answer/2949821107)



#### TS 类型系统中的类型

静态类型系统的目的是把类型检查从运行时提前到编译时，那 TS 类型系统中肯定要把 JS 的运行时类型拿过来，也就是 number、boolean、string、object、bigint、symbol、undefined、null 这些类型，还有就是它们的包装类型 Number、Boolean、String、Object、Symbol。

这些很容易理解，给 JS 添加静态类型，总没有必要重新造一套基础类型吧，直接复用 JS 的基础类型就行。

复合类型方面，JS 有 class、Array，这些 TypeScript 类型系统也都支持，但是又多加了三种类型：元组 ( Tuple )、接口 ( Interface )、枚举 ( Enum ) 。

##### 元组

元组 ( Tuple ) 就是 <font color=FF0000>元素个数 和 类型 **固定的** **数组类型**</font>：

```typescript
type Tuple = [number, string];
```

> 👀 初次接触「元组」这个名词是在 Py，而 TS 中「元组」概念和 Py 不一样。另外，TS 的元组的写法和 JS 中的数组一样；而 TS 对数组的定义是：数组类型是指任意多个同一类型的元素构成的（这个说法下面有说 [[#重新构造做变换#Push]] ）

##### 接口

接口 ( Interface ) 可以描述函数、对象、<font color=FF0000 size=4>**构造器**</font> 的结构

###### 对象

```typescript
interface IPerson {
    name: string;
    age: number;
}

class Person implements IPerson {
    name: string;
    age: number;
}

const obj: IPerson = {
    name: 'guang',
    age: 18
}
```

###### 函数

```typescript
interface SayHello {
    (name: string): string;
}

const func: SayHello = (name: string) => {
    return 'hello,' + name
}
```

###### 构造器

> 👀 这个没见过，不过 接口函数签名前多了 `new` 

```typescript
interface PersonConstructor {
  new (name: string, age: number): IPerson;
}

function createPerson(ctor: PersonConstructor): IPerson {
  return new ctor('guang', 18)
}
```

<font color=FF0000 size=4>**对象类型、class 类型**</font>（ 👀 如下面 “总之” 所说，「数组类型」也是） <font color=FF0000 size=4>**在 TypeScript 里也叫做 “索引类型”**</font>，<font color=FF0000>**也就是索引了多个元素的类型的意思**</font>（ 👀 这个概念下面会有进一步讲解）。对象可以动态添加属性，如果不知道会有什么属性，可以用 <font color=FF0000 size=4>**可索引签名**</font>（ 👀 不要和前面的「索引类型」混淆。另外，下面这个 “索引签名” 和  JS「计算属性名」设计思路是一样的）

```typescript
interface IPerson {
    [prop: string]: string | number;
}
const obj: IPerson = {};
obj.name = 'guang';
obj.age = 18;
```

总之，<font color=red>**接口可以用来描述函数、构造器、索引类型（对象、class、数组）等复合类型**</font>。

##### 枚举

枚举 ( Enum ) 是一系列值的复合：

```typescript
enum Transpiler {
    Babel = 'babel',
    Postcss = 'postcss',
    Terser = 'terser',
    Prettier = 'prettier',
    TypeScriptCompiler = 'tsc'
}

const transpiler = Transpiler.TypeScriptCompiler;
```
###### TS 枚举的反向映射
TypeScript 的枚举类型是支持 <font color=red>**“反向映射” Reverse mappings**</font> 的，也就是：可以根据 枚举值 ( enum values ) 去找到 枚举名( enum names )的。如下示例：
```ts
enum Animal {
	Cat,
	Dog,
	Mouse,
}

console.log(Animal) // { '0': 'Cat', '1': 'Dog', '2': 'Mouse', Cat: 0, Dog: 1, Mouse: 2 }
console.log(Animal[Animal.Cat]) // Cat
```
这里只是做了大概介绍，详见文档： [TS Doc - Enums # Reverse mappings](https://www.typescriptlang.org/docs/handbook/enums.html#reverse-mappings)

##### 字面量类型 ( literal types )

此外，TypeScript 还支持字面量类型，也就是<font color=FF0000>类似</font> `1111`、`'aaaa'`、`{ a: 1 }` <font color=FF0000>这种值也可以作为类型</font>

> 💡 发现这里几乎没有介绍“字面量类型”的作用，另外，2024/9/14 发现对于“字面量类型”的作用几乎遗忘，所以在此补充。
> 可以看下 [[Vue3 + TS 学习笔记#字面量类型]] 中的内容，简单而言一句话概括：<font color=red>**字面量类型必须类型和值一样**</font>

其中，<font color=dodgerBlue>**字符串的字面量类型有两种**</font>：一种是普通的字符串字面量，比如 `'aaa'`；<font color=dodgerBlue>**另一种是模版字面量**</font>，比如 `aaa${string}` ，<font color=FF0000>它的意思是以 “aaa” 开头，后面是任意 string 的字符串字面量类型</font>。

所以想要约束以某个字符串开头的字符串字面量类型时可以这样写：

<img src="https://s2.loli.net/2022/05/01/2ajXHbDgC13WLik.png" alt="image-20220501181210720" style="zoom:50%;" />

> 👀 2025/5/13 补充：自己对于 `${string}` 的用法一点印象也没有了...能想到的都是 `${GenericVariable}`。 以如下代码为例：
>
> ```ts
> const nonImageExtMap: Record<string, string> = {
>   '.doc': 'application/msword',
>   '.docx': 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
>   '.xls': 'application/vnd.ms-excel',
>   '.xlsx': 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
>   '.pdf': 'application/pdf',
> }
> ```
>
> 想对属性名进行约束，使其属性名一定以 `.` 开头，一直在想 `.${ infer ... }` ，结果写了半天也没写出...
>
> 还是问了 AI，才找到解决方法 🔗 https://g.co/gemini/share/00b457aefd4d ，然后还发现上面笔记早就做了... 解决方案如下：
>
> ```ts
> const nonImageExtMap: Record<`.${string}`, string> = {
>   '.doc': 'application/msword',
>   '.docx': 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
>   '.xls': 'application/vnd.ms-excel',
>   '.xlsx': 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
>   '.pdf': 'application/pdf',
> }
> ```

##### 四种特殊类型

还有<font color=dodgerBlue>**四种特殊的类型：void、never、any、unknown**</font>：

- **void** 代表空，可以是 null 或者 undefined，<font color=FF0000>一般是用于函数返回值</font>。

  > 👀 可以参考 [[JavaScript备忘录#undefined & null 与 “空” void]]

- **any** 是任意类型，任何类型都可以赋值给它，它也<font color=fuchsia>可以赋值给任何类型（ <font size=4>**除了 never**</font> ）</font>。

- **unknown** 是<font color=fuchsia>**未知类型**，**任何类型都可以赋值给它**，但是它 <font size=4>**不可以赋值给别的类型**</font></font>。
  
   > 💡 [[#unknown vs never#unknown]] 中做了一些补充
   >
   > 另外，根据 [any和unknown的区别？【渡一教育】](https://www.bilibili.com/video/BV12YdJY7EyJ) 中的说法：unknown 增加了 “显式编程范式” 的应用，参考如下代码：
   >
   > ```ts
   > import getSomething from 'some.js'
   > 
   > const data = getSomething()
   > const user: User = data
   > user.sayHello()
   > ```
   >
   > 当页面中，出现了一些不可控的数据获取，比如从某个 js 文件中引入并使用了 `getSomething`  ，默认情况下 `data` 会是 `any` 类型，一切都可以通过隐式转换形成；这显然会造成风险，因为 any 的存在绕过了 TS 的类型检察，如果 data 的数据类型不符合 User 的类型约束，那么将会造成错误。
   >
   > 所以可以通过 “`unknown` 不可以直接给其他类型的变量直接赋值”的特性，将 `data` 设置为 `unknown` 类型，是隐式转换没法正常运转，从而让开发者认识到自己的代码是有风险的，让开发者自主决定是否要进行这种存在风险的赋值。代码如下：
   >
   > ```ts
   > const data: unknown = getSomething()
   > const user: User = data as User // 这里是必须要加上 as 的，不加会出现警告
   > user.sayHello()
   > ```
   
- **never** <font color=FF0000>**代表不可达，比如函数抛异常的时候，返回值就是 never**</font> 

  > 👀 “异常” 的相关示例 [[#交叉：&]] 。另外，根据下面 [[#推导：infer]] 的代码可知，在类型编程时，使用 `infer ? :` 不符合条件的，也可用 never 作为类型。

  > 💡 补充
  >
  > never 是 `|` 运算的幺元，即：`x | never === x`。
  >
  > 摘自：[TypeScript 中高级应用与最佳实践](http://www.alloyteam.com/2019/07/13796/)
  >

> 💡 [[#数组类型#First]] 中有说 any 和 unknown 的区别，简单来说就是：unknown 不可给别的类型赋值，而 any 可以（除了 never ）。

**这些就是 TypeScript 类型系统中的全部类型了**，<font color=LightSeaGreen>大部分是从 JS 中迁移过来的</font>，比如基础类型、Array、class 等；<font color=LightSeaGreen>也添加了一些类型</font>，比如 枚举 ( enum ) 、接口 ( interface ) 、元组等，<font color=LightSeaGreen>还支持了字面量类型和 void、never、any、unknown 的特殊类型</font>。

> 👀 下面是一点补

##### 神光《这几个 TypeScript 类型，90% 的人说不出原因》文章的笔记

有如下类型：`Test<T>`

```ts
type Test<T> = T extends true ? 1 : 2;
```

<font color=dodgerBlue>传入的类型参数为 boolean，问 res 是？</font>

```typescript
type res = Test<boolean>;
```

res 是 `1 | 2`

<img src="https://s2.loli.net/2022/08/25/XY2bCcQxElep1i7.png" alt="image-20220825234451679" style="zoom:45%;" />

<font color=fuchsia>boolean 其实是 <font size=4>**联合类型**</font>，所以会把 true 和 false 分别传入求值，最后结果合并成联合类型</font>；所以是 `1 | 2` 。

<font color=dodgerBlue>**传入的类型参数为 any，问 res 是 ？**</font>

```ts
type res = Test<any>;
```

res 是 `1 | 2`

<img src="https://s2.loli.net/2022/08/26/qr9ujCOHslfJmyi.png" alt="image-20220826001048827" style="zoom:45%;" />

<font color=red>any **不是**联合类型</font>，这里是因为，<font color=fuchsia>**条件类型对 any 做了特殊处理：如果左边是 any，那么直接把 trueType 和 falseType 合并成联合类型返回**</font>。

<font color=dodgerBlue>**传入的类型参数为 never ，问 res 是 ？**</font>

```ts
type res = Test<never>;
```

res 是 `never` 。

<img src="https://s2.loli.net/2022/08/26/OBaiZm78qrxuySA.png" alt="image-20220826001536786" style="zoom:45%;" />

这里确实也是 TS 的特殊处理，**当条件类型左边是 never 时，直接返回 never**。

> 👀 这后面还有内容，因为考虑到是分析源码的内容，且暂时较忙；等等和 [我读 Typescript 源码的秘诀都在这里了](https://juejin.cn/post/7015567717876908063) 一起阅读 // TODO

摘自：[这几个 TypeScript 类型，90% 的人说不出原因](https://juejin.cn/post/7066745410194243597)



#### TS 类型的装饰

除了描述类型的结构外，TypeScript 的类型系统还支持描述类型的属性  ，比如是否可选 ( `?` )，是否只读 ( `readonly` ) 等：

```typescript
interface IPerson {
    readonly name: string;
    age?: number;
}

type tuple = [string, number?];
```



#### TypeScript 类型系统中的类型运算

我们知道了 TypeScript 类型系统里有哪些类型，那么 <font color=FF0000>可以对这些类型做什么类型运算呢？</font>

##### 条件：`extends ? :`

TypeScript 里的条件判断是 `extends ? :`  ，叫做条件类型 ( Conditional Type ) 。比如：

```typescript
type res = 1 extends 2 ? true : false;
```

<img src="https://s2.loli.net/2022/05/03/oJRMaOD7t63vYHT.png" alt="image-20220501183041897" style="zoom:50%;" />

这就是 TypeScript 类型系统里的 if else。

>  💡 补充
>
>  当前场景对 `extends ? :` 表示为 “是否是子类型”（学习自：[白话typescript中的【extends】和【infer】](https://juejin.cn/post/6844904146877808653)），而不是 “是否继承自” （虽然，在其他场景中也可以用来判断是否是继承关系。另外， TS 是 **结构类型系统 **），如下示例：
>
>  ```typescript
>  type Union = 1 | 2 | 3
>  type IsUnion = (1 | 2) extends Union ? true: false // type IsUnion = true
>  type ISUnion2 = 4 extends Union ? true: false // type ISUnion2 = false
>  ```

但是，上面这样的逻辑没啥意义，静态的值自己就能算出结果来，为什么要用代码去判断呢？所以，类型运算逻辑都是用来做一些动态的类型的运算的，也就是对类型参数的运算

```typescript
type isTwo<T> = T extends 2 ? true : false

type res = isTwo<1>
type res2 = isTwo<2>
```

<img src="https://s2.loli.net/2022/05/01/M4vaIWP7Hzp3VRN.png" alt="image-20220501204824969" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/05/01/JRTgaqY59E8ju6F.png" alt="image-20220501204852937" style="zoom:50%;" />

<font color=FF0000>这种类型也叫做「高级类型」。**高级类型的特点是 传入类型参数，经过一系列类型运算逻辑后，返回新的类型**</font>。

##### 推导：infer

<font color=fuchsia>**如何提取类型的一部分呢？答案是 infer**</font>

> 💡 补充
>
> infer 关键字，可以<font color=FF0000>推断一个类型变量</font>，<font color=FF0000>高效地**对类型进行「模式匹配」**</font> 。但是，这个（模式匹配出的）<font color=FF0000>**类型变量只能在 true 的分支中使用**</font>。
> 
> 学习自：[白话typescript中的【extends】和【infer】](https://juejin.cn/post/6844904146877808653)
> 
> 另外，infer 在官方文档中也有介绍，也做了笔记；见：[[#Conditional Types#infer]]

比如提取元组类型的第一个元素：

```typescript
type First<Tuple extends unknown[]> = Tuple extends [infer T,...infer R] ? T : never;
// 👀 这里的 Tuple 是范型的“类型变量”，可以起其他名字。另外，经过实验发现：类型编程中似乎没有 tuple 这个类型。
// 👀 这里有语句 infer T 和 infer R，infer variable 相当于 声明了一个变量，这个变量可以在后面使用（比如 ? 后面返回的 T ）。学习自：https://juejin.cn/post/6844904146877808653

type res = First<[1,2,3]>;
```

<img src="https://s2.loli.net/2022/05/01/dTDby751tEAlzXS.png" alt="image-20220501210250796" style="zoom:50%;" />

注意，第一个 extends ( ` Tuple extends unknown` ) 不是条件，条件类型是 `extends ? :` ；<font color=FF0000>这里的 extends 是约束的意思</font>，也就是约束类型参数只能是数组类型。另外，因为不知道数组元素的具体类型，所以用 unkown 。 

##### 联合：｜

联合类型 ( Union ) 类似 js 里的或运算符 `|`，但是作用于类型，代表类型可以是几个类型之一。

```typescript
type Union = 1 | 2 | 3;
```

> 👀 上面有自己的关于 `extends ? :` 使用 Union 的示例。

##### 交叉：&

交叉类型（Intersection）类似 js 中的与运算符 &，但是<font color=FF0000>**作用于类型**</font>，<font color=FF0000>**代表对类型做合并**</font>。

```typescript
type ObjType = {a: number} & {c: string}
type res = { a: number, c: boolean} extends ObjType ? true : false
```

<img src="https://s2.loli.net/2022/05/01/jcnv1fU6FzDZmb8.png" alt="image-20220501211038464" style="zoom:50%;" />

注意，同一类型可以合并，不同的类型没法合并，会被舍弃  ：

<img src="https://s2.loli.net/2022/05/01/h5KndlIRLG9Xzp2.png" alt="image-20220501211209605" style="zoom:50%;" />

##### 映射类型

对象、class 在 TypeScript 对应的类型是 「索引类型」 ( Index Type ) ，那么如何对索引类型作修改呢？答案是「映射类型」 。

> 💡 映射类型是通过 in 操作符遍历类型的 key 得到的新的类型（也就是： `[Key in keyof IndexType]` ）
>
> 学习自：[TypeScript 类型编程](https://segmentfault.com/a/1190000040247980)

```typescript
type MapType<T> = { // 👀 注意这里用大括号包裹 {}，
  [Key in keyof T]?: T[Key] // 👀 这里的 ? 表示可选
}
type MapTypeRes = MapType<{a: 1, b: 2}>
```

<img src="https://s2.loli.net/2022/05/01/E9Rny15MuiXbjNC.png" alt="image-20220501212209184" style="zoom:50%;" />

`keyof T` 是查询索引类型中所有的索引，叫做「索引查询」（👀 根据后面的内容 [[#TS 内置的高级类型#Record]] ，可知： `keyof T` 返回的是一个联合类型 ）。

`T[Key]` 是取索引类型某个索引的值，叫做「索引访问」。

`in` 是用于遍历联合类型的运算符（ 👀 类似于 `for...in` ）。

比如我们把一个索引类型的值变成 3 个元素的数组：

```typescript
type MapType<T> = {
    [Key in keyof T]: [T[Key], T[Key], T[Key]]
}

type MapTypeRes = MapType<{a: 1, b: 2}>;
```

<img src="https://s2.loli.net/2022/05/01/thRPmzXavor5ny2.png" alt="image-20220501212611967" style="zoom:50%;" />

**映射类型就相当于把一个集合映射到另一个集合，这是它名字的由来**。

<img src="https://s2.loli.net/2022/05/01/fdso7akByRzTD8m.png" alt="img" style="zoom:70%;" />

<font color=FF0000 size=4>**除了值可以变化，索引也可以做变化**</font>；<font color=red>用 `as` 运算符，叫做「重映射」</font>。

> 👀 `as` 不仅仅可以用作断言，<font color=red>也可以用作重映射</font>，一般用在 `key in keyof T as ...` 中。这点有所遗忘，值得注意 ⚠️

> 💡 TS 文档中也对 `as` 运算符做了介绍，这里也做了笔记；见 [[#Mapped Types#Key Remapping via `as`]]

```typescript
type MapType<T> = {
    [
       Key in keyof T
           as `${Key & string}${Key & string}${Key & string}`
    ]: [T[Key], T[Key], T[Key]]
}
```

<img src="https://s2.loli.net/2022/05/01/a6jQCHfwO41G2vi.png" alt="image-20220501213009300" style="zoom:50%;" />

<font color=dodgerBlue>**这里为什么 Key 后面跟着 `& string`：**</font>因为 <font color=FF0000>**索引类型（对象、class 等）可以用 string、number 和 symbol 作为 key** ，这里 `keyof T` 取出的索引就是 `string | number | symbol` 的联合类型，**和 string 取交叉，结果就只剩下 string 了**</font>。就像前面所说，交叉类型会把同一类型做合并，不同类型舍弃。



### 模式匹配做提取

我们知道，字符串可以和正则做模式匹配，找到匹配的部分，提取子组，之后可以用 1, 2 等引用匹配的子组。

```typescript
const res = 'abc'.replace(/a(b)c/, '$1,$1,$1') // 'b,b,b'
```

<font color=FF0000>TypeScript 的类型也同样可以做 **模式匹配**</font>。比如这样一个 Promise 类型：

```typescript
type P = Promise<'guang'>
```

我们 <font color=FF0000>想提取 value 的类型，可以这样做</font>：

```typescript
type GetValueType<P> = P extends Promise<infer Value> ? Value : never;
```

通过 extends 对传入的类型参数 P 做模式匹配，其中值的类型是需要提取的。<font color=FF0000>通过 infer 声明一个局部变量 Value 来保存：如果匹配，就返回匹配到的 Value；否则就返回 never 代表没匹配到</font>。

<img src="https://s2.loli.net/2022/05/02/e86qSsLR4BY7KWI.png" alt="image-20220502233642842" style="zoom:50%;" />

这就是 Typescript 类型的模式匹配：**Typescript 类型的模式匹配是 通过 extends 对类型参数做匹配，结果保存到 “通过 infer 声明的局部类型变量里”，如果匹配就能从该局部变量里拿到提取出的类型。**

这个模式匹配的套路有多有用呢？我们来看下在数组、字符串、函数、构造器等类型里的应用。

#### 数组类型

##### First

数组类型想提取第一个元素的类型怎么做呢？用它来匹配一个模式类型，<font color=FF0000>提取第一个元素的类型到通过 infer 声明的局部变量里返回</font>。

```typescript
type arr = [1, 2, 3]

type GetFirst<Arr extends unknown[]> = 
    Arr extends [infer First, ...unknown[]] ? First : never;
```

类型参数 Arr 通过 extends 约束为只能是数组类型，数组元素是 unknown 也就是可以是任何值。

> **any 和 unknown 的区别**
>
> any 和 unknown 都代表任意类型，但是 unknown 只能接收任意类型的值，而 <font color=FF0000>any 除了可以接收任意类型的值，也可以赋值给任意类型（除了 never ）</font>。类型体操中<font color=red>**经常用 unknown 接受和匹配**</font> 任何类型，而很少把任何类型赋值给某个类型变量。

对 Arr 做模式匹配，把我们要提取的第一个元素的类型放到通过 infer 声明的 First 局部变量里，后面的元素可以是任何类型，用 unknown 接收，然后把局部变量 First 返回。

当类型参数 Arr 为 `[1, 2, 3]` 时：

<img src="https://s2.loli.net/2022/05/02/ckwxlXWBHE4CNpz.png" alt="image-20220502235103556" style="zoom:50%;" />

当类型参数 Arr 为 `[]` 时：

<img src="https://s2.loli.net/2022/05/03/GNSFKJ9aOh4MHg3.png" alt="image-20220502235223149" style="zoom:50%;" />

##### Last

同理：

```ts
type arr = [1, 2, 3]
type GetLast<Arr extends unknown[]> = Arr extends [...unknown[], infer Last] ? Last : never
```

<img src="https://s2.loli.net/2022/05/02/DRLslNntuM9qvxK.png" alt="image-20220502235408475" style="zoom:50%;" />

##### PopArr

我们分别取了首尾元素，当然也可以取剩余的数组，比如取去掉了最后一个元素的数组：

```ts
type PopArr<Arr extends unknown[]> = 
     Arr extends [] ? []
       : Arr extends [...infer Rest, unknown] ? Rest : never
```

如果是空数组，就直接返回，否则匹配剩余的元素，放到 infer 声明的局部变量 Rest 里，返回 Rest。

<img src="https://s2.loli.net/2022/05/02/i5j8hX9MEUWeNS2.png" alt="image-20220502235934682" style="zoom:50%;" />

##### ShiftArr

类似的：

```ts
type ShiftArr<Arr extends unknown[]> = 
     Arr extends [] ? []: 
         Arr extends [unknown, ...infer Rest] ? Rest : never
```

<img src="https://s2.loli.net/2022/05/03/JRC8purXPK7F9b1.png" alt="image-20220503001221372" style="zoom:50%;" />

#### 字符串类型

字符串类型也同样可以做模式匹配，匹配一个模式字符串，把需要提取的部分放到 infer 声明的局部变量里。

##### StartsWith

判断字符串是否以某个前缀开头，也是通过模式匹配：

```ts
type StartWith<Str extends string, Prefix extends string> = 
     Str extends `${Prefix}${string}` ? true : false
```

需要声明字符串 Str、匹配的前缀 Prefix 两个类型参数，它们都是 string。

用 Str 去匹配一个模式类型，模式类型的前缀是 Prefix；<font color=FF0000>**后面是任意的 string**</font>，如果匹配返回 true，否则返回 false。

<img src="https://s2.loli.net/2022/05/03/5ztyfvGQ9RsIcHn.png" alt="image-20220503002057792" style="zoom:50%;" />

##### Replace

字符串可以匹配一个模式类型，提取想要的部分，自然也可以用这些再构成一个新的类型。比如实现字符串替换：

```ts
type ReplaceStr<
    Str extends string,
    From extends string,
    To extends string
> = Str extends `${infer Prefix}${From}${infer Suffix}` 
        ? `${Prefix}${To}${Suffix}` : Str;
```

声明要替换的字符串 Str、待（被）替换的字符串 From、替换成的字符串 3 个类型参数，通过 extends 约束为都是 string 类型。

用 Str 去匹配模式串，模式串由 From 和之前之后的字符串构成，把之前之后的字符串放到通过 infer 声明的局部变量 Prefix、Suffix 里。用 Prefix、Suffix 加上替换到的字符串 To 构造成新的字符串类型返回。**注：**这种方式只能替换第一个匹配的字符串

<img src="https://s2.loli.net/2022/05/03/nV942EPrGWxI7Qz.png" alt="image-20220503002909549" style="zoom:50%;" />

##### Trim

能够匹配和替换字符串，那也就能实现去掉空白字符的 Trim：

不过因为我们不知道有多少个空白字符，所以只能一个个匹配和去掉，需要递归。

**先实现 TrimRight：**

```ts
type TrimRight<Str extends string> = 
     Str extends `${infer Rest}${' ' | '\n' | '\t'}` 
         ? TrimRight<Rest> : Str
```

类型参数 Str 是要 Trim 的字符串。如果 Str 匹配字符串 + 空白字符（ 空格、换行、制表符 ），那就把字符串放到 infer 声明的局部变量 Rest 里。把 Rest 作为类型参数递归 TrimRight，直到不匹配，这时的类型参数 Str 就是处理结果。

<img src="https://s2.loli.net/2022/05/03/YWsSMpciHq3L8uG.png" alt="image-20220503011523443" style="zoom:50%;" />

同理可得 TrimLeft：

```typescript
type TrimStrLeft<Str extends string> = 
    Str extends `${' ' | '\n' | '\t'}${infer Rest}` 
        ? TrimStrLeft<Rest> : Str;
```

TrimRight 和 TrimLeft 结合就是 Trim：

```typescript
type TrimStr<Str extends string> = TrimStrRight<TrimStrLeft<Str>>;
```

#### 函数

<font color=FF0000>函数同样也可以做类型匹配，比如 **提取参数**、**返回值的类型**</font>。

##### GetParameters

函数类型可以通过模式匹配来提取参数的类型：

```typescript
type GetParameters<Func extends Function> = 
    Func extends (...args: infer Args) => unknown ? Args : never; // 注：注意这里 ...args: infer Args 的写法。
```

类型参数 Func 是要匹配的函数类型，通过 extends 约束为 Function。Func 和模式类型做匹配，参数类型放到用 infer 声明的局部变量 Args 里，返回值可以是任何类型，用 unknown。返回提取到的参数类型 Args。

<img src="https://s2.loli.net/2022/05/03/tVeHcRKb8izXpWd.png" alt="image-20220503012245739" style="zoom:50%;" />

##### GetReturnType

能提取参数类型，同样也可以提取返回值类型：

```ts
type GetReturnType<Func extends Function> = 
    Func extends (...args: any[]) => infer ReturnType // 👀 注意这里就是一个 infer ReturnType 
        ? ReturnType : never;
// 👀 自己尝试写的时候，想要在 infer 前面加 unknown 之类的返回值类型，不过在报错；所以没写出来。
```

Func 和模式类型做匹配，提取返回值到通过 infer 声明的局部变量 ReturnType 里返回。

参数类型可以是任意类型，也就是 `any[]`（<font color=FF0000>**注意，这里不能用 unknown，因为参数类型是要赋值给别的类型的，而 unknown 只能用来接收类型，所以用 any**</font> ）。

> 👀 这里在写的时候，使用 unknown 了

<img src="https://s2.loli.net/2022/05/03/Ts86t1wmAFi3Vgd.png" alt="image-20220503013038807" style="zoom:50%;" />

##### GetThisParameterType

方法里可以调用 this，比如这样：

```js
class Dong {
    name: string;
    constructor() {
        this.name = "dong";
    }
    hello() {
        return 'hello, I\'m ' + this.name;
    }
}

const dong = new Dong();
dong.hello();
```

用 `obj.method()` 的方式调用的时候，this 就指向那个对象。但是方法也可以用 call 或者 apply 调用。

call 调用的时候，this 就变了，但这里却没有被检查出来 this 指向的错误。

如何让编译器能够检查出 this 指向的错误呢？可以在方法声明时指定 this 的类型：

```ts
class Dong {
    name: string;
    constructor() {
        this.name = "dong";
    }
    hello(this: Dong) {
        return 'hello, I\'m ' + this.name;
    }
}
```

这样，当 call / apply 调用的时候，就能检查出 this 指向的对象是否是对的（👀 这里 `tsconfig.json` 要加上 `"strictBindCallApply": true` ，否则不会报错 ）：

<img src="https://s2.loli.net/2022/05/03/J9IsuvlEmdkQpFt.png" alt="img" style="zoom:67%;" />

<font color=FF0000>**这里的 this 类型同样也可以通过模式匹配提取出来：**</font>

```ts
type GetThisParameterType<T> 
    = T extends (this: infer ThisType, ...args: any[]) => any // 注：注意这里“函数签名”的写法，this 作为第一个参数
        ? ThisType 
        : unknown;
```

<font color=FF0000>类型参数 T 是待处理的类型</font>。<font color=FF0000>**用 T 匹配一个 模式类型，提取 this 的类型到 infer 声明的局部变量 ThisType 中**</font>（👀 这里有点没看懂 TODO ），其余的参数是任意类型，也就是 any，返回值也是任意类型。返回提取到的 ThisType，这样就能提取出 this 的类型：

<img src="https://s2.loli.net/2022/05/03/U8fGXHzVY9Dvx4u.png" alt="image-20220503015143577" style="zoom:50%;" />



#### 构造器

构造器和函数的区别是，构造器是用于创建对象的，所以可以被 new 。

同样，我们也<font color=FF0000>可以通过模式匹配提取 构造器的参数和返回值的类型</font>：

##### GetInstanceType

构造器类型可以用 interface 声明，使用 `new(): foo` 的语法。比如：

```ts
interface Person {
    name: string;
}

interface PersonConstructor {
    new (name: string): Person;
}
```

这里的 PersonConstructor 返回的是 Person 类型的实例对象，这个也可以通过模式匹配取出来（**注：**即：将实例对象的类型提取出）

```ts
type GetInstanceType<
    ConstructorType extends new (...args: any) => any // 注：注意“函数签名”前面有 new
> = ConstructorType extends new (...args: any) => infer InstanceType 
        ? InstanceType 
        : any;
```

类型参数 ConstructorType 是待处理的类型，通过 extends 约束为<font color=FF0000>**构造器类型**</font>。

用 ConstructorType 匹配一个模式类型，提取返回的实例类型到 infer 声明的局部变量 InstanceType 里，返回 InstanceType。这样就能取出构造器对应的实例类型：

<img src="https://s2.loli.net/2022/05/03/Uyjbk4OPWv1Rst2.png" alt="image-20220503021238662" style="zoom:50%;" />

##### GetConstructorParameters

GetInstanceType 是提取构造器返回值类型，那同样也可以提取构造器的参数类型：

```ts
type GetConstructorParameters<
    ConstructorType extends new (...args: any) => any
> = ConstructorType extends new (...args: infer ParametersType) => any // 👀 自己尝试实现时，没准备加上 `args:`
    ? ParametersType
    : never;
```

类型参数 ConstructorType 为待处理的类型，通过 extends 约束为构造器类型。

用 ConstructorType 匹配一个模式类型，提取参数的部分到 infer 声明的局部变量 ParametersType 里，返回 ParametersType。这样就能提取出构造器对应的参数类型：

<img src="https://s2.loli.net/2022/05/03/qFPGlZMKv69m3uk.png" alt="image-20220503021900246" style="zoom:50%;" />

#### 索引类型

<font color=FF0000>索引类型也同样可以用模式匹配提取某个索引的值的类型</font>，这个用的也挺多的。比如 React 的 index.d.ts 里的 PropsWithRef 的高级类型，就是通过模式匹配提取了 ref 的值的类型：

<img src="https://s2.loli.net/2022/05/03/m6bs8RoiGjEUOJZ.png" alt="img" style="zoom: 67%;" />

我们简化一下那个高级类型，提取 Props 里 ref 的类型：

##### GetRefProps

我们同样通过模式匹配的方式提取 ref 的值的类型：

```typescript
type GetRefProps<Props> = 
    'ref' extends keyof Props
        ? Props extends { ref?: infer Value | undefined}
            ? Value
            : never
        : never;
```

类型参数 Props 为待处理的类型。

通过 `keyof Props` 取出 Props 的所有索引构成的联合类型，判断下 ref 是否在其中，也就是 `'ref' extends keyof Props`。为什么要做这个判断，上面注释里写了：

<img src="https://s2.loli.net/2022/05/03/yFtbjW4i58GfBDs.png" alt="img" style="zoom:55%;" />

在 ts3.0 里面如果没有对应的索引，`Obj[Key]` 返回的是 `{}` 而不是 `never`，所以这样做向下兼容处理。如果有 ref 这个索引的话，就通过 `infer` 提取 Value 的类型返回，否则返回 never。

<img src="https://s2.loli.net/2022/05/03/jV81pD4QqSUIs7n.png" alt="image-20220503023006564" style="zoom:50%;" />

##### 4.7 新特性 infer extends

> 👀 这不是小册中的内容，算是小册的补充

infer 有一个问题，比如这样：

<img src="https://s2.loli.net/2022/08/25/NxdYXznTvOZDgHV.png" alt="image-20220825224259838" style="zoom:40%;" />

从 string 数组中提取的元素，默认会推导为 unknown 类型，这就导致了不能直接把它当 string 用：

<img src="https://s2.loli.net/2022/11/23/zL8oiAEUDFONIQT.png" alt="image-20220825224420465" style="zoom:50%;" />

之前的处理方式是这样的（如下），加一层判断，这样 Last 就推导为 string 类型了。

<img src="https://s2.loli.net/2022/08/25/gCq26Pho53usRQY.png" alt="image-20220825224622889" style="zoom:40%;" />

或者也可以和 string 取交叉类型（如下），这样也可以作为 string 来用。

<img src="https://s2.loli.net/2022/08/25/4VCvgh59TAuLoHe.png" alt="image-20220825224815358" style="zoom:40%;" />

但是我们明明知道这里就是 string，却还需要 `& string` 或者 `Last extends string` 来转换一次，这也太麻烦了。

TS 也知道有这个问题，所以在 4.7 就引入了新语法：`infer extends` 。现在我们可以这样写：

<img src="https://s2.loli.net/2022/08/25/JupnOmHyLh7Cjtf.png" alt="image-20220825225058608" style="zoom:40%;" />

<font color=fuchsia>**infer 的时候加上 extends 来约束推导的类型，这样推导出的就不再是 unknown 了，而是约束的类型**。</font>

这个语法是 TS 4.7 引入的，<font color=dodgerBlue>在 4.8 又完善了一下</font>。<font color=dodgerBlue>比如这样一个类型：</font>

```typescript
type NumInfer<Str> = 
    Str extends `${infer Num extends number}`
        ? Num
        : never;
```

在 4.7 的时候推导结果是这样：

<img src="https://s2.loli.net/2022/08/25/a1jRs9xtPwkbeWK.png" alt="image-20220825225732763" style="zoom:40%;" />

而 4.8 就是这样了：

<img src="https://s2.loli.net/2022/08/25/hTFOarVp2i1cUdj.png" alt="image-20220825225629741" style="zoom:40%;" />

也就是说 4.7 的时候推导出的就是 extends 约束的类型，但是 <font color=fuchsia>4.8 的时候，如果是基础类型，会推导出字面量类型</font>。

有了这个语法之后，除了能简化类型编程的逻辑之外，也能实现一些之前实现不了的功能。

比如提取枚举的值的类型：

```ts
enum Code { a = 111,  b = 222, c = "abc" }
```

<img src="https://s2.loli.net/2022/08/25/ZtvmCD3w2udxrYi.png" alt="image-20220825231925434" style="zoom:40%;" />

但是有的值明明是数字，却被作为了字符串，所以要再处理一下，转换成数字类型，这时候就可以用 `infer extends` 了：

```ts
type StrToNum<Str> =
  Str extends `${infer Num extends number}`
    ? Num
    : Str
```

做完 string 到 number 的转换，就拿到了我们想要的结果：

<img src="https://s2.loli.net/2022/08/25/RK8cSWLEsA53H7h.png" alt="image-20220825232508422" style="zoom:40%;" />

这就是 infer extends 的第二个作用。

除了 string 转 number 之外，也可以转 boolean、null 等类型：

<img src="https://s2.loli.net/2022/08/25/Coipc4qymhxz3DW.png" alt="image-20220825232821018" style="zoom:40%;" />

<img src="https://s2.loli.net/2022/08/25/SkomQsPvcNbZ2pe.png" alt="image-20220825232930608" style="zoom:40%;" />

摘自：[快速掌握 TypeScript 新语法：infer extends](https://juejin.cn/post/7133438765317488677)



### 重新构造做变换

TypeScript 设计可以做类型编程的类型系统的目的就是为了产生各种复杂的类型，那不能修改，怎么产生新类型呢？答案是 “重新构造”。

#### 重新构造

**TypeScript 的 type、infer、类型参数声明的变量都不能修改，想对类型做各种变换产生新的类型就需要 “重新构造”。**

数组、字符串、函数 等类型的重新构造比较简单。索引类型，也就是多个元素的聚合类型的重新构造复杂一些，涉及到了映射类型的语法。

#### 数组类型的重新构造

##### Push

有这样一个元组类型：

```ts
type tuple = [1, 2, 3];
```

我想给这个元组类型再添加一些类型，怎么做呢？TypeScript 类型变量不支持修改，我们可以构造一个新的元组类型：

```ts
type Push<Arr extends  unknown[], Ele> = [...Arr, Ele];
```

类型参数 Arr 是要修改的 数组 / 元组类型，元素的类型任意，也就是 unknown ；类型参数 Ele 是添加的元素的类型。返回的是用 Arr 已有的元素加上 Ele 构造的新的元组类型。

<img src="https://s2.loli.net/2022/05/03/nioRQDWIr5B3jXx.png" alt="image-20220503023957842" style="zoom:50%;" />

这就是 数组 / 元组 的重新构造

> **数组和元组的区别**：<font color=FF0000>**数组类型 是指任意多个同一类型的元素构成的，比如 number[]、Array\<number>**</font>；而<font color=FF0000>元组则是数量固定，类型可以不同的元素构成的</font>，比如 [1, true, 'guang']。

##### Unshift

可以在后面添加，同样也可以在前面添加：

```typescript
type Unshift<Arr extends  unknown[], Ele> = [Ele, ...Arr];
```

##### Zip

有这样两个元组：

```typescript
type tuple1 = [1,2];
type tuple2 = ['guang', 'dong'];
```

我们想把它们合并成这样的元组：

```typescript
type tuple = [[1, 'guang'], [2, 'dong']]; // 注意这里的形式
```

思路很容易想到，提取元组中的两个元素，构造成新的元组：

```ts
type Zip<One extends [unknown, unknown], Other extends [unknown, unknown]> = 
     One extends [infer OneFirst, infer OneSecond]
         ? Other extends [infer OtherFirst, infer OtherSecond]
            ? [[OneFirst, OtherFirst], [OneSecond, OtherSecond]] : []
            : []
```

两个类型参数 One、Other 是两个元组，类型是 [unknown, unknown]，代表 2 个任意类型的元素构成的元组。

通过 infer 分别提取 One 和 Other 的元素到 infer 声明的局部变量 OneFirst、OneSecond、OtherFirst、OtherSecond 里。

用提取的元素构造成新的元组返回即可：

<img src="https://s2.loli.net/2022/05/03/C2Tm4inx5q6XUHa.png" alt="image-20220503141940873" style="zoom:50%;" />

但是这样

只能合并两个元素的元组，如果是任意个呢？那就得用递归了：

```ts
type Zip<One extends unknown[], Other extends unknown[]> = 
     One extends [infer OneFir, ...infer OneRest] 
         ? Other extends [infer OtherFir, ...infer OtherRest]
             ? [[OneFir, OtherFir], ...Zip<OneRest, OtherRest>] : [] // 注：注意这里的 ...zip
             : []
```

> 👀 自己尝试实现时，`...zip` 没写出，写的是 `Zip<oneRest, OtherRest>` ，显然是错的

类型参数 One、Other 声明为 `unknown[]` ，也就是元素个数任意，类型任意的数组。

每次提取 One 和 Other 的第一个元素 OneFirst、OtherFirst，剩余的放到 OneRest、OtherRest 里。用 OneFirst、OtherFirst 构造成新的元组的一个元素，剩余元素继续递归处理 OneRest、OtherRest。这样，就能处理任意个数元组的合并：

<img src="https://s2.loli.net/2022/05/03/utcTYmVFoRKynib.png" alt="image-20220503143011106" style="zoom:50%;" />

#### 字符串类型的重新构造

##### CapitalizeStr

我们想把一个字符串字面量类型的 'guang' 转为首字母大写的 'Guang'。需要用到字符串类型的提取和重新构造：

```ts
type CapitalizeStr<Str extends string> = Str extends `${infer First}${infer Rest}` ? // 👀 Rest 变量接收剩余字符
      `${Uppercase<First>}${Rest}` : Str // 👀 注意这里的 Uppercase<>
```

我们声明了类型参数 Str 是要处理的字符串类型，通过 extends 约束为 string。

通过 infer 提取出首个字符到局部变量 First，<font color=FF0000>**提取后面的字符到局部变量 Rest**</font>。然后 <font color=FF0000>**使用 TypeScript 提供的内置高级类型 Uppercase 把首字母转为大写**</font>，加上 Rest，构造成新的字符串类型返回。

<img src="https://s2.loli.net/2022/05/03/wvi3oPrcjmteg1O.png" alt="image-20220503145046293" style="zoom:50%;" />

#### CamelCase

我们再来实现 “dong_dong_dong” 到 “dongDongDong” 的变换。同样是提取和重新构造：

```typescript
type CamelCase<Str extends string> = 
    Str extends `${infer Left}_${infer Right}${infer Rest}`
        ? `${Left}${Uppercase<Right>}${CamelCase<Rest>}`
        : Str;
```

> 👀 这个写出来了
>
> ```ts
> type CamelCase<Str extends String> = 
>      Str extends `${infer prefix}_${infer upper}${infer suffix}` 
>          ? `${prefix}${Uppercase<upper>}${CamelCase<suffix>}`: Str
> ```

类型参数 Str 是待处理的字符串类型，约束为 string。

提取 `_` 之前和之后的两个字符到 infer 声明的局部变量 Left 和 Right，剩下的字符放到 Rest 里。然后把右边的字符 Right 大写，和 Left 构造成新的字符串，剩余的字符 Rest 要继续递归的处理。这样就完成了从下划线到驼峰形式的转换：

<img src="https://s2.loli.net/2022/05/03/ghlzYSIbU3wstDv.png" alt="image-20220503150004887" style="zoom:50%;" />

##### DropSubStr

可以修改自然也可以删除，我们再来做一个删除一段字符串的案例：删除字符串中的某个子串

```ts
type DropSubStr<Str extends string, SubStr extends string> = 
    Str extends `${infer Prefix}${SubStr}${infer Suffix}` 
        ? DropSubStr<`${Prefix}${Suffix}`, SubStr> : Str;
```

> 注：这个也写出来了，虽然没错，但是好像没有教程写得好？
>
> ```ts
> type DropSubStr<Str extends string, DelStr extends string> = 
>      Str extends `${infer prefix}${DelStr}${infer suffix}` 
>          ? `${prefix}${DropSubStr<suffix, DelStr>}` : Str
> ```

类型参数 Str 是待处理的字符串， SubStr 是要删除的字符串，都通过 extends 约束为 string 类型。

通过模式匹配提取 SubStr 之前和之后的字符串到 infer 声明的局部变量 Prefix、Suffix 中。如果不匹配就直接返回 Str。如果匹配，那就用 Prefix、Suffix 构造成新的字符串，然后继续递归删除 SubStr。直到不再匹配，也就是没有 SubStr 了。

<img src="https://s2.loli.net/2022/05/03/ku7L49hHy3snDwx.png" alt="image-20220503150708754" style="zoom:50%;" />

#### 函数类型的重新构造

##### AppendArgument

之前我们分别实现了参数和返回值的提取，那么 <font color=FF0000>重新构造就是用这些提取出的类型做下修改，构造一个新的类型</font> 即可。

比如，<font color=FF0000>在已有的 **函数类型**</font> （⚠️ 这里不要看错 ）<font color=FF0000>上添加一个参数</font>：

```ts
type AppendArgument<Func extends Function, Arg> = 
    Func extends (...args: infer Args) => infer ReturnType // 👀 这里实现时 ...args: infer Args 有遗忘
        ? (...args: [...Args, Arg]) => ReturnType : never; // 👀 这里实现时写错了，没组成一个数组
```

类型参数 Func 是待处理的函数类型，通过 extends 约束为 Function，Arg 是要添加的参数类型。

通过模式匹配提取参数到 infer 声明的局部变量 Args 中，提取返回值到局部变量 ReturnType 中。<font color=FF0000>用 Args 数组添加 Arg 构造成新的参数类型</font>，结合 ReturnType 构造成新的函数类型返回。这样就完成了函数类型的修改：

<img src="https://s2.loli.net/2022/05/03/JGdQ7Cvi6hT2r1g.png" alt="image-20220503175404536" style="zoom:50%;" />

#### 索引类型的重新构造

> 👀 这部分内容和 [[#TypeScript 类型系统中的类型运算#映射类型]] 中的几乎一致，这里略。不过，还说了一个 UppercaseKey 的 “参数名大写的变换”；不过由于前面知识的学习，这没有什么难度：

```typescript
type UppercaseKey<Obj extends object> = { 
    [Key in keyof Obj as Uppercase<Key & string>]: Obj[Key]
}
```

##### Record

<font color=FF0000>TypeScript 提供了**内置的高级类型 Record 来创建索引类型**</font>：

```typescript
type Record<K extends string | number | symbol, T> = { [P in K]: T; }
```

<font color=FF0000>**指定索引和值的类型分别为 K 和 T**，就可以 <font size=4>**创建一个对应的索引类型**</font></font>。

> 💡 使用示例：
> ```ts
> type Size = 'small' | 'default' | 'big'
> /*
> {
>   small: number
>   default: number
>   big: number
> }
> */
> type sizeMap = Record<Size, number>
> ```

上面（示例）的 <font color=FF0000>**索引类型的约束** 我们用的 object </font>，其实**<font color=FF0000>更语义化一点我推荐用</font> <font size=4>`Record<string, object>` </font>**：

```typescript
type UppercaseKey<Obj extends Record<string, any>> = { 
    [Key in keyof Obj as Uppercase<Key & string>]: Obj[Key]
}
```

也就是约束类型参数 Obj 为 key 为 string，val 为任意类型 的索引类型。

##### ToReadonly

<font color=FF0000>**索引类型的索引可以添加 readonly 的修饰符，代表只读**</font>。

那我们就可以实现给索引类型添加 readonly 修饰的高级类型：

```typescript
type ToReadonly<T> =  {
    readonly [Key in keyof T]: T[Key];
}
```

通过映射类型构造了新的索引类型，给索引加上了 readonly 的修饰，其余的保持不变，索引依然为原来的索引 `Key in keyof T`，值依然为原来的值 `T[Key]` 。

<img src="https://s2.loli.net/2022/05/03/bHaOZThjJktX4uy.png" alt="image-20220503190050776" style="zoom:50%;" />

##### ToPartial

同理，索引类型还可以添加可选修饰符：

```typescript
type ToPartial<T> = {
    [Key in keyof T]?: T[Key]
}
```

给索引类型 T 的索引添加了 `?` 可选修饰符，其余保持不变。

<img src="https://s2.loli.net/2022/05/03/Si5rp63cKx4by8V.png" alt="image-20220503190459252" style="zoom:50%;" />

##### ToMutable

可以添加 readonly 修饰，当然也可以去掉：

```ts
type ToMutable<T> = {
    -readonly [Key in keyof T]: T[Key]
}
```

给索引类型 T 的每个索引去掉 readonly 的修饰，其余保持不变。

<img src="https://s2.loli.net/2022/05/07/soAim2uHJBtMXaO.png" alt="image-20220503191406123" style="zoom:50%;" />

##### ToRequired

同理，也可以去掉可选修饰符：

```ts
type ToRequired<T> = {
    [Key in keyof T]-?: T[Key]
}
```

给索引类型 T 的索引去掉 ? 的修饰 ，其余保持不变。

<img src="https://s2.loli.net/2022/05/07/F3tBNygZ5JLUEqV.png" alt="image-20220503191609690" style="zoom:50%;" />

##### FilterByValueType

可以在构造新索引类型的时候根据值的类型做下过滤：

```ts
type FilterByValueType<
    Obj extends Record<string, any>, 
    ValueType
> = {
    [Key in keyof Obj 
        as ValueType extends Obj[Key] ? Key : never]
        : Obj[Key]
}
```

类型参数 Obj 为要处理的索引类型，通过 extends 约束为索引为 string，值为任意类型的索引类型 `Record<string, any>` 。类型参数 ValueType 为要过滤出的值的类型。

构造新的索引类型，索引为 Obj 的索引，也就是 `Key in keyof Obj` ；<font color=dodgerBlue>**要做一些变换，也就是 as 之后的部分**</font>。如果原来索引的值 `Obj[Key]` 是 ValueType 类型，索引依然为之前的索引 Key；否则索引设置为 never，<font color=fuchsia size=4>**never 的索引会在生成新的索引类型时被去掉**</font>。值保持不变，依然为原来索引的值，也就是 `Obj[Key]` 。

> ⚠️ 上面 as 中返回 never 的索引类型会被去掉，这一点是没有听过的；而且，2023/9/11 回看也毫无印象...

这样就达到了过滤索引类型的索引，产生新的索引类型的目的：

<img src="https://s2.loli.net/2022/05/03/fVDhR2HLgU69caK.png" alt="image-20220503193100686" style="zoom:50%;" />



### 递归复用做循环

会做类型的提取和构造之后，我们已经能写出很多类型编程逻辑了，但是有时候提取或构造的数组元素个数不确定、字符串长度不确定、对象层数不确定；这时候怎么办呢？ 递归

TypeScript 的高级类型支持类型参数，可以做各种类型运算逻辑，返回新的类型，和函数调用是对应的；自然也支持递归。

**<font color=FF0000>TypeScript 类型系统 <font size=4>不支持循环</font>，但 <font size=4>支持递归</font></font>。当处理数量（个数、长度、层数）不固定的类型的时候，可以只处理一个类型；然后递归的调用自身处理下一个类型，直到结束条件也就是所有的类型都处理完了；就完成了不确定数量的类型编程，达到循环的效果。**

既然提到了数组、字符串、对象等类型，那么我们就来看一下这些类型的递归案例吧。

#### Promise 的递归复用

##### DeepPromiseValueType

先用 Promise 热热身，实现一个提取不确定层数的 Promise 中的 value 类型的高级类型。

```typescript
type ttt = Promise<Promise<Promise<Record<string, any>>>>;
```

这里是 3 层 Promise，value 类型是索引类型。<font color=LightSeaGreen>数量不确定，一涉及到这个就要想到用递归来做，每次只处理一层的提取，然后剩下的到下次递归做，直到结束条件</font>。

所以高级类型是这样的：

```ts
type DeepPromiseValueType<P extends Promise<unknown>> = 
     P extends Promise<infer ValueType>
        ? ValueType extends Promise<unknown> // 👀 这里做的是判断 ValueType 是不是 Promise 类型，还是 Promise 的内容
            ? DeepPromiseValueType<ValueType>
            : ValueType
        : never
```

类型参数 P 是待处理的 Promise，通过 extends 约束为 Promise 类型；<font color=FF0000>value 类型不确定，设为 unknown </font>。

每次只处理一个类型的提取，也就是通过模式匹配<font color=FF0000>提取出 value 的类型到 infer 声明的局部变量 ValueType 中</font>。然后<font color=FF0000>判断如果 ValueType 依然是 Promise类型，就递归处理</font>。结束条件就是 ValueType 不为 Promise 类型，那就处理完了所有的层数，返回这时的 ValueType。

这样，我们就提取到了最里层的 Promise 的 value 类型，也就是索引类型：

<img src="https://s2.loli.net/2022/05/03/YV9GymfBLqo43aN.png" alt="image-20220503205030391" style="zoom:50%;" />

其实这个类型的实现可以进一步的简化：

```typescript
type DeepPromiseValueType2<T> = 
    T extends Promise<infer ValueType> 
        ? DeepPromiseValueType2<ValueType>
        : T;
```

不再约束类型参数必须是 Promise，这样就可以少一层判断。

#### 数组类型的递归

##### ReverseArr

做数组的逆转。我们每次只处理一个类型，剩下的递归做；直到满足结束条件。

```typescript
type ReverseArr<Arr extends unknown[]> = 
    Arr extends [infer First, ...infer Rest] 
        ? [...ReverseArr<Rest>, First] 
        : Arr;
```

> 👀 这个写出来了，而且代码几乎一模一样；讲解略。

##### includes

既然递归可以做循环用，那么像查找元素这种自然也就可以实现。

```ts
type Includes<Arr extends unknown[], FindItem> = 
    Arr extends [infer First, ...infer Rest]
        ? IsEqual<First, FindItem> extends true
            ? true
            : Includes<Rest, FindItem>
        : false;

type IsEqual<A, B> = (A extends B ? true : false) & (B extends A ? true : false);
```

类型参数 Arr 是待查找的数组类型，元素类型任意，也就是 unknown。FindItem 待查找的元素类型。

每次提取一个元素到 infer 声明的局部变量 First 中，剩余的放到局部变量 Rest。判断 First 是否是要查找的元素，也就是和 FindItem 相等，是的话就返回 true，否则继续递归判断下一个元素。直到结束条件也就是提取不出下一个元素，这时返回 false。

相等的判断就是 A 是 B 的子类型并且 B 也是 A 的子类型。另外，这个 IsEqual 是不完善的（<font color=FF0000>没有处理 其中有 any 的情况</font>）具体可见 [[#特殊特性要记清#IsEqual]]

这样就完成了不确定长度的数组中的元素查找，用递归实现了循环。

<img src="https://s2.loli.net/2022/05/03/X97ZDNOHUiRAFmQ.png" alt="image-20220503211043208" style="zoom:50%;" />

##### RemoveItem

可以查找自然就可以删除，只需要改下返回结果，构造一个新的数组返回。

```ts
type RemoveItem<
    Arr extends unknown[], 
    Item, 
    Result extends unknown[] = []
> = Arr extends [infer First, ...infer Rest]
        ? IsEqual<First, Item> extends true
            ? RemoveItem<Rest, Item, Result>
            : RemoveItem<Rest, Item, [...Result, First]>
        : Result;
        
type IsEqual<A, B> = (A extends B ? true : false) & (B extends A ? true : false);
```

类型参数 Arr 是待处理的数组，元素类型任意，也就是 `unknown[]` 。类型参数 Item 为待查找的元素类型。类型参数 Result 是构造出的新数组，默认值是 []。

通过「模式匹配」提取数组中的一个元素的类型，如果是 Item 类型的话就删除，也就是不放入构造的新数组，直接返回之前的 Result ；否则放入构造的新数组，也就是再构造一个新的数组 `[...Result, First]` 。直到模式匹配不再满足，也就是处理完了所有的元素，返回这时候的 Result 。

> 👀 这里 `[...Result, First]` 为什么 First 放在后面，是因为 First 是 “当前“ 递归 的 Item ，所以相当于 将当前的 Item 放在 Result 后面。另外，下面有类似的例子 [[#字符串类型的递归#ReverseStr]]，也有讲解。

这样我们就完成了不确定元素个数的数组的某个元素的删除：

<img src="https://s2.loli.net/2022/05/03/CjneVmRQo2lNiBu.png" alt="image-20220503212650792" style="zoom:50%;" />

##### BuildArray

我们学过数组类型的构造，如果构造的数组类型元素个数不确定，也需要递归。

比如传入 5 和 元素类型，构造一个长度为 5 的该元素类型构成的数组。

```ts
type BuildArray<
    Length extends number, 
    Ele = unknown, 
    Arr extends unknown[] = []
> = Arr['length'] extends Length // 注：Arr 的长度等于规定长度，则返回；否则继续递归
        ? Arr 
        : BuildArray<Length, Ele, [...Arr, Ele]>;
```

> 👀 这个不难，但是没写出；原因在于不知道使用 `Arr['length']`

<img src="https://s2.loli.net/2022/05/03/cs5UVW9xzGelLZA.png" alt="image-20220503225509657" style="zoom:50%;" />

#### 字符串类型的递归

##### ReplaceAll

学模式匹配的时候，我们实现过一个 Replace 的高级类型：

```typescript
type ReplaceStr<
    Str extends string,
    From extends string,
    To extends string
> = Str extends `${infer Prefix}${From}${infer Suffix}` 
    ? `${Prefix}${To}${Suffix}` : Str;
```

它能把一个字符串中的某个字符替换成另一个：

<img src="https://s2.loli.net/2022/05/03/nV942EPrGWxI7Qz.png" alt="image-20220503002909549" style="zoom:50%;" />

但是如果有多个这样的字符就处理不了了。如果不确定有多少个 From 字符，怎么处理呢？

<font color=FF0000>**在类型体操里，遇到数量不确定的问题，就要条件反射的想到递归。**</font>

每次递归只处理一个类型，这部分我们已经实现了，那么加上递归的调用就可以。

```ts
type ReplaceAll<
    Str extends string, 
    From extends string, 
    To extends string
> = Str extends `${infer Left}${From}${infer Right}`
        ? `${Left}${To}${ReplaceAll<Right, From, To>}` // 注：只有这里改了下
        : Str;
```

> 👀 较原版只是添加了递归，讲解略

##### StringToUnion

我们想把字符串字面量类型的每个字符都提取出来组成联合类型，也就是把 'dong' 转为 `'d' | 'o' | 'n' | 'g'` 。

```ts
type StringToUnion<Str extends string> = 
    Str extends `${infer First}${infer Rest}` // 👀 这里写的时候，始终想着 ...infer Rest，这是数组的用法
        ? First | StringToUnion<Rest> // 👀 这里确实没想到 `｜` 可以直接使用
        : never;
```

类型参数 Str 为待处理的字符串类型，通过 extends 约束为 string。

通过模式匹配提取第一个字符到 infer 声明的局部变量 First，其余的字符放到局部变量 Rest。用 First 构造联合类型，剩余的元素递归的取。这样就完成了不确定长度的字符串的提取和联合类型的构造：

<img src="https://s2.loli.net/2022/05/03/26gHYXnUeyE9z3F.png" alt="image-20220503232144568" style="zoom:50%;" />

##### ReverseStr

我们实现了数组的反转，自然也可以实现字符串类型的反转。同样是递归提取和构造。

```ts
type ReverseStr<
    Str extends string, 
    Result extends string = ''
> = Str extends `${infer First}${infer Rest}`
    ? ReverseStr<Rest, `${First}${Result}`>
    : Result;
```

类型参数 Str 为待处理的字符串。类型参数 Result 为构造出的字符，<font color=FF0000>默认值是空串</font>（ ⚠️ 注意这种写法）。

通过模式匹配提取第一个字符到 infer 声明的局部变量 First，其余字符放到 Rest 。用 First 和之前的 Result 构造成新的字符串，<font color=FF0000>**把 First 放到前面，因为 <font size=4>递归是从左到右处理，那么不断往前插就是把右边的放到了左边</font>，完成了反转的效果**</font>。直到模式匹配不满足，就处理完了所有的字符。

这样就完成了字符串的反转：

> 👀 自己写对了，不过方法不一样。
>
> ```ts
> type ReverseStr<Str extends string> = 
>      Str extends `${infer First}${infer Rest}`
>        ? `${ReverseStr<Rest>}${First}`
>        : ''
> ```

#### 对象类型的递归

##### DeepReadonly

对象类型的递归，也可以叫做索引类型的递归。

我们之前实现了索引类型的映射，给索引加上了 readonly 的修饰；如果这个索引类型层数不确定呢？比如下面

```ts
type obj = {
    a: {
        b: {
            c: {
                f: () => 'dong',
                d: {
                    e: {
                        guang: string
                    }
                }
            }
        }
    }
}
```

> ⚠️ 需要注意的是： <font size=4>**`Function extends object ? true : false` 结果为 <font color=FF0000>true</font> **</font> ， <font size=4>**`string extends object ? true : false` 结果为 <font color=FF0000>false</font>**</font>  

另外，一开始写的时候，写的代码和原文错误代码差不多；不过没考虑到 `Function extends object ? true : false` 为 true 的情况。加上这种情况后，代码（也是原文中代码）如下：

```ts
type DeepReadonly<Obj extends Record<string, any>> = {
     readonly [Key in keyof Obj]: 
        Obj[Key] extends object
           ? Obj extends Function
              ? Obj[Key]
              : DeepReadonly<Obj[Key]>
           : Obj[Key]
}
```

不过结果，还是不行的；到第二层就没有计算了：

<img src="https://s2.loli.net/2022/05/04/q2Oo38rIP4cZCav.png" alt="image-20220504003513846" style="zoom:50%;" />

原因是： <font color=FF0000 size=4>**TS 只有类型被用到的时候才会做类型计算**</font>。

所以，<font size=4><font color=FF0000>可以在前面加上一段</font> `Obj extends never ? never` <font color=FF0000>或者</font> `Obj extends any` ，<font color=FF0000>让它触发计算</font></font>；另外，<font size=4>写 `Obj extends any` 还有额外的好处，就是<font color=FF0000>能 **处理联合类型**</font></font>，（👀 形成 “分布式条件类型” ）相关讲解见 [[#联合分散可简化]] 

最终代码如下：

```ts
type DeepReadonly<Obj extends Record<string, any>> =
    Obj extends any
        ? {
            readonly [Key in keyof Obj]:
                Obj[Key] extends object
                    ? Obj[Key] extends Function
                        ? Obj[Key] 
                        : DeepReadonly<Obj[Key]>
                    : Obj[Key]
        }
        : never;
```

<img src="https://s2.loli.net/2022/05/04/DVr1IZNuspa29zc.png" alt="image-20220504003852914" style="zoom:50%;" />



### 数组长度做计数

TS 类型系统 <font color=dodgerBlue>**可以实现**</font> “数值相关的逻辑”

#### 数组长度做计数

TypeScript 类型系统没有加减乘除运算符，怎么做数值运算呢？

不知道大家有没有注意到 <font color=FF0000>**数组类型取 length 就是数值**</font>。如下示例：

<img src="https://s2.loli.net/2022/05/04/3OfnzTmt8H5pGRS.png" alt="image-20220504005055617" style="zoom:50%;" />

<font color=dodgerBlue>**TypeScript 类型系统中 没有加减乘除运算符**</font>，但是 <font color=red>可以通过构造不同的数组然后取 `length` 的方式来完成数值计算，把数值的加减乘除转化为对数组的提取和构造</font>（严格来说构造的是元组，大家知道数组和元组的区别就行）

这点可以说是类型体操中最麻烦的一个点，需要思维做一些转换，绕过这个弯来。

#### 数组长度实现加减乘除

##### Add

我们知道了数值计算要转换为对数组类型的操作，那么加法的实现很容易想到：构造两个数组，然后合并成一个，取 length。比如 3 + 2，就是构造一个长度为 3 的数组类型，再构造一个长度为 2 的数组类型，然后合并成一个数组，取 length。

构造多长的数组是不确定的，需要递归构造，这个我们实现过：

```ts
type BuildArray<
    Length extends number, 
    Ele = unknown, 
    Arr extends unknown[] = []
> = Arr['length'] extends Length 
        ? Arr 
        : BuildArray<Length, Ele, [...Arr, Ele]>;
```

> 👀 这里的讲解代码略，略。

构造数组实现了，那么基于它就能实现加法：

```typescript
type Add<Num1 extends number, Num2 extends number> = 
    [...BuildArray<Num1>,...BuildArray<Num2>]['length'];
```

<img src="https://s2.loli.net/2022/05/04/qU3keIVWlCs7ona.png" alt="image-20220504005852851" style="zoom:50%;" />

##### Subtract

加法是构造数组，那减法怎么做呢？减法是从数值中去掉一部分，很容易想到可以 <font color=FF0000>通过数组类型的提取来做</font>。比如 3 是 `[unknown, unknown, unknown]` 的数组类型，提取出 2 个元素之后，剩下的数组再取 length 就是 1。

所以减法的实现是这样的：

```ts
type Subtract<Num1 extends number, Num2 extends number> = 
    BuildArray<Num1> extends [...arr1: BuildArray<Num2>, ...arr2: infer Rest]
        ? Rest['length']
        : never;
```

> **注：**这里的关键是 Arr 和 Arr2 除非一模一样（元素顺序也一样），才会 `Arr extends Arr2 ? true : false` 为 true；所以，Rest 才会是提取的结果。
>
> 另外，这里 Num1 必须 不小于 Num2，否则结果会是 never。

类型参数 Num1、Num2 分别是被减数和减数，通过 extends 约束为 number。

构造 Num1 长度的数组，<font color=FF0000>通过模式匹配提取出 Num2 长度个元素，**剩下的放到 infer 声明的局部变量 Rest 里**</font>。取 Rest 的长度返回，就是减法的结果。

<img src="https://s2.loli.net/2022/05/04/5dbXhNnOpYs6jBV.png" alt="image-20220504010518304" style="zoom:50%;" />

##### Multiply

我们把加法转换为了数组构造，把减法转换为了数组提取。那乘法怎么做呢？<font color=FF0000>**乘法就是多个加法结果的累加**</font>。

那么我们在减法的基础上，多加一个参数来传递中间结果的数组，算完之后再取一次 length 就能实现乘法：

```ts
type Mutiply<
    Num1 extends number,
    Num2 extends number,
    ResultArr extends unknown[] = []
> = Num2 extends 0 ? ResultArr['length']
        : Mutiply<Num1, Subtract<Num2, 1>, [...BuildArray<Num1>, ...ResultArr]>;
```

类型参数 Num1 和 Num2 分别是被加数和加数。

因为乘法是多个加法结果的累加，我们加了一个类型参数 ResultArr 来保存中间结果，默认值是 []，相当于从 0 开始加。每加一次就把 Num2 减一，直到 Num2 为 0，就代表加完了。加的过程就是往 ResultArr 数组中放 Num1 个元素。这样递归的进行累加，也就是递归的往 ResultArr 中放元素。

最后取 ResultArr 的 length 就是乘法的结果。

<img src="https://s2.loli.net/2022/05/04/fOsdUMyD6gvEPSV.png" alt="image-20220504012228977" style="zoom:50%;" />

##### Divide

乘法是递归的累加，那除法不就是递归的累减么？<font color=FF0000>**除法的实现就是被减数不断减去减数，直到减为 0，记录减了几次就是结果**</font>。

```ts
type Divide<
    Num1 extends number,
    Num2 extends number,
    CountArr extends unknown[] = []
> = Num1 extends 0 ? CountArr['length']
        : Divide<Subtract<Num1, Num2>, Num2, [unknown, ...CountArr]>;
```

类型参数 Num1 和 Num2 分别是被减数和减数。类型参数 CountArr 是用来记录减了几次的累加数组。

如果 Num1 减到了 0 ，那么这时候减了几次就是除法结果，也就是 `CountArr['length']` ；否则继续递归的减，让 Num1 减去 Num2，并且 CountArr 多加一个元素代表又减了一次。

> 👀 有点神奇的是，Divide 的 “代码逻辑” 上没有找到处理 无法整除的情况；但是 `Divide<10, 3>` 结果会是 never 。有点没搞懂 TODO

<img src="https://s2.loli.net/2022/05/04/PKnTMeWisY7lVyr.png" alt="image-20220504012916479" style="zoom:50%;" />

#### 数组长度实现计数

##### StrLen

数组长度可以取 length 来得到，但是<font color=FF0000>**字符串类型不能取 length**</font> （见下面“注” ），所以我们来<font color=FF0000>实现一个求字符串长度的高级类型</font>。

字符串长度不确定，明显要用递归。每次取一个并计数，直到取完，就是字符串长度。

```ts
type StrLen<Str extends string, CountArr extends unknown[] = []> = 
     Str extends `${infer First}${infer Rest}`
         ? StrLen<Rest, [...CountArr, First]> // 注：自己实现的时候，展开运算符漏了；其他没什么问题
         : CountArr["length"]
```

> 👀 TS 中字符串没有 "length" 属性，所以才需要将其转换成 数组，再获取数组的长度。另外，这里实现没什么问题，讲解略。

##### GreaterThan

能够做计数了，那也就能做两个数值的比较。

我们往一个数组类型中不断放入元素取长度，<font color=FF0000>如果（中间数组的长度）先到了 A，那就是 B 大；否则是 A 大</font>：

```ts
type GreaterThan<
    Num1 extends number,
    Num2 extends number,
    CountArr extends unknown[] = []
> = Num1 extends Num2
    ? false
    : CountArr['length'] extends Num2
        ? true
        : CountArr['length'] extends Num1
            ? false
            : GreaterThan<Num1, Num2, [...CountArr, unknown]>
```

类型参数 Num1 和 Num2 是待比较的两个数。类型参数 CountArr 是计数用的，会不断累加，默认值是 [] 代表从 0 开始。

如果 `Num1 extends Num2` 成立，代表相等，直接返回 false 。否则，判断计数数组的长度，如果先到了 Num2，那么就是 Num1 大，返回 true。反之，如果先到了 Num1，那么就是 Num2 大，返回 false。如果都没到就往计数数组 CountArr 中放入一个元素，继续递归。

这样就实现了数值比较。

<img src="https://s2.loli.net/2022/05/04/DNp9nfeUbqRiPES.png" alt="image-20220504021805506" style="zoom:50%;" />

##### Fibonacci

谈到了数值运算，就不得不提起经典的 Fibonacci 数列的计算。*F*(0) = 1，*F*(1) = 1, *F*(n) = *F*(n - 1) + *F*(n - 2)  (*n* ≥ 2，*n* ∈ N*)

也就是递归的加法，在 TypeScript 类型编程里用构造数组来实现这种加法：

```ts
type FibonacciLoop<
    PrevArr extends unknown[], 
    CurrentArr extends unknown[], 
    IndexArr extends unknown[] = [], 
    Num extends number = 1
> = IndexArr['length'] extends Num
    ? CurrentArr['length']
    : FibonacciLoop<CurrentArr, [...PrevArr, ...CurrentArr], [...IndexArr, unknown], Num> 

type Fibonacci<Num extends number> = FibonacciLoop<[1], [], [], Num>;
```

类型参数 PrevArr 是代表之前的累加值的数组。类型参数 CurrentArr 是代表当前数值的数组。类型参数 IndexArr 用于记录 index，每次递归加一，默认值是 []，代表从 0 开始。类型参数 Num 代表求数列的第几个数。

判断当前 index 也就是 `IndexArr['length']` 是否到了 Num，到了就返回当前的数值 `CurrentArr['length']` ；否则求出当前 index 对应的数值，用之前的数加上当前的数 `[...PrevArr, ... CurrentArr]`。然后继续递归，index + 1，也就是 `[...IndexArr, unknown]` 。

这就是递归计算 Fibinacci 数列的数的过程。

<img src="https://s2.loli.net/2022/05/04/BTbFrpmSZARzePO.png" alt="image-20220504022131600" style="zoom:50%;" />



### 联合分散可简化

联合类型在类型编程中是比较特殊的，TypeScript 对它做了专门的处理，写法上可以简化，但也增加了一些认知成本。

#### 分布式条件类型 ( Distributive conditional types )

> 💡 分布式条件类型在官方文档中也有介绍，也做了笔记，见 [[#Conditional Types#Distributive Conditional Types]]

当 <font color=FF0000>类型参数为联合类型</font>，并且在 <font color=fuchsia>**条件类型**</font> （ 👀 即 `extends ? :` 。另外，这个很重要，下面 [[#IsUnion]] 中会用到这个特性 ）左边直接引用该类型参数的时候：<font color=FF0000>TypeScript 会把 **每一个元素单独传入来做类型运算，最后再合并成联合类型**</font>，这种语法叫做「分布式条件类型」。

> 💡 补充
>
> > TS 的这种分配条件规则，<font color=dodgerBlue>可以用集合的分配率来很好地解释</font>：$(B\cup C)\cap A = (B\cap A)\cup (C\cap A)$。我们可以把传入的联合类型看成是公式中的 $(B \cup C)$ ，$A$ 为 `extends` 后面的类型，根据分配率扩展后，最终的结果是 `B extends A ? X : Y | C extends A ？X : Y` 。
> >
> > <font color=dodgerBlue>如果想要打破 TS 这种默认分配规则</font>，可以使用元组类型将泛型看成一个整体，也就是单独一个集合。
> >
> > ```ts
> > type isTwo<T> = T extends 2 ? true: false;
> > 
> > type res3 = isTwo<[1 | 2]>; // false
> > ```
> >
> > 摘自：[从集合论的角度理解 TypeScript # 理解TS类型运算](https://juejin.cn/post/7264549320362934309#heading-8)

比如这样一个联合类型：

```typescript
type Union = 'a' | 'b' | 'c';
```

我们想把其中的 a 大写，就可以这样写：

```typescript
type UppercaseA<Item extends string> = 
    Item extends 'a' ?  Uppercase<Item> : Item; // 注：类似于索引类型的 [Key in keyof Obj]: T[Key]，不过是不同的
```

<img src="https://s2.loli.net/2022/05/04/WpOK3MyP6rn2XNQ.png" alt="image-20220504135043088" style="zoom:50%;" />

可以看到，我们类型参数 Item 约束为 string，条件类型的判断中也是判断是否是 a，但传入的是联合类型。

这就是 TypeScript 对联合类型在条件类型中使用时的特殊处理：会把联合类型的每一个元素单独传入做类型计算，最后合并。

<font color=FF0000>**这和联合类型遇到字符串时的处理一样：**</font>

<img src="https://s2.loli.net/2022/05/04/VvMn5UEaYLQDOWj.png" alt="image-20220504135905352" style="zoom:50%;" />

这样确实是简化了类型编程逻辑，不需要递归提取每个元素再处理。

TypeScript 之所以这样处理联合类型也很容易理解，因为：联合类型的每个元素都是互不相关的，不像数组、索引、字符串那样元素之间是有关系的。所以设计成了每一个单独处理，最后合并。

##### CamelcaseUnion

Camelcase 我们实现过，就是提取字符串中的字符，首字母大写以后重新构造一个新的。

```ts
type Camelcase<Str extends string> = 
    Str extends `${infer Left}_${infer Right}${infer Rest}`
        ? `${Left}${Uppercase<Right>}${Camelcase<Rest>}`
        : Str; // 注：第一遍写没写出来，题目看错了（是由 snake_case 变成 camelCase ）；第二遍写，这里 Str 写成 Rest 了
```

提取 `_` 左右的字符，把右边字符大写之后构造成新的字符串，余下的字符串递归处理。

<img src="https://s2.loli.net/2022/05/04/DOWzqgQj8xhulNY.png" alt="image-20220504142306146" style="zoom:50%;" />

另外，由于上面讲的特性，这里 `Camelcase` 中类型参数可以放入一个联合类型值，效果一样；这里只放一下截图。另外，下面也会讲。

<img src="https://s2.loli.net/2022/05/04/tS5a1cAPTGDYKxR.png" alt="image-20220504142525257" style="zoom:50%;" />

如果是对字符串数组做 Camelcase，那就要递归处理每一个元素：

```ts
type CamelcaseArr<
  Arr extends unknown[] // 👀 这里 写 string[] 下面写 CamelcaseArr<RestArr, string[]> 会报错，不知道为什么 TODO
> = Arr extends [infer Item, ...infer RestArr]
        ? [Camelcase<Item & string>, ...CamelcaseArr<RestArr>] //👀 这里 & string 要注意，没想到这样写。
        : [];
```

类型参数 Arr 为待处理数组。递归提取每一个元素做 Camelcase，<font color=FF0000>因为 Camelcase 要求传入 string ，这里要 `& string` 来变成 string 类型</font>。

<img src="https://s2.loli.net/2022/05/04/3QRwO5lZfN79gbq.png" alt="image-20220504162214676" style="zoom:50%;" />

**那如果是联合类型呢？**

联合类型不需要递归提取每个元素，TypeScript 内部会把每一个元素传入单独做计算，之后把每个元素的计算结果合并成联合类型。

```ts
type CamelcaseUnion<Item extends string> = 
  Item extends `${infer Left}_${infer Right}${infer Rest}` 
    ? `${Left}${Uppercase<Right>}${CamelcaseUnion<Rest>}` 
    : Item;
```

这不和单个字符串的处理没区别么？没错，对联合类型的处理和对单个类型的处理没什么区别，TypeScript 会把每个单独的类型拆开传入。不需要像数组类型那样需要递归提取每个元素做处理。

确实简化了很多，好像都是优点？也不全是，其实这样处理也增加了一些认知成本。

##### IsUnion

判断联合类型我们会这样写：

```ts
type IsUnion<A, B = A> =
    A extends A
        ? [B] extends [A] // 👀 注意这里的 [B] 和 [A]，A 和 B 都使用 [] 包裹，原因见下面。
            ? false
            : true
        : never
```

这段逻辑有点奇怪（见上面代码中的 “注” ），这就是分布式条件类型带来的认知成本。

我们先来看这样一个类型：

```ts
type TestUnion<A, B = A> = 
     A extends A
        ? { a : A, b : B }
        : never;

type TestUnionResult = TestUnion<'a' | 'b' | 'c'>;
```

传入联合类型 `'a' | 'b' | 'c'` 的时候，结果是这样的：

<img src="https://s2.loli.net/2022/05/04/n384fWeyRqhNkms.png" alt="image-20220504162905687" style="zoom:50%;" />

A 和 B 都是同一个联合类型，为啥值还不一样呢？

因为：<font color=FF0000>**条件类型中如果左边的类型是联合类型，会把每个元素单独传入做计算，而 <font size=4>右边不会</font>**</font>。所以 A 是 'a' 的时候，B 是 'a' | 'b' | 'c'， A 是 'b' 的时候，B 还是 'a' | 'b' | 'c' 。所以，<font color=FF0000>**可以利用这个特点就可以实现 Union 类型的判断**</font>

```ts
type IsUnion<A, B = A> =
    A extends A
        ? [B] extends [A]
            ? false
            : true
        : never
```

类型参数 A、B 是待判断的联合类型，B 默认值为 A，也就是同一个类型。

`A extends A` 这段看似没啥意义，主要是为了触发「分布式条件类型」，让 A 的每个类型单独传入。`[B] extends [A]` 这样 <font color=FF0000>**不直接写 B 就可以避免触发「分布式条件类型」**</font>（**注：**这里的避免的原理见 [[#分布式条件类型]] 开头的定义 ），<font color=FF0000>那么 B 就是 整个联合类型</font>。B 是联合类型整体，而 A 是单个类型，自然不成立，而其它类型没有这种特殊处理，A 和 B 都是同一个，怎么判断都成立。

> 👀 上面最后一句没看懂。不过，经过实验：`'a' extends ['a' | 'b' | 'c'] ? true : false`  结果为 false，而 `['a'] extends ['a' | 'b' | 'c'] ? true : false` 结果为 true 。

利用这个特点就可以判断出是否是联合类型。

<font size=4>**其中有两个点比较困惑，我们重点记一下**</font>

**当 A 是联合类型时：**

- **`A extends A` 这种写法是<font color=FF0000>为了触发分布式条件类型</font>，让每个类型单独传入处理的，<font color=FF0000>没别的意义</font>。**
- **`A extends A` 和 `[A] extends [A]` 是不同的处理，<font color=FF0000>前者是单个类型和整个类型做判断</font>，<font color=FF0000>后者两边都是整个联合类型</font>，因为<font color=FF0000>只有 extends 左边直接是类型参数才会触发分布式条件类型</font>。**

#### 练习

##### BEM

bem 是 css 命名规范，用 block__element--modifier 的形式来描述某个区块下面的某个元素的某个状态的样式。

那么我们可以写这样一个高级类型，传入 block、element、modifier，返回构造出的 class 名；这样使用：

```ts
type bemResult = BEM<'guang', ['aaa', 'bbb'], ['warning', 'success']>;
```

它的实现就是 三部分的合并，但传入的是数组，要递归遍历取出每一个元素来和其他部分组合，<font color=FF0000>这样太麻烦了</font>。

而<font color=FF0000>**如果是联合类型就不用递归遍历了**</font>，因为联合类型遇到字符串也是会单独每个元素单独传入做处理。

<font color=fuchsia>**数组转联合类型可以这样写：**</font>

```ts
type union = ['foo', 'bar'][number]
```

<img src="https://s2.loli.net/2022/05/07/mozrZSqcPNkYMDh.png" alt="image-20220504183301073" style="zoom:50%;" />

那么 BEM 就可以这样实现：

```ts
type BEM<
    Block extends string,
    Element extends string[],
    Modifiers extends string[]
> = `${Block}__${Element[number]}--${Modifiers[number]}`; // 👀 注意这里的 [number]，就是上面的用法
```

类型参数 Block、Element、Modifiers 分别是 bem 规范的三部分，其中 Element 和 Modifiers 都可能多个，约束为 `string[]` 。

<font color=FF0000>构造一个字符串类型，其中 Element 和 Modifiers 通过索引 ( `[number]` ) 访问来变为联合类型</font>。

<font color=FF0000 size=4>**字符串类型中遇到联合类型的时候，会每个元素单独传入计算**</font>；也就是这样的效果：

<img src="https://s2.loli.net/2022/05/04/gqpcXQFdVCv8Elr.png" alt="image-20220504191243167" style="zoom:50%;" />

##### AllCombinations

我们再来实现一个全组合的高级类型，也是联合类型相关的：<font color=FF0000>希望传入 'A' | 'B' 的时候，能够返回所有的组合： 'A' | 'B' | 'BA' | 'AB'</font> 。

这种全组合问题的实现思路就是两两组合，组合出的字符串再和其他字符串两两组和：比如 'A' | 'B' | 'c'，就是 A 和 B、C 组合，B 和 A、C 组合，C 和 A、B 组合。然后组合出来的字符串再和其他字符串组合。任何两个类型的组合有四种：A、B、AB、BA 。

```ts
type Combination<A extends string, B extends string> =
    | A
    | B
    | `${A}${B}`
    | `${B}${A}`;
```

然后构造出来的字符串再和其他字符串组合。

所以全组合的高级类型就是这样：

```ts
type AllCombinations<A extends string, B extends string = A> = 
    A extends A
        ? Combination<A, AllCombinations<Exclude<B, A>>>
        : never;
```

类型参数 A、B 是待组合的两个联合类型，B 默认是 A 也就是同一个。

`A extends A` 的意义就是让联合类型每个类型单独传入做处理，上面 ( [[#IsUnion]] ) 我们刚学会。

A 的处理就是 A 和 “B 中去掉 A 以后” 的所有类型组合，也就是 `Combination<A, B 去掉 A 以后的所有组合>` 。

而 B 去掉 A 以后的所有组合就是 `AllCombinations<Exclude<B, A>>` ，所以全组合就是 `Combination<A, AllCombinations<Exclude<B, A>>>` 。

<img src="https://s2.loli.net/2022/05/04/6tuD98l3LUcmH41.png" alt="image-20220504200800250" style="zoom:50%;" />

这里利用到了分布式条件类型的特性，通过 A extends A 来取出联合类型中的单个类型。



### 特殊特性要记清

##### 特殊类型的特性

TypeScript 类型系统中有些类型比较特殊：比如 any、never、联合类型；比如 class 有 public、protected、private 的属性；比如「索引类型」有具体的索引和可索引签名，索引还有可选和非可选。

如果给我们一种类型让我们判断是什么类型，应该怎么做呢？

**类型的判断要根据它的特性来，比如判断联合类型就要根据它的 distributive （ 👀 即“分布式条件类型” ） 的特性。**

我们分别看一下这些特性：

##### IsAny

如何判断一个类型是 any 类型呢？要根据它的特性来：**<font color=FF0000>any 类型与任何类型的交叉都是 any</font> ，也就是 `1 & any` 结果是 any**（👀 有点“粘性” ( sticky ) 的意味？）。所以，可以这样写：

```typescript
type IsAny<T> = 'foo' extends ('bar' & T) ? true : false
```

这里的 'foo' 和 'bar' 可以换成任意类型（ 👀 这里 'foo' 和 'bar' 都代表一种类型，所以可以换成 'string' 和 'number' ）

<img src="https://s2.loli.net/2022/05/04/IjWPzOXD1SAcqkN.png" alt="image-20220504202335587" style="zoom:50%;" />

> 👀 下面 [[#IsNever]] 还提及了为什么 any 不能直接使用 `extends ? :` 去判断（简单来说，会返回 `extends ? :` 设定的 trueVal 和 falseVal 的联合 ( Union ) ）

##### IsEqual

之前我们实现 IsEqual 是这样写的：

```typescript
type IsEqual<A, B> = (A extends B ? true : false) & (B extends A ? true : false);
```

问题也出在 any 的判断上：

<img src="https://s2.loli.net/2022/05/04/q5r3I1gl6ZNo24L.png" alt="image-20220504212729339" style="zoom:50%;" />

<font color=FF0000>因为 any 可以是任何类型，任何类型也都是 any，所以当这样写判断不出 any 类型来</font>。

所以，我们会这样写：

```ts
type IsEqual<A, B> = (<T>() => T extends A ? 1 : 2) extends (<T>() => T extends B ? 1 : 2)
    ? true : false;
```

<img src="https://s2.loli.net/2022/05/04/P3rGUbHFZ5xJKiq.png" alt="image-20220504213554129" style="zoom:50%;" />

这是因为 TS 对这种形式的类型做了特殊处理，是一种 hack 的写法，它的解释要从 TypeScript 源码找答案了，放到原理篇我们一起读下 TypeScript 源码。这里暂时就这样写吧。 TODO

> 👀 下面 [[#IsTuple]] 有实现一个 NonEqual ，有说实现原理，可以参考下。

##### IsUnion

还记得怎么判断 union 类型么？要根据它遇到条件类型时会分散成单个传入做计算的特性：

```typescript
type IsUnion<A, B = A> =
    A extends A
        ? [B] extends [A]
            ? false
            : true
        : never
```

这里的 A 是单个类型，B 是整个联合类型，所以根据 [B] extends [A] 是否成立来判断是否是联合类型。具体可见 [[#联合分散可简化#IsUnion]]

##### IsNever

never 在条件类型中也比较特殊：<font color=FF0000>如果条件类型左边是类型参数，并且传入的是 never，那么**直接返回 never **</font>。如下示例：

```typescript
type TestNever<T> = T extends number ? 1 : 2 // never
```

<img src="https://s2.loli.net/2022/05/04/5RXMdAesBKrVatl.png" alt="image-20220504214213956" style="zoom:50%;" />

所以：要判断 never 类型，就不能直接 `T extends number` ，可以用数组包装；如下：

```typescript
type IsNever<T> = [T] extends [never] ? true : false
```

<img src="https://s2.loli.net/2022/05/04/Q9uWDqnaITFrJOL.png" alt="image-20220504214504633" style="zoom:50%;" />

> 💡 这里没有说为什么要用数组包装，参考下面的引用，可以得到答案：
> > 在 TS 中，<font color=fuchsia>never 被看成是一个 **空联合类型**</font>，结合在 extends 中学到的知识，我们可以这样做
> > ```ts
> > type IsNever<T> = [T] extends [never] ? true : false;
> > ```
> 摘自：[从 never 切入，摸透 TypeScript 的学习思路](https://mp.weixin.qq.com/s/mEAhlkAQtff28Jd_yWkWuw)

除了上述 never 以外，any 在「条件类型」中也比较特殊：如果类型参数为 any，会直接返回 trueType（下例中的 1 ） 和 falseType （下例中的 2 ）的 联合 ( Union ) ：

```ts
type TestAny<T> = T extends number ? 1 : 2;
```

<img src="https://s2.loli.net/2022/05/04/EGy3OoDaPwuBk7Z.png" alt="image-20220504214930961" style="zoom:50%;" />

> 💡 补充
>
> <font color=red>any 代表了任何可能的类型</font>，当使用 `any extends` 时，它包含了“**让条件成立的一部分**”，以及“**让条件不成立的一部分**”。
>
> 摘自：[重新认识 TypeScript类型系统 # 顶层类型](https://juejin.cn/post/7374614838008774656#heading-18)

联合类型、never、any 在作为条件类型的类型参数时的这些特殊情况，也会在后面的原理篇来解释原因。

##### IsTuple

元组类型怎么判断呢？它和数组有什么区别呢？**元组类型也是数组类型，但 <font color=FF0000 size=4>每个元素都是只读的</font>；并且 <font color=FF0000 size=4>元组的 length 是数字字面量</font>，而 <font color=FF0000 size=4>数组的 length 是 number </font>。**

如图，元组和数组的 length 属性值是有区别的：

<img src="https://s2.loli.net/2022/05/04/nwJHC3ABDh1SITM.png" alt="image-20220504215632945" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/05/04/1OaXxn25DKpvsHt.png" alt="image-20220504215702395" style="zoom:50%;" />

那我们就可以根据这两个特性（ readonly 和 length 是数字字面量 ）来判断元组类型：

```ts
type IsTuple<T> = 
    // 👀 第一：readonly 的性质没想到，第二：没想到可以 readonly 这样判断。另外，`params:` 可加可不加
    T extends readonly [...params: infer Eles]
        ? NotEqual<Eles['length'], number> // 注：由上图可知，number 没有引号
        : false
```

类型参数 T 是要判断的类型。

首先判断 T 是否是数组类型，如果不是则返回 false。如果是，继续判断 length 属性是否是 number；如果是数组并且 length 不是 number 类型，那就代表 T 是元组。

###### NotEqual 的实现

```ts
type NotEqual<A, B> = (<T>() => T extends A ? 1 : 2) extends (<T>() => T extends B ? 1 : 2)
    ? false : true;
```

A 是 B 类型，并且 B 也是 A 类型，那么就是同一个类型，返回 false，否则返回 true。

**传入元组时：**

<img src="https://s2.loli.net/2022/05/04/5TdDJwM4WXPVjC7.png" alt="image-20220504222653620" style="zoom:50%;" />

**传入数组时：**

<img src="https://s2.loli.net/2022/05/06/iDYgGmUjy2qzeLO.png" alt="image-20220504223131230" style="zoom:50%;" />

##### UnionToIntersection

<font color=FF0000>**类型之间是有父子关系的，更具体的那个是子类型**；比如 A 和 B 的交叉类型 `A & B` 就是联合类型 `A | B` 的子类型，因为更具体</font>。

<font color=FF0000>**如果允许父类型赋值给子类型，就叫做<font size=4>「逆变」</font>。如果允许子类型赋值给父类型，就叫做<font size=4>「协变」</font>**</font>。详细概念见  [[#逆变、协变、双向协变、不变]]

在 TypeScript 中 <font color=fuchsia><font size=4>**「函数参数」是有「逆变」的性质**</font> 的</font>，<font color=dodgerBlue>也就是</font>：<font color=fuchsia>如果参数可能是多个类型，参数类型会变成它们的交叉类型</font>。

> 👀 这部分内容下面有讲到 [[#逆变 ( contravariant )]]

所以联合转交叉可以这样实现 ：

```typescript
type UnionToIntersection<U> = 
    (U extends U ? (x: U) => unknown : never) extends (x: infer R) => unknown
        ? R
        : never
```

类型参数 U 是要转换的联合类型。

`U extends U` 是为了触发 “联合类型” 的 distributive（即：“分布式条件类型” ） 的性质，让每个类型单独传入做计算，最后（TS自动做）合并（交叉操作）。利用 U 做为参数构造一个函数类型，<font color=FF0000>通过模式匹配取参数的类型</font>，利用函数参数的逆变的性质，就能实现联合转交叉。（ 👀 这里实现原理可见 [[#逆变性质有什么用]]）。结果就是交叉类型：

<img src="https://s2.loli.net/2022/05/07/ORaw4WoSYd78MQk.png" alt="image-20220507123618479" style="zoom:50%;" />

##### GetOptional

<font color=dodgerBlue>如何提取索引类型中的可选索引呢？</font>

这也要利用可选索引的特性：**可选索引的值为 undefined 和 值类型 的联合类型**。

<img src="https://s2.loli.net/2022/05/04/uxlX6D7yPq2fWAY.png" alt="img" style="zoom:65%;" />

> 👀 在当前 VS Code & typescript@4.5.4 环境下，不知道为什么只能显示出 `age?: number` ，不能显示出 `age?: number | undefined` 

```ts
type GetOptional<Obj extends Record<string, any>> = {
    [
        Key in keyof Obj 
            as {} extends Pick<Obj, Key> ? Key : never // 👀 这里将 Pick<Obj, Key> 换成 Obj[Key] 没法得到预期的结果
    ] : Obj[Key];
}
```

类型参数 Obj 为待处理的索引类型，类型约束为索引为 string、值为任意类型的索引类型 `Record<string, any>` 。

用映射类型的语法重新构造索引类型，索引是之前的索引也就是 `Key in keyof Obj` ，但要做一些过滤，也就是 as 之后的部分。过滤的方式就是单独取出该索引之后，<font color=FF0000>判断空对象是否是其子类型</font>（**注：**原理见上面 ）。

这里的 <font color=FF0000>**Pick 是 TS 提供的内置高级类型，就是取出某个 Key 构造新的索引类型**</font>：

```typescript
type Pick<T, K extends keyof T> = { [P in K]: T[P] }
```

比如单独取出 age 构造的新的索引类型是这样的：

<img src="https://s2.loli.net/2022/05/04/TOiut6sRFq94xLv.png" alt="img" style="zoom:75%;" />

<font color=FF0000 size=4>**因为 age 可能为 undefined，也就是索引类型可能是 {}** </font>；所以 `{} extends Pick<Obj, Key>` 就能过滤出可选索引。（<font color=FF0000>可选的意思就是有或者没有，<font size=4>**没有的时候就是空的索引类型**</font></font>）。值的类型依然是之前的，也就是 `Obj[Key]` 。

这样，就能过滤出所有可选索引，构造成新的索引类型：

<img src="https://s2.loli.net/2022/05/04/CQY1K42Zq7gVaj9.png" alt="img" style="zoom:65%;" />

##### RemoveIndexSignature

索引类型可能有索引，也可能有可索引签名。

比如：

```typescript
type Dong = {
  [key: string]: any;
  sleep(): void;
}
```

这里的 sleep 是具体的索引，`[key: string]: any` 就是可索引签名，代表<font color=FF0000>可以添加 **任意个** string 类型的索引</font>（👀 “可索引签名” 和 接口的“任意属性” 有点类似；“任意属性” 可见 [[#对象的类型——接口]] 中的 “任意属性” ）

**如果想删除索引类型中的可索引签名呢？**

同样根据它的性质：<font color=FF0000>**索引签名不能构造成字符串字面量类型，因为它没有名字；而其他索引可以**</font>。所以，就可以这样过滤：

```typescript
type RemoveIndexSignature<Obj extends Record<string, any>> = {
  [
      Key in keyof Obj 
          as Key extends `${infer Str}` ? Str : never /* 👀注意这里 `${infer Str}` 的写法，trick */
  ]: Obj[Key]
}
```

类型参数 Obj 是待处理的索引类型，约束为 `Record<string, any>` 。

通过映射类型语法构造新的索引类型，索引是之前的索引 `Key in keyof Obj` ；但要做一些过滤，也就是 as 之后的部分：<font color=FF0000>如果索引是字符串字面量类型，那么就保留，否则返回 never，代表过滤掉。值保持不变，也就是 `Obj[Key]`</font> 。这样就可以过滤掉可索引签名：

<img src="https://s2.loli.net/2022/05/05/4ZKsiVW6rXD8bLw.png" alt="image-20220505001613812" style="zoom:50%;" />

##### ClassPublicProps

如何过滤出 class 的 public 的属性呢？

也同样是根据它的特性：<font color=FF0000 size=4>**keyof 只能拿到 class 的 public 索引，private 和 protected 的索引会被忽略**</font>。

比如这样一个 class：

```typescript
class Dong {
  public name: string;
  protected age: number;
  private hobbies: string[];

  constructor() {
    this.name = 'dong';
    this.age = 20;
    this.hobbies = ['sleep', 'eat'];
  }
}
```

keyof 拿到的只有 name：

<img src="https://s2.loli.net/2022/05/05/YZGK1RBz6v9T2fC.png" alt="image-20220505002343657" style="zoom:50%;" />

所以，我们就可以根据这个特性实现 public 索引的过滤（ 👀 而不需要添加其他判断）：

```typescript
type ClassPublicProps<Obj extends Record<string, any>> = {
  // 👀 在尝试的时候，还用 as 添加了判断 ( as Key extends keyof Obj ? Key : never )；虽然结果没问题；但这是没有必要的
  [Key in keyof Obj]: Obj[Key]
}
```

类型参数 Obj 为带处理的索引类型，类和对象都是索引类型，约束为 `Record<string, any>` 。

构造新的索引类型，索引是 `keyof Obj` 过滤出的索引，也就是 public 的索引。值保持不变，依然是 `Obj[Key]` 。这样就能过滤出 public 的属性：

<img src="https://s2.loli.net/2022/05/05/gbxDwvYT6e5ynO4.png" alt="image-20220505005120760" style="zoom:50%;" />

##### as const

> 👀 as const 的名称是 “ const 断言” ( const assertions )，官方文档见 [TS Doc - TypeScript 3.4 # const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

TypeScript <font color=red>默认推导出来的类型并不是字面量类型</font>。

比如对象：

<img src="https://s2.loli.net/2022/12/15/fGg78DMtwUBWOnp.png" alt="image-20221215173931113" style="zoom:65%;" />

比如数组：

<img src="https://s2.loli.net/2022/12/15/4Vnfjz9CGdROh2l.png" alt="image-20221215174144657" style="zoom:65%;" />

但是<font color=dodgerBlue>类型编程很多时候是需要推导出字面量类型的</font>，这时候<font color=red>就需要用 `as const`</font> ：

<img src="https://s2.loli.net/2022/12/15/b8n4oDrXOMqkHtI.png" alt="image-20221215174452766" style="zoom:65%;" />

<img src="https://s2.loli.net/2022/12/15/M6EfGyHQzTW2sae.png" alt="image-20221215174310939" style="zoom:65%;" />

因为<font color=dodgerBlue>加上 `as const` 后推导出来的类型是带有 readonly 修饰的</font>，所以<font color=red>在通过模式匹配提取类型的时候也要加上 readonly 的修饰</font>才行。

> <font color=dodgerBlue>const 是常量的意思</font>，<font color=red>也就是说这个变量首先是一个字面量值，而且还不可修改，有字面量和 readonly 两重含义</font>。所以加上 `as const` 会推导出 readonly 的字面量类型。

比如反转上面的三个元素的元组类型，不加上 readonly 再匹配是匹配不出来的：

<img src="https://s2.loli.net/2022/12/15/ndHRlY6jFPeVTNO.png" alt="image-20221215180447220" style="zoom:65%;" />

加上 readonly 之后就可以正常匹配了：

<img src="https://s2.loli.net/2022/12/15/e4mxAqFGXD6iQsa.png" alt="image-20221215180636495" style="zoom:65%;" />

#### 总结

学完前面 5 个套路，我们已经能够实现各种类型编程逻辑了，但一些类型的特性还是要记一下。在判断或者过滤类型的时候会用到：

- any 类型与任何类型的交叉都是 any，也就是 `1 & any` 结果是 any，可以用这个特性判断 any 类型。

  > 👀 感觉有点类似于 [[现代 JavaScript 教程 阅读笔记#NaN]] 中提及的 “粘性” 概念

- 联合类型作为类型参数出现在条件类型左侧时，会分散成单个类型传入，最后合并。

- never 作为类型参数出现在条件类型左侧时，会直接返回 never 。

- any 作为类型参数出现在条件类型左侧时，会直接返回 trueType 和 falseType 的联合类型。

- 元组类型也是数组类型，但每个元素都是只读的，并且 length 是数字字面量，而数组的 length 是 number。可以用来判断元组类型

- <font color=FF0000>函数参数处会发生逆变，可以用来实现联合类型转交叉类型</font>。

- 可选索引的值为 undefined 和值类型的联合类型。可以用来过滤可选索引，反过来也可以过滤非可选索引。

- 索引类型的索引为字符串字面量类型，而可索引签名不是，可以用这个特性过滤掉可索引签名。

- keyof 只能拿到 class 的 public 的索引，可以用来过滤出 public 的属性。

- 默认推导出来的不是字面量类型，加上 `as const` 可以推导出字面量类型，但带有 readonly 修饰，这样模式匹配的时候也得加上 readonly 才行。

这些类型的特性要专门记一下，其实过两遍就记住了。

熟悉了这些特殊的特性，配合提取、构造、递归、数组长度计数、联合分散这五种套路，就可以实现各种类型体操。



### 类型体操顺口溜

> 👀 下面的内容是对前面六章的总结概括，虽然有重复；但是，再复习一遍、部分代码再写一遍，也很好

<font size=4>**类型体操顺口溜**</font>

**模式匹配做提取，重新构造做变换。**

**递归复用做循环，数组长度做计数。**

**联合分散可简化，特殊特性要记清。**

**基础扎实套路熟，类型体操可通关。**

#### 模式匹配做提取

就像字符串可以通过正则提取子串一样，<font color=FF0000>TypeScript 的类型也可以通过 匹配一个模式类型来提取部分类型到 infer 声明的局部变量中返回</font>。比如提取函数类型的返回值类型：

```ts
type GetReturnType<Func extends Function> = 
    Func extends (...args: unknown[]) => infer ReturnType 
        ? ReturnType : never;
```

#### 重新构造做变换

<font color=FF0000>TypeScript 类型系统 **可以通过 type 声明类型变量，通过 infer 声明局部变量，类型参数在类型编程中也相当于局部变量**，但是 <font size=4>**它们都不能做修改，想要对类型做变换只能构造一个新的类型，在构造的过程中做过滤和转换**</font></font>。

在 字符串、数组、函数、索引等类型都有很多应用，特别是索引类型。

比如把索引变为大写：

```ts
type UppercaseKey<Obj extends Record<string, any>> = { 
    [Key in keyof Obj as Uppercase<Key & string>]: Obj[Key] // 注：这里 & string 漏掉了
}
```

#### 递归复用做循环

在 TypeScript 类型编程中，<font color=FF0000>遇到数量不确定问题时，就要条件反射的想到递归</font>；每次只处理一个类型，剩下的放到下次递归，直到满足结束条件，就处理完了所有的类型。

比如把长度不确定的字符串转为联合类型：

```ts
type StringToUnion<Str extends string> = 
    Str extends `${infer First}${infer Rest}`
        ? First | StringToUnion<Rest>
        : never; // 👀 这里再写的时候，写成了 Str，这会导致结果的联合类型中多一个 `""``
```

#### 数组长度做计数

<font color=FF0000>TypeScript 类型系统没有加减乘除运算符</font>，但是<font color=FF0000>可以构造不同的数组再取 length 来得到相应的结果</font>。这样就把数值运算转为了数组类型的构造和提取。

比如实现减法：

```ts
type BuildArray<
    Length extends number, 
    Ele = unknown, 
    Arr extends unknown[] = []
> = Arr['length'] extends Length // 注：再次写的时候，没想到 Arr['length']
        ? Arr 
        : BuildArray<Length, Ele, [...Arr, Ele]>;

type Subtract<Num1 extends number, Num2 extends number> = 
    // 注：再次写的时候，没想到这种匹配方法，卡住了；点破了之后，其他没什么
    BuildArray<Num1> extends [...arr1: BuildArray<Num2>, ...arr2: infer Rest] 
        ? Rest['length']
        : never;
```

#### 联合分散可简化

TypeScript 对联合类型做了特殊处理，当遇到字符串类型或者作为类型参数出现在条件类型左边的时候，会分散成单个的类型传入做计算，最后把计算结果合并为联合类型。这样虽然简化了类型编程，但也带来了一些认知负担。

比如联合类型的判断是这样的：

```ts
type IsUnion<A, B = A> =
    A extends A
        ? [B] extends [A]
            ? false
            : true
        : never
```

联合类型做为类型参数直接出现在条件类型左边的时候就会触发 “分布式条件类型”特性，而不是直接出现在左边的时候不会。

所以， A 是单个类型、B 是整个联合类型。通过比较 A 和 B 来判断联合类型。

#### 特殊特性要记清

会了提取、构造、递归、数组长度计数、联合类型分散这 5 个套路以后，各种类型体操都能写，但是有一些特殊类型的判断需要根据它的特性来，所以要重点记一下这些特性。

比如 any 和任何类型的交叉都为 any，可以用来判断 any 类型：

```ts
type IsAny<T> = 'foo' extends ('bar' & T) ? true : false
```

比如索引一般是 string，而可索引签名不是，可以根据这个来过滤掉可索引签名：

```typescript
type RemoveIndexSignature<Obj extends Record<string, any>> = {
  [
      Key in keyof Obj 
          as Key extends `${infer Str}`? Str : never
  ]: Obj[Key]
}
```

#### 基础扎实套路熟，类型体操可通关

基础指的是 TypeScript 类型系统中的各种类型，以及可以对它们做的各种类型运算逻辑，这是类型编程的原材料。

但是只是会了基础不懂一些套路也很难做好类型编程，所以要熟悉上面 6 种套路。

基础扎实、套路也熟了之后，各种类型编程问题都可以搞定，也就是“通关”。

#### 练习 ParseQueryString

##### 实现效果

<img src="https://s2.loli.net/2022/05/05/6gOqkDzpQuoMEXt.png" alt="image-20220505170419705" style="zoom:50%;" />

##### 思路

`a=1&b=2&c=3&d=4` ，这样的字符串明显是 query param 个数不确定的，遇到数量不确定的问题，条件反射的就要想到递归。

递归解析出每一个 query params，也就是 & 分隔的每个字符串，每个字符串单独去解析，构造成索引类型，最后把这些所有的单个索引类型合并就行。也就是这样的：

<img src="https://s2.loli.net/2022/05/05/qiksEwhyre4P9WM.png" alt="img" style="zoom:75%;" />

第一步并不知道有多少个 a=1、b=2 这种 query param，要递归的做模式匹配来提取。然后每一个 query param 再通过模式匹配取出 key 和 value，构造成索引类型。再把每个索引类型合并成一个大的索引类型就可以了。

##### 递归的提取 & 分隔的 query param

<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1a124009479848e28d6befeb2be79a2b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.image?" alt="img" style="zoom:92%;" />

```ts
type ParseQueryString<Str extends string> = 
    Str extends `${infer Param}&${infer Rest}`
        ? MergeParams<ParseParam<Param>, ParseQueryString<Rest>>
        : ParseParam<Str>;
```

类型参数 Str 为待处理的 query 字符串，通过 extends 约束为 string 类型。

提取 “被 & 分隔” 的字符串到 infer 声明的局部变量 Param 里，后面的字符串放到 Rest 里。

通过 ParseParam 来处理单个的 query param（**注：**也就是上面分隔的变量 Param），剩下 query 字符串也是一样的递归处理，然后把这些处理结果合并到一起，也就是 MergeParams。当提取不出 & 分割的字符串时递归结束，把剩下的字符串也用 ParseParam 来处理。

##### ParseParam 的实现就是提取和构造

![img](https://s2.loli.net/2022/05/05/lCWYBpJANcEjzyF.png)

```ts
type ParseParam<Param extends string> = 
    Param extends `${infer Key}=${infer Value}`
        ? {
            [K in Key]: Value 
        } : {};
```

类型参数 Param 类单个的 query param，比如 a=1 这种。通过模式匹配提取 key 和 value 到 infer 声明的局部变量 Key、Value 里。

通过映射类型语法构造成索引类型返回：

<img src="https://s2.loli.net/2022/05/05/4CgALoEzbQ3DGqj.png" alt="img" style="zoom:65%;" />

##### 每个 query param 处理完了，最后把这一系列构造出的索引类型合并成一个就行了

> 👀 这一步实现的时候卡住了，上面的逻辑处理，实现没什么问题。另外，这里还是有点没看懂 TODO

<img src="https://s2.loli.net/2022/05/05/anyLw6pI3SiNXWU.png" alt="img" style="zoom:75%;" />

这也是构造索引类型：

```typescript
type MergeParams<
    OneParam extends Record<string, any>,
    OtherParam extends Record<string, any>
> = {
  [Key in keyof OneParam | keyof OtherParam]: // 注：这里 OneParam 和 OtherParam 的 key 做和联合
    Key extends keyof OneParam // 注：如果 Key 在 OneParam 中存在？
        ? Key extends keyof OtherParam // 注：Key 也在 OtherParam 中存在？
            ? MergeValues<OneParam[Key], OtherParam[Key]> // 注：同时存在，则合并
            : OneParam[Key]
        : Key extends keyof OtherParam 
            ? OtherParam[Key] 
            : never
}
```

类型参数 OneParam、OtherParam 是要合并的 query param，约束为索引类型（索引为 string，索引值为任意类型）。

构造一个新的索引类型返回，索引来自两个的合并，也就是 `Key in keyof OneParam | keyof OtherParam` 。

**值也要做合并：**

如果两个索引类型中都有，那就合并成一个；也就是 `MergeValues<OneParam[Key], OtherParam[Key]>` 。否则，如果是 OneParam 中的，就取 `OneParam[Key]` ，如果是 OtherParam 中的，就取 `OtherParam[Key]` 。

<font color=FF0000>MegeValues 的合并逻辑就是 **如果两个值是同一个就返回一个，否则构造一个数组类型来合并**</font>：

```ts
type MergeValues<One, Other> = 
    One extends Other 
        ? One
        : Other extends unknown[]
            ? [One, ...Other]
            : [One, Other];
```

类型参数 One、Other 是要合并的两个值。

如果两者是同一个类型，也就是 One extends Other，就返回任意一个。否则，如果是数组就做数组合并，否则构造一个数组把两个类型放进去。



### TS 内置的高级类型

学完了 6 个类型体操的套路之后，各种类型编程逻辑我们都能写出来；但其实一些常见的类型不用自己写， TypeScript 内置了很多（👀 官方文档对应位置 [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html) ）：

#### Parameters

Parameters 用于<font color=FF0000>**提取函数类型的参数类型**</font>（ 👀 类似于 Object.keys() ，不过返回的数组中的元素有数据类型 ）。源码是这样的：

```typescript
type Parameters<T extends (...args: any) => any> 
    = T extends (...args: infer P) => any 
        ? P 
        : never;
```

类型参数 T 为待处理的类型，通过 extends 约束为函数，参数和返回值任意。

通过 extends 匹配一个模式类型，提取参数的类型到 infer 声明的局部变量 P 中返回。这样就实现了函数参数类型的提取：

<img src="https://s2.loli.net/2022/05/05/bPwJ7zdRAUs5qZa.png" alt="image-20220505005619407" style="zoom:50%;" />

这就是个简单的模式匹配，学完套路一轻轻松松就写出来了。

#### ReturnType

ReturnType 用于<font color=FF0000>提取函数类型的返回值类型</font>。源码是这样的：

```ts
type ReturnType<T extends (...args: any) => any> 
    = T extends (...args: any) => infer R 
        ? R 
        : any;
```

类型参数 T 为待处理的类型，通过 extends 约束为函数类型，参数和返回值任意。

用 T 匹配一个模式类型，提取返回值的类型到 infer 声明的局部变量 R 里返回。这样就实现了函数返回值类型的提取：

<img src="https://s2.loli.net/2022/05/05/VSuQArdvk7B49oP.png" alt="image-20220505010230432" style="zoom:50%;" />

> 👀这个自己写出来了，不复杂

#### ConstructorParameters

构造器类型和函数类型的区别就是可以被 new 。

Parameters 用于提取函数参数的类型，而 ConstructorParameters 用于<font color=FF0000>**提取构造器参数的类型**</font>。源码是这样的：

```ts
type ConstructorParameters<
    T extends abstract new (...args: any) => any
> = T extends abstract new (...args: infer P) => any 
    ? P 
    : never;
```

类型参数 T 是待处理的类型，通过 extends 约束为构造器类型；<font color=FF0000>加个 abstract 代表不能直接被实例化（其实不加也行）</font>。

用 T 匹配一个模式类型，提取参数的部分到 infer 声明的局部变量 P 里，返回 P。这样就实现了构造器参数类型的提取：

<img src="https://s2.loli.net/2022/05/05/AG7DMTZ45V3K8aY.png" alt="image-20220505011243327" style="zoom:50%;" />

> 👀 这个也不复杂，自己实现的没有加上 abstract

#### InstanceType

提取了构造器参数的类型，自然也可以<font color=FF0000>提取构造器返回值的类型，就是 InstanceType</font> 。源码是这样的：

```ts
type InstanceType<
    T extends abstract new (...args: any) => any
> = T extends abstract new (...args: any) => infer R 
    ? R 
    : any;
```

> 👀 讲解略

#### ThisParameterType

函数里可以调用 this，这个 this 的类型也可以约束：

<img src="https://s2.loli.net/2022/05/05/Oqld5XUeWKjZL2H.png" alt="img" style="zoom:70%;" />

同样：<font color=FF0000>this 的类型也可以提取出来，通过 ThisParameterType 这个内置的高级类型</font>：

<img src="https://s2.loli.net/2022/05/05/Bo4q1DWOhxagNzU.png" alt="img" style="zoom:75%;" />

它的源码是这样的：

```ts
type ThisParameterType<T> = 
    T extends (this: infer U, ...args: any[]) => any 
        ? U 
        : unknown;
```

类型参数 T 为待处理的类型。

用 T 匹配一个模式类型，提取 this 的类型到 infer 声明的局部变量 U 里返回。这样就实现了 this 类型的提取。

> 👀 这个借鉴了 [[#模式匹配做提取#函数#GetThisParameterType]] 写出来了，原因时 `infer U` 没有 加上 `this:` 。另外，在调用时，添加 typeof ，如： `ThisParameterType<typeof Fn>` 。关于这里 typeof 的使用，可以参考如下代码：
>
> ```ts
> const foo = () => 'foo';
> 
> type Foo = typeof foo; // () => string
> 
> type FooReturn = ReturnType<typeof foo>; // string
> ```
>
> `ReturnType<typeof foo>` 返回 string ，<font color=red>说明 `typeof foo` 的值 是一个函数</font>。
>
> 这里就说到了 TS 中的 typeof 和 ES 中的 typeof 是有区别的：除了 ES 中的的功能外，TS 的 typeof 还可以在类型编程中”提取类型“，无论是上面的 `typeof foo` 获得 `() => string` 类型，还是 `ReturnType<typeof foo>` 获取到返回值类型 string。另外，因为“类型编程”是在“编译期”完成，所以 TS typeof 的 ”提取类型“ 都是在“编译期”完成的。而 ES 的 typeof 是在“运行时”完成。
>
> 💡 关于 TS 的 typeof 可以参考： [TypeScript基础之typeof 类型操作符](https://segmentfault.com/a/1190000042036809) ；另外 TS handbook 中也说的很详细，对于重要部分，在下面做了笔记，见：[[#官方文档阅读#Typeof Type Operator]]
#### OmitThisParameter

提取出 this 的类型之后，自然可以构造一个新的；比如<font color=FF0000>删除 this 的类型，可以用 OmitThisParameter </font>

> 👀 如下截图，`say(this: Person, age: number` 被 OmitThisParameter 处理后， this 参数被去掉

它的源码是这样的：

```ts
type OmitThisParameter<T> = 
    unknown extends ThisParameterType<T> // 注：如果 this 不存在，直接返回函数，不做任何处理
        ? T 
        : T extends (...args: infer A) => infer R 
            ? (...args: A) => R 
            : T;
```

类型参数 T 为待处理的类型。

用 ThisParameterType 提取 T 的 this 类型，如果提取出来的类型是 unknown 或者 any，那么 `unknown extends ThisParameterType ` 就成立，也就是没有指定 this 的类型，所以直接返回 T 。否则，就通过模式匹配提取参数和返回值的类型到 infer 声明的局部变量 A 和 R 中，用它们构造新的函数类型返回

> 👀 没搞懂，this 的参数去哪了？TODO

这样，就实现了去掉 this 类型的目的：

<img src="https://s2.loli.net/2022/05/05/pP8G3A916fjiFah.png" alt="image-20220505014934563" style="zoom:50%;" />

#### Partial

索引类型可以通过映射类型的语法做修改，比如<font color=FF0000>把索引变为可选</font>。

```ts
type Partial<T> = {
    [P in keyof T]?: T[P];
};
```

讲解略。

#### Required

可以把索引变为可选，也同样可以<font color=FF0000>去掉可选</font>，也就是 Required 类型：

```ts
type Required<T> = {
    [P in keyof T]-?: T[P];
};
```

> 👀 写出来了，不过这个 `-` 教程中还没讲过，是在其他地方了解的。

类型参数 T 为待处理的类型。

通过映射类型的语法构造一个新的索引类型，索引取自之前的索引，也就是 `P in keyof T` ，但是要去掉可选，也就是 `-?` ，值的类型也是之前的，就是 `T[P]` 。这样就实现了去掉可选修饰的目的

#### Readonly

同样的方式，也可以添加 readonly 的修饰（👀 Readonly 的 `o` 不大写）：

```ts
type Readonly<T> = {
    readonly [P in keyof T] : T[P];
};
```

讲解略。

#### Pick

映射类型的语法<font color=FF0000>用于构造新的索引类型</font>，在构造的过程中可以对索引和值做一些修改或过滤（**注：**如下面的图，Pick 的第二个类型参数可以是一个「联合类型」 ）。比如可以用 Pick 实现过滤：

```typescript
type Pick<T, K extends keyof T> = { // 👀 注意这里的约束，extends keyof T
    [P in K]: T[P]; // 👀 这里 [ P in K ] 同样要注意，没见过
};
```

类型参数 T 为待处理的类型，类型参数 K 为要过滤出的索引，通过 extends 约束为只能是 T 的索引的子集。

<font color=FF0000 size=4>构造新的索引类型返回，索引取自 K，也就是 `P in K`</font> ；值则是它对应的原来的值，也就是 `T[P]` 。这样就实现了过滤的目的：

<img src="https://s2.loli.net/2022/05/05/vCXZPjyl8UiEGIs.png" alt="image-20220505114610442" style="zoom:50%;" />

#### Record

Record 用于创建索引类型，传入 key 和值的类型：

```typescript
type Record<K extends keyof any, T> = {
    [P in K] : T; // 👀 如下示例的截图，参数 K 可能是个 “联合类型”，所以，这里用了 [P in K]
};
```

这里很巧妙的用到了 `keyof any` ，它的结果是 `string | number | symbol`：

<img src="https://s2.loli.net/2022/05/05/SuHeYaRypEUBr62.png" alt="image-20220505115217239" style="zoom:50%;" />

不过，<font color=fuchsia>**如果开启了 keyOfStringsOnly 的编译选项，它就只是 stirng 了**</font>：

```json
// tsconfig.json
"compilerOptions": {
  // ...
  "keyofStringsOnly": true
}
```

<img src="https://s2.loli.net/2022/05/05/eMpx3nf79SdEkoa.png" alt="image-20220505115451984" style="zoom:50%;" />

<font color=FF0000>**用 `keyof any`是动态获取的，比直接写死 `string | number | symbol` 更好**</font>。

继续讲 Record 这个类型，它用映射类型的语法创建了新的索引类型，索引来自 K，也就是 `P in K` ；值是传入的 T。这样就用 K 和 T 构造出了对应的索引类型。

<img src="https://s2.loli.net/2022/05/05/lZTDVJS3ovPyQqm.png" alt="image-20220505115822627" style="zoom:50%;" />

#### Exclude

当想从一个联合类型中去掉一部分类型时，可以用 Exclude 类型。实现如下：

```ts
type Exclude<T, U> = T extends U ? never : T;
```

<font color=FF0000>联合类型当作为类型参数出现在条件类型左边时，会被分散成单个类型传入</font>，这叫做 <font color=FF0000>**分布式条件类型**</font>。所以写法上可以简化， `T extends U` 就是对每个类型的判断。过滤掉 U 类型，剩下的类型组成联合类型。也就是取差集。

<img src="https://s2.loli.net/2022/05/05/RhjwJsCWodtrq6z.png" alt="image-20220505120911598" style="zoom:50%;" />

#### Extract

可以过滤 ( Exclude ) 掉，自然也可以保留；<font color=FF0000>Exclude 反过来就是 Extract，也就是**取交集**</font>。代码实现和 Excludes 几乎一模一样：

```ts
type Extract<T, U> = T extends U ? T : never;
```

<img src="https://s2.loli.net/2022/05/05/B2vKzlXytE3Yfhb.png" alt="image-20220505121201252" style="zoom:50%;" />

#### Omit

我们知道了 Pick 可以取出索引类型的一部分索引构造成新的索引类型，那<font color=FF0000>反过来就是去掉这部分索引构造成新的索引类型</font>。可以结合 Exclude 来轻松实现：

```ts
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

类型参数 T 为待处理的类型，类型参数 K 为索引允许的类型（ `string | number | symbol` 或者 string ）。

<font color=FF0000>通过 Pick 取出一部分索引构造成新的索引类型，这里 **用 Exclude 把 K 对应的索引去掉**，把剩下的索引保留</font>。这样就实现了删除一部分索引的目的：

<img src="https://s2.loli.net/2022/05/05/WgpeuAk7KtaL1DS.png" alt="image-20220505130809114" style="zoom:50%;" />

Omit 的第二个类型参数可以是联合类型，如下示例：

<img src="https://s2.loli.net/2022/05/05/FNSiXRyBbxosKMq.png" alt="image-20220505130731750" style="zoom:50%;" />

#### Awaited

在递归那节我们写过<font color=FF0000>提取 Promise 的 ValuType 的高级类型，这个比较常用，TS 也给内置了，就是 Awaited </font>。它的实现比我们当时写的（ [[#DeepPromiseValueType]] ）完善一些：

```ts
type Awaited<T> =
    T extends null | undefined
        ? T 
        : T extends object & { then(onfulfilled: infer F): any }
            ? F extends ((value: infer V, ...args: any) => any)
                ? Awaited<V>
                : never 
            : T;
```

类型参数 T 是待处理的类型。

如果 T 是 null 或者 undefined，就返回 T。如果 <font color=FF0000>T 是对象 **并且** 有 then 方法，那就提取 then 的参数，也就是 onfulfilled 函数的类型到 infer 声明的局部变量 F</font> 。继续提取 onfullfilled 函数类型的第一个参数的类型，也就是 Promise 返回的值的类型到 infer 声明的局部变量 V。递归的处理提取出来的 V，直到不再满足上面的条件。

这样就实现了取出嵌套 Promise 的值的类型的目的：

<img src="https://s2.loli.net/2022/05/05/BPkdynYVHX6WuzF.png" alt="image-20220505125830350" style="zoom:50%;" />

为什么要提取 then 方法的第一个参数的返回值类型看下 Promise 的结构就明白了：

```typescript
new Promise(() => {
    // xxx
}).then((value) => { ... });
```

<font color=FF0000>then 第一个参数是 onfullfilled 的回调，从它的第一个参数就能拿到返回的值的类型</font>。

对比下我们之前的实现：

```typescript
type DeepPromiseValueType2<T> = 
    T extends Promise<infer ValueType> 
        ? DeepPromiseValueType2<ValueType>
        : T;
```

<font color=FF0000>内置的高级类型不再限制必须是 Promise，而是只要对象且有 then 方法就可以</font>；这样更通用了一些。

#### NonNullable

NonNullable 就是用于 <font color=FF0000>判断是否为非空类型</font>，也就 <font color=FF0000>是不是 null 或者 undefined 的类型的</font>。实现比较简单：

```ts
type NonNullable<T> = T extends null | undefined ? never : T;
```

<img src="https://s2.loli.net/2022/05/05/Liw5M4dAvoabKTP.png" alt="image-20220505132716463" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/05/05/oSrMkcaI5VjZhYP.png" alt="image-20220505132750515" style="zoom:50%;" />

#### Uppercase、Lowercase、Capitalize、Uncapitalize

这四个类型是分别实现 大写、小写、首字母大写、去掉首字母大写的。它们的源码时这样的：

```typescript
type Uppercase<S extends string> = intrinsic;

type Lowercase<S extends string> = intrinsic;

type Capitalize<S extends string> = intrinsic;

type Uncapitalize<S extends string> = intrinsic;
```

<font color=FF0000><font size=4>**intrinsic 是固有的意思**</font>，就像 js 里面的有的方法打印会显示 `[native code]` 一样。<font size=4>**这部分类型不是在 ts 里实现的，而是编译过程中由 js 实现的**</font></font>。

可以在源码里找到对应的处理代码：

<img src="https://s2.loli.net/2022/05/05/K7IpADbw9xr5Ovc.png" alt="img" style="zoom:95%;" />

其实就是 TS 编译器处理到这几个类型时就直接用 js 给算出来了。

**为啥要这样做呢？**<font color=FF0000>**因为快啊，解析类型是要处理 AST 的，性能比较差，用 js 直接给算出来那多快呀**</font>。

#### 总结

- 用模式匹配可以实现：Parameters、ReturnType、ConstructorParameters、InstanceType、ThisParameterType。

- 用模式匹配 + 重新构造可以实现：OmitThisParameter

- 用重新构造可以实现：Partial、Required、Readonly、Pick、Record

- 用模式匹配 + 递归可以实现： Awaited

- 用联合类型在分布式条件类型的特性可以实现： Exclude

- 还有 NonNullable 和四个编译器内部实现的类型：Uppercase、Lowercase、Capitalize、Uncapitalize。

这些类型也不咋需要记，就算忘记了自己也能很快的实现。重点还是放在 6 个类型编程的套路上。



### 类型编程的意义

类型编程是对类型参数做一系列运算之后产生新的类型。那 <font color=FF0000>**类型和类型之间有关系 的场景必然会用到类型编程**，比如：**返回值的类型** 和 **参数的类型** **有一定的关系**，需要经过计算才能得到</font>。

有的情况下不用类型编程也行，比如：返回值可以是一个字符串类型 string ，但<font color=FF0000>用了类型编程的话，可能 **能更精确的提示出是什么 string**，也就是具体的字符串字面量类型，那 **类型提示的精准度自然就提高了一个级别**，体验也会更好</font>。

<font color=dodgerBlue>这就是类型编程的意义</font>：**类型和类型之间有关系的场景，必然要用类型编程做一些运算。有的场景下可以不用类型编程，但是 用了能够有更精准的类型提示和检查。**

#### 以 ParseQueryString 为例

前面我们实现了一个复杂的高级类型 ParseQueryString [[#类型体操顺口溜#练习 ParseQueryString]] ，用到了提取、构造、递归的套路。<font color=dodgerBlue>这么复杂的高级类型能用在哪里呢？有什么意义呢？</font>

首先，我们写一个 JS 函数，实现对 query string 的 parse，如果有同名的参数就合并，大概实现是这样的：

```javascript
function parseQueryString(queryStr) {
    if (!queryStr || !queryStr.length) {
        return {};
    }
    const queryObj = {};
    const items = queryStr.split('&');
    items.forEach(item => {
        const [key, value] = item.split('=');
        if (queryObj[key]) {
            if(Array.isArray(queryObj[key])) {
                queryObj[key].push(value);
            } else {
                queryObj[key] = [queryObj[key], value]
            }
        } else {
            queryObj[key] = value;
        }
    });
    return queryObj;
}
```

<img src="https://s2.loli.net/2022/05/06/aLFUkeBcitSTp5f.png" alt="image-20220506163224018" style="zoom:60%;" />

如果要给这个函数加上类型，大家会怎么加呢？大部分人会这么加：

```ts
function parseQueryString(queryStr: string): object { ... }
```

参数是 string 类型，返回值是 parse 之后的对象类型 object 。这样是可以的，而且 object 还可以写成 `Record<string, any>` ，因为对象是索引类型（索引类型就是聚合多个元素的类型，比如对象、class、数组都是）

```ts
function parseQueryString(queryStr: string): Record<string, any> { ... }
```

但是不管是返回值类型为 object 还是 Record<string, any> 都存在一个问题：返回的对象不能提示出有哪些属性。

<img src="https://s2.loli.net/2022/05/06/yMUBelARPWCx53i.png" alt="image-20220506163712131" style="zoom:50%;" />

对于习惯了 ts 的提示的同学来说，没有提示太不爽了。怎么能让这个函数的返回的类型有提示呢？

这就要用到类型编程了，我们把函数的类型定义改成这样：

```ts
function parseQueryString<Str extends string>(queryStr: Str): ParseQueryString<Str> { ... }
```

声明一个类型参数 Str，约束为 string 类型，函数参数的类型指定是这个 Str ；<font color=FF0000>返回值的类型通过对 Str 做类型运算得到，也就是 `ParseQueryString<Str>`</font> 。<font color=FF0000>**这个 ParseQueryString 的类型做的事情就是把传入的 Str 通过各种类型运算产生对应的索引类型**</font>。

<img src="https://s2.loli.net/2022/05/06/cB4iLMJUWXC68Gm.png" alt="image-20220506164214600" style="zoom:45%;" />

另外，这里的实现和之前那个还是有一些区别的，主要是这里：

<img src="https://s2.loli.net/2022/05/06/ktVd15QUpyxhjPI.png" alt="image-20220506175024745" style="zoom:60%;" />

当提取 a=1 中的 key 和 value，构造成索引类型的时候，如果提取不出来，之前返回的是空对象；现在改成了 `Record<string, any>` 。因为 ParseQueryString 是针对字符串字面量类型做运算的，<font color=FF0000>如果传入的不是字面量类型，而是 string ；那就会走到这里，如果返回空对象，那取它的任何属性都会报错</font>。

<img src="https://s2.loli.net/2022/05/06/uPvKShC8jgO32za.png" alt="image-20220506174636722" style="zoom:55%;" />

所以要把不满足条件时返回的类型改为 `Record<string, any>` ：

<img src="https://s2.loli.net/2022/05/06/eN6JhpZz8WYKMyl.png" alt="image-20220506174743809" style="zoom:55%;" />

有同学可能会发现我们<font color=FF0000>**用 `as any` 来对返回值类型做了断言**</font> （见上面截图中的代码），这是因为 `ParseQueryString<Str>` 要传入类型参数 Str 才能知道具体的类型，而具体传入什么在类型检查时是不知道的，所以这里要 `as any` 才能通过类型检查。

#### Promise.all

前面提到过：当类型和类型之间有关系的时候，必然会用到类型编程。我们来看个例子。

Promise 的 all 和 race 方法的类型声明是这样的：

```typescript
interface PromiseConstructor {
    all<T extends readonly unknown[] | []>
        (values: T): Promise<{
            -readonly [P in keyof T]: Awaited<T[P]>
        }>;

    race<T extends readonly unknown[] | []>
        (values: T): Promise<Awaited<T[number]>>;
}
```

因为 Promise.all 是等所有 promise 执行完一起返回，Promise.race 是有一个执行完就返回；返回的类型都需要用到参数 Promise 的 value 类型：

<img src="https://s2.loli.net/2022/05/06/a6e75wEogrDkxV4.png" alt="image-20220506181202383" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/05/06/g4Ir9OklXmq1DFM.png" alt="image-20220506181247283" style="zoom:50%;" />

所以自然要用类型编程来提取出 Promise 的 value 的类型，构造成新的 Promise 类型。

##### 具体来看下这两个类型定义

```ts
interface PromiseConstructor {
    // all 方法的签名
    all<T extends readonly unknown[] | []>(values: T): Promise<{ -readonly [P in keyof T]: Awaited<T[P]> }>;
}
```

类型参数 T 是待处理的 Promise 数组，约束为 `unknown[]` 或者空数组 [] 。这个类型参数 T 就是传入的函数参数的类型。

<font color=FF0000>返回一个新的数组类型</font>，也可以用映射类型的语法构造个新的索引类型（ class、对象、数组等聚合多个元素的类型都是索引类型）。

新的索引类型的索引来自之前的数组 T ，也就是 `P in keyof T` ，值的类型是之前的值的类型，但要做下 Promise 的 value 类型提取，用内置的高级类型 Awaited ，也就是 `Awaited<T[P]>` 。同时要把 readonly 的修饰去掉，也就是 `-readonly` 。

这就是 Promise.all 的类型定义。因为返回值的类型和参数的类型是有关联的，所以必然会用到类型编程。

```ts
interface PromiseConstructor {
    // race 方法的签名
    race<T extends readonly unknown[] | []>(values: T): Promise<Awaited<T[number]>>;
}
```

类型参数 T 是待处理的参数的类型，约束为 `unknown[]` 或者空数组 []。

返回值的类型可能是传入的任何一个 Promise 的 value 类型，那就先取出所有的 Promise 的 value 类型，也就是 `T[number]` 。因为数组类型也是索引类型，所以可以用索引类型的各种语法：对数组取索引，会返回数组元素的联合；如下图

<img src="https://s2.loli.net/2022/05/06/WCaUTsPeokZpv9D.png" alt="image-20220506182357184" style="zoom:50%;" />

用 Awaited 取出这个联合类型中的每一个类型的 value 类型，也就是 `Awaited<T[number]>` ，这就是 race 方法的返回值的类型。

同样，<font color=FF0000>因为 返回值的类型 是由参数的类型做一些类型运算得到的，也离不开类型编程</font>。

#### currying

做了一个参数类型和返回值类型有关系的案例，再来看一个更复杂点的：

有这样一个 curring 函数，接受一个函数，返回柯里化后的函数。也就是当传入的函数为：

```ts
const func = (a: string, b: number, c: boolean) => {};
```

返回的函数应该为：

```javascript
(a: string) => (b: number) => (c: boolean) => void
```

JS 怎么实现不用关注，我们只关注这个 curring 函数的类型怎么定义：

```ts
declare function currying(fn: xxx): xxx;
```

明显，这里返回值类型和参数类型是有关系的，所以要用类型编程。

传入的是函数类型，可以用模式匹配提取参数和返回值的类型来，构造成新的函数类型返回。

每有一个参数就返回一层函数，具体层数是不确定的，所以要用递归。那么，这个类型的定义就是这样的：

```ts
type CurriedFunc<Params, Return> = 
    Params extends [infer Arg, ...infer Rest]
        ? (arg: Arg) => CurriedFunc<Rest, Return>
        : never;

declare function currying<Func>(fn: Func): 
    Func extends (...args: infer Params) => infer Result
         ? CurriedFunc<Params, Result>
         : never;
```

curring 函数有一个类型参数 Func ，由函数参数的类型指定。

返回值的类型要对 Func 做一些类型运算，通过模式匹配提取参数和返回值的类型，传入 CurriedFunc 来构造新的函数类型。

构造的函数的层数不确定，所以要用递归，每次提取一个参数到 infer 声明的局部变量 Arg，其余参数到 infer 声明的局部变量 Rest。用 Arg 作为构造的新的函数函数的参数，返回值的类型继续递归构造。

这样就递归提取出了 Params 中的所有的元素，递归构造出了柯里化后的函数类型。

<img src="https://s2.loli.net/2022/05/06/TMcEk4YI6wV2um7.png" alt="image-20220506184418755" style="zoom:50%;" />





### 类型编程综合练习

#### KebabCaseToCamelCase

常用的变量命名规范有两种，一种是 KebabCase，也就是 aaa-bbb-ccc 这种中划线分割的风格；另一种是 CamelCase， 也就是 aaaBbbCcc 这种除第一个单词外首字母大写的风格。

如果想实现 KebabCase 到 CamelCase 的转换，该怎么做呢？比如从 guang-and-dong 转换成 guangAndDong。

```ts
type KebabCaseToCamelCase<Str extends string> = 
    Str extends `${infer Item}-${infer Rest}` // 注：这里判断的设计，有点没想到
        ? `${Item}${KebabCaseToCamelCase<Capitalize<Rest>>}`
        : Str;
```

类型参数 Str 是待处理的字符串类型，约束为 string。

通过模式匹配提取 Str 中 ` -` 分隔的两部分，前面的部分放到 infer 声明的局部变量 Item 里，后面的放到 infer 声明的局部变量 Rest 里。提取的第一个单词不大写，后面的字符串首字母大写，然后递归的这样处理，然后也就是 \`\${Item}${KebabCaseToCamelCase\<Capitalize>\` ；如果模式匹配不满足，就返回 Str。这样就完成了 KebabCase 到 CamelCase 的转换。

#### CamelCaseToKebabCase

同样是对字符串字面量类型的提取和构造，也需要递归处理，但是 CamelCase 没有 `-` 这种分割符，那怎么分割呢？可以判断字母的大小写，用大写字母分割。

```ts
type CamelCaseToKebabCase<Str extends string> = 
    Str extends `${infer First}${infer Rest}`
        ? First extends Lowercase<First> 
            ? `${First}${CamelCaseToKebabCase<Rest>}`
            : `-${Lowercase<First>}${CamelCaseToKebabCase<Rest>}`
        : Str;
```

类型参数 Str 为待处理的字符串类型。

通过模式匹配提取首个字符到 infer 声明的局部变量 First，剩下的放到 Rest。判断下当前字符是否是小写，如果是的话就不需要转换，递归处理后续字符，也就是 \`\${First}\${CamelCaseToKebabCase}\` 。如果是大写，那就找到了要分割的地方，转为 ` -` 分割的形式，然后把 First 小写，后面的字符串递归的处理，也就是 \`-\${Lowercase}\${CamelCaseToKebabCase}\` 。如果模式匹配不满足，就返回 Str。这样就完成了 CamelCase 到 KebabCase 的转换。

#### Chunk

> **注：**这题有点棒，反正一点思路都没有...

希望实现这样一个类型：对数组做分组，比如 [1, 2, 3, 4, 5] 的数组，每两个为 1 组，那就可以分为 [1, 2] 和 [3, 4] 以及 [5] 这三个 Chunk 

这明显是对数组类型的提取和构造，元素数量不确定，需要递归的处理；并且还需要通过构造出的数组的 length 来作为 chunk 拆分的标志。

```ts
type Chunk<
    Arr extends unknown[], 
    ItemLen extends number, 
    CurItem extends unknown[] = [], 
    Res extends unknown[] = []
> = Arr extends [infer First, ...infer Rest] 
        ? CurItem['length'] extends ItemLen // 注：Arr['length'] 这个属性又忘记了
          ? Chunk<Rest, ItemLen, [First], [...Res, CurItem]>
          : Chunk<Rest, ItemLen, [...CurItem, First], Res> 
        : [...Res, CurItem]
```

类型参数 Arr 为待处理的数组类型，约束为 unknown。类型参数 ItemLen 是每个分组的长度。后两个类型参数是用于保存中间结果的：类型参数 CurItem 是当前的分组，默认值 [] ，类型参数 Res 是结果数组，默认值 [] 。

通过模式匹配提取 Arr 中的首个元素到 infer 声明的局部变量 First 里，剩下的放到 Rest 里。

<font color=FF0000>**通过 `CurItem['length']` 判断是否到了每个分组要求的长度 ItemLen**</font> ：如果到了，就把 CurItem 加到当前结果 Res 里，也就是 `[...Res, CurItem]` ，然后开启一个新分组，也就是 `[First]` 。如果没到，那就继续构造当前分组，也就是 `[...CurItem, First]` ，当前结果不变，也就是 Res 。这样递归的处理，直到不满足模式匹配，那就把当前 CurItem 也放到结果里返回，也就是 `[...Res, CurItem]` 。

这样就完成了根据长度对数组分组的功能。

#### TupleToNestedObject

>  👀 这题有卡住的地方，解决了之后，虽然结果看起来没什么问题；不过，对于异常的数据（数组中存在非字符类型）依然有问题

我们希望实现这样一个功能：根据数组类型，比如 `['a', 'b', 'c']` 的元组类型，再加上值的类型 'xxx'，构造出这样的索引类型：

```json
{
    a: {
        b: {
            c: 'xxx'
        }
    }
}
```

这个依然是提取、构造、递归，只不过是对数组类型做提取，构造索引类型，然后递归的这样一层层处理。

```ts
type TupleToNestedObject<Tuple extends unknown[], Value> = 
    Tuple extends [infer First, ...infer Rest]
      ? {
          [Key in First as Key extends keyof any ? Key : never]: 
              Rest extends unknown[]
                  ? TupleToNestedObject<Rest, Value>
                  : Value
      }
      : Value;
```

类型参数 Tuple 为待处理的元组类型，元素类型任意，约束为 `unknown[]` 。类型参数 Value 为值的类型。

通过模式匹配提取首个元素到 infer 声明的局部变量 First，剩下的放到 infer 声明的局部变量 Rest。用提取出来的 First 作为 Key 构造新的索引类型，也就是 `Key in First` ，值的类型为 Value，如果 Rest 还有元素的话就递归的构造下一层。

为什么后面还有个 `as Key extends keyof any ? Key : never` 的重映射呢？因为比如 <font color=FF0000>**null、undefined 等类型是不能作为索引类型的 key 的，就需要做下过滤**；如果是这些类型，就返回 never，否则返回当前 Key </font>。

这里的 `keyof any` 在内置的高级类型那节也有讲到，就是取当前支持索引支持 `string | number | symbol` 类型的。

如果提取不出元素，那就构造结束了，返回 Value。

#### PartialObjectPropByKeys

我们想实现这样一个功能：把一个索引类型的某些 Key 转为 可选的，其余的 Key 不变。比如

```typescript
interface Dong {
    name: string
    age: number
    address: string
}
```

把 name 和 age 变为可选之后就是这样的：

```ts
interface Dong2 {
    name?: string
    age?: number
    address: string 
}
```

这样的类型逻辑<font color=FF0000>**很容易想到是用映射类型的语法构造一个新的类型**</font>，但是<font color=FF0000>这里要求只用内置的高级类型来实现</font>。

```ts
type PartialObjectPropByKeys<
    Obj extends Record<string, any>,
    Key extends keyof any
> = Partial<
      Pick<Obj, Extract<keyof Obj, Key>>
    > & Omit<Obj, Key>;
// 先将Obj中符合“联合类型”Key的键以“联合类型”形式提取(Extract)出来，并挑选(Pick)Extract的结果，组成一个缩阴类型，对其做可选操作(Partial)，最后和“不在‘联合类型’Key组成的索引类型”合并（用 & 合并）。
```

交叉类型 ( `&` ) 会把同类型做合并，不同类型舍弃（👀 这里发现对交叉类型有点遗忘， [[#交叉：&]] 做了笔记）。

不过，上面的代码运行结果有点问题：

<img src="https://s2.loli.net/2022/05/07/t1npLX8acObEAfZ.png" alt="image-20220507004823236" style="zoom:50%;" />

为啥这里没计算出最终的类型呢？

因为 <font color=FF0000 size=4>**TS 只有在需要计算的时候才会去计算**</font>，这里并不会去做计算。<font color=FF0000>可以再做一层映射，当构造新的索引类型的时候，就会做计算了</font>

```ts
type Copy<Obj extends Record<string, any>> = {
    [Key in keyof Obj]:Obj[Key]
}

type PartialObjectPropByKeys<
    Obj extends Record<string, any>,
    Key extends keyof any = keyof Obj
> = Copy<Partial<Pick<Obj,Extract<keyof Obj, Key>>> & Omit<Obj,Key>>;
```

这里的 Copy 就是通过映射类型的语法构造新的索引类型，key 和 value 都不变（**注：**Copy 没有其他作用，只是为了让上面的结果运行）。这样就会计算出最终的索引类型：

<img src="https://s2.loli.net/2022/05/07/NVSef923auCWnMd.png" alt="image-20220507005050965" style="zoom:50%;" />



#### 函数重载的三种写法

TS 支持函数重载，也就是同名的函数可以有多种类型定义。

##### 重载的写法一共有三种

```ts
declare function func(name: string): string;
declare function func(name: number): number;
```

这种大家比较常用，声明两个同名函数，就能达到重载的目的：

<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/baa435154188455fb18f045fcc7fb5bd~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" alt="img" style="zoom:70%;" />

函数可以用 interface 的方式声明，同样，也可以用 interface 的方式声明函数重载：

```ts
interface Func {
  (name: string): string;
  (name: number): number;
}
```

<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6fc6af466fda4757894caa1ae9a85716~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" alt="img" style="zoom:70%;" />

<font color=fuchsia>**函数类型** 可以取交叉类型 ( `&` )，也就是多种类型都可以，其实也是函数重载的意思</font>：

```ts
type Func = (
  ((name: string) => string) & ((name: number) => number);
)
```

<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db167ed7cbee4bc8a9facc31bae34d08~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" alt="img" style="zoom:70%;" />

#### UnionToTuple

要求把 “联合类型” 转成 “元组类型”，也就是 `'a' | 'b' | 'c'` 转成 `['a', 'b', 'c']` 。

没思路很正常，因为这里用到了一些特殊的特性。我们先来过一下用到的特性：

我们知道 <font color=lightSeaGreen>ReturnType 是 TS 内置的一个高级类型，它可以取到函数返回值的类型</font>。但<font color=FF0000>如果这个函数有多个重载呢？</font>

##### 第一种重载方式

```ts
declare function func(name: string): string;
declare function func(name: number): number;

type res = ReturnType<typeof func>;
```

<img src="https://s2.loli.net/2022/05/07/Xls1mI2xBk79LWP.png" alt="image-20220507015726837" style="zoom:50%;" />

##### 第二种重载方式

```ts
interface Func {
  (name: string): string;
  (name: number): number;
}

type res = ReturnType<Func>
```

<img src="https://s2.loli.net/2022/05/07/Yb4fwLx6IP8UNOd.png" alt="image-20220507120738599" style="zoom:50%;" />

##### 第三种重载方式

<img src="https://s2.loli.net/2022/05/07/pGBrdYSMl6WNQEj.png" alt="image-20220507120921480" style="zoom:50%;" />

根据上面的试验可得到：**取 <font color=FF0000>重载函数的 ReturnType 返回的是最后一个重载的返回值类型</font>。**

但这与联合类型有什么关系呢？重载函数不是能通过函数 “交叉” 的方式实现么，而我们又能实现 “联合转交叉” ( UnionToIntersection ) 。所以就能拿到联合类型的最后一个类型：

```ts
type UnionToIntersection<U> = 
    (U extends U ? (x: U) => unknown : never) extends (x: infer R) => unknown
        ? R
        : never

type UnionToFuncIntersection<T> = UnionToIntersection<T extends any ? () => T : never>;
```

UnionToIntersection 的实现在 [[#特殊特性要记清#UnionToIntersection]] 里讲了。这里简单讲一下：`U extends U` 是触发 分布式条件类型，<font color=FF0000>构造一个函数类型</font>；<font color=FF0000>**通过「模式匹配」提取参数的类型，利用函数参数的「逆变」的性质**，就能实现 “联合转交叉”</font>。因为函数参数的类型要能接收多个类型，那肯定要定义成这些类型的交集，所以会发生逆变，转成交叉类型。

**然后是 UnionToFuncIntersection 的类型：**

我们对联合类型 T 做下处理，用 `T extneds any` 触发「分布式条件类型」的特性，它会把联合类型的每个类型单独传入做计算，最后把计算结果合并成联合类型。把每个类型构造成一个函数类型传入。

这样，返回的交叉类型也就达到了函数重载的目的：

#### // TODO 这里有点看不下去了，等等再看



### 逆变、协变、双向协变、不变

#### 类型安全和型变

// TODO：关于这个可以看下 [协变和逆变【渡一教育】](https://www.bilibili.com/video/BV1p4421U77L) ，做下笔记

<font color=dodgerBlue>TypeScript 给 JavaScript 添加了一套静态类型系统，是为了保证类型安全的</font>；也就是：<font color=FF0000>保证变量只能赋同类型的值，对象只能访问它有的属性、方法</font>。比如：<font color=lightSeaGreen>number 类型的值不能赋值给 boolean 类型的变量，Date 类型的对象就不能调用 exec 方法</font>。

这是类型检查做的事情，<font color=FF0000>遇到类型安全问题会在编译时报错</font>。但是<font color=FF0000>**这种类型安全的限制也不能太死板，有的时候需要一些变通**</font>，<font color=dodgerBlue>比如</font> <font color=red>**子类型是可以赋值给父类型的变量的**，可以完全当成父类型来使用，也就是 “型变 ( variant ) ”（类型改变）</font>。

<font color=dodgerBlue>**这种 “型变” 分为两种**</font>：<font color=dodgerBlue>一种是</font> <font color=red>**子类型可以赋值给父类型**，叫做 **协变** ( covariant )</font> ；<font color=dodgerBlue>一种是</font> <font color=red>**父类型可以赋值给子类型** ，叫做 **逆变** ( contravariant ) </font>。

> 型变有四种方式：协变、逆变、双变、不变
>
> 摘自：《TypeScript 编程》6.1.2 型变 P143

#### 协变 ( covariant )

其中协变是很好理解的，比如我们有两个 interface ：

```typescript
interface Person {
    name: string;
    age: number;
} 

interface Guang {
    name: string;
    age: number;
    hobbies: string[]
}
```

这里 <font color=FF0000>Guang 是 Person 的子类型</font>（<font color=FF0000>👀 `ChildType extends ParentType ? true : false` 结果为 true</font> ），更具体，那么 Guang 类型的变量就可以赋值给 Person 类型：

```ts
let person: Person = {
  name: '',
  age: 20
};

let guang: Guang = {
  name: 'guang',
  age: 20,
  hobbies: ['play game', 'writing']
};

person = guang
```

这并不会报错，虽然这俩类型不一样，但是依然是类型安全的。这种 <font color=FF0000>子类型可以赋值给父类型的情况就叫做**「协变」**</font>。

**为什么要支持协变很容易理解：**类型系统支持了父子类型，那如果子类型还不能赋值给父类型，还叫父子类型么（👀 OOP 的多态？）？所以型变是实现类型父子关系必须的，它在保证类型安全的基础上，增加了类型系统的灵活性。

#### 逆变 ( contravariant )

我们有这样两个函数：

```typescript
let printHobbies: (guang: Guang) => void;

printHobbies = (guang) => {
    console.log(guang.hobbies);
}

let printName: (person: Person) => void;

printName = (person) => {
    console.log(person.name);
}
```

<font color=lightSeaGreen>printHobbies 的参数 Guang 是 printName 参数 Person 的子类型</font>。那么问题来了：<font color=FF0000>printName 能赋值给 printHobbies 么？printHobbies 能赋值给 printName 么？</font>测试一下发现是这样的：

<img src="https://s2.loli.net/2022/05/06/xzCZB25DoOHPvAF.png" alt="image-20220506151111699" style="zoom:48%;" />

> 👀 这张图上的报错，是在 `tsconfig.json` 中 `strictFunctionTypes` 属性被设置为 true 时的报错；而默认情况（ `tsconfig.json` 中不设置 `strictFunctionTypes` ）下，值为 false ，是不会报错的。另外，下面也会说到 「双向协变」。

<font color=FF0000>printName 的参数 Person 不是 printHobbies 的参数 Guang 的 **父类型** 么，**为啥能赋值给子类型**？</font>

<font color=FF0000>因为这个函数 ( printHobbies ) 调用的时候是按照 Guang 来约束的类型，但实际上函数只用到了父类型 Person 的属性和方法</font>；当然不会有问题，依然是类型安全的。

<font size=4>**这就是<font color=FF0000>「逆变」</font>，<font color=FF0000>函数的参数有逆变的性质</font>（ 而<font color=FF0000>返回值是协变的</font>，也就是子类型可以赋值给父类型 ）**</font>

<font color=FF0000>**那反过来呢，如果 printHobbies 赋值给 printName 会发生什么？**</font>

因为函数 ( printName ) 声明的时候是按照 Person 来约束类型，但是<font color=FF0000>**调用的时候是按照 Guang 的类型来访问的属性和方法**</font>，那自然类型不安全了，所以就会报错。

但是，<font color=FF0000>**在 ts2.x 之前** 支持这种赋值，也就是父类型可以赋值给子类型，子类型可以赋值给父类型，**既逆变又协变，叫做「双向协变」**</font>。

但是<font color=FF0000>**这明显是有问题的，不能保证类型安全**</font>；所以之后 ts 加了一个编译选项 `"strictFunctionTypes"` ，设置为 true 就只支持函数参数的逆变，设置为 false 则是双向协变。

<img src="https://s2.loli.net/2022/05/06/Td1s2iyHBvKeCac.png" alt="image-20220506152355755" style="zoom:50%;" />

> 👀 上面的截图是 [TS Playground](https://www.typescriptlang.org/play) 中的设置。

我们把 strictFunctionTypes 关掉之后，就会发现两种赋值都可以了：

<img src="https://s2.loli.net/2022/05/06/r1uc8yL62ZBoqWt.png" alt="image-20220506151142094" style="zoom:48%;" />

这样 ( `"strictFunctionTypes"` 为 false ) 就支持函数参数的双向协变，类型检查不会报错，但不能严格保证类型安全。开启（ `"strictFunctionTypes"` 为 true ）之后，函数参数就只支持逆变，子类型赋值给父类型就会报错（如上，报错的图）

#### 逆变性质有什么用

还记得之前 “联合转交叉” 的实现么 [[#UnionToIntersection]] ？

```typescript
type UnionToIntersection<U> = 
    (U extends U ? (x: U) => unknown : never) extends (x: infer R) => unknown
        ? R
        : never
```

类型参数 U 是要转换的联合类型。`U extends U` 是为了触发联合类型的「分布式条件类型」的性质，让每个类型单独传入做计算，最后合并。<font color=FF0000>**利用 U 作为参数构造个函数，通过模式匹配取参数的类型**</font>。结果就是交叉类型：

<img src="https://s2.loli.net/2022/05/04/CSxsB6PjJDNRTlv.png" alt="image-20220504230627100" style="zoom:50%;" />

我们通过构造了多个函数类型，然后模式提取参数类型的方式，来实现了联合转交叉；<font color=FF0000>这里就是因为函数参数是逆变的，会返回联合类型的几个类型的子类型，也就是更具体的交叉类型</font>。

<font color=FF0000>**逆变和协变** 都是型变（类型变化），**是针对父子类型而言**的</font>；<font color=FF0000>非父子类型自然就不会型变</font>，也就是<font color=FF0000>「不变」</font>。

#### 不变 ( invariant )

非父子类型之间不会发生型变，只要类型不一样就会报错：

<img src="https://s2.loli.net/2022/05/06/ATrHucOyZe824xF.png" alt="image-20220506155358011" style="zoom:50%;" />

<font color=FF0000>那**类型之间的父子关系是怎么确定** 的呢，好像也**没有看到 extends 的继承**？</font>

#### 类型父子关系的判断

像 Java 里面的类型都是通过 extends 继承的，如果 `A extends B` ，那 A 就是 B 的子类型。这种叫做<font color=FF0000 size=4>**名义类型系统**</font> ( nominal type ) 。

而 <font color=FF0000>**TS 里不看这个**</font>：<font color=FF0000 size=4>**只要结构上是一致的，那么就可以确定父子关系**</font>，这种叫做 <font color=FF0000 size=4>**结构类型系统**</font> ( structual type ) 。

如上面的示例：

```ts
interface Person {
  name: string;
  age: number;
} 

interface Guang {
  name: string;
  age: number;
  hobbies: string[]
}

let person: Person = {
  name: '',
  age: 20
};

let guang: Guang = {
  name: 'guang',
  age: 20,
  hobbies: ['play game', 'writing']
};
```

Guang 和 Person 有 extends 的关系么？没有呀。

那是怎么确定父子关系的？<font color=FF0000>**通过结构**</font>，<font color=FF0000>**更具体的那个是子类型**</font>。<font color=FF0000>这里的 Guang 有 Person 的所有属性，并且还多了一些属性，所以 Guang 是 Person 的子类型</font>。<font color=FF0000>注意，**这里用的是更具体，而不是更多**</font>。

判断联合类型父子关系的时候， `'a' | 'b'` 和 `'a' | 'b' | 'c'` 哪个更具体？ `'a' | 'b'` 更具体，所以 `'a' | 'b'` 是 `'a' | 'b' | 'c'` 的子类型。

<img src="https://s2.loli.net/2022/05/06/mIF48AVt7LcwkTg.png" alt="image-20220506160308699" style="zoom:50%;" />



### TSC 和 Babel

#### TSC 的编译流程

typescript compiler 的编译流程是这样的：

<img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/85851ebe6f2d41a28ca3885d91beb969~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" alt="img" style="zoom:75%;" />

源码要<font color=FF0000>先用 **Scanner** 进行 <font size=4>**词法分析**</font></font>，<font color=FF0000>拆分成一个个不能细分的单词</font>，叫做 <font color=FF0000>Token</font> 。

<font color=FF0000>然后用 **Parser** 进行 <font size=4>**语法分析**</font>，组装成 **抽象语法树**</font> ( Abstract Syntax Tree ) AST。

<font color=FF0000>之后做 <font size=4>**语义分析**</font>，包括用 **Binder** 进行 **作用域分析**，和 **Checker** 做 **类型检查**</font>。如果有类型的错误，就是在 Checker 这个阶段报的。

**如果有 <font color=FF0000>Transformer 插件</font>**（ TSC 支持 custom transform ），会<font color=FF0000>**在 Checker 之后调用**</font>，<font color=FF0000>**可以对 AST 做各种增删改**</font>。

<font color=FF0000>类型检查通过后，就会用 **Emmiter** 把 AST 打印成目标代码，**生成类型声明文件 d.ts** ，还有 **sourcemap**</font>。

> sourcemap 的作用是<font color=lightSeaGreen>映射源码和目标代码的代码位置</font>，这样调试的时候打断点可以定位到相应的源码，线上报错的时候也能根据 sourcemap 定位到源码报错的位置。

tsc 生成的 AST 可以用 [astexplorer.net](https://link.juejin.cn/?target=https%3A%2F%2Fastexplorer.net%2F) 可视化的查看

生成的目标代码和 d.ts 和报错信息也可以用 [ts playground](https://link.juejin.cn/?target=https%3A%2F%2Fwww.typescriptlang.org%2Fplay%3Fts%3D4.5.0-beta%23code%2FDYUwLgBA9gRgVgLggJRAYygJwCYB4DOYmAlgHYDmANBAIakCeAfANwBQrscEAvBAN6sIQiKRoBbEEgDk5AK51yU1gF8gA) 来直接查看

#### Babel 编译流程

babel 的编译流程是这样的：

<img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b515ccf55fe4706a128ad38b50b1c24~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?" alt="img" style="zoom: 55%;" />

<font color=FF0000>源码经过 Parser 做 **词法分析** 和 **语法分析**，生成 Token 和 AST</font> 。<font color=FF0000>AST 会做 **语义分析** **生成作用域信息**</font>，然后会调用 Transformer 进行 AST 的转换。<font color=FF0000>最后会用 **Generator** 把 AST 打印成 **目标代码** 并 **生成 sourcemap**</font> 。

Babel 的 AST 和 token 也可以用 [astexplorer.net](https://link.juejin.cn/?target=https%3A%2F%2Fastexplorer.net%2F) 可视化的查看，如果想看到 Tokens，需要点开设置，开启 Tokens 。

而且 Babel 也有 [playground](https://link.juejin.cn/?target=https%3A%2F%2Fwww.babeljs.cn%2Frepl)（ Babel 的叫 REPL ）可以直接看编译之后生成的代码。

##### 其实对比下 tsc 的编译流程，区别并不大：

<font color=FF0000>Parser 对应 TSC 的 Scanner 和 Parser</font> ，<font color=FF0000>都是做 **词法分析** 和 **语法分析**</font>，只<font color=FF0000>不过 babel 没有细分</font>。

Transform 阶段做语义分析和代码转换，对应 TSC 的 Binder 和 Transformer。**只不过 <font color=FF0000>babel 不会做 类型检查，没有 Checker</font>。**

Generator 做目标代码和 SourceMap 的生成，对应 TSC 的 Emitter。**只不过因为没有类型信息，不会生成 d.ts。**

对比两者的编译流程，会发现： <font color=FF0000>**babel 除了不会做 类型检查 和 生成类型声明文件 外，tsc 能做的事情，babel 都能做**</font>。

**看起来好像是这样的，但是 Babel 和 TSC 实现这些功能是有区别的**

#### Babel 和 TSC 的区别

抛开 **类型检查** 和 **生成 d.ts** 这两个 Babel 不支持的功能不谈，我们看下其他功能的对比。分别对比下 **语法支持** 和 **代码生成** 两方面：

##### 语法支持

TSC 默认支持最新的 ES 规范的语法和一些还在草案阶段的语法（比如 decorators ），想支持新语法就要升级 TSC 的版本。

<font color=FF0000>Babel 是 **通过 @babel/preset-env 按照目标环境 targets 的配置自动引入需要用到的插件来支持标准语法**，对于还在草案阶段的语法需要单独引入 @babel/proposal-xx 的插件来支持</font>。

所以<font color=FF0000>**如果你只用标准语法，那用 TSC 或者 Babel 都行**</font>；但是<font color=FF0000>如果你**想用一些草案阶段的语法**，**TSC 可能很多都不支持**，而 **Babel 却可以通过引入 @babel/poposal-xx 的插件来支持**</font>。从支持的语法特性上来说，Babel 更多一些。

##### 代码生成

<font color=FF0000>**TSC 生成的代码没有做 polyfill 的处理**，想做兼容处理就需要在入口处引入 core-js</font> <font color=FF0000>（ polyfill 的实现 ）</font>[GitHub - core-js](https://github.com/zloirock/core-js)。**注：**入口处的意思是 “at the top of your entry point” ，摘抄自 [GitHub - core-js](https://github.com/zloirock/core-js) 的 README.md

```typescript
import "core-js";

Promise.resolve;
```

babel 的 `@babel/preset-env` 可以根据 targets 的配置来自动引入需要的插件，引入需要用到的 core-js 模块。

引入方式可以通过 useBuiltIns 参数 来配置：

- **`entry`** 是在入口引入根据 targets 过滤出的所有需要用的 core-js。

- **`usage`** 则是每个模块按照使用到了哪些来按需引入。

```js
// babel.config.js
module.exports = {
    presets: [
        [
            '@babel/preset-typescript',
            '@babel/preset-env',
            {
                targets: '目标环境',
                useBuiltIns: 'entry' // ‘usage’
            }
        ]
    ]
}
```

此外，babel 会注入一些 helper 代码，可以通过 `@babel/plugin-transform-runtime` 插件抽离出来，从 `@babel/runtime` 包引入。（ transform runtime 顾名思义就是 transform to runtime，转换成从 runtime 包引入 helper 代码的方式）。

所以一般 babel 都会这么配：

```js
module.exports = {
    presets: [
        [
            '@babel/preset-typescript',
            '@babel/preset-env',
            {
                targets: '目标环境',
                useBuiltIns: 'usage' // ‘entry’
            }
        ]
    ],
    plugins: [ '@babel/plugin-transform-runtime']
}
```

**总结：**Babel 和 TSC 生成代码的区别

**TSC 生成的代码没有做 polyfill 的处理，<font color=FF0000>需要全量引入 core-js</font> ；而 <font color=FF0000>Babel 则可以用 `@babel/preset-env` 根据 targets 的配置来 <font size=4>按需引入</font> core-js 的部分模块</font>，所以<font color=FF0000>生成的代码体积更小</font>。**

看起来用 Babel 编译 TS 代码全是优点？也不全是：<font color=FF0000 size=4>**Babel 有一些 TS 语法并不支持**</font>

#### Babel 不支持的 TS 语法

> 👀 这里有大量截图和代码，不适合笔记，所以做了省略。

<font color=FF0000>**Babel 是每个文件单独编译的**</font>，而 <font color=FF0000>**TSC 不是**</font>；<font color=FF0000>**TSC 是整个项目一起编译**：会处理类型声明文件，会做跨文件的类型声明合并，**比如 namespace 和 interface 就可以跨文件合并**</font>。所以 Babel 编译 TS 代码有一些特性是没法支持的：

##### const enum 不支持

TS 中 const enum 编译之后是直接替换用到 enum 的地方为对应的值。<font color=FF0000>const enum 是在编译期间把 enum 的引用替换成具体的值，需要解析类型信息</font>；而 <font color=FF0000>**Babel 并不会解析**，所以**它会把 const enum 转成 enum 来处理**</font>。

> 💡 这里提到了 `const enum` ，这是之前完全没有介绍的，这里做下介绍，以及和 `enum` 的区别：
>
> > `enum` 在编译后会生成额外的 js 的的代码，例如：
> >
> > ```ts
> > enum LogLevel {
> >   ERROR,
> >   WARN,
> >   INFO,
> >   DEBUG,
> > }
> > 
> > console.log(LogLevel.ERROR)
> > ```
> >
> > 经过 `tsc` 编译，生成如下 js ：
> >
> > ```ts
> > var LogLevel;
> > (function (LogLevel) {
> >     LogLevel[LogLevel["ERROR"] = 0] = "ERROR";
> >     LogLevel[LogLevel["WARN"] = 1] = "WARN";
> >     LogLevel[LogLevel["INFO"] = 2] = "INFO";
> >     LogLevel[LogLevel["DEBUG"] = 3] = "DEBUG";
> > })(LogLevel || (LogLevel = {}));
> > console.log(LogLevel.ERROR);
> > ```
> >
> > 如果改成 `const enum` ，经过 `tsc` 编译，则不会有 `LogLevel` 的定义与 “Reverse mappings”；代码如下：
> >
> > ```js
> > console.log(0 /* LogLevel.ERROR */);
> > ```
> >
> > 可以看到：编译后，enum 的代码完全没有了，对 js 的侵入没有了。
> >
> > 摘自：[Typescript有什么冷门但是很好用的特性？ - 稳定的强酸的回答 - 知乎](https://www.zhihu.com/question/276172039/answer/3068655031)

##### namespace 部分支持：不支持 namespace 的合并，不支持导出非 const 的值

<font color=FF0000>**Babel 对每个 namespace 都是单独处理**</font> 

##### Babel 不支持 namespace  导出非 const 的值

TS 的 namespace 是可以导出非 const 的值的，后面可以修改；但是 babel 并不支持

##### 部分语法不支持

像 `export = import =` 这种过时的模块语法并不支持

**开启了 jsx 编译之后，不能用 `<type> variable`  的方式做类型断言**。ts 是可以做类型断言来修改某个类型到某个类型的：用 `variable as type`  或者 `<type> variable` 的方式；但是如果开启了 jsx 编译之后， 的形式会和 jsx 的语法冲突，所以就不支持 做类型断言了。TSC 都不支持，Babel 当然也是一样

> 👀 这里有点乱，不过下面有总结

#### babel 还是 tsc ？

babel 不支持 `const enum`（会作为 enum 处理），不支持 namespace 的跨文件合并，导出非 const 的值，不支持过时的 `export = import =` 的模块语法。

这些其实影响并不大，<font color=FF0000>只要代码里没用到这些语法，完全可以用 babel 来编译 ts</font>。

<font color=FF0000>babel 编译 ts 代码的优点是 **可以通过插件支持更多的语言特性**</font>，而且<font color=FF0000>**生成的代码是按照 targets 的配置按需引入 core-js 的**</font>；而 <font color=FF0000>tsc 没做这方面的处理，只能全量引入</font>。而且 <font color=FF0000>tsc 因为要做类型检查所以是比较慢的</font>，而 <font color=FF0000>**babel 不做类型检查，编译会快很多**</font>。

那用 babel 编译，就不做类型检查了么？<font color=FF0000>**可以用 `tsc --noEmit` 来做类型检查，加上 noEmit 选项就不会生成代码了**</font>。如果你要生成 d.ts，也要单独跑下 tsc 编译。



## 官方文档阅读



#### Generic

Note that <font color=red>**it is not possible to create generic enums and namespaces**</font>.

##### Generic Classes

As we cover in [our section on classes](https://www.typescriptlang.org/docs/handbook/2/classes.html), a class has two sides to its type: the static side and the instance side. <font color=red>Generic classes are **only generic over their instance side** rather than their static side</font>, so when working with classes, <font color=red>static members **can not use** the class’s type parameter</font>.

##### Generic Constraints

If you remember from an earlier example（👀 如下截图 ）, you may sometimes want to write a generic function that works on a set of types where you have *some* knowledge about what capabilities that set of types will have. In our `loggingIdentity` example, we wanted to be able to access the `.length` property of `arg` , <font color=LightSeaGreen>but the compiler could not prove that every type had a `.length` property</font>, so <font color=red>it warns us that we can’t make this assumption</font>.

<img src="https://s2.loli.net/2023/03/31/BZJuqtiM2CIfbY9.png" alt="image-20230330225703525" style="zoom:50%;" />

Instead of working with any and all types, we’d like to constrain this function to work with any and all types that *also* have the `.length` property. <font color=dodgerBlue>As long as the type has this member, we’ll allow it</font>, but it’s <font color=dodgerBlue>required to have at least this member</font>. To do so, we must list our requirement as a constraint on what `Type` can be.

To do so, we’ll <font color=red>create an interface that describes our constraint</font>. Here, we’ll create an interface that has a single `.length` property and then we’ll use this interface and the `extends` keyword to denote our constraint:

```ts
interface Lengthwise {
  length: number;
}
 
function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length); // Now we know it has a .length property, so no more error
  return arg;
}
```

Because the generic function is now constrained, it will no longer work over any and all types:

<img src="https://s2.loli.net/2023/03/31/K32jhbOoPJyvEZz.png" alt="image-20230331004952318" style="zoom:45%;" />

Instead, we need to pass in values whose type has all the required properties:

```ts
loggingIdentity({ length: 10, value: 3 });
```

##### Using Class Types in Generics

When creating factories in TypeScript using generics, it is necessary to refer to class types by their constructor functions. For example,

```ts
function create<Type>(c: { new (): Type }): Type {
  return new c();
}
```

A more advanced example uses the prototype property to infer and constrain relationships between the constructor function and the instance side of class types.

```ts
class BeeKeeper {
  hasMask: boolean = true;
}
 
class ZooKeeper {
  nametag: string = "Mikle";
}
 
class Animal {
  numLegs: number = 4;
}
 
class Bee extends Animal {
  keeper: BeeKeeper = new BeeKeeper();
}
 
class Lion extends Animal {
  keeper: ZooKeeper = new ZooKeeper();
}
 
function createInstance<A extends Animal>(c: new () => A): A {
  return new c();
}
 
createInstance(Lion).keeper.nametag;
createInstance(Bee).keeper.hasMask;
```

摘自：[TS handbook - Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)



#### Typeof Type Operator

<font color=red>If we try to use `ReturnType` on a **function name**</font> , we see an instructive error:

<img src="https://s2.loli.net/2023/03/31/VvAm9js1lfLRd3B.png" alt="image-20230331012002664" style="zoom: 50%;" />

Remember that *values* and *types* aren’t the same thing.

To refer to the *type* that the *value `f`* has, we use `typeof` :

<img src="https://s2.loli.net/2023/03/31/raqmAYbNlZ2FiCT.png" alt="image-20230331012115430" style="zoom:50%;" />

##### Limitations

<font color=dodgerBlue size=4>TypeScript **intentionally limits the sorts of expressions you can use `typeof` on**</font>.

Specifically, <font color=fuchsia>it’s only legal to use `typeof` on identifiers</font> (i.e. variable names) <font color=fuchsia>or their properties</font>.

This helps avoid the confusing trap of writing code you think is executing, but isn’t:

<img src="https://s2.loli.net/2023/03/31/CTgHIFNLG1pintS.png" alt="image-20230331012253193" style="zoom:50%;" />

摘自：[TS handbook - Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)



#### Indexed Access Types

We can use an *indexed access type* to <font color=red>look up a specific property on another type</font>:

<img src="https://s2.loli.net/2023/03/31/QKons3y2UeJrGEh.png" alt="image-20230331014535010" style="zoom:50%;" />

<font color=dodgerBlue>The indexing type **is itself a type**</font>, so we <font color=red>**can use unions, `keyof`, or other types entirely**</font> :

<img src="https://s2.loli.net/2023/03/31/F4Ig1rjyQRCzUAV.png" alt="image-20230331014721399" style="zoom:48%;" />

You’ll even see an error if you try to index a property that doesn’t exist:

<img src="https://s2.loli.net/2023/03/31/eLJNkq5hIHC1PXo.png" alt="image-20230331014809650" style="zoom:50%;" />

Another example of indexing with an arbitrary type is <font color=fuchsia size=4>**using `number` to get the type of an array’s elements**</font>（ 👀 见下面的神光教程的补充 💡）. We can combine this with `typeof` to conveniently capture the element type of an array literal:

<img src="https://s2.loli.net/2023/03/31/KGbgVdmDe5OPHhv.png" alt="image-20230331014934299" style="zoom:48%;" />

> 👀 上面 `type Age = typeof MyArray[number]["age"]` 的写法，之前完全没有见过；值得注意。

> 💡 上面神光的教程中，有提过 `[number]` 这种用法，但这里并没有讲到；这里做下补充：
>
> <font color=fuchsia>**数组转联合类型可以这样写：**</font>
>
> ```ts
> type union = ['foo', 'bar'][number]
> ```
>
> <img src="https://s2.loli.net/2022/05/07/mozrZSqcPNkYMDh.png" alt="image-20220504183301073" style="zoom:50%;" />

<font color=fuchsia>You **can only use types when indexing**</font> , meaning you can’t use a `const` to make a variable reference:

<img src="https://s2.loli.net/2023/03/31/92sKQj1HgxOn8Mr.png" alt="image-20230331020443466" style="zoom:48%;" />

However, you can use a type alias for a similar style of refactor:

```ts
type key = "age";
type Age = Person[key];
```

摘自：[TS handbook - Indexed Access Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)



#### Conditional Types

##### infer

Conditional types <font color=dodgerBlue>provide us with a way to **infer from types we compare against in the true branch** using the `infer` keyword</font>. For example, we could have inferred the element type in `Flatten` instead of fetching it out “manually” with an indexed access type（👀 如何 manually 的见下面的注）:

```ts
type Flatten<Type> = Type extends Array<infer Item> ? Item : Type;
```

> 👀 没有使用 infer 的手动写法（也无法获得真正的类型）：
>
> ```ts
> type Flatten<T> = T extends any[] ? T[number] : T;
> ```

Here, we <font color=red>used the `infer` keyword to declaratively introduce a new generic type variable named `Item`</font> instead of specifying how to retrieve the element type of `T` within the true branch. <font color=LightSeaGreen>This frees us from having to think about how to dig through and probing apart the structure of the types we’re interested in</font>.

> 👀 这里省略了 通过 infer 实现的 GetReturnType 的定义和实现

<font color=dodgerBlue>When inferring from a type with **multiple call signatures**</font> (such as the type of an overloaded function), <font color=red>inferences are made from the *last* signature</font> (which, presumably, is the most permissive catch-all case). It is not possible to perform overload resolution based on a list of argument types.

<img src="https://s2.loli.net/2023/03/31/cNu1IoqCrVdaUgb.png" alt="image-20230331032339262" style="zoom:48%;" />

##### Distributive Conditional Types

<font color=dodgerBlue>When **conditional types act on a generic type**</font>, they <font color=red>**become *distributive* when given a union type**</font>. For example, take the following:

```ts
type ToArray<Type> = Type extends any ? Type[] : never;
```

<font color=dodgerBlue>If we plug a union type into `ToArray`</font> , then the <font color=red>conditional type will **be applied to each member of that union**</font>.

<img src="https://s2.loli.net/2023/03/31/RP5AFQdMkCecvbK.png" alt="image-20230331032952530" style="zoom: 48%;" />

What happens here is that `StrArrOrNumArr `distributes on: `string | number` , and maps over each member type of the union, to what is effectively: `ToArray<string> | ToArray<number>;` , which leaves us with: `string[] | number[];` 。

Typically, distributivity is the desired behavior（期望的行为）. <font color=dodgerBlue>**To avoid that behavior**</font>, you can <font color=fuchsia>**surround each side of the `extends` keyword with square brackets**</font>.

<img src="https://s2.loli.net/2023/03/31/bLHloJTk6sUZfFB.png" alt="image-20230331033233966" style="zoom:50%;" />

摘自：[TS handbook - Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)



#### Mapped Types

##### Key Remapping via `as`

<font color=dodgerBlue>In TypeScript 4.1 and onwards</font>, you <font color=red>can re-map keys in mapped types with an `as` clause in a mapped type</font>:

```ts
type MappedTypeWithNewProperties<Type> = {
    [Properties in keyof Type as NewKeyType]: Type[Properties]
}
```

You can leverage features like [template literal types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html) to create new property names from prior ones:

<img src="https://s2.loli.net/2023/03/31/GJyPHXTMgLoZNkn.png" alt="image-20230331113002050" style="zoom:48%;" />

You can filter out keys by producing `never` via a conditional type:

> 💡 这里的 `never` 说的是 `Exclude` 的实现原理，详见 [[#Exclude]]

<img src="https://s2.loli.net/2023/03/31/cEmkluI9jOCFevU.png" alt="image-20230331145007546" style="zoom:48%;" />

You can map over arbitrary unions, not just unions of `string | number | symbol`, but unions of any type:

<img src="https://s2.loli.net/2023/03/31/ELzit9mX8qVNGn1.png" alt="image-20230331145240584" style="zoom:48%;" />

##### Further Exploration

Mapped types work well with other features in this type manipulation section, for example here is [a mapped type using a conditional type](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html) which returns either a `true` or `false` <font color=dodgerBlue>depending on whether an object has the property `pii` set to the literal `true`</font> :

<img src="https://s2.loli.net/2023/03/31/fM3likr4IPEVncu.png" alt="image-20230331150232851" style="zoom:48%;" />

摘自：[TS handbook - Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)



#### Template Literal Types

Template literal types build on [string literal types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) , and <font color=red>have the ability to expand into many strings via unions</font>.

They have the same syntax as [template literal strings in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals), but are used in type positions. When used with concrete（具体的） literal types, <font color=LightSeaGreen>a template literal produces a new string literal type by concatenating</font>（串联） <font color=LightSeaGreen>the contents</font>.

<img src="https://s2.loli.net/2023/03/31/EgZ2upeo1WqF3ib.png" alt="image-20230331153911445" style="zoom:48%;" />

<font color=dodgerBlue>When a union is used in the interpolated</font>（插值的） <font color=dodgerBlue>position</font>, <font color=LightSeaGreen>the type is the set of every possible string literal that could be represented by each union member</font>:

![image-20230331162436534](https://s2.loli.net/2023/03/31/ugoabd3tFjnHPMh.png)

For each interpolated position in the template literal, the unions are cross multiplied:

<img src="https://s2.loli.net/2023/03/31/lQoZcxEv2bfHMgz.png" alt="image-20230331162931897" style="zoom:50%;" />

##### String Unions in Types

<font color=dodgerBlue>The power in template literals comes</font> when <font color=LightSeaGreen>defining a new string based on information inside a type</font>.

> 👀 下面的示例有点啰嗦，下面内容是总结后的内容

有如下需求：有这样一个 `passedObject` 对象，需要给对象添加一个 `on` 方法，该方法有两个参数：第一个参数是一个 string，形式为 `${objMemberName}Changed` ；第二个参数是一个 callback 函数，callback fn 传入一个 newValue，没有返回值。效果示例如下所示：

```ts
const passedObject = {
  firstName: "Saoirse",
  lastName: "Ronan",
  age: 26,
};

// makeWatchedObject has added `on` to the anonymous Object
person.on("firstNameChanged", (newValue) => {
  console.log(`firstName was changed to ${newValue}!`);
});
```

实现代码如下：

```ts
type PropEventSource<Type> = {
    on(eventName: `${string & keyof Type}Changed`, callback: (newValue: any) => void): void;
};
 
/// Create a "watched object" with an 'on' method
/// so that you can watch for changes to properties.
declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;
```

With this, we can build something that errors when given the wrong property:

<img src="https://s2.loli.net/2023/04/03/PTeqtsNy2jYfR48.png" alt="image-20230403141149806" style="zoom:50%;" />

##### Inference with Template Literals

Notice that <font color=lightSeaGreen>we did not benefit from all the information provided in the original passed object</font>. <font color=dodgerBlue>Given change of a `firstName`</font> ( i.e. a `firstNameChanged` event ), we should expect that the callback will receive an argument of type `string`. Similarly, <font color=dodgerBlue>the callback for a change to `age` should receive a `number` argument</font>. We’re naively using `any` to type the `callBack`’s argument. Again, template literal types make it possible to ensure an attribute’s data type will be the same type as that attribute’s callback’s first argument.

The key insight that makes this possible is this: <font color=dodgerBlue>we can use a function with a generic such that:</font>

1. The literal used in the first argument is captured as a literal type
2. That literal type can be validated as being in the union of valid attributes in the generic
3. The type of the validated attribute can be looked up in the generic’s structure using Indexed Access
4. This typing information can *then* be applied to ensure the argument to the callback function is of the same type

```ts
type PropEventSource<Type> = {
    on<Key extends string & keyof Type>
        (eventName: `${Key}Changed`, callback: (newValue: Type[Key]) => void ): void;
};
 
declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;
```

<img src="https://s2.loli.net/2023/04/03/aIvtJGC1yd7iN9W.png" alt="image-20230403150550061" style="zoom:48%;" />

##### Intrinsic String Manipulation Types

To help with string manipulation, <font color=dodgerBlue>TypeScript includes a set of types which can be used in string manipulation</font>. These types come built-in to the compiler for performance and can’t be found in the `.d.ts` files included with TypeScript.

`Uppercase<StringType>`、`Lowercase<StringType>`、`Capitalize<StringType>`、`Uncapitalize<StringType>`

摘自：[TS handbook - Template Literal Types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)



#### The `satisfies` Operator

<font color=dodgerBlue>TypeScript developers are often faced with a dilemma</font>: we <font color=red>want to ensure that some expression *matches* some type</font>, <font color=dodgerBlue>**but also**</font> <font color=red>want to keep the *most specific* type of that expression **for inference purposes**</font>.

```ts
// Each property can be a string or an RGB tuple.
const palette = {
    red: [255, 0, 0],
    green: "#00ff00",
    bleu: [0, 0, 255] // 👀 这里没有报错，但下面会
//  ^^^^ sacrebleu - we've made a typo!
};
// We want to be able to use string methods on 'green'...
const greenNormalized = palette.green.toUpperCase();
```

<img src="https://s2.loli.net/2024/11/19/LQsEZSOuxz9gGaA.png" alt="image-20241119000038787" style="zoom:50%;" />

Notice that we’ve written `bleu`, whereas we probably should have written `blue`. We could try to catch that `bleu` typo by using a type annotation on `palette`, but we’d lose the information about each property.

```ts
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];
const palette: Record<Colors, string | RGB> = {
    red: [255, 0, 0],
    green: "#00ff00",
    bleu: [0, 0, 255]
//  ~~~~ The typo is **now correctly detected**
};
/*
  But we now **have an undesirable error** here - 'palette.green' "could" be of type RGB and property 'toUpperCase' does not exist on type 'string | RGB'.
*/
const greenNormalized = palette.green.toUpperCase();
```

<img src="https://s2.loli.net/2024/11/18/OxtDZceholkUafL.png" alt="image-20241118235500784" style="zoom:50%;" />

<font color=dodgerBlue>The new `satisfies` operator</font> <font color=red>**lets us validate that the type of an expression matches some type**</font>, <font color=dodgerBlue>without changing the resulting type of that expression</font>. As an example, we could use `satisfies` to validate that all the properties of `palette` are compatible with `string | number[]` :

```ts
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];
const palette = {
    red: [255, 0, 0],
    green: "#00ff00",
    bleu: [0, 0, 255]
//  ~~~~ The typo is now caught!
} satisfies Record<Colors, string | RGB>;
// toUpperCase() method is still accessible!
const greenNormalized = palette.green.toUpperCase();
```

<img src="https://s2.loli.net/2024/11/18/8eQ6WXtga7mNdUO.png" alt="image-20241118235708428" style="zoom:50%;" />

<font color=red>`satisfies` can be used to catch lots of possible errors</font>. For example, we could ensure that an object has *all* the keys of some type, but no more:

```ts
type Colors = "red" | "green" | "blue";
// Ensure that we have exactly the keys from 'Colors'.
const favoriteColors = {
    "red": "yes",
    "green": false,
    "blue": "kinda",
    "platypus": false
//  ~~~~~~~~~~ error - "platypus" was never listed in 'Colors'.
} satisfies Record<Colors, unknown>;
// All the information about the 'red', 'green', and 'blue' properties are retained.
const g: boolean = favoriteColors.green;
```

Maybe we don’t care about if the property names match up somehow, but we do care about the types of each property. In that case, we can also ensure that all of an object’s property values conform to some type.

```ts
type RGB = [red: number, green: number, blue: number];
const palette = {
    red: [255, 0, 0],
    green: "#00ff00",
    blue: [0, 0]
    //    ~~~~~~ error!
} satisfies Record<string, string | RGB>;
// Information about each property is still maintained.
const redComponent = palette.red.at(0);
const greenNormalized = palette.green.toUpperCase();
```

<img src="https://s2.loli.net/2024/11/19/qa5CXf1jdSzoYOG.png" alt="image-20241119003334374" style="zoom:50%;" />

摘自：[TS doc - handbook - release-notes - ts4.9 # The `satisfies` Operator](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator)



#### literal types 字面量类型

In addition to the general types `string` and `number` , <font color=red>we can refer to *specific* strings and numbers in type positions</font>.

One way to think about this is to consider how JavaScript comes with different ways to declare a variable. Both `var` and `let` allow for changing what is held inside the variable, and `const` does not. <font color=red>This is reflected in how TypeScript creates types for literals</font>.

<img src="https://s2.loli.net/2022/12/16/2ewPRachpULzHSm.png" alt="image-20221216152859931" style="zoom:45%;" />

By themselves, literal types aren’t very valuable: 

> 👀 感觉意思是：对于它们自身，字面量类型没有那么有用

<img src="https://s2.loli.net/2022/12/16/IHrQLm6aNjvkuRx.png" alt="image-20221216154112706" style="zoom:45%;" />

It’s not much use to have a variable that can only have one value!

<font color=red>But **by *combining* literals into unions** , you can express a much more useful concept</font> - for example, <font color=dodgerBlue>functions that only accept a certain set of known values</font>:

<img src="https://s2.loli.net/2022/12/16/LBnOJDu73vizVNe.png" alt="image-20221216154501257" style="zoom:45%;" />

<font color=dodgerBlue>**Numeric literal types work the same way:**</font>

<img src="https://s2.loli.net/2022/12/16/ZsTVJrNi9lRFS2k.png" alt="image-20221216154735957" style="zoom:45%;" />

Of course, you <font color=dodgerBlue>can combine these with non-literal types</font>:

> 👀 即字面量类型和 非字面量类型 进行联合，从而约束

<img src="https://s2.loli.net/2022/12/16/XxIRulzLwk3nmcJ.png" alt="image-20221216154916542" style="zoom:45%;" />

There’s one more kind of literal type: boolean literals. There are only two boolean literal types, and as you might guess, they are the types `true` and `false`. <font color=red>The type `boolean` itself is actually just an alias for the union `true | false`</font> .

<img src="https://s2.loli.net/2022/12/16/VrogC1Mfhy84eZ7.png" alt="image-20221216155543167" style="zoom:60%;" />

摘自：[TS Doc - Everyday Types # Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types)



#### module

// TODO

摘自：[TS handbook V2 - module](https://www.typescriptlang.org/docs/handbook/2/modules.html)

> 👀 另外，有空的话看下 [TS handbook V1 - module](https://www.typescriptlang.org/docs/handbook/modules.html) V2 删掉了不少内容，考虑到兼容还是有必要稍微看下



#### const assertions 常量断言

> 👀 在 [[#神光《 TypeScript 类型体操通关秘籍》笔记#as const]] 中有对 `as const` 做过介绍，不过在阅读过官方文档之后，发现还有少了点东西，所以做下补充

<font color=red>TypeScript 3.4</font> introduces a new construct for literal values called *`const`* assertions. Its syntax <font color=red>is a type assertion with `const`</font> in place of the type name (e.g. `123 as const` ). When we construct new literal expressions with `const` assertions, we can signal to the language that

- no literal types（字面量类型） in that expression should be widened (e.g. no going from `"hello"` to `string`)

- object literals <font color=red>get `readonly` properties</font>

- array literals <font color=fuchsia>**become `readonly` tuples**</font>

  > 👀 这个说法，在 [使用元祖生成联合类型【渡一教育】](https://www.bilibili.com/video/BV1VNt3e7E4r) 中也有提及，这点是之前完全没注意到的；甚至这里摘抄文档时，也没注意。不过，视频中只说了将其转换为元组，但是没说将其转换为 “readonly” 的元组
  >
  > <img src="https://s2.loli.net/2024/09/24/BEaPfyZHvUoGhpN.png" alt="image-20240924001631007" style="zoom:50%;" />

```ts
// Type '"hello"'
let x = "hello" as const;

// Type 'readonly [10, 20]'
let y = [10, 20] as const;

// Type '{ readonly text: "hello" }'
let z = { text: "hello" } as const;
```

> 👀 关于 上面的 `let x = "hello" as const` ， [[#神光《 TypeScript 类型体操通关秘籍》笔记#as const]] 没有讲，看了文档后感觉作用就是：将 `let` 定义的原始类型变成 `const` 定义的（字面量类型）
>
> <img src="https://s2.loli.net/2022/12/16/w9BPjm2MKlYHQZo.png" alt="image-20221216134143766" style="zoom: 65%;" />
>
> `const str = 'foo'` 是一样的：
>
> <img src="https://s2.loli.net/2022/12/16/g9EKUjxqAXJOPYc.png" alt="image-20221216134408784" style="zoom:67%;" />

<font color=red>Outside of `.tsx` files</font> , the angle bracket assertion syntax can also be used.

```ts
// Type '"hello"'
let x = <const>"hello";

// Type 'readonly [10, 20]'
let y = <const>[10, 20];

// Type '{ readonly text: "hello" }'
let z = <const>{ text: "hello" };
```

摘自： [TS Doc - TypeScript 3.4 # const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)



#### Type-Only Imports and Export

This feature is something most users may never have to think about; however, <font color=lightSeaGreen>if you’ve hit issues under [`isolatedModules`](https://www.typescriptlang.org/tsconfig#isolatedModules), TypeScript’s `transpileModule` API, or Babel</font>, this feature might be relevant.

> 💡关于 `isolatedModules` ：
>
> <img src="https://s2.loli.net/2024/03/17/A18EDMCzijxvXZQ.png" alt="image-20240317145618908" style="zoom:50%;" />

<font color=dodgerBlue>**TypeScript 3.8** adds a new syntax for type-only imports and exports</font>.

```ts
import type { SomeThing } from "./some-module.js";
export type { SomeThing };
```

`import type` only imports declarations to be used for type annotations and declarations. <font color=red>**It *always* gets fully erased**, so there’s no remnant of it at runtime</font>. Similarly, `export type` only provides an export that can be used for type contexts, and <font color=red>is also erased from TypeScript’s output</font>.

<font color=dodgerBlue>It’s important to note that</font> <font color=fuchsia>classes have a value at runtime and a type at design-time</font>, and the use is context-sensitive. When using `import type` to import a class, you can’t do things like extend from it.

```ts
import type { Component } from "react";
interface ButtonProps {
  // ...
}
class Button extends Component<ButtonProps> {
  //               ~~~~~~~~~
  // error! 'Component' only refers to a type, but is being used as a value here.
  // ...
}
```

If you’ve used Flow before, the syntax is fairly similar. One difference is that we’ve added a few restrictions to avoid code that might appear ambiguous.

```ts
// Is only 'Foo' a type? Or every declaration in the import?
// We just give an error because it's not clear.
import type Foo, { Bar, Baz } from "some-module";
//     ~~~~~~~~~~~~~~~~~~~~~~
// error! A type-only import can specify a default import or named bindings, but not both.
```

> ⚠️ 上面的注释可以注意下

<font color=dodgerBlue>In conjunction with `import type`</font> , <font color=red>TypeScript 3.8 also **adds a new compiler flag** to control what happens with imports that won’t be utilized at runtime: [`importsNotUsedAsValues`](https://www.typescriptlang.org/tsconfig#importsNotUsedAsValues)</font> . <font color=dodgerBlue>This flag takes 3 different values</font>:

- `remove` : this is today’s behavior of dropping these imports. It’s going to continue to be the default, and is a non-breaking change.
- `preserve` : this *preserves* all imports whose values are never used. This <font color=red>can cause imports/side-effects to be preserved</font>.
- `error` : this preserves all imports (the same as the `preserve` option), but <font color=red>will error when a value import is only used as a type</font>. This might be useful if you want to ensure no values are being accidentally imported, but still make side-effect imports explicit.

For more information about the feature, you can [take a look at the pull request](https://github.com/microsoft/TypeScript/pull/35200), and [relevant changes](https://github.com/microsoft/TypeScript/pull/36092/) around broadening where imports from an `import type` declaration can be used.

摘自：[TS Doc - handbook - TypeScript 3.8 # Type-Only Imports and Export](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-export)



#### `// @ts-ignore`

<font color=dodgerBlue>**TypeScript 2.6**</font> support <font color=fuchsia>suppressing</font>（镇压，翻译成 隐藏/忽略） <font color=fuchsia>errors</font> in .js files using `// @ts-ignore` comments <font color=fuchsia>placed above the offending</font>（惹麻烦的） <font color=fuchsia>lines</font>.

示例：

```ts
if (false) {
  // @ts-ignore: Unreachable code error
  console.log("hello");
}
```

> 👀 这里上面注释中的 “Unreachable code error” 很重要，同时也容易忽略，即：`@ts-ignore` 仅会对不会执行的错误代码进行忽略。如果会执行，也会报错。比如如下代码（ log 写错），一定会报错
>
> ```ts
> // @ts-ignore
> console.logg('hello')
> ```

A `// @ts-ignore` comment <font color=fuchsia>suppresses **all errors**</font> that originate on the <font color=fuchsia>**following line**</font>（ 👀 接下来的**一行**，即无法作用于代码块）. It is <font color=red>recommended practice to **have the remainder of the comment following `@ts-ignore` explain which error is being suppressed**</font> （译：建议实践中在 `@ts-ignore ` 之后添加相关提示，解释忽略了什么错误。👀 如上示例代码）.

Please note that this comment <font color=red>only suppresses the error reporting</font>（译：仅会隐藏报错）, and we <font color=red>recommend you use this comments *very sparingly*</font>.

摘自：[TS Doc -  handbook - TS 2.6 - Suppress errors in .ts files using ’// @ts-ignore’ comments](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-6.html#suppress-errors-in-ts-files-using--ts-ignore-comments)



#### `// @ts-nocheck`

<font color=dodgerBlue>**TypeScript 3.7**</font> allows us to <font color=red>add `// @ts-nocheck` comments to the **top of TypeScript files** to <font size=4>**disable semantic checks**</font></font>. <font color=dodgerBlue>**Historically**</font> this comment was only respected in **JavaScript source files** in the presence of [`checkJs`](https://www.typescriptlang.org/tsconfig#checkJs), but we’ve <font color=red>expanded support to TypeScript files to make migrations easier for all users</font>.

摘自：[TS doc - handbook - TS 3.7 - // @ts-nocheck in TypeScript Files](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html)

<font color=red>TypeScript 2.3 以后的版本支持使用 `--checkJs` 对 `.js` 文件进行类型检查并提示错误的模式</font>。

你可以通过添加 `// @ts-nocheck` 注释来忽略类型检查；相反你<font color=red>可以通过去掉 `--checkJs` 设置并添加 `// @ts-check` 注释来选择检查某些 `.js` 文件</font>。 你还可以使用 `// @ts-ignore` 来忽略本行的错误。

> 💡关于 `// @ts-check` 的简单原理，可以看下 [[VS Code 使用备忘录#Type checking JavaScript]]

摘自：[TypeScript 中文手册 - JavaScript文件里的类型检查](https://typescript.bootcss.com/type-checking-javascript-files.html)



#### Twoslash Annotations

> 💡 背景
>
> 在看 ts handbook 时，发现代码演示中存在一个 popover 可以显示 types 内容。
>
> <img src="https://s2.loli.net/2023/04/02/v1zMBirV7k9OGEm.png" alt="image-20230402234226080" style="zoom:50%;" />
>
> 类似的，点击旁边的 “Try”，进入演示代码的 ts playground 时，发现也有类似的效果：
>
> <img src="https://s2.loli.net/2023/04/02/SKZLHR2nUl5Nmjz.png" alt="image-20230402234505014" style="zoom:70%;" />
>
> 可以发现 `// ^?` 是真实内容，而后面的 `type Greeting = "hello world"` 是自动生成的 placeholder ？这便引发了我的好奇。经过查找才知道 “Twoslash Annotations” 的存在，以及 `^?` 是 “Twoslash Annotations” 中的 “Twoslash Queries”。

<font color=dodgerBlue>[Twoslash](https://www.npmjs.com/package/@typescript/twoslash) is an annotation format for TypeScript</font> which <font color=dodgerBlue>uses specially crafted comments (two slashes `//` ) as a markup format for writing code samples</font> (available on npm at [`@typescript/twoslash`](https://www.npmjs.com/package/@typescript/twoslash) ). It powers all the code samples in the TypeScript website, you can learn more about it [here](https://shikijs.github.io/twoslash/).

> 💡点击上面的 ”[here](https://shikijs.github.io/twoslash/)“ 可以发现链接是一个文档。另外，该文档还有一个 playground：[Shiki Twoslash Playground](https://shikijs.github.io/twoslash/playground) ，感觉可以制作类似的 ts doc 中代码示例（带有 popover）的截图

##### Twoslash Queries

The Playground supports <font color=dodgerBlue>showing the types at a certain location</font> by using an empty comment with a `^?` to indicate the symbol you’re interested in:

```ts
const abc = "Hello"
//    ^?
```

Would <font color=red>add a **realtime inline annotation**</font> about what the type of `abc` is into the editor. This can make typing complex types easier, and make it much more obvious when sharing code what you think is important.

> 💡 Twoslash Queries 是 TS playground 的 feat，vsc 不支持；不过，可以通过 vsc 插件 [Twoslash Query Comments](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-twoslash-queries)，使 vsc 得到支持。

##### Twoslash Compiler Flags

A more arcane, but very ergonomic（符合人体工程学的） way to set a compiler setting is via compiler flag which are comments starting with `// @`.

The editor will auto-complete twoslash commands for any compiler setting for the current version of TypeScript in your Playground. If the setting is a boolean, you don’t need to set a value:

```ts
// @isolatedModules
```

> 👀 也就是说默认为 true

<font color=LightSeaGreen>Would have `isolatedModules` turned on in your Playground</font>. You can set the values via `true/false`:

```ts
// @strictPropertyInitialization: false
```

For more settings with more choices of values, you <font color=red>can use the same descriptive string that is available in a `tsconfig.json`</font>:

```ts
// @target: esnext
// @module: nodenext
```

You can write a list with a comma separated string:

```ts
// @lib: es2015,dom
```

Writing a twoslash command will set the compiler flag as you type, and will be set instantly if you reload or share the URL with another person. This <font color=dodgerBlue>can act as a power-user tool for setting compiler flags</font> *and* <font color=dodgerBlue>making that change much more explicit than a query param</font> (which may require looking up in the TS Config dropdown.)

摘自：[TS play handbook - Twoslash Annotations](https://www.typescriptlang.org/play?#handbook-14)



#### unique symbol

// TODO : https://www.typescriptlang.org/docs/handbook/symbols.html#unique-symbol



#### using & using await

##### `using` Declarations and Explicit Resource Management

TypeScript 5.2 adds support for the upcoming [Explicit Resource Management](https://github.com/tc39/proposal-explicit-resource-management) feature in ECMAScript. Let’s explore some of the motivations and understand what the feature brings us.

> 💡 点进链接看了下：是一个 TC39 提案的 GitHub 仓库，且已经进入了 Stage 3

<font color=dodgerBlue>It’s common to need to do some sort of “clean-up” after creating an object</font>. For example, <font color=lightSeaGreen>you might need to close network connections, delete temporary files, or just free up some memory</font>.

Let’s imagine a function that creates a temporary file, reads and writes to it for various operations, and then closes and deletes it.

```ts
import * as fs from "fs";
export function doSomeWork() {
    const path = ".some_temp_file";
    const file = fs.openSync(path, "w+");
    // use file...
    // Close the file and delete it.
    fs.closeSync(file);
    fs.unlinkSync(path);
}
```

This is fine, but <font color=dodgerBlue>what happens if we need to perform an early exit?</font>

```ts
export function doSomeWork() {
    const path = ".some_temp_file";
    const file = fs.openSync(path, "w+");
    // use file...
    if (someCondition()) {
        // do some more work...
        // Close the file and delete it.
        fs.closeSync(file);
        fs.unlinkSync(path);
        return;
    }
    // Close the file and delete it.
    fs.closeSync(file);
    fs.unlinkSync(path);
}
```

We’re starting to see some duplication of clean-up which can be easy to forget. We’re also not guaranteed to close and delete the file if an error gets thrown. This could be solved by wrapping this all in a `try`/`finally` block.

```ts
export function doSomeWork() {
    const path = ".some_temp_file";
    const file = fs.openSync(path, "w+");
    try {
        // use file...
        if (someCondition()) {
            // do some more work...
            return;
        }
    }
    finally {
        // Close the file and delete it.
        fs.closeSync(file);
        fs.unlinkSync(path);
    }
}
```

While this is more robust, <font color=lightSeaGreen>it’s added quite a bit of “noise” to our code</font>. There are also other foot-guns we can run into if we start adding more clean-up logic to our `finally` block — for example, exceptions preventing other resources from being disposed. This is what the [explicit resource management](https://github.com/tc39/proposal-explicit-resource-management) proposal aims to solve. The key idea of the proposal is to support resource disposal — this clean-up work we’re trying to deal with — as a first class idea in JavaScript.

<font color=red>This **starts by adding a new built-in `symbol` called `Symbol.dispose`** </font>, and we can create objects with methods named by `Symbol.dispose`. <font color=dodgerBlue>For convenience</font>, <font color=red>**TypeScript defines a new global type called `Disposable` which describes these**</font>.

> 👀 第一遍看的时候，以为 `Symbol.dispose` 是一个实现 “Explicit Resource Management” 的方案之一，实际上它就是 一个新增的 well-known Symbol。Chrome125 和 Edge 已经支持

```ts
class TempFile implements Disposable {
    #path: string;
    #handle: number;
    constructor(path: string) {
        this.#path = path;
        this.#handle = fs.openSync(path, "w+");
    }
    // other methods
    [Symbol.dispose]() {
        // Close the file and delete it.
        fs.closeSync(this.#handle);
        fs.unlinkSync(this.#path);
    }
}
```

Later on we can call those methods.

```ts
export function doSomeWork() {
    const file = new TempFile(".some_temp_file");
    try { /* ... */ }
    finally {
        file[Symbol.dispose]();
    }
}
```

Moving the clean-up logic to `TempFile` itself doesn’t buy us much; we’ve basically just moved all the clean-up work from the `finally` block into a method, and that’s always been possible. But having a well-known “name” for this method means that JavaScript can build other features on top of it.

That brings us to the first star of the feature: `using` declarations! `using` is a new keyword that lets us declare new fixed bindings, kind of like `const`. The key difference is that variables declared with `using` get their `Symbol.dispose` method called at the end of the scope!

So we could simply have written our code like this:

```ts
export function doSomeWork() {
    using file = new TempFile(".some_temp_file");
    // use file...
    if (someCondition()) {
        // do some more work...
        return;
    }
}
```

Check it out — no `try`/`finally` blocks! At least, none that we see. Functionally, that’s exactly what `using` declarations will do for us, but we don’t have to deal with that.

You might be familiar with [`using` declarations in C#](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-8.0/using), [`with` statements in Python](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement), or [`try`-with-resource declarations in Java](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). These are all similar to JavaScript’s new `using` keyword, and provide a similar explicit way to perform a “tear-down” of an object at the end of a scope.

`using` declarations do this clean-up at the very end of their containing scope or right before an “early return” like a `return` or a `throw`n error. They also dispose in a first-in-last-out order like a stack.

```ts
function loggy(id: string): Disposable {
    console.log(`Creating ${id}`);
    return {
        [Symbol.dispose]() {
            console.log(`Disposing ${id}`);
        }
    }
}
function func() {
    using a = loggy("a");
    using b = loggy("b");
    {
        using c = loggy("c");
        using d = loggy("d");
    }
    using e = loggy("e");
    return;
    // Unreachable.
    // Never created, never disposed.
    using f = loggy("f");
}
func();
// Creating a
// Creating b
// Creating c
// Creating d
// Disposing d
// Disposing c
// Creating e
// Disposing e
// Disposing b
// Disposing a
```

`using` declarations are supposed to be resilient to exceptions; if an error is thrown, it’s rethrown after disposal. On the other hand, the body of your function might execute as expected, but the `Symbol.dispose` might throw. In that case, that exception is rethrown as well.



## TS 类型工具实现

##### Curry 实现

```ts
/** 
 * A 为参数类型，R 为返回类型
 * curry 的参数类型很好写，any[] 即可；但返回值类型不好写，因为它是受到参数类型影响的，参数类型和返回值类型之间是存在关联的。
 * 关联如下，A 和 R 可能有如下关系：
 * 1. () => R ：A 为空数组，即没有参数；返回值为 R
 * 2. (A) => R ：A 只有一项，返回值为 R
 * 3. (第一个参数类型) => 新的函数 ：A 存在多项，R 为除了第一个参数，剩余参数生成的数组
 */

type Curried<A extends any[], R> =
  A extends []
    ? () => R :
      A extends [infer P]
        ? (x: P) => R :
          A extends [infer P, ...infer Rest]
            ? (x: P) => Curried<Rest, R>
            : never

declare function curry<A extends any[], R>(
  fn: (...args: A) => R
): Curried<A, R>
```

###### 测试代码

```ts
function sum(a: number, b: number, c: number) {
  return a + b + c
}

const curried = curry(sum)
const aCurried = curried(34)
const bCurried = aCurried(12)
const cCurried = bCurried(3)
```

###### 最终效果

<img src="https://s2.loli.net/2024/07/29/BRKTxjX93i4PuSM.png" alt="image-20240729232515628" style="zoom:50%;" />

学习自：[对柯里化进行类型标注【渡一教育】](https://www.bilibili.com/video/BV1dT421r7dL)

##### 前置不定量参数

```ts
type JSTypeMap = {
  'string': string,
  'number': number,
  'boolean': boolean,
  'symbol': symbol,
  'bigint': bigint,
  'undefined': undefined,
  'object': object,
  'function': Function
}

type JSTypes = keyof JSTypeMap

type ArgsType<T extends JSTypes[]> = {
  [I in keyof T]: JSTypeMap[T[I]]
}

declare function addImpl<T extends JSTypes[]>(...args: [
  ...T,
  (...args: ArgsType<T>)=>any
]): any


addImpl('boolean', 'string', 'number', (a, b, c) => {})
```

###### 最终效果

<img src="https://s2.loli.net/2024/07/31/HjJW4v9CNUksYG2.png" alt="image-20240731234929104" style="zoom:45%;" />

<img src="https://s2.loli.net/2024/07/31/Aen5L9EgjJwMlmk.png" alt="image-20240731235014472" style="zoom:45%;" />

<img src="https://s2.loli.net/2024/07/31/fdLvShIEmC9VPHD.png" alt="image-20240731235055331" style="zoom:45%;" />

学习自：[前置的不定量参数【渡一教育】](https://www.bilibili.com/video/BV14M4m117zR)



##### 联合类型转交叉类型

```ts
type UnionToIntersection<T> = 
  (T extends any ? (x: T) => any : never) extends 
    (x: infer R) => void ? R : never
```

这里的原理是：当一个联合类型被作为参数传入函数是，将会触发逆变，变成交叉类型

可以参见 [[#UnionToIntersection]] 部分的内容，另外，其中除了讲解外，还有另一种差不多的实现

学习自：[联合类型转交叉类型【渡一教育】](https://www.bilibili.com/video/BV1tC4y1k7cK)



##### Optional 实现

```ts
type Optional<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>
```

学习自：[实现Optional【渡一教育】](https://www.bilibili.com/video/BV1fHWGeSEPf)



##### GetOptionals 实现

```ts
type GetOptionals<T> = {
  [P in keyof T as T[P] extends Required<T>[P] ? never : P]: T[P]
}
```

> ⚠️ 这里用到了 `as` 运算符做重映射，这个有所遗忘，但是 `as` 还是挺重要的，值得注意。

###### 最终效果

<img src="https://s2.loli.net/2024/09/01/9ESUD4uyRqcdZQF.png" alt="image-20240901194434421" style="zoom:50%;" />

学习自：[实现GetOptionals【渡一教育】](https://www.bilibili.com/video/BV1x2WGeYE9J)



##### 获取数组/元组的类型

```ts
type ArrayType<T> = T extends (infer I)[] ? I : T
```

> 💡 这里相当值得注意的是：`(infer I)[]` ，表示 `infer` 出的 `I` 形成的数组；不加上括号显然是错的。

###### 最终效果

<img src="https://s2.loli.net/2024/09/02/shcM9eYvP4ASHwL.png" alt="image-20240902221652949" style="zoom:50%;" />

学习自：[使用infer封装通用类型工具【渡一教育】](https://www.bilibili.com/video/BV142WGeYECk/)



##### DeepReadonly 实现

```ts
type DeepReadonly<T extends Record<string | symbol, any>> = {
  readonly [k in keyof T]: DeepReadonly<T[k]>;
};
```

###### 最终效果

<img src="https://s2.loli.net/2024/09/06/TFkGUVOnKPQuhYa.png" alt="image-20240906215702544" style="zoom:50%;" />

这里可以看下同样的实现 [[#DeepReadonly]] ，不过这里的写法更加简洁，同时上面所说的“对函数进行 readonly 限制”，好像也没问题？

学习自：[不可变类型【渡一教育】](https://www.bilibili.com/video/BV1NesHe4Eyf/)



##### 构建长属性列表

```typescript
type FieldUnion<
  Count extends number,
  Prefix extends string,
  UnionResult extends string[] = [],
> = UnionResult["length"] extends Count
  ? UnionResult[number]
  : FieldUnion<Count, Prefix, [...UnionResult, `${Prefix}${UnionResult['length']}`]>

type GeneratedObj<
  Num extends number,
  Key extends string
> = {
  [key in FieldUnion<Num, `${Key}`>]: string
}

type GeneratedObjOmitted<
  Num extends number,
  Key extends string
> = Omit<GeneratedObj<Num, Key>, `${Key}0`>

type genObj = GeneratedObjOmitted<100, 'key'>
```

> 👀 这里在原有教程的基础上，加上了 `Prefix` 作为生成对象的 key 前缀。

这里的原理并不复杂：首先生成 `` `${Prefix}0` | `${Prefix}1` | ... ｜ `${Prefix}${Count - 1}` `` 键的联合类型( `FieldUnion` ) ，之后再根据 “分布式条件类型” 的特性，生成对象类型 ( `GeneratedObj` ) ；最后再去除 `` `${Key}0` `` ，即可实现。

学习自：[用TS构建长属性列表【渡一教育】](https://www.bilibili.com/video/BV18cU5YJEgM)



## 其他笔记

#### interface VS type
##### type 可以，interface 不能
###### type 可以声明基本类型别名，联合类型，元组等类型

```ts
// 基本类型别名
type Name = string

// 联合类型
interface Dog {
    wong();
}
interface Cat {
    miao();
}

type Pet = Dog | Cat

// 具体定义数组每个位置的类型
type PetList = [Dog, Pet]
```

###### type 语句中还可以使用 typeof 获取实例的 类型进行赋值

```ts
const divEl = document.createElement('div');
type divElType = typeof divEl
```

###### type 其他操作

```ts
type StringOrNumber = string | number;  
type Text = string | { text: string };  
type NameLookup = Dictionary<string, Person>;  
type Callback<T> = (data: T) => void;  
type Pair<T> = [T, T];  
type Coordinates = Pair<number>;  
type Tree<T> = T | { left: Tree<T>, right: Tree<T> };
```

##### interface 可以，type 不能

###### interface 能够声明合并

```ts
interface User {
  name: string
  age: number
}

interface User {
  sex: string
}

/*
User 接口为 {
  name: string
  age: number
  sex: string
}
*/
```

摘自：[TypeScript真香系列——接口篇](https://juejin.cn/post/7038447161813499912)

> You should prefer `interface`. Use `type` when you need specific features.
>
> 摘自：[ts doc - TypeScript for JavaScript Programmers # Defining Types](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#defining-types)



#### 装箱类型

> 👀 虽然知道 JS 中存在包装类型，但是确实没想到 TS 中也有；想了下，JS 中存在，那么作为超集 TS 有也是顺理成章的事情了

原始类型是其装箱类型的子类型，比如 <font color=red>**string 类型会是 String 类型的子类型**</font>，同时 <font color=red>String 类型会是 Object 类型的子类型</font>。

<font color=dodgerBlue>看个比较特殊的例子：</font>

```ts
type Result = String extends {} ? 1 : 2; // 1
```

<font color=red>**`{}` 是 object 的字面量类型**</font>（👀 这点之前完全没有听过的），<font color=dodgerBlue>为什么 String 会是他的子类型</font>？在 TypeScript 中<font color=red>可以把 String 看作一个普通的对象（继承了 `{}` 这个空对象）</font>，同时有着一些独特的方法，那么在结构化类型系统的比较下，**String 会被认为是** **`{}`** **的子类型**。

从前文可知 `{}` 是 objcet 的子类型，看起来这就构建起了 `string < {} < object` 这个类型链，但实际上 `string extends object` 并不成立：

```ts
type Result = string extends object ? 1 : 2; // 2
```

<font color=red>由于结构化类型系统的存在</font>，<font color=dodgerBlue>TypeScript 中存在着一些看着不符合直觉的类型关系</font>：

```ts
type Result1 = {} extends object ? 1 : 2; // 1
type Result2 = object extends {} ? 1 : 2; // 1

type Result3 = object extends Object ? 1 : 2; // 1
type Result4 = Object extends object ? 1 : 2; // 1

type Result5 = Object extends {} ? 1 : 2; // 1
type Result6 = {} extends Object ? 1 : 2; // 1
```

- `{} extends object` 和 `{} extends Object` 意味着：`{}` 是 object 和 Object 的字面量类型，是从**类型信息的层面**出发的，即**字面量类型在基础类型之上提供了更详细的类型信息**。
- `object extends {}` 和 `Object extends {}` 则是从**结构化类型系统的比较**出发的，即 `{}` 作为一个一无所有的空对象，几乎可以被视作是所有类型的基类，万物的起源。
- 而 `object extends Object` 和 `Object extends object` 这两者的情况就要特殊一些，它们是因为“系统设定”的问题，Object 包含了所有除 Top Type 以外的类型（基础类型、函数类型等），object 包含了所有非原始类型的类型，即数组、对象与函数类型，<font color=lightSeaGreen>**这就导致了你中有我、我中有你的神奇现象**</font>。

<font color=dodgerBlue>结论</font>：只关注从类型信息层面出发部分的话，即 **原始类型 < 原始类型对应的装箱类型 < Object 类型**

学习自：[重新认识 TypeScript类型系统 # 装箱类型](https://juejin.cn/post/7374614838008774656#heading-17)



#### 类型匹配

在 TS 中，<font color=dodgerBlue>定义一个有具体属性的对象类型有三种写法</font>：接口( `interface` )、类型别名( `type` )、对象类型( `let person: { name: string, age: number }` )，<font color=lightSeaGreen>三种写法本质都一样</font>。<font color=red>如果用一个 **字面量** 的值赋值，该值所具有的属性不能多不能少，否则 TS 会报错</font>

```ts
interface IPerson {
  name: string;
  age: number;
}
 
type TPerson = {
  name: string;
  age: number;
};

const person1: { name: string; age: number } = { name: '1',age: 1, gender: 'male' };
// error: 'gender' does not exist in type '{ name: string; age: number; }'

const person2: IPerson = { name: '1',age: 1, gender: 'male' }
// error: 'gender' does not exist in type 'IPerson'

const person3: TPerson = { name: '1',age: 1, gender: 'male' }
// error: 'gender' does not exist in type 'TPerson'
```

但是如果用一个变量赋值，该变量的值就不再遵循“不多不少”的原则，而是”只多不少”。

```ts
// 结构式类型匹配 shape
const more_person = { name: '1', age: 1, gender: 'male' }
const m_person1: IPerson = more_person;
const m_person2: TPerson = more_person;
const m_person3: { name: string; age: number } = more_person;

const less_person = { name: '1' }

const l_person1: IPerson = less_person;
// Property 'age' is missing in type '{ name: string; }' but required in type 'IPerson'.

const l_person2: TPerson = less_person;
// Property 'age' is missing in type '{ name: string; }' but required in type 'TPerson'

const l_person3: { name: string; age: number } = less_person;
// Property 'age' is missing in type '{ name: string; }' but required in type '{ name: string; age: number; }
```

这种现象是与 TS 核心特性有关，TS 类型系统做的是 [结构式匹配](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#structural-type-system) ，这意味着做类型检查的时候，只要有相同的`shape`，就能匹配成功。光一句定义以及简单的几个示例真的很难理解 TS 的结构式匹配，让我们来用集合论理解试试。

我们来分析上面代码中蕴含的集合间的关系。`more_person`的类型是 `{ name: string; age: number; gender: string }`，该类型是 `IPerson`、`TPerson` 以及 `{ name: string; age: number }` 的子集，所以`more_person` 可以赋值给 `m_person1` ，`m_person2` ，`m_person3` 。

`less_person` 的类型是 `{ name: string }`，该类型是 `IPerson`、`TPerson` 以及 `{ name: string; age: number }` 的超集，所以 `less_person` 无法赋值给 `l_person1`，`l_person2`，`l_person3`。

<font color=dodgerBlue>**为什么每增加一个属性，类型集合的空间就缩小，可赋值的元素就越少？**</font>

这是因为 <font color=fuchsia>每增加一个属性，该类型可以匹配的值必然得包含这个属性，这样就使得符合该 `shape` 的值越来越少</font>。我们用**极限**的思想可以进一步理解，如果一个接口类型没有任何属性，也就是 `{}` 类型，<font color=lightSeaGreen>根据 “只多不少” 原则，所有的对象结构都可以赋值给这个类型</font>；<font color=red>如果一个接口类型包含了所有可能的属性，那可以和这个类型匹配的对象，必然要包含所有这些可能的属性，不能少一个，无法多一个</font>。可以看到，这两个极限条件下对应的集合，前者包含了后者，说明在可比的情况下，属性越少，集合越大，元素越多。

摘自：[从集合论的角度理解 TypeScript # 理解结构式类型匹配](https://juejin.cn/post/7264549320362934309#heading-7)



#### unknown vs never
##### TypeScript 中的 top type、bottom type
<font color=dodgerBlue>**在类型系统设计中，有两种特别的类型**</font>：

- Top type：被称为通用父类型，也就是能够包含所有值的类型。

- Bottom type：代表没有值的类型，它也被称为**零**或**空**类型，<font color=red>是所有类型的子类型</font>。

  > 👀 考虑到 TS 是鸭子类型，类似于空集的空类型是所有类型的子类型是很合理的

按照类型系统的解释，<font color=dodgerBlue>在 TypeScript 3.0 中，有两个 top type</font>（any 和 unknown）和<font color=dodgerBlue>一个 bottom type</font> ( never )

##### unknown

参考 [[#TS 类型结构关系图]] ：unknown 是所有类型的“通用父类型” ( Top type )。

> 💡 补充
>
> > 下表给出了 [TS官方文档](https://www.typescriptlang.org/docs/handbook/type-compatibility.html#any-unknown-object-void-undefined-null-and-never-assignability) 中给出的常见类型的赋值特性（assignability）。
> >
> > |           | any  | unknown | object | void | undefined | null | never |
> > | --------- | ---- | ------- | ------ | ---- | --------- | ---- | ----- |
> > | any       |      | ✓       | ✓      | ✓    | ✓         | ✓    | ✕     |
> > | unknown   | ✓    |         | ✕      | ✕    | ✕         | ✕    | ✕     |
> > | object    | ✓    | ✓       |        | ✕    | ✕         | ✕    | ✕     |
> > | void      | ✓    | ✓       | ✕      |      | ✕         | ✕    | ✕     |
> > | undefined | ✓    | ✓       | ✓      | ✓    |           | ✓    | ✕     |
> > | null      | ✓    | ✓       | ✓      | ✓    | ✓         |      | ✕     |
> > | never     | ✓    | ✓       | ✓      | ✓    | ✓         | ✓    |       |
> >
> > 从上面这张表中，我们还可以看出 `unknown` 类型可以被所有类型赋值，但是除了 `any` 外，不能被任何类型赋值，所以 <font color=red>**`unknown` 类型是一个全集**</font>，与数学上的所有自然数集合 `N` 类似。
> >
> > 摘自：[从集合论的角度理解 TypeScript # 理解`never`和`unknown`](https://juejin.cn/post/7264549320362934309#heading-4)

<font color=fuchsia>unknown 是安全版本的 any 类型</font>。unknown 调用特定类型的方法时，代码会飘红（因为不确定）；而 any 不会飘红（使用 any ，意味着放弃类型检查了，因为它不是用来描述具体类型的）。如下示例：

```ts
function format1(value: any) {
  value.toFixed(2); // 不飘红，想干什么干什么，very dangerous
}

function format2(value: unknown) {
  value.toFixed(2); // 代码会飘红，阻止你这么做

  // 你需要收窄类型范围，例如：

  // 1、类型断言 —— 不飘红，但执行时可能错误
  (value as Number).toFixed(2);

  // 2、类型守卫 —— 不飘红，且确保正常执行
  if (typeof value === 'number') {
     // 推断出类型: number
     value.toFixed(2);
  }

  // 3、类型断言函数，抛出错误 —— 不飘红，且确保正常执行
  assert IsNumber(value);
  value.toFixed(2);
}


/** 类型断言函数，抛出错误 */
function assertIsNumber(arg: unknown): asserts arg is Number {
  if (!(arg instanceof Number)) {
     throw new TypeError('Not a Number: ' + arg);
  }
}
```

unknown 结合类型守卫等方式，可以确保上游数据结构不确定时，也能让代码正常执行。

##### never

`never` 类型表示的是空类型，也就是值永不存在的类型。

###### 值会永不存在的两种情况

1. 如果一个<font color=red>函数执行时抛出了 **异常**</font>，那么这个函数永远不存在返回值（因为抛出异常会直接中断程序运行，这使得程序运行不到返回值那一步，即具有不可达的终点，也就永不存在返回了）；
2. 函数中<font color=red>执行无限循环的代码（**死循环**）</font>，使得程序永远无法运行到函数返回值那一步，永不存在返回。

```ts
// 异常
function err(msg: string): never { // OK
  throw new Error(msg); 
}

// 死循环
function loopForever(): never { // OK
  while (true) {};
}
```

###### never 是唯一的 bottom type

参考 [[#TS 类型结构关系图]] ：never 是（ ts 中唯一一个）所有类型的“子类型” ( bottom type )，它能够表示任何类型的子类型，所以能够赋值给任何类型：

```ts
let err: never;
let num: number = 4;

num = err; // OK
```

可以用集合来理解 never：unknown 是全集，never 是最小单元（空集），任意类型都包含了 never。

<img src="https://s2.loli.net/2023/03/20/cDGnsEYBWivauOq.png" style="zoom:50%;" />

###### null/undefined 和 never

<font color=dodgerBlue>null 和 undefined 好像也可以表示任何类型的子类型，为什么不是 bottom type？</font>**never 特殊就特殊在**：除了自身以外，没有任何类型是它的子类型，或者说可以赋值给它。而 null 和 undefined，除了自身，还有 never 可以赋值给它

```ts
// null 和 undefined，可以被 never 赋值
declare const n: never;

let a: null = n; // 正确
let b: undefined = n; // 正确

// never 是 bottom type，除了自己以外没有任何类型可以赋值给它
let ne: never;

ne = null; // 错误
ne = undefined; // 错误

declare const an: any;
ne = an; // 错误，any 也不可以

declare const nev: never;
ne = nev; // 正确，只有 never 可以赋值给 never
```

###### never 的使用场景

- Unreachable code 检查：标记不可达代码，获得编译提示。
- 类型运算：作为类型运算中的最小因子。
- Exhaustive Check：为复合类型创造编译提示。
- ...

关于 never 的用途，知乎上有个很好的讨论 [TypeScript中的never类型具体有什么用？](https://www.zhihu.com/question/354601204)。never 这个东西很奇妙，从集合论的角度，它是一个空集合，因此它可以通过空集合的一些特性，为我们的类型运算工作带来很大便利。接下来来具体讲讲各个使用场景

###### Unreachable code 检查

如下代码：

```ts
process.exit(0);
console.log("hello world") // Unreachable code detected.ts(7027)
```

如果你使用了 ts，它会给你一个编译器提示：`Error: Unreachable code detected.ts(7027)`

因为 <font color=LightSeaGreen>`process.exit()` 返回类型被定义为了 never，在它之后的自然就是「unreachable code」了</font>。

<font color=dodgerBlue>类似的还有：</font>

```ts
function listen(): never {
  while(true){
    let conn = server.accept();
  }
}

listen();
console.log("!!!"); // Unreachable code detected.ts(7027)
```

通常来说，我们手动标记函数返回值为 never 类型，来帮助编译器识别「unreachable code」，并帮助我们收窄 ( narrow ) 类型。下面是一个没标记的例子：

```ts
function throwError() {
  throw new Error();
}

function firstChar(msg: string | undefined) {
  if (msg === undefined)
    throwError();
  let chr = msg.charAt(1) // Object is possibly 'undefined'.
}
```

由于编译器不知道 throwError 是一个无返回的函数，所以 `throwError()` 之后的代码被认为在任意情况下都是可达的，让编译器误会 msg 的类型是 `string | undefined` 。

<font color=red>**这时候如果标记上了 never 类型，那么 msg 的类型将会在空检查之后收窄为 string**</font>：

```diff
- function throwError()
+ function throwError(): never {
    throw new Error();
  }

  function firstChar(msg: string | undefined) {
    if (msg === undefined)
      throwError();
    let chr = msg.charAt(1) // ✅
  }
```

##### 类型运算

###### 最小因子

never 可以理解为一个空集，那么它将满足下面的运算规则：

```ts
T | never => T
T & never => never
```

如下示例：

<img src="https://s2.loli.net/2023/03/20/gfmTGApvdJ9eVi7.png" style="zoom:55%;" />

<img src="https://s2.loli.net/2023/03/20/DgzSVkNH9WX6CZy.png" style="zoom:55%;" />

即：never 是类型运算的最小因子，这些规则帮助我们简化了一些琐碎的类型运算。如下示例：

```ts
function fetchData(userId: string): Promise<{ userName: string }> {
    return new Promise((resolve) => {
        resolve({ userName: 'lll' });
    })
}

function timeout(ms: number): Promise<never> {
    return new Promise((_, reject) => {
        setTimeout(() => reject(new Error("Timeout!")), ms)
    })
}

async function fetchNameWithTimeout(userId: string): Promise<string> {
    const data = await Promise.race([
        fetchData(userId),
        timeout(3000)
    ]);
    return data.userName;
}
```

`Promise.race` 合并的多个 `Promise`，有时是无法确切知道时序和返回结果的。这里使用一个 `Promise.race` 将一个有网络请求返回值的 `Promise` 和另一个在给定时间之内就会被 `reject` 的 `Promise` 合并起来。

timeout 函数的实现，如果超过指定时间，将会抛出一个 Error。<font color=fuchsia>由于它是无返回的，所以返回结果定义为了 `Promise<never>`</font> 。

接下来编译器会去推断 `Promise.race` 的返回值，因为 race 会取最先完成的那个 `Promise` 的结果，所以在上面这个例子里，它的函数签名类似这样：

```ts
function race<A, B>(inputs: [Promise<A>, Promise<B>]): Promise<A | B>
```

代入 `fetchData` 和 `timeout` 进来，A 则是 `{ userName : string }`，而 B 则是 `never`。因此，函数输出的 `promise` 返回值类型为 `{ userName: string } | never`。 又因为 `never` 是最小因子，可以消去。故返回值可简化为 `{ userName: string }`，这正是我们希望的。

###### 条件类型中使用

我们经常在条件类型中见到 never，它被用于表示 else 的情况。

```ts
type Arguments<T> = T extends (...args: infer A) => any ? A : never
type Return<T> = T extends (...args: any[]) => infer R ? R : never
```

在收窄联合类型时，never 也巧妙地发挥了它作为最小因子的作用。如下示例，从 `T` 中排除 `null` 和 `undefined` 

```ts
type NullOrUndefined = null | undefined
type NonNullable<T> = T extends NullOrUndefined ? never : T

// 运算过程
type NonNullable<string | null> 
  // 联合类型被分解成多个分支单独运算 👀 分布式条件类型
  => (string extends NullOrUndefined ? never : string) | (null extends NullOrUndefined ? never : null)
  // 多个分支得到结果，再次联合
  => string | never
  // never 在联合类型运算中被消解
  => string
```

##### Exhaustive Check

联合类型、代数数据类型等复合类型，可以结合 switch 语句来进行类型收窄：

```ts
interface Foo {
  type: 'foo'
}

interface Bar {
  type: 'bar'
}

type All = Foo | Bar;

function handleValue(val: All) {
  switch (val.type) {
    case'foo': // val 此时是 Foo
      break;
    case'bar': // val 此时是 Bar
      break;
    default:   // val 此时是 never
      const exhaustiveCheck: never = val;
      break;
  }
}
```

如果后面有人修改了 `All` 类型，它会发现产生了一个编译错误：

```ts
type All = Foo | Bar | Baz;

function handleValue(val: All) {
  switch (val.type) {
    case'foo': // val 此时是 Foo
      break;
    case'bar': // val 此时是 Bar
      break;
    default:   // val 此时是 Baz
      // ❌ Type 'Baz' is not assignable to type 'never'.(2322)
      const exhaustiveCheck: never = val; // 👀 另外，注意这里的写法。
      break;
  }
}
```

<font color=red>在 default branch 里面 val 会被收窄为 `Baz`，导致无法赋值给 never，产生一个编译错误</font>；开发者能够意识到 handleValue 里面需要加上针对 Baz 的处理逻辑。通过这个办法，可以确保 handleValue 总是穷尽 ( exhaust ) 了 All 所有可能的类型。

> 👀 上面这个示例来自：[TypeScript中的never类型具体有什么用？ - 尤雨溪的回答 - 知乎](https://www.zhihu.com/question/354601204/answer/888551021)。类似的， [never类型的妙用【渡一教育】](https://www.bilibili.com/video/BV1pGPCebE7H) 也讲了类似的东西
>
> 另外，看了下这个问题的更多回答，感觉这篇文章就是这个知乎问题回答整理做的笔记。

摘自：[【TypeScript】never 和 unknown 的优雅之道](https://mp.weixin.qq.com/s/rZ96wy8xUrx4T1qG5OKS0w)




#### TS 的鸭子类型

> 👀 关于鸭子类型的介绍，详见 [[CS知识杂谈#鸭子类型]]

<font color=fuchsia>TypeScript 的类型系统是 “结构类型系统”</font> ( Structural type system )，任两个以相同结构所描述的值的类型都是等价的。<font color=dodgerBlue>与之相反的是</font>，<font color=fuchsia>“标明类型系统” Nominative type system：表示类型若要相等，就必须具有相同的“名字”</font>。

全集 any，表示范围最大的集合。

空集 never，表示范围最小的集合。**此时集合中没有任何值。**

摘自：[TypeScript类型系统（5/5）-类型编程篇 - Ethan Ruan的文章 - 知乎](https://zhuanlan.zhihu.com/p/145679056)

> 💡 **关于“类型系统” 的“集合”与“鸭子类型”**
> 
> 类型其实表达的是一种**集合**。集合是数学上的概念，通俗来说，表达的就是一个范围。我们在学习之前，一定要用集合的概念去理解所有的类型。也就是说，实际上我们学 TS，就是一个探讨**集合范围大小**的问题。
>
> 例如：**any 表示最大集合。** 他可以是任意类型，你怎么用他都不会出错。
> 
> 当我们知道类型是一种集合之后，那么，我们就可以很自然的衍生出一些非常常见的概念，例如：**全集、子集、空集、交集、并集、补集**等。
> 
> 摘自：[从 never 切入，摸透 TypeScript 的学习思路](https://mp.weixin.qq.com/s/mEAhlkAQtff28Jd_yWkWuw)

> 👀 看了下 [关于 TS ，你必须知道的鸭子?类型](https://jishuin.proginn.com/p/763bfbd6f46f) 其中结合 Py 和 JS 再引入 TS 的鸭子类型，（以我当前的水平）没完全看懂... 有空再看下。另外，还提到了 TS 的 `is` 关键字及 类型谓词 ”type predicates“  ，还有“类型保护”



#### `is` 关键字

>  👀 可以参考下：[[#使用定义的类型保护]] 中 GPT 给出的回复，很好的说明了 `is` 以及类型谓词 type predicate 所起到的作用

// TODO 参见 [TS handbook V2 - narrowing # Using type predicates](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates) 以及 [StackOverflow - What does the `is` keyword do in typescript?](https://stackoverflow.com/questions/40081332/what-does-the-is-keyword-do-in-typescript)





#### Tripe-slash Directives

在实际应用开发时有一种场景，当前作用域下可以访问某个变量，但这个变量并不由开发者控制。例如通过 Script 标签直接引入的第三方库 CDN、一些宿主环境的 API 等。这个时候可以利用 TS 的环境声明功能，来告诉 TS 当前作用域可以访问这些变量，以获得类型提醒。

具体有两种方式，declare 和三斜线指令。

###### 声明文件

略

###### 三斜线指令

用三斜线指令可以一次性引入整个类型声明文件。

```ts
/// <reference path="../typings/monaco.d.ts" />
const range = new monaco.Range(2, 3, 6, 7);
```

摘自：[TypeScript 中高级应用与最佳实践](http://www.alloyteam.com/2019/07/13796/)

// TODO https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html

// TODO 《Programming Typescript》Appendix E. Triple-Slash Directives



#### 声明文件 d.ts

TS 目前支持三种文件格式：`.ts` `.tsx` 和 `.d.ts`，分别是普通的 ts 文件，主要用于 React 支持的 tsx 文件和 声明 ( declaration ) 文件。<font color=fuchsia>d.ts 文件会参与编译，但不会输出任何代码</font>，<font color=red>它的主要作用是为其他 JS 文件提供类型声明</font>。 比较像 C C++ 的 头文件。

##### ts 内置 d.ts

默认情况下我们每个 TS 项目都包含了至少一个 d.ts 文件，就是 `lib.d.ts`。比如如下代码：

```ts
const arr = [1, 2, 3]
arr.forEach(e => console.log(e))
```
在 `forEach` 上右击，选择“转到定义”

<img src="https://s2.loli.net/2023/03/14/ZIwYxigKEz7djJo.png" alt="image-20230314121830275" style="zoom:50%;" />

会打开 `lib.d.ts` 文件，其中包含：

```ts
interface Array<T> {
    /**
     * Performs the specified action for each element in an array.
     * @param callbackfn  A function that accepts up to three arguments. forEach calls the callbackfn function one time for each element in the array.
     * @param thisArg  An object to which the this keyword can refer in the callbackfn function. If thisArg is omitted, undefined is used as the this value.
     */
    forEach(callbackfn: (value: T, index: number, array: T[]) => void, thisArg?: any): void;
}
```

`lib.d.ts` 中定义了 `Array` 类型拥有的属性和方法。

<font color=dodgerBlue>**`lib.d.ts` 是被 TSC 默认包含的**</font>，它<font color=red>里面定义了 JavaScript 的公共库</font>，如 Array、Math、RegExp 等。 除了 JS 公共库，它里面<font color=red>还包含了 HTML DOM 的类型定义</font>，比如 `window`、`document`、`DOMParser`等等。这样的默认设定 可以满足大部分 TS 开发的需求。

##### 指定项目需要的公共库

如果开发一个 NodeJS 应用，不希望有 DOM API，因为运行时肯定会报错。TSC 有对应的解决方案：<font color=red>可以在 `tsconfig.json` 中通过 `lib` 指定需要包含的 libs</font>。<font color=fuchsia>没有指定时，它的值相当于 `target` 语言版本 + `dom`</font>。

```json
{
    "compilerOptions": {
        "target": "es5",
        "module": "es2015",
        "lib": ["es5", "dom"]
    }
}
```

可以通过指定 `lib` 为 `es5` 或者 `es2015` 表示 NodeJS 这种没有 DOM 的开发环境。<font color=dodgerBlue>**`lib` 的可选值非常多**</font>，TS team 针对当下的 JS 开发环境，<font color=red>对 `es2015` 的库进行了细分</font>，让开发者可以方便的使用部分 `es2015` 中的库，<font color=red>比如可以通过添加
`es2015.promise` 来增加 `Promise` 支持</font>。

##### 第三方库的 d.ts

知道了 JS 公共库的定义方式，其他第三方库也是一样的道理。<font color=LightSeaGreen>社区中已经有大量常用库的 d.ts 文件，可以根据需要下载使用</font>；但社区中的 d.ts 五花八门，良莠不齐。为了更好的让他们为我所用，我们还是 <font color=dodgerBlue>**需要了解一些编写 d.ts 的技巧，这样遇到不符合我们 需求的 d.ts 文件，我们可以对他们稍加修改**</font>。<font color=LightSeaGreen>对于没那么热门的库，自己编写 d.ts 就尤为重要了</font>。

##### 为 jQuery 编写 d.ts

<font color=dodgerBlue>**jQuery 的 `$()` 函数主要有两个用法**</font>：
1. 在 `$()` 中添加回调函数，在 DOM Ready 时执行该回调，如下：
   ```js
   $(() => console.log('DOM Ready'))
   ```
2. 在 `$()` 中添加 css selector ，返回 jQuery 封装的对象，并进行操作。如下：
    ```js
    var button = $('#id')
    var color = button.css('color')
    button.css('height', '20px')
               .click(e => console.log('click'))
    ```

编写 `jQuery.d.ts` 中的 `$()` 显然需要使用“重载”：
```json
declare function $(callback: Function): void;
declare function $(selector: string): any;
```
这里重复声明了 `$`，TSC 会根据 `$` 的参数类型自动匹配函数的返回值。

不过，第二种情况还不够准确，毕竟函数签名中返回的是 any，而事实上返回的对象( jQueryElement )，还包含 `css`、`click` 等方法；any 显然描述不够准确，这就需要给 jQueryElement 定义一个 interface。

##### interface 定义

```ts
interface JQueryElement {
    click(Function): JQueryElement;

    css(name: string): string;
    css(name: string, value: any): JQueryElement;
}

declare function $(selector: string): JQueryElement;
```
<font color=red>**针对可以链式访问的方法返回类型同样设置成 `JQueryElement`**</font> ，另外 `css` 这个方法也使用了重载。

##### 复杂类型的变量声明
jQuery 远没有这么简单，比如使用频度跟 Selector 差不多的还有它对 XHR 的封装。
```js
$.get("http://domain.com/data", data => { });
$.get("http://domain.com/data", { param: 1 }, data => { });
```
<font color=dodgerBlue>此时 `$` 其实已经不是一个简单的 Function 了，它同时还是一个包含属性的 Object</font>。于是需要给它单独定义一个 interface 来满足它的各种特性。先把刚刚写的 function 形式转化成 interface 形式：
```js
interface JQueryVariable {
    (callback: Function): void;
    (selector: string): JQueryElement;
}

declare var $: JQueryVariable;
```
可以简单的通过定义一个没有名字的方法来告诉 TSC，这个类型的变量是一个 Function。 然后我们声明一个类型是 `JQueryVariable` 的变量 `$`。

此时，为 `$` 添加 `get` 方法就很容易了，完成后的效果如下

```ts
interface JQueryVariable {
    (callback: Function): void;
    (selector: string): JQueryElement;

    get(url: string, success: Function);
    get(url: string, data: any, success: Function);
}
```

上面 `d.ts` 中所有的回调函数都用使用了 Function 类型，但在实际项目中，应该尽量避免；它起不到任何约束或者智能提示的作用。
> 👀 这里的意思是说：不推荐使用 Function 类型，而是使用 Function 的签名，以描述清楚 ？

> 💡补充：感觉上面的写法有点麻烦 / 过度抽象，其实没有绝对必要将其封装到一个 interface 中。这里摘抄一下 [如何编写一个d.ts文件](https://segmentfault.com/a/1190000009247663) 中的内容
>
> > ###### 混合类型
> >
> > 有时候有些值既是函数又是class又是对象的复杂对象。比如常用的 jQuery 有各种用法：
> >
> > ```javascript
> > new $()
> > $.ajax()
> > $()
> > ```
> >
> > 既是函数又是对象
> >
> > ```typescript
> > declare function $2(s:string): void
> > 
> > declare namespace $2{
> >     let aaa: number
> > }
> > ```
> >
> > **效果**
> >
> > 作为函数用：
> >
> > ![](https://s2.loli.net/2023/03/16/zrtlKkR9XFaqmiB.png)
> >
> > 作为对象用：
> >
> > ![](https://s2.loli.net/2023/03/16/JOwZhNv1P3KMmiu.png)
> >
> > 也就是说：TS 会自动把同名的 namespace 和 function 合并到一起。
> >
> > > 💡 本文下面还说：ts3.6 增加了新功能，function 声明和 class 声明可以合并了；所以，可以声明同名的 function、class 还有 namespace。

##### 总结
在 TS 中，函数的类型一般有下面几种表示方式：

```ts
// 直接声明
declare function name(): void;

// interface 形式
interface Callback {
    (): void;
}
declare var name: Callback;
```

还有最常用的：lambda 表达式方式
```ts
declare var name: () => void;
```

给回调函数指定类型时，最常用的是 lambda 表达式方式，毕竟写起来比较简单。但是，<font color=red>当一个回掉函数类型需要被重用， 或者回掉函数需要比较丰富的注释时，还是更加推荐 interface 的写法</font>。

摘自：[TypeScript 进阶：给第三方库编写声明文件](http://imzc.me/dev/2016/11/30/write-d-ts-files/)

##### 全局变量 与 模块化的全局变量

在 TypeScript 规则中，<font color=dodgerBlue>如果一个 `.ts`、`.d.ts` 文件如果没有用到 import 或者 export 语法的话</font>，<font color=fuchsia>**那么最顶层声明的变量就是全局变量**</font>。

###### 模块化的全局变量

如果定义全局变量的时候需要引入（👀 使用 import ）第三方文件，比如：

```ts
import { Request, Response } from 'express'

declare var req: Request
declare var res: Response
```

由于上面的 `d.ts` 文件使用了 import / export 语法，（按照上面的规则）ts 编译器不会将 `req` 和 `res` 当作全局变量，那么就需要通过以下的方式声明全局变量：

```ts
import { Request, Response } from 'express'

declare glob {
  var req: Request
  var res: Response
  
  // 后面还可以跟 namespace 等声明
}
```

##### 模块化导出

###### CommonJS 的导出

如果想要使用 commonJS 的方式引入模块化的代码，比如：

```ts
const content = require('cjs_module')
```

对应的模块，需要这样声明：

```ts
declare module 'cjs_module' {
  export let foo: number
  export function bar(): number
  export namespace quz { ... }
}
```

基本上就是在 export 外层套了一个 `declare module "module_name"` 。另外，甚至可以先定义再导出：

```ts
declare module 'cjs_module' {
  let foo: number
  export = foo
}
```

###### ESM 的导出

```ts
declare var foo: 'foo'
declare var bar: 'bar'

export { foo, bar }
```

摘自：[如何编写一个d.ts文件](https://segmentfault.com/a/1190000009247663)



#### tsconfig.json

> 💡 ts 官方有 tsconfig.json 各种配置选项的介绍，见：[ts doc - Intro to the TSConfig Reference](https://www.typescriptlang.org/tsconfig)



##### `files`、`include` 和 `exclude`
files、include 和 exclude 都是用于指定（设置）需要被编译的文件的：其中 files 是直接指定文件路径，include 和 exclude 是一对，用匹配模式 包含和排除一些文件。

> 💡 以下是 new bing 的回答：

<img src="https://s2.loli.net/2023/03/14/jEohQ2uMGLg8Bpv.png" alt="image-20230314223540978" style="zoom: 40%;" />

如果 files 和 exclude 设置了同时一个文件，则以 files 为准。

> 👀 显然这也是解释得通的：iles 是直接指定，exclude 是匹配，直接指定更具体，所以优先级更高。

> 💡 以下是 new bing 的解答：

<img src="https://s2.loli.net/2023/03/14/1HutaoNlv8PzJQA.png" alt="image-20230314223200401" style="zoom:45%;" />

##### `paths`

和 [[Vue2 学习笔记#在 `vue.config.js` 中实现 `resolve.alias`]] 中的内容没太大区别，这里不做赘述；另外，可以看下 [Say Goodbye to ‘../../../..’ in your TypeScript Imports](https://decembersoft.com/posts/say-goodbye-to-relative-paths-in-typescript-imports/)



##### `skipLibCheck`

> Skip Lib Check

Skip type checking of declaration files.

**This can save time during compilation at the expense of type-system accuracy**. <font color=dodgerBlue>For example</font>, <font color=lightSeaGreen>two libraries could **define two copies of the same `type`** in an inconsistent way</font>. <font color=dodgerBlue>Rather than doing a full check of all `d.ts` files</font>, TypeScript will type check the code you specifically refer to in your app’s source code.

<font color=dodgerBlue>A common case where you might think</font> to use `skipLibCheck` is when there are two copies of a library’s types in your `node_modules`. In these cases, you should consider using a feature like [yarn’s resolutions](https://yarnpkg.com/lang/en/docs/selective-version-resolutions/) to ensure there is only one copy of that dependency in your tree or investigate how to ensure there is only one copy by understanding the dependency resolution to fix the issue without additional tooling.

<font color=dodgerBlue>Another possibility is</font> when you are <font color=lightSeaGreen>migrating between TypeScript releases</font> and **the changes cause breakages in node_modules and the JS standard libraries** which you do not want to deal with during the TypeScript update.

Note, that if these issues come from the TypeScript standard library you can replace the library using [TypeScript 4.5’s lib replacement](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-5.html#supporting-lib-from-node_modules) technique.

摘自：[ts doc - tsconfig # skipLibCheck](https://www.typescriptlang.org/tsconfig/#skipLibCheck)

###### 其他补充

> 在大多数项目中，<font color=lightSeaGreen>开发者会在 TypeScript 的配置文件 ( `tsconfig.json` ) 中将 `skipLibCheck` 选项设置为 `true`</font> 。`skipLibCheck` 的作用是跳过对库文件（包括 `.d.ts` 文件）的类型检查。当设置为 true 时，<font color=lightSeaGreen>TypeScript 编译器不会对这些库文件进行严格的类型检查，从而加快编译速度</font>。但<font color=red>这也会影响项目中自己编写的 `.d.ts` 文件</font>。<font color=dodgerBlue>这意味着</font>，<font color=red>即使 `.d.ts` 文件中定义的类型存在错误，TypeScript 编译器也不会报错，从而失去了类型安全性的保障</font>。



##### `lib`

TypeScript <font color=red>**includes a default set of type definitions for built-in JS APIs**</font> (like `Math`), as well as type definitions for things found in browser environments (like `document`). <font color=lightSeaGreen>TypeScript also includes APIs for newer JS features matching the [`target`](https://www.typescriptlang.org/tsconfig/#target) you specify</font>; for example the definition for `Map` is available if [`target`](https://www.typescriptlang.org/tsconfig/#target) is `ES6` or newer.

<font color=dodgerBlue>You may want to change these for a few reasons:</font>

- Your program doesn’t run in a browser, so you don’t want the `"dom"` type definitions
- Your runtime platform provides certain JavaScript API objects (maybe through polyfills), but doesn’t yet support the full syntax of a given ECMAScript version
- <font color=red>You have polyfills or native implementations for some, *but not all*, of a higher level ECMAScript version</font>

In TypeScript 4.5, lib files can be overridden by npm modules, find out more [in the blog](https://devblogs.microsoft.com/typescript/announcing-typescript-4-5-beta/#supporting-lib-from-node_modules).

##### High Level libraries

| Name         | Contents                                                     |
| :----------- | :----------------------------------------------------------- |
| `ES5`        | Core definitions for all ES5 functionality                   |
| `ES2015`     | Additional APIs available in ES2015 (also known as ES6) - `array.find`, `Promise`, `Proxy`, `Symbol`, `Map`, `Set`, `Reflect`, etc. |
| `ES6`        | Alias for “ES2015”                                           |
| `ES2016`     | Additional APIs available in ES2016 - `array.include`, etc.  |
| `ES7`        | Alias for “ES2016”                                           |
| `ES2017`     | Additional APIs available in ES2017 - `Object.entries`, `Object.values`, `Atomics`, `SharedArrayBuffer`, `date.formatToParts`, typed arrays, etc. |
| `ES2018`     | Additional APIs available in ES2018 - `async` iterables, `promise.finally`, `Intl.PluralRules`, `regexp.groups`, etc. |
| `ES2019`     | Additional APIs available in ES2019 - `array.flat`, `array.flatMap`, `Object.fromEntries`, `string.trimStart`, `string.trimEnd`, etc. |
| `ES2020`     | Additional APIs available in ES2020 - `string.matchAll`, etc. |
| `ES2021`     | Additional APIs available in ES2021 - `promise.any`, `string.replaceAll` etc. |
| `ES2022`     | Additional APIs available in ES2022 - `array.at`, `RegExp.hasIndices`, etc. |
| `ES2023`     | Additional APIs available in ES2023 - `array.with`, `array.findLast`, `array.findLastIndex`, `array.toSorted`, `array.toReversed`, etc. |
| `ESNext`     | Additional APIs available in ESNext - <font color=red>This changes as the JavaScript specification evolves</font> |
| `DOM`        | [DOM](https://developer.mozilla.org/docs/Glossary/DOM) definitions - `window`, `document`, etc. |
| `WebWorker`  | APIs available in [WebWorker](https://developer.mozilla.org/docs/Web/API/Web_Workers_API/Using_web_workers) contexts |
| `ScriptHost` | APIs for the [Windows Script Hosting System](https://wikipedia.org/wiki/Windows_Script_Host) |

##### `target`

Modern browsers support all ES6 features, so `ES6` is a good choice. You might choose to set a lower target if your code is deployed to older environments, or a higher target if your code is guaranteed to run in newer environments.

The `target` setting changes which JS features are downleveled and which are left intact. For example, an arrow function `() => this` will be turned into an equivalent `function` expression if `target` is ES5 or lower.

<font color=red>**Changing `target` also changes the default value of `lib`**</font>. You may “mix and match” `target` and `lib` settings as desired, but you could just set `target` for convenience.

For developer platforms like Node there are baselines for the `target`, depending on the type of platform and its version. You can find a set of community organized TSConfigs at [tsconfig/bases](https://github.com/tsconfig/bases#centralized-recommendations-for-tsconfig-bases), which has configurations for common platforms and their versions.

<font color=red>The special `ESNext` value refers to the highest version your version of TypeScript supports</font>. <font color=red>**This setting should be used with caution**</font>, since <font color=lightSeaGreen>it doesn’t mean the same thing between different TypeScript versions and can make upgrades less predictable</font>.

> 💡如上面所说：修改 `target` 将会修改 `lib` 的默认值。
>
> 另外，我也有点好奇 `lib` 的默认值是什么，于是问了下 Gemini ，得到如下结果：
>
> 



#### TS Decorators

##### What are TypeScript Decorators?

<font color=fuchsia>Decorators are **basically just functions at their core**.</font>

With decorators you <font color=red>can apply **re-useable** behaviors to</font> <font color=fuchsia>**classes, methods, properties**</font>. Decorators are inspired by decorators in other languages like Java and Python.

> 💡 感觉这句话有点问题：Java 似乎只有注解 annotation，没有装饰器；只有装饰器模式

##### Types of Decorators

<font color=dodgerBlue>Decorators can apply to</font>

1. Class
2. Method
3. Class Property
4. <font color=fuchsia>Accessor</font>
5. <font color=red>Method Parameter</font>

> 👀 由于 Class、Method、Class Property 的装饰器较为常见，下面省略；详见原文

###### Parameter Decorator

> 👀 这个 Spring 中有，不过没什么印象了...

Parameter decorators are used to <font color=red>**watch and change** the functional or methods parameters</font>.

let us look at the parameter decorator with an example

```typescript
function TestParameter(target: any, propertyName: string, CoolParameter: number) {
  console.log(`Cool Parameter: ${CoolParameter}`);
}

class SomeClass {
  exampleMethod(@TestParameter saySomething: string) {
    console.log(`from exampleMethod: ${saySomething}`);
  }
}

const instance = new SomeClass();
instance.exampleMethod('Hello');
```

Here we have a function `TestParameter` that `console.logs` the `CoolParameter` parameter

and then we have `SomeClass` with a method named `exampleMethod`. When the `exampleMethod` is called the decorator is triggered and the `TestParameter` function is called which then logs the `CoolParameter` to the `console`

###### Accessor Decorators

Accessor decorators are <font color=dodgerBlue>like method decorators</font> but <font color=red>the only difference is the that the **Method decorators have keys in their descriptor**</font> <font color=dodgerBlue>**like**</font>

- Value
- writable
- configurable
- enumerable

<font color=dodgerBlue>**The descriptor in the Accessor decorators has keys:**</font>

- get
- set
- enumerable
- configurable

<font color=dodgerBlue>**The accessor decorators accept the follow theee parameters:**</font>

1. Constructor function or target prototype
2. property name 
3. property descriptor

##### When to use Decorators

1. Before/After Hooks
2. Watch property changes and method calls.
3. Transform parameters
4. Add extra method or properties
5. <font color=red>Runtime type validation</font>
6. Auto serialization and deserialization
7. <font color=red>Dependency Injection</font>

###### Before /After Hooks

Before / After hooks means the decorator is called before and after the function is called which <font color=red>is quite useful in debugging , logging , and measuring performance</font>

###### Watch property changes and method calls

Watch is a property decorator that logs the property as well as the method calls of a decorated method.

This is especially useful when debugging and performance monitering.

Let us learn more about Watch property and how it works using an example:

```ts
function see(target: any, propertyName: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`function has been called: ${propertyName} with args:`, args);
    return originalMethod.apply(this, args);
  };
}

class SomeClass {
  myProperty: string;

  @see
  setProperty(value: string) {
    console.log('Set the value to ');
    this.myProperty = value;
  }
}

const instance = new SomeClass();
instance.setProperty('Awesome Great work Done');
```

###### Transform Parameters

In transform parameters function <font color=red>applies the transformation function to a specific parameter **before the method is called**</font>.

Let us learn more about transform parameters with the help of an example:

```ts
function CoolFunc(SomeFunc: (arg: any) => any): ParameterDecorator {
  return (target: any, propertyKey: any, parameterIndex: any) => {
    const originalMethod = target[propertyKey];

    target[propertyKey] = function (...args: any[]) {
      args[parameterIndex] = SomeFunc(args[parameterIndex]);
      return originalMethod.apply(this, args);
    };
  };
}

class MyClass {
  myMethod(@CoolFunc((x) => x * 2) num: number) { // 👀 @CoolFunc 传入了一个函数
    console.log(`Transformed parameter: ${num}`);
  }
}

const instance = new MyClass();
instance.myMethod(5);
```

###### Add extra method or property

You can add an extra method or property using the decorators. Here is how you can achieve this using an example:

```ts
function AddMethod() {
  return function (constructor: Function) {
    constructor.prototype.AddMethod = function () {
      console.log('Added a new method the property by the decorator');
    };
  };
}

@AddMethod()
class SomeClass {}

const instance = new SomeClass();
(instance as any).AddMethod();
```

<font color=LightSeaGreen>When an instance of `SomeClass` is called</font> the <font color=red>`addMethod` can be called on it</font>. The add method logs a message to the console

###### Runtime Type Validation

> ⚠️ 注意这里是 Runtime

Runtime type validation is <font color=red>ensuring the type of</font> *values of the parameters and variables and const and data used in the function* during run time <font color=red>matches the values of the expected type</font>.

<font color=dodgerBlue>unlike static type validation</font> which happens during software compilation the run time type validation happens when the function or programing is running <font color=red>thus it is helpful in validating external input data</font>

Let us learn more about Runtime Type Validation using an example:

```ts
function SomeFunc(target: any, propertyKey: string, parameterIndex: number) {
  const originalMethod = target[propertyKey];

  target[propertyKey] = function (...args: any[]) {
    if (typeof args[parameterIndex] !== 'number') {
      throw new Error(`An error has occoured ${parameterIndex}`);
    }
    return originalMethod.apply(this, args);
  };
}

class TestClass {
  calculateArea(@SomeFunc radius: number) {
    return Math.PI * radius * radius;
  }
}

const instance = new TestClass();

try {
  instance.calculateArea('this is NaN'); 
} catch (error) {
  console.error(error.message);
}

console.log(instance.calculateArea(6)); 
```

###### Auto serialization and deserialization

Auto serialization and deserialization refers to the practice of <font color=red>converting from one data type to another data type deserialization</font> in order to store the data in a particular format 

<font color=LightSeaGreen>then converting again from one data type to the orignal data type</font> for the purpose of rendering the data or other function can consume the data

Here is an example explaining how decorators can be used to do this

```typescript
function SerializeFunc(constructor: Function) {
  constructor.prototype.serialize = function () {
    return JSON.stringify(this);
  };

  constructor.deserialize = function (json: string) {
    return new constructor(JSON.parse(json));
  };
}

function DeserializeFunc(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (serializedData: string) {
    const deserializedData = JSON.parse(serializedData);
    return originalMethod.apply(this, deserializedData);
  };
}

@SerializeFunc
class Person {
  constructor(public name: string, public age: number) {}

  @SerializeFunc
  greet(data: { name: string; age: number }) {
    return `Awesome, people call me ${data.name} my age is ${data.age} years old.`;
  }
}

const man = new Man('Jim Class', 43);
const serializedMan = man.serialize();

console.log(serializedMan); // {"name":"Jim Class","age":43}

const deserializedMan = (Man as any).deserialize(serializedMan);
console.log(deserializedMan.greet()); 

const serializedData = '{"name":"Don markell","age":56}';
console.log(man.greet(serializedData));
```

###### Dependency Injection

Decorators can also be used for dependency injection. Dependency injection is the practice of <font color=red>providing the functions and Objects that a method depends upon to it as parameter</font>.

this <font color=red>helps in decoupling the code</font> and <font color=red>makes it cleaner</font>.

Decorators helps in dependency injection by providing a way to inject dependencies directly into class constructors <font color=red>without changing class implementation</font>.

This making them easier to maintain and test.

##### Advantages of using decorators

###### Cross Cutting concerns

<font color=dodgerBlue>When parts of a program relay on many other parts of the program</font>. Like a program might relay on other classes methods or properties then this could be a use case for decorators and they provide an easy way to maintain the code and provide improved readability

###### Dependency Injection

Dependency injection is a software design pattern where an object or a function that relies on other objects or functions receives then 

This is like an inversion of control and results in loosely coupled program and a code base that is modular

###### Validation

Decorators can be used to validate inputs or the state of an Object. 

###### Code organization

The <font color=LightSeaGreen>code that doesn't belong in the main function or logic can be encapsulated using decorators</font> thus <font color=red>making the code more readable and maintainable</font>

摘自：[TypeScript Decorators: A complete guide](https://deadsimplechat.com/blog/typescript-decorators-a-complete-guide/#7-dependency-injection)



#### json schema 和 TypeScript

<img src="https://s2.loli.net/2023/05/30/gJHwcuLI5B3l4S1.png" alt="image-20230530225419605" style="zoom:46%;" />
