[TOC]

# Rematch

Rematch 在 Redux 基础上构建并减少了样板代码，同时执行了一些最佳实践，灵感来源于 Dva 和 Mirror

移除的内容主要有

+ 声明 action 类型
+ action 创建函数
+ thunks
+ store 配置
+ mapDispatchToProps
+ sagas



## @rematch/core

**API**

`import {init, dispatch, getState} from '@rematch/core'`

### init

初始化 Rematch 并返回 store

```javascript
const store = init(config)
// config 包含 model, plugins, redux
```



**model**

```js
const example = {
  // model 包含变量 state
  state: { loading: false },
  // 改变 state 的函数对象，以上一次 state 和 payload 为形参，返回 model 的下一个形态。是一个纯函数
  reducer: {
    add: (state, payload) => state + payload,
    // 通过列出'model 名字' + 'action 名字'作为 key，可以监听来自于其他 model 的 action
    'otherModel/actionName': (state, payload) => state + payload
  },
  // effect 处理副作用的函数对象
  effects: {
    logState(payload, rootState) {
      console.log(rootState)
    },
    async loadData(payload, rootState) {
      // wait for data to load
      const response = await fetch('http://example.com/data')
      const data = await response.json()
      // pass the result to a local reducer
      dispatch.example.update(data)
    }
  }
}
```



**plugins**

plugins 用来自定义 init 配置或内部 hooks，他能添加功能到你的 Rematch 设置中

```js
init({
  plugins: [loadingPlugin, persistPlugin]
})
```



**redux**

直接访问 Redux

```js
init({
  redux: {
    middlewares: [reduxLogger],
    reducers: {
      someReducer: (state, action) => ...,
    }
  },
})
```



**store api** 

初始化后返回的 store 的功能

`store.dispatch`

```js
import store from './index'

const { dispatch } = store
// state = { count: 0 }
// reducers
dispatch({ type: 'count/increment', payload: 1 }) // state = { count: 1 }
dispatch.count.increment(1) // state = { count: 2 }

// effects
dispatch({ type: 'count/incrementAsync', payload: 1 }) // state = { count: 3 } after delay
dispatch.count.incrementAsync(1) // state = { count: 4 } after delay
```



`store.getState` 返回该 store 的 state

`store.name` 给 store 提供一个名称，这样当使用多个 store 时，全局的`getState`被调用时这个名字将变成 key



`store.model`

在调用`init`之后，可以延迟加载 model 并将它们合并到 Rematch 中去

```js
const store = init({
  models: {
    count: { state: 0 }
  }
})
store.getState()				// { count: 0 }
store.model({ name: 'countB', state: 99 })
store.getState()				// { count: 0, countB: state: 99}
```



### dispatch

`dispatch.modelName.actionName(any, meta)` 

可以用`store.dispatch`代替

```js
import { dispatch } from '@rematch/core'
dispatch.cart.addToCart(item)
```



第二个可选的属性 meta，可以用于 subscriptions 或 middleware



### action

```js
{ type: 'modelName/actionName', payload: any }
```

Action 是 Redux 中发送的消息，始终是以 model 名称和 action 名称类型的结构



### getState

返回包含所有 store state 的对象

```js
import { init, getState } from '@rematch/core'
const firstStore = init({
  name: 'first',
  models: { count: { state: 0 } }
})
const secondStore = init({
    name: 'second',
    models: { count: { state: 5 } },
})

getState() // { first: { count: 1 }, second: { count: 5 } }
```



## Plugin

## 技巧

在配合 typescript 时，由于 Rematch 经常指定 this 上下文，导致 TS 报错。因此可以在 `tsconfig.json `中指定 `noImplicitThis` 来关闭

```json
// tsconfig.json
{
  "compilerOptions": {
    "noImplicitThis": false,
  }
}
```



重置 state 全部数据

```react
const store = init({
  models,
  redux: {
    devtoolOptions: {},
    rootReducers: { RESET_APP: () => undefined }
  }
});

export default {
  state: {
    ...
  },
  reducers: {
    ...
  },
  effects: (dispatch) => ({
    logout(payload, state) {
      ...
      dispatch({ type: 'RESET_APP' });
    }
  })
};
```