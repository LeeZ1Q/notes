

# try...catch

## 语法

`try...catch` 结构由两部分组成：`try` 和 `catch`：

```javascript
try {
  // 代码...
} catch (err) {
  // 错误捕获
}
```

`try...catch` **仅对运行时的 error 有效**(只能处理**有效**代码中出现的错误)

`try...catch` **同步执行**

为了捕获到计划的（scheduled）函数中的异常，那么 `try...catch` 必须在这个函数内(eg.setTimeout)

```javascript
setTimeout(function() {
  try {
    noSuchVariable; // try...catch 处理 error 了！
  } catch {
    alert( "error 被在这里捕获了！" );
  }
}, 1000);
```

## Error 对象

对于所有内建的 error，error 对象具有两个主要属性：

- `name`

  Error 名称。例如，对于一个未定义的变量，名称是 `"ReferenceError"`。

- `message`

  关于 error 的详细文字描述。

还有其他非标准的属性在大多数环境中可用。其中被最广泛使用和支持的是：

- `stack`

  当前的调用栈：用于调试目的的一个字符串，其中包含有关导致 error 的嵌套调用序列的信息。

例如：

```javascript
try {
  lalala; // error, variable is not defined!
} catch (err) {
  alert(err.name); // ReferenceError
  alert(err.message); // lalala is not defined
  alert(err.stack); // ReferenceError: lalala is not defined at (...call stack)

  // 也可以将一个 error 作为整体显示出来
  // error 信息被转换为像 "name: message" 这样的字符串
  alert(err); // ReferenceError: lalala is not defined
}
```

## throw 

我们抛出这个 error。

```javascript
let json = '{ "age": 30 }'; // 不完整的数据

try {

  let user = JSON.parse(json); // <-- 没有 error

  if (!user.name) {
    throw new SyntaxError("数据不全：没有 name"); // (*)
  }

  alert( user.name );

} catch(err) {
  alert( "JSON Error: " + err.message ); // JSON Error: 数据不全：没有 name
}
```

在 `(*)` 标记的这一行，`throw` 操作符生成了包含着我们所给定的 `message` 的 `SyntaxError`，与 JavaScript 自己生成的方式相同。`try` 的执行立即停止，控制流转向 `catch` 块

## rethrowing

**`catch` 应该只处理它知道的 error，并“抛出”所有其他 error。**

“再次抛出（rethrowing）”技术可以被更详细地解释为：

1. Catch 捕获所有 error。
2. 在 `catch (err) {...}` 块中，我们对 error 对象 `err` 进行分析。
3. 如果我们不知道如何处理它，那我们就 `throw err`。

在下面的代码中，我们使用“再次抛出”，以达到在 `catch` 中只处理 `SyntaxError` 的目的：

```javascript
let json = '{ "age": 30 }'; // 不完整的数据
try {

  let user = JSON.parse(json);

  if (!user.name) {
    throw new SyntaxError("数据不全：没有 name");
  }

  blabla(); // 预料之外的 error

  alert( user.name );

} catch (err) {

  if (err instanceof SyntaxError) {
    alert( "JSON Error: " + err.message );
  } else {
    throw err; // 再次抛出 (*)
  }

}
```

如果 `(*)` 标记的这行 `catch` 块中的 error 从 `try...catch` 中“掉了出来”，那么它也可以被外部的 `try...catch` 结构（如果存在）捕获到，如果外部不存在这种结构，那么脚本就会被杀死。

所以，`catch` 块实际上只处理它知道该如何处理的 error，并“跳过”所有其他的 error。

## try…catch…finally

`finally` 子句（clause）通常用在：当我们开始做某事的时候，希望无论出现什么情况都要完成完成某个任务

在 `try` 中有一个 `return`。在这种情况下，`finally` 会在控制转向外部代码前被执行

```javascript
function func() {

  try {
    return 1;

  } catch (err) {
    /* ... */
  } finally {
    alert( 'finally' );
  }
}

alert( func() ); // 先执行 finally 中的 alert，然后执行这个 alert
```

## 全局Catch

在 `try...catch` 结构外有一个致命的 error，然后脚本死亡了。

我们可能想要记录这个 error，并向用户显示某些内容（通常用户看不到错误信息）等。

规范中没有相关内容，但是代码的执行环境一般会提供这种机制，因为它确实很有用。例如，Node.JS 有 [`process.on("uncaughtException")`](https://nodejs.org/api/process.html#process_event_uncaughtexception)。在浏览器中，我们可以将一个函数赋值给特殊的 [window.onerror](https://developer.mozilla.org/zh/docs/Web/API/GlobalEventHandlers/onerror) 属性，该函数将在发生未捕获的 error 时执行。

语法如下：

```javascript
window.onerror = function(message, url, line, col, error) {
  // ...
};
```

## 总结

`try...catch` 结构允许我们处理执行过程中出现的 error。从字面上看，它允许“尝试”运行代码并“捕获”其中可能发生的 error。

语法如下：

```javascript
try {
  // 执行此处代码
} catch (err) {
  // 如果发生 error，跳转至此处
  // err 是一个 error 对象
} finally {
  // 无论怎样都会在 try/catch 之后执行
}
```

这儿可能会没有 `catch` 或者没有 `finally`，所以 `try...catch` 或 `try...finally` 都是可用的。

Error 对象包含下列属性：

- `message`  ——人类可读的 error 信息。
- `name` —— 具有 error 名称的字符串（Error 构造器的名称）。
- `stack`（没有标准，但得到了很好的支持）—— Error 发生时的调用栈。

如果我们不需要 error 对象，我们可以通过使用 `catch {` 而不是 `catch (err) {` 来省略它。

我们也可以使用 `throw` 操作符来生成自定义的 error。从技术上讲，`throw` 的参数可以是任何东西，但通常是继承自内建的 `Error` 类的 error 对象。下一章我们会详细介绍扩展 error。

再次抛出（rethrowing）是一种错误处理的重要模式：`catch` 块通常期望并知道如何处理特定的 error 类型，因此它应该再次抛出它不知道的 error。

即使我们没有 `try...catch`，大多数执行环境也允许我们设置“全局” error 处理程序来捕获“掉出（fall out）”的 error。在浏览器中，就是 `window.onerror`。