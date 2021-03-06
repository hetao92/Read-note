[TOC]



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

基于请求与响应模式、无状态、应用层的协议

位于tcp/ip四层模型中的应用层

超文本传输协议

1. 浏览器（客户端）按照规定的格式发送文本数据（请求）到服务器
2. 服务器解析请求，按照规定的格式返回文本数据到浏览器
3. 浏览器解析得到的数据，并做相应处理



对 TCP 协议来说，TCP 协议是一条双向的通讯通道，HTTP 在 TCP 基础上，规定了 Request-Response 模式，这模式决定了通讯必定是由浏览器端首先发起

**特点**

+ 支持客户/服务器模式
+ 简单快速，客户请求服务时，只需传送请求方法和路径，通信速度快
+ 灵活 ：允许传输任意类型数据对象
+ 无连接：每次连接只处理一个请求，收到应答即断开连接，可节省传输时间
+ 无状态协议： 对于事务处理无记忆能力
+ 同源策略
+ 明文传输，不做加密，因此缺少安全性



请求和返回是一样的数据格式，分为4部分：

1. 请求行或者响应行
2. Header（消息报头，请求的 Header 中 Host 字段是必须的，其他都是可选）
3. \r\n\r\n（连续两个换行回车符，用来分隔Header和Body）空白行来通知服务器，它已经结束了该头信息的发送
4. Body（请求/响应正文，可选，浏览器发的数据）



请求的格式，注意大小写（这是一个不包含Body的请求）：
原始数据如下
'GET / HTTP/1.1\r\nhost:g.cn\r\n\r\n'
打印出来如下
GET / HTTP/1.1
Host: g.cn

其中
1. GET 是**请求方法**（还有POST等，这就是个标志字符串而已）
2. / 是请求的**路径URI**（这代表根路径）
3. HTTP/1.1  中，1.1是**HTTP协议版本号**，通用了20年

具体字符串是 'GET / HTTP/1.1\r\nhost:g.cn\r\n\r\n'



请求方法有：GET/POST/HEAD/PUT/DELETE/CONNECT/OPTIONS/TRACE

HEAD和 GET 类似，只返回请求头，多数由 JavaScript 发起

OPTIONS 和 TRACE 一般用于调试，多数线上服务都不支持



消息报头常用的有：

host	

user-agent	

accept	浏览器端接受的格式

accept-language	浏览器接受的语言，用于服务端判断多语言

accept-charset

Accept-Encoding	浏览器端接受的编码方式

Content-Type	

connection 连接方式，如果是 keep-alive，且服务端支持，则会复用连接	

cookie

Cache-control 缓存的时效性

If-modified-since 上次访问时的更改时间

If-none-match 上次访问时使用的 E-Tag

charset是字符集，如 ASCII、GB2312 、Unicode 等，其中 utf-8 是针对 Unicode的可变长度，是 ASCII 的超集

encoding 是字符编码

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

1. HTTP/1.1 是**版本**
2. 301 是**「状态码」**
3. Moved Permanently 是**状态码的描述**
   浏览器会自己解析Header部分，然后将Body显示成网页

URL只能使用ASCII，因此若有ASCII集合外的字符，会用 "%" 其后跟随两位的十六进制数来替换非 ASCII 字符。比如空格，中文等。

**状态码**

1** ：请求已接收，继续处理

2** ：请求已被成功接收、理解、接受

+ 202 服务器已收到请求，但还未进行处理，会在未来再处理，常用于异步操作
+ 203 非授权信息，请求成功。但返回的meta信息不在原始的服务器，而是一个副本
+ 204 无内容。服务器成功处理，但未返回内容。

3** ：重定向，要完成请求需进一步操作

+ 301 永久移动
+ 302 临时移动
+ 303 参考另一个 URL，也是暂时重定向(302，307用于 get 请求，而 303 用于 post、put、delete 请求。收到 303浏览器不会自动跳转，由用户决定下一步)
+ 304 所请求资源未修改，服务器不返回任何资源
+ 305 使用代理
+ 306 已经被废弃的 HTTP 状态码
+ 307 临时重定向

　302跳转是暂时的跳转，搜索引擎会抓取新的内容而保留旧的网址。因为服务器返回302代码，搜索引擎认为新的网址只是暂时的。

　　301重定向是永久的重定向，搜索引擎在抓取新内容的同时也将旧的网址替换为重定向之后的网址。

4** ：客户端错误，请求无法实现或者有语法错误

+ 401 未授权访问页面
+ 402 为了将来需求而预留
+ 403 禁止访问
+ 404 未找到
+ 405 客户端请求中的方法未允许
+ 408 服务器等待客户端发送的请求时间过长，超时
+ 410 请求资源已从这个地址转移，不再可用

5** ：服务器端错误，服务器未能实现合法的请求

+ 500 服务器内部错误，无法完成请求

+ 501 服务器不支持请求中要求的功能

+ 502 错误的网关
+ 503 由于超载或系统维护，服务器暂时的无法处理客户端的请求。
+ 504 网关超时

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



referer 历史拼写错误。用户在地址栏输入网址，或者选中浏览器书签，就不发送`Referer`字段。

`document.referrer`可以查看当前页面的引荐来源。注意，这里采用的是正确拼写。

它的作用：用于用户追踪。有些网站不允许图片外链，只有自家的网站才能显示图片，外部网站加载图片就会报错。它的实现就是基于`Referer`字段，如果该字段的网址是自家网址，就放行。

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

​		`get`用于获取资源，没有副作用，是幂等的，执行多少遍不影响最终存储的结果。而`post`每次调用都会创建新的资源。(幂等性是指一次和多次请求某一个资源应该具有同样的**副作用**，不是说会得到不同的结果，比如删除一次和多次应该是一样的)([幂等性](https://www.cnblogs.com/weidagang2046/archive/2011/06/04/idempotence.html))。POST所对应的URI并非创建的资源本身，而是资源的接收者
2.GET产生的URL地址可以被Bookmark（被书签保存），而POST不可以。
3.GET请求会被浏览器主动cache，而POST不会，除非手动设置。
4.GET请求只能进行url编码，而POST支持多种编码方式。

+ application/x-www-form-urlencoded
+ multipart/form-data（多用于上传文件）
+ application/json
+ text/xml

5.GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
6.GET请求在URL中传送的参数是有长度限制的，而POST没有。
7.对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
8.GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
9.GET参数通过URL传递，POST放在Request body中。

本质上，GET / POST 都是基于 TCP/IP 的 HTTP 协议的发送请求（TCP 链接），而HTTP只是规定了他们的语义以及参数的传送方式，而不同的浏览器和服务器（多数浏览器限制 url 长度在2K 字节内，多数服务器最多处理64K 大小的 url） 对于不同方式也有着不同的处理方式，比如如果你用 GET，在 request body 传参，有些服务器可能会读数据，有些会直接忽略。
最后一个区别：GET 产生一个 TCP 数据包；POST 产生两个 TCP 数据包（部分浏览器 POST 也仅发送一次，比如 Firefox）。
GET:浏览器会把 header 和data 一起发出去，服务器响应；而 POST，浏览器会先发送 header，服务器响应100 continue，浏览器再发送 data，服务器响应200 ok。两次包的TCP在验证数据包完整性上，有非常大的优点。但是并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次。



拓：

get请求安全且幂等；post 不安全且不幂等；put 不安全但幂等；delete 不安全但幂等

## 互联网协议

[参考](https://juejin.im/post/5b3aecd6f265da62ed1080fe#heading-17)

+ 应用层(客户端、服务器)

  > 应用层是与网络相关的程序为了通过网络与其他程序通信所使用的层。数据从网络相关程序以这种应用内部使用的格式进行传送，然后被编码成标准协议的格式。
  >
  > TCP 协议可以为各种各样的程序传递数据，比如 Email、WWW、FTP 等等。那么，必须有不同协议规定电子邮件、网页、FTP 数据的格式，这些应用程序协议就构成了"应用层"。

+ 传输层(TCP)

  > 网络层实现了主机到主机的通信，而传输层是建立端口到端口的通信，因此 Unix 系统把主机+端口叫做套接字(socket)。
  >
  > 在 IP 地址 和 Mac 地址的协助下，计算机可以实现全网络下通信了，但是需要区分不同的网络请求，也就是说当接受一个数据包，如何分辨它是网页内容还是聊天内容，这时候需要一个叫做“端口”的参数来确定使用这个数据包的程序(进程)。
  >
  > UDP 协议（无连接的包传输用户数据报协议）就是加上了端口信息的数据包。标头定义了发出端口和接收端口，数据部分就是具体的内容，该数据包存储在 IP 数据包中。
  >
  > TCP 协议（面向连接的传输控制协议）就是带有必须确认功能的 UDP 协议。每发出一个数据包都需要得到对方的确认，一旦得不到哪个数据包的确认，就知道需要重发这个数据包了。

+ 网络层(IP)

  > 由于广播的局限性会导致不在同一子网络下的计算机无法通信，且每个计算机”人手一包“的效率也是低下的。于是网络层引入一套新的地址，使我们能区分不同计算机是否属于同一网络，这个就叫做网络地址，简称”网址“。
  >
  > 计算机有了两个地址。一个是 Mac 地址，一个是网络地址。前者是绑定网卡上的，用于接受子网络下广播的数据包，而后者是管理员分配。处理顺序也是后者先于前者，毕竟要先知道你在哪个省再在哪个市。
  >
  > 而网络地址遵循 IP 协议

+ 链路层(网络) 位于实体层上方，确定 0 1 的分组方式

  >以太网协议规定一组电信号构成一个数据包，叫做“帧”，每一帧分成两个部分：标头(head)和数据(data)。 标头说明数据包的发送者、接受者，数据类型等等，而数据则是数据包的具体内容。
  >
  >以太网规定了连入网络的所有设备都必须具备“网卡”接口，数据包都是从一块网卡传递到另一块网卡，网卡的地址就是 Mac 地址。每一个 Mac 地址都是独一无二的，具备了一对一的能力。
  >
  >通过 ARP 协议，向本网络内的所有计算机发送，接受方通过标头来与自身 Mac 地址比较，如果一致就接受并处理，否则则抛弃。
  >
  >通过以太网协议、Mac 地址、广播，链路层就实现了在同一网络内的多计算机通信。

+ 实体层(光缆电缆等物理连接)





## URL请求过程

地址解析，DNS解析，寻找 ip

封装请求数据包

封装成TCP包，建立TCP连接

发送请求命令

服务器响应

浏览器开始接受数据文件

关闭TCP连接

浏览器对资源解析，并建立DOM结构

载入资源文件，渲染页面

一个可选的步骤是对位图进行合成，这会极大地增加后续绘制的速度



HTTP 协议高于 TCP/IP 协议

TCP是应用软件和网络软件间的通信

IP 是计算机之间的通信



HTTPS使用了HTTP协议，但使用了不同于HTTP协议的默认端口及一个加密身份验证



**三次握手:**

第一次握手：主机A发送位码为syn＝1，随机产生seq number=1234567的序列号数据包到服务器，并进入SYN_SEND状态，等待服务器确认。主机B由SYN=1知道，A要求建立联机；

 第二次握手：主机B收到请求后要确认联机信息，向A发送ack number=(主机A的seq+1)，syn=1，ack=1确认号，随机产生seq=7654321的包，此时服务器进入SYN_RECV状态；；

 第三次握手：主机A收到后检查ack number是否正确，即第一次发送的seq number+1，以及位码ack是否为1，若正确，主机A会再发送ack number=(主机B的seq+1)，ack=1，主机B收到后确认seq值与ack=1则连接建立成功。

 完成三次握手，主机A与主机B开始传送数据。客户端和服务器进入ESTABLISHED状态。



SYN表示建立连接synchronous，

FIN表示关闭连接finish，

ACK表示确认，响应，acknowledgement，

PSH表示有 DATA数据传输push，

RST表示连接重置reset。



**四次挥手**

客户端或服务器均可主动发起挥手动作。四次挥手也叫做改进的三次握手

第一次挥手：假设客户端想关闭连接，则发送一个 FIN 码为 1 ，seq为随机数 x 的包，表示自己已经没有数据可以发送，但仍然可以接受数据。发送完毕后客户端进入 FIN_WAIT_1状态

第二次挥手：服务器确认客户端的 FIN 包，发送一个确认包ACK=1, ACK num = x + 1，标明自己接收到了客户端关闭连接的请求，但还没准备好关闭连接。发送完毕后，服务器端进入 CLOSE_WAIT 状态，客户端接收到确认包后，进入 FIN_WAIT_2 状态，等待服务器端关闭连接

第三次挥手：服务器端准备好关闭连接时，向客户端发送结束连接请求，FIN 为 1，seq = y。发送完毕后，服务端进入 LAST_ACK 状态，等待来自客户端的最后一个 ACK

第四次挥手：客户端接收到关闭请求后，发送一个确认包，并进入 TIME_WAIT 状态，等待可能出现的要求重传的 ACK 包。

服务器端接收到确认包后，关闭连接，进入 CLOSED 状态。

客户端等待了某个固定时间（两个最大段生命周期，最大报文段生存时间2MSL，2 Maximum Segment Lifetime）之后，没有收到服务器端的 ACK ，认为服务器端已经正常关闭连接，于是自己也关闭连接，进入 CLOSED 状态。





连接的时候是三次握手，关闭的时候却是四次握手？

因为当Server端收到Client端的SYN连接请求报文后，可以直接发送SYN+ACK报文。其中ACK报文是用来应答的，SYN报文是用来同步的。但是关闭连接时，当Server端收到FIN报文时，很可能并不会立即关闭SOCKET，所以只能先回复一个ACK报文，告诉Client端，"你发的FIN报文我收到了"。只有等到我Server端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送。故需要四步握手。





## URL->页面加载

+ 查看浏览器缓存

  若未缓存，则发起请求；

  若被缓存，验证是够新鲜(expires、cache-control)，若否，则发送请求

+ 解析URL获得协议、主机、端口、路径等信息

+ 组装 HTTP request 报文

+ DNS查询

+ TCP握手：应用层下发数据到传输层，指明端口号到网络层，网络层确定 IP 地址然后指示数据传输要如何跳转路由器，最后封装成数据帧到数据链路层。

+ TLS握手

+ 发送HTTP请求

+ 服务器检查HTTP的缓存头部(协议缓存ETag/last-modified)

+ 浏览器接受请求，根据情况选择关闭TCP连接或者保留复用，断开连接四次握手

+ 浏览器检查 status code

+ 若资源可缓存，进行缓存

+ 解码(Gzip)

+ 解析 HTML 文档

  构建DOM树

  根据 css 构建 css rule tree

  关联以上构建渲染树

  布局 layout

  painting



在这一过程中，涉及到网络层面的主要有三个主要过程：DNS 解析，TCP连接，HTTP 请求/响应。前端在前两步能优化的程度有限，对于最后一步，主要还是减少请求次数和减少单次请求所花费的时间



### DNS查询

首先在本地域名服务器中查询IP地址，如果没有找到的情况下，本地域名服务器会向根域名服务器发送一个请求，如果根域名服务器也不存在该域名时，本地域名会向com顶级域名服务器发送一个请求，依次类推下去。直到最后本地域名服务器得到google的IP地址并把它缓存到本地，供下次查询使用

DNS同时占用**UDP**和**TCP**端口

> TCP---传输控制协议，一种**面向连接**的协议，提供可靠的数据传输，传输速度慢，适用于传输大量数据，可靠性要求高的场合。
>
> 可靠：协议中包含了专门的传递保证机制。当数据接收方收到发送方传来的信息时，会自动向发送方发出确认消息；发送方只有在接收到该确认消息之后才继续传送其它信息，否则将一直等待直到收到确认信息为止。
>
> UDP---用户数据报协议，是一种**无连接**的传输层协议，提供面向事务的简单不可靠信息传送服务。 无连接就是说它不与对方建立连接，而是直接就把数据包发送过去！ 传输速度快，适用于一次只传送少量数据、对可靠性要求不高的应用环境。
>
> 从发送方到接收方的传递过程中出现数据报的丢失，协议本身并不能做出任何检测或提示。但是UDP有TCP望尘莫及的速度优势。所以DNS用UDP较多

优化点：

DNS缓存：多级缓存，浏览器缓存，系统缓存，路由器缓存，IPS服务器缓存，根域名服务器缓存，顶级域名服务器缓存，主域名服务器缓存。

DNS负载均衡(DNS重定向)：DNS服务器会返回一个跟用户最接近的点的IP地址给用户，CDN节点的服务器负责响应用户的请求，提供所需的内容。

(CDN content delivery network)



## HTTPS

[参考](https://mp.weixin.qq.com/s/3NKOCOeIUF2SGJnY7II9hA)

SSL(Secure Socket Layer安全套接层)

TLS(Transport Layer Security安全层传输协议)

配合HTTP协议可加密其中的通信内容

请求或响应在传输途中，遭攻击者拦截并篡改内容的攻击称为中间人攻击



而HTTPS并非是应用层的新协议，只是HTTP通信接口部分用SSL和TLS协议代替而已。



客户端服务器在**通信交换报文阶段**通过对称加密使用同一秘钥可以获取发送数据，但需要对每个客户端使用不同的对称加密算法。同时秘钥发给对方需要保证安全，不能被劫持获取到。



所以在**交换秘钥阶段**使用非对称加密，客户端使用公钥加密，服务端使用私钥解密，那么客户端往服务端传送数据一定是安全的，因为只有服务端有私钥，即使被劫持也没有私钥可以解密。



而客户端需要一开始就持有公钥，所以服务器端需要将公钥发送给客户端。这个过程不能被劫持，可以被中间人用自有的公钥秘钥解析后传递，同样能劫持到数据，而客户端此时没法判断这个公钥是不是服务端发过来的。因此需要由数字证书认证机构(CA)第三方机构来认证。

服务器将由CA颁发的公钥证书发送给客户端，而客户端可使用CA的公钥进行数字签名验证（多数浏览器在版本内会植入常见CA的公钥），一旦通过，那么说明服务器发送的公钥是可信赖的。



但是中间人同样可以对证书调包，因为大家都可以使用第三方机构的公钥进行解密。所以客户端需要验证证书。而证书上写着如何根据证书的内容生成证书编号，客户端拿到证书后根据证书上的方法自己生成一个证书编号，若两者相同，则真实。



握手(handshake): 客户端和服务器建立连接并交换参数

1. 客户端给出协议版本号、一个生成的随机数以及客户端支持的加密方法
2. 服务器确认双方使用的加密方法，并给出数字证书（包含公钥），以及一个服务器生成的随机数
3. 客户端确认数字证书有效，并生成一个新的随机数秘钥，并使用数字证书中的公钥，加密这个随机数，发给服务端
4. 服务端使用自己的私钥，获取客户端发来的随机数
5. 双方依据约定的加密方法，使用前面三个随机数，生成对话秘钥(session key)，用来加密接下来的对话



**另一个版本参考**

1. 客户端通过发送 Client Hello 报文开始 SSL 通信。报文中包含客户端支持的 SSL 的指定版本、加密组件(Cipher Suite)列表(所使用的加密算法及密钥长度等)

2. 服务器可进行 SSL 通信时，会以 Server Hello 报文作为应答。和客户端一样，在报文中包含 SSL 版本以及加密组件。服务器的加密组件内容是从接收 到的客户端加密组件内筛选出来的。

3. 之后服务器发送 Certificate 报文。报文中包含公开密钥证书。

4. 最后服务器发送 Server Hello Done 报文通知客户端，最初阶段的 SSL 握手协商部分结束。

5. SSL 第一次握手结束之后，客户端以 Client Key Exchange 报文作为回应。报文中包含通信加密中使用的一种被称为 Pre-master secret 的随机密码串。该 报文已用步骤 3 中的公开密钥进行加密。

6. 接着客户端继续发送 Change Cipher Spec 报文。该报文会提示服务器，在此报文之后的通信会采用 Pre-master secret 密钥加密。

7. 客户端发送 Finished 报文。该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准。

8. 服务器同样发送 Change Cipher Spec 报文。

9. 服务器同样发送 Finished 报文。

10. 服务器和客户端的 Finished 报文交换完毕之后，SSL 连接就算建立完成。当然，通信会受到 SSL 的保护。从此处开始进行应用层协议的通信，即发 送 HTTP 请求。

11. 应用层协议通信，即发送 HTTP 响应。

12. 最后由客户端断开连接。断开连接时，发送 close_notify 报文。



非对称加密

**特点是私钥加密后的密文，只要是公钥，都可以解密，但是公钥加密后的密文，只有私钥可以解密。私钥只有一个人有，而公钥可以发给所有的人**。



概括：HTTPS要保证客户端与服务器端的通信安全，必须使用的对称加密算法，但是协商对称加密算法的过程，需要使用非对称加密算法来保证安全，然而直接使用非对称加密的过程本身也不安全，会有中间人篡改公钥的可能性，所以客户端与服务器不直接使用公钥，而是使用数字证书签发机构颁发的证书来保证非对称加密过程本身的安全。



HTTPS缺陷：使用SSL时处理速度会变慢

## 跨域

**同源策略**就是浏览器出于网站安全性的考虑，限制不同源之间的资源相互访问的一种政策。以下操作具有同源策略的限制：

- AJAX 请求不能发送。
- 无法获取DOM元素并进行操作。
- 无法读取Cookie、LocalStorage 和 IndexDB 。

有些请求是不受到跨域限制。例如：WebSocket，script、img、iframe、video、audio标签的`src`属性等。



解决方案：

**使用代理 proxy**

将请求移至后端转发，避开浏览器层面

**CORS**

Cross-origin-resource sharing.CORS通过相应的请求头与响应头来实现跨域资源访问。

origin请求头标明请求来源，是浏览器自动添加。

服务器端的响应头中一个头信息为`Access-Control-Allow-Origin`，表明接受的跨域请求来源，设置为`*`，则会接受所有域的请求。

CORS默认是不会发送cookie。`Access-Control-Allow-Credentials`的响应头，如果设置为`true`则表明服务器允许该请求内包含cookie信息。并在客户端 ajax 请求中设置`withCredentials`属性为`true`。

**jsonp**

通过`script`标签发起的请求，携带回调函数。

只支持`get`请求，兼容性非常好。



CORS分为  **简单请求** 和 **复杂请求**

**简单请求**

> HTTP方法只能是：HEAD / GET / POST
>
> HTTP头信息不超过以下字段
>
> ```
> Accept
> Accept-Language
> Content-Language
> Last-Event-ID
> Content-Type，但仅能是下列之一
> 	application/x-www-form-urlencoded
>     multipart/form-data
>     text/plain
> ```

**简单请求**的发送从代码上来看和普通的XHR没太大区别，但是HTTP头当中要求总是包含一个域（Origin）的信息。该域包含协议名、地址以及一个可选的端口。不过这一项实际上由浏览器代为发送，并不是开发者代码可以触及到的。

```
简单请求的部分响应头

Access-Control-Allow-Origin（必含）- 不可省略，否则请求按失败处理。该项控制数据的可见范围，如果希望数据对任何人都可见，可以填写"*"。
Access-Control-Allow-Credentials（可选） – 该项标志着请求当中是否包含cookies信息，只有一个可选值：true（必为小写）。如果不包含cookies，请略去该项，而不是填写false。这一项与XmlHttpRequest2对象当中的withCredentials属性应保持一致，即withCredentials为true时该项也为true；withCredentials为false时，省略该项不写。反之则导致请求失败。
Access-Control-Expose-Headers（可选） – 该项确定XmlHttpRequest2对象当中getResponseHeader()方法所能获得的额外信息。通常情况下，getResponseHeader()方法只能获得如下的信息：
Cache-Control
Content-Language
Content-Type
Expires
Last-Modified
Pragma
当你需要访问额外的信息时，就需要在这一项当中填写并以逗号进行分隔
```



**复杂请求**

不满足上述要求的请求即为复杂请求，比如说你需要发送PUT、DELETE等HTTP动作，或者发送Content-Type: application/json的内容。它不仅有包含通信内容的请求，同时也包括**预请求**

**预请求**实际上是对服务端的一种权限请求，只有当预请求成功返回，实际请求才开始执行。

预请求以OPTIONS形式发送，当中同样包含域，并且还包含了两项CORS特有的内容：

Access-Control-Request-Method – 该项内容是实际请求的种类，可以是GET、POST之类的简单请求，也可以是PUT、DELETE等等。
Access-Control-Request-Headers – 该项是一个以逗号分隔的列表，当中是复杂请求所使用的头部。

```
复杂请求的部分响应头

Access-Control-Allow-Origin（必含） – 和简单请求一样的，必须包含一个域。
Access-Control-Allow-Methods（必含） – 这是对预请求当中Access-Control-Request-Method的回复，这一回复将是一个以逗号分隔的列表。尽管客户端或许只请求某一方法，但服务端仍然可以返回所有允许的方法，以便客户端将其缓存。
Access-Control-Allow-Headers（当预请求中包含Access-Control-Request-Headers时必须包含） – 这是对预请求当中Access-Control-Request-Headers的回复，和上面一样是以逗号分隔的列表，可以返回所有支持的头部。这里在实际使用中有遇到，所有支持的头部一时可能不能完全写出来，而又不想在这一层做过多的判断，没关系，事实上通过request的header可以直接取到Access-Control-Request-Headers，直接把对应的value设置到Access-Control-Allow-Headers即可。
Access-Control-Allow-Credentials（可选） – 和简单请求当中作用相同。
Access-Control-Max-Age（可选） – 以秒为单位的缓存时间。预请求的的发送并非免费午餐，允许时应当尽可能缓存。
```





## 缓存

[参考](https://github.com/amandakelake/blog/issues/41)

重用已获取的资源，减少延迟与网络阻塞，减少显示资源所等待的时间，响应性更快

![缓存分类](/Users/hetaohua/Documents/Read-note/img/cache.png)



- **强缓存**

  > 强缓存表示在缓存期间不需要请求
  >
  > 浏览器在请求某一资源时，会先获取该资源缓存的header信息，判断是否命中强缓存。可以通过两种响应头实现：`Expires` 和 `Cache-Control`。expires受限于本地时间，容易被修改。`Expires` 是 HTTP / 1.0 的产物，`Cache-Control` 出现于 HTTP / 1.1
  >
  > cache-control 优先级高于 expires
  >
  > 浏览器在加载资源时，根据这两个字段判断，若读取缓存，则不会发送请求到服务器
  >
  > `Expires: Wed, 22 Oct 2018 08:41:00 GMT`

  Cache-Control：

  + No-cache: 不使用本地可能过期的缓存（**实际还是可以缓存到本地！**），使用协商缓存前必须与服务器验证资源是否过期，不能擅自提供给客户端，若有效、资源并未被修改，则读取缓存避免重新下载

  + No-store：在 no-cache 基础上，连服务端的缓存确认也绕开了，禁止浏览器缓存数据，每次请求都会下载完整资源

  + public：可以被所有用户缓存，包括终端用户和CDN等中间代理服务器
  + private：只能被终端用户的浏览器缓存，不能作为共享缓存，即代理服务器不能缓存

  200（from memory）资源在内存当中，一般脚本、字体、图片会存在内存当中

  200（from disk cache）在磁盘当中，一般非脚本会存在内存当中，如css等

  对于脚本随时可能会执行，若存在磁盘，io 开销比较大，容易失去响应

  [图解](https://zhuanlan.zhihu.com/p/55623075)

- **协商缓存**

  > 如果没有命中强缓存，浏览器会发送请求到服务器，请求会携带第一次请求返回的有关缓存的header字段信息（Last-Modified/If-Modified-Since和Etag/If-None-Match），由服务器根据请求中的相关header信息来比对结果是否协商缓存命中。若有变动就发送新资源回来，没有则返回**304(not modified)**，从本地读取缓存。etag 优先级高于 last-modified

  可能某些文件修改频繁（秒以下）If-Modified-Since能检查到的粒度是s级的，这种修改无法判断

  某些文件会周期性修改，实际内容不变只是改变修改时间，服务器可能不希望客户端认为它被修改了。

  服务器可能不能精确得到最后修改时间。基于这些原因，添加 etag 标识符

  ​	last-modified表示本地文件最后修改日期
  
  ​	if-modified-since会在 request header 上返回last-modified的值
  
  ​	ETag类似指纹，与修改时间无关联
  
  ​	if-none-match返回上次带过来的 etag 给服务器



1. cookie：4K，可以手动设置失效期，可能会有主Domain污染的情况
2. localStorage：5M，除非手动清除，否则一直存在
3. sessionStorage：5M，**不可以跨标签访问**，页面关闭就清理
4. indexedDB：浏览器端数据库，无限容量，除非手动清除，否则一直存在



除此之外，还有几类

+ MemoryCache

  内存中的缓存，他是浏览器最先尝试去命中的缓存，也是响应速度最快的，对于 Base64 格式的图片以及体积不大的 js、css 文件等都可以写进去

+ Service Worker Cache

  独立于主线程之外的 JavaScript 线程，脱离于浏览器窗体，因此无法直接访问 DOM，所以它往往无法干扰页面性能，可以用来实现离线缓存，消息推送和网络代理等功能

+ Push Cache

  HTTP2 在 server push 阶段存在的缓存，他是缓存的最后一道防线，浏览器只有在 memory cache、HTTP cache 以及 service worker cache 均未命中的情况下才会去访问 push cache。这是一种存在于会话阶段的缓存，当 session 终止时，缓存即被释放。如果不同页面共享同一 HTTP2 连接，那么可以共享同一个 Push cache

### Storage

5M空间。

只能操作字符串(JSON.stringify/parse)。

明文存储。

可以监听`window.addEventListener('storage',callback)`



### cookie & session

一个用户的所有请求操作都应该属于同一个会话。但是HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。

由服务器端生成的为辨别用户身份进行追踪、发送给浏览器并储存在用户本地终端上的数据，有一定的生存周期，随每一个请求发送至同一个服务器

cookie机制采用的是在客户端保持状态的方案

cookie的作用就是为了解决HTTP协议无状态的缺陷



Set cookie

Name: value

Expires 失效时间

Domain 域	可以不同二级域名下共享cookie，而Storage不可以

path 路径

secure 安全标志， 只能在 https 下发送 cookie



**session机制**采用的是一种在服务器端保持状态的解决方案。服务器使用一种类似于散列表的结构来保存信息。同时我们也看到，由于采用服务器端保持状态的方案在客户端也需要保存一个标识，所以session机制可能需要借助于cookie机制来达到保存标识的目的。而session提供了方便管理全局变量的方式 。

session是针对每一个用户的，变量的值保存在服务器上，用一个sessionID来区分是哪个用户session变量,这个值是通过用户的浏览器在访问的时候返回给服务器，当客户禁用cookie时，这个值也可能设置为由get来返回给服务器。

当程序为客户端的请求创建session 时，服务器会先检查客户端请求里是否包含了 session 标识，若已包含，服务器就按照 session id 把这个 session 检索出来(检索不到则新建)，如果客户端请求不包含，则重建session 并生成相关联的 session id，并在响应中返回给客户端保存。客户端保存 session id 的方式可以用 cookie，或者附加在URL路径后面，或者表单隐藏字段等



两者不同：

1. **存取方式**---session 能存取任何类型数据，包括String/Integer/List/Map/Java类等。cookie 只能保管 ASCII 码，对于Unicode或者二进制需要先编码。
2. **隐私策略**---cookie 存储在客户端是可见，容易被程序复制修改等操作，需要进行加密。而 session 是存储在服务器上，不存在敏感信息被透露的风险。
3. **有效期**---有效期上，cookie 可以设置很长的过期时间。而 session依赖于名为JSESSIONID的 cookie，默认-1，不能达到长久有效的效果，即使设置时间过长，服务器累积的 session也会过多，导致内存溢出
4. **服务器压力**---session 保管在服务端，若并发量过多，则会产生过多的 session，耗费内存。而 cookie 保留在客户端不占用服务器资源。
5. **浏览器支持**---cookie 需要客户端浏览器支持，若禁用或不支持则失效。
6. **跨域支持**---cookie 支持跨域名访问，而 session 则仅在他所在的域名内有效



### Storage和cookie的区别

共同点：都是保存在浏览器端、且同源的
区别：

- cookie数据始终在同源的http请求中携带（即使不需要），而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下
- 存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大
- 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭之前有效；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭
- 作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的
- web Storage支持事件通知机制，可以将数据更新的通知发送给监听者
- web Storage的api接口使用更方便



缓存实践

+ 配置本地缓存
+ 采用内容摘要作为缓存更新依据
+ 静态资源CDN部署
+ 资源发布路径实现非覆盖式发布
+ 对于频繁变动的资源可以使用no-catch及ETag表示已被缓存，但每次都会发请求询问是否更新
+ 静态资源通过 service worker 进行缓存控制和离线化加载

## 安全相关

### XSS

> 跨网站指令码(Cross-site scripting)，属于代码注入的一种，允许恶意者将代码注入到网页上使其他用户受到影响
>
> 恶意攻击者往 Web 页面里插入恶意 Script 代码，当用户浏览该页之时，嵌入其中 Web 里面的 Script 代码会被执行，从而达到恶意攻击用户的目的。

分类有

反射型: 发送请求时，XSS代码出现在URL中，提交服务端。服务端返回的内容也带上了这段代码，最后浏览器执行代码

`http://www.hasxss.com?x=<script>alert(document.cookie)</script>`

存储型:提交的XSS代码会存储在服务器端。

DOM-based:DOM XSS不需要服务端参与，可以认为是前端代码漏洞导致。比如 eval

防御手段：

1. 过滤转义输入输出
2. 避免使用 eval/new Function等执行字符串的方法
3. 对于 cookie，标识为 http only。js 就无法获取，提高安全性
4. 使用 innerHTML，document.write时对关键字符进行过滤转义



### CSRF攻击

> 跨站请求伪造，挟制用户在当前已登录的 Web 应用程序上执行非本意的操作的攻击方法。XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。
>
> CSRF 攻击是攻击者借助受害者的 Cookie 骗取服务器的信任，可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击服务器，从而在并未授权的情况下执行在权限保护之下的操作。

比如`<img src=“www.weibo.com/attention?userid=123” />`

防御方式：

1. 检查 http referer是否是同域名，通常用户提交请求时 referer 应该是来自站内地址，所以若有异常，可能遭到攻击
2. 避免登陆的 session 长时间存储在客户端
3. 关键请求使用验证码或 token 机制
4. 阻止第三方请求接口
5. 不让第三方网站访问到用户 cookie



### HTTP劫持

运营商在我们访问页面时，在页面的 HTML 代码中插入弹窗、广告等HTML代码以获取相应利益

解决办法：使用HTTPS



总的来说需要运用防护手段来提高安全性

1. HTTP响应头添加部分字段：

   X-Frame-Options	禁止页面被加载仅 iframe 中

   X-XSS-Protection	对于反射型XSS进行一些防御

   X-Content-Security-Policy	可开启内容安全策略(CSP)

   内容安全策略 (CSP) 是一个额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本 (XSS) 和数据注入攻击等。

2. 使用HTTPS，使用HTTP only的 cookie，cookie 的 secure 字段设置为 true

3. get 与 post 请求严格遵守规范使用



###iframe

iframe 中内容往往由第三方来提供，不受我们控制。

可以在 iframe 标签加 sandbox



###不安全的第三方依赖包



## JWT

JSON Web Token，一种跨域认证解决方案

常见的用户认证流程：

1、用户向服务器发送用户名和密码。

2、服务器验证通过后，在当前对话（session）里面保存相关数据，比如用户角色、登录时间等等。

3、服务器向用户返回一个 session_id，写入用户的 Cookie。

4、用户随后的每一次请求，都会通过 Cookie，将 session_id 传回服务器。

5、服务器收到 session_id，找到前期保存的数据，由此得知用户的身份。

这种方案扩展性不好，对于服务器集群，或是跨域的服务导向架构，要求 session 数据共享，这需要 session 数据持久化，写入数据库或别的持久层，然后服务向持久层请求数据，架构虽然清晰，但工程量较大。

因此还有另一种方案，服务器不保存 session 数据，将数据保存在客户端，每次请求都发挥服务器。JWT 即是如此



**JWT 原理**：服务器认证后，生成一个 JSON 对象，发回给客户。以后用户给服务端通信时都发回这个 JSON 对象，为防止篡改数据，服务器在生成对象的时候，会加上签名。



**JWT数据结构**

他是一个很长的字符串，中间用`.`分隔成三个部分，字符串内部是没有换行的。三部分依次为 Header(头部)、Payload(负载)、Signature(签名)。`Header.Payload.Signature`

Header:描述 JWT 元数据的 JSon 对象

```js
{
  "alg": "HS256",	//签名的算法
  "typ": "JWT"	//令牌 token 的类型
}
```

Payload:存放实际需要传递的数据，JSon 对象。

官方字段有

- iss (issuer)：签发人
- exp (expiration time)：过期时间
- sub (subject)：主题
- aud (audience)：受众
- nbf (Not Before)：生效时间
- iat (Issued At)：签发时间
- jti (JWT ID)：编号

由于 JWT 默认不加密，因此避免将秘密信息放在这部分

signature：指定只有服务器知道的密钥，通过 header 里指定的签名算法产生签名



header 和 payload 的 JSon 对象都是通过 Base64URL算法(除了 Base64 算法类似，还有对 URL 有特殊含义的`+`、`/`和`=`三个符号进行替换为`-` 、`_` 和省略)转成字符串



**使用方式**

客户端对于受到的 JWT 储存在 cookie 或 localstorage 里，放在 cookie 里自动发送但不能跨域，也可以放在 HTTP 请求的头信息 Authorization 里。或在跨域的时候放在 POST 请求的数据体里。



**特点**

+ JWT 默认不加密，但也可以在生成原始 token 以后再用密钥再加密一次
+ 在不加密的情况下应避免将秘密数据写入 JWT
+ JWT 不仅可以用于认证，也可以用于交换信息，有效使用可以降低服务器查询数据库的次数
+ 本身包含了认证信息，一旦泄露，任何人都可以获得令牌的所有权限，因此为了减少盗用，有效期应设置的短一点。对于比较重要的权限，使用时应该再次对用户进行认证。
+ 为减少盗用，JWT 不应该使用 HTTP 协议明码传输，应使用 HTTPS
+ JWT 最大缺点是由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或更改 token 的权限。一旦 JWT 签发了，在到期之前始终有效，除非服务器部署额外的逻辑



## HTTP2

**HTTP 1.1缺陷**

+ HTTP 1.1很难榨干TCP协议所能提供的所有性能。

+ HTTP 1.1 对网络延迟比较敏感

+ 线头阻塞

  > HTTP pipelining(在等待上一个请求响应的同时，发送下一个请求。或者说把多个HTTP请求放到一个TCP连接中一一发送，而在发送过程中不需要等待服务器对前一个请求的响应；只不过，客户端还是要按照发送请求的顺序来接收响应。) 
  >
  > 他存在许多问题，服务器是要按照顺序处理请求的，如果前一个请求非常耗时，那么后续请求都会受到影响，这就是所谓的线头阻塞（head-of-line blocking）

http2 是一个二进制协议，使用二进制文本传输，可以使成帧的使用更为便捷，容易识别帧的起始和结束，也能更便捷的从帧结构中分离出那部分协议本身的内容

最大的改进有两点，一是支持服务端推送，二是支持 TCP 连接复用。服务端推送是指能够在客户端发送第一个请求到服务端时，提前把一部分内容推送给客户端，放入缓存当中，避免客户端请求顺序带来的并行度不高导致的性能问题。

TCP 连接复用则使用同一个 TCP 连接来传输多个 HTTP 请求，避免了 TCP 连接建立时的三次握手开销，和初建 TCP 连接时传输窗口小的问题



**多路复用**

在HTTP/1中，由于每次请求都要建立连接，经过3次握手4次挥手，会有效率上的问题，对于串行的文件传输请求时间过长以及连接数过多的问题。

HTTP/2的多路复用可以避免，他在一个TCP连接中可以存在多个流，也就是说可以发送多个请求，所有相同域名的请求都可以通过一个TCP连接并发完成，单个连接上可以并行交错的请求和响应