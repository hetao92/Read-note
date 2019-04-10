[TOC]

# Vuex

> 包含应用中大部分状态(state)的容器(store)
>
> 特点： 状态存储是**响应式**的；改变 store 中的状态需要显示地提交(commit)，而不能直接改变
>
> 响应式意味着
>
> + 提前在 store 内初始化所有属性
> + 在对象上添加新属性使用`Vue.set(obj, key, val)`



## State

> 单一状态树，唯一数据源

### mapState

在组件内获取多个状态时利用`mapState`辅助函数生成计算属性，可以精简代码

```js
import { mapState } from 'vuex' //需要引入
computed: mapState({
  // 箭头函数可使代码更简练
  count: state => state.count,

  // 传字符串参数 'count' 等同于 `state => state.count`
  countAlias: 'count',

  // 为了能够使用 `this` 获取局部状态，必须使用常规函数
  countPlusLocalState (state) {
    return state.count + this.localCount
  }
})
//当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```



## Getter

> 当需要对 state 中数据进行处理派生出其他状态数据(如过滤等)并在多个组件内应用到时，，低效的方法是复制函数或者抽取公共函数
>
> 而 getter 就像计算属性一样，getter 的返回值会根据它的依赖被**缓存**起来，且只有当它的依赖值发生了改变才会被重新计算。

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    //函数参数 state getters(其他 getter)
    doneTodos: (state, getters) => {
      return state.todos.filter(todo => todo.done)
    },
    //也可以返回函数，但此时不会缓存结果，每次都会调用
    getTodoById: (state) => (id) => {
    	return state.todos.find(todo => todo.id === id)
  	}
  }
})
//组件内有store.getters对象暴露
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]

//使用 mapGetters 
import { mapGetters } from 'vuex'
computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
  ...mapGetters([
    'doneTodosCount',
    'anotherGetter',
    // ...
  ])
}
```



## Mutation

> 更改状态的唯一方法
>
> 必须是同步事务，但不是说不能放异步操作，是为了能用 devtools追踪状态变化，混合异步调用会难以调试

```js
//事件类型 type -> increment 
//回调
mutations: {
  increment (state) {
    // 变更状态
    state.count++
  }
}

store.commit('increment')
//commit 可以传额外参数(载荷 payload)
store.commit('increment', 10)
//对象风格
store.commit({
  type: 'increment',
  amount: 10
})

//mapMutations
import { mapMutations } from 'vuex'
methods: {
  ...mapMutations([
    'increment', // 将 `this.increment()` 映射为`this.$store.commit('increment')`

    // `mapMutations` 也支持载荷：
    'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
  ]),
    ...mapMutations({
    add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
  })
}
```



##Action

> Action 提交的是 mutation，而不是直接变更状态。
>
> Action 可以包含任意异步操作

```js
const store = new Vuex.Store({
  actions: {
    // context 对象与 store 实例具有相同方法和属性
    increment (context) {
      context.commit('increment')
      //也可以使用 context.state 和 context.getters 获取 state 和 getters
      //context 可以使用参数结构 {commit, state ...}
    }
  }
})

store.dispatch('increment')
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})

//mapActions 同 mapMutations

//store.dispatch` 仍旧返回 Promise
store.dispatch('actionA').then(() => {
  // ...
})
//也可以在 action 里使用
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}

//也可以使用 async/ await，假设 getData()返回的是 Promise
 async actionA ({ commit }) {
    commit('gotData', await getData())
  },
```



## Module

由于使用单一状态树，这在复杂情况下会相当臃肿

因此可以将 store 分割成模块，每个模块有自己的 state、mutation、action、getter

对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象

对于 action，context.state 为局部状态，context.rootState为根节点状态

对于 getter，根节点会作为第三个参数 (state, getters, rootState)



###命名空间

默认情况下，模块内部的 action、mutation、getter 均注册在全局命名空间，使得多个模块能够对同一 mutation 或 action 作出响应。

可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块，使模块具有更高的封装度和复用性

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,
      // 模块内容（module assets）
      state: { ... }, // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
    }          
  }
})
//使用全局 state 和 getter，rootState 和 rootGetter 会作为第三和第四参数传入 getter，也会通过 context 对象的属性传入 action。
//若需要在全局命名空间内分发 action 或提交 mutation，将 { root: true } 作为第三参数传给 dispatch 或 commit 即可。
dispatch('someOtherAction') // -> 'foo/someOtherAction'
dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

commit('someMutation') // -> 'foo/someMutation'
commit('someMutation', null, { root: true }) // -> 'someMutation'
//注册全局 action，你可添加 root: true，并将这个 action 的定义放在函数 handler 中。例如：
    actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
```



## Store构造器

`state`

`mutations`

`actions`

`getters`

`modules`

`plugins`

`strict`boolean  是否进入严格模式，任何 mutation处理函数以外修改 state 的操作都会抛出错误

`devtools` boolean	为某个特定的 Vuex 实例打开或关闭 devtools插件



**Store 实例方法**

`commit`

`dispatch`

`replaceState(state: Object)`替换 store 根状态，仅用状态合并或时光旅行调试。

`watch(fn: Function, callback: Function, options?: Object): Function`响应式地侦听 `fn` 的返回值，当值改变时调用回调函数。`fn` 接收 store 的 state 作为第一个参数，其 getter 作为第二个参数。最后接收一个可选的对象参数表示 Vue 的 [`vm.$watch`](https://cn.vuejs.org/v2/api/#vm-watch) 方法的参数。

`subscribe`

`subscribeAction`

`registerModule`

`unregisterModule`

`hotUpdate`