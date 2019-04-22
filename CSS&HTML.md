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
DataTransfer 对象：退拽对象用来传递的媒介，使用一般为Event.dataTransfer。
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
元素选择器   h1{}



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

id/class选择器

多元素选择器

后代选择器

子元素选择器

毗邻选择器

属性选择器

伪类选择器

伪元素选择器



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

隐性改变display类型: 当元素设置`position : absolute`或`float : left/right`时，元素display就转变为inline-block，可以设置宽高了

`margin:0 auto`自动居中属性不能和float、position一起使用，会失效

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
7. css-table`

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

**分辨率**：手机屏幕的实际像素尺寸，这与物理尺寸无关，不是成比例的。

**像素密度**（pixels per inch）即每英寸长度上排列的像素点数量。

实际像素/倍率 = 逻辑像素尺寸

逻辑像素也就是 css 像素

如 iPhone5使用 retina 屏幕，使用`2px x 2px`的 device pixel 代表`1px x 1px`的 css pixel，所以设备像素为640 x 1136px 在 css 逻辑像素数为320 x 568px

**css px** 就是 css 像素，其显示大小是相对大小而非绝对大小，是与设备相关的。

对于多数 pc 屏幕，因为倍率 devicePixelRatio 通常为1，所以会误以为 css px 即为实际屏幕像素。

而 css `width:100px;height:100px`，假设倍率为3，相当于三个物理像素来描绘一个电子像素，所以会显得模糊，因此做法是把图片换成300x300的，css 设置不变，这时在移动端 css 宽高会被换算为300x300，就不会糊了

逻辑像素尺寸：ios 单位是 `pt`, Android 单位是`dp`，其实是一样的，根据倍率转换为 px

iPhone 逻辑像素尺寸：320x568；375x667；414x736
retina 屏，相应倍率@2x，@3x
安卓逻辑像素趋于统一：360X640
安卓根据屏幕像素密度分为 `ldpi/mdpi/hdpi/xhdpi`等，以160px 为基准

`window.devicePixelRatio`可以查询设备像素比，即倍率（DPPR）
```css
//兼容性好的媒体查询写法

@media (min-resolution: 192dpi), (-webkit-min-device-pixel-ratio: 2), (min--moz-device-pixel-ratio: 2), (-o-min-device-pixel-ratio: 2/1), (min-device-pixel-ratio: 2), (min-resolution: 2dppx) {
**All your high resolution styles go here**

}
```



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
2. 处理 CSS 构建 CSSOM 树。
3. 将 DOM 与 CSSOM 合并成一个渲染树。
4. 根据渲染树来布局，计算每个节点的位置。
5. 调用 GPU 绘制，合成图层，显示在屏幕上。



在构建 CSSOM 树时，会阻塞渲染，直至 CSSOM 树构建完成。并且构建 CSSOM 树是一个十分消耗性能的过程，所以应该尽量保证层级扁平，减少过度层叠，越是具体的 CSS 选择器，执行速度越慢。

当 HTML 解析到 script 标签时，会暂停构建 DOM，完成后才会从暂停的地方重新开始。也就是说，如果你想首屏渲染的越快，就越不应该在首屏就加载 JS 文件。并且 CSS 也会影响 JS 的执行，只有当解析完样式表才会执行 JS，所以也可以认为这种情况下，CSS 也会暂停构建 DOM。



### repaint/reflow

- 重绘是当节点需要更改外观而不会影响布局的，比如改变 `color` 就叫称为重绘
- 回流是布局或者几何属性需要改变就称为回流。

回流必定会发生重绘，重绘不一定会引发回流。回流所需的成本比重绘高的多，改变深层次的节点很可能导致父节点的一系列回流。

减少重绘和回流的方法：

+ 使用`translate`代替`top`
+ 使用`visibility`替换`display: none`，前者只会引起重绘，后者引起回流
+ 把DOM离线后修改，比如先`display:none`，再多次修改后显示出来
+ 不要把DOM节点的属性值放在循环里当成变量
+ 避免使用 table 布局，即使很小的改动也会造成整个 table 的重新布局
+ css 选择符从右往左匹配查找，避免DOM深度过深
+ 将频繁运行的动画变成图层，图层能够阻止该节点回流影响别的元素。



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

