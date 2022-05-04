# TypeScript 学习笔记



### 《TypeScript入门教程》学习笔记

链接🔗：[《TypeScript入门教程》](https://ts.xcatliu.com)

#### TypeScript是静态语言

类型系统<font color=FF0000> 按照「类型检查的时机」来分类，可以分为动态类型和静态类型</font>。

- **动态类型** 是指在<font color=FF0000> 运行时才会进行类型检查</font>，这种语言的类型错误往往会导致运行时错误。
- **静态类型** 是指<font color=FF0000> 编译阶段就能确定每个变量的类型</font>，这种语言的类型错误往往会导致语法错误。

<mark>JavaScript 是一门解释型语言，没有编译阶段，所以它是动态类型。</mark>

<mark style=background-color:hotpink>TypeScript 在运行前需要先编译为 JavaScript，而在编译阶段就会进行类型检查，所以 TypeScript 是静态类型</mark>

#### TypeScript 是弱类型

类型系统按照「是否允许隐式类型转换」来分类，可以分为强类型和弱类型

TypeScript 是完全兼容 JavaScript 的，它不会修改 JavaScript 运行时的特性，所以它们都是弱类型。

**补充：**

![20191031230816827](https://i.loli.net/2021/08/31/mvDCBdjIHUfTO8V.png)



#### TS的类型和编译

在 TypeScript 中，我们使用 : 指定变量的类型，: 的前后有没有空格都可以。

**TypeScript 只会在编译时对类型进行静态检查，如果发现有错误，编译的时候就会报错**。

TypeScript 编译的时候即使报错了，还是会生成编译结果，我们仍然可以使用这个编译之后的文件。

如果要在报错的时候终止 js 文件的生成，可以在 tsconfig.json 中配置 noEmitOnError 即可



#### 空值

JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 void 表示没有任何返回值的函数：

```ts
function alertName(): void {
    alert('My name is Tom');
}
```

声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null（只在 --strictNullChecks 未指定时）：

```ts
let unusable: void = undefined;
```



#### Null 和 Undefined

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



#### 任意值

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

联合类型（Union Types）表示<font color=FF0000> 取值可以为多种类型中的一种</font>。

**示例：**

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

联合类型使用 | 分隔每个类型。

这里的` let myFavoriteNumber: string | number `的含义是，<font color=FF0000> 允许 myFavoriteNumber 的类型是 string 或者 number，但是不能是其他类型</font>。

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

**什么是接口**

在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

接口一般首字母大写。

**示例：**

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

上面的例子中，我们定义了一个接口 Person，接着定义了一个变量 tom，它的类型是 Person。<font color=FF0000> 这样，我们就约束了 tom 的<font size=4>**形状必须和接口 Person 一致**</font></font>。定义的变量比接口少了一些属性是不允许的，多一些属性也是不允许的。

**可选属性**

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

**任意属性**

有时候我们<font color=FF0000><font size=4> **希望一个接口允许有任意的属性，可以使用如下方式**</font></font>：

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

<font color=FF0000> 使用 [propName: string] 定义了任意属性取 string 类型的值</font>。

需要注意的是，<font color=FF0000 size=4> **一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**：</font>

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

另外，在报错信息中可以看出，此时 `{ name: 'Tom', age: 25, gender: 'male' }` 的类型被推断成了 `{ [x: string]: string | number; name: string; age: number; gender: string; }`，这是联合类型和接口的结合。

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

<font color=FF0000> **自我补充：**</font>类似的属性在对象中可以定义多个。不是一个接口中的可选属性，在对象中只能定义一次；而是可以定义成多个

**只读属性**

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

**「类型 + 方括号」表示法**

<font color=FF0000> 最简单的方法是使用「类型 + 方括号」来表示数组</font>：

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

数组的项中**不允许**出现其他的类型：

```ts
let fibonacci: number[] = [1, '1', 2, 3, 5];
// Type 'string' is not assignable to type 'number'.
```

**数组泛型**

我们也<font color=FF0000> 可以使用数组泛型（Array Generic） Array\<elemType> 来表示数组</font>

```ts
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

**用接口表示数组**

<font color=FF0000> 接口也可以用来描述数组</font>：

```ts
interface NumberArray {
	[index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

**类数组**

类数组（Array-like Object）不是数组类型

**any 在数组中的应用**

一个比较常见的做法是，用 any 表示数组中允许出现任意类型：

```ts
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```



#### 函数的类型

**函数声明**

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

**函数表达式**

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

在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

在 ES6 中，=> 叫做箭头函数，应用十分广泛，可以参考 [ES6 中的箭头函数](http://es6.ruanyifeng.com/#docs/function#箭头函数)

**用接口定义函数的形状**

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

**可选参数**

<font color=FF0000> 用 ? 表示可选的参数，需要注意的是，可选参数必须接在必需参数后面。换句话说，**可选参数后面不允许再出现必需参数了**</font>

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

**参数默认值**

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

ES6 中，<font color=FF0000> 可以使用 ...rest 的方式获取函数中的剩余参数（rest 参数）</font>。items 是一个数组。所以我们可以用数组的类型来定义它：

```ts
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}
let a = [];
push(a, 1, 2, 3);
```

<font color=FF0000> 注意，rest 参数只能是最后一个参数</font>

**重载**

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



### 《深入理解 TypeScript 》学习笔记

链接🔗：[深入理解 TypeScript](https://jkchao.github.io/typescript-book-chinese/)

#### infer







## 神说要有光《 TypeScript 类型体操通关秘籍》笔记

链接🔗：[TypeScript 类型体操通关秘籍](https://juejin.cn/book/7047524421182947366/section)



#### 类型是什么

类型具体点来说就是指 <font color=FF0000>number、boolean、string 等**基础类型**</font> 和 <font color=FF0000>Object、Function 等**复合类型**</font>，它们<font color=FF0000>是编程语言提供的对不同内容的抽象</font>：

- <font size=4>**不同类型变量占据的 <font color=FF0000>内存大小不同</font>**</font>：boolean 类型的变量只会分配 1 个字节的内存，而 <font color=FF0000>**number 类型的变量则会分配 8 个字节的内存**</font>（**注：**因为 JS / TS 中数字类型，不区分 int、float、double 等，所以 number 被分配了 8 个字节 ），<mark>给变量声明了不同的类型就代表了会占据不同的内存空间</mark>。
- <font size=4>**不同类型变量 <font color=FF0000>可做的操作不同</font>**</font>：number 类型可以做加减乘除等运算，boolean 就不可以；<mark>复合类型中不同类型的对象可用的方法不同</mark>，比如 Date 和 RegExp，变量的类型不同代表可以对该变量做的操作就不同。

我们  <font color=FF0000>知道了什么是类型，那自然可以想到类型和所做的操作要匹配才行，<font size=4>**这就是为什么要做类型检查**</font></font>。



#### 类型检查

<font color=FF0000>**如果能保证对某种类型只做该类型允许的操作，这就叫做 「类型安全」**</font>。比如你对 boolean 做加减乘除，这就是类型不安全，你对 Date 对象调用 exec 方法，这就是类型不安全。反之，就是类型安全。所以，<font color=FF0000>**类型检查是为了保证类型安全的**</font>。

<img src="https://s2.loli.net/2022/04/30/VkhC3MlB8J2Uxpo.png" alt="img" style="zoom:55%;" />

##### 动态类型检查 & 静态类型检查

<mark>类型检查可以 **在运行时** 做</mark>，也<mark style="background: aqua">可以 **运行之前的编译期** 做</mark>。这是两种不同的类型，<mark>前者叫做 **动态类型检查**</mark>，<mark style="background: aqua">后者叫做 **静态类型检查**</mark>。两种类型检查各有优缺点。

- **动态类型检查：**<font color=FF0000>**在源码中不保留类型信息**，对某个变量赋什么值、做什么操作都是允许的，写代码很灵活</font>。但这也埋下了类型不安全的隐患，比如对 string 做了乘除，对 Date 对象调用了 exec 方法，这些都是运行时才能检查出来的错误。

  其中，最常见的错误应该是 `null is not an object` 、`undefined is not a function` 之类的了，写代码时没发现类型不匹配，到了运行的时候才发现，就会有很多这种报错。

  <mark>所以，动态类型虽然代码写起来简单，但代码中很容易藏着一些类型不匹配的隐患</mark>。

  <img src="https://s2.loli.net/2022/04/30/nfgwztehkpbEX7C.png" alt="img" style="zoom:55%;" />

- **静态类型检查：**<font color=FF0000>**在源码中保留类型信息**，声明变量要指定类型，对变量做的操作要和类型匹配，有专门的编译器在编译期间做检查</font>。

  静态类型给写代码增加了一些难度，因为你 **除了要考虑代码要表达的逻辑之外**，<font color=FF0000>还要考虑 **类型逻辑**：变量是什么类型的、是不是匹配、要不要做类型转换等</font>。

  不过，静态类型也消除了类型不安全的隐患，因为在编译期间就做了类型检查，就不会出现对 string 做了乘除，调用了 Date 的 exec 方法这类问题。

  <mark style="background: aqua">所以，静态类型虽然代码写起来要考虑的问题多一些，会复杂一些，但是却消除了代码中潜藏类型不安全问题的可能</mark>。

  <img src="https://s2.loli.net/2022/04/30/XVKUm1fPwirLMnh.png" alt="img" style="zoom:55%;" />



#### TS 给 JS 添加的「静态类型系统」

<font color=FF0000>**TypeScript 给 JavaScript 添加了一套「静态类型系统」**，从动态类型语言变成了静态类型语言</font>，可以在编译期间做类型检查，提前发现一些类型安全问题。

<img src="https://s2.loli.net/2022/04/30/gL815ZfivRCYjut.png" alt="img" style="zoom:55%;" />

而且，因为代码中添加了静态类型，也就可以配合编辑器来实现更好的提示、重构等，这是额外的好处。

![img](https://s2.loli.net/2022/04/30/KnUlzd6SZ2OsHp5.png)



#### 类型体操 & 支持类型编程的类型系统

<font color=FF0000 size=4>**类型系统**</font> 不止 TypeScript 有，别的语言 Java、C++ 等都有，<font color=FF0000>为什么 TypeScript 的类型编程被叫做「类型体操」，而其他语言没有呢？</font>

<font color=FF0000>TypeScript 给 JavaScript 增加了一套静态类型系统，**通过 TS Compiler 编译为 JS**</font> （**注：**即 TSC ），<font color=FF0000 size=4>**编译的过程做类型检查**</font>。它并没有改变 JavaScript 的语法，只是在 JS 的基础上添加了类型语法，所以被叫做 JavaScript 的超集。

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

- **<font color=FF0000>支持泛型</font>的类型系统**：泛型的英文是 Generic Type，通用的类型；它可以代表任何一种类型，也叫做<font color=FF0000 size=4>**「类型参数」**</font>（**注：**这个说法下面有更多提到）

  它给类型系统增加了一些灵活性，在整体比较固定，部分变量的类型有变化的情况下，可以减少很多重复代码。比如上面的 add 函数，有了泛型之后就可以这样写：

  ```cpp
  T add<T>(T a, T b) {
      return a + b;
  }
  
  add(1,2);
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

  <mark>在 Java 里面，**拿到了对象的类型就能找到它的类，进一步拿到各种信息**，所以类型系统支持泛型就足够了</mark>。但 <font color=FF0000>在 JavaScript 里面，对象可以字面量的方式创建，还可以灵活的增删属性，**拿到对象并不能确定什么，所以要支持对传入的类型参数做进一步的处理**</font>。

  <font color=FF0000 size=4>**对传入的类型参数（泛型）做各种逻辑运算，产生新的类型，这就是类型编程。**</font>

  比如上面那个 getProps 的函数，类型可以这样写：

  ```ts
  function getPropValue<
      T extends object, // 注：类型参数 T 必须是对象。
      Key extends keyof T // 注：类型参数 Key 必须是 T 的成员
  >(obj: T, key: Key): T[Key] {
      return obj[key];
  }
  ```

  这里的 `keyof T`、`T[Key]` 就是对 “ 类型参数 T ” 的类型运算。

  <mark style="background: aqua">TypeScript 的类型系统就是第三种，支持对类型参数做各种逻辑处理，可以写很复杂的类型逻辑</mark>。

##### TS 的类型系统

<font color=FF0000>**TypeScript 的类型系统是<font size=4>「图灵完备」的</font>，也就是能描述各种可计算逻辑**</font>。<mark>简单点来理解就是 循环、条件 等各种 JS 里面有的语法它都有，JS 能写的逻辑它都能写</mark>。

“对类型参数的编程” 是 TypeScript 类型系统最强大的部分，可以实现各种复杂的类型计算逻辑，是它的优点。但同时也被认为是它的缺点，因为除了业务逻辑外还要写很多类型逻辑。不过，我倒是觉得这种复杂度是不可避免的，因为 JS 本身足够灵活，要准确定义类型那类型系统必然也要设计的足够灵活。

<font color=FF0000>是不是感觉 TypeScript 类型系统挺复杂的？确实，不然大家也不会把 TS 的类型编程戏称为「**类型体操**」了</font>。



#### TS 类型系统中的类型

静态类型系统的目的是把类型检查从运行时提前到编译时，那 TS 类型系统中肯定要把 JS 的运行时类型拿过来，也就是 number、boolean、string、object、bigint、symbol、undefined、null 这些类型，还有就是它们的包装类型 Number、Boolean、String、Object、Symbol。

这些很容易理解，给 JS 添加静态类型，总没有必要重新造一套基础类型吧，直接复用 JS 的基础类型就行。

复合类型方面，JS 有 class、Array，这些 TypeScript 类型系统也都支持，但是又多加了三种类型：元组 ( Tuple )、接口 ( Interface )、枚举 ( Enum ) 。

##### 元组

元组 ( Tuple ) 就是 <font color=FF0000>元素个数 和 类型 **固定的** **数组类型**</font>：

```typescript
type Tuple = [number, string];
```

**注：**初次接触「元组」这个名词是在 Py，而 TS 中「元组」概念和 Py 不一样。另外，TS 的元组的写法和 JS 中的数组一样；而 TS 对数组的定义是：数组类型是指任意多个同一类型的元素构成的（**注：**这个说法下面有说 [[#重新构造做变换#Push]] ）

##### 接口

接口 ( Interface ) 可以描述函数、对象、<font color=FF0000 size=4>**构造器**</font> 的结构

- **对象：**

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

- **函数：**

  ```typescript
  interface SayHello {
      (name: string): string;
  }
  
  const func: SayHello = (name: string) => {
      return 'hello,' + name
  }
  ```

- **<font color=FF0000>构造器</font>**（**注：**这个没见过，不过 接口函数签名前多了 `new` ）：

  ```typescript
  interface PersonConstructor {
    new (name: string, age: number): IPerson;
  }
  
  function createPerson(ctor: PersonConstructor): IPerson {
    return new ctor('guang', 18)
  }
  ```

<font color=FF0000 size=4>**对象类型、class 类型**</font>（**注：**如下面 “总之” 所说，「数组类型」也是） <font color=FF0000 size=4>**在 TypeScript 里也叫做索引类型**</font>，<font color=FF0000>**也就是索引了多个元素的类型的意思**</font>（**注：**这个概念下面会有进一步讲解）。对象可以动态添加属性，如果不知道会有什么属性，可以用 <font color=FF0000 size=4>**可索引签名**</font>（**注：**不要和前面的「索引类型」混淆。另外下面这个类似 JS「计算属性」的就是）

```typescript
interface IPerson {
    [prop: string]: string | number;
}
const obj: IPerson = {};
obj.name = 'guang';
obj.age = 18;
```

总之，<font color=FF0000 size=4>**接口可以用来描述函数、构造器、索引类型（对象、class、数组）等复合类型**</font>。

##### 枚举

枚举 ( Enum) 是一系列值的复合：

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

##### 字面量类型

此外，TypeScript 还支持字面量类型，也就是<font color=FF0000>类似</font> `1111`、`'aaaa'`、`{ a: 1 }` <font color=FF0000>这种值也可以作为类型</font>

其中，<font color=FF0000>**字符串的字面量类型有两种**</font>：一种是普通的字符串字面量，比如 `'aaa'`；<font color=FF0000>**另一种是模版字面量**，比如</font> `aaa${string}` ，<font color=FF0000>它的意思是以 “aaa” 开头，后面是任意 string 的字符串字面量类型</font>。

所以想要约束以某个字符串开头的字符串字面量类型时可以这样写：

<img src="https://s2.loli.net/2022/05/01/2ajXHbDgC13WLik.png" alt="image-20220501181210720" style="zoom:50%;" />

##### 四种特殊类型

还有<font color=FF0000>**四种特殊的类型：void、never、any、unknown**</font>：

- **void** 代表空，可以是 null 或者 undefined，<font color=FF0000>一般是用于函数返回值</font>。
- **any** 是任意类型，任何类型都可以赋值给它，它也<font color=FF0000>可以赋值给任何类型（ <font size=4>**除了 never**</font> ）</font>。
- **unknown** 是<font color=FF0000>**未知类型**，**任何类型都可以赋值给它**，但是它 <font size=4>**不可以赋值给别的类型**</font></font>。
- **never** <font color=FF0000>**代表不可达，比如函数抛异常的时候，返回值就是 never**</font> （**注：**“异常” 的相关示例 [[#交叉：&]] 。另外，根据下面（[[#推导：infer]]）的代码可知，在类型编程时，使用 `infer ? :` 不符合条件的，也可用 never 作为类型）。

**注：**下面有说 any 和 unknown 的区别： [[#数组类型#First]] ，简单来说就是：unknown 不可给别的类型赋值，而 any 可以（除了 never ）。

**这些就是 TypeScript 类型系统中的全部类型了**，<mark>大部分是从 JS 中迁移过来的</mark>，比如基础类型、Array、class 等；<mark>也添加了一些类型</mark>，比如 枚举 ( enum ) 、接口 ( interface ) 、元组等，<mark>还支持了字面量类型和 void、never、any、unknown 的特殊类型</mark>。

#### 

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

##### 条件：extends ? :

TypeScript 里的条件判断是 `extends ? :`  ，叫做条件类型 ( Conditional Type ) 。比如：

```typescript
type res = 1 extends 2 ? true : false;
```

<img src="https://s2.loli.net/2022/05/03/oJRMaOD7t63vYHT.png" alt="image-20220501183041897" style="zoom:50%;" />

这就是 TypeScript 类型系统里的 if else。

>  **注：**这里的 `extends ? :` 可以表示为 “是否是子集”（学习自：[白话typescript中的【extends】和【infer】](https://juejin.cn/post/6844904146877808653)），而不是 “是否继承自” ，如下示例：
>
> ```typescript
> type Union = 1 | 2 | 3
> type IsUnion = (1 | 2) extends Union ? true: false // type IsUnion = true
> type ISUnion2 = 4 extends Union ? true: false // type ISUnion2 = false
> ```

但是，上面这样的逻辑没啥意义，静态的值自己就能算出结果来，为什么要用代码去判断呢？所以，类型运算逻辑都是用来做一些动态的类型的运算的，也就是对类型参数的运算

```typescript
type isTwo<T> = T extends 2 ? true : false

type res = isTwo<1>
type res2 = isTwo<2>
```

<img src="https://s2.loli.net/2022/05/01/M4vaIWP7Hzp3VRN.png" alt="image-20220501204824969" style="zoom:55%;" />

<img src="https://s2.loli.net/2022/05/01/JRTgaqY59E8ju6F.png" alt="image-20220501204852937" style="zoom:55%;" />

<font color=FF0000>这种类型也叫做「高级类型」。**高级类型的特点是 传入类型参数，经过一系列类型运算逻辑后，返回新的类型**</font>（**注：**感觉和 “高阶函数” 有点类似 ）。

##### 推导：infer

<font color=FF0000 size=4>**如何提取类型的一部分呢？答案是 infer**</font>

比如提取元组类型的第一个元素：

```typescript
type First<Tuple extends unknown[]> = Tuple extends [infer T,...infer R] ? T : never;
// 注：这里的 Tuple 是范型的“类型变量”，可以起其他名字。另外，经过实验发现：类型编程中似乎没有 tuple 这个类型。
// 注：这里有语句 infer T 和 infer R，infer variable 相当于 声明了一个变量，这个变量可以在后面使用（比如 ? 后面返回的 T ）。学习自：https://juejin.cn/post/6844904146877808653

type res = First<[1,2,3]>;
```

<img src="https://s2.loli.net/2022/05/01/dTDby751tEAlzXS.png" alt="image-20220501210250796" style="zoom:50%;" />

注意，第一个 extends ( ` Tuple extends unknown` ) 不是条件，条件类型是 `extends ? :` ；<font color=FF0000>这里的 extends 是约束的意思</font>，也就是约束类型参数只能是数组类型。另外，因为不知道数组元素的具体类型，所以用 unkown 。 

##### 联合：｜

联合类型 ( Union ) 类似 js 里的或运算符 `|`，但是作用于类型，代表类型可以是几个类型之一。

```typescript
type Union = 1 | 2 | 3;
```

**注：** 上面有自己的关于 `extends ? :` 使用 Union 的示例。

##### 交叉：&

交叉类型（Intersection）类似 js 中的与运算符 &，但是作用于类型，代表对类型做合并。

```typescript
type ObjType = {a: number} & {c: string}
type res = { a: number, c: boolean} extends ObjType ? true : false
```

<img src="https://s2.loli.net/2022/05/01/jcnv1fU6FzDZmb8.png" alt="image-20220501211038464" style="zoom:55%;" />

注意，同一类型可以合并，不同的类型没法合并，会被舍弃  ：

<img src="https://s2.loli.net/2022/05/01/h5KndlIRLG9Xzp2.png" alt="image-20220501211209605" style="zoom:55%;" />

##### 映射类型

对象、class 在 TypeScript 对应的类型是 「索引类型」 ( Index Type ) ，那么如何对索引类型作修改呢？答案是「映射类型」 。

```typescript
type MapType<T> = { // 注：注意这里有一对 {}
  [Key in keyof T]?: T[Key] // 注：这里的 ? 表示可选
}
type MapTypeRes = MapType<{a: 1, b: 2}>
```

<img src="https://s2.loli.net/2022/05/01/E9Rny15MuiXbjNC.png" alt="image-20220501212209184" style="zoom:55%;" />

`keyof T` 是查询索引类型中所有的索引，叫做「索引查询」。

`T[Key]` 是取索引类型某个索引的值，叫做「索引访问」。

`in` 是用于遍历联合类型的运算符（**注：**类似于 for...in ）。

比如我们把一个索引类型的值变成 3 个元素的数组：

```typescript
type MapType<T> = {
    [Key in keyof T]: [T[Key], T[Key], T[Key]]
}

type MapTypeRes = MapType<{a: 1, b: 2}>;
```

<img src="https://s2.loli.net/2022/05/01/thRPmzXavor5ny2.png" alt="image-20220501212611967" style="zoom:55%;" />

**映射类型就相当于把一个集合映射到另一个集合，这是它名字的由来**。

<img src="https://s2.loli.net/2022/05/01/fdso7akByRzTD8m.png" alt="img" style="zoom:70%;" />

<font color=FF0000 size=4>**除了值可以变化，索引也可以做变化**</font>；用 `as` 运算符，叫做「重映射」。

```typescript
type MapType<T> = {
    [
       Key in keyof T
           as `${Key & string}${Key & string}${Key & string}`
    ]: [T[Key], T[Key], T[Key]]
}
```

<img src="https://s2.loli.net/2022/05/01/a6jQCHfwO41G2vi.png" alt="image-20220501213009300" style="zoom:50%;" />

<font size=4>**这里为什么 Key 后面跟着 `& string`：**</font>因为 <font color=FF0000>**索引类型（对象、class 等）可以用 string、number 和 symbol 作为 key** ，这里 `keyof T` 取出的索引就是 `string | number | symbol` 的联合类型，**和 string 取交叉，结果就只剩下 string 了**</font>。就像前面所说，交叉类型会把同一类型做合并，不同类型舍弃。



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

<img src="https://s2.loli.net/2022/05/02/e86qSsLR4BY7KWI.png" alt="image-20220502233642842" style="zoom:55%;" />

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

类型参数 Arr 通过 extends 约束为只能是数组类型，数组元素是 unkown 也就是可以是任何值。

> **any 和 unknown 的区别**： any 和 unknown 都代表任意类型，但是 unknown 只能接收任意类型的值，而 <font color=FF0000>any 除了可以接收任意类型的值，也可以赋值给任意类型（除了 never ）</font>。类型体操 中经常用 unknown 接受和匹配任何类型，而很少把任何类型赋值给某个类型变量。

对 Arr 做模式匹配，把我们要提取的第一个元素的类型放到通过 infer 声明的 First 局部变量里，后面的元素可以是任何类型，用 unknown 接收，然后把局部变量 First 返回。

当类型参数 Arr 为 `[1, 2, 3]` 时：

<img src="https://s2.loli.net/2022/05/02/ckwxlXWBHE4CNpz.png" alt="image-20220502235103556" style="zoom:55%;" />

当类型参数 Arr 为 `[]` 时：

<img src="https://s2.loli.net/2022/05/03/GNSFKJ9aOh4MHg3.png" alt="image-20220502235223149" style="zoom:55%;" />

##### Last

同理：

```ts
type arr = [1, 2, 3]
type GetLast<Arr extends unknown[]> = Arr extends [...unknown[], infer Last] ? Last : never
```

<img src="https://s2.loli.net/2022/05/02/DRLslNntuM9qvxK.png" alt="image-20220502235408475" style="zoom:55%;" />

##### PopArr

我们分别取了首尾元素，当然也可以取剩余的数组，比如取去掉了最后一个元素的数组：

```ts
type PopArr<Arr extends unknown[]> = 
     Arr extends [] ? []
       : Arr extends [...infer Rest, unknown] ? Rest : never
```

如果是空数组，就直接返回，否则匹配剩余的元素，放到 infer 声明的局部变量 Rest 里，返回 Rest。

<img src="https://s2.loli.net/2022/05/02/i5j8hX9MEUWeNS2.png" alt="image-20220502235934682" style="zoom:55%;" />

##### ShiftArr

类似的：

```ts
type ShiftArr<Arr extends unknown[]> = 
     Arr extends [] ? []: 
         Arr extends [unknown, ...infer Rest] ? Rest : never
```

<img src="https://s2.loli.net/2022/05/03/JRC8purXPK7F9b1.png" alt="image-20220503001221372" style="zoom:55%;" />

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

<img src="https://s2.loli.net/2022/05/03/5ztyfvGQ9RsIcHn.png" alt="image-20220503002057792" style="zoom:55%;" />

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

<img src="https://s2.loli.net/2022/05/03/YWsSMpciHq3L8uG.png" alt="image-20220503011523443" style="zoom:55%;" />

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

<img src="https://s2.loli.net/2022/05/03/tVeHcRKb8izXpWd.png" alt="image-20220503012245739" style="zoom:55%;" />

##### GetReturnType

能提取参数类型，同样也可以提取返回值类型：

```ts
type GetReturnType<Func extends Function> = 
    Func extends (...args: any[]) => infer ReturnType // 注：注意这里就是一个 infer ReturnType 
        ? ReturnType : never;
// 注：自己尝试写的时候，想要在 infer 前面加 unknown 之类的返回值类型，不过在报错；所以没写出来。
```

Func 和模式类型做匹配，提取返回值到通过 infer 声明的局部变量 ReturnType 里返回。

参数类型可以是任意类型，也就是 any[]（<font color=FF0000>**注意，这里不能用 unknown，因为参数类型是要赋值给别的类型的，而 unknown 只能用来接收类型，所以用 any**</font> ）。**注：**这里在写的时候，使用 unknown 了

<img src="https://s2.loli.net/2022/05/03/Ts86t1wmAFi3Vgd.png" alt="image-20220503013038807" style="zoom:55%;" />

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

这样，当 call / apply 调用的时候，就能检查出 this 指向的对象是否是对的（**注：**这里 tsconfig.json 要加上 `"strictBindCallApply": true` ，否则不会报错 ）：

<img src="https://s2.loli.net/2022/05/03/J9IsuvlEmdkQpFt.png" alt="img" style="zoom:67%;" />

<font color=FF0000>**这里的 this 类型同样也可以通过模式匹配提取出来：**</font>

```ts
type GetThisParameterType<T> 
    = T extends (this: infer ThisType, ...args: any[]) => any // 注：注意这里“函数签名”的写法，this 作为第一个参数
        ? ThisType 
        : unknown;
```

<font color=FF0000>类型参数 T 是待处理的类型</font>。<font color=FF0000>**用 T 匹配一个 模式类型，提取 this 的类型到 infer 声明的局部变量 ThisType 中**</font>（ **注：**这里有点没看懂 TODO ），其余的参数是任意类型，也就是 any，返回值也是任意类型。返回提取到的 ThisType，这样就能提取出 this 的类型：

<img src="https://s2.loli.net/2022/05/03/U8fGXHzVY9Dvx4u.png" alt="image-20220503015143577" style="zoom:55%;" />



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

<img src="https://s2.loli.net/2022/05/03/Uyjbk4OPWv1Rst2.png" alt="image-20220503021238662" style="zoom:55%;" />

##### GetConstructorParameters

GetInstanceType 是提取构造器返回值类型，那同样也可以提取构造器的参数类型：

```ts
type GetConstructorParameters<
    ConstructorType extends new (...args: any) => any
> = ConstructorType extends new (...args: infer ParametersType) => any // 注：自己尝试实现时，没准备加上 `args:`
    ? ParametersType
    : never;
```

类型参数 ConstructorType 为待处理的类型，通过 extends 约束为构造器类型。

用 ConstructorType 匹配一个模式类型，提取参数的部分到 infer 声明的局部变量 ParametersType 里，返回 ParametersType。这样就能提取出构造器对应的参数类型：

<img src="https://s2.loli.net/2022/05/03/qFPGlZMKv69m3uk.png" alt="image-20220503021900246" style="zoom:55%;" />

#### 索引类型

<font color=FF0000>索引类型也同样可以用模式匹配提取某个索引的值的类型</font>，这个用的也挺多的。比如 React 的 index.d.ts 里的 PropsWithRef 的高级类型，就是通过模式匹配提取了 ref 的值的类型：

![img](https://s2.loli.net/2022/05/03/m6bs8RoiGjEUOJZ.png)

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

在 ts3.0 里面如果没有对应的索引，Obj[Key] 返回的是 {} 而不是 never，所以这样做向下兼容处理。如果有 ref 这个索引的话，就通过 infer 提取 Value 的类型返回，否则返回 never。

<img src="https://s2.loli.net/2022/05/03/jV81pD4QqSUIs7n.png" alt="image-20220503023006564" style="zoom:55%;" />



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

<img src="https://s2.loli.net/2022/05/03/nioRQDWIr5B3jXx.png" alt="image-20220503023957842" style="zoom:55%;" />

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

<img src="https://s2.loli.net/2022/05/03/C2Tm4inx5q6XUHa.png" alt="image-20220503141940873" style="zoom:55%;" />

但是这样只能合并两个元素的元组，如果是任意个呢？那就得用递归了：

```ts
type Zip<One extends unknown[], Other extends unknown[]> = 
     One extends [infer OneFir, ...infer OneRest] 
         ? Other extends [infer OtherFir, ...infer OtherRest]
             ? [[OneFir, OtherFir], ...Zip<OneRest, OtherRest>] : [] // 注：注意这里的 ...zip
             : []
```

 **注：**自己尝试实现时，`...zip` 没写出，写的是 `Zip<oneRest, OtherRest>` ，显然是错的

类型参数 One、Other 声明为 unknown[]，也就是元素个数任意，类型任意的数组。

每次提取 One 和 Other 的第一个元素 OneFirst、OtherFirst，剩余的放到 OneRest、OtherRest 里。用 OneFirst、OtherFirst 构造成新的元组的一个元素，剩余元素继续递归处理 OneRest、OtherRest。这样，就能处理任意个数元组的合并：

<img src="https://s2.loli.net/2022/05/03/utcTYmVFoRKynib.png" alt="image-20220503143011106" style="zoom:55%;" />

#### 字符串类型的重新构造

##### CapitalizeStr

我们想把一个字符串字面量类型的 'guang' 转为首字母大写的 'Guang'。需要用到字符串类型的提取和重新构造：

```ts
type CapitalizeStr<Str extends string> = Str extends `${infer First}${infer Rest}` ? // 注：Rest 变量接收剩余字符
      `${Uppercase<First>}${Rest}` : Str // 注：注意这里的 Uppercase<>
```

我们声明了类型参数 Str 是要处理的字符串类型，通过 extends 约束为 string。

通过 infer 提取出首个字符到局部变量 First，<font color=FF0000>**提取后面的字符到局部变量 Rest**</font>。然后 <font color=FF0000 size=4>**使用 TypeScript 提供的内置高级类型 Uppercase 把首字母转为大写**</font>（**注：**这个没接触过），加上 Rest，构造成新的字符串类型返回。

<img src="https://s2.loli.net/2022/05/03/wvi3oPrcjmteg1O.png" alt="image-20220503145046293" style="zoom:55%;" />

#### CamelCase

我们再来实现 `dong_dong_dong` 到 dongDongDong 的变换。同样是提取和重新构造：

```typescript
type CamelCase<Str extends string> = 
    Str extends `${infer Left}_${infer Right}${infer Rest}`
        ? `${Left}${Uppercase<Right>}${CamelCase<Rest>}`
        : Str;
```

> 注：这个写出来了
>
> ```ts
> type CamelCase<Str extends String> = 
>      Str extends `${infer prefix}_${infer upper}${infer suffix}` 
>          ? `${prefix}${Uppercase<upper>}${CamelCase<suffix>}`: Str
> ```

类型参数 Str 是待处理的字符串类型，约束为 string。

提取 `_` 之前和之后的两个字符到 infer 声明的局部变量 Left 和 Right，剩下的字符放到 Rest 里。然后把右边的字符 Right 大写，和 Left 构造成新的字符串，剩余的字符 Rest 要继续递归的处理。这样就完成了从下划线到驼峰形式的转换：

<img src="https://s2.loli.net/2022/05/03/ghlzYSIbU3wstDv.png" alt="image-20220503150004887" style="zoom:55%;" />

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

<img src="https://s2.loli.net/2022/05/03/ku7L49hHy3snDwx.png" alt="image-20220503150708754" style="zoom:55%;" />

#### 函数类型的重新构造

##### AppendArgument

之前我们分别实现了参数和返回值的提取，那么 <font color=FF0000>重新构造就是用这些提取出的类型做下修改，构造一个新的类型</font> 即可。

比如，<font color=FF0000>在已有的 **函数类型**</font> （**注：**这里不要看错 ）<font color=FF0000>上添加一个参数</font>：

```ts
type AppendArgument<Func extends Function, Arg> = 
    Func extends (...args: infer Args) => infer ReturnType // 注：这里实现时 ...args: infer Args 有遗忘
        ? (...args: [...Args, Arg]) => ReturnType : never; // 注：这里实现时写错了，没组成一个数组
```

类型参数 Func 是待处理的函数类型，通过 extends 约束为 Function，Arg 是要添加的参数类型。

通过模式匹配提取参数到 infer 声明的局部变量 Args 中，提取返回值到局部变量 ReturnType 中。<font color=FF0000>用 Args 数组添加 Arg 构造成新的参数类型</font>，结合 ReturnType 构造成新的函数类型返回。这样就完成了函数类型的修改：

<img src="https://s2.loli.net/2022/05/03/JGdQ7Cvi6hT2r1g.png" alt="image-20220503175404536" style="zoom:55%;" />

#### 索引类型的重新构造

**注：**这部分内容和 [[#TypeScript 类型系统中的类型运算#映射类型]] 中的几乎一致，这里略。不过，还说了一个 UppercaseKey 的 “参数名大写的变换”；不过由于前面知识的学习，这没有什么难度：

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

<img src="/Users/yan/Library/Application Support/typora-user-images/image-20220503191406123.png" alt="image-20220503191406123" style="zoom:50%;" />

##### ToRequired

同理，也可以去掉可选修饰符：

```ts
type ToRequired<T> = {
    [Key in keyof T]-?: T[Key]
}
```

给索引类型 T 的索引去掉 ? 的修饰 ，其余保持不变。

<img src="/Users/yan/Library/Application Support/typora-user-images/image-20220503191609690.png" alt="image-20220503191609690" style="zoom:50%;" />

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

构造新的索引类型，索引为 Obj 的索引，也就是 `Key in keyof Obj` ；<font color=FF0000>**要做一些变换，也就是 as 之后的部分**</font>。如果原来索引的值 `Obj[Key]` 是 ValueType 类型，索引依然为之前的索引 Key；否则索引设置为 never，<font color=FF0000 size=4>**never 的索引会在生成新的索引类型时被去掉**</font>。值保持不变，依然为原来索引的值，也就是 `Obj[Key]` 。

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

这里是 3 层 Promise，value 类型是索引类型。<mark>数量不确定，一涉及到这个就要想到用递归来做，每次只处理一层的提取，然后剩下的到下次递归做，直到结束条件</mark>。

所以高级类型是这样的：

```ts
type DeepPromiseValueType<P extends Promise<unknown>> = 
     P extends Promise<infer ValueType>
        ? ValueType extends Promise<unknown> // 注：这里做的是判断 ValueType 是不是 Promise 类型，还是 Promise 的内容
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

**注：**这个写出来了，而且代码几乎一模一样；讲解略。

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

通过「模式匹配」提取数组中的一个元素的类型，如果是 Item 类型的话就删除，也就是不放入构造的新数组，直接返回之前的 Result ；否则放入构造的新数组，也就是再构造一个新的数组 `[...Result, First]` 。直到模式匹配不再满足，也就是处理完了所有的元素，返回这时候的 Result 。**注：**这里 `[...Result, First]` 为什么 First 放在后面，是因为 First 是 “当前“ 递归 的 Item ，所以相当于 将当前的 Item 放在 Result 后面。另外，下面有类似的例子 [[#字符串类型的递归#ReverseStr]]，也有讲解。

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

注：这个不难，但是没写出；原因在于不知道使用 `Arr['length']`

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

**注：**较原版只是添加了递归，讲解略

##### StringToUnion

我们想把字符串字面量类型的每个字符都提取出来组成联合类型，也就是把 'dong' 转为 'd' | 'o' | 'n' | 'g' 。

```ts
type StringToUnion<Str extends string> = 
    Str extends `${infer First}${infer Rest}` // 注：这里写的时候，始终想着 ...infer Rest，这是数组的用法
        ? First | StringToUnion<Rest> // 注：这里确实没想到 `｜` 可以直接使用
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

类型参数 Str 为待处理的字符串。类型参数 Result 为构造出的字符，<font color=FF0000>默认值是空串</font>（注：注意这种写法）。

通过模式匹配提取第一个字符到 infer 声明的局部变量 First，其余字符放到 Rest 。用 First 和之前的 Result 构造成新的字符串，<font color=FF0000>**把 First 放到前面，因为 <font size=4>递归是从左到右处理，那么不断往前插就是把右边的放到了左边</font>，完成了反转的效果**</font>。直到模式匹配不满足，就处理完了所有的字符。

这样就完成了字符串的反转：

> 注：自己写对了，不过方法不一样。
>
> ```ts
> type ReverseStr<Str extends string> = 
>      Str extends `${infer First}${infer Rest}`
>      ? `${ReverseStr<Rest>}${First}`
>      : ''
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

> **注：**⚠️ 需要注意的是： <font size=4>**`Function extends object ? true : false` 结果为 <font color=FF0000>true</font> **</font> ， <font size=4>**`string extends object ? true : false` 结果为 <font color=FF0000>false</font>**</font>  

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

所以，<font size=4><font color=FF0000>可以在前面加上一段</font> `Obj extends never ? never` <font color=FF0000>或者</font> `Obj extends any` ，<font color=FF0000>让它触发计算</font></font>；另外，<font size=4>写 `Obj extends any` 还有额外的好处，就是<font color=FF0000>能 **处理联合类型**</font></font>，（注：形成 “分布式条件类型” ）相关讲解见 [[#联合分散可简化]] 

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

TS 类型系统 <font color=FF0000>**可以实现**</font> “数值相关的逻辑”

#### 数组长度做计数

TypeScript 类型系统没有加减乘除运算符，怎么做数值运算呢？

不知道大家有没有注意到 <font color=FF0000>**数组类型取 length 就是数值**</font>。如下示例：

<img src="https://s2.loli.net/2022/05/04/3OfnzTmt8H5pGRS.png" alt="image-20220504005055617" style="zoom:50%;" />

<font color=FF0000>**TypeScript 类型系统中 <font size=4>没有加减乘除运算符</font>，但是 <font size=4>可以通过构造不同的数组然后取 length 的方式来完成数值计算</font>，<font size=4>把数值的加减乘除转化为对数组的提取和构造</font>。**</font>（严格来说构造的是元组，大家知道数组和元组的区别就行）

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

**注：**这里的讲解代码略，略。

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

> **注：**有点神奇的是，Divide 的 “代码逻辑” 上没有找到处理 无法整除的情况；但是 `Divide<10, 3>` 结果会是 never 。有点没搞懂 TODO

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

**注：**TS 中字符串没有 "length" 属性，所以才需要将其转换成 数组，再获取数组的长度。另外，这里实现没什么问题，讲解略。

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

**当 <font color=FF0000>类型参数为联合类型</font>，并且在 <font color=FF0000 size=4>条件类型</font> （注：即 `extends ? :` 。另外，这个很重要，下面 [[#IsUnion]] 中会用到这个特性 ）左边直接引用该类型参数的时候：<font color=FF0000>TypeScript 会把 <font size=4>每一个元素单独传入来做类型运算，最后再合并成联合类型</font></font>，这种语法叫做「分布式条件类型」。**

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

提取 \_ 左右的字符，把右边字符大写之后构造成新的字符串，余下的字符串递归处理。

<img src="https://s2.loli.net/2022/05/04/DOWzqgQj8xhulNY.png" alt="image-20220504142306146" style="zoom:50%;" />

另外，由于上面讲的特性，这里 `Camelcase` 中类型参数可以放入一个联合类型值，效果一样；这里只放一下截图。另外，下面也会讲。

<img src="https://s2.loli.net/2022/05/04/tS5a1cAPTGDYKxR.png" alt="image-20220504142525257" style="zoom:50%;" />

如果是对字符串数组做 Camelcase，那就要递归处理每一个元素：

```ts
type CamelcaseArr<
  Arr extends unknown[] // 注：这里 写 string[] 下面写 CamelcaseArr<RestArr, string[]> 会报错，不知道为什么 TODO
> = Arr extends [infer Item, ...infer RestArr]
        ? [Camelcase<Item & string>, ...CamelcaseArr<RestArr>] //注：这里 & string 要注意，没想到这样写。
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
        ? [B] extends [A] // 注：注意这里的 [B] 和 [A]，A 和 B 都使用 [] 包裹，原因见下面。
            ? false
            : true
        : never
```

这段逻辑有点奇怪（见上面 “注” ），这就是分布式条件类型带来的认知成本。

我们先来看这样一个类型：

```ts
type TestUnion<A, B = A> = 
     A extends A
        ? { a : A, b : B }
        : never;

type TestUnionResult = TestUnion<'a' | 'b' | 'c'>;
```

传入联合类型 'a' | 'b' | 'c' 的时候，结果是这样的：

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

`A extends A` 这段看似没啥意义，主要是为了触发「分布式条件类型」，让 A 的每个类型单独传入。`[B] extends [A]` 这样<font color=FF0000 size=4>**不直接写 B 就可以避免触发「分布式条件类型」**</font>（**注：**这里的避免的原理见 [[#分布式条件类型]] 开头的定义 ），<font color=FF0000>那么 B 就是 整个联合类型</font>。B 是联合类型整体，而 A 是单个类型，自然不成立，而其它类型没有这种特殊处理，A 和 B 都是同一个，怎么判断都成立。

> 注：上面最后一句没看懂。不过，经过实验：`'a' extends ['a' | 'b' | 'c'] ? true : false`  结果为 false，而 `['a'] extends ['a' | 'b' | 'c'] ? true : false` 结果为 true。

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

<font color=FF0000 size=4>**数组转联合类型可以这样写：**</font>

```ts
type union = ['foo', 'bar'][number]
```

<img src="/Users/yan/Library/Application Support/typora-user-images/image-20220504183301073.png" alt="image-20220504183301073" style="zoom:50%;" />

那么 BEM 就可以这样实现：

```ts
type BEM<
    Block extends string,
    Element extends string[],
    Modifiers extends string[]
> = `${Block}__${Element[number]}--${Modifiers[number]}`; // 注：注意这里的 [number]，就是上面的用法
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

**类型的判断要根据它的特性来，比如判断联合类型就要根据它的 distributive （注：即“分布式条件类型” ） 的特性。**

我们分别看一下这些特性：

##### IsAny

如何判断一个类型是 any 类型呢？要根据它的特性来：**<font color=FF0000>any 类型与任何类型的交叉都是 any</font> ，也就是 `1 & any` 结果是 any**（**注：**有点“粘性” ( sticky ) 的意味？）。所以，可以这样写：

```typescript
type IsAny<T> = 'foo' extends ('bar' & T) ? true : false
```

这里的 'foo' 和 'bar' 可以换成任意类型（**注：**这里 'foo' 和 'bar' 都代表一种类型，所以可以换成 'string' 和 'number' ）

<img src="https://s2.loli.net/2022/05/04/IjWPzOXD1SAcqkN.png" alt="image-20220504202335587" style="zoom:50%;" />

**注：**另外，下面 [[#IsNever]] 还提及了为什么 any 不能直接使用 `extends ? :` 去判断（简单来说，会返回 `extends ? :` 设定的 trueVal 和 falseVal 的联合 ( Union) ）

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

**注：**下面 [[#IsTuple]] 有实现一个 NonEqual ，有说实现原理，可以参考下。

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

除了上述 never 以外，any 在「条件类型」中也比较特殊：如果类型参数为 any，会直接返回 trueType（下例中的 1 ） 和 falseType （下例中的 2 ）的 联合 ( Union ) ：

```ts
type TestAny<T> = T extends number ? 1 : 2;
```

<img src="https://s2.loli.net/2022/05/04/EGy3OoDaPwuBk7Z.png" alt="image-20220504214930961" style="zoom:50%;" />

联合类型、never、any 在作为条件类型的类型参数时的这些特殊情况，也会在后面的原理篇来解释原因。

##### IsTuple

元组类型怎么判断呢？它和数组有什么区别呢？**元组类型也是数组类型，但 <font color=FF0000 size=4>每个元素都是只读的</font>；并且 <font color=FF0000 size=4>元组的 length 是数字字面量</font>，而 <font color=FF0000 size=4>数组的 length 是 number </font>。**

如图，元组和数组的 length 属性值是有区别的：

<img src="https://s2.loli.net/2022/05/04/nwJHC3ABDh1SITM.png" alt="image-20220504215632945" style="zoom:50%;" />

<img src="https://s2.loli.net/2022/05/04/1OaXxn25DKpvsHt.png" alt="image-20220504215702395" style="zoom:50%;" />

那我们就可以根据这两个特性（ readonly 和 length 是数字字面量 ）来判断元组类型：

```ts
type IsTuple<T> = 
    // 注：第一：readonly 的性质没想到，第二：没想到可以 readonly 这样判断。另外，`params:` 可加可不加
    T extends readonly [...params: infer Eles]
        ? NotEqual<Eles['length'], number> // 注：由上图可知，number 没有引号
        : false
```

类型参数 T 是要判断的类型。

首先判断 T 是否是数组类型，如果不是则返回 false。如果是，继续判断 length 属性是否是 number；如果是数组并且 length 不是 number 类型，那就代表 T 是元组。

**NotEqual 的实现：**

```ts
type NotEqual<A, B> = (<T>() => T extends A ? 1 : 2) extends (<T>() => T extends B ? 1 : 2)
    ? false : true;
```

A 是 B 类型，并且 B 也是 A 类型，那么就是同一个类型，返回 false，否则返回 true。

**传入元组时：**

<img src="https://s2.loli.net/2022/05/04/5TdDJwM4WXPVjC7.png" alt="image-20220504222653620" style="zoom:50%;" />

**传入数组时：**

<img src="/Users/yan/Library/Application Support/typora-user-images/image-20220504223131230.png" alt="image-20220504223131230" style="zoom:50%;" />

##### UnionToIntersection

<font color=FF0000>**类型之间是有父子关系的，更具体的那个是子类型**；比如 A 和 B 的交叉类型 `A & B` 就是联合类型 `A | B` 的子类型，因为更具体</font>。

<font color=FF0000>**如果允许父类型赋值给子类型，就叫做<font size=4>「逆变」</font>。如果允许子类型赋值给父类型，就叫做<font size=4>「协变」</font>**</font>。详细概念见原理篇 TODO

在 TypeScript 中 <font color=FF0000>有「函数参数」是有逆变的性质的，也就是：如果参数可能是多个类型，参数类型会变成它们的交叉类型</font>。

所以联合转交叉可以这样实现 ：

```typescript
type UnionToIntersection<U> = 
    (U extends U ? (x: U) => unknown : never) extends (x: infer R) => unknown
        ? R
        : never
```

类型参数 U 是要转换的联合类型。

`U extends U` 是为了触发 “联合类型” 的 distributive（即：“分布式条件类型” ） 的性质，让每个类型单独传入做计算，最后合并。利用 U 做为参数构造个函数，通过模式匹配取参数的类型。结果就是交叉类型：

<img src="https://s2.loli.net/2022/05/04/CSxsB6PjJDNRTlv.png" alt="image-20220504230627100" style="zoom:50%;" />

##### GetOptional

如何提取索引类型中的可选索引呢？

这也要利用可选索引的特性：**可选索引的值为 undefined 和 值类型 的联合类型**。

<img src="https://s2.loli.net/2022/05/04/uxlX6D7yPq2fWAY.png" alt="img" style="zoom:65%;" />

**注：**在当前 VS Code & typescript@4.5.4 中，不知道为什么只能显示出 `age?: number` ，而不能显示出 `age?: number | undefined` 

```ts
type GetOptional<Obj extends Record<string, any>> = {
    [
        Key in keyof Obj 
            as {} extends Pick<Obj, Key> ? Key : never // 注：这里将 Pick<Obj, Key> 换成 Obj[Key] 没法得到预期的结果
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

<font color=FF0000 size=4>**因为 age 可能为 undefined，也就是索引类型可能是 {}** </font>；所以 `{} extends Pick<Obj, Key>` 就能过滤出可选索引。（<font color=FF0000>可选的意思就是有或者没有，<font size=4>**没有的时候就是空的索引类型**</font></font>）。值的类型依然是之前的，也就是 Obj[Key]。

这样，就能过滤出所有可选索引，构造成新的索引类型：

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8c0cdb56dfe343229a5f9136f52980c6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.image?" alt="img" style="zoom:65%;" />
