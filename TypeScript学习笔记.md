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







### 神说要有光《 TypeScript 类型体操通关秘籍》笔记

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

- **<font color=FF0000>支持泛型</font>的类型系统**：泛型的英文是 Generic Type，通用的类型；它可以代表任何一种类型，也叫做「类型参数」。

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

复合类型方面，JS 有 class、Array，这些 TypeScript 类型系统也都支持，但是又多加了三种类型：元组 ( Tuple )、接口 ( Interface )、枚举 (Enum ) 。

##### 元组

元组 ( Tuple ) 就是 <font color=FF0000>元素个数 和 类型 **固定的** **数组类型**</font>：

```typescript
type Tuple = [number, string];
```

**注：**初次接触「元组」这个名词是在 Py，而 TS 中「元组」概念和 Py 不一样。另外，TS 的元组的写法和 JS 中的数组一样；而 TS 对数组的定义是：数组合并了相同类型的对象（ 这个说法来自：[TypeScript 入门教程 - 元组](https://ts.xcatliu.com/advanced/tuple.html) ）

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

- **<font color=FF0000>构造器</font>**（**注：**这个没见过）：

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

**注：**下面有说 any 和 unknown 的区别： [[#数组类型#First]] ，简单来说就是：unknown 不可给别的类型赋值，而 any （除了 never ）可以。

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

<img src="/Users/yan/Library/Application Support/typora-user-images/image-20220501183041897.png" alt="image-20220501183041897" style="zoom:50%;" />

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
// 注：这里有语句 infer T 和 infer R，infer varible 相当于 声明了一个变量，这个变量可以在后面使用（比如 ? 后面返回的 T ）。学习自：https://juejin.cn/post/6844904146877808653

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

##### 映射类型 ⭐️

对象、class 在 TypeScript 对应的类型是 「索引类型」 ( Index Type ) ，那么如何对索引类型作修改呢？答案是「映射类型」 。

```typescript
type MapType<T> = {
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

> **any 和 unknown 的区别**： any 和 unknown 都代表任意类型，但是 unknown 只能接收任意类型的值，而 any 除了可以接收任意类型的值，也可以赋值给任意类型（除了 never ）。类型体操中经常用 unknown 接受和匹配任何类型，而很少把任何类型赋值给某个类型变量。
