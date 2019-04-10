[TOC]

# node

https://nodejs.org/en/docs/
npm  node package manager 安装主流库
进入cmd 
-- cd到相应文件夹
-- 输入`node 文件名`即可运行



为什么 Node 约定，回调函数的第一个参数，必须是错误对象err（如果没有错误，该参数就是null）？

原因是（异步）执行分成两段，第一段执行完以后，任务所在的上下文环境就已经结束了。在这以后抛出的错误，原来的上下文环境已经无法捕捉，只能当作参数，传入第二段。



## 常用模块

### fs 文件系统

可以通过 `require('fs') 来获得文件系统模块
模块内所有方法均有同步、异步版本。由完成时的回调函数决定



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