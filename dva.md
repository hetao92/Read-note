[TOC]

# Dva

```js
import dva from 'dva';
import { createBrowserHistory } from "history";

const App = () => <div>Hello dva</div>;

// 创建应用
const app = dva({
  hitstory: createBrowserHistory(), //默认 hashHistory
});
// 配置 hooks 或者注册插件
app.use(hooks) 
// 注册视图
app.router(() => <App />);
// 启动应用
app.start('#root');
```

dva options 有 `history`, `initialState`, `onError`, `onAction`, `onStateChange`, `onReducer`, `onEffect`, `onHmr`, `extraReducers`,  `extraEnhancers`

## 核心概念

- State：一个对象，保存整个应用状态

- View：React 组件构成的视图层

- Action：一个对象，描述事件

- connect 方法：一个函数，绑定 State 到 View

  函数的第一个参数是 `mapStateToProps`函数，返回对象，用于建立 state 到 props 的映射关系

  函数最后返回一个容器组件，即在原始 UI 组件的外面包了一层 state 

  ```js
  import { connect } from 'dva';
  
  function mapStateToProps(state) {
    return { todos: state.todos };
  }
  connect(mapStateToProps)(App);
  ```

- dispatch 方法：一个函数，发送 Action 到 State



**Model**

```js
// 注册 Model
app.model({
  namespace: 'count',
  state: 0,		// 该 model 当前的数据状态
  reducers: {	// Action 处理器，处理同步动作
    add(state, action) { return state + 1 },
  },
  effects: {	//Action 处理器，处理异步动作
    *addAfter1Second(action, { call, put }) {
      yield call(delay, 1000);
      yield put({ type: 'add' });
    },
  },
  subscriptions: {
    setup({ history, dispatch }) {
      // 监听 history 变化，当进入 `/` 时触发 `load` action
      return history.listen(({ pathname }) => {
        if (pathname === '/') {
          dispatch({ type: 'load' });
        }
      });
    },
  },
});
```

**namespace**

model 的命名空间，同时也是他在全局 state 上的属性，只能用字符串，不支持通过 `.` 的方式创建多层命名空间。



**state**

初始值，优先级上低于初始化`dva()`时传入的`opts.initialState`



**reducers**

处理同步操作，是唯一可以修改 state 的地方，由 action 触发。格式为 `(state, action) => newState` 或 `[(state, action) => newState, enhancer]`。



**effects**

处理异步操作和业务逻辑，不能直接修改 state。格式为 `*(action, effects) => void` 或 `[*(action, effects) => void, { type }]`。type 具体有 `takeEvery`, `takeLatest`, `throttle`, `watcher`



**subscriptions**

订阅一个数据源，并根据需要 dispatch 响应的 action，在`app.start()`时被执行，数据源可以是当前时间，服务器的 websocket 连接，keyboard 输入，geolocation 变化，history 路有变化等

格式为 `({ dispatch, history }, done) => unlistenFunction`。