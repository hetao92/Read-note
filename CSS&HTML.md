[TOC]

**注释方法** 
HTML                `<!--  ....   -->` 
javascript          多行   `/*     */`
                    单行   `//`
CSS                 `/*      */`

[HTML预留字符](http://www.w3school.com.cn/tags/html_ref_entities.html)在HTML中需使用字符实体，而不是原字符，比如空格 `&nbsp;`



```
<meta name="viewport" content="width=device-width,initial-scale=1">
//适配移动端
<meta http-equiv="X-UA-Compatile" content="IE=edge">
//用最新版 chrome 或者 ie 渲染
<meta charset="utf-8">
```


基础格式
```
<!DOCTYPE html>
<html>
    <head>
        <mata charset="utf-8">
        <title></title>
        <link rel="stylesheet" href="url">
        <style>
        </style>
    </head>
    <body>
    </body>
</html>
```
`<!DOCTYPE>`DTD为浏览器提供一项信息（声明），即 HTML 是用什么版本编写的。
文档模式
标准模式(严格型)；混杂模式；准标准模式（过渡型(Transitional)；框架型(Frameset)


`<head>` 头部元素：以下标签都可以添加到 head 部分：`<title>`、`<base>`、`<link>`、`<meta>`、`<script>` 以及 `<style>`
meta 元素被用于规定页面的描述、关键词、文档的作者、最后 修改时间以及其他元数据。
属性有 http-equiv ；name；scheme

http-equiv指示服务器在发送实际的文档之前先在要传送给浏览器的 MIME文档头部包含名称/值对。：content-type；expires；refresh；set-cookie
name：	author；description；keywords；generator；revised；others

`<script>`有以下属性：
async：异步执行脚本（只适应于外部脚本文件）
defer:延迟执行，直到页面加载为止。（只适应于外部脚本文件）
src
charset
type



# HTML5

语义化：明白每个标签的用途
1. 更容易被搜索引擎收录。
2. 更容易让屏幕阅读器读出网页内容。

标签中name 属性规定锚（anchor）的名称,将 # 符号和锚名称添加到 URL 的末端，就可以直接链接到这个命名锚了。可以用于跳转到同一页面的不同位置！！！



`<em>test</em>`  <em>斜体</em>
`<strong>test</strong>`  <strong>加粗</strong>

`<q>test</q>`表示短文本引用，不用加双引号
`<blockquote></blockquote>`长文本引用，带有缩进样式

`&nbsp;` 由于html忽略空格回车等，这是用来输入空格的

`<address>联系地址信息</address>`
`<code>code</code>`在网页中显示代码（单行），多行参下
`<pre>语言代码段</pre>`  主要作用:预格式化的文本。被包围在 pre



**块状元素**

> 独占一行；元素的高度、宽度、行高以及顶和底边距都可设置

`<div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>`



**内联(行内)元素**

> 不独占一行；元素的高度、宽度及顶部和底部边距不可设置；内联元素彼此间有间距

`<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>`



**内联块状元素**

> 不独占一行；可以设置宽高

`<img>、<input>`



**表格**(tbody表示表格全部加载完后才会显示，内容大时比较好用)
```css
<table summary="表格简介文本">
    <caption>标题文本</caption>
    <tbody>
        <tr>行
            <th>表头</th> //列
            <th></th> //列
        </tr>
        <tr>
            <td></td>
            <td></td>
        <tr>
    </tbody>
<table>
```
tbody,自然也有thead/tfoot 表格的页眉页脚
table frame属性定义框架显示
`<table frame="above">` 上边框显示
below：下边框  ； hsides：上下边框； vsides：左右边框



**空标签**
`<br />`或者`<br>`换行，前者是xhtml1.0版本写法，相对规范
`<hr />`或`<hr>`段落间添加水平横线
`&nbsp;` 由于html忽略空格回车等，这是用来输入空格的



**label**标签
作用是为鼠标用户改进了可用性。如果你在 label 标签内点击文本，就会触发此控件。就是说，当用户单击选中该label标签时，浏览器就会自动将焦点转到和标签相关的表单控件上
`<label for="控件id名称">`

```css
<label for="male">男</label>
<input type="radio" name="gender" id="male" />
<br />
<label for="female">女</label>
<input type="radio" name="gender" id="female" />
<br />
<label for="email">输入你的邮箱地址</label>
<input type="email" id="email" placeholder="Enter email">
```

##常用语义元素

| 标签    | 含义                         |
| ------- | ---------------------------- |
| header  | 定义文档或节的页眉           |
| nav     | 定义导航链接的容器           |
| section | 定义文档中的节               |
| article | 定义独立的自包含文章         |
| aside   | 定义内容之外的内容(如侧边栏) |
| footer  | 定义文档或节的页脚           |
| details | 定义额外的细节               |
| summary | 定义 details 元素的标题      |
| time    | 时间                         |
| main    | 主内容                       |
| mark    | 强调                         |
|         |                              |

##新的表单元素

| 标签         | 描述                             |
| ------------ | -------------------------------- |
| `<datalist>` | 定义输入控件的预定义选项。       |
| `<keygen>`   | 定义键对生成器字段（用于表单）。 |
| `<output>`   | 定义计算结果。                   |
|              |                                  |

```css
HTML5中新表单元素
<form action="action_page.php">
<input list="browsers">
<datalist id="browsers">
   <option value="Internet Explorer">
   <option value="Firefox">
   <option value="Chrome">
   <option value="Opera">
   <option value="Safari">
</datalist> 
</form>
//<input> 元素的 list 属性必须引用 <datalist> 元素的 id 属性。
//此时input可以自己输入，也可以选择下拉列表内容
```



##新的输入类型

color;date;datetime;datetime-local;email;month;number;range;search;tel;url;week



##新的媒介元素

| 标签       | 描述                                 |
| :--------- | :----------------------------------- |
| `<audio>`  | 定义声音或音乐内容。                 |
| `<embed>`  | 定义外部应用程序的容器（比如插件）。 |
| `<source>` | 定义 `<video>` 和 `<audio>` 的来源。 |
| `<track>`  | 定义 `<video>`和`<audio>` 的轨道。   |
| `<video>`  | 定义视频或影片内容。                 |

`<embed src='' />`
`<object data="movie.swf" height="200" width="200"/>`



## 新图像

`<canvas>`	定义使用 JavaScript 的图像绘制。
`<svg>`     定义使用 SVG 的图像绘制。
SVG 是一种使用 XML 描述 2D 图形的语言。意味着 SVG DOM 中的每个元素都是可用的。
Canvas 通过 JavaScript 来绘制 2D 图形。逐像素进行渲染。不支持事件处理。



## 总结：HTML5新特性

语义化标签如 article、footer、header、nav、section
视频和音频标签 video 和 audio
本地离线存储 localStorage 和 sessionStorage
新增表单特性如新控件 calendar email color 等
用于绘图的 canvas 标签(用于游戏等)

localStorage无时间限制，sessionStorage关闭浏览器后即删除
当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。
因此HTML5新增web worker ，是运行在后台的 JavaScript，不会影响页面的性能，通常用作处理耗时的计算。



```JavaScript
 w=new Worker("demo_workers.js")    //创建对象，引入外部脚本
 //添加事件监听
 w.onmessage=function(event){
    document.getElementById("result").innerHTML=event.data;
 };
 w.terminate();     //终止web worker
```

带控制器的视频/音频标签, 不同浏览器有不同的文件格式要求
所以用 2 个 source 标签指定不同的视频/音频格式

```css
<video width="300" height="200" controls="controls">
    <source src="movie.mp4">
    <source src="movie.ogv">
</video>
<audio id='id-audio-player' controls="controls">
  <source src="audio.ogg">
  <source src="audio.mp3">
</audio >	
/*audio 基本操作如下*/
var a = document.querySelector('#id-audio-player')
a.play()
a.pause()
a.autoplay
a.src
a.volume
a.duration
a.currentTime = 1

```



##响应式布局（responsive web design）
确定需要兼容的设备类型尺寸-制作线框原型并测试-视觉设计前端实现
以可变尺寸传递网页，利用css3的media query媒体查询功能
`@media screen and (min-width: 320px) and (max-width : 479px)`



##HTML5 拖放
DataTransfer 对象：拖拽对象用来传递的媒介，使用一般为Event.dataTransfer。
draggable 属性：就是标签元素要设置draggable=true，否则不会有效果。
dragstart 事件：当拖拽元素开始被拖拽的时候触发的事件，此事件作用在被拖曳元素上
dragenter 事件：当拖曳元素进入目标元素的时候触发的事件，此事件作用在目标元素上
也有dragleave
dragover 事件：拖拽元素在目标元素上移动的时候触发的事件，此事件作用在目标元素上
drop 事件：被拖拽的元素在目标元素上同时鼠标放开触发的事件，此事件作用在目标元素上
dragend 事件：当拖拽完成后触发的事件，此事件作用在被拖曳元素上
Event.preventDefault()方法：阻止默认的些事件方法等执行。在dragover中一定要执行preventDefault()，否则drop事件不会被触发。另外，如果是从其他应用软件或是文件中拖东西进来，尤其是图片的时候，默认的动作是显示这个图片或是相关信息，并不是真的执行drop。此时需要用用document的ondragover事件把它直接干掉。
Event.effectAllowed 属性：就是拖拽的效果。有none;copy;move;link;两两结合;all
类似的有dropEffect

```javascript
 //设置元素可拖放
 <img draggable='true' />
 
 //添加dragstart事件监听器（只能使用DOM，不能用jquery）
 var img = document.querySelector('#dragimg')
 var container = document.querySelector('.container')
 img.addEventListener('dragstart',function(event){
    //拖拽数据
    //所有的拖拽事件都有一个属性——dataTransfer，它包含着拖拽数据。拖拽发生时，数据需与拖拽关联，以标识谁在被拖拽。如，当拖拽文本框中的选中文本时，关联到拖拽的数据即是文本本身。类似，拖拽链接时，拖拽数据即是链接的URL。拖拽数据包含两类信息，类型(type)或者格式(format)或者数据(data)，和数据的值(data value)。
    event.dataTransfer.setData('text/plain',event.target.id)
})
container.addEventListener('dragover',function(event){
    event.preventDefault()
})
    
container.addEventListener('drop',function(event){
    event.preventDefault()
    var data = event.dataTransfer.getData('text/plain')
    var x = $(`#${data}`)[0]
    event.target.appendChild(x)
})
   
```



##canvas

```javascript
//创建Canvas元素
<canvas id="myCanvas" width="200" height="100"></canvas>
//通过 JavaScript 来绘制
var c=document.getElementById("myCanvas");
//取得绘图上下文对象的引用
var cxt=c.getContext("2d"); 
//绘制2D得到CanvasRenderingContext2D对象；绘制3D可用"webgl"代替'2d'
ctx.clearRect(0, 0, 200, 200); //擦除，把该区域变为透明
cxt.fillStyle="#FF0000";    // 设置颜色
cxt.fillRect(0,0,150,75); //绘制矩形
```

##2D上下文

```
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext(contextType)
返回 canvas 上下文
contextType
'2d'
'webgl'
'webgl2'
'bitmaprenderer'
```

2D上下文的基本绘图操作时填充和描边
填充颜色 fillStyle
描边颜色 strokeStyle

**绘制矩形**
fillRect(x,y,width,height)
strokeRect(x,y,width,height)
clearRect()
常用属性：lineWidth线条宽度;lineCap线条末端的形状;lineJoin线条相交的方式

**绘制路径**
开始绘制路径
`context.beginPath()`
游标移动    `moveTo()`
画弧线 `arc()/arcTo()`
直线   ` lineTo()`
矩形   ` rect()`
曲线   ` bezierCurveTo()`
绘制一条连接到路径起点的线条    `closePath()`
填充/描边 设置fillStyle/strokeStyle后再fill()/stroke()
剪切   ` clip()`

确定某点是否在路径上    `context.isPointInPath(x,y)`

**绘制文本**
fillText()
strokeText()

**变化**
rotate(angle) 旋转
scale(x,y)  缩放
translate(x,y)  原点移动
transform() 变化矩阵

**绘制图像**
`context.drawImage(img,x,y,[width,height])`
也可以截取图像的某一部分，多几个参数而已

**设置阴影**
`context.shadowColor`   颜色
`context.shadowOffsetX` x轴阴影偏移量
`context.shadowOffsetY` y轴阴影偏移量
`context.shadowBlur`    模糊像素数
**渐变**
`context.createLinearGradient(x1,y1,x2,y2)`
`context.createRadialGradient(x1,y1,x2,y2)`
**设置模式**
`pattern = context.creatPattern(img,重复方式)`
`context.fillStyle = pattern`



# CSS

1. 内联属性  （不推荐） `<span style='color:red;'>hello</span>`
2. 嵌入式`<style>`标签
3. 外部式`<link>`标签 `<link href="base.css" rel="stylesheet" type="text/css" />`



**样式优先级（从高到低）**
`！important `> 内联样式 > 嵌入式`<style>` > 外部式`<link>`
嵌入式>外部式有一个前提：嵌入式css样式的位置一定在外部式的后面,否则采用就近原则
`!important`要写在分号的前面



**选择器优先级（从高到低）**
！important
内联样式
id选择器  #id{}
class选择器   .class{}
元素选择器   h1{} 、伪类、属性选择

伪对象

继承

通配符



## 选择器

子选择器：选择指定标签元素的**第一代**（所有）子元素,用 >
`.div-container>p{color:red;}`
包含(后代)选择器:选择指定标签元素下的后辈元素,不止第一代。用空格` `
即下例标签内所有span元素
`.div-container span{color:red;}`
伪类选择符，它允许给html不存在的标签（标签的某种状态）设置样式
`a:hover{color:red;}`
分组选择符,为html中多个标签元素设置同一个样式时,用 `h1,span`即可

注意：CSS选择器的匹配是**从右向左**进行的

相比于`#markdown-content-h3`，显然使用`#markdown .content h3`时，浏览器生成渲染树（render-tree）所要花费的时间更多。因为后者需要先找到DOM中的所有`h3`元素，再过滤掉祖先元素不是`.content`的，最后过滤掉`.content`的祖先不是`#markdown`的

>从右向左匹配原因：
>CSS中更多的选择器是不会匹配的，所以在考虑性能问题时，需要考虑的是如何在选择器不匹配时提升效率。从右向左匹配就是为了达成这一目的的，通过这一策略能够使得CSS选择器在不匹配的时候效率更高。



标签选择器

1. id/class选择器

2. 多元素选择器

   E,F	同时匹配 E 或 F 元素

   E F	后代元素选择，匹配所有属于 E 元素后代的 F 元素

   E > F	子元素选择，匹配所有 E 元素的子元素 F

   E + F	毗邻元素选择，匹配所有紧随 E 元素之后的同级元素 F

3. 后代选择器

4. 子元素选择器

5. 毗邻选择器

6. 属性选择器 

   `E[att]/E[att=val]`

   `E[att~=val]`匹配所有att属性具有多个空格分隔的值、其中一个值等于"val"的E元素

   `E[att|=val]`匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以"val"开头的E元素，主要用于lang属性，比如"en"、"en-us"、"en-gb"等等

   `E[att^="val"]`属性 att 值以 val 开头的元素

   `E[att$="val"]`属性 att 值以 val 结尾的元素

   `E[att*="val"]`属性 att 值包含 val 的元素

   

7. 伪类选择器

   | 序号 | 选择器        | 含义                                    |
   | ---- | ------------- | --------------------------------------- |
   | 1.   | E:first-child | 匹配父元素的第一个子元素                |
   | 2    | E:link        | 匹配所有未被点击的链接                  |
   | 3    | E:visited     | 匹配所有已被点击的链接                  |
   | 4    | E:active      | 匹配鼠标已经其上按下、还没有释放的E元素 |
   | 5    | E:hover       | 匹配鼠标悬停其上的E元素                 |
   | 6.   | E:focus       | 匹配获得当前焦点的E元素                 |
   | 7.   | E:lang(c)     | 匹配lang属性等于c的E元素                |

   结构性伪类

   | 序号 | 选择器                | 含义                                                         |
   | ---- | --------------------- | ------------------------------------------------------------ |
   | 8.   | E:root                | 匹配文档的根元素，对于HTML文档，就是HTML元素                 |
   | 9.   | E:nth-child(n)        | 匹配其父元素的第n个子元素，第一个编号为1                     |
   | 10.  | E:nth-last-child(n)   | 匹配其父元素的倒数第n个子元素，第一个编号为1                 |
   | 11.  | E:nth-of-type(n)      | 与:nth-child()作用类似，但是仅匹配使用同种标签的元素         |
   | 12.  | E:nth-last-of-type(n) | 与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素   |
   | 13.  | E:last-child          | 匹配父元素的最后一个子元素，等同于:nth-last-child(1)         |
   | 14.  | E:first-of-type       | 匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1) |
   | 15.  | E:last-of-type        | 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1) |
   | 16.  | E:only-child          | 匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1) |
   | 17.  | E:only-of-type        | 匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1) |
   | 18.  | E:empty               | 匹配一个不包含任何子元素的元素，注意，文本节点也被看作子元素 |

   反选伪类

   `E:not(s)`匹配不符合当前选择器的任何元素

8. 伪元素选择器

| 序号 | 选择器         | 含义                      |
| ---- | -------------- | ------------------------- |
| 1    | E:first-line   | 匹配E元素的第一行         |
| 2    | E:first-letter | 匹配E元素的第一个字母     |
| 3    | E:before       | 在E元素之前插入生成的内容 |
| 4    | E:after        | 在E元素之后插入生成的内容 |



同级元素通用选择器

E ~ F	匹配任何在E元素之后的同级F元素

##元素定位 Position

position: static;fixed;relative;absolute
非static属性均可用top、left来定位
absolute 

> 完全绝对定位：脱离其他标签顺序，相对于其最接近的一个具有定位属性的父包含块进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于浏览器窗口。

fixed 

> 相对窗口定位，不随窗口移动而移动，由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化

relative  

>  相对定位：首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于以前的位置移动,且<strong>偏移前的位置保留不动</strong>页面上的其他元素并不会因该元素的位置变化而受到影响。该元素在正常流中的位置会被保留

sticky

> 元素在页面滚动时如同在正常流中，但当其滚动到相对于视口的某个特定位置时就会固定在屏幕上，如同 fixed 一般。
>
> 盒位置根据正常流计算(这称为正常流动中的位置)，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位。在所有情况下（即便被定位元素为 `table 时`），该元素定位均不对后续元素造成影响。当元素 B 被粘性定位时，后续元素的位置仍按照 B 未定位时的位置来确定。`position: sticky `对 `table` 元素的效果与 `position: relative `相同。

static 正常布局行为

Inherit/initial/unset

隐性改变display类型: 当元素设置`position : absolute`或`float : left/right`时，元素display就转变为inline-block，可以设置宽高了

`margin:0 auto`自动居中属性不能和float、position一起使用，会失效



如果 `top` 和 `bottom` 都被指定（严格来说，这里指定的值不能为 `auto` ），`top` 优先。

如果指定了 `left` 和 `right` ，当 `direction`设置为 `ltr`（水平书写的中文、英语）时 `left` 优先， 当`direction`设置为 `rtl`（阿拉伯语、希伯来语、波斯语由右向左书写）时 `right` 优先。

## 盒模型

padding ：内边距（内填充）
margin： 外边距
border： 边距

padding和margin在设置时，采用上右下左（top,right,bottom,left）
当top = bottom, left = right :padding:10px 20px;
当top = bottom=left = right ： padding:10px;
当left = right ： padding：10px 20px 30px;



Box-sizing: content-box; border-box

元素宽度是否包含边框及内边距



##布局模型：
1、流动模型（Flow） 即普通的状态，块元素自上而下分布，各占一行；内联元素都会在所处的包含元素内从左到右水平分布

2、浮动模型 (Float)
`float:left/right`可以将块元素并排浮动显示，浮动不在普通文档流，导致父容器不能够包含浮动元素。

### 清除浮动

[参考](http://www.cnblogs.com/lianghongjin/p/4741858.html)

1. clear属性：both；left；right

2. 设置width：100% + overflow：hidden
   （当父级元素受到子级元素的浮动影响时可以通过“overflow:hidden;”来清除影响
   邻级元素受到浮动影响时通过“clear:both;”来清除）

3. 利用 css 伪元素 (本质还是 clear)

   ```css
   .clearfix:after {
   	content: " ";
   	height: 0;
   	visibility: hidden;
     display: block;
     clear: both;
   }
   ```



####BFC(会计格式化上下文)

BFC能清理浮动主要运用的是它的布局规则：

1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. BFC的区域不会与float box重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算
7. `display: flow-root`做的唯一的一件事就是去创建一个BFC，因此可以避免其他创建BFC方法带来的问题。

浮动清理利用的主要是第六条规则，只要将父容器触发为BFC，就可以实现包含的效果。

那么触发BFC有哪几种方法？

1. 根元素
2. float属性不为none
3. position为absolute或fixed
4. display为inline-block, table-cell, table-caption, flex, inline-flex
5. overflow不为visible(可以用overflow: auto 或 overflow: hidden)



##伪类

引入伪类和伪元素概念是为了格式化文档树以外的信息。也就是说，伪类和伪元素是用来修饰不在文档树中的部分

###锚伪类
:link | :visited | :hover | :active

伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。



在 CSS 定义中，a:hover 必须位于 a:link 和 a:visited 之后，，a:active 必须位于 a:hover 之后这样才能生效！



 **:first-child 伪类**
 **:lang 伪类**

###伪元素

伪元素用于创建一些不在文档树中的元素，并为其添加样式。

规范中的要求使用双冒号(::)表示伪元素



**:first-line** 向文本的首行设置特殊样式，只能用于块级元素。
**:first-letter** 向文本的首字母设置特殊样式
**:before | :after**

在元素的内容前面插入新内容。内容

```css
h1:before {
  content:url(logo.gif);
}
```



Text-transform:capitalize 首字母大写

## 居中对齐

**块状元素**

**定宽块状元素**

`margin:0 auto`

**不定状块状元素**

1.在要设置居中元素外面加入 table标签，利用其长度自适应性，可看做定宽块状元素
2.设置 display: inline 方法：与第一种类似，显示类型设为 行内元素，进行不定宽元素的属性设置
3.设置 position:relative 和 left:50%：利用 相对定位 的方式，通过给父元素设置 `float：left,position:relative,left:50%`，子元素设置 `position:relative , left: -50%` 来实现水平居中。



**水平垂直居中**

1. `lineheight`

```css
.parent{
    width: 100px;
    height: 100px;
    line-height: 100px;
    text-align: center;
}
.content {
    display:inline-block;
    vertical-align: middle;
    line-height:initial;
}
```
2. `absolute` + `负margin`(已知父元素宽高)

```css
.parent{
    position: relative;
    width: 100px;
    height: 100px;
}
.content{
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -50px;
    margin-top: -50px;
}
```
3. `absolute` + `margin auto`(已知父元素宽高)

```css
.parent{
    position: relative;
    width: 100px;
    height: 100px;
}
.content{
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```
4. `absolute` + `calc`(已知父元素宽高)

```css
.parent{
    position: relative;
    width: 300px;
    height: 300px;
}
.content{
    position: absolute;
    top: calc(50%-50px);
    left: calc(50%-50px);
    width: 100px;
    height: 100px;
}
```
5. `absolute` + `transform`

```css
.content{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```
6. `table`
7. `css-table`

```css
.parent{
    display: table-cell;
    text-align: center;
    vertical-align: middle;
    width: 300px;
    height: 300px;
}
.content{
    display: inline-block;
}
```
8. `flex`

```css
    display: flex;
    justify-content: center;
    align-items: center;
```
9. `writing-mode`

```css
.parent{
    writing-mode: vertical-lr;
    text-align: center;
}
.content{
    writing-mode: horizontal-tb;
    display: inline-block;
    text-align: center;
    width: 100%;
}
```

10. `grid`

```css
.parent{
    display:grid;
}
.content{
    align-self: center;
    justify-self: center;
}
```

套路

```css
.vertical-center{
    position: absolute;
    top: 50%;
    transform:     translateY(-50%);（向上自身高度的一半）
}
```



## Flex布局

[参考](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。



容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
子项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。



父元素-容器的属性
1.  flex-direction属性
  flex-direction: row | row-reverse | column | column-reverse;

2.  flex-wrap属性
  定义如果一条轴线排不下，如何换行
  nowrap 不换行| wrap 换行，第一行在上方| wrap-reverse 换行，第一行在下方;

3.  flex-flow
  是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap

4.  justify-content
  定义了项目在主轴上的对齐方式。
    flex-start 左对齐
    flex-end 右对齐
    center 居中
    space-between 两端对齐，项目之间的间隔都相等。
    space-around 每个项目两侧的间隔相等;

5.  align-items
  定义项目在交叉轴(y轴)上如何对齐。
  flex-start  交叉轴的起点对齐。
  flex-end    交叉轴的终点对齐。
  center  交叉轴的中点对齐。
  baseline    项目的第一行文字的基线对齐。
  stretch 如果项目未设置高度或设为auto，将占满整个容器的高度。

6.  align-content
  定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
  flex-start | flex-end | center | space-between | space-around | stretch;



子元素属性
1.  order
  定义项目的排列顺序。数值越小，排列越靠前，默认为0。

2.  flex-grow
  定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

3.  flex-shrink
  定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

4.  flex-basis
  定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

5.  flex
  flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto

6.  align-self
  align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。



## 可继承属性

1. 字体系列属性

   font、font-family、font-weight、font-size、font-style、font-variant

2. 文本系列属性

   Text-indent、text-align、text-shadow、line-height、word-spacing、letter-spacing、

   Text-transform、direction、color

3. 元素可见性: visibility

4. 表格布局属性

   caption-size(表格标题位置)、border-collapse(合并边框)、empty-cells(是否显示表格中的空单元格)

5. 列表属性

   List-style-type(列表项标记的类型)、list-style-image(列表标记用图片代替)、list-style-position、list-style

6. 设置嵌套引用的引号类型： quotes

7. 光标属性：cursor

8. 其他 speak、page 等



所有元素可以继承的属性：

1、元素可见性：visibility

2、光标属性：cursor

内联元素可以继承的属性:

1、字体系列属性

2、除text-indent、text-align之外的文本系列属性

块级元素可以继承的属性:

text-indent、text-align

无继承的属性

1、display

2、文本属性：vertical-align、text-decoration

3、盒子模型的属性:宽度、高度、内外边距、边框等

4、背景属性：背景图片、颜色、位置等

5、定位属性：浮动、清除浮动、定位position等

6、生成内容属性:content、counter-reset、counter-increment

7、轮廓样式属性:outline-style、outline-width、outline-color、outline

8、页面样式属性:size、page-break-before、page-break-after

继承中比较特殊的几点

1、a 标签的字体颜色不能被继承

1、<h1>-<h6>标签字体的大下也是不能被继承的

因为它们都有一个默认值



inherit 关键字可用于任何 HTML 元素上的任何 CSS 属性。

initial:用来设置css属性值为它的默认值，也就是浏览器默认设置的css属性值。

unset:一个属性定义了unset值，如果该属性是默认继承属性，该值等同于inherit，如果该属性是非继承属性，该值等同于initial



## 文本省略

text-overflow: ellipsis;

1. 块级元素
2. overflow: hidden; // 也有说非visible
3. width: 具体值，非auto
4. white-space: nowrap;
5. overflow确实是`非visible`，但是，是计算值，并不是设定值。因为CSS里有个叫`inherit`的关键字。
6. 元素宽度，而不是width，比如，max-width，以及厉害的flex布局也是可以的。
7. white-space: pre; 也是可以的，这个属性的设定主要是为了`不折行`，`pre`也是可以达到目的。

```css
/*底部 padding必须为 0 ，否则多余字会露出来*/
text-overflow: ellipsis;
overflow: hidden;
white-space: pre-wrap;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
```



## 总结：CSS3新特性

**选择器**
**框模型：**
border-radius   --  圆角边框
box-shadow  --  添加阴影（文本有text-shadow）
border-image  --    使用图片来绘制边框

**背景和边框:**
background-size --  背景图片的尺寸
background-origin   --  背景图片的定位区域(content-box、padding-box 或 border-box )

**文本效果:**
text-shadow
text-outline

@font-face{}    -- 定义自己的字体

###断句

[参考](http://www.cnblogs.com/2050/archive/2012/08/10/2632256.html)

word-wrap:break-word;(单词拆分)

> word-wrap 属性用来标明是否允许浏览器在单词内进行断句，这是为了防止当一个字符串太长而找不到它的自然断句点时产生溢出现象。

word-break：break-all

>  word-break 属性用来标明怎么样进行单词内的断句。





**2D/3D 转换**
2D：transform：方法
translate(x,y)  水平移动x，垂直移动y
也有translateX，translateY
rotate(xdeg)    顺时针旋转x度
scale(x,y)  宽度为原来的x倍，高度为原来的y倍 
也有scaleX，scaleY
skew(xdeg,ydeg)     围绕X轴旋转x度，围绕Y轴旋转y度  
matrix() 六个参数，改变旋转、缩放、移动以及倾斜元素。
3D：
rotate3d(x,y,z,angle)   以下均有rotateX()同类
translate3d(x,y,z)
scale3d(x,y,z)



**动画/过渡**

过渡：改变的属性+时间
transition:  width 2s, height 2s, transform 2s;
具体属性：

transition-property 属性的名称
transition-duration 过渡完成时长(必须用，默认0则无效果)
transition-timing-function  效果的速度曲线（比如匀速等）
transition-delay    效果何时开始
例：transition: width 1s linear 2s;

transition局限性
（1）transition需要事件触发，所以没法在网页加载时自动发生。
（2）transition是一次性的，不能重复发生，除非一再触发。
（3）transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。
（4）一条transition规则，只能定义一个属性的变化，不能涉及多个属性。
因此有了animation
动画：
建立@keyframes 规则
```css
@keyframes name{
    //用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。
    from {background: red;}
    50% { background: orange }
    to {background: yellow;}
    //浏览器会自动计算中间状态
    0%   {background: red;}
    25%  {background: yellow;}
    50%  {background: blue;}
    100% {background: green;}
}
div{
    animation:name time;
    /*animation-name
    animation-duration
    animation-timing-function
    animation-delay
    animation-iteration-count 动画播放次数 0/infinite/num
    animation-direction   是否在下一周期逆向地播放
    animation-play-state  是否正在运行或暂停
    animation-fill-mode   播放之前或之后，动画效果是否可见*/
}
```



`window.requestAnimationFrame()`告诉浏览器某个JavaScript代码要执行动画，浏览器收到通知后，则会运行这些代码的时候进行优化，实现流畅的效果，而不再需要开发人员烦心刷新频率的问题了。

而 settimeout 实现是在队列完成后再执行回调，这个时间间隔不一定可靠

**多列布局：**
column-count   -- 规定元素应该被分隔的列数
column-gap  -- 规定列之间的间隔
column-rule --设置列之间的宽度、样式和颜色规则
column-rule-style 属性指定了列与列间的边框样式
column-rule-width 属性指定了两列的边框厚度
column-rule-color 属性指定了两列的边框颜色
column-span	指定元素要跨越多少列
column-width	指定列的宽度



**用户界面：**
resize  -- 规定是否可由用户调整元素尺寸
box-sizing  --以确切的方式定义适应某个区域的具体内容
outline-offset  --对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓（配合outline使用）
轮廓与边框有两点不同：
轮廓不占用空间
轮廓可能是非矩形

表单：
选择框（`<select>`/`<option>`）的change事件与其他表单input字段的change事件触发条件不一样，前者只要选中了选项就会触发，后者会在值被修改且焦点离开当前字段时触发

DOM的appendChild()方法可以将一个选项框中的选项从父节点中移除，再移动到第二个选项框中



**渐变**

渐变 gradient

线性 linear

径向 radial

### 常用方法

`font-weight:bold` 粗体
`font-style:italic` 斜体
`text-decoration:underline/line-through` 下划线/删除线
`p{text-indent:2em;}` 段落排版时段前缩进 2em即文字的2倍大小
`h1{letter-spacing:50px;word-spacing:50px;word-spacing:50px;}`中文字间隔、字母间隔设置/单词间距设置



`border-style（边框样式）：dashed（虚线）| dotted（点线）| solid（实线）`

`color:#336699 >>>color:#369}`
颜色的css样式也是可以缩写的，当你设置的颜色是16进制的色彩值时，如果每两位的值相同，可以缩写一半。



`text-transform : capitalize;` /*首字大写*/ 
`text-transform : uppercase; `/*英文大写*/ 
`text-transform : lowercase; `/*英文小写*/ 



## 移动端

###分辨率、像素

[参考](https://github.com/jawil/blog/issues/21)

**分辨率**：手机屏幕的实际像素尺寸，这与物理尺寸无关，不是成比例的。

**设备像素（物理像素）**：设备上真实的物理单元

+ **PPI像素密度**（pixels per inch）即每英寸包括的像素点数量。可以用于描述屏幕的清晰度以及一张图片的质量。PPI 越高，图片质量越高或者屏幕越清晰。

+ **DPI ** (Dot Per Inch): 即每英寸包括的点数，点是一个抽象的单位，它可以是屏幕像素点、图片像素点也可以是打印机的墨点。若描述图片和屏幕，则和 PPI 等价



**DIP/DP 设备独立像素** (Device Independent Pixels)，也可以叫逻辑像素

照理来说，相同大小的图片文字，若屏幕分辨率越高，则它在屏幕上会越来越小。因此需要一个单位来告诉不同分辨率的设备在界面上显示元素的大小是多少。ios 尺寸单位是 pt，Android 单位是 dp



web 端写 css 时，单位 px 即 css 像素，当页面缩放比例是 100%时，一个 css 像素等于一个设备独立像素。但当用户对浏览器放大时，css 像素也会被放大，跨越更多的物理像素。页面的缩放系数=CSS像素/设备独立像素

如 iPhone5使用 retina 屏幕，使用`2px x 2px`的 device pixel 代表`1px x 1px`的 css pixel，所以设备像素为640 x 1136px 在 css 逻辑像素数为320 x 568px

在 Chrome 开发者工具上，比如 `iPhone X`显示的尺寸是 `375x812`，实际 `iPhone X`的分辨率会比这高很多，这里显示的就是设备独立像素。



**drp 设备像素比** (device pixel ratio): 即物理像素和设备独立像素的比值。

实际像素/倍率 = 逻辑像素尺寸



屏幕可以用 K 和 P 单位来形容屏幕

P 代表屏幕纵向的像素个数，1080P 即纵向上有 1080 个元素

K 代表屏幕横向有几个 1024 个像素，2K 即横向像素超过 2048



**css px是图像显示的基本单元，既不是一个确定的物理量，也不是一个点或者小方块，而是一个抽象概念**。

不同的设备，图像基本采样单元是不同的，显示器上的物理像素等于显示器的点距，而打印机的物理像素等于打印机的墨点。而衡量点距大小和打印机墨点大小的单位分别称为`ppi`和`dpi`：


安卓根据屏幕像素密度分为 `ldpi/mdpi/hdpi/xhdpi`等，以160px 为基准

`window.devicePixelRatio`可以查询设备像素比，即倍率（DPPR）
```css
//兼容性好的媒体查询写法

@media (min-resolution: 192dpi), (-webkit-min-device-pixel-ratio: 2), (min--moz-device-pixel-ratio: 2), (-o-min-device-pixel-ratio: 2/1), (min-device-pixel-ratio: 2), (min-resolution: 2dppx) {
**All your high resolution styles go here**

}
```



移动端中的 1px 问题：

为了适配各种屏幕，我们写代码时一般使用设备独立像素来对页面进行布局。

而在设备像素比大于 `1`的屏幕上，我们写的 `1px`实际上是被多个物理像素渲染，这就会出现 `1px`在有些屏幕上看起来很粗的现象。

1.可以使用基于 media 判断不同设备像素比设定 `border-image`或`background-image`

```css
.border_1px {
	border-bottom: 1px solid #000;      
}
@media only screen and (-webkit-min-device-pixel-ratio:2) {
  .border_1px {
 		border-bottom: none;
    border-width: 0 0 1px 0;
    border-image: url(../img/1pxline.png) 0 0 2 0 stretch;
	}
  .border_1px {
 		background: url(../img/1pxline.png ) repeat-x left bottom;
	}
}
```

2.基于 `media`查询判断不同的设备像素比对线条进行缩放：

```css
.border_1px:before {
  content: '';
  position: absolute;
  top: 0;
  height: 1px;
  width: 100%;
  background-color: #000;
  transform-origin: 50% 0%;
}
@media only screen and (-webkit-min-device-pixel-ratio:2) {
  .border_1px:before { 
  	transform: scaleY(0.5);
  }
}
@media only screen and (-webkit-min-device-pixel-ratio:3) {
	.border_1px:before { 
  	transform: scaleY(0.33);
  }
}
```



3.svg,借助 `PostCSS`的 `postcss-write-svg`我们能直接使用 `border-image`和 `background-image`创建 `svg`的 `1px`边框：



### 视口

> 当前可见的计算机图形区域，在浏览器中不包括浏览器的 UI、菜单栏等。通常包括布局视口、视觉视口和理想视口

**布局视口**是网页布局的基准窗口，在 `PC`浏览器上，布局视口就等于当前浏览器的窗口大小（不包括 `borders` 、 `margins`、滚动条）。在移动端，布局视口被赋予一个默认值，大部分为 `980px`，这保证 `PC`的网页可以在手机浏览器上呈现，但是非常小，用户可以手动对网页进行放大。

通过调用 `document.documentElement.clientWidth/clientHeight`来获取布局视口大小。



**视觉视口**( `visual viewport`)：用户通过屏幕真实看到的区域。默认等于当前浏览器的窗口大小（包括滚动条宽度）。

可以通过调用 `window.innerWidth/innerHeight`来获取视觉视口大小。



**理想视口**( `ideal viewport`)：网站页面在移动端展示的理想大小。单位正是设备独立像素。`页面的缩放系数=CSS像素/设备独立像素`，实际上说 `页面的缩放系数=理想视口宽度/视觉视口宽度`更为准确。

可以通过调用 `screen.width/height`来获取理想视口大小。



在移动端 meta 中定义 viewpoint 时必须让布局视口、视觉视口都尽可能等于理想视口。

`device-width`就等于理想视口的宽度，所以设置 `width=device-width`就相当于让布局视口等于理想视口。

由于 `initial-scale=理想视口宽度/视觉视口宽度`，所以我们设置 `initial-scale=1;`就相当于让视觉视口等于理想视口。



获取浏览器大小

- `window.innerHeight`：获取浏览器视觉视口高度（包括内容、边框以及滚动条）。

- `window.outerHeight`：获取浏览器窗口外部的高度。表示整个浏览器窗口的高度，包括侧边栏、窗口镶边和调正窗口大小的边框。

- `window.screen.Height`：获取获屏幕取理想视口高度，这个数值是固定的， `设备的分辨率/设备像素比`

- `window.screen.availHeight`：浏览器窗口可用的高度。

- `document.documentElement.clientHeight`：获取浏览器布局视口高度，包括内边距，但不包括垂直滚动条、边框和外边距。

- `document.documentElement.offsetHeight`：包括内边距、滚动条、边框和外边距。

- `document.documentElement.scrollHeight`：在不使用滚动条的情况下适合视口中的所有内容所需的最小宽度。测量方式与 `clientHeight`相同：它包含元素的内边距，但不包括边框，外边距或垂直滚动条。

  

  `documentElement`是文档根元素，就是`<html>`标签

  在不支持`window.innerHeight`的浏览器中，可以读取`documentElement`和`body`的高度

  

  ```js
  var height = window.innerHeight
      || document.documentElement.clientHeight
      || document.body.clientHeight;
  //通常这样兼容大多浏览器
  //documentElement.clientHeight不包括整个文档的滚动条，但包括<html>元素的边框。
  body.clientHeight不包括整个文档的滚动条，也不包括<html>元素的边框，也不包括<body>的边框和滚动条。
  ```

  

  

  ![img](http://imweb-io-1251594266.file.myqcloud.com/FmoyfwVMoVKbeRfuHAghzFPImwr5)

![img](http://imweb-io-1251594266.file.myqcloud.com/Fi-h0tWFq6Nz79kgOdc4t3f0y5js)



### 移动端适配方案

- flexible 方案

- vh、vw 方案

  > 将视觉视口宽度 `window.innerWidth`和视觉视口高度 `window.innerHeight` 等分为 100 份。
  >
  > `vw(Viewport's width)`： `1vw`等于视觉视口的 `1%`
  >
  > `vh(Viewport's height)` : `1vh` 为视觉视口高度的 `1%`



## CSS hack

不同浏览器对 css 支持解析不同，为获得统一的页面效果，针对不同浏览器写特定的样式

优雅降级：一开始构建完整功能，再针对低版本浏览器进行兼容

渐进增强：针对低版本先构建页面，再针对高级浏览器追加效果



**条件注释法**

`<!-- [if IE]> content <![endif -->`

`[if IE 6]`	`[if !IE8]` IE8不生效

`[if gte IE6]` `<![endif]—>`	只在IE6以上版本生效



**属性前缀法**

*color

+color

-color

_color

color: red \0

\9 : IE6 - IE10都支持

-和_：仅IE6支持

*：IE6 IE7支持

\0：IE8 IE9支持

\9\0：IE8部分支持，IE9支持





**选择器前缀法**

*html  仅IE6

*+html 仅IE7

@media screen\9{...}只对IE6/7生效
@media \0screen {body { background: red; }}只对IE8有效
@media \0screen\,screen\9{body { background: blue; }}只对IE6/7/8有效
@media screen\0 {body { background: green; }} 只对IE8/9/10有效
@media screen and (min-width:0\0) {body { background: gray; }} 只对IE9/10有效

**link和@import区别**

1. link 是 XHTML标签，除了加载 css 外，还可以定义其他事务

@import 只能加载 css

2. link 在页面载入时同时加载，@import 需页面网页完全载入后再加载
3. link 是XHTML标签，无兼容问题，@import 低版本不支持
4. link 支持 JavaScript 控制DOM改变样式，@import 不支持



## CSS布局

**正常文档流**

**浮动**

​	[参考](https://juejin.im/post/5b3b56a1e51d4519646204bb)

**定位**

**弹性布局**

一种为一维布局而设计的布局方法。一维的意思是你希望内容是按行或者列来布局。

**网络布局**

一种用来进行二维布局的技术。二维（two-dimesional）意味着你希望按照行和列来排布你的内容。

##浏览器渲染机制

1. 处理 HTML 并构建 DOM 树。

   + 浏览器从磁盘或网络读取HTML的原始字节，并根据文件的指定编码（例如 UTF-8）将它们转换成字符串，也就是代码。

   - 将字符串转换成Token，例如：`<html>`、`<body>`等。**Token中会标识出当前Token是“开始标签”或是“结束标签”亦或是“文本”等信息**。
   - 生成节点对象并构建DOM，一边生成Token一边消耗Token来生成节点对象。换句话说，每个Token被生成后，会立刻消耗这个Token创建出节点对象。**注意：带有结束标签标识的Token不会创建节点对象。**

2. 处理 CSS 构建 CSSOM 树（css rule tree,主要是为了完成匹配并把CSS Rule附加上Rendering Tree上的每个Element。）。

   因为样式可以自行设置给某个节点，也可以通过继承获得。在这一过程中，浏览器得递归 CSSOM 树，然后确定具体的元素到底是什么样式。

3. 将 DOM 与 CSSOM 合并成一个渲染树。

   **渲染树只会包括需要显示的节点和这些节点的样式信息**，如果某个节点是 `display: none` 的，那么就不会在渲染树中显示。

4. 根据渲染树来布局（回流），计算每个节点的位置。

5. 调用 GPU 绘制，合成图层，显示在屏幕上。



在构建 CSSOM 树时，会阻塞渲染，直至 CSSOM 树构建完成。并且构建 CSSOM 树是一个十分消耗性能的过程，所以应该尽量保证层级扁平，减少过度层叠，越是具体的 CSS 选择器，执行速度越慢。

当 HTML 解析到 script 标签时，会暂停构建 DOM，完成后才会从暂停的地方重新开始。也就是说，如果你想首屏渲染的越快，就越不应该在首屏就加载 JS 文件。并且 CSS 也会影响 JS 的执行，只有当解析完样式表才会执行 JS，所以也可以认为这种情况下，CSS 也会暂停构建 DOM。

![2019-01-03-0](https://image.fundebug.com/2019-01-03-0.png)

### repaint/reflow

**reflow**

> DOM节点中的各个元素都是以盒模型的形式存在，这些都需要浏览器去计算其位置和大小等，当部分或全部元素的尺寸、结构或某些属性发生改变，浏览器重新渲染的过程称为回流

​	通常操作有：页面首次渲染，浏览器窗口大小改变，元素尺寸或位置改变，内容变化，元素字体大小变化，添加删除**可见**元素，激活 css 伪类等

**repain**

>当盒模型的位置,大小以及其他属性，如颜色,字体,等确定下来之后，且不影响他在文档流中的位置时，浏览器便开始将新样式赋予给元素并绘制内容，这个过程称为重绘

**回流必定会发生重绘，重绘不一定会引发回流**。回流所需的成本比重绘高的多，改变深层次的节点很可能导致父节点的一系列回流。

减少重绘和回流的方法：

+ 使用`translate`代替`top`
+ 使用`visibility`替换`display: none`，前者只会引起重绘，后者引起回流
+ 把DOM离线后修改，比如先`display:none`，再多次修改后显示出来
+ 不要把DOM节点的属性值放在循环里当成变量
+ 避免使用 table 布局，即使很小的改动也会造成整个 table 的重新布局
+ css 选择符从右往左匹配查找，避免DOM深度过深
+ 将频繁运行的动画变成图层，图层能够阻止该节点回流影响别的元素。



由于浏览器使用流式布局，对`Render Tree`的计算通常只需要遍历一次就可以完成，但`table`及其内部元素除外，他们可能需要多次计算，通常要花3倍于同等元素的时间，这也是为什么要避免使用`table`布局的原因之一。



浏览器会维护一个队列，把所有引起回流和重绘的操作放入队列中，如果队列中的任务数量或者时间间隔达到一个阈值的，浏览器就会将队列清空，进行一次批处理，这样可以把多次回流和重绘变成一次。



避免方式：

css：

+ 避免使用table 布局
+ 尽可能在DOM树最末端改变 class
+ 避免设置多层内联样式
+ 将动画效果应用到 position 为 absolute 或fixed 的元素上
+ 避免使用 css 表达式，如 calc()

JavaScript：

+ 避免频繁操作样式，最好一次性重写 style 属性，或直接定义 class 改变 class 属性
+ 避免频繁操作DOM，创建 documentFragment
+ 可以将元素设置`display:none`，操作结束后再显示出来
+ 避免频繁读取会引发回流/重绘的属性
+ 对复杂动画的元素使用绝对定位来脱离文档流，可以避免父元素及后续元素的频繁回流



一些会导致回流、重绘的属性方法

```
clientWidth、clientHeight、clientTop、clientLeft
offsetWidth、offsetHeight、offsetTop、offsetLeft
scrollWidth、scrollHeight、scrollTop、scrollLeft
scrollIntoView()、scrollIntoViewIfNeeded()
getComputedStyle()
getBoundingClientRect()
scrollTo()
```



JavaScript的加载、解析与执行会阻塞DOM的构建。想首屏渲染的越快，就越不应该在首屏就加载 JS 文件，这也是都建议将 script 标签放在 body 标签底部的原因。或者加 defer、async

JS文件不只是阻塞DOM的构建，它会导致CSSOM也阻塞DOM的构建。因为JavaScript不只是可以改DOM，它还可以更改样式，也就是它可以更改CSSOM。



async 属性表示异步执行引入的 JavaScript，与 defer 的区别在于，如果已经加载好，就会开始执行——无论此刻是 HTML 解析阶段还是 DOMContentLoaded 触发之后。需要注意的是，这种方式加载的 JavaScript 依然会阻塞 load 事件。

defer 属性表示延迟执行引入的 JavaScript，即这段 JavaScript 加载时 HTML 并未停止解析，这两个过程是并行的。整个 document 解析完毕且 defer-script 也加载完成之后（这两件事情的顺序无关），会执行所有由 defer-script 加载的 JavaScript 代码，然后触发 DOMContentLoaded 事件。





## others

### z-index

提供z-index栈空间特性并影响子元素渲染顺序的结构，我们称之为stacking context。

- root元素(html)
- 「已定位」元素（position: absolute or relative）且 指定z-index值非auto的元素
- flex item且指定z-index值非auto的元素
- opacity小于1的元素
- 指定transform值非none的元素
- 指定mix-blend-mode值非normal的元素
- 指定filter值非none的元素
- 指定isolation值为isolate的元素
- ==特例 mobile webkit & chrome 22+， 指定position: fixed的元素==
- 在will-change属性上指定值为上述列表任意属性的元素
- 指定-webkit-overflow-scrolling值为touch的元素

特点：

1. stacking context可以嵌套
2. 每个stacking context相对于兄弟元素是完全独立的，其内部规则不会影响到外部
3. 每个stacking context元素都会被父stacking context当做一个元素施加stacking规则

给已定位元素（position: absolute or relative)指定z-index值以改变元素在其parent stacking context中Z轴的「相对偏移」量。这里的「相对偏移」指的是**以parent stacking context为基准**，相对于其它兄弟元素距离用户远近的顺序。

如果一个元素不是通过「定位」(position: absolute or relative)实现了stacking context，它将会以z-index: 0（高于auto）被看待，因此无论如何更改非「定位」元素的z-index都是无效的。



### icon 相关

雪碧图 古老

iconfont 三种形式

​	Unicode 格式 兼容性好，支持调整图标大小颜色，但不支持多色图标。不同设备浏览器字体渲染会略有差别

​	Font-class 兼容性好，相比于unicode语意明确，书写更直观。相比 Unicode 会做一步转化，在 class 里直接写了Unicode，只要引入 class 就好

​	symbol 	svg-icon,支持多色图标，也可以像字体一样调整大小等，兼容性好，支持 ie9+；矢量缩放不失真。因为是用 js 生成 svg，相对不直观，无法按需加载，添加改动不友善



# 浏览器

浏览器包括：

用户界面

浏览器引擎	在用户界面和呈现引擎之间传送指令

呈现引擎	负责显示请求的内容

网络	网络调用

用户界面后端

JavaScript 解释器

数据存储（持久层）

其中 Chrome 每个标签页都是独立的进程，分别对应一个呈现引擎



**流程**

呈现引擎从网络层获取请求文档内容，解析文档，并将各标记逐个转化为内容树上的DOM节点，同时解析外部 css 文件及样式元素中的样式数据，创建“呈现树”

呈现树包含多个包含视觉属性（颜色尺寸等）的矩形。呈现树构建完毕后，进入**布局处理**阶段，为每个节点分配一个应出现在屏幕上的确切坐标。然后**绘制**，呈现引擎遍历呈现树，由用户界面后端层将每个节点绘制出来

呈现器和DOM元素并非一一对应。非可视化的DOM元素(如head)或者 display 为 none 的元素不会插入呈现树中。有些DOM元素对应多个可视化对象(如 select 有按钮区域、下拉列表框区域以及显示区域)

