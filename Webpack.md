#Webpack

> 模块打包器。分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。

[参考](http://www.jianshu.com/p/42e11515c10f)



工作方式：

> 把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将递归地构建一个依赖关系图,从这个主文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。



**四个核心概念：**
入口(entry)：即从哪里开始逐步确定需要打包的内容
输出(output)：将所有资源归拢，在哪里如何打包程序
模块转换器(loader)：将文件转换为模块添加进依赖中，其中，`test`属性能识别出被对应 loader 转换的文件，`use`属性转换相应文件
插件(plugins)：在打包模块的“compilation”、“chunk”生命周期执行操作和自定义功能



**特点：**
**代码拆分**
webpack有两种组织模块依赖的方式，同步和异步，异步依赖作为分割点，形成一个新的块。在优化了依赖树后，每一个异步区块都作为一个文件被打包。
**Loader**
webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换为 JavaScript 模块，那么 webpack 就可以处理任何资源
**智能解析**
webpack 的智能解析器，几乎可以处理任何第三方库，无论形式是 CommonJS、AMD 还是普通 JS 文件
**插件系统**
**快速运行**
使用异步 I/O 和多级缓存运行效率。

使用方式：
1.在文件夹内创建`package.json`文件，或可在终端使用`npm init`自动创建。该文件是一个标准的 npm 说明文件，包含项目的依赖模块，自定义的脚本任务等

2.项目安装 webpack 作为依赖包
`npm install --save-dev webpack`

3.通常创建两个文件夹 比如app 文件夹和 public 文件夹，一个存放原始数据和 JavaScript 模块，另一个用于存放之后供浏览器读取的文件（包括使用 webpack 打包生成的 js 文件以及 html 文件）

4.1终端使用 webpack
`webpack {entry file} {destination for bundled file}`

entry file：入口（起始）文件路径
destination for bundled file：打包文件名及存放路径

4.2配置文件`webpack.config.js`使用webpack
命令行模式不太方便且易出错，可在根目录下建`webpack.config.js`文件，写入具体配置，主要涉及入口文件路径以及打包后文件的存放路径

```javascript
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
//“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
```

有了配置以后，打包文件只需在终端运行 webpack 命令即可，他会自动引用`webpack.config.js`的配置

`./node_modules/.bin/webpack --config webpack.config.js`

如果`webpack.config.js `存在，则 webpack 命令将默认选择使用它。我们在这里使用 `--config` 选项只是向你表明，可以传递任何名称的配置文件。这对于需要拆分成多个文件的复杂配置非常有用。

4.3 更快捷的执行打包`package.json`
可以对 npm 进行配置，再用简单的`npm start`来代替上述的命令，
在`package.json`中对`scripts`对象进行相关配置
```javascript
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    "start": "webpack" // 修改的是这里，JSON文件不支持注释，引用时请清除
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.12.9"
  }
}
```



package.json中的script可以通过模块名，来引用本地安装的 npm 包，而不是写出完整路径。它会按照一定顺序寻找命令对应位置，本地的node_modules/.bin路径就在这个寻找清单中，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。



##1.构建本地服务器
> 基于 `node.js`建立本地开发服务器，让你的浏览器监听你的代码的修改，并自动刷新显示修改后的结果

配置前需要单独安装作为项目依赖
`npm install --save-dev webpack-dev-server`
然后在 webpack 配置中可以对`devserver`对象进行配置
诸如

| devserver的配置选项 | 功能描述                                                     |
| :------------------ | :----------------------------------------------------------- |
| contentBase         | 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录） |
| port                | 设置默认监听端口，如果省略，默认为”8080“                     |
| inline              | 设置为true，当源文件改变时会自动刷新页面                     |
| historyApiFallback  | 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html |

在配置文件`config.js`内
```javascript
    devServer:{
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        historyApiFallback: true,//不跳转
        inline: true//实时刷新
    }
```

再在`package.json`的`scripts`添加如下命令即可开启本地服务器
```javascript
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open"
  },
  
//还有一种 “观察模式”
// "watch": "webpack --watch"
//在命令行中运行 npm run watch，就会看到 webpack 编译代码，然而却不会退出命令行。这是因为 script 脚本还在观察文件。
//修改并保存文件并检查终端窗口。应该可以看到 webpack 自动重新编译修改后的模块！但实际效果需要刷新浏览器。因此这种方式不如devserver
```



##2.Source Maps

webpack就可以在打包时为我们生成的source maps，这为我们提供了一种对应编译文件和源文件的方法，使得编译后的代码可读性更高，也更容易调试。

在webpack的配置文件中配置source maps，需要配置devtool，它有以下四种不同的配置选项，各具优缺点，描述如下：

| devtool选项                  | 配置结果                                                     |
| :--------------------------- | :----------------------------------------------------------- |
| source-map                   | 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度； |
| cheap-module-source-map      | 在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便； |
| eval-source-map              | 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项； |
| cheap-module-eval-source-map | 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点； |



##3.Loaders
Loader 可以理解为模块和资源的转换器，它本身是一个函数，接收源文件作为参数，返回转换的结果。
通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件

使用 loader 有三种方式

- 配置（推荐）：在 webpack.config.js 文件中指定 loader。
- 内联：在每个 import 语句中显式指定 loader。
- CLI（命令行接口）：在 shell 命令中指定它们。

**配置**
```
 module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
  //执行的顺序默认是从右到左执行的
```

**内联**
在`import`语句内前置指定 loader，用`!`将多个 loader 分开，后接上路径
`import Styles from 'style-loader!css-loader?modules!./styles.css';`



**特点：**

- Loader 可以通过管道方式链式调用，把资源转换为任意格式传递给下一个 loader，但最后一步必须返回 JavaScript。
- Loader 可以同步或异步执行
- Loader 运行在 node 环境
- Loader 可以接受查询参数，传递配置项给 loader
- Loader 可以通过文件扩展名（或正则）绑定给不同类型文件
- Loader 可以通过 npm 发布安装
- 可以安装插件让 loader 拥有更多特性
- 除了使用 package.json 常见的 main 属性，还可以将普通的 npm 模块导出为 loader，做法是在 package.json 里定义一个 loader 字段。


Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：

test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
loader：loader的名称（必须）
include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
query：为loaders提供额外的设置选项（可选）

按照惯例而言，一般 loader 以`xxx-loader`方式命名，xxx 即转换功能

常见 Loader：
style-loader css-loader 引入 css 文件
file-loader 引入图片等文件
##4.Babel

Babel 是一个编译JavaScript的平台，通过编译可以使用 ES6,ES7的 JavaScript 代码在未被完全支持的浏览器上使用，或使用基于 JavaScript 拓展的语言

Babel本身十几个模块化的包，核心功能位于`babel-core`的 npm 包中，对于每一个需要的功能或拓展，都需要安装相应的单独的包，比如解析 ES6使用`babel-preset-es2015`包，解析 JSX 使用`babel-preset-react`

安装：

```
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
```

在 config.js 中配置 babel
```
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader",
                    options: {
                        presets: [
                            "es2015", "react"
                        ]
                    }
                },
                exclude: /node_modules/
            }
        ]
    }
```

Babel 配置 可以在 config.js 中进行，也可以放在名为`.babelrc`中进行集中配置
```
{
    "presets": [
        "es2015",
    ],
    "plugins": [
        "transform-runtime"
    ],
    "comments": false
}
```



##编译 CSS
webpack 提供`css-loader`和`style-loader`来处理样式表，其中`css-loader`能使用`@import`和`url(...)`实现`require()`的功能，`style-loader`能将计算后的样式加入页面，结合两者即可把样式表嵌入打包后的 JS 文件中。

安装
`npm install --save-dev style-loader css-loader`

使用
在 webpack 中配置 module 中的 rule

```
module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader"
                },
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "style-loader"
                    }, {
                        loader: "css-loader"
                    }
                ]
            }
        ]
    }
    
//同一文件引入多个 loader 以数组形式
```



##插件（Plugins）
Loaders 是用于打包构建过程中用来处理源文件的（Scss,Less,JSX等），插件不直接操作单个文件，而是直接对整个构建过程起作用。

安装
使用
在 webpack 配置中的plugins 关键字添加插件的实例
```
    plugins:[
        new webpack.BannerPlugin('版权所有，翻版必究')
    ],
```

##属性
##entry

单入口语法`entry: string|Array<string>`
```
entry: './path/to/my/entry/file.js'
//对于单入口，可以写成
entry:{
    main:'path'
}
```
对象语法`entry:{[entryChunkName:string]:string|Array}`
```
entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
}
```
相对单入口语法，能分离应用程序入口以及第三方库入口，且更加可扩展，易与其他配置组合使用。可以配合多页面应用程序



##output

```
output:{
    filename:用于输出文件的文件名
    path:目标输出目录的绝对路径
}
```
若配置创建了多个入口，可以使用占位符`[name]`来确保每个文件具有唯一名称
```
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}

// 写入到硬盘：./dist/app.js, ./dist/search.js
```



##模块间的依赖关系

- ES2015 import 语句
- CommonJS require() 语句
- AMD define 和 require 语句
- css/sass/less 文件中的 @import 语句。
- 样式(url(...))或 HTML 文件(<img src=...>)中的图片链接(image url)



##模块解析
webpack 使用resolver 库来找到 bundle 中需要引入的模块代码的绝对路径。当打包模块时，使用`enhanced-resolve`来解析路径
绝对路径：无需解析
相对路径：使用 `import`或`require`的资源文件所在目录默认为上下文目录，相对路径会添加此上下文路径以产生绝对路径
模块路径：模块将在`resolve.modules`中指定的所有目录内搜索，其中初始模块路径可以被替换，通过使用`resolve.alias`配置来创建别名

解析 Loader 和文件解析规则类似

Runtime & Manifest
webpack 构建的项目中，包括的代码有：源码；依赖的第三方库；webpack管理模块交互的代码

runtime：模块交互时，连接模块所需的加载和解析逻辑，包括浏览器中已加载模块的连接以及懒加载模块的执行逻辑。

Manifest：项目启动加载到浏览器中时，均被打包成如`index.html`文件及一些 bundle 和资源，源码所在的目录结构已不复存在，那么 manifest 就会在编译器开始执行、解析和映射时保留所有模块的详细要点，当完成打包发送到浏览器时，可以通过 manifest 来解析和加载模块，`import/require`语句均会转换为`_webpack_require_`方法，指向模块标识符，再通过 manifest 中的数据，runtime 能够查询模块标识符，检索出相应的模块。

##模块热替换 Hot Module Replacement
应用程序运行过程中替换、添加或删除模块，从而无需重新加载整个页面。

实现方式：
保留在完全重新加载页面时丢失的应用程序状态
值更新变更内容，以节省开发时间
调整样式更加快速，几乎相当于在浏览器调试其中更改样式

原理：
程序要求 HMR runtime 检查更新
HMR runtime 异步下载更新，并通知程序
程序要求 HMR runtime 应用更新，HMR 应用更新



##生产环境构建
由于开发环境和生产环境构建目标差距较大，前者需要实时重新加载、热模块替换以及 localhost server 等功能，而生产环境只需要更小的 bundle、优化的资源等。而又为了遵循不重复原则，可以保留通用配置。
安装`webpack-merge`，即可在环境特定的配置引入共用配置 `merge()` 各个环境的特有配置

最后在 npm 配置`package.json`的 script 脚本指向具体文件`webpack --comfig [name.js]`



##环境变量
在运行 webpack 时设置环境变量，并且使用 Node.js 的 `process.env` 来引用变量。`NODE_ENV` 变量通常被视为事实标准
`NODE_ENV`是由`Node.js`暴露给执行脚本的系统环境变量，通常用于决定在开发环境与生产环境（dev-vs-prod）下，服务器工具、构建脚本和客户端 library 的行为。
？？？注意一点。在`webpack.config.js`中设置`process.env.NODE_ENV`设置为`production`将无法成功构建

```
module.exports = {
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compress: process.env.NODE_ENV === 'production'
    })
  ]
};
```

使用 `cross-env` 包来跨平台设置(cross-platform-set)环境变量：
```
{
  "scripts": {
    "build": "cross-env NODE_ENV=production PLATFORM=web webpack"
  }
}
```



##代码分离
代码分离可以用于获取更小的 bundle，以及控制资源加载优先级。
常用的代码分离方法：
入口起点`entry`配置手动分离
防止重复：使用`CommonsChunkPlugin`去重和分离 chunk
动态导入：通过模块的内联函数调用来分离

其中，使用入口配置手动分离有几点缺陷：
若入口文件之间包含重复模块，则在 bundle 也会重复。此外，这个方法不够灵活，不能将核心应用程序逻辑进行动态拆分代码。

`CommonsChunkPlugin`是 webpack 内置功能，它除了分离代码，还能降 webpack 的样板和 manifest 提取出来
```
new webpack.optimize.CommonsChunkPlugin({
    name:'common'   // 指定公共 bundle 的名称。
})
```
此外将第三方库例如`lodash`或`react`等可以提取到单独的`vendor`文件中，因为他们很少频繁修改
```
entry:{
    main:'./src/index.js',
    vendor:[
        'lodash'
    ]
},
plugins:[
    new webpack.optimize.CommonsChunkPlugin({
        name:'vendor'
    })
]
//vendor 实例需在其他实例如'runtime' 之前引入
```
##缓存
缓存的存在，当需要更新文件新代码时就会棘手
可以对`output.filename`进行文件名替换`[name].[hash].js`

对于`hash`，需要注意每个 `module.id` 会基于默认的解析顺序进行增量。所以新增内容时：
`main`bundle 会随着自身新增内容修改发生变化
`vendor`bundle 会随着自身`module.id`修改发生变化。
`runtime`会因当前包含一个新模块的引用而发生变化

外部依赖externals
对于项目的库而言，引用的外部库无需打包进 bundle，因此可以在`webpack.config.js`内配置`externals`
```
module.exports = {
    ...
    externals: {
        "lodash": {
            commonjs: "lodash",
            commonjs2: "lodash",
            amd: "lodash",
            root: "_"
        }
    }
    ...
};
```

完整配置模板
```javascript
const path = require('path');

module.exports = {
  entry: "./app/entry", // string | object | array
  // 这里应用程序开始执行
  // webpack 开始打包

  output: {
    // webpack 如何输出结果的相关选项

    path: path.resolve(__dirname, "dist"), // string
    // 所有输出文件的目标路径
    // 必须是绝对路径（使用 Node.js 的 path 模块）

    filename: "bundle.js", // string
    // 「入口分块(entry chunk)」的文件名模板（出口分块？）

    publicPath: "/assets/", // string
    // 输出解析文件的目录，url 相对于 HTML 页面

    library: "MyLibrary", // string,
    // 导出库(exported library)的名称

    libraryTarget: "umd", // 通用模块定义
    // 导出库(exported library)的类型

    /* 高级输出配置（点击显示） */
  },

  module: {
    // 关于模块配置

    rules: [
      // 模块规则（配置 loader、解析器等选项）

      {
        test: /\.jsx?$/,
        include: [
          path.resolve(__dirname, "app")
        ],
        exclude: [
          path.resolve(__dirname, "app/demo-files")
        ],
        // 这里是匹配条件，每个选项都接收一个正则表达式或字符串
        // test 和 include 具有相同的作用，都是必须匹配选项
        // exclude 是必不匹配选项（优先于 test 和 include）
        // 最佳实践：
        // - 只在 test 和 文件名匹配 中使用正则表达式
        // - 在 include 和 exclude 中使用绝对路径数组
        // - 尽量避免 exclude，更倾向于使用 include

        issuer: { test, include, exclude },
        // issuer 条件（导入源）

        enforce: "pre",
        enforce: "post",
        // 标识应用这些规则，即使规则覆盖（高级选项）

        loader: "babel-loader",
        // 应该应用的 loader，它相对上下文解析
        // 为了更清晰，`-loader` 后缀在 webpack 2 中不再是可选的
        // 查看 webpack 1 升级指南。

        options: {
          presets: ["es2015"]
        },
        // loader 的可选项
      },

      {
        test: /\.html$/,
        test: "\.html$"

        use: [
          // 应用多个 loader 和选项
          "htmllint-loader",
          {
            loader: "html-loader",
            options: {
              /* ... */
            }
          }
        ]
      },

      { oneOf: [ /* rules */ ] },
      // 只使用这些嵌套规则之一

      { rules: [ /* rules */ ] },
      // 使用所有这些嵌套规则（合并可用条件）

      { resource: { and: [ /* 条件 */ ] } },
      // 仅当所有条件都匹配时才匹配

      { resource: { or: [ /* 条件 */ ] } },
      { resource: [ /* 条件 */ ] },
      // 任意条件匹配时匹配（默认为数组）

      { resource: { not: /* 条件 */ } }
      // 条件不匹配时匹配
    ],

    /* 高级模块配置（点击展示） */
  },

  resolve: {
    // 解析模块请求的选项
    // （不适用于对 loader 解析）

    modules: [
      "node_modules",
      path.resolve(__dirname, "app")
    ],
    // 用于查找模块的目录

    extensions: [".js", ".json", ".jsx", ".css"],
    // 使用的扩展名

    alias: {
      // 模块别名列表

      "module": "new-module",
      // 起别名："module" -> "new-module" 和 "module/path/file" -> "new-module/path/file"

      "only-module$": "new-module",
      // 起别名 "only-module" -> "new-module"，但不匹配 "module/path/file" -> "new-module/path/file"

      "module": path.resolve(__dirname, "app/third/module.js"),
      // 起别名 "module" -> "./app/third/module.js" 和 "module/file" 会导致错误
      // 模块别名相对于当前上下文导入
    },
    /* 可供选择的别名语法（点击展示） */

    /* 高级解析选项（点击展示） */
  },

  performance: {
    hints: "warning", // 枚举
    maxAssetSize: 200000, // 整数类型（以字节为单位）
    maxEntrypointSize: 400000, // 整数类型（以字节为单位）
    assetFilter: function(assetFilename) {
      // 提供资源文件名的断言函数
      return assetFilename.endsWith('.css') || assetFilename.endsWith('.js');
    }
  },

  devtool: "source-map", // enum
  // 通过在浏览器调试工具(browser devtools)中添加元信息(meta info)增强调试
  // 牺牲了构建速度的 `source-map' 是最详细的。

  context: __dirname, // string（绝对路径！）
  // webpack 的主目录
  // entry 和 module.rules.loader 选项
  // 相对于此目录解析,默认使用当前目录

  target: "web", // 枚举
  // 包(bundle)应该运行的环境
  // 更改 块加载行为(chunk loading behavior) 和 可用模块(available module)

  externals: ["react", /^@angular\//],
  // 不要遵循/打包这些模块，而是在运行时从环境中请求他们

  stats: "errors-only",
  // 精确控制要显示的 bundle 信息

  devServer: {
    proxy: { // proxy URLs to backend development server
      '/api': 'http://localhost:3000'
    },
    contentBase: path.join(__dirname, 'public'), // boolean | string | array, static file location
    compress: true, // enable gzip compression
    historyApiFallback: true, // true for index.html upon 404, object for multiple paths
    hot: true, // hot module replacement. Depends on HotModuleReplacementPlugin
    https: false, // true for self-signed, object for cert authority
    noInfo: true, // only errors & warns on hot reload
    // ...
  },

  plugins: [
    // ...
  ],
}
```

`context`：`string`
基础目录，绝对路径，用于从配置中解析入口起点(entry point)和 loader,默认使用当前目录

`entry`:`string | [string] | object { <key>: string | [string] }`
如果传入一个字符串或字符串数组，chunk 会被命名为 main。如果传入一个对象，则每个键(key)会是 chunk 的名称，该值描述了 chunk 的入口起点。

`output.filename`
对于单个入口可以是静态名称，对于多个bundle，可以使用替换方式
使用入口名称 `[name].bundle.js`
使用内部 chunk id`[id].bundle.js`
使用每次构建生成的唯一hash`[name].[hash].bundle.js`
使用基于`[chunkhash].bundle.js`



### 常用插件

**DLLPlugin**

对于依赖的第三方库，与项目本身的文件代码区分开来，每次文件更改的时候他只会打包项目本身的代码，所以打包速度会更快

在 webpack.config.js外单独配置 webpack.dll.config.js，将所有第三方依赖打包到 bundle 的 dll 文件里，并生成 manifest.json文件，可以用于让DLLRdferencePlugin映射到相关依赖上

DLLReferencePlugin则是把刚刚在webpack.dll.config.js中打包生成的dll文件引用到需要的预编译的依赖上来。

97744ms —> 80919ms

**HappyPack**

在webpack构建过程中，我们需要使用Loader对js，css，图片，字体等文件做转换操作，并且转换的文件数据量也是非常大的，且这些转换操作不能并发处理文件，而是需要一个个文件进行处理，HappyPack的基本原理是将这部分任务分解到多个子进程中去并行处理，子进程处理完成后把结果发送到主进程中，从而减少总的构建时间。



**ParallelUglifyPlugin**

webpack默认使用UglifyJS压缩 js 代码，由于这是使用单线程代码，且压缩需要先把代码解析成 object 抽象表示的AST语法树，再去应用各种各种规则分析和处理AST，耗时非常大。

ParallelUglifyPlugin会开启多个子进程，分别去完成多个文件的压缩，但每个子进程都用 uglifyjs 来压缩，相当于并行处理压缩

100923ms —> 97744ms