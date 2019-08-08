[TOC]

# Redux

JavaScript状态容器，提供可预测化的状态管理。负责组织管理 state

**单一数据源**

整个应用的 `state` 被储存在一棵 object tree 中，且只存在于唯一一个 store 中

**State 是只读的**

唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象

**使用纯函数来执行修改**

编写 reduces 来描述 action 如何改变 state tree

**单向数据流**



## Action

> Action 是把数据从应用传递到 store 的有效载荷，是 store 数据的唯一来源。

action 本质上是一个 JavaScript 普通对象。通常约定 action 内需要使用一个字符串类型的 `type` 字段来表示**要执行的动作**，可以使用单独的模块或文件来存放。

```react
const ADD_TOTO = 'ADD_TODO'
{
	type: ADD_TODO,
	text: 'Test add todo'
}
```



**Action 创建函数**

这是生成 action 的方法

```react
function addTodo(text) {
	return {
		type: ADD_TOTO,
		text
	}
}
```



## Reducer

> 指定应用状态的变化如何响应 actions 并发送到 store

接收旧的 state 和 action 的纯函数，并返回新的 state

Reducer 和 `Array.reduce`类似，因此

+ **不要在里面直接修改传入的参数**

+ **不要执行有副作用的操作，比如 API 请求、路由跳转等**

+ **不要调用费纯函数，如`Date.now()`**



当不同功能的 reducer 进行拆分时以一个函数作为主 reducer，来调用多个子 reducer

```js
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
   	//... other type
    default:
      return state
  }
}

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}

function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
//redux 专门提供 combineReducers() 工具类来实现合并
import { combineReducers } from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```



## Store

Store 将 state 与 action 相关联起来

+ 维持应用的 state
+ 提供 `getState()` 来获取 state
+ 提供 `dispatch(action)` 来更新 state
+ 通过 `subscribe(listener)` 注册监听器
+ 通过 `subscribe(listener)` 返回的函数注销监听器



**创建 store**

```js
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```



**获取 state**

`store.getState()`



**监听与注销**

返回一个函数用于后面注销

`const unsubscribe = store.subscrive(() => console.log(store.getState()))`

`unsubscribe() //注销`



**发起 action**

`store.dispatch(addTodo('Learn about actions'))`



## 异步 action

在调用异步 API 时，会有发起请求的时刻，和接收到响应的时刻。因此每个 API 请求需要 dispatch 至少三种 action

+ 通知 reducer 请求开始的 action

  这种情况下，reducer 可能会切换一下 state 的 `isFetching` 标记来告诉 UI 来显示加载界面

+ 通知 reducer 请求成功的 action

  此时 reducer 会把接收到的新数据合并到 state 中并重置 `isFetching`。 UI 会隐藏加载界面，并显示接收到的数据

+ 通知 reducer 请求失败的 action

  reducer 会重置 `isFetching`， 或许还会保存失败信息并显示在 UI



##middleware & enhancer

middleware 为 redux 的 dispatch 方法添加额外的功能

enhancer 给 redux store 提供额外的功能



**API**

导出类 exports

**createStore(reducer, [preloadedState],[enhancer])**

preloadedState 即 initial state

enhancer 是一个高阶参数，参数是创建 store 的函数，返回值是一个可以创建更强大的 store 的函数

```js
function enhancerCreator() {
  return createStore => (...args) => {
    // do something based old store
    // return a new enhanced store
  }
}
//enhancerCreator 就是个创建 store enhancer 的函数，...args 就是创建 store 所需的参数，也就是(reducer,preloadedState,enhancer)
export default function autoLogger() {
  return createStore => (reducer, initialState, enhancer) => {
    const store = createStore(reducer, initialState, enhancer)
    function dispatch(action) {
      console.log(`dispatch an action: ${JSON.stringify(action)}`);
      const res = store.dispatch(action);
      const newState = store.getState();
      console.log(`current state: ${JSON.stringify(newState)}`);
      return res;
    }
    return {...store, dispatch}
  }
}

```

其实 applyMiddleware 本质也是一个 store enhancer，他最终返回的结构也是`{...store, dispatch}`，所以说中间件主要是用来修改 dispatch 的方法。所以当 applyMiddleware 和 enhancer 一起使用时，可以使用 compose 将两者进行组合

一般来说会通过 middleware 处理异步 action，为了保证其他 enhancer 接收到普通 action，因此需要将`applyMiddleware(...middlewares)`作为第一个参数传给 compose。

**combineReducers(reducers)**

**applyMiddleware(..middlewares)**

**bindActionCreators(actionCreators, dispatch)**

**compose(..functions)**

compose 方向是从右往左



**Store API**

`getState()`

`dispatch(action)`

`subscribe(listener)`

`replaceReducer(nextReducer)`