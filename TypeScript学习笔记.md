# TypeScript学习笔记



> 学习教程为[《TypeScript入门教程》](https://ts.xcatliu.com)

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
