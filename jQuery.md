[TOC]

# jQuery

> 一个常用的 js 库, 提供了 DOM 操作, AJAX 封装, 兼容性等功能

`$` 即 `Jquery`
`var jq=jQuery.noConflict()`，帮助您使用自己的名称（比如 jq）来代替 $ 符号。



## 选择器

| 语法                                      | 含义                                      |
| ----------------------------------------- | ----------------------------------------- |
| `s = $('.class')`                         | 返回的是一个列表                          |
| `s.find('.class')/children()`             | 多用于查找子元素，同样返回数组/所有子元素 |
| `siblings`                                | 查找兄弟元素                              |
| `closest/parent/parents`                  | 最近的符合条件的父元素节点/父元素         |
| `next()/prev()`                           | 位于同一层级的节点                        |
| `filter()/first()/last()/eq(index)/not()` | 过滤掉不符合选择器条件的节点              |

还有一些过滤器  :even /:odd/:first/:nth-child(n) 第n个元素（从1开始）
针对表单元素，有:input/:checkbox/:radio/:checked此类



对于选择class等返回的是所有元素，相当于`document.querySelectorAll`



按属性查找
`var email = $('[name=email]')`



对于返回的列表，
`var s =$('#id-div')`返回的列表
`[<h1>hello</h1>,<h1>world</h1>]`
s[0]又是一个DOM元素了，比如`<h1>hello</h1>`
所以对s[0]不能使用jQuery了，只能用DOM方法：`s[0].value`
或者`$(s[0]).val()`



## 事件

对于change事件，用JavaScript代码去改动文本框的值，将不会触发change事件，那么可以在修改后直接调用无参数的change()方法来触发该事件

jQuery能够绑定的事件主要包括：

### 鼠标事件

click: 鼠标单击时触发；
dblclick：鼠标双击时触发；
mouseenter：鼠标进入时触发；
mouseleave：鼠标移出时触发；
mousemove：鼠标在DOM内部移动时触发；
hover：鼠标进入和退出时触发两个函数，相当于mouseenter加上mouseleave。



mouseover与mouseenter
不论鼠标指针穿过被选元素或其子元素，都会触发 mouseover 事件。
只有在鼠标指针穿过被选元素时，才会触发 mouseenter 事件。
mouseout与mouseleave
不论鼠标指针离开被选元素还是任何子元素，都会触发 mouseout 事件。
只有在鼠标指针离开被选元素时，才会触发 mouseleave 事件。



### 键盘事件

键盘事件仅作用在当前焦点的DOM上，通常是<input>和<textarea>。

keydown：键盘按下时触发；
keyup：键盘松开时触发；
keypress：按一次键后触发。



### 其他事件

focus：当DOM获得焦点时触发；
blur：当DOM失去焦点时触发；
change：当<input>、<select>或<textarea>的内容改变时触发；
submit：当<form>提交时触发；
ready：当页面被载入并且DOM树完成初始化后触发。ready仅作用于document对象



一个已被绑定的事件可以解除绑定，通过`off('click',function)`实现，是function不是function () {...}



```javascript
$('body').on('click',function(){
    var button = $(event.target)  //注意在使用jQuery时候将对象转换为jQuery的对象，否则是DOM元素
})

//事件委托 给id-div-todo委托删除按钮delete-button的功能
$('#id-div-todo').on('click','.delete-button',function(event){
    console.log('click')
    var button = $(event.target)
    button.closest('.todo-cell').remove()
})
```



## DOM操作

1. append/prepend  同级节点可以用after()/before()
2. remove （删除元素及内部所有内容）
3. empty（清空元素内的内容，但是元素即某个标签还在）
4. show, hide, toggle （开关）



## Class 操作

1. addClass removeClass
2. toggleClass



## 属性、特性操作

**属性、特性操作**
1. attr, prop, data
2. removeAttr
3. css 



`$().css("propertyname",value)`

`div.attr('name', 'Hello')`

prop()方法和attr()类似，但是HTML5规定有一种属性在DOM节点中可以没有值，只有出现与不出现两种，比如`checked`



###data

data用于自建属性 `<div class='todo-cell' data-属性名（比如id）= "401"` (一般用双引号)

```javascript
//data数据获取方式
dom API 如下
    var domDiv = $('.todo-cell')[0]
    domDiv.dataset.id
    "401"

jQuery API 如下
    var jqdiv = $($('.todo-cell')[0])
    jqdiv.data("id")
```



## 取值

1. val  设置或返回表单字段的值
2. text 设置或返回所选元素的文本内容
3. html 设置或返回所选元素的内容（包括 HTML 标记）



`$('id-input').val()` 相当于DOM里的  XXX.value
text和html区别：text只取文本元素，html取标签内的所有元素，即使里面还有html标签
`<p><h1>hello</h1></h>`
`$('p').text()`  >>>> `hello`
`$('p').html()`  >>>> `<h1>hello</h1>`
同理，
text()和html()括号内可以加文本，区别和上面一样



## 数组方法

1. each
2. map
3. grep
```javascript
var cells = $('.todo-cell')
for (var i = 0; i < cells.length; i++) {
    var c = cells[i]
    console.log('cell', i, c)
}
// jQuery 提供遍历数组的 each 函数
// 对每个元素调用函数, 参数是 index 和 element,可省略
$('.todo-cell').each(function(i, e) {
    console.log(i, e)
})

// map 操作
// 对每个数组中的元素调用函数得到返回值组成新的数组
var foo = [1, 2, 3, 4, 5]
var bar = $.map(foo, function(value){
    return value * value
})

$.map(foo,function(value,key) {
    ...
})
//map中function可以有第二个参数key。当对象是object，value和key就是值和键；当对象是数组时，key是下标（index）

// grep 相当于 filter 函数
var far = $.grep(foo, function(value){
    return value % 2 == 0
})
```



## ajax

async：是否异步执行AJAX请求，默认为true，千万不要指定为false；

type：发送的Method，缺省为'GET'，可指定为'POST'、'PUT'等；

contentType：发送POST请求的格式，默认值为'application/x-www-form-urlencoded; charset=UTF-8'，也可以指定为text/plain、application/json；

data：发送的数据，可以是字符串、数组或object。如果是GET请求，data将被转换成query附加到URL上，如果是POST请求，根据contentType把data序列化成合适的格式；

headers：发送的额外的HTTP头，必须是一个object；

dataType：接收的数据格式，可以指定为'html'、'xml'、'json'、'text'等，缺省情况下根据响应的Content-Type猜测。



```javascript
jQuery AJAX 函数用法
var request = {
    url: '/uploads/tags.json',
    type: 'get',
    contentType: 'application/json',
    success: function() {
        console.log(arguments)
    },
    error: function() {
        console.log(arguments)
    }
}
$.ajax(request)
$.get(url)  //请求数据
$.post(url,data)    //发送数据
```



## 动画

[动画](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434500456006abd6381dc3bb439d932cb895b62d9eee000)
show/ hide/ toggle

slideUp / slideDown 滑动

fadeIn / fadeOut/ fadeToggle(speed,callback)/fadeTo(speed,opacity,callback) 	淡入淡出



## 套路

```javascript
$('document').ready(function(){
    .....
})
```
把script的调用函数放进去，可以等页面全部加载完再调用script
还可以再简化为：`$(function () {...})`

$().animate({属性},speed,callback)  //动画
$(selector).stop(stopAll,goToEnd)  //用于停止动画或效果
//可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。