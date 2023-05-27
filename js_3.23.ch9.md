# Class 类

## 基本语法

constructor 里写私有属性

能被继承的方法写外面

new会自动调用constructor 

对象里用逗号，类的方法之间不用



```javascript
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

// 佐证：User 是一个函数
alert(typeof User); // function
```

`class User {...}` 构造实际上做了如下的事儿：

1. 创建一个名为 `User` 的函数，该函数成为类声明的结果。该函数的代码来自于 `constructor` 方法（如果我们不编写这种方法，那么它就被假定为空）。
2. 存储类中的方法，例如 `User.prototype` 中的 `sayHi`。



区别

1. 通过 `class` 创建的函数具有特殊的内部属性标记 `[[IsClassConstructor]]: true`。因此，它与手动创建并不完全相同
2. 类方法不可枚举
3. 类总是使用 `use strict`



基本的类语法看起来像这样：

```javascript
class MyClass {
  prop = value; // 属性

  constructor(...) { // 构造器
    // ...
  }

  method(...) {} // method

  get something(...) {} // getter 方法
  set something(...) {} // setter 方法

  [Symbol.iterator]() {} // 有计算名称（computed name）的方法（此处为 symbol）
  // ...
}
```

技术上来说，`MyClass` 是一个函数（我们提供作为 `constructor` 的那个），而 methods、getters 和 setters 都被写入了 `MyClass.prototype`。

## 类的继承

扩展  

原理就是原型链

用extends 关键字 完成继承  ***<font color='cornflowerblue'>class Child extends Parent</font>*** 

假设我们有 class `Animal`：

```javascript
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
  run(speed) {
    this.speed = speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }
  stop() {
    this.speed = 0;
    alert(`${this.name} stands still.`);
  }
}

let animal = new Animal("My animal");
```

这是我们对对象 `animal` 和 class `Animal` 的图形化表示：

![image-20230323111219234](https://s2.loli.net/2023/05/27/odAbDpIwkc6sghx.png)



一个继承自 `Animal` 的 `class Rabbit`：

```javascript
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }
}

let rabbit = new Rabbit("White Rabbit");

rabbit.run(5); // White Rabbit runs with speed 5.
rabbit.hide(); // White Rabbit hides
```

![image-20230323112359340](https://s2.loli.net/2023/05/27/8IgjLUGqVDxSEnM.png)



类语法不仅允许指定一个类，在 `extends` 后可以指定任意表达式。

### 重写方法 super

希望在父类方法的基础上进行调整或扩展其功能

Class 为此提供了 `"super"` 关键字。

- 执行 `super.method(...)` 来调用一个**父类**方法。
- 执行 `super(...)` 来调用一个父类 constructor

实际上

`super.method = = this._proto_.method`

（**箭头函数没有** `super`）

### 重写 constructor

**继承类的 constructor 必须调用 `super(...)`，并且 (!) 一定要在使用 `this` 之前调用。**

给 `Rabbit` 添加一个自定义的 constructor。除了 `name` 之外，会指定 `earLength`。

```js
class Rabbit extends Animal {   		          		constructor(name, earLength) {    				*super(name);*    
		this.earLength = earLength;  
}
```

### Summary

1. 想要扩展一个类：`class Child extends Parent`：
   - 这意味着 `Child.prototype.__proto__` 将是 `Parent.prototype`，所以方法会被继承。
2. 重写一个 constructor：
   - 在使用 `this` 之前，我们必须在 `Child` 的 constructor 中将父 constructor 调用为 `super()`。
3. 重写一个方法：
   - 我们可以在一个 `Child` 方法中使用 `super.method()` 来调用 `Parent` 方法。
4. 内部：
   - 方法在内部的 `[[HomeObject]]` 属性中记住了它们的类/对象。这就是 `super` 如何解析父方法的。
   - 因此，将一个带有 `super` 的方法从一个对象复制到另一个对象是不安全的。

补充：

- 箭头函数没有自己的 `this` 或 `super`，所以它们能融入到就近的上下文中，像透明似的。



## 插入—解构赋值

- 解构赋值可以简洁地将一个对象或数组拆开赋值到多个变量上。

- 解构对象的完整语法：

  ```javascript
  let {prop : varName = default, ...rest} = object
  ```

  这表示属性 `prop` 会被赋值给变量 `varName`，如果没有这个属性的话，就会使用默认值 `default`。

  没有对应映射的对象属性会被复制到 `rest` 对象。

- 解构数组的完整语法：

  ```javascript
  let [item1 = default, item2, ...rest] = array
  ```

  数组的第一个元素被赋值给 `item1`，第二个元素被赋值给 `item2`，剩下的所有元素被复制到另一个数组 `rest`。

- 从嵌套数组/对象中提取数据也是可以的，此时等号左侧必须和等号右侧有相同的结构。



## 静态属性和方法

静态方法可以在类上调用，而不是在单个对象上：操作类

Object.asign 是静态方法

`extends` 让 `Rabbit` 的 `[[Prototype]]` 指向了 `Animal`。

![image-20230323150534890](https://s2.loli.net/2023/05/27/qvSbhNRJlMaVti7.png)

所以，`Rabbit extends Animal` 创建了两个 `[[Prototype]]` 引用：

1. `Rabbit` 函数原型继承自 `Animal` 函数。

   	`Rabbit.__proto__ === Animal`
		
   	**为了获取静态方法**

2. `Rabbit.prototype` 原型继承自 `Animal.prototype`。

	`Rabbit.prototype.__proto__ === Animal.prototype`		

		**为了获取实例方法**



### 总结

静态方法被用于实现属于整个类的功能。它与具体的类实例无关。

静态属性被用于当我们想要存储类级别的数据时，而不是绑定到实例。

语法如下所示：

```javascript
class MyClass {
  static property = ...;

  static method() {
    ...
  }
}
```

从技术上讲，静态声明与直接给类本身赋值相同：

```javascript
MyClass.property = ...
MyClass.method = ...
```

静态属性和方法是可被继承的。

对于 `class B extends A`，类 `B` 的 prototype 指向了 `A`：`B.[[Prototype]] = A`。因此，如果一个字段在 `B` 中没有找到，会继续在 `A` 中查找。

## 私有的和受保护的属性和方法

在面向对象的编程中，属性和方法分为两组：

- **内部接口** —— 可以通过该类的其他方法访问，但不能从外部访问的方法和属性。
- **外部接口** —— 也可以从类的外部访问的方法和属性。

在 JavaScript 中，有两种类型的对象字段（属性和方法）：

- 公共的：可从任何地方访问。它们构成了外部接口。

- 私有的：只能从类的内部访问。这些用于内部接口。

  

**受保护的属性通常以下划线 `_` 作为前缀**   意识层面，约定俗成，不是真正的禁止

**受保护的字段是可以被继承的**

在语言级别，`#` 是该字段为私有的特殊标志。我们无法从外部或从继承的类中访问它。

**私有字段不能通过 this[name] 访问**

为了隐藏内部接口，我们使用受保护的或私有的属性：

- 受保护的字段以 `_` 开头。这是一个众所周知的约定，不是在语言级别强制执行的。程序员应该只通过它的类和从它继承的类中访问以 `_` 开头的字段。
- 私有字段以 `#` 开头。JavaScript 确保我们只能从类的内部访问它们。

## 扩展内置类

内建的类，例如 `Array`，`Map` 等也都是可以扩展的（extendable）

内建类没有静态方法继承

## 类检查

`instanceof` 操作符用于检查一个对象是否属于某个特定的 class。同时，它还考虑了继承

根据 `instanceof` 的逻辑，真正决定类型的是 `prototype`，而不是构造函数。

语法：

```javascript
obj instanceof Class
```

如果 `obj` 隶属于 `Class` 类（或 `Class` 类的衍生类），则返回 `true`。

总结一下我们知道的类型检查方法：

|               | 用于                                                         | 返回值     |
| :------------ | :----------------------------------------------------------- | :--------- |
| `typeof`      | 原始数据类型                                                 | string     |
| `{}.toString` | 原始数据类型，内建对象，包含 `Symbol.toStringTag` 属性的对象 | string     |
| `instanceof`  | 对象                                                         | true/false |

## Mixins

mixin是一个包含可被其他类使用而无需继承的方法的类。

换句话说，*mixin* 提供了实现特定行为的方法，但是我们不单独使用它，而是使用它来将这些行为添加到其他类中   ——————  **解决多继承**（JavaScript不支持多继承的主要原因是以简单性和灵活性为代价的）

拷贝方法 Object.assign(User.prototype, sayHiMixin)
