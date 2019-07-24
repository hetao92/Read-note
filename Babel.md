[TOC]

#Babel

> babel 是一个 JavaScript 编译器。 总共分为三个阶段： 解析，转换，生成

不过 babel 本身是不具备转化功能的，提供这些功能的是一个个 plugin。

babel 自 6.0 起只负责 parse 和 generate，transform 由插件去做

![img](/Users/hetaohua/Documents/Read-note/img/babel.png)



```js
// .babelrc 文件
{
 "plugins": [
    "transform-es2015-template-literals" // 转译模版字符串的 plugins
	],
  "presets": ["env", "stage-2"]
}
```



Babel 将 JavaScript 语法分为 `syntax` 和 `api`

**api**是指可以通过函数重新覆盖的语法，类似`includes/map/includes/Promise`等浏览器没有的方法

**syntax**，像箭头函数，`let/const/class/依赖注入 Decorators`等，在 JavaScript 在运行时是无法重写的

babel 只负责转换 `syntax`，对于 `api` 层面则交给 `polyfill`。

又因为 polyfill 体积过大，需要使用 `useBuiltIns`来配合实现按需加载，而 `runtime`可以用来做隔离



**preset**

> preset 是一些 babel 插件集合的**预设**，包含了某些插件 plugin

plugins 和 presets 的执行顺序

1. 先执行 plugins 的配置项，在执行 preset 的配置项
2. plugins 配置项，按照声明**顺序**执行
3. preset 配置项，按照声明**逆序**执行



**babel 相关模块**

**babel-core**

> 核心模块，babel 的核心 api 都在这里，会把项目的 js 代码抽象成 AST 树，再将 plugins 转义号的内容解析成 js 代码



**babel-cli**

> 通过命令行对 js 文件进行转换的工具。这个工作一般都被集成到如 webpack、Rollup 等模块化管理工具中



**babel-polyfill**

> 内部集成了 core-js 和 regenerator. 由于 babel 对部分新 API 无法转换，如 Generator、Set、Proxy、Promise 等全局对象及一些新增的方法如 includes、Array.form、Object.assign 等，所以需要它来为浏览器做这部分的兼容

它的缺点：

1. 使用后会导致打出来的包非常大，因为很多其实没有用到
2. 可能会污染全局变量，给很多类的原型链上都作了修改，存在不可控因此

因此可以在`babel-preset-env`中设置`"useBuiltIns": "usage"` 来按需加载



**@babel/plugin-transform-runtime**

> 相比 polyfill，它的内部也集成了 core-js、regenerator、helpers 等，Babel 转译后的代码要实现源代码同样的功能需要借助一些帮助函数，这一类的工具函数若是重复出现在模块里会使编译后体积变大，因此会使用 babel-runtime 提供的工具函数在项目中使用，这样可以**减小库和工具包的体积**。
>
> 同时 runtime 会为源代码的非实例方法和 runtime/helps 下的工具函数自动引用 polyfill，可以避免污染



**实践中用到的插件**

```js
["@babel/plugin-proposal-decorators",{"legacy": true}],
["@babel/plugin-proposal-class-properties",{"loose": true}],
//支持装饰器
```

`@babel/plugin-proposal-object-rest-spread`	支持对象内使用扩展运算符

`@babel/plugin-syntax-dynamic-import`	支持webpack的import()插件。`import()`调用会在内部用到 promise，这在部分浏览器中不支持，需要使用插件