[TOC]

# Vue CLI

- 通过 `@vue/cli` 搭建交互式的项目脚手架。

- 通过 `@vue/cli` + `@vue/cli-service-global` 快速开始零配置原型开发。

包含CLI / CLI 服务 / CLI 插件

**CLI**

CLI (`@vue/cli`),通过`vue create`快速创建新项目脚手架，通过 `vue serve` 构建新想法的原型， 通过 `vue ui` 通过一套图形化界面管理你的所有项目

**CLI服务**

CLI服务`@vue/cli-service`是开发环境依赖，是构建于 webpack 和 webpack-dev-server 之上的，包含

- 加载其它 CLI 插件的核心服务；
- 一个针对绝大部分应用优化过的内部的 webpack 配置；
- 项目内部的 `vue-cli-service` 命令，提供 `serve`、`build` 和 `inspect` 命令。

**CLI插件**

Vue CLI 插件的名字以 `@vue/cli-plugin-` (内建插件) 或 `vue-cli-plugin-` (社区插件) 开头

在项目内部运行 `vue-cli-service` 命令时，它会自动解析并加载 `package.json` 中列出的所有 CLI 插件。



**创建项目**

```bash
vue create <app-name>
配置 preset 并可保存为默认可复用的preset 到 .vuerc文件

vue ui
打开图形化界面来创建管理项目
```



**在现有项目安装插件**

```bash
vue add @vue/eslint
对于普通的 npm 包仍可以使用包管理器
vue-router 和 vuex 的情况比较特殊——它们并没有自己的插件，但是你仍然可以这样添加它们：
vue add router
vue add vuex
```



## CLI 服务

**使用命令**

```text
启动开发服务器
vue-cli-service serve [options] [entry]	
options：
	--open    在服务器启动时打开浏览器
  --copy    在服务器启动时将 URL 复制到剪切版
  --mode    指定环境模式 (默认值：development)
  --host    指定 host (默认值：0.0.0.0)
  --port    指定 port (默认值：8080)
  --https   使用 https (默认值：false)
  
命令行参数 [entry] 将被指定为唯一入口，而非额外的追加入口。尝试使用 [entry] 覆盖 config.pages 中的 entry 将可能引发错误。

打包
vue-cli-service build [options] [entry|pattern]
 	--mode        指定环境模式 (默认值：production)
  --dest        指定输出目录 (默认值：dist)
  --modern      面向现代浏览器带自动回退地构建应用
  --target      app | lib | wc | wc-async (默认值：app)
  --name        库或 Web Components 模式下的名字 (默认值：package.json 中的 "name" 字段或入口文件名)
  --no-clean    在构建项目之前不清除目标目录
  --report      生成 report.html 以帮助分析包内容
  --report-json 生成 report.json 以帮助分析包内容
  --watch       监听文件变化
  
审查项目里的webpack.config
vue-cli-service inspect [options] [...paths]
	--mode    指定环境模式 (默认值：development)
```



package.json里的`browserslist` 字段或者`.browserslistrc`文件保存了项目的目标浏览器的范范围，会被 @babel/preset-env和 Autoprefixer 用来确定需要转译的 JavaScript 特性和需要添加的 CSS 浏览器前缀。

Polyfill 用 JavaScript 实现浏览器不支持的 API



`public/index.html` 文件是一个会被 html-webpack-plugin 处理的模板。在构建时候注入资源链接

可以在其中插入`html-webpack-plugin`暴露的默认值或者客户端环境变量



**resource hint**是一系列相关标准，来告诉浏览器哪些源（origin）下的资源我们的Web App想要获取，哪些资源在之后的操作或浏览时需要被使用，以便让浏览器能够进行一些预先连接或预先加载等操作。

preload优先加载的资源

prefetch页面加载完成后利用空闲时间获取可能会用到的内容

### 静态资源

- 在 JavaScript 被导入或在 template/CSS 中通过相对路径被引用。这类引用会被 webpack 处理。
- **放置在 `public` 目录下**或通过**绝对路径被引用**。这类资源将会直接被拷贝，而不会经过 webpack 的处理。

当在 JavaScript、css、vue 文件使用相对路径引用静态资源时，资源会被包含进入 webpack 的依赖图中，他的 URL 会被`解析为一个模块依赖`。`url(./image.png)` 会被翻译为 `require('./image.png')`

### URL 转换规则

- 如果 URL 是一个绝对路径 (例如 `/images/foo.png`)，它将会被保留不变。

- 如果 URL 以 `.` 开头，它会作为一个相对模块请求被解释且基于你的文件系统中的目录结构进行解析。

- 如果 URL 以 `~` 开头，其后的任何内容都会作为一个模块请求被解析。这意味着你甚至可以引用 Node 模块中的资源：

  ```html
  <img src="~some-npm-package/foo.png">
  ```

- 如果 URL 以 `@` 开头，它也会作为一个模块请求被解析。它的用处在于 Vue CLI 默认会设置一个指向 `<projectRoot>/src` 的别名 `@`。**(仅作用于模版中)**

`public` 文件夹

任何放置在 `public` 文件夹的静态资源都会被简单的复制，而不经过 webpack。你需要通过绝对路径来引用它们。这只是个应急手段。将资源纳入模块依赖的好处有

- 脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。
- 文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。
- 最终生成的文件名包含了内容哈希，因此你不必担心浏览器会缓存它们的老版本。



## webpack

`vue.config.js` 中的 `configureWebpack` 选项提供一个对象，会被 webpack-merge 合并入最终的 webpack 配置。

有些 webpack 选项是基于 `vue.config.js` 中的值设置的，所以不能直接修改。例如你应该修改 `vue.config.js` 中的 `outputDir` 选项而不是修改 `output.path`；你应该修改 `vue.config.js` 中的 `publicPath` 选项而不是修改 `output.publicPath`。这样做是因为 `vue.config.js` 中的值会被用在配置里的多个地方，以确保所有的部分都能正常工作在一起。



## 环境变量和模式

默认情况下，一个 Vue CLI 项目有三个模式：

- `development` 模式用于 `vue-cli-service serve`

- `production` 模式用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`

- `test` 模式用于 `vue-cli-service test:unit`

  

模式不同于 `NODE_ENV`，一个模式可以包含多个环境变量。也就是说，每个模式都会将 `NODE_ENV` 的值设置为模式的名称——比如在 development 模式下 `NODE_ENV` 的值会被设置为 `"development"`。

```bash
.env                # 在所有的环境中被载入
.env.local          # 在所有的环境中被载入，但会被 git 忽略
.env.[mode]         # 只在指定的模式中被载入
.env.[mode].local   # 只在指定的模式中被载入，但会被 git 忽略

文件内包含环境变量的键值对
FOO=bar
VUE_APP_SECRET=secret

只有以 VUE_APP_ 开头的变量会被 webpack.DefinePlugin 静态嵌入到客户端侧的包中。你可以在应用的代码中这样访问它们：

console.log(process.env.VUE_APP_SECRET)
```



## vue.config.js

**配置属性**

**publicPath**

基本URL，默认`'/'`，用法和 webpack 本身的`output.publicPath`一致

但由于在其他地方可能也会用到这个值，因此不要修改 webpack 里的这个值

设置为空字符串`''`或相对路径`'./'`，所以资源会被链接到相对路径



**outputDir**

生成的生产环境构建文件的目录，默认'dist'



**assetsDir**

放置生成的静态资源(js/css/img/fonts)的目录



**indexPath**

指定生成的`index.html`的输出路径，相对于 outputDir，默认即 index.html



**filenameHashing**

文件名是否含 hash 以便更好的控制缓存，默认 true



**pages**

构建多页应用，每个“page”应该有一个对应的 JavaScript 入口文件。

```js
module.exports = {
  pages: {
    index: {
      // page 的入口
      entry: 'src/index/main.js',
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html',
      // 当使用 title 选项时，
      // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // 在这个页面中包含的块，默认情况下会包含
      // 提取出来的通用 chunk 和 vendor chunk。
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // 当使用只有入口的字符串格式时，
    // 模板会被推导为 `public/subpage.html`
    // 并且如果找不到的话，就回退到 `public/index.html`。
    // 输出文件名会被推导为 `subpage.html`。
    subpage: 'src/subpage/main.js'
  }
}
```



**lintOnSave**

在开发环境下通过`eslint-loader`在保存时 lint 代码。默认 true，可选 false、error(错误时直接编译失败)



**runtimeCompiler**

是否使用包含运行时编译器的 Vue 构建版本。默认 false



**configureWebpack**

值是对象时会通过 webpack-merge 合并到最终配置

```js
module.exports = {
  configureWebpack: {
    plugins: [
      new MyAwesomeWebpackPlugin()
    ]
  }
  
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
    } else {
      // 为开发环境修改配置...
    }
  }
}
```

**chainWebpack**

Function,接收基于 webpack-chain 的实例，允许对内部的 webpack 配置进行更细粒度的修改

```js
module.exports = {
  chainWebpack: config => {
    config.module
      .rule('vue')
      .use('vue-loader')
        .loader('vue-loader')
        .tap(options => {
          // 修改它的选项...
          return options
        })
    //添加新 loader
 		config.module
      .rule('graphql')
      .test(/\.graphql$/)
      .use('graphql-tag/loader')
        .loader('graphql-tag/loader')
        .end()
  
    //替换已有规则里的 loader
    const svgRule = config.module.rule('svg')

    // 清除已有的所有 loader。
    // 如果你不这样做，接下来的 loader 会附加在该规则现有的 loader 之后。
    svgRule.uses.clear()

    // 添加要替换的 loader
    svgRule
      .use('vue-svg-loader')
        .loader('vue-svg-loader')
    
    //修改插件选项
    config
      .plugin('html')
      .tap(args => {
        return [/* 传递给 html-webpack-plugin's 构造函数的新参数 */]
      })
  }
}
```



**css**

```js
css: {
  //loaderOptions 向 CSS 相关的 loader 传递选项
  loaderOptions: {
    css: {
      // 这里的选项会传递给 css-loader
    },
      postcss: {
        // 这里的选项会传递给 postcss-loader
      }
  },
    //为 css 开启 source map，为 true会影响性能
    sourceMap: false,
      
    //将组件中的 css 提取到独立的 css 文件
    extract: true(生产环境) false(开发环境)
}
```

