[TOC]

#Nuxt

> 服务端渲染	路由功能	支持异步数据	静态文件服务
>
> 打包压缩JS/CSS	HTML头部标签管理	支持热加载	集成ESLint
>
> 代码分层	支持HTTP/2推送



request -> nuxtServerInit -> middleware -> validate() -> asyncData() / fetch() -> Render



npx（npx在NPM版本5.2.0默认安装了）

**创建项目**

```bash
npx create-nuxt-app <项目名>
```



## 目录结构

*代表不可更改

**资源目录 assets**

> 组织未编译的静态资源如 `LESS`、`SASS` 或 `JavaScript`。



**组件目录 components**

> 组件目录下组件不会像页面组件那样有 `asyncData` 方法的特性。



***布局目录 layouts**



**中间件目录**

> 中间件执行流程顺序：nuxt.config.js -> 匹配布局 -> 匹配页面

***页面目录 pages**

>组织路由及视图，读取目录下的 vue 文件并自动生成对应的路由配置



**插件目录 plugins**

> 组织在根 vue.js 应用实例化前需要运行的插件



***静态文件目录 static**

> 存放静态文件，不会被 webpack构建编译处理，文件会映射到根路径`/`下



***store 目录**

> 组织 Vuex 状态树



***nuxt.config.js**

> 个性化配置



***package.json**



| 别名         | 目录    |
| :----------- | :------ |
| `~` 或 `@`   | src目录 |
| `~~` 或 `@@` | 根目录  |



## nuxt.config.js配置

**build**

> 在自动生成的 `vendor.bundle.js` 文件中添加一些模块，以减少应用 bundle 的体积

```js
build: {
	//是否使用 webpack-bundle-analyzer 分析并可视化构建后的打包文件
  analyze: boolean/object，
  //为 js 和 vue 自定义 babel 配置
  babel: {
    babelrc: false,
    cacheDirectory: undefined,
    presets: ['@nuxt/babel-preset-app']
  }，
  //开启 css source map 支持
  cssSourceMap: true(开发模式) false(生产环境)，	//默认
  //是否允许 vue-devtools调试
  devtools: false,
  //为客户端和服务端的构建配置进行手工扩展处理
  //在服务端打包构建和客户端打包构建时候均会调用
  extend(config, ctx) {
		//config 为 webpack 配置对象
    //ctx 为构建环境对象，包括 isDev, isClient, isServer 这些布尔值
  },
  //自定义 webpack 加载器
  loaders: {},
  //配置 webpack 插件
  plugins: [],
  //将 .nuxt/dist/client 文件上传到 CDN 以获得渲染性能
  publicPath: 'https://cdn.nuxtjs.org',
  //监听文件更改
    watch: [
      '~/.nuxt/support.js'
    ]
}
```



**buildDir**

> 定义 .nuxt 默认目录

**css**

> 定义应用的全局样式文件、模块或第三方库

```js
css: [
  // 直接加载一个 Node.js 模块。（在这里它是一个 Sass 文件）
  'bulma',
  // 项目里要用的 CSS 文件
  '@/assets/css/main.css',
  // 项目里要使用的 SCSS 文件
  '@/assets/css/main.scss'
]
```



**dev**

> 配置项目是开发还是生产模式

默认为 true。但值会被 nuxt 命令覆盖：

+ 使用`nuxt`命令时，会被强制设为 true
+ 使用`nuxt build`, `nuxt start`, `nuxt generate`时会被设置为 false



**env**

> 定义应用客户端和服务端的环境变量

```js
env: {
  baseUrl: process.env.BASE_URL || 'http://localhost:3000'
}
//通过 process.env.baseUrl 读取
//通过 context.baseUrl 读取
```



Nuxt使用webpack的**definePlugin**来定义环境变量。这意味着Node.js中的`process`或`process.env`既不可用也不能定义。nuxt.config.js中定义的每个env属性都单独映射到`process.env.xxxx`并在编译期间进行转换编译。

也就是说，`console.log(process.env)`将输出`{}`，但`console.log(process.env.you_var)`仍将输出您的值。



**generate**

> 定义每个动态路由的参数，当调用 `nuxt.generate()` 会根据这个配置生成对应目录结构的静态文件

dir 生成的目录名称

devtools boolean 配置是否允许 vue-devtools 调试

fallback

interval 渲染周期间隔 number，避免使用来自Web应用程序的API调用互相干扰。

routes 由于在 generate命令下动态路由会被忽略，因此需要在这里指定动态值

```js
routes: [
  '/users/1',
  '/users/2',
  '/users/3'
]
//也可以是函数
routes: function () {
  return axios.get('https://my-api/users')
    .then((res) => {
    return res.data.map((user) => {
      return '/users/' + user.id
    })
  })
}
```



**head**

> 配置默认的 meta 标签

```js
head: {
    title: 'note',
    meta: [
        { charset: 'utf-8' },
        { name: 'viewport', content: 'width=device-width, initial-scale=1, user-scalable=0' },
        { hid: 'description', name: 'description', content: pkg.description }
    ],
    link: [
        { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
    ]
},
```



**loading**

>个性化定制加载组件



**modules**

> 将Nuxt 模块添加到项目中，如 vue-i18n



**plugins**

> 配置需要在根vue.js实例化之前运行的插件，数组元素为 string  或者 object(含 src 及 ssr)
>
> ssr为 false 则只会在客户端被打包时引入



**router**

> 可用于覆盖 Nuxt.js 默认的 `vue-router` 配置。

```js
router: {
  //根路径
  base: '/app/',
  //默认中间件
  middleware,
  //扩展路由
  extendRoutes (routes, resolve) {
    routes.push({
      name: 'custom',
      path: '*',
      component: resolve(__dirname, 'pages/404.vue')
    })
  },
  linkActiveClass,
  mode,
  scrollBehavior
}
```


## 路由

定义带参数的动态路由需要创建**以下划线**作为前缀的vue 文件， 如`_id.vue`，这样生成的路由`'/users/:id?'`会有可选的选项，需要在同级文件夹建 index.vue



嵌套路由：需要配置一个 vue 文件以及和它同名的目录文件夹来存放子视图

```
pages/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```



## 视图

###布局

在布局文件中添加 <nuxt/>来显示主体内容



### 页面

页面内的特殊配置项

| 属性名      | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| asyncData   | 最重要的一个键, 支持异步数据处理，另外该方法的第一个参数为当前页面组件的上下文对象。 |
| fetch       | 与 `asyncData` 方法类似，用于在渲染页面之前获取数据填充应用的状态树（store）。不同的是 `fetch` 方法不会设置组件的数据。 |
| head        | 配置当前页面的 Meta 标签                                     |
| layout      | 指定当前页面使用的布局（`layouts` 根目录下的布局文件)        |
| loading     | 如果设置为`false`，则阻止页面自动调用`this.$nuxt.$loading.finish()`和`this.$nuxt.$loading.start()` |
| transition  | 指定页面切换的过渡动效                                       |
| scrollToTop | 布尔值，默认: `false`。 用于判定渲染页面前是否需要将当前页面滚动至顶部。 |
| validate    | 校验方法用于校验动态路由的参数。                             |
| middleware  | 指定页面的中间件，中间件会在页面渲染之前被调用               |
| watchQuery  | 监听参数字符串的更改。 如果定义的字符串发生变化，将调用所有组件方法(asyncData, fetch, validate, layout, ...) |



###asyncData

在组件每次加载前被调用，可以在服务端或路由更新之前被调用，第一个参数为当前页上下文对象 context，nuxt 会将返回的数据融合组件的 data 一并返回

由于是初始化前调用。因此不能引用this

1. 返回 promise

   ```js
   asyncData ({ params }) {
     return axios.get(`https://my-api/posts/${params.id}`)
       .then((res) => {
       return { title: res.data.title }
     })
   }
   ```

2. 使用 async 或 await

   ```js
   async asyncData ({ params }) {
     let { data } = await axios.get(`https://my-api/posts/${params.id}`)
     return { title: data.title }
   }
   ```

3. 使用回调

   ```js
   asyncData ({ params }, callback) {
     axios.get(`https://my-api/posts/${params.id}`)
       .then((res) => {
       callback(null, { title: res.data.title })
     })
   }
   ```



由于默认情况下 query 的改变不会调用 asyncData，因此需要调用watchQuery来监听参数



从Nuxt 2.0开始，`~/alias`将无法在**CSS文件**中正确解析。你必须在url CSS引用中使用`~assets`（没有斜杠）或`@`别名，即`background:url("~assets/banner.svg")`



### fetch

用于在渲染页面前填充应用的状态树（store）数据，与 asyncData 类似，但不会设置组件的数据



### others

```js
head () {
  return {
    title: this.title,
    meta: [
      { hid: 'description', name: 'description', content: 'My custom description' }
    ]
    //hid 键为meta标签配一个唯一的标识编号。可以避免子组件的 meta 不能覆盖父组件的同一标签
  }
},
//校验方法用于校验动态路由参数的有效性。false则显示错误页面
validate({ params, query }) {
  return true // 如果参数有效
  return false // 参数无效，Nuxt.js 停止渲染当前页面并显示错误页面
}
layout: 'blog',
  // 或
layout (context) {
  return 'blog'
},

middleware: 'authenticated',
//scrollToTop 属性用于控制页面渲染前是否滚动至页面顶部。
scrollToTop: true,
//过渡特效
transition: '' | {} | (to, from) {}
//监听参数字符串更改并在更改时执行组件方法 (asyncData, fetch, validate, layout, ...)
watchQuery: ['page']
```



## Vuex状态树

Nuxt.js 支持两种使用 `store` 的方式，你可以择一使用：

- **普通方式：** `store/index.js` 返回一个 Vuex.Store 实例

  ```js
  export const state = () => ({
    counter: 0
  })
  
  export const mutations = {
    increment (state) {
      state.counter++
    }
  }
  ```

  

- **模块方式：** `store` 目录下的每个 `.js` 文件会被转换成为状态树指定命名的子模块



## nuxt 命令

| 命令          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| nuxt          | 启动一个热加载的Web服务器（开发模式）                        |
| nuxt build    | 利用webpack编译应用，压缩JS和CSS资源（发布用）。             |
| nuxt start    | 以生产模式启动一个Web服务器 (`nuxt build` 会先被执行)。      |
| nuxt generate | 编译应用，并依据路由配置生成对应的HTML文件 (用于静态站点的部署)。 |



##上下文Context

| 属性字段               | 类型            | 可用            | 描述                                                         |
| :--------------------- | :-------------- | :-------------- | :----------------------------------------------------------- |
| `app`                  | Vue 根实例      | 客户端 & 服务端 | 包含所有插件的Vue根实例。例如，在使用`axios`的时候, 你想获取 `$axios` 可以直接通过 `context.app.$axios` 来获取 |
| `isClient`             | `Boolean`       | 客户端 & 服务端 | 是否来自客户端渲染（废弃。请使用 process.client）            |
| `isServer`             | `Boolean`       | 客户端 & 服务端 | 是否来自服务端渲染（废弃。请使用 process.server）            |
| `isStatic`             | `Boolean`       | 客户端 & 服务端 | 是否是通过`nuxt generate` (不建议使用 `process.static`)      |
| `isDev`                | `Boolean`       | 客户端 & 服务端 | 是否是开发(dev) 模式，在生产环境的数据缓存中用到             |
| `isHMR`                | `Boolean`       | 客户端 & 服务端 | 是否是通过模块热替换`webpack hot module replacement` (*仅在客户端以dev模式*) |
| `route`                | vue-router 路由 | 客户端 & 服务端 | `vue-router` 路由实例                                        |
| `store`                | vuex 数据       | 客户端 & 服务端 | `Vuex.Store` 实例。                                          |
| `env`                  | `Object`        | 客户端 & 服务端 | `nuxt.config.js` 中配置的环境变量                            |
| `params`               | `Object`        | 客户端 & 服务端 | `route.params` 的别名                                        |
| `query`                | `Object`        | 客户端 & 服务端 | `route.query` 的别名                                         |
| `req`                  | `http.Request`  | 服务端          | Node.js API 的 Request 对象。如果 nuxt 以中间件形式使用的话，这个对象就根据你所使用的框架而定。*nuxt generate 不可用* |
| `res`                  | `http.Response` | 服务端          | Node.js API 的 Response 对象。如果 nuxt 以中间件形式使用的话，这个对象就根据你所使用的框架而定。*nuxt generate 不可用* |
| `redirect`             | `Function`      | 客户端 & 服务端 | 用这个方法重定向用户请求到另一个路由。状态码在服务端被使用，默认 302 `redirect([status,] path [, query])` |
| `error`                | `Function`      | 客户端 & 服务端 | 用这个方法展示错误页：`error(params)`。`params` 参数应该包含 `statusCode` 和 `message` 字段 |
| `nuxtState`            | `Object`        | 客户端          | Nuxt 状态, 在使用 `beforeNuxtRender`之前，用于客户端获取nuxt状态，仅在`universal`模式下可用 |
| `beforeNuxtRender(fn)` | `Function`      | 服务端          | 使用此方法更新 `__NUXT__` 在客户端呈现的变量, `fn` 调用 (可以是异步) `{ Components, nuxtState }` |

## 组件

<nuxt/>

在布局中显示页面组件



<nuxt-child />

在嵌套路由场景下显示子路由页面内容



<nuxt-link>

和`router-link`一致



<no-ssr></no-ssr>

设置组件不在服务器渲染中呈现。