[TOC]

# Vue-Router

> Vue Router 是 Vue 官方的路由管理器。
>
> 监听URL变化，然后匹配路由规则，显示相应页面 

功能点：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为



##动态路由匹配

对于同一组件复用时，原来的组件实例也会被复用，这意味着组件生命周期钩子将不会被复用。因为比起销毁再创建，复用更加高效。

若想作出相应，可以采取以下方法

1. `watch`监听路由`$route`(to,from)变化
2. 利用导航守卫`beforeRouteUpdate (to, from, next)`



通配符`*`匹配任意路径

`path: '*'`	所有路径

`path:'user-*'`	user 开头的任意路径



## 嵌套路由

`<router-view></router-view>`

再在 new VueRouter routes 内 children 参数配置

**以 / 开头的嵌套路径会被当作根路径。**

因此在 children 里 path 可以不用带`'/'`相对路径



## 编程式导航

`$router`即访问路由实例

| 声明式                    | 编程式             |
| ------------------------- | ------------------ |
| `<router-link :to="...">` | `router.push(...)` |

其实点击`<router-link>`也就是在内部调用`router.push(...)`



1. `router.push(location, onComplete?, onAbort?)`

   `onComplete` 和 `onAbort` 回调作为第二个和第三个参数。将会在导航成功完成 (在所有的异步钩子被解析之后) 或终止 (导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由) 的时候进行相应的调用。

   ```javascript
   // 字符串
   router.push('home')
   
   // 对象
   router.push({ path: 'home' })
   
   // 命名的路由
   router.push({ name: 'user', params: { userId: '123' }})
   
   // 带查询参数，变成 /register?plan=private
   router.push({ path: 'register', query: { plan: 'private' }})
   
   //path 搭配 query
   //name 搭配 params
   ```

2. `router.replace(location, onComplete?, onAbort?)`

   replace 不会向 history 添加新记录

3. `router.go(n)`   前进、倒退

   上述三个和 `window.history.pushState/replaceState/go`类似



## 多个视图(非嵌套)

为视图设置 name，没有 name 则默认 default

```html
<router-view name="b"></router-view>
```

在路由设置里配置 components

```javascript
routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
]
```



## 重定向、别名

重定向可以采用路径、命名路由或者一个方法来配置

```js
{ path: '/a', redirect: '/b' }
{ path: '/a', redirect: { name: 'foo' }}
{ path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
}}
```

别名`{ path: '/a', alias: '/b' }`

访问别名时URL保持别名，但路由匹配则按照原名



## 路由组件传参

在组件内调用`$route`会形成高度耦合，可以使用`props`来解耦

比如 template 里本来使用`$route.params.id`，可以使用

`props:['id']`

配置`{ path: '/user/:id', props: true },`

对于包含多个命名视图的路由，需对每个路由都配置

```js
{
  path: '/user/:id',
  components: { default: User, sidebar: Sidebar },
  props: { default: true, sidebar: false }
}
```

###布尔模式

props: true 	`route.params`会被设置为组件属性

### 对象模式

`props: { newsletterPopup: false }`

### 函数模式

`props: (route) => ({ query: route.query.q })`



##hash、History模式

**hash 模式**

> 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，不会向服务器请求数据，利用 hashchange 事件监听到URL变化，从而跳转页面

**history 模式**

> 利用 `history.pushState `API 来完成 URL 跳转而无须重新加载页面。

但 history 需要后台配置，当 URL 未匹配到时能够返回同一个 index 页面

如 Nginx

```js
location / {
  try_files $uri $uri/ /index.html;
}
```



##导航守卫

**参数或查询的改变并不会触发进入/离开的导航守卫**

当一个导航触发时，全局前置守卫按照创建顺序调用。

守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。



###全局前置守卫`router.beforeEach`

`router.beforeEach((to, from, next) => {})`

`to: Route`: 进入的路由目标对象

`from: Route`: 当前即将离开的路由目标对象

`next: Function`: **必须**调用方法来**resolve**钩子

- **next()**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed**(确认的)。
- **next(false)**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
- **next('/') 或者 next({ path: '/' })**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 `router-link` 的 `to` prop或`router.push`中的选项。
- **next(error)**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 `router.onError()` 注册过的回调。



### 全局解析守卫`router.beforeResolve`

在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。



### 全局后置钩子

```js
router.afterEach((to, from) => {
  // ...
})
//与守卫不同，不接受 next
```



### 路由独享守卫

在路由配置里单独配置`beforeEnter`守卫

```js
{
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
```



### 组件内守卫

```js
beforeRouteEnter (to, from, next) {
  // 在渲染该组件的对应路由被 confirm 前调用
  // 不！能！获取组件实例 `this`
  // 因为当守卫执行前，组件实例还没被创建,但是传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
},
beforeRouteUpdate (to, from, next) {
  // 在当前路由改变，但是该组件被复用时调用
  // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
  // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
  // 可以访问组件实例 `this`
},
beforeRouteLeave (to, from, next) {
  // 导航离开该组件的对应路由时调用
  // 可以访问组件实例 `this`
}
```



##路由元信息 meta

```js
{
  path: 'bar',
    component: Bar,
      // a meta field
      meta: { requiresAuth: true }
}
```



`routes` 配置中的每个路由对象为 **路由记录**

路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

一个路由匹配到的所有路由记录会暴露给`$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。



## 过渡特效

```vue
<transition>
  <router-view></router-view>
</transition>
```



## 滚动行为

**只在支持 history.pushState 的浏览器中可用**

在创建Router 实例时配置

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
//第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
//这个方法返回滚动位置的对象信息，长这样：

{ x: number, y: number }
{ selector: string, offset? : { x: number, y: number }}
if (savedPosition) {
  return savedPosition
} else {
  return { x: 0, y: 0 }
}
```



## 路由懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

实现原理：Vue 的异步组件 和 webpack 的代码分割功能

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```js
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```

第二，在 Webpack 2 中，我们可以使用[动态 import](https://github.com/tc39/proposal-dynamic-import)语法来定义代码分块点 (split point)：

```js
import('./Foo.vue') // 返回 Promise

//两者结合即
const Foo = () => import('./Foo.vue')
routes: [
  { path: '/foo', component: Foo }
]
```



想把某个路由下的所有组件都打包在同个异步块 (chunk) 中,使用 命名 chunk

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
//webpack 版本 > 2.4
```



##API

###router-link

`<router-link>`默认渲染`a`标签，可以配置`tag`来生成别的标签

链接元素激活时后又表示激活的 css 类名

若想让激活的class 应用在外层，可以用`router-link`包裹原生`a`标签

```html
<router-link tag="li" to="/foo">
  <a>/foo</a>
</router-link>
```

相比`a`标签的优势：

+ 无论是 history 还是 hash 模式里的表现行为一致，无需改动
+ history 模式下，router-link 会守卫点击事件，让浏览器不再重新加载页面
+ history 模式下配置 base 选项后，所有 `to` 属性不用写基路径了

**to**

> 即路由链接，类型 string| Location

**replace**

> 用 router.replace代替 push， 类型 boolean

**append**

> 在当前 (相对) 路径前添加基路径。类型 boolean

**tag**

> 渲染标签类型，类型 boolean

**active-class**

> 激活 css 类名，默认'router-link-active'

**exact**

> "是否激活" 默认类名的依据是 **inclusive match**(全包含匹配),类型 boolean

**event**

**exact-active-class**



### router-view

**name**

> 对应路由配置下 components 里的组件



### Router

**mode**

> 路由模式： hash | history | abstract(支持所有 JavaScript 运行环境，如Node.js环境)

**base**

> 基路径。默认`'/'`

**linkActiveClass**

> 默认“激活 class 类名”，默认`router-link-active`

**linkExactActiveClass**

> 精确激活的class 类名，默认`router-link-exact-active`

**scrollBehavior**

> 滚动行为。类型 Function

**parseQuery / stringifyQuery**

> 自定义查询字符串的解析、反解析函数。

**fallback**

> 当浏览器不支持 `history.pushState` 控制路由是否应该回退到 `hash` 模式。默认值为 `true`



### Router实例

**属性**

router.app

router.mode

router.currentRoute



**方法**

router.beforeEach

router.beforeResolve

router.afterEach

router.push

router.replace

router.go

router.back

router.forward

router.getMatchedComponents 返回路由匹配的组件数组

router.resolve

router.addRoutes	动态添加路由规则配置

router.onReady(callback,[errorCallback])

router.onError(callback)



### 路由对象

> 一个**路由对象 (route object)** 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的**路由记录 (route records)**,是不可变的。

**属性**

**path**

**params**

**query**

**hash**

**fullPath**

**matched**

**name**

**redirectedFrom**



`this.$router`	router 实例

`this.$route`	当前激活的路由信息对象，只读