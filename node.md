[TOC]

# Node

https://nodejs.org/en/docs/
npm  node package manager 安装主流库
进入cmd 
-- cd到相应文件夹
-- 输入`node 文件名`即可运行



为什么 Node 约定，回调函数的第一个参数，必须是错误对象err（如果没有错误，该参数就是null）？

原因是（异步）执行分成两段，第一段执行完以后，任务所在的上下文环境就已经结束了。在这以后抛出的错误，原来的上下文环境已经无法捕捉，只能当作参数，传入第二段。



NodeJS是一个JS脚本解析器，任何操作系统下安装NodeJS本质上做的事情都是把NodeJS执行程序复制到一个目录，然后保证这个目录在系统PATH环境变量下，以便终端下可以使用`node`命令。

**异步IO**

**事件驱动**

**单线程**

**模块化**

js代码复杂度日益提高，维护成本高

依赖关系不明显，

容易造成全局污染等。模块化可以避免这种情况

**require**

> 在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。

解析路径规则：

内置模块

node_modules目录	

```
/home/user/node_modules/foo/bar
/home/node_modules/foo/bar
/node_modules/foo/bar
```

NODE_PATH环境变量，可以指定额外的模块搜索路径，在Linux下使用`:`分隔，在Windows下使用`;`分隔

将多个模块放在同一个目录里，可以当做**包(package)**

**当模块的文件名是`index.js`，加载模块时可以使用模块所在目录的路径代替模块文件路径**

也可以在包目录下包含package.json定义入口模块的路径，这样也可以用目录所在路径自动加载模块

**exports**

> 当前模块的导出对象，用于导出模块公有方法和属性

**module**

> 访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。



**NPM**

> 包管理工具

版本号

X.Y.Z（主版本号、次版本号和补丁版本号）

主版本号变动往往意味着大变动，向下不兼容。次版本号是新增功能，但向下兼容

- `~`表示：支持最新的修订版本号

  ~1.1.2，表示>=1.1.2 <1.2.0

  ~1.1，表示>=1.1.0 <1.2.0，可以是同上

  ~1，表示>=1.0.0 <2.0.0

- `^`表示：支持最新的次版本号

  ^1.1.2 ，表示>=1.1.2 <2.0.0

  ^0.2.3 ，表示>=0.2.3 <0.3.0

  ^0.0，表示 >=0.0.0 <0.1.0



**深度优先**，意味着到达一个节点后，首先接着遍历子节点而不是邻居节点。**先序遍历**，意味着首次到达了某节点就算遍历完成，而不是最后一次返回某节点才算数。



## 常用模块

### fs 文件系统

可以通过 `require('fs') `来获得文件系统模块
模块内所有方法均有同步、异步版本。由完成时的回调函数决定

+ 文件属性读写
+ 文件内容读写
+ 底层文件操作

```JavaScript
var fs = require('fs') 
//然后可以调用方法，如
fs.readdir()   
//读取目录

//异步读取文本文件
fs.readFile('sample.txt', 'utf-8', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
//当读取二进制文件时，不传入文件编码时，回调函数的data参数将返回一个Buffer对象。在Node.js中，Buffer对象就是一个包含零个或任意个字节的数组（和Array不同）。Buffer对象可以和String作转换，例如，把一个Buffer对象转换成String或者String转换成Buffer
var text = data.toString('utf-8');
var buf = new Buffer(text, 'utf-8');

//同步读取文件
var data = fs.readFileSync('sample.txt', 'utf-8');
//写文件
fs.writeFile(filename, data, function (err){}
fs.writeFileSync('output.txt', data)

//读取文件的详细信息
fs.stat(filename, function (err, stat) {}
```



## Buffer

Js本身只有字符串数据类型，没有二进制数据类型。Buffer可以对二进制数据进行操作

字符串与Buffer之间的转换

`str = buf.toString('utf-8')`	使用指定编码将二进制数据转化为字符串

`buf = new Buffer(str, 'utf-8')`	指定编码下的二进制数据

字符串是只读的，并且对字符串的任何修改得到的都是一个新字符串，原字符串保持不变。至于`Buffer`，更像是可以做指针操作的C语言数组。如用索引可以修改某位置的字节。slice也是返回某位置的指针



##Stream

当内存中无法一次装下需要处理的数据时，或者一边读取一边处理更加高效时，我们就需要用到数据流。



## Path



## HTTP

从规范上讲，HTTP请求头和响应头字段都应该是驼峰的。但不是每个HTTP服务端或客户端程序都严格遵循规范，所以NodeJS在处理收到的头字段时，都统一地转换为了小写字母格式，以便开发者能使用统一的方式来访问头字段，例如`headers['content-length']`。

##Express

body-parser 

> node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。

cookie-parser 

> 这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。

multer

> node.js 中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。



框架实例
```javascript
// express_demo.js 文件

// 引入 express 并且创建一个 express 实例赋值给 app
var express = require('express')
var app = express()

// 配置静态文件目录
app.use(express.static('static'))

// 用 get 定义一个给用户访问的网址
// request 是浏览器发送的请求
// response 是我们要发给浏览器的响应
app.get('/', function(request, response) {}

// listen 函数的第一个参数是我们要监听的端口
// 这个端口是要浏览器输入的
// 默认的端口是 80
// 所以如果你监听 80 端口的话，浏览器就不需要输入端口了
// 但是 1024 以下的端口是系统保留端口，需要管理员权限才能使用
var server = app.listen(8081, function () {
  var host = server.address().address
  var port = server.address().port

  console.log("应用实例，访问地址为 http://%s:%s", host, port)
})
//查询 ip
const dns = require('dns')
const host = 'zhihu.com'
dns.lookup(host,(error,address,family) => {
    console.log('address and family',address,family)
})
```



##摘要算法

    是一种能产生特殊输出格式的算法给定任意长度的数据生成定长的密文，摘要结果是不可逆的, 不能被还原为原数据，理论上无法通过反向运算取得原数据内容，通常只被用来做数据完整性验证
    常用的摘要算法主要有 md5 和 sha1md5 的输出结果为 32 字符
 sha1 的输出结果为 40 字符