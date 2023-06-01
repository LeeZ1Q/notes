# U don‘t know JS

## book1: get started

### ch2

var 没有块级作用域

let 块级作用域 

const不要与对象一起用



### ch3

闭包：内层函数保留了外层函数传进来的参数，并且在**别的地方**运行的时候还得以保留

  --------------不一定得是外层函数，外层作用域就行----------

闭包里的变量就是真实的变量，不是快照

原型链≈继承



## book2: scope-closures

### ch1 What's the Scope?

作用域在编译中产生 运行时才真正创建

JS通常被规划解释型脚本语言，但实际上有 解析 + 编译

Modern JS  编译 + 解释

编译然后执行

不要用 eval 和 with



### ch2 Illustrating Lexical Scope

声明在编译阶段、赋值在运行阶段

not defined 都没声明；undefined 声明了没赋值



### ch3 The Scope Chain

V8引擎已经知道变量来自哪个作用域了

里面和外面的变量同名，用里面的

var有函数作用域



### ch4 Around the Global Scope

全局作用域

JS引擎如何将单独的文件拼接在一起？

- ES modules  import/export

- bundler 创建一个公共的作用域

  ```jsx
  var moduleOne = (function one(){
      // ..
  })();
  var moduleTwo = (function two(){
      // ..
  
      function callModuleOne() {
          moduleOne.someMethod();
      }
  
      // ..
  })();
  ```

  

- 借助全局作用域

```jsx
var moduleOne = (function one(){
    // ..
})();
var moduleTwo = (function two(){
    // ..

    function callModuleOne() {
        moduleOne.someMethod();
    }

    // ..
})();
```



var是一个全局对象的属性，let/const在全局作用域下是变量

仅在全局范围本身内，同名的的全局对象属性可以被全局变量遮蔽：

```jsx
window.something = 42;

let something = "Kyle";

console.log(something);
// Kyle

console.log(window.something);
// 42
```



### ch5 The (Not So) Secret Lifecycle of Variables

*function hoisting*：当 `function` 声明的名称标识符在其作用域的顶部注册时，它还会自动初始化为该函数的引用

```jsx
greeting = "Hello!";
console.log(greeting);
// Hello!

var greeting = "Howdy!";
----------	
var greeting;           // hoisted declaration
greeting = "Hello!";    // the original line 1
console.log(greeting);  // Hello!
greeting = "Howdy!";    // `var` is gone!
```

提升函数声明后提升变量

```jsx
studentName = "Suzy";
greeting();
// Hello Suzy!

function greeting() {
    console.log(`Hello ${ studentName }!`);
}
var studentName;
---------
function greeting() {
    console.log(`Hello ${ studentName }!`);
}
var studentName;

studentName = "Suzy";
greeting();
// Hello Suzy!
```



var的重复声明没有任何意义

`var  studentName`  !=  `var studentName = undefined;` 人为的赋值操作

let和const都不允许重复声明



**每个循环都是其自己新作用域的实例**

for循环的i也是在块级作用域的内部

`for..in` 和 `for..of` 可以与 `const` 一起使用 ，但

```jsx
for (const i = 0; i < 3; i++) {
    // oops, this is going to fail with
    // a Type Error after the first iteration
}
```

给const重新赋值了！



```jsx
for(let i = 0; i < 3; i++){
	setTimeout(() =>{
	 console.log(i);
	},0);
}
// 0 1 2

for(var i = 0; i < 3; i++){
	setTimeout(() =>{
	 console.log(i);
	},0);
}
// 333   setTimeout使异步(循环后的值) var使i全局变量
```



let 和const 的初始化得声明和赋值一起



#### let/const 的变量提升

```jsx
var studentName = "Kyle";

{
    console.log(studentName);
    // ???

    // ..

    let studentName = "Suzy";

    console.log(studentName);
    // Suzy
}
```

这里抛出了TDZ错误，说明内部范围的 `studentName` 被提升了

TDZ 错误的发生是因为 `let` / `const` 声明确实将其声明提升到其范围的顶部，但与 `var` 不同的是，它们将变量的自动初始化推迟到代码序列中出现原始声明的那一刻。



### ch6 Limiting Scope Exposure

为什么不都放在全局范围？

- 命名冲突
- 意外行为/依赖

The Principle of Least Exposure 最小作用域原则（POLE）：

在尽可能小且嵌套深度的作用域中声明变量，而不是将所有内容都放在全局（甚至外部函数）作用域中。

trick：用一个立即执行函数来提供一个作用域  

```jsx
var factorial = (function () {
    var cache = {};

    function factorial(x) {
        if (x < 2) return 1;
        if (!(x in cache)) {
            cache[x] = x * factorial(x - 1);
        }
        return cache[x];
    }

    return factorial;
})();

factorial(6);
// 720
----------------------------------
// outer scope

(function(){
    // inner hidden scope
})();

// more outer scope
```

作者的观点：函数作用域用var ；剩下的用 let / const

立即执行函数构建函数作用域；{ } 构建块级作用域（可嵌套）

不要在块级作用域里写函数的声明：函数定义在外部，通过函数表达式的赋值



### ch7 Using Closures

闭包建立在POLE之上：对于我们需要随着时间的推移使用的变量，我们可以封装（更小的作用域）它们，但仍然保留从内部函数的访问，以便更广泛地使用，而不是将它们放在更大的外部作用域中。函数通过闭包记住这些引用的作用域变量。

箭头函数也会创建一个作用域



闭包与**函数的实例**相关联

每一个实例对应一个闭包

闭包实际上是一个实时链接，保留对完整变量本身的访问（不是快照）

闭包的封闭范围通常来自函数，但实际上并不是必需的;外部作用域中只需要存在一个内部函数

闭包无法保存一个值，保存的是变量



**闭包的定义**

- 内层函数对外部的变量有引用
- 必须在别的作用域调用



由于闭包本质上与函数实例相关联，因此只要仍有对该函数的引用，它对变量的闭包就会持续存在

一旦最后一个函数引用断开了，这个变量的闭包就消失了，那么变量就要被垃圾回收



另一种想法：闭包传递的是对内部函数的引用（之前的想法是把内部函数赋值给某个变量）



闭包的两种心智模型：

- 观察：闭包是一个函数实例，它记住它的外部变量，即使该函数被传递给其他作用域并在其他作用域中调用。

- 实现：闭包是一个函数实例，其作用域环境保留在原地，同时对它的任何引用都会从其他作用域传递和调用。

  

### ch8 The Module Pattern

revealing module：IIFE 单例

Module Factory ：普通的独立函数，多个实例

**Classic Module Definition**

- 必须有一个外部作用域，通常来自至少运行一次的模块工厂函数。
- 模块的内部作用域必须至少具有一条表示模块状态的隐藏信息。
- 模块必须在其公共 API 上返回对至少一个对隐藏模块状态具有闭包的函数的引用（以便实际保留此状态）。

**Node CommonJS Modules**

CommonJS 模块的行为就像单例实例一样，类似于前面介绍的 IIFE 模块定义样式。无论您将同一模块 `require(..)` 多少次，您都只会获得对单个共享模块实例的额外引用。

**ESM**

ESM 是基于文件的，模块实例是单例的，默认情况下所有内容都是私有的。一个值得注意的区别是ESM文件被假定为严格模式，不需要顶部的"use strict"。无法将 ESM 定义为非严格模式。 import/export

模块的实现原理：词法作用域；闭包



## book3： Objects & Classes

#### ch1 Object Foundations

> Everything in JS is an object.

（还真不对）但对象确实重要

对象是JS三大支柱中的第二个支柱的基础：原型。

{ } ：

- 对象文本
- 解构赋值
- 模板字符串
- 块级作用域
- 定义函数体

JSON里面

- 属性名称必须用 `"` 个双引号字符括起来
- 属性值必须是文本（基元、对象或数组），而不是任意 JS 表达式



...object 只能用于声明， `Object.assign(..)`用于修改

浅拷贝

```jsx
myObjShallowCopy = { ...myObj };
```

深拷贝

- lodash库
- `JSON.parse(JSON.stringify(..))` 没循环引用时
- `myObjCopy = structuredClone(myObj);` 不支持克隆函数或 DOM 元素。



`myObj?.address?.city`   == 

`(myObj != null && myObj.address != null) ? myObj.address.city : undefined`



```jsx
fave = 42;

fave;              // 42
fave.toString();   // "42"
```

boxing ：把原始值变为引用类型  unboxing：把引用类型变为 原始值

in 检测整个原型链

hasOwnProperty(..)   ==> Object.hasOwn(..) 仅查询目标对象