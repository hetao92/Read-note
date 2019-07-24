[TOC]

# Redux-saga

> redux 的一个中间件，就像是应用程序中单独的线程，单独负责处理程序的副作用（比如异步获取数据，访问浏览器缓存等），他可以通过正常的 redux action 从主应用程序启动，暂停和取消，也能访问完整的 redux state，也可以 dispatch redux action



创建 sagas 文件并在主文件中引入

创建 Saga middleware 和要运行的 Sagas

将 Saga middleware 连接至 Redux store



sagas 被实现为 Generator，它会 yield 对象到 redux-saga middleware。该对象是一个类指令，可以被 middleware 解释执行。当 middleware 取得一个 yield 后的 promise，会暂停 saga，直到 promise 完成

```js
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'
import { testSaga } from './sagas'
const store = createStore(
	reducer,
	applyMiddleware(createSagaMiddleware(testSaga))
)
//用 createSagaMiddleware工厂函数来创建 Saga middleware
// 使用 applyMiddleware 将中间件连接至 store
//使用 sagaMiddleware.run(testSaga) 来运行 saga

//另一种写法
import testSaga from './sagas'
const sagaMiddleware = createSagaMiddleware()
const store = createStore(
	reducer,
  applyMiddleware(sagaMiddleware)
)
sagaMiddleware.run(testSaga)
```



```js
import { delay } from 'redux-saga'
import { put, taskEvery, all } from 'redux-saga/effects'
export function* incrementAsync() {
  yield delay(1000)
  yield put({ type: 'INCREMENT' })
}

export function* watchIncrementAsync() {
  yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}
export default function* rootSaga() {
  yield all([
    helloSaga(),
    watchIncrementAsync()
  ])
}
//delay 是一个工具函数，返回一个延迟一定时间再 resolve 的 promise
//put 是告诉 middleware 发起一个某类型的 action
//辅助函数 taskEvery 监听所有匹配的 action，当匹配到时就会执行响应的任务
//all 负责 yield 一个数组，值是调用里面所有 saga 的结果

```



## Middleware

**createSagaMiddleware(options)**

创建 redux middleware，并将 sagas 连接到 redux store

options:

+ sagaMonitor
+ emitter
+ onError



**Middleware.run(saga, …args)**

动态运行 saga，只能用于在`applyMiddleware`后执行 saga

saga 即为一个 Generator 函数

args 为提供给 saga 的参数



##Effect

在 saga generator 里 yield 一些纯 JavaScript 对象来表达 saga 的逻辑，这些对象即是 Effect

它包含一些让 middleware 解释执行的信息，让它来执行某些操作，比如异步操作，发起 action 到 store。 effect 本身不会执行任何其他操作



**take(pattern)**创建 Effect 描述信息，暂停 generator 直到一个匹配的 action 被发起了

pattern

+ 空参数或`*`，则将匹配所有 action

+ 一个函数则匹配返回结果为 true 的 action
+ 字符串 string，则匹配 `action.type === pattern`
+ 数组，则数组中每一项都适用上述规则



**put(action)** 用于创建 dispatch Effect，是非阻塞的，所有向下抛出的错误不会冒泡回到 saga 中

**put.resolve(action)** 类似put，但是是阻塞的



**call(fn, ...args)** 这种会返回一个 Effect, 然后被传递给 next 的调用者，来命令 middleware 以参数 `args` 调用函数 `fn` 。有点和 put 类似

调用对象方法时可以用`call([context, fnName], ...args)`

例如 `yield call([localStorage, 'getItem'], 'redux-saga')`。



**apply(context, fn, [args])**  是 `call([context, fn], ...args)` 的另一种写法。



```javascript
put({type: 'INCREMENT'}) // => { PUT: {type: 'INCREMENT'} }
call(delay, 1000)        // => { CALL: {fn: delay, args: [1000]}}
// effect 类型是 Put，middleware 会 dispatch 一个 action 到 store
// 若是 call，则会调用给定的函数

```

**cps(fn, …args) | cps([context, fn], ...args)**命令 middleware 以 Node 风格的函数方式调用 function。function 是 Node 风格，除了接收自身参数外，还接受一个在执行结束后调用的回调函数(err, success)



**fork(fn, …args) | fork([context, fn], ...args)** fork 一个任务，任务会在后台以**非阻塞形式**启动，调用者可以继续他自己的流程，而不用等待被 fork 的任务结束

因为 take 有一个问题，如果上一个阻塞操作还在阻塞过程中，就触发了 take 相关的操作，则会被错过。此时需要用到 fork

`yield fork(fn ...args)` 的结果是一个 Task对象 —— 一个具备着某些实用方法及属性的对象。

所有 fork 任务都会被附加到他们的父级任务上，当父级任务终止其自身命令的执行，它会在返回之前等待所有 fork 任务终止。

同样，子级任务的错误会自动冒泡到父级任务。



**spawn(fn, …args)**类似 fork，不过创建的任务被分离，彼此独立，不会有影响



**join(…tasks)**命令 middleware 等待之前的一个或多个分叉任务的结果



**cancel(...tasks) **用于取消一个或多个 fork 任务



如果要同时执行多个任务，对于正常的 yield 写法都是按照顺序执行

```javascript
// 正确写法, effects 将会同步执行
const [users, repos] = yield [
  call(fetch, '/users'),
  call(fetch, '/repos')
]
//此时 generator 会被阻塞直到所有的 effects 都执行完毕或其中一个被拒绝(类似 promise.all)
```



**select(selector, …args)**

创建一个 Effect，用来命令 middleware 在当前 Store 的 `state` 上调用指定的选择器（即返回 `selector(getState(), ...args)` 的结果）。

selector  ---  一个 `(state, ...args) => args` 的函数。为空时则取得完整的 state



**actionChannel(pattern,  [buffer])**命令 middleware 通过一个 channel 对匹配到的 action 进行排序，buffer 可以控制如何缓存这些 actions。



**flush(channel)**

创建一个 Effect，用来命令 middleware 从 channel 中冲除所有被缓存的数据。被冲除的数据会返回至 saga，这样便可以在需要的时候再次被利用。



**race **类似 promise.race，同时启动多个任务，无需等待所有任务完成，等第一个被 resolve 或 reject 的任务

**all([...effects]) | all(effects)**

创建一个 Effect 描述信息，用来命令 middleware 并行地运行多个 Effect，并等待它们全部完成。



##辅助函数

**taskEvery`(pattern, saga, ...args)`** 允许多个实例同时启动，即使之前有一个或多个还没有结束，还是可以启动新的任务。也不会对多个任务的响应排序。

在dispatch 到 store 并且匹配 pattern 的每一个 action 上派生一个 saga，args 为传递给启动任务的参数，其中当前的 action 会被追加到参数列表最后一位。

`takeEvery('*')`即可捕获发起的所有类型的 action

`taskEvery` 可以使用`take`和`fork`构建出来



**taskLatest`(pattern, saga, ...args)`** 任何时刻只允许一个任务执行，并且是最后启动的那个，之前已经启动但仍在执行中的任务都会被自动取消



**taskLeading`(pattern, saga, ...args)`** 将在派生一次任务后阻塞，直到派生的 saga 完成，然后再再次开始监听指定的 pattern



**throttle(ms, pattern, saga, …args) ** 节流，在派生一次任务之后，仍将新传入的 action 接收到底层的 buffer 中，至多保留最近的一个，但在 ms 毫秒内将暂停派生新的任务。

