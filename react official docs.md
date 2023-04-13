## Quick Start

**React 组件**是返回标签的 **JavaScript 函数**

```jsx
//wsrv.nl/?url=原始链接
```



**条件渲染**：

`if else` or 条件运算符 `condition ? exprIfTrue : exprIfFalse`

通常使用 `map`来渲染组件列表



**响应事件**

`onClick={handleClick}` 的结尾没有小括号。不要 **调用** 事件处理函数：只需 **传递给事件** 即可。当用户点击按钮时，React 会调用事件处理函数`handleClick`



## Thinkin in React

step1.将UI拆解为组件层级结构

step2.构建静态版本

step3.精简且完整的state

- 随着时间推移 **保持不变**？ 如此，便不是 state。
- 通过 props **从父组件传递**？ 如此，便不是 state。
- 是否可以基于已存在于组件中的 state 或者 props **进行计算**？ 如此，它*肯定*不是state!

- **Props** 像是你传递的参数至函数。它们使父组件可以传递数据给子组件，定制它们的展示。
- **State** 像是组件的内存。它使组件可以对一些信息保持追踪，并根据交互来改变。

Props 和 state 是不同的，但它们可以共同工作。父组件将经常在 state 中放置一些信息(以便它可以改变)，并且作为子组件的属性*向下*传递至它的子组件

step4.state放哪儿

1. 通常情况下，可以直接放置 state 于它们共同的父组件。
2. 也可以将 state 放置于它们父组件上层的组件。
3. 如果找不到一个有意义拥有这个 state 的地方，单独创建一个新的组件去管理这个 state，并将它添加到它们父组件上层的某个地方。

step5.添加反向数据流

`useState` `onChange`



## JSX规则

- 需要一个父标签包裹起来

- 必须闭合

- 驼峰式命名

  

### 哪里使用大括号 

在 JSX 中，只能在以下两种场景中使用大括号：

1. 用作 JSX 标签内的**文本**：`<h1>{name}'s To Do List</h1>` 是有效的，但是 `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` 无效。
2. 用作紧跟在 `=` 符号后的 **属性**：`src={avatar}` 会读取 `avatar` 变量，但是 `src="{avatar}"` 只会传一个字符串 `{avatar}`。



内联 `style` 属性 使用驼峰命名法编写



- JSX 引号内的值会作为字符串传递给属性。
- 大括号让你可以将 JavaScript 的逻辑和变量带入到标签中。
- 它们会在 JSX 标签中的内容区域或紧随属性的 `=` 后起作用。
- `{{` 和 `}}` 并不是什么特殊的语法：**它只是**包在 JSX 大括号内的 **JavaScript 对象**





### props

- 要传递 props，请将它们添加到 JSX，就像使用 HTML 属性一样。
- 要读取 props，请使用 `function Avatar({ person, size })` 解构语法。
- 你可以指定一个默认值，如 `size = 100`，用于缺少值或值为 `undefined` 的 props 。
- 你可以使用 `<Avatar {...props} />` JSX 展开语法转发所有 props，但不要过度使用它！
- 像 `<Card><Avatar /></Card>` 这样的嵌套 JSX，将被视为 `Card` 组件的 `children` prop。
- Props 是只读的时间快照：每次渲染都会收到新版本的 props。
- 你不能改变 props。当你需要交互性时，你可以设置 state。



## Keeping Components Pure

**纯函数**通常具有如下特征：

- **只负责自己的任务**。它不会更改在该函数调用前，就已存在的对象或变量。
- **输入相同，则输出相同**。给定相同的输入，纯函数应总是返回相同的结果。

**React 假设你编写的所有组件都是纯函数**

在严格模式下开发时，它将会调用每个组件函数两次。**通过重复调用组件函数，严格模式有助于找到违反这些规则的组件**。

## Render and Commit





## State as a Snapshot





## Queueing a Series of State Updates





## Reacting to Input with State





## Choosing the State Structure 





## Preserving and Resetting State