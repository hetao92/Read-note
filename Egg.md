[TOC]

# Egg.js

## 目录结构

app/router.js` 配置路由规则

`app/controller` 解析用户输入，处理后返回相应结果

`app/service`	编写业务逻辑层

`app/middleware`	中间件

`app/public`	静态资源放置

`app/extend` 框架扩展

`app/schedule/**`	用于定时任务

`config/config.{env}.js`  不同环境配置文件

`config/plugin.js`	配置需要加载的插件

`test/**`	单元测试

`app.js 和 agent.js` 用于自定义启动时的初始化工作



## 内置对象

1. Application

   所有被框架 Loader加载的文件（Controller，Service，Schedule 等）中的函数都可以使用 app 作为参数

   `Application` 是全局应用对象，在框架运行时会在其实例上触发一些事件

   + server 

     一个 worker 进程只会触发一次，在 HTTP 服务完成启动后，会将 HTTP server 通过此事件暴露出来

     `app.on('server', server => {})`

   + error

     运行时右任何异常被 `onerror` 捕获后都会触发

     `app.on('error', (err, ctx) => {})`

   + request 和 response

     应用收到请求和响应请求时，会触发，将当前请求上下文暴露出来

     `app.on('request', (ctx) => {})`

     `app.on('response', (ctx) => {})`

2. Context

   `Context` 是一个请求级别的对象，在每次收到用户请求时框架会实例化一个`Context`对象，该对象封装了用户请求的信息。

   Middleware、Controller、Service 等中都可以获取到 Context 实例

3. Request & Response

   可以在 `Context` 实例上获取到当前请求的相应实例`ctx.request/ctx.response`

   - Koa 会在 Context 上代理一部分 Request 和 Response 上的方法和属性
   - 如`ctx.request.query.id` 和 `ctx.query.id` 是等价的，`ctx.response.body=` 和 `ctx.body=` 是等价的。
   - 需要注意的是，获取 POST 的 body 应该使用 `ctx.request.body`，而不是 `ctx.body`。

4. Controller

   框架提供了 Controller 基类，所有的 Controller 都继承于该基类实现，该基类包含了属性 `ctx`、`app`、`config`、`service`、`logger`

   ```js
   // 从 egg 上获取（推荐）
   const Controller = require('egg').Controller;
   class UserController extends Controller {
     // implement
   }
   module.exports = UserController;
   
   // 从 app 实例上获取
   module.exports = app => {
     return class UserController extends app.Controller {
       // implement
     };
   };
   ```

5. Service

   Service 基类包含属性与 Controller 一致，访问方式也相同

6. Helper

   提供一些实用的 utility 函数，将常用的动作抽离到 helper.js 里成为独立函数，Helper 本身是一个类，有和 Controller 基类一样的属性，会在每次请求时进行实例化，因此内部的所有函数都可以获取到当前请求相关的上下文信息。可以在 Context 实例上获取到 `ctx.helper`

   可以通过框架扩展的形式自定义 helper 方法

7. Config

   可以通过`app.config`从 Application 实例上获取到 config 对象，也可以在 Controller, Service, Helper实例上通过`this.config`获取到

8. Logger

   + app.logger 

     常用于做应用级别的日志记录，记录一些业务上与请求无关的信息

   + app.coreLogger

     通常给框架和插件来记录日志，而不是给应用使用

   + ctx.logger

     与请求相关，打印的日志往往带有请求相关的信息

   + ctx.coreLogger

   + Controller Logger & Service Logger

     可以在 Controller 和 Service 实例上通过`this.logger`获取到他们，本质上就是 Context Logger，不过还会额外加上文件路径

9. Subscription

   订阅模型

   ```js
   const Subscription = require('egg').Subscription;
   
   class Schedule extends Subscription {
     // 需要实现此方法
     // subscribe 可以为 async function 或 generator function
     async subscribe() {}
   }
   ```



## 配置运行环境

`app.config.env` 表示当前运行环境

可以通过 `EGG_SERVER_ENV`  环境变量来指定，如果未指定 `EGG_SERVER_ENV` 会根据 `NODE_ENV` 来映射匹配

| NODE_ENV   | EGG_SERVER_ENV | 说明         |
| ---------- | -------------- | ------------ |
|            | local          | 本地开发环境 |
| test       | unittest       | 单元测试     |
| production | prod           | 生产环境     |



## 配置 Config

指定 env 的配置文件会覆盖 `config.default.js` 默认配置文件的同名配置

配置的优先级 应用 > 框架 > 插件

写法

```js
// 返回 object 对象
module.exports = {
  logger: {
    dir: '/home/admin/logs/demoapp',
  },
};

// 返回 key value 形式
exports.keys = 'my-cookie-secret-key';

// 返回 function(appInfo)
const path = require('path');
module.exports = appInfo => {
  return {
    logger: {
      dir: path.join(appInfo.baseDir, 'logs'),
    },
  };
};

// appinfo 包含属性
pkg	package.json
name	应用名，同 pkg.name
baseDir	应用代码的目录
HOME	用户目录，如 admin 账户为 /home/admin
root	应用根目录，只有在 local 和 unittest 环境下为 baseDir，其他都为 HOME。
```



## Middleware

洋葱圈模型，便于后置事件处理

中间件的加载是有先后顺序的，但这由使用者决定，中间件本身无法管理顺序

中间件的定位是拦截用户请求，并在前后做一些事情，如鉴权、安全检查、访问日志等。

所有的中间件都支持通用的配置项

+ enable 控制中间件是否开启

+ match 设置只有复合某些规则的请求才会经过中间件

+ ignore 设置符合某些规则的请求不经过这个中间件

  match 和 ignore 支持多种类型的配置方式

  1. 字符串：当参数为字符串类型时，配置的是一个 url 的路径前缀，所有以配置的字符串作为前缀的 url 都会匹配上。 当然，你也可以直接使用字符串数组。

  2. 正则：当参数为正则时，直接匹配满足正则验证的 url 的路径。

  3. 函数：当参数为一个函数时，会将请求上下文`ctx`传递给这个函数，最终取函数返回的结果（true/false）来判断是否匹配。

     

### 应用层中间件

一个完整的中间件 exports 一个 function，接受两个参数

+ options	中间件的配置项，框架会将 `app.config[${middlewareName}]` 传递进来。
+ app          Application 实例

```js
module.exports = (options, app) => {
	return async function middleware_name(ctx, next) {
    ...
    await next();
    ...
  }
}
  
// 在 config.default.js 中加入配置
module.exports = {
  // 数组顺序即中间件加载顺序
  middleware: [ 'middleware_name' ],
  // 配置 option, 以中间件名为 key, object 内属性即可在中间件中以 options[key] 获取到
  'middleware_name': {
    ...
  }
}
```

这些自定义中间件最终会在启动时合并到`app.config.appMiddleware`



### 在框架和插件中使用中间件

框架和插件不支持在 `config.default.js` 中匹配 middleware

```js
// app.js
module.exports = app => {
  // 在中间件最前面统计请求时间
  app.config.coreMiddleware.unshift('report');
};

// app/middleware/report.js
module.exports = () => {
  return async function (ctx, next) {
    const startTime = Date.now();
    await next();
    // 上报请求时间
    reportTime(Date.now() - startTime);
  }
};
//应用层定义的中间件（app.config.appMiddleware）和框架默认中间件（app.config.coreMiddleware）都会被加载器加载，并挂载到 app.middleware 上。
```



### router 中使用中间件

以上两种方式配置的中间件是全局的，会处理每一次请求。 如果你只想针对单个路由生效，可以直接在 `app/router.js` 中实例化和挂载，如下：

```js
module.exports = app => {
  const gzip = app.middleware.gzip({ threshold: 1024 });
  app.router.get('/needgzip', gzip, app.controller.handler);
};
```



## Router

```js
// app/router.js
module.exports = app => {
  const { router, controller } = app;
  router.verb('path-match', app.controller.action);
  router.verb('router-name', 'path-match', app.controller.action);
  router.verb('path-match', middleware1, ..., middlewareN, app.controller.action);
  router.verb('router-name', 'path-match', middleware1, ..., middlewareN, app.controller.action);
};

// verb 即请求方式，支持所有 HTTP 方法，如 
// router.head(HEAD) 
// router.options(OPTIONS) 
// router.get(GET) 
// router.put(PUT) 
// router.post(POST) 
// router.patch(PATCH) 
// router.delete(DELETE)	router.del 是一个别名，由于 delete 是一个保留字
// router.redirect 重定向

// router-name 给路由设定别名，可以通过 Helper 提供的辅助函数 pathFor 和 urlFor来生成 URL
// path-match 路由 url 路径
// middleware 可配置多个 middleware
// controller
```



若是生成一套 CRUD (create/read/update/delete)的路由结构，可以使用 `resources` 快速生成

`router.resources('routerName', 'pathMatch', controller)`

如  `router.resources('posts', '/api/posts', controller.posts);`

| Method | Path            | Route Name | Controller.Action             |
| ------ | --------------- | ---------- | ----------------------------- |
| GET    | /posts          | posts      | app.controllers.posts.index   |
| GET    | /posts/new      | new_post   | app.controllers.posts.new     |
| GET    | /posts/:id      | post       | app.controllers.posts.show    |
| GET    | /posts/:id/edit | edit_post  | app.controllers.posts.edit    |
| POST   | /posts          | posts      | app.controllers.posts.create  |
| PUT    | /posts/:id      | post       | app.controllers.posts.update  |
| DELETE | /posts/:id      | post       | app.controllers.posts.destroy |

### 重定向

```js
// router.js 内
app.router.redirect('/', '/home/index', 302);

// 其他 controller
ctx.redirect(`http://cn.bing.com/search?q=${q}`);
```



## Controller

`Controller` 负责解析用户的输入，处理后返回相应的结果

1. 获取用户通过 HTTP 传递过来的请求参数
2. 校验、组装参数
3. 调用 Service 进行业务处理，必要时处理转换 Service 的返回结果使适应用户需求
4. 通过 HTTP 将结果响应给用户



Controller 支持自定义 Controller 并继承

```js
// app/core/base_controller.js
const { Controller } = require('egg');
class BaseController extends Controller {
  get user() {
    return this.ctx.session.user;
  }

  success(data) {
    this.ctx.body = {
      success: true,
      data,
    };
  }
}

//app/controller/post.js
const Controller = require('../core/base_controller');
class PostController extends Controller {
  async list() {
    const posts = await this.service.listByUser(this.user);
    this.success(posts);
  }
}

```



支持多级目录，在 router 中可以根据文件名和方法名定位

```js
// app/controller/sub/post/js
// app/router.js
module.exports = app => {
  app.router.post('createPost', '/api/posts', 	app.controller.sub.post.create);
}
```



Controller 类下的属性

+ `this.ctx`
+ `this.app`
+ `this.service` 等价于 `this.ctx.service` 。
+ `this.config`
+ `this.logger` 对象上有`debug`、`info`、`warn`、`error`方法



### 获取 HTTP 请求参数

`this.ctx.query` 获取 query，当 query 内有 key 重复时，也只会取第一次出现时的值，后面会被忽略

`this.ctx.queries` 如`GET /posts?category=egg&id=1&id=2&id=3` 这种可能就是需要相同的 key，因此`queries`就会解析成一个**数组** `{ category: ['egg'], id: ['1', '2', '3']}`

`this.ctx.params` 获取 router 上参数

`this.ctx.request.body`  

​		`this.ctx.body`是`this.ctx.response.body`的简写

`ctx.headers` `ctx.header` `ctx.request.headers` `ctx.request.header` 这几个都是等价的

`ctx.get(name)` `ctx.request.get(name)`等价，不过前者会自动处理大小写

`ctx.host` ，优先读 `config.hostHeaders`中配置的值

`ctx.protocol` 判断是否为加密连接，是则返回 https

`ctx.cookies` 可以通过 get/set 方法来设置读取 cookie

`ctx.session` 直接修改，无需 get/set，赋值为 null 即为删除



### 参数校验

通过 validate 插件可以对参数进行校验

```js
// config/plugin.js
exports.validate = {
  enable: true,
  package: 'egg-validate',
};

class PostController extends Controller {
  async create() {
    // 校验参数 ctx.validate(rule, [body])
    // 如果不传第二个参数会自动校验 `ctx.request.body`
    this.ctx.validate({
      title: { type: 'string' },
      content: { type: 'string' },
    });
  }
}

//当校验异常时，会直接抛出一个异常，异常的状态码为 422，errors 字段包含了详细的验证不通过信息。如果想要自己处理检查的异常，可以通过 try catch 来自行捕获。
try {
  ctx.validate(createRule);
} catch (err) {
  ctx.logger.warn(err.errors);
  ctx.body = { success: false };
  return;
}

// app.js 可以新增自定义规则
app.validator.addRule('json', (rule, value) => {
  try {
    JSON.parse(value);
  } catch (err) {
    return 'must be json string';
  }
});
```



## Service

Service 属性同 controller

支持多级目录，在 router 中可以根据文件名和方法名定位

框架在每次请求中首次访问 `ctx.service.xx`时延迟实例化，所以 service 可以通过 `this.ctx`获取请求的上下文。



## 插件

插件和应用 app 有点类似，他包含了 Service、中间件、配置和框架扩展等。

但没有独立的 Router 和 Controller

没有`plugin.js`，只能声明跟其他插件的依赖，不能决定其他插件开启与否

`plugin.js` 中的每个配置项支持：

- `{Boolean} enable` - 是否开启此插件，默认为 true
- `{String} package` - `npm` 模块名称，通过 `npm` 模块形式引入插件
- `{String} path` - 插件绝对路径，跟 package 配置互斥
- `{Array} env` - 只有在指定运行环境才能开启，会覆盖插件自身 `package.json` 中的配置

```js
$ npm i egg-mysql --save
// config/plugin.js
// 使用 mysql 插件
exports.mysql = {
  enable: true,
  package: 'egg-mysql',
};

//使用
app.mysql.query(sql, values);
// 内置插件在使用时就不用配置 package 或者 path，直接配置即可
exports.onerror = false;

```

插件支持文件命名`plugin.{env}.js`会根据运行环境加载插件，不存在 `plugin.default.js`



## 定时任务

`app/schedule`目录下

```js
import { Subscription } from 'egg';
class UpdateCache extends Subscription {
// 通过 schedule 属性来设置定时任务的执行间隔等配置
  static get schedule() {
    return {
      interval: '1m', // 1 分钟间隔
      type: 'all', // 指定所有的 worker 都需要执行
    };
  }

  // subscribe 是真正定时任务执行时被运行的函数
  async subscribe() {
    const res = await this.ctx.curl('http://www.api.com/cache', {
      dataType: 'json',
    });
    this.ctx.app.cache = res.data;
  }
}

module.exports = UpdateCache;

//简写
module.exports = {
  schedule: {
    interval: '1m', // 1 分钟间隔
    type: 'all', // 指定所有的 worker 都需要执行
  },
  async task(ctx) {
    const res = await ctx.curl('http://www.api.com/cache', {
      dataType: 'json',
    });
    ctx.app.cache = res.data;
  },
};
```

- `task` 或 `subscribe` 同时支持 `generator function` 和 `async function`。

- `task` 的入参为 `ctx`，匿名的 Context 实例，可以通过它调用 `service` 等。

- 支持`interval`或`cron`两种定时方式

  `schedule.interval` 支持

  + 数字类型 单位为毫秒数，如 5000
  + 字符类型，会转化为毫秒数，如 `'5s'` ，规则参照 [ms](https://github.com/vercel/ms)

  `schedule.cron` 支持依据表达式在特定时间点执行

- `type` 支持 `worker/all`

  + `worker`类型：每台机器上只有一个 worker 会执行任务，每次执行的 worker 随机选择

  + `all`：每台机器上的每个 worker 都会执行任务

    

定时任务还支持这些参数：

- `cronOptions`: 配置 cron 的时区等，参见 [cron-parser](https://github.com/harrisiirak/cron-parser#options) 文档
- `immediate`：配置了该参数为 true 时，这个定时任务会在应用启动并 ready 后立刻执行一次这个定时任务。
- `disable`：配置该参数为 true 时，这个定时任务不会被启动。
- `env`：数组，仅在指定的环境下才启动该定时任务。



**手动执行定时任务**

通过 `app.runSchedule(schedulePath)` 来运行一个定时任务。`app.runSchedule` 接受一个定时任务文件路径（`app/schedule` 目录下的相对路径或者完整的绝对路径），执行对应的定时任务，返回一个 Promise。



## 框架扩展

`app/extend`文件夹下

如 `application.js` 定义的对象，即会与 Koa Application 的 prototype 对象合并，在应用启动时会基于扩展后的 prototype 生成 `app` 对象

`app/extend/context.js` 中定义的对象与 Koa Context 的 prototype 对象进行合并，在处理请求时会基于扩展后的 prototype 生成 ctx 对象。

同样支持根据环境对文件命名`[name].{env}.js`进行扩展



## 启动自定义

`app.js` 中可返回一个类，并在一下生命周期中执行一些初始化工作

- 配置文件即将加载，这是最后动态修改配置的时机（`configWillLoad`）
- 配置文件加载完成（`configDidLoad`）
- 文件加载完成（`didLoad`）
- 插件启动完毕（`willReady`）
- worker 准备就绪（`didReady`）
- 应用启动完成（`serverDidReady`）
- 应用即将关闭（`beforeClose`）





## 单元测试

测试用例执行时，应用是以 `env: unittest` 启动的，读取的配置也是 `config.default.js` 和 `config.unittest.js` 合并的结果。

```js
const { app, mock, assert } = require('egg-mock/bootstrap');
```

`egg-mock` 提供 bootstrap 快捷创建了 app 实例，便于测试用例引用

使用 before/after/beforeEach/afterEach 来处理前置后置任务，基本能处理所有问题。 每个用例会按 before -> beforeEach -> it -> afterEach -> after 的顺序执行，而且可以定义多个。

```js
describe('egg test', () => {
  before(() => console.log('order 1'));
  before(() => console.log('order 2'));
  after(() => console.log('order 6'));
  beforeEach(() => console.log('order 3'));
  afterEach(() => console.log('order 5'));
  it('should worker', () => console.log('order 4'));
});
```

测试支持使用 返回 Promise、 callback、 async 方式来写测试



### Crontoller 测试

由于 Controller 和 router 相关，需要利用 `app.httpRequest()` 发起一个请求，将两者连接起来来测试 Controller 的参数校验完整性。



## HttpClient

`app.httpclient`

`app.curl(url, options)` 等价于 `app.httpclient.request(url, options)`

`ctx` 下也可以直接访问到上述两个