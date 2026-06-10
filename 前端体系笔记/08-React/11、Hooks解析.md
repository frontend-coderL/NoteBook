## 一. 认识hook

### 1.1. 为什么需要hook

*Hook 是 React 16.8 的新增特性，它可以让我们在不编写class的情况下使用state以及其他的React特性（比如生命周期）。*

```react
import React, { PureComponent } from "react";

class HelloWord extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      message: "Hello World",
    };
  }

  changeText() {
    this.setState({ message: "你好啊, 李银河!" });
  }

  render() {
    const { message } = this.state;
    return (
      <div>
        <h2>内容: {message}</h2>
        <button onClick={(e) => this.changeText()}>修改文本</button>
      </div>
    );
  }
}

function HelloWorld2(props) {
  let message = "Hello World";
  // 函数式组件存在的最大的缺陷:
  // 1.组件不会被重新渲染: 修改message之后, 组件知道自己要重新进行渲染吗?
  // 2.如果页面重新渲染: 函数会被重新执行, 第二次重新执行时, 会重新给message赋值为hello world
  // 3.类似于生命周期的回调函数: 也是没有的
  return (
    <div>
      <h2>内容2: {message}</h2>
      <button onClick={(e) => (message = "你好啊, 李银河!")}>修改文本</button>
    </div>
  );
}

export class App extends PureComponent {
  render() {
    return (
      <div>
        <HelloWord />
        <hr />
        <HelloWorld2 />
      </div>
    );
  }
}

export default App;
```

我们先来思考一下`class组件`相对于`函数式组件`有什么优势？比较常见的是下面的优势：

- class组件可以定义自己的state，用来保存组件自己内部的状态；
  - 函数式组件不可以，因为函数每次调用都会产生新的临时变量；

- class组件有自己的生命周期，我们可以在对应的生命周期中完成自己的逻辑；
  - 比如在`componentDidMount`中发送网络请求，并且该生命周期函数只会执行一次；
  - 函数式组件在学习hooks之前，如果在函数中发送网络请求，意味着每次重新渲染都会重新发送一次网络请求；

- class组件可以在状态改变时只会重新执行render函数以及我们希望重新调用的生命周期componentDidUpdate等；
  - 函数式组件在重新渲染时，整个函数都会被执行，似乎没有什么地方可以只让它们调用一次；


所以，在Hook出现之前，对于上面这些情况我们通常都会编写class组件。

**但是class组件依然存在很多的问题：**

**复杂组件变得难以理解：**

- 我们在最初编写一个class组件时，往往逻辑比较简单，并不会非常复杂。
- 但是随着业务的增多，我们的class组件会变得越来越复杂；
- 比如componentDidMount中，可能就会包含大量的逻辑代码：包括网络请求、一些事件的监听（还需要在componentWillUnmount中移除）；
- 而对于这样的class实际上非常难以拆分：因为它们的逻辑往往混在一起，强行拆分反而会造成过度设计，增加代码的复杂度；

**难以理解的class：**

- 很多人发现学习ES6的class是学习React的一个障碍。
- 比如在class中，我们必须搞清楚this的指向到底是谁，所以需要花很多的精力去学习this；
- 虽然我认为前端开发人员必须掌握this，但是依然处理起来非常麻烦；

**组件复用状态很难**：

- 在前面为了一些状态的复用我们需要通过高阶组件或render props；
- 像我们之前学习的redux中connect或者react-router中的withRouter，这些高阶组件设计的目的就是为了状态的复用；
- 或者类似于Provider、Consumer来共享一些状态，但是多次使用Consumer时，我们的代码就会存在很多嵌套；
- 这些代码让我们不管是编写和设计上来说，都变得非常困难；

> Hook的出现，可以解决上面提到的这些问题；

**简单总结一下hooks：**

- **它可以让我们在不编写class的情况下使用state以及其他的React特性**；
- 但是我们可以由此延伸出非常多的用法，来让我们前面所提到的问题得到解决；

Hook的使用场景：

- Hook的出现基本可以代替我们之前所有使用class组件的地方（除了一些非常不常用的场景）；
- 但是如果是一个旧的项目，你并不需要直接将所有的代码重构为Hooks，因为它完全向下兼容，你可以渐进式的来使用它；
- Hook只能在函数组件中使用，不能在类组件，或者函数组件之外的地方使用；

**Class组件和Functional组件对比：**

![image-20230510202831563](.\img\image-20230510202831563.png)

### 1.2. hooks的基本演练

我们通过一个计数器案例，来对比一下class组件和函数式组件结合hooks的对比：

class组件实现：

```react
import React, { PureComponent } from "react";

export class CounterClass extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      counter: 0,
    };
  }

  increment() {
    this.setState({
      counter: this.state.counter + 1,
    });
  }
  decrement() {
    this.setState({
      counter: this.state.counter - 1,
    });
  }

  render() {
    const { counter } = this.state;
    return (
      <div>
        <h2>当前计数: {counter}</h2>
        <button onClick={(e) => this.increment()}>+1</button>
        <button onClick={(e) => this.decrement()}>-1</button>
      </div>
    );
  }
}

export default CounterClass;
```

函数式组件实现：

```react
import { memo, useState } from "react";

function CounterHook(props) {
  const [counter, setCounter] = useState(0);

  return (
    <div>
      <h2>当前计数: {counter}</h2>
      <button onClick={(e) => setCounter(counter + 1)}>+1</button>
      <button onClick={(e) => setCounter(counter - 1)}>-1</button>
    </div>
  );
}

export default memo(CounterHook);
```

**useState解析**

你会发现上面的代码差异非常大：

- 函数式组件结合hooks让整个代码变得非常简洁，
- 并且再也不用考虑this相关的问题；

那么我们来研究一下核心的一段代码代表什么意思：

- useState来自react，需要从react中导入，它是一个hook；
  - 参数：初始化值，如果不设置为undefined；
  - 返回值：数组，包含两个元素；
    - 元素一：当前状态的值（第一调用为初始化值）；
    - 元素二：设置状态值的函数；

- 点击button按钮后，会完成两件事情：
  - 调用setCount，设置一个新的值；
  - 组件重新渲染，并且根据新的值返回DOM结构；


**相信通过上面的一个简单案例，你已经会喜欢上Hook的使用了。**

- Hook 就是 JavaScript 函数，这个函数可以帮助你 `钩入（hook into）` React State以及生命周期等特性；

**但是使用它们会有两个额外的规则：**

- 只能在**函数最外层**调用 Hook。不要在循环、条件判断或者子函数中调用。
- 只能在 **React 的函数组件**中调用 Hook。不要在其他 JavaScript 函数中调用。（还有一个地方可以调用 Hook —— 就是自定义的 Hook 中，后面学习）。

> Tip：Hook指的类似于useState、useEffect这样的函数，Hooks是对这类函数的统称；

## 二. hooks基础

### 2.1. State Hook

State Hook的API就是 `useState`，我们在前面已经进行了学习：

- **`useState`**会帮助我们定义一个 `state变量`，`useState` 是一种新方法，它与 class 里面的 `this.state` 提供的功能完全相同。
  - 一般来说，在函数退出后变量就会”消失”，而 state 中的变量会被 React 保留。

- **`useState`**接受唯一一个参数，在第一次组件被调用时使用来作为初始化值。（如果没有传递参数，那么初始化值为undefined）。
- **`useState`**是一个数组，我们可以通过数组的解构，来完成赋值会非常方便。
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

FAQ：为什么叫 `useState` 而不叫 `createState`?

- “Create” 可能不是很准确，因为 state 只在组件首次渲染的时候被创建。
- 在下一次重新渲染时，`useState` 返回给我们当前的 state。
- 如果每次都创建新的变量，它就不是 “state”了。
- 这也是 Hook 的名字*总是*以 `use` 开头的一个原因。

当然，我们也可以在一个组件中定义多个变量和复杂变量（数组、对象）：

```react
import React, { useState } from 'react';

export default function Home() {
  const [age, setAge] = useState(0);
  const [names, setNames] = useState(["abc", "cba"]);
  const [info, setInfo] = useState({name: "why", age: 18});

  function addFriend() {
    names.push("nba");
    console.log(names);
    setNames(names);
  }

  return (
    <div>
      <h2>当前年龄: {age}</h2>
      <button onClick={e => setAge(age + 1)}>age+1</button>
      <h2>朋友列表</h2>
      <ul>
        {
          names.map((item, index) => {
            return <li key={index}>{item}</li>
          })
        }
      </ul>
      <button onClick={e => setNames([...names, "nba"])}>添加好友</button>
      {/* 思考: 这样的方式是否可以实现 */}
      <button onClick={addFriend}>添加好友</button>
      <h2>我的信息:</h2>
      <div>我的名字: {info.name}</div>
      <button onClick={e => setInfo({...info, name: "lilei"})}>修改名字</button>
    </div>
  )
}
```

### 2.2. Effect Hook

目前我们已经通过hook在函数式组件中定义state，那么类似于生命周期这些呢？

- Effect Hook 可以让你来完成一些类似于class中生命周期的功能；
- 事实上，类似于网络请求、手动更新DOM、一些事件的监听，都是React更新DOM的一些副作用（Side Effects）；
- 所以对于完成这些功能的Hook被称之为 Effect Hook；

#### 2.2.1. Effect基本使用

假如我们现在有一个需求：**页面的title总是显示counter的数字**

使用class组件如何实现呢？

- 我们会发现 `document.title` 的设置必须在两个生命周期中完成；
- 这是因为React的class组件并没有给我们提供一个统一的生命周期函数，可以让无论是否是第一次渲染都会执行的生命周期函数；

```react
import React, { PureComponent } from 'react'

export default class CounterTitle01 extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      counter: 0
    }
  }

  componentDidMount() {
    document.title = `当前计数: ${this.state.counter}`
  }

  componentDidUpdate() {
    document.title = `当前计数: ${this.state.counter}`
  }

  render() {
    console.log("111");
    return (
      <div>
        <h2>当前计数: {this.state.counter}</h2>
        <button onClick={e => this.increment()}>+1</button>
        <button onClick={e => this.decrement()}>-1</button>
      </div>
    )
  }

  increment() {
    this.setState({counter: this.state.counter + 1});
  }
  decrement() {
    this.setState({counter: this.state.counter - 1});
  }
}
```

这个时候，我们可以使用useEffect的Hook来完成：

```react
import React, { useState, useEffect } from 'react';

export default function CounterTitle02() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `当前计数: ${count}`;
  })

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <button onClick={e => setCount(count - 1)}>-1</button>
    </div>
  )
}
```

**useEffect的解析：**

- 通过useEffect的Hook，可以告诉React需要在渲染后执行某些操作；
- useEffect要求我们传入一个`回调函数`，在React执行完更新DOM操作之后，就`会回调这个函数`；
- 默认情况下，无论是第一次渲染之后，还是每次更新之后，都会执行这个 `回调函数`；

#### 2.2.2. 需要清除Effect

在class组件的编写过程中，某些副作用的代码，我们需要在componentWillUnmount中进行清除：

- 比如我们之前的事件总线或Redux中手动调用subscribe；
- 都需要在componentWillUnmount有对应的取消订阅；
- Effect Hook通过什么方式来模拟componentWillUnmount呢？

useEffect传入的`回调函数A本身`可以有一个返回值，这个返回值是`另外一个回调函数B`：

```
type EffectCallback = () => (void | (() => void | undefined));
```

我们可以这样来编写Effect Hook：

```react
import React, { useState, useEffect } from 'react';

export default function EffectHookClear() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `当前计数: ${count}`;
    console.log("每次DOM更新时会回调");

    return () => {
      console.log("DOM被移除时会回调");
    }
  })

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <button onClick={e => setCount(count - 1)}>-1</button>
    </div>
  )
}
```

**为什么要在 effect 中返回一个函数？**

- 这是 effect 可选的清除机制。每个 effect 都可以返回一个清除函数；
- 如此可以将添加和移除订阅的逻辑放在一起；
- 它们都属于 effect 的一部分；

**React 何时清除 effect？**

- React 会在组件更新和卸载的时候执行清除操作；
- 正如之前学到的，effect 在每次渲染的时候都会执行；

#### 2.2.3. 使用多个Effect

使用Hook的其中一个目的就是解决class中生命周期经常将很多的逻辑放在一起的问题：

- 比如网络请求、事件监听、手动修改DOM，这些往往都会放在componentDidMount中；

使用Effect Hook，我们可以将它们分离到不同的useEffect中：

```react
import React, { useEffect } from 'react';

export default function MultiUseEffect() {
  useEffect(() => {
    console.log("网络请求");
  });

  useEffect(() => {
    console.log("修改DOM");
  })

  useEffect(() => {
    console.log("事件监听");
    return () => {
      console.log("取消监听");
    }
  })

  return (
    <div>
      <h2>MultiUseEffect</h2>
    </div>
  )
}
```

**Hook 允许我们按照代码的用途分离它们，** 而不是像生命周期函数那样：

- React 将按照 effect 声明的顺序依次调用组件中的每一个 effect；

#### 2.2.4. Effect性能优化

默认情况下，useEffect的回调函数会在每次渲染时都重新执行，但是这会导致两个问题：

- 某些代码我们只是希望执行一次即可，类似于componentDidMount和componentWillUnmount中完成的事情；（比如网络请求、订阅和取消订阅）；
- 另外，多次执行也会导致一定的性能问题；

我们如何决定useEffect在什么时候应该执行和什么时候不应该执行呢？

- useEffect实际上有两个参数：
- 参数一：执行的回调函数；
- 参数二：该useEffect在哪些state发生变化时，才重新执行；（受谁的影响）

我们来看下面的一个案例：

- 在这个案例中，我们修改show的值，是不会让useEffect重新被执行的；

```react
import React, { useState, useEffect } from 'react';

export default function EffectPerformance() {
  const [count, setCount] = useState(0);
  const [show, setShow] = useState(true);

  useEffect(() => {
    console.log("修改DOM");
  }, [count])

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <button onClick={e => setShow(!show)}>切换</button>
    </div>
  )
}
```

但是，如果一个函数我们不希望依赖任何的内容时，也可以传入一个空的数组 []：

- 那么这里的两个回调函数分别对应的就是componentDidMount和componentWillUnmount生命周期函数了；

```react
useEffect(() => {
  console.log("监听事件");

  return () => {
    console.log("取消监听");
  }
}, [])
```

## 三. Hook高级使用

### 3.1. Context Hook

在之前的开发中，我们要在组件中使用共享的Context有两种方式：

- 类组件可以通过 `类名.contextType = MyContext`方式，在类中获取context；
- 多个Context或者在函数式组件中通过 `MyContext.Consumer` 方式共享context；

但是多个Context共享时的方式会存在大量的嵌套：

- Context Hook允许我们通过Hook来直接获取某个Context的值；

```
const value = useContext(MyContext);
```

在App.js中使用Context

```react
import React, { createContext } from 'react';
import ContextHook from './04_useContext使用/01_ContextHook';

export const UserContext = createContext();
export const ThemeContext = createContext();

export default function App() {
  return (
    <div>
      <UserContext.Provider value={{name: "why", age: 18}}>
        <ThemeContext.Provider value={{color: "red", fontSize: "20px"}}>
          <ContextHook/>
        </ThemeContext.Provider>
      </UserContext.Provider>
    </div>
  )
}
```

在对应的函数式组件中使用Context Hook：

```react
import React, { useContext } from 'react'
import { UserContext, ThemeContext } from '../App'

export default function ContextHook() {
  const user = useContext(UserContext);
  const theme = useContext(ThemeContext);
  console.log(user);
  console.log(theme);

  return (
    <div>
      ContextHook
    </div>
  )
}
```

注意事项：当组件上层最近的 `<MyContext.Provider>` 更新时，该 Hook 会触发重新渲染，并使用最新传递给 `MyContext` provider 的 context `value` 值。

### 3.2. useReducer（了解）

很多人看到useReducer的第一反应应该是redux的某个替代品，其实并不是。

useReducer仅仅是useState的一种替代方案：

- 在某些场景下，如果state的处理逻辑比较复杂，我们可以通过useReducer来对其进行拆分；
- 或者这次修改的state需要依赖之前的state时，也可以使用；

单独创建一个reducer/counter.js文件：

```react
export function counterReducer(state, action) {
  switch(action.type) {
    case "increment":
      return {...state, counter: state.counter + 1}
    case "decrement":
      return {...state, counter: state.counter - 1}
    default:
      return state;
  }
}
```

home.js

```react
import React, { useReducer } from 'react'
import { counterReducer } from '../reducer/counter'

export default function Home() {
  const [state, dispatch] = useReducer(counterReducer, {counter: 100});

  return (
    <div>
      <h2>当前计数: {state.counter}</h2>
      <button onClick={e => dispatch({type: "increment"})}>+1</button>
      <button onClick={e => dispatch({type: "decrement"})}>-1</button>
    </div>
  )
}
```

我们来看一下，如果我们创建另外一个profile.js也使用这个reducer函数，是否会进行数据的共享：

```react
import React, { useReducer } from 'react'
import { counterReducer } from '../reducer/counter'

export default function Profile() {
  const [state, dispatch] = useReducer(counterReducer, {counter: 0});

  return (
    <div>
      <h2>当前计数: {state.counter}</h2>
      <button onClick={e => dispatch({type: "increment"})}>+1</button>
      <button onClick={e => dispatch({type: "decrement"})}>-1</button>
    </div>
  )
}
```

数据是不会共享的，它们只是使用了相同的counterReducer的函数而已。

> 所以，useReducer只是useState的一种替代品，并不能替代Redux。

### 3.3. useCallback

useCallback实际的目的是为了进行性能的优化。

如何进行性能的优化呢？

- useCallback会返回一个函数的 memoized（记忆的） 回调函数；
- 在依赖不变的情况下，多次定义的时候，返回的值是相同的；

```react
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b]
);
```

我们来看下面一段很有趣的代码：

- increment1在每次函数组件重新渲染时，会返回相同的值；
- increment2每次定义的都是不同的值；
- 问题：是否increment1会比increment2更加节省性能呢？
  - 事实上，经过一些测试，并没有更加节省内存，因为useCallback中还是会传入一个函数作为参数；
  - 所以并不存在increment2每次创建新的函数，而increment1不需要创建新的函数这种性能优化；

- 那么，为什么说useCallback是为了进行性能优化呢？

```react
import React, { memo, useState, useCallback } from 'react'

export default function CallbackHookDemo() {
  const [count, setCount] = useState(0);

  // 闭包陷阱: useCallback
  const increment1 = useCallback(function increment() {
    setCount(count + 1);
  }, []);

  const increment2 = function() {
    setCount(count + 1);
  }

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <button onClick={increment1}>+1</button>
      <button onClick={increment2}>+1</button>
    </div>
  )
}
```

我们来对上面的代码进行改进：

- 在下面的代码中，我们将回调函数传递给了子组件，在子组件中会进行调用；
- 在发生点击时，我们会发现接受increment1的子组件不会重新渲染，但是接受increment2的子组件会重新渲染；
- 所以useCallback最主要用于性能渲染的地方应该是和memo结合起来，决定子组件是否需要重新渲染；

```react
import React, { memo, useState, useCallback } from 'react';

// useCallback性能优化的点:
// 1.当需要将一个函数传递给子组件时, 最好使用useCallback进行优化, 将优化之后的函数, 传递给子组件

// props中的属性发生改变时, 组件本身就会被重新渲染
const CounterIncrement = memo((props) => {
  console.log("CounterIncrment被渲染:", props.name);
  return <button onClick={props.increment}>+1</button>
})

export default function CallbackHookDemo() {
  const [count, setCount] = useState(0);

  // 闭包陷阱: useCallback
  const increment1 = useCallback(function increment() {
    setCount(count + 1);
  }, []);

  const increment2 = function() {
    setCount(count + 1);
  }
  
  // 进一步的优化: 当count发生改变时, 也使用同一个函数(了解)
  // 做法一: 将count依赖移除掉, 缺点: 闭包陷阱
  // 做法二: useRef, 在组件多次渲染时, 返回的是同一个值
  const countRef = useRef();
  countRef.current = count;
  const increment = useCallback(function foo() {
    setCount(countRef.current + 1);
  }, []);

  return (
    <div>
      <h2>当前计数: {count}</h2>
      {/* <button onClick={increment1}>+1</button>
      <button onClick={increment2}>+1</button> */}
      <CounterIncrement increment={increment1} name="increment1"/>
      <CounterIncrement increment={increment2} name="increment2"/>
    </div>
  )
}
```

> 通常使用useCallback的目的是不希望子组件进行多次渲染，并不是为了函数进行缓存；

### 3.4. useMemo

useMemo实际的目的也是为了进行性能的优化。

如何进行性能的优化呢？

- useMemo返回的也是一个 memoized（记忆的） 值；
- 在依赖不变的情况下，多次定义的时候，返回的值是相同的；
- `useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`

```react
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

我们来看一个案例：

- 无论我们点击了是 `+1`还是 `切换` 案例都会重新计算一次；
- 事实上，我们只是希望在count发生变化时重新计算；

```react
import React, { useState, useMemo } from 'react';

function calcNum(count) {
  let total = 0;
  for (let i = 0; i < count; i++) {
    total += i;
  }
  console.log("计算一遍");
  return total
}

export default function MemoHookDemo() {
  const [count, setCount] = useState(10);
  const [isLogin, setIsLogin] = useState(true);

  const total = calcNum(count);

  return (
    <div>
      <h2>数字和: {total}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      {isLogin && <h2>Coderwhy</h2>}
      <button onClick={e => setIsLogin(!isLogin)}>切换</button>
    </div>
  )
}
```

这个时候，我们可以使用useMemo来进行性能的优化：

```react
import React, { useState, useMemo } from 'react';

function calcNum(count) {
  let total = 0;
  for (let i = 0; i < count; i++) {
    total += i;
  }
  console.log("计算一遍");
  return total
}

export default function MemoHookDemo() {
  const [count, setCount] = useState(10);
  const [isLogin, setIsLogin] = useState(true);

  const total = useMemo(() => {
    return calcNum(count);
  }, [count]);

  return (
    <div>
      <h2>数字和: {total}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      {isLogin && <h2>Coderwhy</h2>}
      <button onClick={e => setIsLogin(!isLogin)}>切换</button>
    </div>
  )
}
```

当然，useMemo也可以用于子组件的性能优化：

- **ShowCounter子组件依赖的是一个基本数据类型，所以在比较的时候只要值不变，那么就不会重新渲染；**
- **ShowInfo接收的是一个对象，每次都会定义一个新的对象，所以我们需要通过useMemo来对其进行优化；**

```react
import React, { useState, useMemo, memo } from 'react';

function calcNum(count) {
  let total = 0;
  for (let i = 0; i < count; i++) {
    total += i;
  }
  console.log("计算一遍");
  return total
}

const ShowCounter = memo((props) => {
  console.log("重新渲染");
  return <h1>Counter: {props.total}</h1>
})

const ShowInfo = memo((props) => {
  console.log("ShowInfo重新渲染");
  return <h1>信息: {props.info.name}</h1>
})

export default function MemoHookDemo() {
  const [count, setCount] = useState(10);
  const [isLogin, setIsLogin] = useState(true);

  const total = useMemo(() => {
    return calcNum(count);
  }, [count]);

  const info = useMemo(() => {
    return {name: "why"}
  }, [])

  return (
    <div>
      <h2>数字和: {total}</h2>
      <ShowCounter total={total} />
      <ShowInfo info={info}/>
      <button onClick={e => setCount(count + 1)}>+1</button>
      {isLogin && <h2>Coderwhy</h2>}
      <button onClick={e => setIsLogin(!isLogin)}>切换</button>
    </div>
  )
}
```

### 3.5. useRef

useRef返回一个ref对象，返回的ref对象在组件的整个生命周期保持不变。

最常用的ref是两种用法：

- 用法一：引入DOM（或者组件，但是需要是class组件）元素；
- 用法二：保存一个数据，这个对象在整个生命周期中可以保存不变；

#### 3.5.1.用法一：引用DOM

```react
import React, { memo, useRef } from "react";

const App = memo(() => {
  const titleRef = useRef();
  const inputRef = useRef();

  function showTitleDom() {
    console.log(titleRef.current);
    inputRef.current.focus();
  }

  return (
    <div>
      <h2 ref={titleRef}>Hello World</h2>
      <input type="text" ref={inputRef} />
      <button onClick={showTitleDom}>查看title的dom</button>
    </div>
  );
});

export default App;
```

#### 3.5.2.用法二：使用ref保存上一次的某一个值

- useRef可以想象成在ref对象中保存了一个.current的可变盒子；
- useRef在组件重新渲染时，返回的依然是之前的ref对象，但是current是可以修改的；

```react
import React, { memo, useRef, useCallback, useState } from "react";

let obj = null;

const App = memo(() => {
  const [count, setCount] = useState(0);
  const nameRef = useRef();
  console.log(obj === nameRef); // true
  obj = nameRef;

  // 通过useRef解决闭包陷阱
  const countRef = useRef();
  countRef.current = count;
  const increment = useCallback(() => {
    setCount(countRef.current + 1);
  }, []);

  return (
    <div>
      <h2>Hello World: {count}</h2>
      <button onClick={(e) => setCount(count + 1)}>+1</button>
      <button onClick={increment}>+1</button>
    </div>
  );
});

export default App;
```

### 3.6. useImperativeHandle

useImperativeHandle并不是特别好理解，我们一点点来学习。

我们先来回顾一下ref和forwardRef结合使用：

- 通过forwardRef可以将ref转发到子组件；
- 子组件拿到父组件中创建的ref，绑定到自己的某一个元素中；

```react
import React, { useRef, forwardRef } from 'react';

const HYInput = forwardRef(function (props, ref) {
  return <input type="text" ref={ref}/>
})

export default function ForwardDemo() {
  const inputRef = useRef();

  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
    </div>
  )
}
```

上面的做法本身没有什么问题，但是我们是将子组件的DOM直接暴露给了父组件：

- 直接暴露给父组件带来的问题是某些情况的不可控；
- 父组件可以拿到DOM后进行任意的操作；
- 但是，事实上在上面的案例中，我们只是希望父组件可以操作的focus，其他并不希望它随意操作；

通过useImperativeHandle可以只暴露固定的操作：

- 通过useImperativeHandle的Hook，将`传入的ref`和`useImperativeHandle第二个参数返回的对象`绑定到了一起；
- 所以在父组件中，使用 `inputRef.current`时，实际上使用的是`返回的对象`；
- 比如我调用了 `focus函数`，甚至可以调用 `printHello函数`；

```react
import React, { useRef, forwardRef, useImperativeHandle } from 'react';

const HYInput = forwardRef(function (props, ref) {
  // 创建组件内部的ref
  const inputRef = useRef();
  
  // 子组件对父组件传入的ref进行处理
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    printHello: () => {
      console.log("Hello World")
    }
  }))

  // 这里绑定的是组件内部的inputRef
  return <input type="text" ref={inputRef}/>
})

export default function ImperativeHandleHookForwardDemo() {
  const inputRef = useRef();

  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
      <button onClick={e => inputRef.current.printHello()}>Hello World</button>
    </div>
  )
}
```

### 3.7. useLayoutEffect

useLayoutEffect看起来和useEffect非常的相似，事实上他们也只有一点区别而已：

- useEffect会在渲染的内容更新到DOM上后执行，不会阻塞DOM的更新；
- useLayoutEffect会在渲染的内容更新到DOM上之前执行，会阻塞DOM的更新；

如果我们希望在某些操作发生之后再更新DOM，那么应该将这个操作放到useLayoutEffect。

我们来看下面的一段代码：

- 这段代码在开发中会发生闪烁的现象；
- 因为我们先将count设置为了0，那么DOM会被更新，并且会执行一次useEffect中的回调函数；
- 在useEffect中我们发现count为0，又执行一次setCount操作，那么DOM会再次被更新，并且useEffect又会被执行一次；

```react
import React, { useEffect, useState, useLayoutEffect } from 'react';

export default function EffectHookDemo() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    if (count === 0) {
      setCount(Math.random()*200)
    }
  }, [count]);

  return (
    <div>
      <h2>当前数字: {count}</h2>
      <button onClick={e => setCount(0)}>随机数</button>
    </div>
  )
}
```

事实上，我们上面的操作的目的是在count被设置为0时，随机另外一个数字：

- 如果我们使用useLayoutEffect，那么会等到useLayoutEffect代码执行完毕后，再进行DOM的更新；

```react
import React, { useEffect, useState, useLayoutEffect } from 'react';

export default function EffectHookDemo() {
  const [count, setCount] = useState(0);

  useLayoutEffect(() => {
    if (count === 0) {
      setCount(Math.random()*200)
    }
  }, [count]);

  return (
    <div>
      <h2>当前数字: {count}</h2>
      <button onClick={e => setCount(0)}>随机数</button>
    </div>
  )
}
```

![image-20230510223315266](.\img\image-20230510223315266.png)

## 四. 自定义Hook

### 4.1. 认识自定义hook

**自定义Hook本质上只是一种函数代码逻辑的抽取，严格意义上来说，它本身并不算React的特性。**

需求：所有的组件在创建和销毁时都进行打印

- 组件被创建：打印 `组件被创建了`；
- 组件被销毁：打印 `组件被销毁了`；

```react
export default function CustomHookDemo() {
  useEffect(() => {
    console.log("组件被创建了");
    return () => {
      console.log("组件被销毁了");
    }
  }, [])

  return (
    <div>
      <h2>CustomHookDemo</h2>
    </div>
  )
}
```

但是这样来做意味着所有的组件都需要有对应的逻辑：

```react
function Home(props) {
  useEffect(() => {
    console.log("组件被创建了");
    return () => {
      console.log("组件被销毁了");
    }
  }, [])
  return <h2>Home</h2>
}

function Profile(props) {
  useEffect(() => {
    console.log("组件被创建了");
    return () => {
      console.log("组件被销毁了");
    }
  }, [])
  return <h2>Profile</h2>
}
```

如何可以对它们的逻辑进行抽取呢？

- 我们可能希望抽取到一个函数中；

```react
function loggingLife() {
  useEffect(() => {
    console.log("组件被创建了");
    return () => {
      console.log("组件被销毁了");
    }
  }, [])
}
```

但是，抽取到这里调用之后，代码是报错的：

- 原因是普通的函数中不能使用hook

那么，我们应该如何操作呢？

- 非常简单，函数以特殊的方式命名，以 `use` 开头即可；

```react
function useLoggingLife() {
  useEffect(() => {
    console.log("组件被创建了");
    return () => {
      console.log("组件被销毁了");
    }
  }, [])
}
```

当然，自定义Hook可以有参数，也可以有返回值：

```react
function useLoggingLife(name) {
  useEffect(() => {
    console.log(`${name}组件被创建了`);
    return () => {
      console.log(`${name}组件被销毁了`);
    }
  }, [])
}
```

### 4.2. 自定义Hook练习

我们通过一些案例来练习一下自定义Hook。

**使用User、Token的Context**

比如多个组件都需要使用User和Token的Context：

- 这段代码我们在每次使用user和token时都需要导入对应的Context，并且需要使用两次useContext；

```react
import React, { useContext } from 'react'
import { UserContext, TokenContext } from '../App'

export default function CustomHookContextDemo() {
  const user = useContext(UserContext);
  const token = useContext(TokenContext);

  console.log(user, token);

  return (
    <div>
      <h2>CustomHookContextDemo</h2>
    </div>
  )
}
```

我们可以抽取到一个自定义Hook中：

```react
function useUserToken() {
  const user = useContext(UserContext);
  const token = useContext(TokenContext);

  return [user, token];
}
```

**获取窗口滚动的位置**

在开发中，某些场景我们可能总是希望获取创建滚动的位置：

```react
import React, { useEffect, useState } from 'react'

export default function CustomScrollPositionHook() {

  const [scrollPosition, setScrollPosition] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScrollPosition(window.scrollY);
    }
    document.addEventListener("scroll", handleScroll);

    return () => {
      document.removeEventListener("scroll", handleScroll);
    }
  }, [])

  return (
    <div style={{padding: "1000px 0"}}>
      <h2 style={{position: "fixed", top: 0, left: 0}}>CustomScrollPositionHook: {scrollPosition}</h2>
    </div>
  )
}
```

但是如果每一个组件都有对应这样的一个逻辑，那么就会存在很多的冗余代码：

```react
function useScrollPosition() {
  const [scrollPosition, setScrollPosition] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScrollPosition(window.scrollY);
    }
    document.addEventListener("scroll", handleScroll);

    return () => {
      document.removeEventListener("scroll", handleScroll);
    }
  }, [])

  return scrollPosition;
}
```

**数据存储的localStorage**

在开发中，我们会有一些数据希望通过localStorage进行存储（当然，你可以根据自己的情况选择sessionStorage）

```react
import React, { useState, useEffect } from 'react'

export default function CustomDataStoreHook() {
  const [name, setName] = useState(() => {
    return JSON.parse(window.localStorage.getItem("name"))
  });

  useEffect(() => {
    window.localStorage.setItem("name", JSON.stringify(name));
  }, [name])

  return (
    <div>
      <h2>CustomDataStoreHook: {name}</h2>
      <button onClick={e => setName("coderwhy")}>设置name</button>
    </div>
  )
}
```

如果每一个里面都有这样的逻辑，那么代码就会变得非常冗余：

```react
function useLocalStorange(key) {
  const [data, setData] = useState(() => {
    const item = localStorage.getItem(key);
    if (!item) return "";
    return JSON.parse(window.localStorage.getItem(key))
  });

  useEffect(() => {
    window.localStorage.setItem(key, JSON.stringify(data));
  }, [data]);

  return [data, setData];
}
```

## 五. Redux Hooks

在之前的redux开发中，为了让组件和redux结合起来，我们使用了react-redux中的connect：

- 但是这种方式必须使用`高阶函数`结合返回的`高阶组件`；
- 并且必须编写：`mapStateToProps`和 `mapDispatchToProps`映射的函数；

```react
import React, { memo } from 'react'
import { connect } from "react-redux"
import { addNumberAction, subNumberAction } from './store/modules/counter'

const App = memo((props) => {
  const { count, addNumber, subNumber } = props

  function addNumberHandle(num, isAdd = true) {
    if (isAdd) {
      addNumber(num)
    } else {
      subNumber(num)
    }
  }

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <button onClick={e => addNumberHandle(1)}>+1</button>
      <button onClick={e => addNumberHandle(6)}>+6</button>
      <button onClick={e => addNumberHandle(6, false)}>-6</button>
    </div>
  )
})

const mapStateToProps = (state) => ({
  count: state.counter.count
})

const mapDispatchToProps = (dispatch) => ({
  addNumber(num) {
    dispatch(addNumberAction(num))
  },
  subNumber(num) {
    dispatch(subNumberAction(num))
  }
})

export default connect(mapStateToProps, mapDispatchToProps)(App)
```

在Redux7.1开始，提供了Hook的方式，我们再也不需要编写connect以及对应的映射函数了

### 5.1. useSelector使用

useSelector的作用是将state映射到组件中：

- 参数一：将state映射到需要的数据中；
- 参数二：可以进行比较来决定是否组件重新渲染；（后续讲解）

```react
const result: any = useSelector(selector: Function, equalityFn?: Function)
```

现在，我可以改进一下之前使用redux的代码：

```react
import React, { memo } from 'react'
import { useSelector, useDispatch, shallowEqual } from "react-redux"
import { addNumberAction, changeMessageAction, subNumberAction } from './store/modules/counter'

// memo高阶组件包裹起来的组件有对应的特点: 只有props发生改变时, 才会重新渲染
const Home = memo((props) => {
  const { message } = useSelector((state) => ({
    message: state.counter.message
  }), shallowEqual)

  const dispatch = useDispatch()
  function changeMessageHandle() {
    dispatch(changeMessageAction("你好啊, 师姐!"))
  }

  return (
    <div>
      <h2>Home: {message}</h2>
      <button onClick={e => changeMessageHandle()}>修改message</button>
    </div>
  )
})

const App = memo((props) => {
  // 1.使用useSelector将redux中store的数据映射到组件内
  const { count } = useSelector((state) => ({
    count: state.counter.count
  }), shallowEqual)

  // 2.使用dispatch直接派发action
  const dispatch = useDispatch()
  function addNumberHandle(num, isAdd = true) {
    if (isAdd) {
      dispatch(addNumberAction(num))
    } else {
      dispatch(subNumberAction(num))
    }
  }

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <button onClick={e => addNumberHandle(1)}>+1</button>
      <button onClick={e => addNumberHandle(6)}>+6</button>
      <button onClick={e => addNumberHandle(6, false)}>-6</button>
      <Home/>
    </div>
  )
})

export default App
```

但是这段代码会有一个问题：

- 当前我们的组件并不依赖counter，但是counter发生改变时，依然会引起Home的重新渲染；

原因是什么呢？

- useSelector默认会比较我们返回的两个对象是否相等；
- 如何比较呢？`const refEquality = (a, b) => a === b`；
- 也就是我们必须返回两个完全相等的对象才可以不引起重新渲染；

这个时候，我们可以使用react-redux中给我们提供的 shallowEqual：

- 这段代码的作用是避免不必要的重新渲染；

```react
const { count } = useSelector(
  (state) => ({
    count: state.counter.count,
  }),
  shallowEqual
);
```

当然，你也可以编写自己的比较函数，来决定是否重新渲染。

### 5.2. useDispatch

useDispatch非常简单，就是直接获取dispatch函数，之后在组件中直接使用即可：

```react
const dispatch = useDispatch()
```

直接使用dispatch：

```react
<button onClick={e => dispatch(subAction(1))}>-1</button>
<button onClick={e => dispatch(subAction(5))}>-5</button>
```

我们还可以通过useStore来获取当前的store对象：

```react
const store = useStore()
```

在组件中可以使用store：

```react
const store = useStore();
console.log(store.getState());
```

## 六.其它Hooks

### 6.1.useId

官方的解释：useId 是一个用于生成横跨服务端和客户端的稳定的唯一 ID 的同时避免 hydration 不匹配的 hook。

这里有一个词叫hydration，要想理解这个词，我们需要理解一些服务器端渲染（SSR）的概念。

什么是SSR？

- SSR（Server Side Rendering，服务端渲染），指的是页面在服务器端已经生成了完成的HTML页面结构，不需要浏览器通过执行JS代码创建页面结构；
- 对应的是CSR（Client Side Rendering，客户端渲染），我们开发的SPA页面通常依赖的就是客户端渲染；

早期的服务端渲染包括PHP、JSP、ASP等方式，但是在目前前后端分离的开发模式下，前端开发人员不太可能再去学习PHP、JSP等技术来开发网页；

不过我们可以借助于Node来帮助我们执行JavaScript代码，提前完成页面的渲染；

![image-20230512204401545](.\img\image-20230512204401545.png)

**SSR同构应用**

什么是同构？

- 一套代码既可以在服务端运行又可以在客户端运行，这就是同构应用。

同构是一种SSR的形态，是现代SSR的一种表现形式。

- 当用户发出请求时，先在服务器通过SSR渲染出首页的内容。
- 但是对应的代码同样可以在客户端被执行。
- 执行的目的包括事件绑定等以及其他页面切换时也可以在客户端被渲染；

![image-20230512204731289](.\img\image-20230512204731289.png)

![image-20230512204747494](.\img\image-20230512204747494.png)

**Hydration**

什么是Hydration？这里我引入vite-plugin-ssr插件的官方解释。

![image-20230512205124935](.\img\image-20230512205124935.png)

在进行 SSR 时，我们的页面会呈现为 HTML。

- 但仅 HTML 不足以使页面具有交互性。例如，浏览器端 JavaScript 为零的页面不能是交互式的（没有 JavaScript 事件处理程序来响应用户操作，例如单击按钮）。

- 为了使我们的页面具有交互性，除了在 Node.js 中将页面呈现为 HTML 之外，我们的 UI 框架（Vue/React/...）还在浏览器中加载和呈现
  页面。（它创建页面的内部表示，然后将内部表示映射到我们在 Node.js 中呈现的 HTML 的 DOM 元素。）

这个过程称为hydration。

**useId的作用**

我们再来看一遍：useId 是一个用于生成横跨服务端和客户端的稳定的唯一 ID 的同时避免 hydration 不匹配的 hook。

所以我们可以得出如下结论：

- useId是用于react的同构应用开发的，前端的SPA页面并不需要使用它；
- useId可以保证应用程序在客户端和服务器端生成唯一的ID，这样可以有效的避免通过一些手段生成的id不一致，造成hydration mismatch；

```react
import React, { memo, useId, useState } from "react";

const App = memo(() => {
  const [count, setCount] = useState(0);

  const id = useId();
  console.log(id);

  return (
    <div>
      <button onClick={(e) => setCount(count + 1)}>count+1:{count}</button>
      <label htmlFor={id}>
        用户名:
        <input id={id} type="text" />
      </label>
    </div>
  );
});

export default App;
```

### 6.2.useTransition

官方解释：返回一个状态值表示过渡任务的等待状态，以及一个启动该过渡任务的函数。

useTransition到底是干嘛的呢？它其实在告诉react对于某部分任务的更新优先级较低，可以稍后进行更新。

```react
import React, { memo, useState, useTransition } from "react";
import namesArray from "./namesArray";

const App = memo(() => {
  const [showNames, setShowNames] = useState(namesArray);
  const [pending, startTransition] = useTransition();

  function valueChangeHandle(event) {
    startTransition(() => {
      const keyword = event.target.value;
      const filterShowNames = namesArray.filter((item) =>
        item.includes(keyword)
      );
      setShowNames(filterShowNames);
    });
  }

  return (
    <div>
      <input type="text" onInput={valueChangeHandle} />
      <h2>用户名列表: {pending && <span>data loading</span>} </h2>
      <ul>
        {showNames.map((item, index) => {
          return <li key={index}>{item}</li>;
        })}
      </ul>
    </div>
  );
});

export default App;
```

```react
import { faker } from "@faker-js/faker";

const namesArray = [];
for (let i = 0; i < 10000; i++) {
  namesArray.push(faker.name.fullName());
}

export default namesArray;
```

### 6.3.useDeferredValue

官方解释：useDeferredValue 接受一个值，并返回该值的新副本，该副本将推迟到更紧急地更新之后。

在明白了useTransition之后，我们就会发现useDeferredValue的作用是一样的效果，可以让我们的更新延迟。

```react
import React, { memo, useState, useDeferredValue } from "react";
import namesArray from "./namesArray";

const App = memo(() => {
  const [showNames, setShowNames] = useState(namesArray);
  const deferedShowNames = useDeferredValue(showNames);

  function valueChangeHandle(event) {
    const keyword = event.target.value;
    const filterShowNames = namesArray.filter((item) => item.includes(keyword));
    setShowNames(filterShowNames);
  }

  return (
    <div>
      <input type="text" onInput={valueChangeHandle} />
      <h2>用户名列表: </h2>
      <ul>
        {deferedShowNames.map((item, index) => {
          return <li key={index}>{item}</li>;
        })}
      </ul>
    </div>
  );
});

export default App;
```









