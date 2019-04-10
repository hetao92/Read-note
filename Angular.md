AngularJS 可以构建一个单一页面应用程序（SPAs：Single Page Applications）。
使用双大括号{{}}语法进行数据绑定；
使用DOM控制结构来实现迭代或者隐藏DOM片段；
支持表单和表单的验证；
能将逻辑代码关联到相关的DOM元素上；
能将HTML分组成可重用的组件。

MVC:
View(视图), 即 HTML。
Model(模型), 当前视图中可用的数据。
Controller(控制器),即JavaScript函数，可以添加或修改属性

引入脚本
`<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>`

##Augular指令##

```
<div ng-app="">
     <p>名字 : <input type="text" ng-model="name"></p>
     <h1>Hello {{name}}</h1>
</div>
```

**ng-app**
ng-app指令标记了AngularJS脚本的作用域，在`<html>`中添加ng-app属性即说明整个`<html>`都是AngularJS脚本作用域。开发者也可以在局部使用ng-app指令，如`<div ng-app>`，则AngularJS脚本仅在该`<div>`中运行。

AngularJS将会链接根作用域中的DOM，从用ngApp标记的HTML标签开始，逐步处理DOM中指令和绑定。


**ng-model**
ng-model指令把元素值（比如输入域的值）绑定到应用程序。

ng-model 指令可以为应用数据提供状态值(invalid, dirty, touched, error)

`{{name.$valid}}`输入的值是合法的则为 true
`{{name.$dirty}}`值改变则为 true
`{{name.$touched}}`通过触屏点击则为 true

还可以为HTML元素提供CSS类：
ng-empty
ng-not-empty
ng-touched
ng-untouched
ng-valid
ng-invalid
ng-dirty等

**ng-controller** 
定义了应用程序控制器。

```
<div ng-app="myApp" ng-controller="myCtrl">

名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}

</div>

<script>
//angular.module 函数来创建模块
var app = angular.module('myApp', []);
// ng-controller 指令来添加应用的控制器
app.controller('myCtrl', function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
</script>
```

这个ng-app定义Angular在div内运行，控制器myCtrl定义了函数，用$scope对象来调用控制器，而ng-modle绑定输入域到控制器的属性

也可以外联控制器js文件

**作用域scope**
Scope(作用域) 是应用在 HTML (视图) 和 JavaScript (控制器)之间的纽带。
Scope 是一个对象，是MVC中的模型，有可用的方法和属性。
Scope 可应用在视图和控制器上。
根作用域$rootScope 可作用于整个应用中。是各个 controller 中 scope 的桥梁。

```
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
<p>姓名： <span ng-bind="firstName + ' ' + lastName"></span></p>
</div>
```
**ng-bind**
把应用程序数据绑定到 HTML 视图，比如绑定到某个段落的 innerHTML

**ng-init**
初始化 AngularJS 应用程序变量。

**ng-repeat** 
对于集合中（数组中）的每个项重复一个 HTML 元素：
```
<div ng-app="" ng-init="names=['Jani','Hege','Kai']">
  <p>使用 ng-repeat 来循环数组</p>
  <ul>
    <li ng-repeat="x in names">
      {{ x }}
    </li>
  </ul>
</div>
```

适用于显示表格
```
<table>
<tr ng-repeat="x in names">
<td ng-if="$odd" style="background-color:#f1f1f1">{{ x.Name }}</td>
<td ng-if="$even">{{ x.Name }}</td>
<td ng-if="$odd" style="background-color:#f1f1f1">{{ x.Country }}</td>
<td ng-if="$even">{{ x.Country }}</td>
</tr>
</table>
```
表格显示序号可以在 `<td> `中添加 $index

**ng-include**
 ng-include 指令来包含 HTML 内容，如为跨域文件，需要设置域名访问白名单
```
<body ng-app="">
 
<div ng-include="'runoob.htm'"></div>
 
</body>
```


**自定义指令**

```
<body ng-app="myApp">

<runoob-directive></runoob-directive>

<script>
//通过 AngularJS 的 angular.module 函数来创建模块
var app = angular.module("myApp", []);
app.directive("runoobDirective", function() {
    return {
        template : "<h1>自定义指令!</h1>"
    };
});
</script>

</body>
```
通过以下方式来调用指令：元素名;属性;类名;注释
`<runoob-directive></runoob-directive>`
`<div runoob-directive></div>`
`<div class="runoob-directive"></div>`
`<!-- directive: runoob-directive -->`
添加 restrict 属性来设置指令只能通过某种方式来调用（默认EA  ）:
E 作为元素名使用
A 作为属性使用
C 作为类名使用
M 作为注释使用


**过滤器 |**
currency	格式化数字为货币格式。
filter：''	从数组项中选择一个子集。
lowercase	格式化字符串为小写。
orderBy：''	根据某个表达式排列数组。

uppercase	格式化字符串为大写。
表达式中`{{ lastName | uppercase }}`即表示返回大写数据

**ng-options** 
创建选择框
ng-repeat 指令是通过数组来循环 HTML 代码来创建下拉列表，但 ng-options 指令更适合创建下拉列表，它有以下优势：
使用 ng-options 的选项的一个对象， ng-repeat 是一个字符串。
```
<div ng-app="myApp" ng-controller="myCtrl">

<select ng-model="selectedName" ng-options="x for x in names">
</select>

</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.names = ["Google", "Runoob", "Taobao"];
});
</script>
```


##Service##

** $location服务**
如 ` $location.absUrl()`

** $http 服务**
用于读取web服务器上数据,同样会存在跨域问题
```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    $http.get("welcome.htm").then(function (response) {
        $scope.myWelcome = response.data;
    });
})
```

** $timeout 服务**
对应了 JS `window.setTimeout` 函数

** $interval 服务**
对应了 JS `window.setInterval `函数。

**自定义服务**
```
app.service('hexafy', function() {
    this.myFunc = function (x) {
        return x.toString(16);
    }
})
```
`$dirty`	表单有填写记录
`$valid`	字段内容合法的
`$invalid`	字段内容是非法的
`$pristine`	表单没有填写记录

##DOM属性##
**ng-disabled** 
直接绑定应用程序数据到 HTML 的 disabled 属性。
```
<div ng-app="" ng-init="mySwitch=true">
<p>
<button ng-disabled="mySwitch">点我!</button>
</p>
```

**ng-show/ng-hide** 
隐藏或显示一个 HTML 元素。
`<p ng-show="true">`
` <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>`

##事件##
**ng-click** 
定义了 AngularJS 点击事件。

[##动画##](http://www.runoob.com/angularjs/angularjs-animations.html)
1.引入库
`<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular-animate.min.js"></script>`

2.使用模型ngAnimate
`<body ng-app="ngAnimate">`

主要用于添加移除class

常用API：
`angular.lowercase()`	转换字符串为小写
`angular.uppercase()`	转换字符串为大写
`angular.isString()`	判断给定的对象是否为字符串，如果是返回 true。
`angular.isNumber()`	判断给定的对象是否为数字，如果是返回 true。