## hello-react

SPA Single-page applications 单页面 1个html

现代JS 从服务器到客服端 ==减少服务器的压力==

## requirements

npm node yarn

## setup

Vite

命令都在*package.json*

```
# Runs the application locally for the browser
npm run dev

# Builds the application for production
npm run build
```

## Meet the React Component

每个react应用都是在react组建的基础上构建的

1. 不同于驼峰式命名，组件是`PascalCase`，必须首字母大写  eg.App ，js函数定义的就是函数式组件 （还有别的方式的组件）
2. 暂时不会向组件传参，组件间借助 `props`
3. js返回了html，jsx能让你在js里写html，避免了原生的DOM操作

就像普通的函数，return前可以随便写

不想每次运行都重新定义变量，那就写到外面去

变量写死的话就丢在外面，别放函数里面

 

index.html 是根html ，main.jsx是入口文件 ，主要修改App.jsx

## JSX

 why `htmlFor` ?

jsx和js关系更近，遵循驼峰命名 ， class → className ，onclick → onClick

react用虚拟DOM储存DOM元素

![image-20230407100408409](./notes.assets/image-20230407100408409.png)

在大括号里写js

jsx是js语法层面的扩展

## List in React

react里，map()方法经常用来把每一个元素变成jsx

```react
let array = ['Hello','React','JSX','Components']

function App() {

  return (
    <div className="App">
        {array.map((item) => <h1>{item}</h1>)}
     </div>
  )
}
```

​    `{list.map(item => <h1>{item.title}</h1>)}`

![image-20230407111215806](./notes.assets/image-20230407111215806.png)



  `{list.map(item => <h1 key = {item.objectID}>{item.title}</h1>)}`

可以看到开发者工具里面 `<h1>`并没有key这个属性，so why？

每当React重新渲染list，用来检查这个item是否改变

![image-20230407111835474](./notes.assets/image-20230407111835474.png)

[渲染列表 – React (reactjs.org)](https://zh-hans.reactjs.org/learn/rendering-lists#keeping-list-items-in-order-with-key)

React 里需要 key 和文件夹里的文件需要有文件名的道理是类似的。它们（key 和文件名）都让我们可以从众多的兄弟元素中唯一标识出某一项（JSX 节点或文件）。即使元素的位置在渲染的过程中发生了改变，它提供的 `key` 值也能**让 React 在整个生命周期中一直认得它**。

## Meet another React Component

先前都住在操作 App这个组件

写一个巨大的组件  传统

把一整个文件拆分成很多组件

抽离出一个List组件 

```React
function List(){
  return(
      <div>
        {list.map(item => {
          return (
            <ul key={item.objectID}>
              <li>{item.title}</li>
              <li>{item.author}</li>
              <li>{item.url}</li>
            </ul>
          );
        })}
      </div>  //需要封装在一个根元素里
  )
}
```

App,List,Search 

![image-20230407144732783](./notes.assets/image-20230407144732783.png)

组件树

App是根组件，List和Search是App的子组件，它们互为兄弟组件

小组件可能是写完后发现可以抽离出来的 

根里面一般就写组件

觉得组件太大太复杂那就抽离

## React Component Instantiation

组件实例化

每个类都有构造函数，当实例化的时候，调用构造函数去创建新变量

类是一个蓝图，实例是建出来的房子

类和组件的声名和实例化挺像的，一个声明，多个实例

## React DOM

App组件的实例在main.jsx

React DOM 把 React 和 HTML （index.html）连接起来

非常小的html接收到巨大的js

目前来说静态的还体会不到好处，一旦有了数据...

## React Component Declaration

函数式声明和箭头函数可以，但

箭头函数好！ 碰到this丢失再说！

```react
const App = () => {
  // perform a task in between

  return (
    <div>
      ...
    </div>
  );
};
```

## Handler Function in JSX

处理事件

到目前为止还没交互

原生HTML `addEventListener` 来监听事件，JSX `onChange`

synthetic event是对原生事件的包装，有些默认事件是需要禁止的（原生提交表单会触发刷新，但React里不允许刷新）。如果真的需要用原生事件，用`event.nativeEvent`，但基本用不到

**传函数，而不是函数返回值，除非返回值是函数**

```react
const handleChange = (e) => {
  console.log(e);
  console.log(e.currentTarget);
  console.log(e.currentTarget.value);
}

const Search = () => {
  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange = {handleChange}/>
    </div>
  );
}
```

阻止事件传播 `e.stopPropagation()`

阻止默认行为  `e.preventDefault()`

## Props

借用props在组件间传值 **不可被改变**

我们通过组件元素的HTML属性从父组件传递给子组件的所有东西都可以在子组件中访问。 子组件在其函数签名中接收一个参数（`props`）作为对象，其中包括所有传递的属性（简称：props）。

只能从上往下传 父到子 

```react
const App = () => {
  const stories = [...];
  return (
    <div>
       	...
         <List list = {stories}/>
    </div>
  )
};

//解构赋值写法
const List = ({ list }) => {
  return(
      <ul>
        {list.map((item) => (
          <Item 
            title = {item.title} 
            author = {item.author} 
            url = {item.url}  
            key = {item.objectID} 
          />
        ))}
      </ul>
  )
}

const Item = ({title, author, url}) => {
  return (
    <ul>
      <li>{title}</li>
      <li>{author}</li>
      <li>{url}</li>
    </ul>
  );
}
```

## State

**可以被修改**  响应式 

![./image-20230407213326497](./notes.assets/image-20230407213326497.png)

修改数据，自动重新渲染

当`props`被用来向下传递信息时，`state`被用来随时间改变信息

`useState` （是个hook），告诉React这变量是个状态值，帮我盯着它

```react
const Search = () => {
  const [searchTerm, setSearchTerm] = React.useState('');
  const handleChange = (e) => {
    setSearchTerm(e.target.value);
  };
  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange = {handleChange}/>
      <p>
        Searching for <strong>{searchTerm}</strong>
      </p>
    </div>
  );
}
```

可设初始值，一个状态值，一个状态更新函数

**React只重新渲染状态改变了的状态值的组件**

![./image-20230407214241994](./notes.assets/image-20230407214241994.png)

初次渲染 → 交互 → 状态值改变 → 重新渲染

![./image-20230407215234747](notes.assets/image-20230407215234747.png)

## Callback Handlers 

回调处理函数

目前只能从上往下传值

![image-20230407215813760](./notes.assets/image-20230407215813760.png)

事件处理程序在父组件`App`里声明（A），传到子组件B `Seacrh`，在那里作为回调处理程序`handleChange`被执行（C），并回调到父组件App中被引入的地方D

```jsx
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  // A
  const handleSearch = (event) => {
    // D
    console.log(event.target.value);
  };
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      {/* // B */}
      <Search onSearch={handleSearch} />
# leanpub-end-insert

      <hr />

      <List list={stories} />
    </div>
  );
};

# leanpub-start-insert
const Search = (props) => {
# leanpub-end-insert
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = (event) => {
    setSearchTerm(event.target.value);

# leanpub-start-insert
    // C
    props.onSearch(event);
# leanpub-end-insert
  };

  return ( ... );
};
```

这方法比较隐蔽，也比较垃圾，和state没关系

## Lifting State in React

状态提升--往上传

callback handler 不好用，父组件甚至没法调用，filter更不可行了

**直接在父组件里定义**！（从子组件提升到父组件）

![image-20230407223301270](./notes.assets/image-20230407223301270.png)

**在父组件定义状态！**就只从上往下传就行了！

```jsx
const App = () => {
  const stories = [ ... ];
	//直接父组件定义状态
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleSearch = (event) => {
    setSearchTerm(event.target.value);
  };

# leanpub-start-insert
  const searchedStories = stories.filter(function (story) {
      //不区分大小写了呗
      return story.title.toLowerCase().includes(searchTerm.toLowerCase());
  });
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search onSearch={handleSearch} />

      <hr />

# leanpub-start-insert
      <List list={searchedStories} />
# leanpub-end-insert
    </div>
  );
};

const Search = (props) => (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange = {props.onSearch}/>
    </div>
  );
```



实际开发里面数组、对象用的更多

```jsx
const App = () => {
    const [list, setList] = React.useState([
        { uname: 'Lee'},
        { uname: 'React'},
        { uname: 'Redux'},
        { uname: 'Hello'},
    ]);
    const[input, setInput] = React.useState('');
    
    const handleAdd = () => {
        setList((prev) => [...prev,{ uname: input}]);
    };
    
    const handleSubtract = () =>{
         setList((prev) => {
             const arrCopy = [...prev];
             cosnt index = prev.findIndex({item} => item.uname === input);
             arrCopy.splice(index,1);
             return arrCopy;
         });
    };
    
    const handleChange = (e) =>{
        setInput(e.target.value);
    };
    
    return {
        <div className = 'App'>
        	<input type = 'text' onChange = {handleChange} />
            <button onClick = {handleAdd}>Add</button>
            <button onClick = {handleSubtract}>Subtract</button>
            {list.map((item) =>(
                //这里key可以更优化
                <h1 key={item.uname}>{item.uname}</h1>
            ))}
        </div>
    }
}
```

## controlled-components

受控组件

Input没有显示这个默认值，当我们开始输入才会显示

input不知道React的状态这玩意儿，那就把状态值绑到input的value里

那现在inpt就不自由了，是 **受控** 的 ，用state来约束inpt

![image-20230408112413194](./notes.assets/image-20230408112413194.png)

## Props Handling



![image-20230408112732517](./notes.assets/image-20230408112732517.png)

**解构赋值！** **Object Destructuring**

最常用的形态，传参的时候 解构出来

用`{...item}` 代替

```jsx
	    title={item.title}
        url={item.url}
        author={item.author}
        num_comments={item.num_comments}
        points={item.points}
```

- 用解构赋值
- 用`spread ...`

## Side-Effects

关联、影响、牵连

可以处理第三方API

```jsx
const App = () => {
  ...

  const handleSearch = (event) => {
    setSearchTerm(event.target.value);
    localStorage.setItem('search', event.target.value);
  };

  ...
);
```

用浏览器的本地存储的值初始化状态，当处理程序被调用时，我们将新的值写入浏览器的存储和组件的状态。

但如果在其他地方使用`setSearchTerm`状态更新函数，本地存储不会被更新

用**React的`useEffect` Hook**来解决

```jsx
React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);
```

第一个参数 运行side-effect函数，第二个参数，side-effect依赖的变量 (the dependency array) 

没第二个参数，每次渲染这个side-effect都会运行，写了只有依赖变的时候才会运行



 **官方文档 详解** [Synchronizing with Effects](https://zh-hans.reactjs.org/learn/synchronizing-with-effects)

和Effect同步   有些组件要和外部同步

*Effects* 让你可以在被渲染之后运行代码来同步组件和外部的数据

### *Effects* 和事件的区别?

React组件有两种逻辑：

- 渲染相关的代码：要纯粹 仅执行计算操作，不做其他操作（不应该有请求、console等）
- 事件处理函数：不纯粹 可能会更新输入框、重定向网页、提交请求，包含 side-effect（特定的事件由用户去触发）

但有时候没有事件触发的函数，所以有了 `Effects` ：**帮我们处理不是被特定事件触发的，而是被渲染触发的side-effect** 

Effects 在渲染和提交过程之后执行。都渲染完了，就是同步数据的好时机咯

不是全部的side-effect（组件间的状态） 都要用*Effect*，还有事件处理函数呢 ，啥会儿用啊：浏览器API、第三方组件、网络处理blabla

### 怎么写*Effect*

- 声明
- 确定依赖：大多数Effect应该只在需要时重新运行，而不是在每次渲染后重新运行。
- 加个清理函数：有些要停止、断开

s1:声明

```jsx
import { useEffect } from 'react';
function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
  });
  return <div />;
}
```

用useEffect包住side-effect，把它从渲染计算中移出来。

```jsx
import { useState, useRef, useEffect } from 'react';

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  useEffect(() => {
    if (isPlaying) {
      ref.current.play();
    } else {
      ref.current.pause();
    }
  });

  return <video ref={ref} src={src} loop playsInline />;
}

export default function App() {
  const [isPlaying, setIsPlaying] = useState(false);
  return (
    <>
      <button onClick={() => setIsPlaying(!isPlaying)}>
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <VideoPlayer
        isPlaying={isPlaying}
        src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
      />
    </>
  );
}
```

**Pitfall** 死循环

```jsx
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1);
});
```

Effect的运行是渲染的结果。设置状态会触发渲染。Effect运行时，它设置了状态，这将导致重新渲染，这将导致Effect运行，它再次设置状态，这将导致另一次重新渲染，如此循环。

s2:确定依赖

```jsx
  useEffect(() => {
    if (isPlaying) {
      console.log('Calling video.play()');
      ref.current.play();
    } else {
      console.log('Calling video.pause()');
      ref.current.pause();
    }
  }, [isPlaying]);
```

依赖是个数组，只要其中一个改变了就会运行Effect。

有时候依赖变了不想运行Effect：不该去掉依赖，去修改Effect代码（添加条件）

```jsx
useEffect(() => {
  //	每次渲染都触发
});

useEffect(() => {
  //	第一次渲染后触发
}, []);

useEffect(() => {
  // 第一次 + 其中一个变化了
}, [a, b]);
```

s3：加清理函数

为了帮助发现像没有`cleanup`的bug，在开发过程中，React会在每个组件首次挂载后立即重挂一次。

```jsx
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => connection.disconnect();
  }, []);
  return <h1>Welcome to the chat!</h1>;
}
```

```
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  return () => {
    ignore = true;
  };
}, [userId]);
```

取数据，第一次拿不ignore，后面再拿就ignore：从缓存里取

```jsx
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  return () => {
    ignore = true;
  };
}, [userId]);
```

### 什么不是Effect

- 初始化应用：有些代码只在应用启动的时候运行，放在组件外就行
- 发送post告诉服务器买了个东西：不会买两次，就不该放在`Effect`

```jsx
function handleClick() {
    // ✅ Buying is an event because it is caused by a particular interaction.
    fetch('/api/buy', { method: 'POST' });
  }
```

- 与事件不同，Effects是由渲染本身引起的，而不是由某个特定的交互引起的。
- Effect让你将一个组件与一些外部系统（第三方API、网络等）同步。
- 默认情况下，Effects在每次渲染后运行（包括初始渲染）。
- 如果它的所有依赖关系的值与上次渲染时相同，React将跳过Effect。
- 你不能 "选择 "你的依赖关系。它们是由Effect内部的代码决定的。
- 空的依赖关系数组（[]）对应于组件的 "安装"，即被添加到屏幕上。
- 在严格模式下，React会对组件进行两次挂载（仅在开发中！），以对你的Effect进行压力测试。
- 如果你的Effect因为重新挂载而损坏，你需要实现一个清理函数。
- React会在Effect下次运行前和卸载时调用你的清理函数

## fragments

目前写的组件里面都被打包在根元素（容器元素）`<div>`，否则标签和输入元素就无法在没有包装的顶层元素的情况下并排返回

为解决这情况  → `fragments`

```jsx
const Search = ({ search, onSearch }) => (
  <div>  //容器元素
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
  </div> //容器元素
);
```

```jsx
const Search = ({ search, onSearch }) => (
  <React.Fragment> //<>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
  </React.Fragment> //</>
);
```

 **<React.Fragment> （<>）不会实际渲染**  更简洁的写法 `<> </>`

## reusable-components

可复用的组件  

之前`search`组件写的太死了

怎么能不写死呢？给`search`组件传动态的`id`、`label`

```jsx
const InputWithLabel = ({ 
  id, 
  label , 
  value, 
  type = 'text', //默认
  onInputChange,
}) => (
    <>
      <label htmlFor={id}>{label}: </label>
      <input 
        id={id}
        type={type} 
        value={value}
        onChange = {onInputChange} 
      />
    </>
  );
```

想复用，给外部提供接口，少写死

## Component Composition

组件组合 将 JSX 作为子组件传递 **嵌套组件**

一个React App 就像一棵组件树

在组件元素之间传递的所有东西都可以作为组件中的 `**children**` 被访问，并在某处被渲染

```jsx
<InputWithLabel
    id="search"
    value={searchTerm}
    onInputChange={handleSearch}
#########
>
#########
	<strong>Search:</strong>
#########
</InputWithLabel>
```

```jsx
const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
#########
  children,
#########
}) => (
  <>
#########
    <label htmlFor={id}>{children}</label>
#########
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      onChange={onInputChange}
    />
  </>
);
```

将内容嵌套在 JSX 标签中时，父组件将在名为 `children` 的 prop 中接收到该内容

## Imperative

（React本质是声明式的）

side-effect：react管辖之外的，这时候需要命令式去访问元素

怎么写命令式？一个`useEffect`、一个`ref`

1. 用**React的useRef Hook**创建一个`ref`。这个`ref`对象是一个持久的值，在React组件的生命周期内保持不变。它有一个叫做`current`的属性，与`ref`对象相反，它可以被**改变**
2. `ref`被传递到元素的JSX保留的`ref`属性，因此元素实例被分配到可改变的`current`属性
3. 用React的`useEffect` Hook选择进入React的生命周期，在组件渲染（或其依赖关系改变）时执行
4. 由于`ref`被传递到元素的`ref`属性，它的`current`属性可以访问该元素。

## Ref

引用值，不触发渲染

**能保存、更新数据，不引起组件的重新渲染** 

```jsx
import { useRef } from 'react';
```

调用 `useRef` Hook 并传入你想要引用的初始值作为唯一参数。例如，这里的 ref 引用的值是“0”：

```jsx
const ref = useRef(0);
```

`useRef` 返回一个这样的对象:

```jsx
{ 

  current: 0 // 你向 useRef 传入的值

}
```

用 `ref.current` 属性访问该 ref 的当前值。这个值是有意被设置为可变的，意味着既可以读取它也可以写入它。

**组件不会在每次递增时重新渲染。** 与 state 一样，React 会在每次重新渲染之间保留 ref。但是，设置 state 会重新渲染组件，更改 ref 不会！

### ref 和 state 的不同之处 

ref 似乎没有 state 那样“严格” —— 例如，可以改变它们而非总是必须使用 state 设置函数。但在大多数情况下，用 state吧！ref 是一个“应急方案”，并不会经常用到它。 以下是 state 和 ref 的对比：

| ref                                                     | state                                                        |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| `useRef(initialValue)`返回 `{ current: initialValue }`  | `useState(initialValue)` 返回 state 变量的当前值和一个 state 设置函数 ( `[value, setValue]`) |
| 更改时不会触发重新渲染                                  | 更改时触发重新渲染。                                         |
| 可变 —— 你可以在渲染过程之外修改和更新 `current` 的值。 | “不可变” —— 你必须使用 state 设置函数来修改 state 变量，从而排队重新渲染。 |
| 你不应在渲染期间读取（或写入） `current` 值。           | 你可以随时读取 state。但是，每次渲染都有自己不变的 state [快照](https://zh-hans.reactjs.org/learn/state-as-a-snapshot)。 |

### 何时使用 ref 

通常，当你的组件需要“跳出” React 并**与外部 API 通信**时，你会用到 ref —— 通常是不会影响组件外观的浏览器 API。以下是这些罕见情况中的几个：

- 存储 [timeout ID](https://developer.mozilla.org/docs/Web/API/setTimeout)
- 存储和操作 [DOM 元素](https://developer.mozilla.org/docs/Web/API/Element)
- 存储不需要被用来计算 JSX 的其他对象。

如果你的组件需要存储一些值，但不影响渲染逻辑，请选择 ref。

### ref 的最佳实践 

- **将 ref 视为应急方案。** 使用外部系统或浏览器 API 时，ref 很有用。如果很大一部分应用程序逻辑和数据流都依赖于 ref，可能需要重新考虑你的方法。**能不用就不用**
- **不要在渲染过程中读取或写入 `ref.current`。** 如果渲染过程中需要某些信息，使用 [state](https://zh-hans.reactjs.org/learn/state-a-components-memory) 代替。由于 React 不知道 `ref.current` 何时发生变化，即使在渲染时读取它也会使组件的行为难以预测。（唯一的例外是像 `if (!ref.current) ref.current = new Thing()` 这样的代码，它只在第一次渲染期间设置一次 ref）

### ref 和 DOM 

你可以将 ref 指向任何值。但是，ref 最常见的用法是访问 DOM 元素。例如，如果你想以编程方式聚焦一个输入框，这种用法就会派上用场。当你将 ref 传递给 JSX 中的 `ref` 属性时，比如 `<div ref={myRef}>`，React 会将相应的 DOM 元素放入 `myRef.current` 中

## Inline Handler 

内联处理程序 允许我们**从列表中删除项目**

```jsx
const handleDelete = () => {
    setStories((prev) => prev.filter(item => item.title !== title));
  };
```

回调函数使用数组的 `filter` 方法来遍历之前的stories列表 `prev`，并将其中所有标题不等于当前要删除的标题 `title` 的故事筛选出来，最终形成一个新的stories列表。这个新列表会被传递给 `setStories`，从而更新组件状态中存储的故事列表，达到了删除指定故事的目的

就系介玩：

```jsx
 <button type="button" onClick={() => onRemoveItem(item)}>
```

## Asynchronous Data

第一个交互：搜索

第二个交互：删除

搞点真实数据！模拟一哈请求API

API请求属于side-effect，用`useEffect`

```jsx
const [stories, setStories] = React.useState([]); //[]是因为有filter这个数组方法

  React.useEffect(() => {
      new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(storiesList);
        }, 2000);
      }).then((data) => {
        setStories(data)
      });
  },[]); //[]表示只在组件挂载时执行一次
```



## Conditional Rendering

条件渲染

eg.loading... 

在React中，如果我们必须根据信息（如state、props）来渲染不同的JSX，就会发生条件渲染

那就三元表达式？

```jsx
{stories.length?<List list = {searchedStories} setStories = {setStories}/> : <h1>Loading...</h1>}
```

```jsx
{isError && <p>Something went wrong ...</p>}
```

当 `isError` 为真时，表达式的值为 `<p>Something went wrong ...</p>`，React 就会将其渲染到页面中。如果 `isError` 为假，那么整个表达式的值就为假，React 则不会渲染任何东西

## Third-Party Libraries 、 Data Fetching

原生Fetch不是所有浏览器都兼容，不是默认json，所以用`axios`

```jsx
 React.useEffect(() => {
    axios.get('https://hn.algolia.com/api/v1/search?query=react')
    .then((value) => {
      setStories(value.data.hits);
    })
    .catch((error) => {
      console.log(error);
    })
  },[]); 
```

### 单引号（'') 、双引号（""和反引号（``）

单引号（'') 和双引号（""）都可以用来创建字符串。而反引号（``）则是在 ECMAScript 6（ES6）中引入的一种新的字符串语法，也被称为“模板字面量”（template literals）。

与单引号和双引号不同，反引号可以包含嵌入式表达式（embedded expressions），也就是使用 `${}` 语法将任意 JavaScript 表达式插入到字符串中。这使得创建动态字符串变得更加方便，避免了使用字符串拼接的繁琐过程。

### refetch

```jsx
React.useEffect(() => {
    if(!searchTerm) return;
    axios
    .get(`https://hn.algolia.com/api/v1/search?query=${searchTerm}`)
    .then((value) => {setStories(value.data.hits);
    })
    .catch((error) => {
      console.log(error);
    })
  },[searchTerm]);  
```

### 显式Data Fetching

不要每次输入都在请求，加个搜索按钮吧！

async

```jsx
React.useEffect(() => {
    if(!searchTerm) return;
   (async () => {
      let value = await axios.get(url);
      setStories(value.data.hits);
    })();
  }, [url]);
```

## Form

`handleSearchSubmit()`处理程序没有传递给按钮，而是用在**表单**元素的`onSubmit`属性中。

按钮收到一个新的`type`属性，叫做`submit`，点击，触发`onSubmit`事件，运行`handleSearchSubmit`

```jsx
 <form onSubmit = {handleSearchSubmit}>
      <InputWithLabel
        id = "search"
        value = {searchTerm}
        onInputChange = {handleSearch}
      >
        <strong> Search : </strong>
      </InputWithLabel>
      <button type = "submit" disabled = {!searchTerm}>Submit</button>
    </form>
```

这样，回车`Enter` 也可以提交啦！

（不想每次都触发HTML原生的表单行为导致浏览器重新加载，加上` event.preventDefault();`）
