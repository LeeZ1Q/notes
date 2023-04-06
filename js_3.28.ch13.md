# Modules  模块

## 简介

es6 引入export、import

html   加  `<script type="module">` 

npm + node 要在 packge.json 里  加 ` "type"："module"`

 永远启用 严格模式

**模块只通过 HTTP(s) 工作，而非本地**

如果你尝试通过 `file://` 协议在本地打开一个网页，你会发现 `import/export` 指令不起作用。你可以使用本地 Web 服务器，例如 static-server，或者使用编辑器的“实时服务器”功能，例如 VS Code 的 Live Server Extension 来测试模块。

### 模块代码仅在第一次导入时被解析

模块可以提供需要配置的通用功能。例如身份验证需要凭证。那么模块可以导出一个配置对象，期望外部代码可以对其进行赋值。这是经典的使用模式：

1. 模块导出一些配置方法，例如一个配置对象。
2. 在第一次导入时，我们对其进行初始化，写入其属性。可以在应用顶级脚本中进行此操作。
3. 进一步地导入使用模块。

### 模块脚本是延迟的

- 下载外部模块脚本 `<script type="module" src="...">` 不会阻塞 HTML 的处理，它们会与其他资源并行加载。
- 模块脚本会等到 HTML 文档完全准备就绪（即使它们很小并且比 HTML 加载速度更快），然后才会运行。
- 保持脚本的相对顺序：在文档中排在前面的脚本先执行。

### Async 适用于内联脚本（inline script）

异步脚本会在准备好后立即运行，独立于其他脚本或 HTML 文档。

对于模块脚本，它也适用于内联脚本。

例如，下面的内联脚本具有 `async` 特性，因此它不会等待任何东西。

它执行导入（fetch `./analytics.js`），并在导入完成时运行，即使 HTML 文档还未完成，或者其他脚本仍在等待处理中。

这对于不依赖任何其他东西的功能来说是非常棒的，例如计数器，广告，文档级事件监听器。

```html
<!-- 所有依赖都获取完成（analytics.js）然后脚本开始运行 -->
<!-- 不会等待 HTML 文档或者其他 <script> 标签 -->
<script async type="module">
  import {counter} from './analytics.js';

  counter.count();
</script>
```

### 外部脚本

1. 具有相同 `src` 的外部脚本仅运行一次：

   ```markup
   <!-- 脚本 my.js 被加载完成（fetched）并只被运行一次 -->
   <script type="module" src="my.js"></script>
   <script type="module" src="my.js"></script>
   ```

​	2.从另一个源（例如另一个网站）获取的外部脚本需要 CORSheader。换句话说，如果一个模块脚本是从另一个源获取的，则远程服务器必须提供表示允许获取的 header `Access-Control-Allow-Origin`。

```markup
<!-- another-site.com 必须提供 Access-Control-Allow-Origin -->
<!-- 否则，脚本将无法执行 -->
<script type="module" src="http://another-site.com/their.js"></script>
```

### 总结

下面总结一下模块的核心概念：

1. 一个模块就是一个文件。浏览器需要使用`<script type="module">以使import/export`

    可以工作。模块（译注：相较于常规脚本）有几点差别：

   - 默认是延迟解析的（deferred）。
   - Async 可用于内联脚本。
   - 要从另一个源（域/协议/端口）加载外部脚本，需要 CORS header。
   - 重复的外部脚本会被忽略

2. 模块具有自己的本地顶级作用域，并可以通过 `import/export` 交换功能。

3. 模块始终使用 `use strict`。

4. 模块代码只执行一次。导出仅创建一次，然后会在导入之间共享。

当我们使用模块时，每个模块都会实现特定功能并将其导出。然后我们使用 `import` 将其直接导入到需要的地方即可。浏览器会自动加载并解析脚本。

在生产环境中，出于性能和其他原因，开发者经常使用诸如 **Webpack** 之类的打包工具将模块打包到一起。

## 导出和导入

### 在声明前导出

例如，这里的所有导出均有效：

```javascript
// 导出数组
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

// 导出 const 声明的变量
export const MODULES_BECAME_STANDARD_YEAR = 2015;

// 导出类
export class User {
  constructor(name) {
    this.name = name;
  }
}
```

**导出 class/function 后没有分号**

### 导出与声明分开

我们还可以将 `export` 分开放置。

下面的例子中，我们先声明函数，然后再导出它们：

```javascript
// 📁 say.js
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

function sayBye(user) {
  alert(`Bye, ${user}!`);
}

export {sayHi, sayBye}; // 导出变量列表
```

### Import*

一般来说 明确列出所需要导入的内容，这里有几个原因。

1. 现代的构建工具（webpack和其他工具）将模块打包到一起并对其进行优化，以加快加载速度并删除未使用的代码。
2. 明确列出要导入的内容会使得名称较短：`sayHi()` 而不是 `say.sayHi()`。
3. 导入的显式列表可以更好地概述代码结构：使用的内容和位置。它使得代码支持重构，并且重构起来更容易。

`as` 相当于重命名

### 重新导出

文件结构可能是这样的：

```none
auth/
    index.js
    user.js
    helpers.js
    tests/
        login.js
    providers/
        github.js
        facebook.js
        ...
```

我们希望通过单个入口暴露包的功能。

换句话说，想要使用我们的包的人，应该只从“主文件” `auth/index.js` 导入。

**在 `{...}` 中的 import/export 语句无效**

## 动态导入

等异步看完来