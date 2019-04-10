scheme://host.domain:port/path/filename
scheme - 定义因特网服务的类型。最常见的类型是 http；https；ftp；file
host - 定义域主机（http 的默认主机是 www）
domain - 定义因特网域名，比如 w3school.com.cn
:port - 定义主机上的端口号（http 的默认端口号是 80）
path - 定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）。
filename - 定义文档/资源的名称

网址组成（四部分）
    协议      http, https（https 是加密的 http）
    主机      g.cn  zhihu.com之类的网址
    端口      HTTP 协议默认是 80，因此一般不用填写
    路径      下面的「/」和「/question/31838184」都是路径
http://www.zhihu.com/
http://www.zhihu.com/question/31838184

电脑通信靠IP地址，IP地址记不住就发明了域名（domain name），然后电脑
自动向DNS服务器（domain name server）查询域名对应的IP地址

比如g.cn这样的网址，可以通过电脑的ping程序查出对应 IP 地址
➜    ping g.cn
PING g.cn (74.125.69.160): 56 data bytes



## HTTP协议

超文本传输协议

1. 浏览器（客户端）按照规定的格式发送文本数据（请求）到服务器
2. 服务器解析请求，按照规定的格式返回文本数据到浏览器
3. 浏览器解析得到的数据，并做相应处理

请求和返回是一样的数据格式，分为4部分：

1. 请求行或者响应行
2. Header（请求的 Header 中 Host 字段是必须的，其他都是可选）
3. \r\n\r\n（连续两个换行回车符，用来分隔Header和Body）
4. Body（可选，浏览器发的数据）



请求的格式，注意大小写（这是一个不包含Body的请求）：
原始数据如下
'GET / HTTP/1.1\r\nhost:g.cn\r\n\r\n'
打印出来如下
GET / HTTP/1.1
Host: g.cn

其中
1. GET 是请求方法（还有POST等，这就是个标志字符串而已）
2. / 是请求的路径（这代表根路径）
3. HTTP/1.1  中，1.1是版本号，通用了20年

具体字符串是 'GET / HTTP/1.1\r\nhost:g.cn\r\n\r\n'

响应返回的数据如下

```
HTTP/1.1 301 Moved Permanently （协议版本号 状态码（比如404））
Alternate-Protocol: 80:quic,p=0,80:quic,p=0
Cache-Control: private, max-age=2592000
Content-Length: 218
Content-Type: text/html; charset=UTF-8
Date: Tue, 07 Jul 2015 02:57:59 GMT
Expires: Tue, 07 Jul 2015 02:57:59 GMT
Location: http://www.google.cn/
Server: gws
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
```



其中响应行（第一行）：

1. HTTP/1.1 是版本
2. 301 是「状态码」，参见文末链接
3. Moved Permanently 是状态码的描述
   浏览器会自己解析Header部分，然后将Body显示成网页

URL只能使用ASCII，因此若有ASCII集合外的字符，会用 "%" 其后跟随两位的十六进制数来替换非 ASCII 字符。比如空格，中文等。



##AJAX (异步 JavaScript 和 XML)

AJAX函数普通情况下只能在同一个网站里使用，不能跨域！
```
// 创建 AJAX 对象
var r = new XMLHttpRequest()
//XMLHttpRequest用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
// 设置请求方法和请求地址
r.open('GET', '/login', true)
// 注册响应函数
r.onreadystatechange = function() {
    console.log('state change', r)
}
// 发送请求
r.send()

// 创建 AJAX 对象
var r = new XMLHttpRequest()
// 设置请求方法和请求地址
r.open('POST', '/login', true)
// 设置发送的数据的格式
r.setRequestHeader('Content-Type', 'application/json')
// 注册响应函数
r.onreadystatechange = function() {
    if (r.readyState === 4) {
        console.log('state change', r, r.status, r.response)
        var response = JSON.parse(r.response)
        console.log('response', response)
    } else {
        console.log('change')
    }
}
// 发送请求
var account = {
    username: 'gua',
    password: '123',
}
var data = JSON.stringify(account)
r.send(data)

//如需获得服务器发回的响应
r.responseText	获得字符串形式的响应数据。
r.responseXML	获得 XML 形式的响应数据。

r.status:响应的HTTP状态
r.statusText:HTTP状态的说明

```

open中的第三个值true异步（默认）/false同步
异步：AJAX代码运行中的时候其他代码一样可以运行（有点类似两个线程）
同步：当JS代码加载到当前AJAX的时候会把页面里所有的代码停止加载，页面出现假死状态，当这个AJAX执行完毕后才会继续运行后面的代码页面，假死状态解除。

对于异步，readystate属性值服务器返回的含义：
0：初始化。尚未调用open（）方法。
1：启动。已经调用open（）方法，但尚未调用send（）方法。
2：发送。已经调用send（）方法，但尚未接收到响应。
3：接收。已经接收到部分响应数据。
4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。

readyState属性值每变一次都会触发一次readystatechange事件

r.status	
200: "OK"
404: 未找到页面

[跨域与安全限制](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499861493e7c35be5e0864769a2c06afb4754acc6000)


与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。
然而，在以下情况中，请使用 POST 请求：
无法使用缓存文件（更新服务器上的文件或数据库）
向服务器发送大量数据（POST 没有数据量限制）
发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

由于GET请求可以将查询字符串参数追加到URL的末尾，因此需要对字符串每个参数都是用`encodeURIComponent()`进行编码

HTTP头部信息：
Accept:浏览器能处理的内容类型
Accept-Charset:浏览器能显示的字符集
Accept-Encoding:浏览器能处理的压缩编码
Accept-Language:浏览器当前设置的语言
Connection: 浏览器与服务器间连接的类型
Cookie:
Host:当前页面所在域
Referer:发出请求的页面的URI
User-Agent:浏览器的用户代理字符串



##JSON

JavaScript 对象表示法
存储和交换文本信息的语法
JSON中的数据类型：
简单值：number；boolean；string；null；不支持undefined
数组：array；
对象：object
JSON的字符串规定必须用双引号""，Object的键也必须用双引号""。
末位不需要分号，这不是javascript语句
JSON中没有变量的概念，故不存在声明变量

**序列化：**
`JSON.stringify(value[, replacer [, space]])`
除javascript对象外，还可以接收两个参数
第二个参数是个过滤器filter，可以是一个数组，返回数组内存在的属性；也可以是一个函数function（key，value），针对key以及函数返回
第三个参数用数字或者字符串表示是否在JSON字符串中保留缩进与换行（数字即每个级别缩进的空格数，字符串即被用作缩进字符）

不可枚举的属性会被忽略

toJSON()方法可以作为函数过滤器的补充，当stringify时，序列化的顺序：
1.若存在toJSON方法且能获得有效值，则调用。
2.若提供了第二个参数，则应用，传入值为第一步返回的值
3.对上一步返回值进行序列化
4.根据第三个参数进行相应的格式化

`JSON.parse(text[, reviver])`解析一个JSON字符串，构造由字符串描述的JavaScript值或对象。可以提供可选的reviver函数以在返回之前对所得到的对象执行变换。

| JavaScript 类型 | JSON 区别                                                    |
| :-------------- | :----------------------------------------------------------- |
| 对象和数组      | 属性名称必须是双引号字符串；后缀逗号被禁止。最后一个属性后面不能有逗号。 |
| 数组            | 前导0是禁止的（在JSON.stringify 0将被忽略，但在JSON.parse它将抛出SyntaxError）；小数点后必须至少有一位数字。 |
| 字符串          | 只有有限的一些字符可能会被转义；字符串必须是双引号。         |

在对象中：
JSON 只允许"property": value syntax形式的属性定义。属性名必须用双引号括起来。且属性定义不允许使用简便写法。
JSON中，属性的值仅允许字符串，数字，数组，true，false，或者其他JSON对象。 
JSON中，不允许将值设置为函数。
 Date 等对象，经JSON.parse()处理后，会变成字符串。
JSON.parse() 不会处理计算的属性名，会当做错误抛出。



## Get&Post区别

[链接](https://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd)
1.GET在浏览器回退时是无害的，而POST会再次提交请求。
2.GET产生的URL地址可以被Bookmark，而POST不可以。
3.GET请求会被浏览器主动cache，而POST不会，除非手动设置。
4.GET请求只能进行url编码，而POST支持多种编码方式。
5.GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
6.GET请求在URL中传送的参数是有长度限制的，而POST没有。
7.对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
8.GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
9.GET参数通过URL传递，POST放在Request body中。

本质上，GET / POST 都是基于 TCP/IP 的 HTTP 协议的发送请求（TCP 链接），而HTTP只是规定 了他们的语义以及参数的传送方式，而不同的浏览器和服务器（多数浏览器限制 url 长度在2K 字节内，多数服务器最多处理64K 大小的 url） 对于不同方式也有着不同的处理方式，比如如果你用 GET，在 request body 传参，有些服务器可能会读数据，有些会直接忽略。
最后一个区别：GET 产生一个 TCP 数据包；POST 产生两个 TCP 数据包（部分浏览器 POST 也仅发送一次，比如 Firefox）。
GET:浏览器会把 header 和data 一起发出去，服务器响应；而 POST，浏览器会先发送 header，服务器响应100 continue，浏览器再发送 data，服务器响应200 ok