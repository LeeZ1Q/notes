---
title: Redux official docs-notes

date: 2023-04-017

---

# Redux 深入浅出

## 基础概念

JS 应用的状态容器，提供可预测的状态管理

可预测、集中管理、可调试、灵活

#### what

**Redux 是一个使用 “action” 的事件来管理和更新应用状态的模式和工具库**

它以集中式 Store（centralized store）的方式对整个应用中使用的状态进行**集中管理**，其规则确保状态只能以**可预测**的方式更新。

#### why

**Redux 提供的模式和工具使你更容易理解应用程序中的状态何时、何地、为什么、state 如何被更新，以及当这些更改发生时你的应用程序逻辑将如何表现**. 

#### when

- 应用中有很多 state 在多个组件中需要使用
- 应用 state 会随着时间的推移而频繁更新
- 更新 state 的逻辑很复杂
- 中型和大型代码量的应用，很多人协同开发

### Redux Store

所有 Redux 应用的中心都是 **store** 。"store" 是保存应用程序的全局 **state** 的容器。

store 是一个 JavaScript 对象，具有一些特殊的功能和能力，使其与普通的全局对象不同：

- 切勿直接修改（modify）或更改（change）保存在 Redux 存储中的状态
- 相反，导致状态更新的唯一方法是创建一个描述“应用程序中发生的某些事情”的普通 **action** 对象，然后将该 action **dispatch** 到 store 以告诉它发生了什么。
- 当一个 action 被 dispatch 后，store 会调用根 **reducer** 方法，让其根据 action 和旧 state 计算出新 state
- 最后，store 会通知 **订阅者(subscribers)** 状态已更新，以便可以使用新数据更新 UI。

### Example

#### State, Actions 和 Reducers

1. 首先定义一个初始 **state** 值来描述应用程序

2. 接着，我们定义一个 **reducer** 方法。 接收俩参数， 当前的 `state` 和一个描述发生了什么的 `action` 对象。 当 Redux 应用启动时，我们还没有任何状态，所以我们提供一个 ==`initialState` 作为该 reducer 的默认值==。

3. 通过调用 Redux 库 `createStore` API 来创建一个 **store** 实例。==将 reducer 函数传递给 "createStore"，它使用 reducer 函数生成初始状态，并计算任何未来的更新==。

4. 编写一个 render 函数，该函数知道使用 `store.getState()` 方法从 Redux store 中获取最新状态，然后获取该值并更新 UI 以显示它。

   Redux store 允许我们调用 `store.subscribe()` 方法，并传递一个订阅者回调函数，该函数将在每次更新 store 时调用。==因此，我们可以将 `render` 函数作为订阅者传递，并且知道每次 store 更新时，我们都可以使用最新值更新 UI==。

#### Dispatching Actions

最后，我们需要通过创建描述所发生情况的 **action** 对象，并将其 **dispatching** 到 store 来响应用户输入。当我们调用 `store.dispatch(action)` 时，store 运行 reducer ，计算更新的状态，并执行订阅者来更新 UI。

### 数据流

- actions 会在用户交互如点击时被 dispatch
- store 通过执行 reducer 方法计算出一个新的 state
- UI 读取最新的 state 来展示最新的值

![Redux data flow diagram](https://s2.loli.net/2023/05/27/bWZidqsx8He2PVF.gif)

### 总结

- Redux 是一个用于管理应用程序中全局状态的库
  - Redux 通常与 React-Redux 库一起使用，用于将 Redux 和 React 集成在一起
  - Redux Toolkit 是编写 Redux 逻辑的推荐方式
- Redux 使用多种类型的代码
  - *Actions* 是一个有 `type` 字段的普通对象，描述应用程序中“发生了什么”
  - *Reducers* 是基于旧 state 和 action 计算出新 state 的方法
  - 每当一个 action 被 *dispatch* 时，Redux *store* 都会执行根（root）reducer

## 概念与数据流

### State管理

![One-way data flow](https://s2.loli.net/2023/05/27/hGx82yXuPKLVtE1.png)

单向数据流：

- State 描述了应用程序在特定时间点的状况
- 基于 state 来渲染 UI
- 发生某些事情时（例如用户单击按钮），state 会根据发生的事情进行更新
- 基于新的 state 重新渲染 UI

状态提升不一定有用

Redux 的**基本思想**：应用中**使用集中式的全局状态来管理，并明确更新状态的模式，以便让代码具有可预测性。**

### 不可变性 Immutability

**如果想要不可变的方式来更新，代码必需先 *复制*  原来的 object/array，然后更新它的复制体**。

**Redux 期望所有状态更新都是使用不可变的方式**

### 核心概念和原则

- **单一数据源**

  应用程序的**全局状态**作为对象存储在单个 **store** 中。任何给定的数据片段都应仅存在于一个位置，而不是在许多位置重复。

  这并 *不*  意味着应用中的 *所有*  状态都必须放进 Redux store 管理！你应该根据需要的位置来决定一段状态是属于 Redux 还是属于你的 UI 组件

- **State 是只读的**

  更改状态的唯一方法是 dispatch 一个 **action**，这是一个描述所发生事件的对象

  这样，UI 就不会意外覆盖数据，并且更容易跟踪发生状态更新的原因。

- **使用 Reducer 纯函数进行更改**

  若要指定如何基于 action 更新状态树，请编写 **reducer** 函数。Reducers 是**纯函数**，它接收旧 state 和 action，并返回新 state。

### 数据流

- 初始启动：
  - 使用最顶层的 root reducer 函数创建 Redux store
  - store 调用一次 root reducer，并将返回值保存为它的初始 `state`
  - 当 UI 首次渲染时，UI 组件访问 Redux store 的当前 state，并使用该数据来决定要呈现的内容。同时监听 store 的更新，以便他们可以知道 state 是否已更改。
- 更新环节：
  - 应用程序中发生了某些事情，例如用户单击按钮
  - dispatch 一个 action 到 Redux store，例如 `dispatch({type: 'counter/increment'})`
  - store 用之前的 `state` 和当前的 `action` 再次运行 reducer 函数，并将返回值保存为新的 `state`
  - store 通知所有订阅过的 UI，通知它们 store 发生更新
  - 每个订阅过 store 数据的 UI 组件都会检查它们需要的 state 部分是否被更新。
  - 发现数据被更新的每个组件都强制使用新数据重新渲染，紧接着更新网页

### 总结

- Redux 的意图可以总结为三个原则
  - 全局应用状态保存在单个 store 中
  - store 中的 state 是只读的
  - Reducer 函数用于更新状态以响应 actions
- Redux 使用“单向数据流”
  - State 描述了应用程序在某个时间点的状态，UI 基于该状态渲染
  - 当应用程序中发生某些事情时：
    - UI dispatch 一个 action
    - store 调用 reducer，随后根据发生的事情来更新 state
    - store 通知 UI state 发生了变化
  - UI 基于新 state 重新渲染

## State， Actions 和 Reducers

- Redux 应用程序使用普通的 JS 对象、数组和 primitives 作为状态值
  - 根状态值应该是一个普通的 JS 对象
  - 状态应该包含使应用程序工作所需的最少数据量
  - 类、Promises、函数和其他非普通值不应进入 Redux 状态
  - redux 不得创建随机值，例如 `Math.random()` 或 `Date.now()`
  - 可以将其他不在 Redux 存储中的状态值（如本地组件状态）与 Redux 并排放置
- actions 是带有 `type` 并且描述发生了什么的普通对象
  - 该 `type` 字段应该是一个可读的字符串，通常写成 `'feature/eventName'`
  - 动作可能包含其他值，这些值通常存储在 `action.payload` 字段中
  - 操作应该包含描述发生的事情所需的最少数据量
- Reducers 是看起来像的函数 `(state, action) => newState`
  - Reducers 必须始终遵循特殊规则：
    - 仅根据 `state` 和 `action` 参数计算新状态
    - 永远不要改变现有的 `state` - 总是返回一个副本
    - 不要有像 AJAX 调用或异步逻辑这样的“副作用”
- Reducers 应该被==拆分==以使它们更易于阅读
  - Reducer 通常根据顶级状态键或状态“ slices ”进行拆分
  - Reducers 通常写在“ slice ”文件中，组织成“ feature ”文件夹
  - Reducers 可以与 Redux `combineReducers` 函数结合使用
  - 用于 `combineReducers` 定义顶级状态对象键的键名

## Store

store 有以下几个职责:

- 在内部保存当前应用程序 state
- 通过 [`store.getState()`](https://cn.redux.js.org/api/store#getState) 访问当前 state;
- 通过 [`store.dispatch(action)`](https://cn.redux.js.org/api/store#dispatch) 更新状态;
- 通过 [`store.subscribe(listener)`](https://cn.redux.js.org/api/store#subscribe) 注册监听器回调;
- 通过 [`store.subscribe(listener)`](https://cn.redux.js.org/api/store#subscribe) 返回的 `unsubscribe` 函数注销监听器

 **Redux 应用程序中只有一个 store**

每次我们调用 `store.dispatch(action)` 时：

- store 调用`rootReducer(state, action)`
  - 该根 reducer 可能会在其内部调用其他的 slice reducers，就像 `todosReducer(state.todos, action)`
- store 将*新的* state 保存在里面
- store 调用所有的监听器订阅回调
- 监听器现在通过调用 `store.getState()` 来访问 `store` 并读取最新的 state

### Redux Store 内部

一个关于 Redux Store 实现的简化示例，大约 25 行代码：

miniReduxStoreExample.js

```js
function createStore(reducer, preloadedState) {
  let state = preloadedState
  const listeners = []

  function getState() {
    return state
  }

  function subscribe(listener) {
    listeners.push(listener)
    return function unsubscribe() {
      const index = listeners.indexOf(listener)
      listeners.splice(index, 1)
    }
  }

  function dispatch(action) {
    state = reducer(state, action)
    listeners.forEach(listener => listener())
  }

  dispatch({ type: '@@redux/INIT' })

  return { dispatch, subscribe, getState }
}
```

这个小版本的 Redux store 运行良好，你可以使用这个自己编写的 `createStore` 函数替换实际的 Redux `createStore` 函数（实际的 Redux 存储实现更长，更复杂，==但是其中大部分是评论信息、警告信息和一些极端情况的处理==）。

如你所见，这里的实际逻辑相当短：

- store 内部有当前的 `state` 值和 `reducer` 函数
- `getState` 返回当前 state 值
- `subscribe` 保存一个监听回调数组并返回一个函数来移除新的回调
- `dispatch` 调用 reducer，保存 state，并运行监听器
- store 在启动时 dispatch 一个 action 来初始化 reducers 的 state
- store API 是一个对象，里面有 `{dispatch, subscribe, getState}`

特别强调其中之一：注意 `getState` 只返回当前的 `state` 值。这意味着**默认情况下，没有什么可以阻止你意外改变当前 state 值**

换句话说：

- 当你调用 `getState()` 时，Redux store 不会产生 `state` 值的额外副本。它与根 reducer 函数返回的引用完全相同。
- Redux store 对于意外更改没有做任何防护，我们可以在 reducer 内部或者 store 外部改变状态，所以必须小心避免意外更改。

无意中发生变动的一个常见原因是对数组进行排序。[**调用 `array.sort()` 实际上会改变现有数组**](https://doesitmutate.xyz/sort/)。

### 配置 Store

Redux store 是使用一种叫做 **store enhancer** 的东西来定制的。store enhancer 就像一个特殊版本的“ createStore ”，它添加了另一个包裹原始 Redux store 的层。然后，增强的 store 可以通过提供其自定义 store 的 `dispatch`、`getState` 和 `subscribe` 函数而不是原始版本来改变 store 的行为方式。

**Redux 核心包含一个 `compose` 函数，可用于将多个 enhancer 合并在一起**

### Middleware

Redux 使用一种称为 **middleware** 的特殊插件来让我们自定义 `dispatch` 函数。

**Redux middleware 在 dispatch action 和到达 reducer 之间提供第三方扩展点**

。Redux Middleware 实际上是在 Redux 内置的一个非常特殊的 store enhancer 之上实现的，称为 **`applyMiddleware`**

**Middleware 围绕 store 的 `dispatch` 方法形成pipeline**

==*不像*  reducer，**middleware 内部可能有副作用**，包括超时和其他异步逻辑==

我们可以用中间件做很多事！

当一个 middleware 遇到 dispatch 一个 action 时，它可以做到任何想做的事：

- 将某些内容记录到控制台
- 设置定时
- 进行异步 API 调用
- 修改 action
- 暂停 action，甚至完全停止

以及你能想到的任何其他事情。

特别的是，**middleware旨在包含具有副作用的逻辑**。此外，**middleware 可以修改 `dispatch` 来接受 不是普通 action 对象**的东西

### 总结

- Redux 应用程序始终只有一个 store
  - 使用 Redux `createStore` API 创建 store
  - 每个 store 都有一个独立的根 reducer 方法
- Stores 主要有三种方法
  - `getState` 返回当前 state
  - `dispatch` 向 reducer 发送一个 action 来更新 state
  - `subscribe` 接受一个监听器回调，该回调在每次 dispatch action 时运行
- Store enhancers 让我们能够在创建 store 时进行自定义操作
  - Enhancers 包装了 store 并且可以覆盖它的方法
  - `createStore` 接受一个 enhancer 作为参数
  - 可以使用 `compose` API 将多个 enhancers 合并在一起
- Middleware 是自定义 store 的主要方式
  - 使用 `applyMiddleware` enhancer 添加 middleware
  - Middleware 被写成三个相互嵌套的函数
  - 每次 dispatch action 时都会运行 middleware
  - Middleware 内部可能有副作用
- Redux DevTools 可让你查看应用程序随时间发生的变化
  - DevTools 扩展可以安装在你的浏览器中
  - Store 需要添加 DevTools enhancer，使用 `composeWithDevTools`
  - DevTools 显示已 dispatch action 和 state 随时间的变化

## UI 和 React

与React一起使用可以自动完成订阅和渲染的过程

### 使用 `useSelector` 从 Store 中读取 State

**`useSelector`**，它**使得 React 组件可以从 Redux store 中读取数据**

**selector 函数接收 Redux store 的 state 作为其参数，然后从 state 中取值并返回**。

**`useSelector` 会自动订阅 Redux store**

是严格等于 **===**

如果用了`map`、`filter`等返回copy的方法，加上`shallowEqual`比较函数，我们可以使用它来检查数组 *内部每一项* 是否仍然相同

`useSelector` 可以将比较函数作为它的第二个参数。比较函数接收旧值和新值作为参数，内部会判断两个值是否相同，相同则返回 “true”，那么组件也就不会被重新渲染。

### 使用 `useDispatch` 来 Dispatch Action

**`useDispatch` hook 函数**会返回 store 的 `dispatch` 方法。（事实上这个 hook 的内部实现真的是 `return store.dispatch`。）

### 使用 `Provider` 透传 Store

**使用 `<Provider>` 组件包裹 `<App>` 组件，并将 Redux store 作为 prop 传递给 `<Provider>` 组件**

react和redux建立起关联

- 使用 `useSelector` hook 函数来读取 React 组件中的数据
- 使用 `useDispatch` hook 函数在组件中 dispatch action
- 使用 `<Provider store={store}>` 组件包裹 `<App>` 组件，这样其他组件都能够和 store 进行交互

## 异步逻辑和数据获取

这里是**action函数**！

![Redux 异步数据流程图](https://s2.loli.net/2023/05/27/C93HOFeWRAb5nQt.gif)

- Redux middleware 旨在支持编写具有副作用的逻辑
  - “副作用”是指更改函数外部 state 或行为的代码，例如 AJAX 调用、修改函数参数或生成随机值
- middleware 为标准 Redux 数据流增加了一个额外的步骤
  - middleware 可以拦截传递给 `dispatch` 的其他值
  - middleware 可以访问 `dispatch` 和 `getState`，因此它们可以作为异步逻辑的一部分 dispatch 更多 action
- Redux “Thunk” middleware 使得可以传递函数给 `dispatch`
  - “Thunk” 函数让我们可以提前编写异步逻辑，而不需要知道当前使用的 Redux store
  - Redux thunk 函数接收 `dispatch` 和 `getState` 作为参数，并且可以 dispatch 诸如“此数据是从 API 响应中接收到的”之类的 action

## 使用 Redux Toolkit 的现代 Redux

- Redux Toolkit (RTK) 是编写 Redux 逻辑的标准方式
  - RTK 包含用于简化大多数 Redux 代码的 API
  - RTK 围绕 Redux 核心，并包含其他有用的包
- `configureStore` 用来设置一个具有良好默认值的 Redux store
  - 自动组合 slice reducers 来创建根 reducer
  - 自动设置 Redux DevTools 扩展和调试 middleware
- `createSlice` 简化了 Redux actions 和 reducers 的编写
  - 根据 slice/reducer 名称自动生成 action creators
  - Reducers 可以使用 Immer 在 `createSlice` 中“改变”（mutate）state
- `createAsyncThunk` 为异步调用生成 thunk
  - 自动生成一个 thunk + `pending/fulfilled/rejected` action creators
  - dispatch thunk 运行 payload creator 并 dispatch actions
  - 可以在 `createSlice.extraReducers` 中处理 thunk actions
- `createEntityAdapter` 为标准化 state 提供了 reducers + selectors
  - 包括用于常见任务的 reducer 功能，例如添加/更新/删除 items
  - 为 `selectAll` 和 `selectById` 生成记忆化 selectors



## Mini-redux



由于 Redux 的设计使得 state 是不可变的，因此在每次更新状态时都需要创建一个新的状态对象。因此，在合并多个 reducer 时，`combineReducers` 函数会检查每个子 reducer 是否已经改变了它所对应的 state，以避免不必要的重新渲染，提高性能。如果子 reducer 返回的 state 引用与之前的 state 相同，则说明 state 并没有被修改，可以直接使用之前的 state。如果子 reducer 返回的 state 引用与之前的 state 不同，则说明 state 已经被修改，需要使用新的 state 来更新状态。



```jsx
// 导出模块
export function myFunction() {...}

// 引入模块
import { myFunction } from './myModule';

// 导出模块
export default function myFunction() {...}

// 引入模块
import myFunction from './myModule';

```





Redux 管理全局变量的范式、思想Pattern

函数式编程

too much 模板

 **Context 去解决 prop-drilling**

**Tradeoff 权衡**



 **Redux ToolKit**