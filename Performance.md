[toc]

# 性能优化

页面加载速度优化的核心包括三点： 减少资源文件的请求数量；减小每个资源文件的大小；提高每个资源的加载速度。

像合并 API，压缩文件，支持 webp图片，资源 cdn 缓存等都是以此为出发点



对于性能问题，首先需要对工作的任务做数据统计，将数据和指标记录下来。比如说 webpack 打包花费时间性能优化前后对比，脚本使用增加数量等

对于数据统计往往可以使用埋点

1. 添加埋点
2. 收集埋点数据信息
3. 展示埋点数据信息



网络、热保护、缓存回收、第三方脚本、解析器阻塞模式、磁盘的读写、IPC jank、插件安装、CPU、硬件和内存限制、L2/L3缓存、RTTS、图像、Web字体加载行为的差异 —— JavaScript 的代价是最大的，web 字体阻塞默认渲染和图片的加载消耗了大量的内存。



**DNS解析**

**缓存**

**预加载**

Preload可以一定程度上降低首屏的加载时间，因为可以将一些不影响首屏但重要的文件延后加载，唯一缺点就是兼容性不好

**预渲染**

**懒加载**(资源延迟加载)

**懒执行**(逻辑延迟加载)

**图片加载优化**

少用图片，用 css 代替

CDN加载，根据屏幕宽度裁剪

小图使用 base64（这并非一种图片格式，而是一种编码方式）

（Base64编码是从二进制到字符的过程，可用于在 HTTP 环境下传递较长的标识信息，他的数据体积通常是原数据的体积4/3。64个字符。包括大小写拉丁字母各26个、数字10个、加号+和斜杠/，共64个字符。此外还有等号=用来作为后缀用途。它利用 6bit 字符来表达原本的 8bit 字符。6 和 8 的最小公倍数是 24，因此用 4 个 base64 字符来表示三个传统的 8bit 字符，因此编码结果会多 1/3的长度，这也解释了编码后的体积会大 1/3）

雪碧图

JPEG/JPG

有损压缩、体积小、加载快、不支持透明

PNG

无损压缩、质量高、体积大、支持透明

SVG

文本文件、体积小、不失真、兼容性好，它相比其他图片种类本质不同：他对图像的处理不是基于像素点，而是基于对图像的形状描述

WebP

除了兼容性，其他都好。

**文件优化**

css放 head，script 放底部，用 defer、async

webworker

服务端开启文件压缩功能

**webpack**

压缩代码

优化图片

根据路由拆分代码，按需加载

文件名添加哈希

Tree-shaking清理构建过程的方法通过只加载生产中实际使用的代码并清除未使用的 import

对于lodash,使用`babel-plugin-lodash`只加载你仅在源码中使用的模块。这可能会为你节省相当多的 JavaScript 负载。



- 合并 js, css
- 用 css sprites
- 压缩文本图片等
- 延迟显示可见区域外内容，懒加载
- 让部分图片按钮等关键信息优先加载
- 精简代码
- ajax 异步更新
- 缓存



可以参考的度量

- 首次有效渲染(主要内容出现在页面所需的时间)
- 重要渲染时间(页面最重要部分渲染完成所需的时间)
- 可交互时间(页面布局稳定，关键页面字体可见，主进程可以足够处理用户的输入即可在 UI 上点击交互)
- 输入响应(接口响应用户操作所需的时间)
- speed index，测量填充页面内容的速度
- 其他



测量实际环境的体验并设定适当的目标。一个好的目标是：第一次有意义的绘制 < 1 s，速度指数 < 1250，在慢速的 3G 网络上的交互 < 5s，对于重复访问，TTI < 2s。优化渲染开始时间和交互时间。

为您的主模板准备关键的 CSS，并将其包含在页面的 `<head>` 中。（你的预算是 14 KB）。对于 CSS/JS，文件大小[不超过 170 KB gzipped](https://link.juejin.im?target=https%3A%2F%2Finfrequently.org%2F2017%2F10%2Fcan-you-afford-it-real-world-web-performance-budgets%2F)（解压后 0.8-1 MB）。

延迟加载尽可能多的脚本，包括您自己的和第三方的脚本——特别是社交媒体按钮、视频播放器和耗时的 JavaScript 脚本。

添加资源提示，使用 `dns-lookup`、`preconnect`、`prefetch` 和 `preload` 加速传输。

分离 web 字体，并以异步方式加载它们（或切换到系统字体）。

优化图像，并在重要页面（例如登录页面）中考虑使用 WebP。

检查 HTTP 缓存头和安全头是否设置正确。

在服务器上启用 Brotli 或 Zopfli 压缩。（如果做不到，不要忘记启用 Gzip 压缩。）

如果 HTTP/2 可用，启用 HPACK 压缩并开启混合内容警告监控。如果您正在运行 LTS，也可以启用 OCSP stapling。

在 service worker 缓存中尽可能多的缓存资产，如字体、样式、JavaScript 和图像。



## Chrome performance

缩略图中一共分为5行，从上到下依次是：

1. FPS，表示每一秒的帧数，用来衡量页面动画的性能指标。fps图中绿色柱状越高表示体验越好。若出现红色长条则表示在该时间端出现长帧，可能影响用户体验。一般保持在 60 是很好的体验，每一帧动画应该要在 16 毫秒内完成，从而达到 60 帧每秒（1秒 ÷ 60 = 16.6 毫秒） —— 最好可以在 10 毫秒完成。因为浏览器需要时间将新框架绘制到屏幕上，你的代码应该在触发 16.6 毫秒以内完成。
2. CPU，表示cpu的使用情况，其中颜色含义和底下的`Summary`模块中相同。从该行中颜色块的跨越时长可以分析哪类事件消耗的时间较长，从而找到性能瓶颈。
3. NET，每一个颜色条表示加载一种文件。蓝色表示html文件、黄色表示js文件、紫色表示样式文件、绿色表示媒体文件、灰色表示其他资源。
4. 缩略图，对应每一时刻页面的显示情况。通过勾选上方 `Screenshots` 来控制显示或隐藏。
5. HEAP，表示堆内存使用情况。可通过勾选上访 `Menory` 来控制显示或隐藏。



## window.performance

```
// 获取 performance 数据
var performance = {  
    // memory 是非标准属性，只在 Chrome 有
    // 财富问题：我有多少内存
    memory: {
        usedJSHeapSize:  16100000, // JS 对象（包括V8引擎内部对象）占用的内存，一定小于 totalJSHeapSize
        totalJSHeapSize: 35100000, // 可使用的内存
        jsHeapSizeLimit: 793000000 // 内存大小限制
    },
 
    //  哲学问题：我从哪里来？
    navigation: {
        redirectCount: 0, // 如果有重定向的话，页面通过几次重定向跳转而来
        type: 0           // 0   即 TYPE_NAVIGATENEXT 正常进入的页面（非刷新、非重定向等）
                          // 1   即 TYPE_RELOAD       通过 window.location.reload() 刷新的页面
                          // 2   即 TYPE_BACK_FORWARD 通过浏览器的前进后退按钮进入的页面（历史记录）
                          // 255 即 TYPE_UNDEFINED    非以上方式进入的页面
    },
 
    timing: {
        // 在同一个浏览器上下文中，前一个网页（与当前页面不一定同域）unload 的时间戳，如果无前一个网页 unload ，则与 fetchStart 值相等
        navigationStart: 1441112691935,
 
        // 前一个网页（与当前页面同域）unload 的时间戳，如果无前一个网页 unload 或者前一个网页与当前页面不同域，则值为 0
        unloadEventStart: 0,
 
        // 和 unloadEventStart 相对应，返回前一个网页 unload 事件绑定的回调函数执行完毕的时间戳
        unloadEventEnd: 0,
 
        // 第一个 HTTP 重定向发生时的时间。有跳转且是同域名内的重定向才算，否则值为 0 
        redirectStart: 0,
 
        // 最后一个 HTTP 重定向完成时的时间。有跳转且是同域名内部的重定向才算，否则值为 0 
        redirectEnd: 0,
 
        // 浏览器准备好使用 HTTP 请求抓取文档的时间，这发生在检查本地缓存之前
        fetchStart: 1441112692155,
 
        // DNS 域名查询开始的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等
        domainLookupStart: 1441112692155,
 
        // DNS 域名查询完成的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等
        domainLookupEnd: 1441112692155,
 
        // HTTP（TCP） 开始建立连接的时间，如果是持久连接，则与 fetchStart 值相等
        // 注意如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接开始的时间
        connectStart: 1441112692155,
 
        // HTTP（TCP） 完成建立连接的时间（完成握手），如果是持久连接，则与 fetchStart 值相等
        // 注意如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接完成的时间
        // 注意这里握手结束，包括安全连接建立完成、SOCKS 授权通过
        connectEnd: 1441112692155,
 
        // HTTPS 连接开始的时间，如果不是安全连接，则值为 0
        secureConnectionStart: 0,
 
        // HTTP 请求读取真实文档开始的时间（完成建立连接），包括从本地读取缓存
        // 连接错误重连时，这里显示的也是新建立连接的时间
        requestStart: 1441112692158,
 
        // HTTP 开始接收响应的时间（获取到第一个字节），包括从本地读取缓存
        responseStart: 1441112692686,
 
        // HTTP 响应全部接收完成的时间（获取到最后一个字节），包括从本地读取缓存
        responseEnd: 1441112692687,
 
        // 开始解析渲染 DOM 树的时间，此时 Document.readyState 变为 loading，并将抛出 readystatechange 相关事件
        domLoading: 1441112692690,
 
        // 完成解析 DOM 树的时间，Document.readyState 变为 interactive，并将抛出 readystatechange 相关事件
        // 注意只是 DOM 树解析完成，这时候并没有开始加载网页内的资源
        domInteractive: 1441112693093,
 
        // DOM 解析完成后，网页内资源加载开始的时间
        // 在 DOMContentLoaded 事件抛出前发生
        domContentLoadedEventStart: 1441112693093,
 
        // DOM 解析完成后，网页内资源加载完成的时间（如 JS 脚本加载执行完毕）
        domContentLoadedEventEnd: 1441112693101,
 
        // DOM 树解析完成，且资源也准备就绪的时间，Document.readyState 变为 complete，并将抛出 readystatechange 相关事件
        domComplete: 1441112693214,
 
        // load 事件发送给文档，也即 load 回调函数开始执行的时间
        // 注意如果没有绑定 load 事件，值为 0
        loadEventStart: 1441112693214,
 
        // load 事件的回调函数执行完毕的时间
        loadEventEnd: 1441112693215
 
        // 字母顺序
        // connectEnd: 1441112692155,
        // connectStart: 1441112692155,
        // domComplete: 1441112693214,
        // domContentLoadedEventEnd: 1441112693101,
        // domContentLoadedEventStart: 1441112693093,
        // domInteractive: 1441112693093,
        // domLoading: 1441112692690,
        // domainLookupEnd: 1441112692155,
        // domainLookupStart: 1441112692155,
        // fetchStart: 1441112692155,
        // loadEventEnd: 1441112693215,
        // loadEventStart: 1441112693214,
        // navigationStart: 1441112691935,
        // redirectEnd: 0,
        // redirectStart: 0,
        // requestStart: 1441112692158,
        // responseEnd: 1441112692687,
        // responseStart: 1441112692686,
        // secureConnectionStart: 0,
        // unloadEventEnd: 0,
        // unloadEventStart: 0
    }
};
// 计算加载时间
function getPerformanceTiming () {  
    var performance = window.performance;
 
    if (!performance) {
        // 当前浏览器不支持
        console.log('你的浏览器不支持 performance 接口');
        return;
    }
 
    var t = performance.timing;
    var times = {};
 
    //【重要】页面加载完成的时间
    //【原因】这几乎代表了用户等待页面可用的时间
    times.loadPage = t.loadEventEnd - t.navigationStart;
 
    //【重要】解析 DOM 树结构的时间
    //【原因】反省下你的 DOM 树嵌套是不是太多了！
    times.domReady = t.domComplete - t.responseEnd;
 
    //【重要】重定向的时间
    //【原因】拒绝重定向！比如，http://example.com/ 就不该写成 http://example.com
    times.redirect = t.redirectEnd - t.redirectStart;
 
    //【重要】DNS 查询时间
    //【原因】DNS 预加载做了么？页面内是不是使用了太多不同的域名导致域名查询的时间太长？
    // 可使用 HTML5 Prefetch 预查询 DNS ，见：[HTML5 prefetch](http://segmentfault.com/a/1190000000633364)            
    times.lookupDomain = t.domainLookupEnd - t.domainLookupStart;
 
    //【重要】读取页面第一个字节的时间
    //【原因】这可以理解为用户拿到你的资源占用的时间，加异地机房了么，加CDN 处理了么？加带宽了么？加 CPU 运算速度了么？
    // TTFB 即 Time To First Byte 的意思
    // 维基百科：https://en.wikipedia.org/wiki/Time_To_First_Byte
    times.ttfb = t.responseStart - t.navigationStart;
 
    //【重要】内容加载完成的时间
    //【原因】页面内容经过 gzip 压缩了么，静态资源 css/js 等压缩了么？
    times.request = t.responseEnd - t.requestStart;
 
    //【重要】执行 onload 回调函数的时间
    //【原因】是否太多不必要的操作都放到 onload 回调函数里执行了，考虑过延迟加载、按需加载的策略么？
    times.loadEvent = t.loadEventEnd - t.loadEventStart;
 
    // DNS 缓存时间
    times.appcache = t.domainLookupStart - t.fetchStart;
 
    // 卸载页面的时间
    times.unloadEvent = t.unloadEventEnd - t.unloadEventStart;
 
    // TCP 建立连接完成握手的时间
    times.connect = t.connectEnd - t.connectStart;
 
    return times;
}
```



`performance.getEntries()`获取所有资源请求的时间数据

```
var entry = {  
    // 资源名称，也是资源的绝对路径
    name: "http://cdn.xxx/style.css",
    // 资源类型
    entryType: "resource",
    // 谁发起的请求
    initiatorType: "link", // link 即 <link> 标签
                           // script 即 <script>
                           // redirect 即重定向
    // 加载时间
    duration: 18.13399999809917,
 
    redirectStart: 0,
    redirectEnd: 0,
 
    fetchStart: 424.57699999795295,
 
    domainLookupStart: 0,
    domainLookupEnd: 0,
 
    connectStart: 0,
    connectEnd: 0,
 
    secureConnectionStart: 0,
 
    requestStart: 0,
 
    responseStart: 0,
    responseEnd: 442.7109999960521,
 
    startTime: 424.57699999795295
};
```

