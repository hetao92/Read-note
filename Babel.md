[TOC]

#Babel

> babel 总共分为三个阶段： 解析，转换，生成

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

> 内部集成了 core-js 和 regenerator. 由于 babel 对部分新 API 无法转换，如 Generator、Set、Proxy、Promise 等全局对象及一些新增的方法如 includes、Array.form 等，所以需要它来为浏览器做这部分的兼容

它的缺点：

1. 使用后会导致打出来的包非常大，因为很多其实没有用到
2. 可能会污染全局变量，给很多类的原型链上都作了修改，存在不可控因此

因此可以在`babel-preset-env`中设置`"useBuiltIns": "usage"` 来按需加载