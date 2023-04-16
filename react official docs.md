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



**事件处理函数是执行副作用的最佳位置**

与渲染函数不同，事件处理函数不需要是 [纯函数](https://zh-hans.reactjs.org/learn/keeping-components-pure)，因此它是用来 *更改* 某些值的绝佳位置。例如，更改输入框的值以响应键入，或者更改列表以响应按钮的触发。但是，为了更改某些信息，你首先需要某种方式存储它。在 React 中，这是通过 [state（组件的记忆）](https://zh-hans.reactjs.org/learn/state-a-components-memory) 来完成的。

## Render and Commit

- [`e.stopPropagation()`](https://developer.mozilla.org/docs/Web/API/Event/stopPropagation) 阻止触发绑定在外层标签上的事件处理函数。
- [`e.preventDefault()`](https://developer.mozilla.org/docs/Web/API/Event/preventDefault) 阻止少数事件的默认浏览器行为。

请求和提供 UI 的过程总共包括三个步骤：

1. **触发** 一次渲染（把客人的点单分发到厨房）
2. **渲染** 组件（在厨房准备订单）
3. **提交** 到 DOM（将菜品放在桌子上）

![image-20230414113219409](./notes.assets/image-20230414113219409.png)

**步骤 1:** 

有两种原因会导致组件的渲染:

1. 组件的 **初次渲染。**
2. 组件（或者其祖先之一）的 **状态发生了改变。**



一旦组件被初次渲染，就可以通过使用 [`set` 函数](https://zh-hans.reactjs.org/reference/react/useState#setstate) 更新其状态来触发之后的渲染。更新组件的状态会自动将一次渲染送入队列。（您可以想象这种情况成餐厅客人在第一次下单之后又点了茶、点心和各种东西，具体取决于他们的胃口。）

![image-20230414113339457](./notes.assets/image-20230414113339457.png)

**步骤 2: React 渲染组件** 

在触发渲染后，React 会调用组件来确定要在屏幕上显示的内容。**“渲染中” 即 React 在调用组件。**

- **在进行初次渲染时,** React 会调用根组件。
- **对于后续的渲染,** React 会调用内部状态更新触发了渲染的函数组件



渲染必须始终是一次 纯计算:

- **输入相同，输出相同。** 给定相同的输入，组件应始终返回相同的 JSX。（当有人点了西红柿沙拉时，他们不应该收到洋葱沙拉！）
- **只做它自己的事情。** 它不应更改任何存在于渲染之前的对象或变量。（一个订单不应更改其他任何人的订单。）
- 

**步骤 3: React 把更改提交到 DOM 上** 

在渲染（调用）组件之后，React 将会修改 DOM。

- **对于初次渲染，** React 会使用 [`appendChild()`](https://developer.mozilla.org/docs/Web/API/Node/appendChild) DOM API 将其创建的所有 DOM 节点放在屏幕上。
- **对于重渲染，** React 将应用最少的必要操作（在渲染时计算！），以使得 DOM 与最新的渲染输出相互匹配

**React 仅在渲染之间存在差异时才会更改 DOM 节点**

## State as a Snapshot

[“正在渲染”](https://zh-hans.reactjs.org/learn/render-and-commit#step-2-react-renders-your-components) 就意味着 React 正在调用你的组件——一个函数。你从该函数返回的 JSX 就像是 UI 的一张及时的快照。它的 props、事件处理函数和内部变量都是 **根据当前渲染时的 state** 被计算出来的。

当 React 重新渲染一个组件时：

1. React 会再次调用你的函数
2. 你的函数会返回新的 JSX 快照
3. React 会更新界面来匹配你返回的快照

![image-20230414141024875](./notes.assets/image-20230414141024875.png)

当 React 调用你的组件时，它会为特定的那一次渲染提供一张 state 快照。你的组件会在其 JSX 中返回一张包含一整套新的 props 和事件处理函数的 UI 快照 ，其中所有的值都是 **根据那一次渲染中 state 的值** 被计算出来的

![image-20230414141400329](./notes.assets/image-20230414141400329.png)

- 设置 state 请求一次新的渲染。
- React 将 state 存储在组件之外，就像在架子上一样。
- 当你调用 `useState` 时，React 会为你提供*该次渲染* 的一张 state 快照。
- 变量和事件处理函数不会在重渲染中“存活”。每个渲染都有自己的事件处理函数。
- 每个渲染（以及其中的函数）始终“看到”的是 React 提供给*这个* 渲染的 state 快照。
- 你可以在心中替换事件处理函数中的 state，类似于替换渲染的 JSX。
- 过去创建的事件处理函数拥有的是创建它们的那次渲染中的 state 值。



## Queueing a Series of State Updates





## Reacting to Input with State

**React 会等到事件处理函数中的 \*所有\* 代码都运行完毕再处理你的 state 更新**

just like 餐厅里帮你点菜的服务员。服务员不会在你说第一道菜的时候就跑到厨房！相反，他们会让你把菜点完，让你修改菜品，甚至会帮桌上的其他人点菜。

![image-20230414142109352](./notes.assets/image-20230414142109352.png)

- 设置 state 不会更改现有渲染中的变量，但会请求一次新的渲染。
- React 会在事件处理函数执行完成之后处理 state 更新。这被称为批处理。
- 要在一个事件中多次更新某些 state，你可以使用 `setNumber(n => n + 1)` 更新函数。



## Reacting to Input with State

在 **命令式编程** 中，以上的过程直接告诉你如何去实现交互。你必须去根据要发生的事情写一些明确的命令去操作 UI。对此有另一种理解方式，想象一下，当你坐在车里的某个人旁边，然后一步一步地告诉他该去哪。

![image-20230414142804752](./notes.assets/image-20230414142804752.png)

在 React 中，你不必直接去操作 UI —— 你不必直接启用、关闭、显示或隐藏组件。相反，你只需要 **声明你想要显示的内容，** React 就会通过计算得出该如何去更新 UI。想象一下，当你上了一辆出租车并且告诉司机你想去哪，而不是事无巨细地告诉他该如何走。将你带到目的地是司机的工作，他们甚至可能知道一些你没有想过并且不知道的捷径！

![image-20230414142943376](./notes.assets/image-20230414142943376.png)

1. **定位**你的组件中不同的视图状态
2. **确定**是什么触发了这些 state 的改变
3. **表示**内存中的 state（需要使用 `useState`）
4. **删除**任何不必要的 state 变量
5. **连接**事件处理函数去设置 state



## Choosing the State Structure 

1. **将相关的状态分组**。如果你总是同时更新两个或更多的状态变量，考虑将它们合并成一个状态变量。
2. **避免状态的矛盾**。当状态的结构是几块状态可能相互矛盾和 "不一致 "的时候，你就为错误留下了空间。尽量避免这种情况。
3. **避免多余的状态**。如果你可以在渲染过程中从组件的道具或其现有的状态变量中计算出一些信息，你不应该把这些信息放到该组件的状态中。
4. **避免状态的重复**。当相同的数据在多个状态变量之间或在嵌套对象中重复时，就很难保持它们的同步。尽可能地减少重复。
5. **避免深度嵌套的状态**。深度分层的状态在更新时不是很方便。在可能的情况下，最好以扁平的方式来构造状态。



## Preserving and Resetting State

React 使用树形结构来对你创造的 UI 进行管理和建模。React 根据你的 JSX 生成 **UI 树**。React DOM 根据 UI 树去更新浏览器的 DOM 元素。（React Native 则将这些 UI 树转译成移动平台上特有的元素。）

![image-20230414150438895](./notes.assets/image-20230414150438895.png)

- 只要在相同位置渲染的是相同组件， React 就会保留状态。
- state 不会被保存在 JSX 标签里。它与你在树中放置该 JSX 的位置相关联。
- 你可以通过为一个子树指定一个不同的 key 来重置它的 state。
- 不要嵌套组件的定义，否则你会意外地导致 state 被重置。

## Extracting State Logic into a Reducer

**迁移状态逻辑至 Reducer 中**

对于拥有许多状态更新逻辑的组件来说，过于分散的事件处理程序可能会令人不知所措。对于这种情况，你可以将组件的所有**状态更新逻辑整合到一个外部函数中**，这个函数叫作 **reducer**。

### **使用 reducer 整合状态逻辑** 

为了降低状态逻辑的复杂度，并让所有逻辑都可以存放在一个易于理解的地方，你可以将这些状态逻辑移到组件之外的一个称为 **reducer** 的**函数**中。//非必须，state可以完成

随着组件复杂度的增加，你将很难一眼看清所有的组件状态更新逻辑

Reducer 是处理状态的另一种方式。你可以通过三个步骤将 `useState` 迁移到 `useReducer`：

1. 将设置状态的逻辑 **修改** 成 dispatch 的一个 action；
2. **编写** 一个 reducer 函数；
3. 在你的组件中 **使用** reducer。

使用 reducers 管理状态与直接设置状态略有不同。它不是通过设置状态来告诉 React “要做什么”，**而是通过事件处理程序 dispatch 一个 “action” 来指明 “用户刚刚做了什么”**。（而状态更新逻辑则保存在其他地方）

#### 将设置状态的逻辑 **修改** 成 dispatch 的一个 action

传递给 `dispatch` 的对象叫做 “action”：是一个普通的 JavaScript 对象。它的结构是由你决定的，但通常来说，它应该至少包含可以表明 *发生了什么事情* 的信息

action 对象可以有多种结构。

按照惯例，我们通常会添加一个字符串类型的 **`type` 字段来描述发生了什么**，并通过其它字段传递额外的信息。`type` 是特定于组件的，在这个例子中 `added` 和 `addded_task` 都可以。选一个能描述清楚发生的事件的名字！

```jsx
dispatch({

  // 针对特定的组件

  type: 'what_happened',

  // 其它字段放这里

});
```

#### **编写** 一个 reducer 函数

reducer 函数就是你放置状态逻辑的地方。接受两个参数，分别为当前 state 和 action 对象，并且**返回的是更新后的 state**：

```jsx
function yourReducer(state, action) {

  // 给 React 返回更新后的状态

}
```

React 会将状态设置为你从 reducer 返回的状态。

要将状态设置逻辑从事件处理程序移到 reducer 函数中，你需要：

1. 声明当前状态（`tasks`）作为第一个参数；
2. 声明 `action` 对象作为第二个参数；
3. 从 `reducer` 返回 **下一个** 状态（React 会将旧的状态设置为这个最新的状态）。

```jsx
function tasksReducer(tasks, action) {
  switch (action.type) {
    case '1': {
      return 
        ...
      ;
    }
    case '2': {
      return 
        ...
    }
    case '3': {
      return 
        ...
    }
    default: {
      throw Error('未知 action: ' + action.type);
    }
  }
}
```

建议将每个 `case` 块包装到 `{` 和 `}` 花括号中，这样在不同 `case` 中声明的变量就不会互相冲突。

#### 组件中 **使用** reducer

导入`useReducer` Hook

```jsx
import { useReducer } from 'react';
```

替换掉之前的 `useState`:

```
const [tasks, setTasks] = useState(initialTasks);
```

只需要像下面这样使用 `useReducer`:

```
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```

### 对比 `useState` 和 `useReducer` 

Reducers 并非没有缺点！以下是比较它们的几种方法：

- **代码体积：** 通常，在使用 `useState` 时，一开始只需要编写少量代码。而 `useReducer` 必须提前编写 reducer 函数和需要调度的 actions。但是，当多个事件处理程序以相似的方式修改 state 时，`useReducer` 可以减少代码量。
- **可读性：** 当状态更新逻辑足够简单时，`useState` 的可读性还行。但是，一旦逻辑变得复杂起来，它们会使组件变得臃肿且难以阅读。在这种情况下，**`useReducer` 允许你将状态更新逻辑与事件处理程序分离开来**。
- **可调试性：** 当使用 `useState` 出现问题时, 你很难发现具体原因以及为什么。 而使用 `useReducer` 时， 你可以在 reducer 函数中通过打印日志的方式来观察每个状态的更新，以及为什么要更新（来自哪个 `action`）。 如果所有 `action` 都没问题，你就知道问题出在了 reducer 本身的逻辑中。 然而，与使用 `useState` 相比，你必须单步执行更多的代码。
- **可测试性：** reducer 是一个不依赖于组件的纯函数。这就意味着你可以单独对它进行测试。一般来说，我们最好是在真实环境中测试组件，但对于复杂的状态更新逻辑，针对特定的初始状态和 `action`，断言 reducer 返回的特定状态会很有帮助。
- **个人偏好：** 并不是所有人都喜欢用 reducer，没关系，这是个人偏好问题。你可以随时在 `useState` 和 `useReducer` 之间切换，它们能做的事情是一样的！



**best practice**

- **reducers 必须是纯粹的**
- **每个 action 都描述了一个单一的用户交互，即使它会引发数据的多个变化**

### 使用 Immer 简化 reducers 

Reducers 应该是纯净的，所以它们不应该去修改 state。而 Immer 为你提供了一种特殊的 `draft` 对象，你可以通过它安全的修改 state。在底层，Immer 会基于当前 state 创建一个副本

### 摘要

- 把`useState`转化为`useReducer`
  1. 通过事件处理函数 dispatch actions；
  2. 编写一个 reducer 函数，它接受传入的 state 和一个 action，并返回一个新的 state；
  3. 使用 `useReducer` 替换 `useState`；
- Reducers 可能需要你写更多的代码，但是这有利于代码的调试和测试。
- Reducers 必须是纯净的。
- 每个 action 都描述了一个单一的用户交互。
- 使用 Immer 来帮助你在 reducer 里直接修改状态。