[TOC]

# React Router

`import { Router, Route, Link } from 'react-router'`

```react
import {IndexRoute} from 'react-router'
React.render((
  <Router>
  	<Router path="/" component={App}>
      <IndexRoute component={Dashboard} />
    	<Router path="about" component={About} />
      <Router path="inbox" component={Inbox}>
      	<Route path="message/:id" component={Message} />
      </Router>
    </Router>
  </Router>
), document.body)

//目前 Message 的路由是 '/inbox/messages/:id'
```

**默认路由IndexRoute与IndexLink**

上例当路由为'/'时，如果没有 indexRoute 则若组件 APP 内使用了 props.children，则是 undefined，现在将会是 Dashboard 元素。类似指定 index.html 的入口文件



同样若在 APP 内使用`<Link to="/">Home</Link>`，则会一直处于激活状态，因为所有 URL 开头都匹配`/`，因此可以使用`<IndexLink to="/">Home</IndexRoute>`



**绝对路径**

无法在动态路由中使用

```react
<Router path="inbox" component={Inbox}>
  <Route path="/message/:id" component={Message} />
</Router>
//这种情况下 /inbox 既可以从 /inbox/message/:id 中取出，又可以让 Message 组件嵌套在 Inbox 中渲染
```



**重定向**

```react
<Route path="inbox" component={Inbox}>
  <Route path="/messages/:id" component={Message} />
  {/* 跳转 /inbox/messages/:id 到 /messages/:id */}
  <Redirect from="messages/:id" to="/messages/:id" />
</Route>
```



**Hook**

`onEnter`从将要离开的路由中**逐级**触发，从最下层的子路由直到最外层父路由结束。

`onLeave`从最外层的父路由开始**逐级**直到最下层子路由结束



如上述路由下，从`/message/5`跳转到`about`

- `/messages/:id` 的 `onLeave`
- `/inbox` 的 `onLeave`
- `/about` 的 `onEnter`

```react
const requireAuth = (nextState, replace) => {
    if (!auth.isAdmin()) {
        // Redirect to Home page if not an Admin
        replace({ pathname: '/' })
    }
}
//重定向
export const AdminRoutes = () => {
  return (
     <Route path="/admin" component={Admin} onEnter={requireAuth} />
  )
}
```

**原生 route 数组对象写法**

```react
const routeConfig = [
  {
    path: '/',
    component: App,
    indexRoute: {component: Dashboard},
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
      	component: Inbox,
       	childRoutes: [
          { path: '/massages/:id', component: Message },
          { path: 'message/:id',
          	onEnter: function(nextState, replaceState) {
              replaceState(null, '/messages/' + nextState.params.id)
            }
          }
        ]
      }
    ]
  }
]
React.render(<Router routes={routeConfig} />, document.body)
```



## 路径语法

- `:paramName` – 匹配一段位于 `/`、`?` 或 `#` 之后的 URL。 命中的部分将被作为一个参数
- `()` – 在它内部的内容被认为是可选的
- `*` – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 `splat` 参数

使用相对路径，完整的路径将由它的所有祖先节点的路径和自身指定的相对路径拼接而成。使用绝对路径可以使路由匹配行为忽略嵌套关系。

```react
<Route path="/hello/:name">         // 匹配 /hello/michael 和 /hello/ryan
<Route path="/hello(/:name)">       // 匹配 /hello, /hello/michael 和 /hello/ryan
<Route path="/files/*.*">           // 匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
```



## Histories

三种形式

+ browserHistory
+ hashHistory
+ createMemoryHistory



```react
import { browserHistory } from 'react-router'
render(<Router history={browserHistory} routes={routes} />, document.body)
```



**browserHistory**

使用 History API 处理 URL，需要服务器做配置

**hashHistory**

使用hash`#`来创建路由

**createMemoryHistory**

Memory history 不会在地址栏被操作或读取，可以用于服务器渲染。

首先需要创建

`const history = createMemoryHistory(location)`



## 动态路由

对于 React Router 里的路径匹配以及组件加载都是异步完成的。因此不仅可以延迟加载组件，还可以延迟加载路由配置

逐渐匹配：通过定义`getChildRoutes`, `getIndexRoute`, `getComponents`这几个函数可以异步执行，只有在需要时才被调用。

```react
const CourseRoute = {
  path: 'course/:courseId',
  getChildRoutes(location, callback) {
    require.ensure([], function(require) {
      callback(null, [
        require('./routes/Announcements'),
        require('./routes/Assignments'),
        require('./routes/Grades'),
      ])
    })
  },
  getIndexRoute(location, callback) {
    require.ensure([], function(require) {
      callback(null, require('./components/Index'))
    })
  },
  getComponents(location, callback) {
    require.ensure([], function(require) {
      callback(null, require('./components/Course'))
    })
  }
}
```



**跳转前确认`routerWillLeave`**

