[TOC]

# Javascript

## 常识

Javascript 包括 ECMAScript、DOM、BOM

API 是应用程序编程接口 (Application Programming Interface) 的缩写。他是包含属性和方法的库，用于操作HTML DOM元素。



命名时变量名区分大小写 ，大小写英文、$ 和 下划线（_）开头，后续可以加数字



`/r` 回车，使光标移到行首

`/n` 换行，使光标下移一行，配合使用即回车换行



![关键字保留字](http://img.mukewang.com/529c07c000014f5103080447.jpg)



对于`return`、`throw` 此类关键字，和值之间不允许有行终止符。否则根据自动分号插入，易出错



`for-in `  循环是循环所有能够通过对象访问的可枚举的属性，无论是实例中的属性还是原型的属性

`a++` ： 把（a++）作为一个整体表达式，a 的值虽然自增1，但是整个表达式的值是取 a自增  之前  的值；
`++a ` : 也把（++a）作为一个整体表达式，a 的值也自增1，但是整个表达式的值是取 a 自增  之后  的值。

```Javascript
var a = 5
b = a++  //b=5 a=6
b = ++a  //b=6 a=6
```



面向对象语言的要求
一种面向对象语言需要向开发者提供四种基本能力：
封装 - 把相关的信息（无论数据或方法）存储在对象中的能力
聚集 - 把一个对象存储在另一个对象内的能力
继承 - 由另一个类（或多个类）得来类的属性和方法的能力
多态 - 编写能以多种方法运行的函数或方法的能力



**计时器**
`setInterval(代码,交互时间)`  时间以毫秒计时
`clearInterval(id_of_setInterval)` id 即 对setInterval 赋值的变量
`setTimeout(代码,延迟时间)` 仅执行一次
`clearTimeout(id_of_setTimeout)`

定时器指定的时间间隔表示何时将定时器的代码添加到队列，而不是何时实际执行代码，如果在那个时间点上队列中没有其他东西，则会被立刻执行



### 变量声明&提升

| 类型     | var           | let        | const      |
| -------- | ------------- | ---------- | ---------- |
| 作用域   | 全局/局部变量 | 块级作用域 | 块级作用域 |
| 变量提升 | 提升          | 不提升     | 不提升     |

1. 声明变量的作用域限制是**其声明位置的上下文**，而非总是全局的
2. 声明变量在任何代码执行前创建，而不是在执行赋值操作的时候才会被创建
3. 声明变量是它所在上下文环境的不可配置属性（non-configurable property）（无法被删除），非声明变量是可配置的（例如非声明变量可以被删除）。 

ES5 只有两种声明变量的方法：`var`命令和`function`命令。ES6 除了添加`let`和`const`命令，还有另外两种声明变量的方法：`import`命令和`class`命令。所以，ES6 一共有 6 种声明变量的方法。



JavaScript编译器会对const进行优化，所以多使用const，有利于提高程序的运行效率，也就是说let和const的本质区别，其实是编译器内部的处理不同。



**变量提升**

+ `var ` 声明会被提升至作用域顶部，但它们的赋值不会提升，`const`  和 `let` 不会提升
+ 匿名函数表达式的变量名会被提升，但函数内容不会
+ 命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会。
+ 函数声明的名称和函数体都会被提升。

```javascript
var x = y, y = 'A';
console.log(x + y); // undefinedA

var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
//内层变量可能会覆盖外层变量。原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。
```



```javascript
function example() {
  console.log(anonymous); // => undefined
	//匿名函数
  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {
    console.log('anonymous function expression');
  };
  //命名函数
  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
    console.log('Flying');
  };
}
```

```JavaScript
//函数声明
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
	}
}
```



##数据类型

1. 数字 Number
2. 字符串 String
3. 布尔值 Boolean
4. 对象 Object
5. null 关键字，不能写Null/NULL
6. undefined
7. Symbol  (ES6增) 实例唯一且不可改变



[null & undefined 区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)

+ `null`  表示"没有对象"，即该处不应该有值。

  > 1.  作为函数的参数，表示该函数的参数不是对象。
  > 2. 作为对象原型链的终点。

+ `undefined` 表示"缺少值"，就是此处应该有一个值，但是还没有定义。

  >1. 变量被声明了，但没有赋值时，就等于undefined。
  >2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
  >3. 对象没有赋值的属性，该属性的值为undefined。
  >4. 函数没有返回值时，默认返回undefined。



###JavaScript 中的假值

1. false
2. null
3. undefined
4. 数字 0
5. 空字符串 ''
6. NaN

当使用 `new Boolean(value)`创建布尔值时，参数为这几个假值，则生成 false。但不要将原始值 true/false 和值为 true/false 的布尔对象混淆。

```javascript
let a = new Boolean('')
let b = false
a == b // true
a === b // false
typeof a  // object
typeof b // boolean
```



### 类型检测 typeof & instanceof

```javascript
typeof 123 // 'number'
typeof NaN // 'number'
typeof Infinity // 'number'
typeof 'str' // 'string'
typeof true // 'boolean'
typeof undefined // 'undefined'
typeof Math.abs // 'function'
typeof null // 'object'
typeof [] // 'object'
typeof {} // 'object'
typeof window.length;// "number"
//JS 最初的实现中，JavaScript 中的值是由一个表示类型的标签和实际数据值表示的。对象的类型标签是0。由于 null 代表的是空指针(大多数平台下值为0x00)，因此，null的类型标签也成为了0，typeof null就错误的返回了"object".
//null的类型是object，Array的类型也是object，如果我们用typeof将无法区分出null、Array和通常意义上的object {}。

//容易混淆
typeof new Boolean(true) === 'object';
typeof new Number(1) ==== 'object';
```

判断 Array 使用 `Array.isArray(arr)`
判断 null 使用 `myVar === null`；



**instanceof**

对于引用类型可以用`instanceof`，但`instanceof` 的一个缺点在于它假定单一的全局执行环境，若存在多个框架，当一个框架向另一个框架传入一个数组，那传入的数组与第二个框架中原生创建的数组具有不同的构造函数。



在检测对象是原生还是自定义的，可以使用`toString()` 方法，可以返回一个`[object NativeConstructorName]`，对于原生数组/函数等会返回`[object Array]/[object Function]`；而对于自定义函数，`toString()`不能检测其构造函数名，会返回`[object object]`



### 数字 Number

JavaScript 中数字是双精度64位二进制格式，不区分整数浮点数

`+0 === -0` 为 `true` 

`NaN === NaN` 为 `false`

前缀为 `0`,则为八进制，0后面的数字大于7的范围内，该数字将会被转换成十进制。ES6改为`0o`

`0x` 则为十六进制

`Math.floor `可以把小数向下取整
`Math.ceil` 可以把小数向上取整
`Math.round `  四舍五入

格式化：
`num.toFixed(x)` 显示x个小数位
`num.toExponential()` 指数表示
`num.toPrecision()`结合上述两个，视情况而定

![img](http://img.mukewang.com/532fe841000174db05160622.jpg)



`isNaN(x)/Number.isNaN(x)`  会尝试将值转换为数字再判断,因此对于被强制转换成功的非数字参数无法判断
`isFinite()/Number.isFinite()`判断是否为有效数字

上述Number方法与全局方法相比，不会强制将非数值参数转换为数值，只有参数是数值且满足条件才会返回 true

`Number.isInteger()`判断是否为整数。JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。均为 true

`Number.isNaN()` 确定传递的值是否为 NaN和其类型是 Number。

`isFinite() ` 和 ` isNaN()` 会先调用`Number()`将非数值的值转为数值再判断

```javascript
Number.isInteger(3.0000000000000002) // true

//JS中数值存储为64位双精度格式，数值精度最多可以达到 53 个二进制位（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃，这种情况下，Number.isInteger可能会误判。这个小数的精度达到了小数点后16个十进制位，转成二进制位超过了53个二进制位，导致最后的那个2被丢弃了
```



`parseInt(string[,radix])`函数可解析一个字符串,并返回一个整数,第二个参数为进制(radix)参数，这个参数用于指定使用哪一种进制。如果不指定，str 以‘0x’或‘0X’开头则基础为16，以‘0’开头则基础为8或者10，即使ECMAscript 5规定用10，具体仍由环境决定。因此需要明确给出 radix 参数值

`parseFloat()`/ `Number(value)` 可用于任何数据类型- 创建新值！把给定的值转换成数字

ES6将全局方法`parseInt/parseFloat`移植到`Number`上，即`Number.parseInt()/Number.parseFloat()`，行为完全不变



**指数运算符** `**`
特点是右结合，而不是常见的左结合。多个指数运算符连用时，是从最右边开始计算的。

```
// 相当于 2 ** (3 ** 2)
2 ** 3 ** 2
// 512
```

####转换规则

**Number()**

true\false -- 1\0
null -- 0
undefined --NaN

空字符串'' — 0

**parseInt()**

第一个为数字会继续解析下一个直到结束或遇到非数字字符
如 '22.5' 会转换为22，因为小数点不是有效的数字字符

**parseFloat()**

在解析过程中遇到正负号、数字、小数点及 e/E 外的字符会忽略该字符及其以后的所有字符，返回当前已经解析的浮点数,第一个字符不能被解析为数字则返回 NaN

parseInt可以用第二个参数解析其他进制，parseFloat只能解析十进制



#### ES6新增部分方法

`Math.trunc()`去除小数部分，返回整数部分。对于非数值会先转为数值，否则返回NaN。相当于

```javascript
Math.trunc = Math.trunc || function(x) {
  return x < 0 ? Math.ceil(x) : Math.floor(x);
};
```

`Math.sign()` 用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

返回值包括：

+ 参数为正数，返回+1
+ 参数为负数，返回-1
+ 参数为 0，返回0
+ 参数为-0，返回-0
+ 其他值，返回NaN



`Math.cbrt()`用于计算一个数的立方根。



`Number.EPSILON`
ES6中新增的极小的常量，表示1与大于1的最小浮点数之间的差（2的 -52 次方），实际是Js 能够表示的最小精度。可以在浮点数计算时候，设置一个误差范围

```javascript
0.1 + 0.2 // 0.30000000000000004
//此时可以将误差范围设为2的-50次方 Number.EPSILON*Math.pow(2,2)
//当两数差小于这个值时即可认为相等

function withinErrorMargin (left, right) {
  return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
}

```



`numObj.toLocaleString([locales [, options]])`
很有意思的功能
返回这个数字在特定语言环境下的表示字符串。
locales：缩写语言代码的字符串或者这些字符串组成的数组.
options：常用的有 style（格式）、currency（货币属性）

```
var number = 123456.789;

// 德国使用逗号作为小数分隔符，分位周期为千位
console.log(number.toLocaleString('de-DE'));
// → 123.456,789

// 在大多数阿拉伯语国家使用阿拉伯语数字
console.log(number.toLocaleString('ar-EG'));
// → ١٢٣٤٥٦٫٧٨٩

// nu 扩展字段要求编号系统，e.g. 中文十进制
console.log(number.toLocaleString('zh-Hans-CN-u-nu-hanidec'));
// → 一二三,四五六.七八九

var number = 123456.789;

// 要求货币格式
console.log(number.toLocaleString('de-DE', { style: 'currency', currency: 'EUR' }));
// → 123.456,79 €

// 日元不是用小数位
console.log(number.toLocaleString('ja-JP', { style: 'currency', currency: 'JPY' }))
// → ￥123,457

// 限制三位有效数字
console.log(number.toLocaleString('en-IN', { maximumSignificantDigits: 3 }));
// → 1,23,000
```

### 数组 Array

`var array = new Array(1,2,3)`
`var array = Array(1,2,3)`
`var array = [1,2,3]`

`var arr=[4]` 和` var arr=new Array(4)`是不等效的，前者是单值数组，后者指数组长度但又不含内容。指定长度必须是整数



在实施层面， JavaScript实际上是将元素作为标准的对象属性来存储，把数组索引作为属性名。



`arr[2]`也可以用`arr['2']`代替，Js引擎会自动调用 toString 转换成一个 string 类型变量，因此注意 `arr['2']`和`arr['02']`会指向不同的对象

**原型方法**

`Array.from(arrayLike[, mapFn[, this]])`依据一个类似数组或可迭代的对象中创建一个新的数组实例。mapFn 为生成数组需经过此函数处理后返回，this为 mapFn的 this 值

`Array.isArray(arr)` 判断arr 是否为数组
`Array.of()`可以将一组值转为数组，返回参数值组成的数组。如果没有参数，就返回一个空数组。

```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array(3) // [, , ,]
```



| 语法                                                     | 描述                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| `array.length`                                           | 数组长度                                                     |
| `array.slice(begin,end)`                                 | 返回浅拷贝的新数组                                           |
| ` array[i]`                                              | 下标访问                                                     |
| `array.push()`                                           | 末尾添加一个或多个元素并返回长度                             |
| `arr.unshift()`                                          | 头部添加一个或多个元素并返回长度                             |
| `array.pop()`                                            | 末尾删除最后一个元素并返回该元素                             |
| `arr.shift()`                                            | 头部删除第一个元素并返回该元素                               |
| `arr.includes(value[,fromIndex])`                        | 判断一个数组是否包含指定的值                                 |
| `arr.sort([fun]) / arr.reverse()`                        | 排序与翻转                                                   |
| `arr.indexof(value[,fromIndex])/lastIndexof()`           | 索引返回索引值,不存在则返回-1                                |
| `arr.concat(...arr)`                                     | 连接两个以上数组返回新数组，不改变现有数组                   |
| `arr.join([separator])`                                  | 连接数组元素形成字符串，不改变数组，separator默认`','`       |
| `arr.splice(index,howmany,items)`                        | 从数组中添加/删除项目并返回被删除的项目（index：添加/删除项目的起始位置；howmany:要删除的项目数量，若为0，则不删除；item1，…… 添加的新项目，可选。返回删除元素形成的数组 |
| `arr.toString()`                                         | 返回一个包含数组中所有元素的字符串，每个元素通过逗号分隔。   |
| `arr.toLocalString()`                                    | 根据区域设置，返回一个包含数组中所有元素各自用`toLocalString`转换的字符串，每个元素通过逗号分隔。 |
| `arr.find(callback[, thisArg])`                          | 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。 |
| `arr.findIndex(callback[, thisArg])`                     | 返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。 |
| `arr.fill(val,startPos,endPos)`                          | 用给定值填充数组中指定的起始位置到结束位置间的位置           |
| `arr.flat(n)`                                            | 将嵌套的数组拉平'n'层<br/>n可以取`Infinity`作为参数表示不管有多少层嵌套都可以拉平
数组中有空位（空位不是 undefined，ubdefined 还是有值的）会被 flat 跳过 |
| `arr.copyWithin(目标索引[, [源开始索引], [结束源索引]])` | 浅复制数组的一部分到同一数组中的另一个位置，并返回它，改变数组但不修改其大小。 |

```javascript
//源开始索引被忽略，则从0开始复制
//结束源索引被忽略，则会复制到arr.length
[1, 2, 3, 4, 5].copyWithin(-2);
// [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3);
// [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4);
// [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1);
// [1, 2, 3, 3, 4]

[].copyWithin.call({length: 5, 3: 1}, 0, 3);
// {0: 1, 3: 1, length: 5}
```



ES5中`forEach()/filter()/reduce()/every()/some()`都会跳过空位，`map()`会跳过但会保留
ES6则将空位转为 undefined



`arr.entries()`是对键值对的遍历
`arr.keys`是对键名的遍历
`arr.values`是对键值的遍历

上述三个方法返回的都是Generator，可以用`for...of`遍历

`for(let el of arr.values())`



**注意事项：**
`Array`实例继承自 `Array.prototype`。而 `Array.prototype` 本身也是一个 `Array`。



`splice()`与`slice()`不同，前者修改数组，后者创建新数组
数组的长度是比数组最大索引值多一的数。并不总是等于数组中元素的个数，因此`length`属性不能真正表示数组中定义的值的数目



`arr.indexOf` **缺点**：内部使用严格相等运算符（===）进行判断，会导致对`NaN`的误判。
`includes`就没有这个问题

```javascript
[NaN].indexOf(NaN)
// -1
[NaN].includes(NaN)
// true
```



在空数组中调用`pop()/shift()`，则返回`undefined`



删除数组元素 `delete arr[index]`数组 length 并不会变小



`array.join()`当某一项为null或undefined时，返回的结果中那项以空字符串显示。



`reverse()`会改变原来的数组，而不会创建新的数组。
`sort`排序不一定是稳定的。默认排序顺序是根据字符串Unicode码点。数字会先被转换为字符串，所以 "80" 比 "9" 要靠前。



下标访问时：如果 i 是非整数，那么 i 将作为数组对象的属性property创建，而不是作为数组的元素



复制数组：
`.slice(0)` 和 `concat ()`
这两种数组复制的操作都不适合对数组中包括复杂数据类型的数据，如果数组中包括复杂数据类型(对象引用)的数据，对这些数据的修改仍然会同时影响复制数组与被复制数组。需要深层克隆函数



#### 遍历迭代

1. `a.forEach(callback,[,thisArg])`

   >对函数每个元素执行函数，无返回值(总是返回 `undefined`)，影响原数组

2. `a.map(callback[,thisArg])`

   > 对函数每个元素执行函数，将所有结果放入新数组并返回

3. `a.filter(callback[, thisArg])`

   > 返回true的项组成的新数组(过滤),不改变原数组

4. `a.every(callback[, thisArg])`

   > 对每个元素执行函数，若每项都返回true，则返回true，否则返回 false, 不改变原数组

5. `a.some(callback[, thisArg])`

   > 若某一项返回 true，就返回true

6. `a.reduce(callback[,initialvalue]) / a.reduceRight()`

   > 将函数元素两两递归处理把数组计算为一个值/反方向，建议加上 initialvalue / 反向

前五个迭代方法的 `callback` 在调用时会依次传入`（value,index,array原数组）`

`a.reduce `的 `callback`传入`(first,currentValue,currentIndex,array)`,如果数组为空，且没有提供 `initialValue` 参数，将会抛出一个 TypeError 错误。如果数组只有一个元素且没有提供 `initialValue `参数，或者提供了 `initialValue `参数，但是数组为空将会直接返回数组中的那一个元素或` initialValue` 参数，而不会调用` callback`。



`forEach`不可链式调用，因为无返回值



通常情况下 `callback`函数只需要接受一个参数，就是正在被遍历的数组元素本身。但这并不意味着 map 只给 callback 传了一个参数。
```JavaScript
["1", "2", "3"].map(parseInt);
// 你可能觉的会是[1, 2, 3]
// 但实际的结果是 [1, NaN, NaN]
//因为map方法在调用callback函数时,会给它传递三个参数,parseInt有两个参数，第二个 index 被当做进制参数
```

`callback`只会为那些已经被赋值的索引调用。不会为那些被删除或从来没被赋值的索引调用。在处理数组时，数组元素的范围是在 `callback` 方法第一次调用之前就已经确定了。在 方法执行的过程中：原数组中新增加的元素将不会被 `callback` 访问到；若已经存在的元素被改变或删除了，则它们的传递到 `callback`的值是方法遍历到它们的那一时刻的值；而被删除的元素将不会被访问到。



#### 类数组对象

外观和行为像数组，但不共享数组的全部方法

如 函数内部可用的 `arguments` 对象

`documents.getElementsByTagName()` 返回的 NodeList



`typeof arguments` `//'object'`

arguments除了长度之外没有任何数组属性



### 字符串 String

注意区分 JavaScript 字符串对象和基本字符串值 
`'hello'`、`String('hello')`都是基本字符串
`new String()`则是对象

当基本字符串需要调用一个字符串对象才有的方法或者查询值的时候(基本字符串是没有这些方法的)，JavaScript 会自动将基本字符串转化为字符串对象并且调用相应的方法或者执行查询。

下标访问  同数组
切片  `s.slice(x1,x2)` 左包右闭
可以省略结束（默认切到最后）
但不可以省略开始
`s.slice(x1)` right
`s.slice(,x2)`  wrong
`slice`返回新字符串，不会修改原字符串



`string.length`返回字符串长度，由于 JavaScript 使用 UTF-16编码，使用一个16比特的编码单元来表示大部分常见的字符，使用两个代码单元表示不常见的字符，因此 length 返回值可能与字符串中实际的字符数量**不相同**。

| 方法                                         | 描述                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| `toUpperCase()/toLowerCase() `               | 返回大小写形式的新字符串，不影响原字符串                     |
| `trim()`                                     | 删除前后缀空白字符及所有行终止符字符，返回新字符串，不影响原字符串 |
| `str.charAt(index)`                          | 返回指定位置字符                                             |
| `str.indexOf(str[,fromIndex])`               | 返回指定的字符串首次出现的位置，fromIndex可选参数，开始检索位置 |
| `str.lastIndexOf(str[,fromIndex])`           | 从 fromIndex 处开始查找，默认为`str.length`,返回指定的字符串最后出现的位置 |
| `'hello'.includes(str[,position])`           | 返回是否包含true/false，区分大小写，po为起始索引位置，默认为 0 |
| `str.split([separator[,limit]])`             | 分割，limit为分割只截取 X 个元素，可选，返回一个数组         |
| `str.substr(start[,length])`                 | 提取从指定位置指定长度的字符                                 |
| `str.substring(indexStart[, indexEnd])`      | 返回指定索引区间的子串                                       |
| `str.startsWith/endsWith(str[,position])`    | 返回是否以指定字符串开始或结束(ES6) true/false , position为str内搜索字符的开始/结束位置，默认为0/`str.length` |
| `str.repeat(num)`                            | 返回一个新字符串，表示将原字符串重复n次。参数如果是小数，会被取整；是负数或者Infinity，会报错；参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0；NaN等同于 0；参数是字符串，则会先转换成数字。 |
| `str.replace(substr|regexp, newSubStr|func)` | 由新字符串或者一个返回字符串的函数来替换一个字符串/正则。返回新的字符串，不改变新字符串。 |

**注意事项：**

`x.toString()`转换字符串
`String(x)` 相比tostring，对 null 和 undefined值强制类型转换可以生成字符串而不引发错误



`str.charAt(index)`效果和`str[index]`等同，ES6会把字符串当做一个类似数组的对象



`x.split()`如果 separator 为空字符串，会将字符串以数组形式返回，若字符串为空，则返回一个包含一个空字符串的数组，而不是一个空数组。



string 也存在` concat` 方法，且不影响原字符串，但性能不如用 `+` 直接赋值
性能：`+`>`concat`>`join`



`str.substr(start[,length])`

> 如果 start 为正值，且大于或等于字符串的长度，则 substr 返回一个空字符串。

>  如果 start 为负值，则 substr 把它作为从字符串末尾开始的一个字符索引。如果 start 为负值且 abs(start) 大于字符串的长度，则 substr 使用 0 作为开始提取的索引。注意负的 start 参数不被 Microsoft JScript 所支持。

> 如果 length 为 0 或负值，则 substr 返回一个空字符串。如果忽略 length，则 substr 提取字符，直到字符串末尾。
> `'hello'.substr(2,-1) --- ''`



`str.substring(indexStart[, indexEnd]`

> 如果 indexStart 等于 indexEnd，substring 返回一个空字符串。

> 如果省略 indexEnd，substring 提取字符一直到字符串末尾。

> 如果任一参数小于 0 或为 NaN，则被当作 0。

> 如果任一参数大于 `stringName.length`，则被当作 `stringName.length`。

> 如果 indexStart 大于 indexEnd，则 substring 的执行效果就像两个参数调换了一样。

`'hello'.substring(2,-1)` 

相当于`substring(2,0)`，在转换为`substring（0,2）--'he'`



ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart(maxLen, str)`用于头部补全，`padEnd(maxLen, str)`用于尾部补全。
常见用途是为数值补全指定位数。

```javascript
//如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。
'xxx'.padStart(2, 'ab') // 'xxx'
//如果用来补全的字符串与原字符串，两者的长度之和超过了最大长度，则会截去超出位数的补全字符串。
'abc'.padStart(10, '0123456789') // '0123456abc'
//如果省略第二个参数，默认使用空格补全长度。
'x'.padStart(4) // '   x'
```



**模板字符串**（必须是反引号）

```javascript
name = 'eric'
s = `${name},你好`
s = 'eric,你好'
```
**转义符**

```javascript
\n     // 表示一个换行符
\t     // 表示一个 TAB（制表符）
\\     // 表示一个反斜杠 \
\'     // 表示一个单引号
\"     // 表示一个双引号

```



### 对象 Object

`var obj = {}` 
对象字面量法（同样也有数组字面量）
相对 `new Object`，前者可以立即求值，后者本质是方法调用，需要去原型链遍历寻找
 创建方法还有`new Object()`,` Object.create() `



对象属性名字可以是任意字符串，包括空串。如果对象属性名字不是合法的javascript标识符，它必须用""包裹。


键都是字符串，但是在javascript中通常可以省略引号
`var object = {name:eric,'height':180}`





Js 中 objects 是一种引用类型。两个独立声明的对象永远也不会相等，即使他们有相同的属性，无论是`==`还是`===`。
只有在比较一个对象和这个对象的引用时，才会返回true。



`Object.assign(target,...sources)`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target)并返回目标对象，继承属性和不可枚举属性不能拷贝,且是浅拷贝,如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。
对于同名属性，会进行替换，而不是添加



####读取对象元素
`object['name']`
或者点语法 `object.name`
但是点语法有限制，比如key中存在空格，或者key存在于一个变量中，或者key是个数字，或者 key 是一个预留关键字
点语法相对方便，使用较多

```javascript
object['a b']       //right
object.a b          //wrong 
var k = 'name'
object[k]           //right
object.k            //wrong.会把k当做一个key
```



####增加删除元素

```javascript
object[key] = value
object.key = value
delete object[key]
```

delete可以删除一个对象、对象的属性或者数组中的一个键值
可以删除各种隐式声明，被 var 声明的除外。
成功删除返回true，否则返回 false。 如果删除的属性不存在，delete 不起作用，但仍返回true
delete操作只会在自身的属性上起作用
delete 不能删除全局作用域或函数作用域内`var`声明的属性
用 `let`/`const`声明的属性不能从它所在的作用域中被删除



检查元素： `key in object` 返回boolean，不过这个key也可能是object继承得到的
`in`操作符右必须是一个对象，当是一个数组时 左侧必须是索引号 index,不能是数组具体元素



#### 循环遍历

`for..in` 遍历自身和继承的可枚举属性
`Object.keys(o)`    包含自身的不含继承的所有可枚举属性
`Object.getOwnPropertyNames(o)` 包含对象自身的所有属性的键名，包括不可枚举属性，不包含Symbol属性
`Object.getOwnPropertySymbols`比上条多了Symbol属性的键名
`Reflect.ownKeys(obj)`包含Symbol、是否可枚举等的所有键名

**遍历的次序规则：**

1. 首先遍历所有数值键，按照数值升序排列。
2. 其次遍历所有字符串键，按照加入时间升序排列。
3. 最后遍历所有 Symbol 键，按照加入时间升序排列。



由于 `for in`依次访问一个对象**及其原型链**中所有可枚举的属性，需要用 hasOwnProperty 过滤一遍，
```
for (var name in object) {
    if (object.hasOwnProperty(name)) {
        ...
    }
    else {
        ...
    }
}
```
比较麻烦
可以使用`Object.keys(shit).forEach(()=>{})`



####getters & setters
 getter 是一个获取某个特定属性的值的方法。
 setter 是一个设定某个属性的值的方法。
 定义一个已经声明的函数作为的getter和setter方法，使用`Object.defineProperty`

```javascript
var o = {
  a: 7,
  get b() { 
    return this.a + 1;
  },
  set c(x) {
    this.a = x / 2
  }
};
console.log(o.a); // 7
console.log(o.b); // 8
o.c = 50;
console.log(o.a); // 25
//o.a — 数字
//o.b — 返回 o.a + 1 的 getter
//o.c — 由  o.c 的值所设置 o.a 值的 setter
//getter方法必须是无参数的，setter方法只接受一个参数(设置为新值）

var o = { a:0 }

Object.defineProperties(o, {
    "b": { get: function () { return this.a + 1; } },
    "c": { set: function (x) { this.a = x / 2; } }
})
//这个方法的第一个参数是你想定义getter或setter方法的对象，第二个参数是一个对象，这个对象的属性名用作getter或setter的名字，属性名对应的属性值用作定义getter或setter方法的函数
```



#### 对象数据属性

+ `[[Configurable]]`

  > 可改动性，表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。

+ `[[Enumerable]]`

  > 可枚举性，表示能否通过`for-in`循环返回属性

+ `[[Writable]]`

  > 可改动性，表示能否修改属性值

+ `[[Value]]`

  > 包含属性的数据值，读取属性值即从这个位置读取，写入也是保存在这个位置，默认 undefined

```javascript
var o = {}
Object.defineProperty(o, "a", {
  value : 37,
  writable : true,
  enumerable : true,
  configurable : true
});

// 对象o拥有了属性a，值为37
var bValue;
Object.defineProperty(o, "b", {
  get : function(){
    return bValue;
  },
  set : function(newValue){
    bValue = newValue;
  },
  enumerable : true,
  configurable : true
});

o.b = 38;
// 对象o拥有了属性b，值为38
// 数据描述符和存取描述符不能混合使用
Object.defineProperty(o, "conflict", {
  value: 0x9f91102, 
  get: function() { 
    return 0xdeadbeef; 
  } 
});
// throws a TypeError: value appears only in data descriptors, get appears only in accessor descriptors
​```
```



相比点运算符直接赋值，数据描述符的默认值均为 true，`Object.defineProperty()` 默认均为 false



**访问功能：**
可枚举属性：`Object.keys`
不可枚举属性：使用 `Object.getOwnPropertyNames(obj)` 进行迭代，并筛选不通过`propertyIsEnumerable`方法检测的属性
可枚举属性及不可枚举属性:`Object.getOwnPropertyNames(obj)`

迭代功能：
可枚举属性：`for..in`迭代，以` hasOwnProperty`筛选
不可枚举属性：使用 `getOwnPropertyNames` 进行迭代，并筛选不通过`propertyIsEnumerable`方法检测的属性
可枚举属性及不可枚举属性:使用`getOwnPropertyNames`进行迭代



**部分方法**

`Object.getOwnPropertyDescriptor(obj, prop)`返回 obj 某个属性的属性描述符

`Object.getOwnPropertyDescriptors(obj)`获取一个对象的所有自身属性的描述符。


`Object.isExtensible(obj)`方法判断一个对象是否是可扩展的
`Object.preventExtensions`让一个对象变的不可扩展

`Object.seal`  让一个对象密封，不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象。

`Object.isSealed(obj)`方法判断一个对象是否被密封。

`Object.freeze(obj)`冻结对象，使不可扩展，所有属性不可配置，且所有数据属性都是不可写的，如果值也是一个非冻结的对象，还是可以修改的（浅冻结）
`Object.isFrozen()` 方法判断一个对象是否被冻结 



**对象实例方法**

`object.toString()`返回一个表示该对象的字符串。

`obj.hasOwnProperty(prop)`判断对象是否具有指定的属性作为自身（不继承）属性。

`prototypeObj.isPrototypeOf(object)`判断对象是否存在于另一个对象的原型链上。
相比`a instanceof b`，后者是针对 `b.prototype`检查的，前者是针对 b 本身



##运算

### 算数运算

加(+)减(-)乘(*)除(/)求余(%)幂(`**`)

###条件（三元）运算符

`条件 ? 值1 : 值2`

### 关系运算符 

`propNameOrNumber in objectName`

objectName 为对象时，propNameOrNumber为属性名
objectName为数组时，propNameOrNumber为索引值或者数组的属性

### 比较运算

| 比较运算     | code    |
| ------------ | ------- |
| 严格相等     | `===`   |
| 严格不）相等 | `(!)==` |
| (不)等于     | `(!)=`  |
| 大于、小于   | `> & <` |
| 大于等于     | `>=`    |

在javascript中，由于是弱类型语言，类型不同有时候会发生转换。
如 `1 == '1'`返回的是true
`===`也有缺点。NaN彼此不相等，以及`+0 = -0 `

`Object.is(val1,val2)`基本与`===`一致，不会做类型转换，但`NaN`等于自身，`+0`也不等于`-0`



### 逻辑运算

| 逻辑运算 | code |
| -------- | ---- |
| 与       | `&&` |
| 或       | `||` |
| 非       | `!`  |

###短路求值

`false && anything `  短路求值为false
`true || anything`   短路求值为true

对于非布尔值
`a && b` 如果 a 能转换成 false，则运算会返回false，否则返回 b 值
`a || b` 如果 a 能转换成 true，则运算返回 true，否则返回 a 值

由于逻辑表达式是从左往右计算的,所以通常可以按照规则删除小括号.

优先级：
算术操作符 → 比较操作符 → 逻辑操作符 → "="赋值符号



###位运算

位运算符将它的操作数视为32位元的二进制串(0和1组成)而非十进制八进制或十六进制数.例如:十进制数字9用二进制表示为1001，位运算符就是在这个二进制表示上执行运算，但是返回结果是标准的JavaScript数值。

| 位运算符   | code      | description                                                  |
| ---------- | --------- | ------------------------------------------------------------ |
| 与 AND     | `a & b`   | 每一个对应的位都为1则返回1， 否则返回0                       |
| 或 OR      | `a | b`   | 每一个对应的位，只要有一个为1则返回1， 否则返回0             |
| 非 XOR     | `~a`      | 反转                                                         |
| 异或 XOR   | `a ^ b`   | 每一个对应的位，两个不相同则返回1，相同则返回0               |
| 左移 shift | `a << b`  | 将a的二进制串向左移动b位,右边移入0                           |
| 算数右移   | `a >> b`  | 把a的二进制表示向右移动b位，丢弃被移出的所有位(左边空出的位是根据最高位是0和1来进行填充的) |
| 无符号右移 | `a >>> b` | 把a的二进制表示向右移动b位，丢弃被移出的所有位，并把左边空出的位都填充为0 |

移位运算符把操作数转为32bit整数，然后得出一个与待移位数相同种类的值。

例：`9<<2`产生36，因为1001移位2比特向左变为100100，它是36。
`9>>2`产生2，因为1001移位2位向右变为10，其是2。同样，-9>>2产生-3，由于符号被保留。
`19>>>2`产生4，因为10011移位2位向右变为100，它是4。对非负数值，补零右移和带符号右移产生相同结果。



## 语句

###循环语句

`for`

`do white`

`while`

`break` 中断

`continue` 连续

####`for … in`

迭代一个指定的变量去遍历这个对象的**属性** 
其迭代的顺序是依赖于执行环境的，对于数组不一定按次序访问元素

对于*数组遍历*，如果修改了数组对象，添加了通用属性或者方法，`for...in`语句还会返回除了数字索引（index）外的自定义属性的名称（name）。因此对于数组还是使用传统 for 循环比较好。 

在迭代过程中最好不要在对象上进行添加、修改或者删除属性的操作，除非是对当前正在被访问的属性。这里并不保证是否一个被添加的属性在迭代过程中会被访问到，不保证一个修改后的属性（除非是正在被访问的）会在修改前或者修改后被访问，不保证一个被删除的属性将会在它被删除之前被访问。

####`for...of statement` (ES6)

`for...of`语句在可迭代的对象上创建了一个循环(包括Array, Map, Set, 参数对象（ arguments）等等)，对值的每一个独特的属性调用一个将被执行的自定义的和语句挂钩的迭代。

比如循环数组，`for...in`循环遍历的结果是数组元素的下标，而 `for...of` 遍历的结果是元素的值

`for...in`缺点：

1. 数组的键名是数字，而`for...in`是以字符串'0','1'等作为键名的
2. 循环过程中还会遍历手动添加的其他键
3. 某些情况下，`for...in`循环还会以任意顺序遍历键名
   而`for...of`相比下，没有这些缺点。且不同于`forEach`，可以与`break`、`continue`和`return`配合使用，也提供了遍历所有数据结构的统一操作接口



### 条件判断语句

`if…else`

`switch`

```javascript
//多种选择适用
switch(表达式){
    case 值1:
      执行代码块 1
      break;
    case 值2:
      执行代码块 2
      break;
    case 值3：
    case 值4：
      执行代码块3 //3,4情况共用
      break；
    ...
    case 值n:
      执行代码块 n
      break;
    default:
      与 case值1 、 case值2...case值n 不同时执行的代码
}
```

switch…case首先容易出错的地方是其比较的全等性，即在case语句判断中使用的是===运算
再另外，case中的判断是逐条进行的，也就意味着当判断条件增多时，查找对应匹配的条件可能会更费时。

一个好的替换方式是`Object 字面量查询`

```javascript
function getGender(gender) {
  var genderMap = {
    '0': function() {
      return 'female';
    },
    '1': function() {
      return 'male';
    },
    'default': function() {
      return 'other';
    }
  }
  return genderMap[gender] ? genderMap[gender]() || genderMap['default']();
}

//多条件共用业务逻辑
function getGender(gender) {
  function isFemale() {
    return 'female';
  }
  function isMale() {
    return 'male'
  }
  function isDefault() {
    return 'other';
  }
  var genderMap = {
    '1': isFemale,
    '3': isFemale,
    '2': isMale,
    '4': isMale,
    'default': isDefault
  };
  return genderMap[gender] ? genderMap[gender]() || genderMap['default']();
}
```



##函数

函数有 name 属性

```javascript
var f = function () {};

// ES5
f.name // ""

// ES6
f.name // "f"
```

1. 函数表达式

   ```javascript
   var console = function(){
       console.log.apply(console,arguments)
   }
   ```

2. 函数声明

   ```javascript
   function ensure(condition,message){
       if(!condition) {
           log('wrong',message)
       }
   }
   ```

3. 匿名函数

   ```javascript
   (function (x) {
       return x * x;
   })(n)
   ```

   

### 函数提升:

用函数声明创建的函数可以在定义之前就进行调用，解析器会通过声明提升读取并将其添加到执行环境中；而用函数表达式创建的函数不能在被赋值之前进行调用。

而函数表达式定义的函数实际是匿名函数，此时的 `var a = function(){}` , a 相当于一个变量，类型为 undefined，因此浏览器认为 a()并不是一个函数

###默认参数
参数可以设置默认值，当设置默认值时，即使在调用中显式地传入 undefined，也会取默认值。
`function(a,b=5){}`
`function(1,undefined) => a=1 b=5`



前置参数对于后面的默认参数是可见的。已经被声明的参数对于后面的默认参数是可见的。
使用参数默认值时，函数不能有同名参数，不使用则不会报错

```javascript
// 不报错
function foo(x, x, y) {
  // ...
}

// 报错
function foo(x, x, y = 1) {
  // ...
}
```



参数默认值是惰性求值，即每一次重新调用函数时才会计算

```javascript
let x = 99;
function foo(p = x + 1) {
  console.log(p);
}

foo() // 100

x = 100;
foo() // 101
```



通常情况下，默认参数应该是函数的尾参数，这样比较容易看出那些可以省略，若非尾部的参数设置了默认值，则这个参数实际上是没办法省略的。

```javascript
function f(x, y = 5, z) {
  return [x, y, z];
}

f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // 报错
f(1, undefined, 2) // [1, 5, 2]
```



指定了默认值后，函数`length`属性将返回没有指定默认值的参数个数，这容易导致属性失真。因为length属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。
```javascript
(function (a) {}).length // 1
(function (a, b, c = 5) {}).length // 2
//如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了。
(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
```



**默认值作用域**

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（context）。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

```javascript
var x = 1;

function f(x, y = x) {
  console.log(y);
}

f(2) // 2
//调用函数f时，参数形成一个单独的作用域。在这个作用域里面，默认值变量x指向第一个参数x，而不是全局变量x，所以输出是2。

let x = 1;

function f(y = x) {
  let x = 2;
  console.log(y);
}

f() // 1
//函数f调用时，参数y = x形成一个单独的作用域。这个作用域里面，变量x本身没有定义，所以指向外层的全局变量x。函数调用时，函数体内部的局部变量x影响不到默认值变量x。如果此时，全局变量x不存在，就会报错。
```

闭包对于默认参数是不能引用的。因为默认参数被最先解析，而函数内部的声明会在之后解析。



###arguments属性

函数内部对象有this和arguments，
arguments是一个类数组对象，是说它有一个索引编号和Length属性。尽管如此，它并不拥有全部的Array对象的操作方法。



`arguments.callee`，是个指针，指向当前执行的（拥有这个arguments对象的）函数，可以避免与函数名耦合的问题，`arguments.callee()`可用于递归



### 闭包

[闭包&原型链 参考1](http://www.cnblogs.com/wangfupeng1988/p/3977924.html)

[参考2](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/00143449934543461c9d5dfeeb848f5b72bd012e1113d15000)

> JavaScript允许函数嵌套，并且内部函数可以访问定义在外部函数中的所有变量和函数，以及外部函数能访问的所有变量和函数。但是，外部函数却不能够访问定义在内部函数中的变量和函数。这给内部函数的变量提供了一定的安全性。而且，当内部函数生存周期大于外部函数时，由于内部函数可以访问外部函数的作用域，定义在外部函数的变量和函数的生存周期就会大于外部函数本身。当内部函数以某一种方式被任何一个外部函数作用域访问时，一个闭包就产生了。

**原理**

> Js 执行函数时会创建一个作用域对象（每次函数执行时都会创建新的特定的作用域对象，且无法被访问），用来保存函数所创建的局部变量。它和被传入的变量一起被初始化。那么当闭包返回一个内部函数时，Js 的垃圾回收器会回收外部函数所创建的作用域对象，但是返回的内部函数却保留一个指向该作用域对象的引用。所以该作用域对象不会被回收，直到指向的引用计数为零。作用域对象组成了一个名为作用域链。闭包就是一个函数和被创建的函数中的作用域对象的组合。
>
> JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。



### 原型链

[参考](http://www.cnblogs.com/wyaocn/p/5815761.html)

> arr -> Array.prototype -> Object.prototype -> null

因为每个对象和原型都有原型，对象的原型指向原型对象，
而父的原型又指向父的父，这种原型层层连接起来的就构成了原型链。

原型对象用来继承。
`Object.getPrototypeOf(object)`返回对象的原型
`obj.hasOwnProperty(prop)`返回判断某个对象是否含有指定属性的 Boolean，此方法会忽略原型链上继承的属性，若obj 有 hasOwnProperty 属性，可以使用 call
`Object.prototype.hasOwnProperty.call(object,name)`

而`in`操作符只要通过对象能够访问到属性即会返回true，无论属性来自原型还是实例



**原理**

> 每个**函数**都有一个`prototype`对象属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以有特定类型的所有实例共享的属性与方法。
>
> 当一个函数被用作构造函数来创建实例时，这个函数的`prototype` 属性值会被作为**原型**赋值给所有对象实例（也就是设置实例的`__proto__`属性），也就是说，所有实例的原型引用的是函数的`prototype` 属性。
>
> 而对于所有的**对象**，都有`__proto__`属性，这个属性对应该对象的原型，即原型对象的`prototype` 属性值
> `var p = new Person()` 即 `p.__proto__ = Person.prototype`
>
> 函数的原型对象`constructor`默认指向构造函数本身，原型对象除了有原型属性外，为了实现继承，还有一个原型链指针`_proto_`，该指针指向上一层的原型对象，而上一层的原型对象的结构依然类似，这样利用proto一直指向Object的原型对象上，而Object的原型对象用Object.proto = null表示原型链的最顶端，如此变形成了javascript的原型链继承，同时也解释了为什么所有的javascript对象都具有Object的基本方法。

1. 所有的对象都有`__proto__`属性，该属性对应该对象的原型
2. 所有的函数对象都有`prototype`属性，该属性的值会被赋值给该函数创建的对象的`_proto_`属性
3. 所有的原型对象都有constructor属性，该属性对应创建所有指向该原型的实例的构造函数
4. 函数对象和原型对象通过`prototype`和`constructor`属性进行相互关联



**原型继承**
当一个构造函数A中调用了另一个构造函数B，并不等于A继承了B的属性，他的原型链中没有B

 `A -> Object.prototype -> null`
需要插入B
1.直接定义 `A.prototype = B.prototype`
2.引入中间对象C(可以使空函数)指向B     
`C.prototype = B.prototype`
`A.prototype = new C()`
再把A原型的构造函数修复为A
`A.prototype.constructor = A;`



### 类与原型对象

在基于类的面向对象语言，比如 Java 和 C++，类定义了所有用于具有某一组特征对象的属性，实例是类的实例化体现
基于原型的语言（如JavaScript）并不存在这种区别：它只有对象。

定义类时只需要定义构造函数来创建具有一组特定的初始属性和属性值的对象。任何 JavaScript 函数都可以用作构造器。 也可以使用 new 操作符和构造函数来创建一个新对象。

```javascript
var Student = function(name, height) {
    // 用 this.变量名 来创造一个只有类的实例才能访问的变量
    this.name = name
    this.height = height
    this.hello = function() {
        log('hello from ' + this.name)
    }
}

// 用 new 函数名(参数) 初始化一个类的实例
var s1 = new Student('gua', 169)
// 可以通过 . 语法访问对象的属性
log('class, s1', s1.name, s1.height)
//给实例添加改变属性并不会影响类本身
```



**添加方法函数**

```javascript
Student.prototype.greeting = function() {
    console.log(`hello, I'm ${this.name}`)
}
//类名.prototype.方法名 = function(){}
//当然也可以用于设置属性,但这样产生的值并不会直接添加到类的值中，会添加到__proto__属性中
Student.prototype.class = 3
//在浏览器中输入s1，返回
Student {name: "gua", height: 178}
    height: 178
    name: "gua"
    __proto__: Object
        class: 3
        constructor: (name,height)
        __proto__: Object 
        
student1.greeting 实际指向 Object.getPrototypeOf(student1).greeting
//所以 Object.getPrototypeOf(student1).greeting == Object.getPrototypeOf(student2).greeting == Student.prototype.greeting
//简而言之， prototype 是用于类型的，而 Object.getPrototypeOf() 是用于实例的（instances），两者功能一致。
```



####继承属性
访问对象的属性时的步骤：

1. 检查本地值是否存在。如果存在，返回该值。
2. 如果本地值不存在，检查原型链（通过 __proto__ 属性）。
3. 如果原型链中的某个对象具有指定属性的值，则返回该值。
4. 如果这样的属性不存在，则对象没有该属性。

```javascript
var mark = new WorkerBee()
// 通过 new 操作符创建通用对象，并将其作为关键字 this 的值传递给 WorkerBee 的构造器函数，显式地设置构造器内设置的属性的值。

//再隐式地将内部__proto__属性设置为 WorkerBee.prototype值，决定了用于返回属性值的原型链。
//原型链继承的属性不会显式的作为本地变量存放在对象中。
```

继承的一个问题：
在创建父类对象的任意实例时，实例的属性值都将获得本地值，这意味着在创建一个新的父类对象作为子类的原型时，子类.prototype 的该属性值就会有一个本地值，这样当查找一个子类实例的该属性值时就会采用本地值，即使之后修改原型的属性值也不会影响

```javascript
function Employee () {
  this.name = "";
  this.dept = "general";
}

function WorkerBee () {
  this.projects = [];
}
WorkerBee.prototype = new Employee;
var amy = new WorkerBee;
//amy.name == "";
//amy.dept == "general";
//amy.projects == [];
Employee.prototype.name = "Unknown"
//此时 amy.name == "";并不会影响
```



所以希望对象属性有默认值，且运行时修改对象属性值能被后代继承的方法是：
不要在对象构造器函数中定义该属性，
将属性添加到该对象所关联的原型中。

```javascript
function Employee () {
  this.dept = "general";
}
Employee.prototype.name = "";

function WorkerBee () {
  this.projects = [];
}
WorkerBee.prototype = new Employee;

var amy = new WorkerBee;

Employee.prototype.name = "Unknown";
//此时 amy.name == 'Unknown'
```



`new.target`属性检测函数或构造方法是否是通过 new 运算符调用的，返回一个指向构造方法或函数的引用，否则返回 undefined

通常`new.`的作用是提供属性访问的上下文，不过这里的 new 不是真正的对象，而是一个虚拟上下文。

```javascript
function Foo() {
  if (!new.target) throw "Foo() must be called with new";
  console.log("Foo instantiated with new");
}

Foo(); // throws "Foo() must be called with new"
new Foo(); // logs "Foo instantiated with new"
```



### 常用方法

`function.caller`返回调用该函数的函数，如在全局作用域，则返回 null

**对象实例方法**
`Function.prototype.apply(this,[args])`指定 this 值，参数作为数组或类数组对象提供

`Function.prototype.call(this[, arg1[, arg2[, ...]]])`

`Function.prototype.bind(this[, arg1[, arg2[, ...]]])`

`function.toString()`返回一个表示当前函数源代码的字符串。覆盖了 `Object.prototype.toString` 方法。

## 错误

```javascript
try {
    //定义在执行时进行错误测试的代码块
} catch(err) {
    //当 try 代码块发生错误时，所执行的代码块。
} finally {
}
throw exception //抛出异常
```



异常只会被离它最近的封闭catch块捕获一次。当然，在“内部”块抛出的任何新异常 （因为catch块里的代码也可以抛出异常），将会被“外部”块所捕获。因此如果有嵌套的`try..catch`要注意外部的`catch`不一定捕获到嵌套内部的异常

finally子句在try块和catch块之后执行但是在一个try声明之前执行. 无论是否有异常抛出它总是执行。 如果有异常抛出finally子句将会被执行。
对于嵌套语句，内部的 finally 执行也优于外部的 catch

```javascript
try {
  try {
    throw new Error("oops");
  }
  finally {
    console.log("finally");
  }
}
catch (ex) {
  console.error("outer", ex.message);
}

// Output:
// "finally"
// "outer" "oops"

try {
  try {
    throw new Error("oops");
  }
  catch (ex) {
    console.error("inner", ex.message);
  }
  finally {
    console.log("finally");
  }
}
catch (ex) {
  console.error("outer", ex.message);
}

// Output:
// "inner" "oops"
// "finally"

try {
  try {
    throw new Error("oops");
  }
  catch (ex) {
    console.error("inner", ex.message);
    throw ex;
  }
  finally {
    console.log("finally");
  }
}
catch (ex) {
  console.error("outer", ex.message);
}

// Output:
// "inner" "oops"
// "finally"
// "outer" "oops"

//如果从finally块中返回一个值，那么这个值将会成为整个try-catch-finally的返回值，无论是否有return语句在try和catch中。这包括在catch块里抛出的异常。
    finally {
        console.log("finally");
        return;
    }
    //则无"outer" "oops"
```

## BOM 浏览器对象

###window 对象

![看看](http://img.mukewang.com/535483720001a54506670563.jpg)
`confirm(str)` 弹出对话框（包括str和一个确定按钮和取消按钮，返回bool值

`prompt(str1, str2)`
str1: 要显示在消息对话框中的文本，不可修改
str2：文本框中的内容，可以修改
用于和用户交互，弹出对话框（包含文本输入框和确定取消按钮），确定则将文本框中的内容作为返回值。

`window.open([URL], [窗口名称], [参数字符串])`
窗口名称：名称或者"_top"、"_blank"、"_self"等有意义的名称
参数字符串：top/left等参数设置
![参数](http://img.mukewang.com/52e3677900013d6a05020261.jpg)



###时间对象

![看看](http://img.mukewang.com/555c650d0001ae7b04180297.jpg)
时间单位：毫秒

getYear和setYear方法因为会受千年虫问题的影响,所以已经被getFullYear和setFullYear方法替代



### Navigator 对象

> Navigator 对象包含有关浏览器的信息，通常用于检测浏览器与操作系统的版本。

![属性](http://img.mukewang.com/5354cff70001428b06880190.jpg)



### screen 对象

> 用于获取用户的屏幕信息。

![属性](http://img.mukewang.com/5354d2810001a47706210213.jpg)



### Location 对象

> `location.[属性|方法]` 用于获取或设置窗体的URL，并且可以用于解析URL

![](http://img.mukewang.com/53605c5a0001b26909900216.jpg)



location 对象属性：
![](http://img.mukewang.com/5354b1d00001c4ec06220271.jpg)



location 对象方法:
![](http://img.mukewang.com/5354b1eb00016a2405170126.jpg)



### document 页面对象

document对象有一个cookie属性，可以获取当前页面的Cookie。

#### Cookie

> 由服务器发送的key-value标示符。因为HTTP协议是无状态的，但是服务器要区分到底是哪个用户发过来的请求，就可以用Cookie来区分。当一个用户成功登录后，服务器发送一个Cookie给浏览器，例如user=ABC123XYZ(加密的字符串)...，此后，浏览器访问该网站时，会在请求头附上这个Cookie，服务器根据Cookie即可区分出用户。

Cookie还可以存储网站的一些设置，例如，页面显示的语言等等。

### History 对象

> 记录了用户曾经浏览过的页面(URL)，并可以实现浏览器前进与后退相似导航的功能。

![](http://img.mukewang.com/53548c030001759e05840068.jpg)
![](http://img.mukewang.com/53548c200001228206210123.jpg)

back()相当于go(-1)

alert("文本")   --  警告框
confirm("文本") --  确认框
prompt("文本","默认值") --  提示框



## DOM

> 它定义了访问HTML和XML文档的标准，提供了对文档的结构化的表述,可以使从程序中对该结构进行访问，从而改变文档的结构，样式和内容。简言之，它会将web页面和脚本或程序语言连接起来。

严格地讲，我们这里的DOM节点是指Element，但是DOM节点实际上是Node，在HTML中，Node包括Element、Comment、CDATA_SECTION等很多种，以及根节点Document类型，但是，绝大多数时候我们只关心Element，也就是实际控制页面结构的Node，其他类型的Node忽略即可。根节点Document已经自动绑定为全局变量document。

### 查找元素

使用 `document.querySelector(选择器)`
烂办法：`document:getElementById()`
`document:getElementsByClassName()`

**获取多个选择器**

`document.querySelectorall('.class')`

返回的是一个`NodeList`,类似数组的对象



### 操作元素（创建，删除，修改）

```javascript
var button = document.creatElement('button')
button.innerHTML = 'xxx'        
//用innerHTML来设置元素button的属性XXX.以上相当于创建了<button>xxx</button>

form.insertAdjacentHTML('beforebegin','<a href="#">这是一个链接</a>')
//即在form元素中 开头 插入一个链接
//位置有：beforebegin,afterbegin,beforeend,afterend.前后两个区别在于是否在form元素块内
//插入指定位置
parentElement.insertBefore(newElement, referenceElement);

var pwd = document.querySelector(#id)
form.removeChild(pwd)
//或者
pwd.remove()
//删除元素：先获取想要删除的元素名，然后通过父节点（父元素）来删除!!!不知道父节点的可用以下方法
pwd.parentElement.removeChild(pwd)

```

### 添加事件

```javascript
//添加事件, 使用 addEventListener 函数,（又叫事件监听） 它有三个参数：第一个是事件的名字,第二个是事件发生后会被自动调用的函数，第三个见后面
loginButton.addEventListener('click', clicked，false)
```

### 事件委托

> 对于部分元素需要在运行的时候才能绑定事件，对此可以将事件绑定到父元素上，在运行时候检查被点击的对象是否是我们想要的。

**失去焦点**
 `target.blur()`
**拥有焦点**
``target.focus()`

**阻止默认行为的发生, 也就是不插入回车**
` event.preventDefault()`

### 冒泡/捕获

**事件捕获**
当你使用事件捕获时，父级元素先触发，子级元素后触发。
**事件冒泡**
当你使用事件冒泡时，子级元素先触发，父级元素后触发。
`target.addEventListener(event,fn,useCapture)` 
useCapture默认为false，即冒泡，true则为捕获



*支持事件冒泡*（EventBubbling）的事件类型为鼠标事件和键盘事件，例如：mouseover, mouseout, click, keydown, keypress。

*接口事件*则通常不支持事件冒泡（Event Bubbling），例如：load, change, submit, focus, blur。
[参考](http://www.nowamagic.net/javascript/js_EventBubbling.php)



停止冒泡 ` event.cancelBubble = true `

###获取/设置/查询/删除元素的属性

```javascript
var user = document.querySelector('#id')
var uservalue = user.getAttribute('value')
//即查找了带有#id的元素，再进一步获取该元素的value属性

user.setAttribute('value',XXX)
//即增加了user元素一个属性value，属性值为XXX

user.hasAttributes()        //查询是否含属性，不用提供参数
user.hasAttribute('value')      //查询是否含特定属性value
//查询的返回值均为true/false

user.removeAttribute('value')       //删除元素的'value'属性
```

### DOM事件流

事件捕获阶段：实际目标在捕获阶段不会接收到事件（比如事件从document到`<html>`到`<body>`后就停止了。

处于目标价段：事件在目标元素上发生，并在事件处理中被看成冒泡阶段的一部分

事件冒泡阶段：事件又传播回文档。



节点属性: 

![](http://img.mukewang.com/5375c953000117ee05240129.jpg)

nodeName 属性: 节点的名称，是只读的。 
1. 元素节点的 nodeName 与标签名相同 
2. 属性节点的 nodeName 是属性的名称 
3. 文本节点的 nodeName 永远是 #text 
4. 文档节点的 nodeName 永远是 #document

nodeValue 属性：节点的值 

1. 元素节点的 nodeValue 是 undefined 或 null 
2. 文本节点的 nodeValue 是文本自身 
3. 属性节点的 nodeValue 是属性的值

nodeType 属性:

| 元素类型 | 节点类型 |
| :------- | :------- |
| 元素     | 1        |
| 属性     | 2        |
| 文本     | 3        |
| 注释     | 8        |
| 文档     | 9        |

遍历节点树: 

![](http://img.mukewang.com/53f17a6400017d2905230219.jpg)
DOM操作:
![](http://img.mukewang.com/538d29da000152db05360278.jpg)



### 操作 class

classList属性：
对于所有元素的classList属性，有以下方法：
add(value)
contains(value)
remove(value)
toggle(value)



在事件流（捕获，处于目标，冒泡）中 
事件对象： 
DOM中传入事件处理程序中的event对象

| 属性/方法                  | 类型         | 说明                                                         |
| :------------------------- | :----------- | :----------------------------------------------------------- |
| bubbles                    | Boolean      | 说明事件是否冒泡                                             |
| cancleable                 | Boolean      | 表明是否可以取消事件的默认行为                               |
| currentTarget              | Element      | 其事件处理程序当前正在处理事件的那个元素                     |
| detail                     | Integer      | 与事件相关的细节信息                                         |
| eventphase                 | Integer      | 调用事件处理程序的阶段：捕获，处于目标，冒泡                 |
| preventDefault()           | Function     | 取消事件的默认行为                                           |
| stopImmediatePropagation() | Funcion      | 取消事件的进一步捕获或冒泡，同时组织任何事件处理程序被调用   |
| stopPropagation()          | Function     | 取消事件的进一步捕获或冒泡                                   |
| target                     | Element      | 事件的目标                                                   |
| trusted                    | Boolean      | 为true表示事件是浏览器生成的，为false表示事件是由开发人员通过Js创建的 |
| type                       | String       | 被触发的事件的类型                                           |
| view                       | AbstractView | 与事件关联的抽象视图                                         |



在IE中的event对象：
cancleBubble，ueviw设置为true可取消事件冒泡，与stopPropagation()作用相同
returnValue，设flase可取消事件的默认行为与preventDefault()作用相同
srcElement，事件目标，与target相同
type
由于IE中事件处理程序的作用域由使用方式决定，由attachEvent使用则this指向window。

###事件类型：
**UI事件：**
1.load事件
2.upload事件（用户从一个页面切换到另一个页面，清除引用避免内存泄露)
3.resize事件。浏览器窗口被调整时发生（firefox会在停止调整时触发，其他会在变化1像素时触发并随着变化不断触发）
4.scroll事件 发生在window对象上

**焦点事件**
blur:元素失去焦点时触发，这个事件**不会冒泡**
focus：获得焦点时触发，这个事件**不会冒泡**
focusin：元素获得焦点时触发，与focus等价，但他冒泡
focusout：元素失去焦点时触发，同blur

焦点移动时依次顺序：
focusout在失去焦点元素上触发；
focusin在获得焦点的元素上触发；
blur在失去焦点的元素上触发；
focus在获得焦点的元素上触发

**鼠标事件、滚轮事件**
click：单击或者按回车键
dblclick
mousedown
mouseenter：鼠标从元素外部首次移动到元素范围之内时触发，**不冒泡**，且移动到后代元素上不触发
mouseleave：位于元素上的鼠标移动到元素范围外时触发，**不冒泡**，且移动到后代元素上不会触发。
mousemove：鼠标在元素内部移动时重复触发。
mouseout：从一个元素上方移动到另一个元素
mouseover：鼠标位于一个元素外部并首次移入另一个元素边界内时触发
mouseup：释放鼠标时触发
mousewheel：鼠标滚轮

mouseover和mouseout会涉及较多元素，所以DOM通过event的relatedTarget属性提供相关元素的信息，仅对这两个事件有效

在触摸设备中：
1.不支持dllclick
2.轻击可单击元素会触发mousemove事件，若此操作没有导致内容变化会依次产生mousedown、mouseup、click事件，轻击不可单击的元素不会触发任何事件。
3.mousemove也会触发mouseover和mouseout事件
4.两个手指在屏幕上且页面随之滚动时触发mousewheel和scroll事件。


**键盘事件、文本事件**
keydown:按下任意键时触发
keypress：按下字符键以及Esc时触发
keyup：释放键时触发

DOM3中新增了key和char属性。key值是相应的文本字符，非字符键则是相应键名。char在按下字符键时与key相同，非字符键则返回null。这些都存在跨浏览器问题

textInput和keypress稍有不同，任何可获得焦点元素都可以触发keypress，但只有可编辑区域才能触发textinput事件。且，textinput只会在用户按下能够输入实际字符的键时才会触发，keypress在按下能够影响文本显示的键时也会触发。textinput有data属性，值就是用户输入的字符


**合成事件**(用于处理输入法编辑器的输入序列)
**变动事件**（当底层DOM结构发生变化）

## ES6新增语法功能

### let

就是限制作用域在 {} 内（块级作用域）的 var
（var 的作用域是函数！不是 if 之类的语句块）

let（const）将不会提升变量到代码块的顶部。
```javascript
for(let i = 0; i < 3; l; i++) {
   log(i)
}
log(i) //此时已经没有i变量了

//let并不会像var一样在全局对象上创造一个属性
let x = 3
this.x //undefined
```

当用到内部函数的时候，let会让你的代码更加简单。
```javascript
var list = document.getElementById("list");

for (let i = 1; i <= 5; i++) {
  var item = document.createElement("LI");
  item.appendChild(document.createTextNode("Item " + i));

  let j = i;
  item.onclick = function (ev) {
    console.log("Item " + j + " is clicked.");
  };
  list.appendChild(item);
}
```
上面这段代码的意图是创建5个li,点击不同的li能够打印出当前li的序号。如果不用let，而改用var的话，将总是打印出 Item 5 is Clicked，因为 j 是函数级变量，5个内部函数都指向了同一个 j ,而 j 最后一次赋值是5。用了let后，j 变成块级域（也就是花括号中的块，每进入一次花括号就生成了一个块级域）,所以 5 个内部函数指向了不同的 j 。

`switch`内不能使用 let，因为 switch只有一个作用块。



### const

> 用来声明一个不可赋值的变量
> 变量的值只能在声明的时候赋予
> 当常量是一个对象时，对象的属性并不在保护的范围内，可以修改属性值，但不可以修改 key
> 当常量是一个数组时，可以继续填充数据

```javascript
const a = 1
a = 2 // 错误

// 下面的不是赋值 是操作 所以是可以的
const arr = [1, 2]
arr.push(3)
// [1, 2, 3]
//个人理解：一个变量名（a）引用内存一个位置上的变量（b）,那么，a就不能再指向别的位置的变量，但是变量本身还可以改变
```



### Map/Set 对象

> Map对象就是一个简单的键值对映射集合，可以按照数据插入时的顺序遍历所有的元素。

`new Map([iterable])`

Iterable 可以是一个数组或者其他 iterable 对象，其元素或为键值对，或为两个元素的数组。 每个键值对都会添加到新的 Map。null 会被当做 undefined。



Map和object区别：

1. 键值可以是除字符串外的其他数据类型
2. Map 的遍历遵循元素的插入顺序
3. Object有原型，所以映射中有一些缺省的键。可以理解为（map = Object.create(null)）
4. 必须手动计算Object的尺寸，但是可以很容易地获取使用Map的尺寸
5. NaN 也可以作为Map对象的键，虽然两个NaN不同，但在 Map 键内视作相等，+0和-0相等



Map 的键是和内存地址绑定的，因此对象作为键名，只要不是同一个引用，即使看起来相等，也被视为两个键，而键时简单类型的值，只要两个值严格相等，就会视为一个键



| 方法                             | 作用                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| `m.set(k,v)`                     | 返回 Map 对象                                                |
| `m.get(k)`                       | 获取指定元素，不存在则返回 undefined                         |
| `m.has(k)`                       | 返回布尔值                                                   |
| `m.delete(k)`                    | 移除元素并返回 true，不存在则返回 false                      |
| `m.size`                         | 返回键值对数量                                               |
| `m.clear()`                      | 移除所有元素                                                 |
| `m.forEach(callback[, thisArg])` | 以插入顺序对 Map 对象中的每一个键值对执行一次参数中提供的回调函数。 |



> Set对象是一组值的集合，这些值是不重复的，可以按照添加顺序来遍历。

Set 和 array区别:

1. 存储不重复的key（重复自动过滤掉）
2. 数组的  无法找到 NaN 值
3. 集合允许删除元素，数组需要使用 splice
4. 数组中用于判断元素是否存在的函数效率低下
   值得相等：NaN相等，正负0相等
   `new Set([iterable])`
   如果传递一个可迭代对象，它的所有元素将被添加到新的 Set中。如果不指定此参数或其值为null，则新的 Set为空。



| 方法                            | 作用                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| `s.add(key)`                    | 添加元素，返回对象本身                                       |
| `s.has(key)`                    | 返回布尔值                                                   |
| `s.delete(key)`                 | 删除元素并返回 true，否则返回false                           |
| `s.size`                        | 返回元素数量                                                 |
| `s.clear()`                     | 清空元素，返回 undefined                                     |
| `s.forEach(callback[,thisArg])` | 根据集合中元素的顺序，对每个元素都执行提供的 callback 函数一次。 |

set 内部判断两个值是否不同，使用的算法类似于`===`，区别在于NaN等于自身

**Set的遍历方法：**
`keys()` 返回键名的遍历器
`values()` 返回键值的遍历器
`entries()` 返回键值对的遍历器
`forEach()` 使用回调函数遍历每个成员
由于Set结构没有键名只有键值，因此`keys`和`values`完全一致
Set的遍历顺序就是插入顺序。
Map 的遍历同，顺序也是插入顺序。



####数组和集合的转换
数组 > 集合：`var mySet = new Set(array)`
集合 > 数组：`Array.from(mySet)`/`[...mySet]`
所谓类似数组的对象，本质特征只有一点，即必须有length属性。因此，任何有length属性的对象（类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）），都可以通过Array.from方法转为数组



`Array.from`还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
`Array.from(arrayLike, x => x * x);`



对于Map和Set循环，他们为iterable变量。可采用for(var x of a)
而for..in实际循环的是对象的属性名称（索引即一个属性），而for..of只循环集合内的元素
更好的方法是调用forEach函数：
`a.forEach(function (element, index, array) {}）`
`s.forEach(function (element, sameElement, set) {}）`
`m.forEach(function (value, key, map) {}）`



### 箭头函数

`(x,y)==>{...}`
箭头函数内部的this是词法作用域，由上下文确定,也就是外层调用者obj。因此如果通过 call() 或 apply() 方法调用一个函数时，只是传入了参数而已，对 this 并没有什么影响



箭头函数都是匿名函数，不用写 function

如果箭头函数 无参数 , 必须使用 ()圆括号或者 _下划线
`() => { statements; } `
` _ => { statements; }`

如果只有一个参数，圆括号是可选的
`singleParam => { statements; }`

当删除大括号时，它将是一个隐式的返回值，这意味着我们不需要指定我们返回
`(param1, param2, …, paramN) => expression`

箭头函数在参数和箭头之间不能换行



```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 25
```



**注意点：**

1. 箭头函数不会在其内部暴露出`arguments`参数，使用 arguments 会报未声明的错误。

   只会指向具体的名为 arguments 的值。即除非存在全局变量`arguments`或者继承的函数的 arguments

   因此无法使用`arguments.length`, `arguments[0]`等。可以使用**rest 参数**`f = (...args) => args[0]`来解决

2. 箭头函数不能用作构造器，没有 constructor，和 new 一起用就会抛出错误。且不支持 `new.target`

3. 箭头函数没有原型属性(`prototype`)，所以箭头函数本身没有 this。

   ```javascript
   var Foo = () => {};
   console.log(Foo.prototype); 
   // undefined
   ```

   

4. 箭头函数不可以使用 yield 命令，因此不能用作 Generator 函数

5. 箭头函数的 this 指向定义时所在的外层第一个普通函数，被继承的函数的 this 指向改变，箭头函数的 this 也会改变(去修改被继承的函数的 this 的指向，那么箭头函数的 this 指向也会跟着改变)



对于普通函数，在非严格模式下，this 默认绑定指向全局对象，严格模式下指向 undefined

当箭头函数外层没有普通函数继承，无论是严格模式还是非严格模式都指向`window`全局对象



### class

虽然是 class，实际仍是基于原型的。
关键字： `class`, `constructor`, `static`, `extends`, 和`super`.
`super `关键字用于调用一个对象的父对象上的函数。

Class声明不可以提升

```javascript
super([arguments]); 
// 调用 父对象/父类 的构造函数
//子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。

super.functionOnParent([arguments]); 
// 调用 父对象/父类 上的方法
//super()单独出现必须在使用 this 关键字之前
```

```javascript
class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }
  get area() {
    return this.height * this.width;
  }
  set sideLength(newLength) {
    this.height = newLength;
    this.width = newLength;
  }
}

var square = new Square(2);
```



`super`这个关键字，既可以当作函数使用，也可以当作对象使用。
作为函数调用时，代表父类的构造函数。但是返回的是子类的实例，即super内部的this指的是子类的实例，因此super()在这里相当于`A.prototype.constructor.call(this)`
第二种情况，super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。



```javascript
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```



ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）。ES6 的继承机制完全不同，先新建父类的实例对象this，然后再用子类的构造函数修饰this，使得父类的所有行为都可以继承，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。

这也导致了ES5由于父类的内部属性无法获取，导致无法继承原生的构造函数。



### Generator

> 生成器函数在执行时能中途退出，后面又能重新进入继续执行。而且在函数内定义的变量的状态都会保留，不受中途退出的影响。

由function*定义
```javascript
function* foo(x) {
    yield x + 1;
    yield x + 2;
    return x + 3;
}
//调用foo(5)仅仅是创建了一个generator迭代器（iterator）对象，还没有去执行它。
//调用generator对象有两个方法，一是不断地调用generator对象的next()方法,返回一个对象{value: x, done: true/false},done表示这个generator是否已经执行结束了。如果done为true，则value就是return的返回值。
//yield 也可以用 yield* 委派至另一个生成器函数。
//调用 next() 方法时，如果传入了参数，那么这个参数会取代生成器函数中对应执行位置的 yield 表达式（整个表达式被这个值替换）

foo.next('pretzel'); // pretzel


var x = foo(5)
x.next()
x.next()

//当在生成器函数中显式 return 时，会导致生成器立即变为完成状态，即调用 next() 方法返回的对象的 done 为 true。

//第二个方法是直接用for ... of循环迭代generator对象
for (var x of fib(5)) {
    console.log(x); // 依次输出0, 1, 1, 2, 3
}
```
生成器函数不能当构造器使用



### Rest/扩展符号`…`

> 作用是把数组解开成单独的元素

```javascript
var a1 = [1, 2, 3]
var a2 = [...a1, 4]
// 结果是 [1, 2, 3, 4]

...可以用来传参
var a1 = [1, 2, 3]
log(...a1)
// 相当于 log(1, 2, 3)
// 类似于 log.apply(null, a1)
```
由于js允许对函数传入任意个参数，若多余定义的参数量，便可用`...rest`来接收，`var x = function(a,b,...rest){..}`  `x(1,2,3,4,5,6)`

**注意**

 剩余参数`...rest `必须是参数列表的**最后一个参数**！**且后面不能加逗号**！
 但是`...spread` 无限制!

 函数的**length属性，不包括 rest 参数**。



```javascript
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```



任何定义了遍历器（Iterator）接口的对象，都可以用扩展运算符转为真正的数组。



### 解构赋值

> 解构数组
> 交换变量
> 将剩余数组赋值给变量
> 解构对象
> 加载模块
> For of 迭代和解构

```javascript
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true 

// 从一个对象中提取变量并赋值给和对象属性名不同的新的变量名
var {p: foo, q: bar} = o;

console.log(foo); // 42
console.log(bar); // true  
//这实际上说明，对象的解构赋值是下面形式的简写
let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
//也就是说，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。


//变量可以先赋予默认值,默认值生效的条件是，对象的属性值严格等于undefined。
var {a = 10, b = 5} = {a: 3};

console.log(a); // 3
console.log(b); // 5

//加载模块
const { Loader, main } = require('toolkit/loader');
```



可变参数
用 ... 语法可以实现可变长度的参数, 多余的参数会被放进 args 数组中
args 是任意的变量名..类似python中的*args

```javascript
 var foo = function(a, ...args) {
   log(a, args.length)
}
foo(1, 2, 3, 4)
```



### export & import

从给定的文件 (或模块) 中导出函数，对象或原语。
**命名导出**

```javascript
export { myFunction }; // 导出一个函数声明
export const foo = Math.sqrt(2); // 导出一个常量
```
**默认导出**

```javascript
export default myFunctionOrClass // 或者 'export default class {}'
```



### class

```javascript
class Module {
    static functionName{...} 
    //静态方法
    function(){...}     
    //实例方法，相当于ES5内Module.prototyle.function(){}
}
//静态方法，属于类的方法，即类可以直接调用的方法。为类所有实例化对象所共用（但不能用实例对象之间调用），所以静态成员只在内存中占一块区域；

//实例方法，属于实例化类后对象的方法，即实例对象调用的方法。每创建一个类的实例，都会在内存中为非静态成员分配一块存储；

 //静态方法在一启动时就实例化了，因而静态内存是连续的，且静态内存是有限制的；而非静态方法是在程序运行中生成内存的，申请的是离散的空间。
 
 class a extends b {
    constructor(){
        //用于继承
        super()
        this.XXX=XXX
    }
 }
 //a继承 b
```



##this 作用域

this是一个动态作用域，总是指向调用该方法的对象。改变其指向的方法有：

1. `fun.bind(this,[arg1,arg2,..])`返回由指定的this值和初始化参数改造的原函数拷贝
2. `fun.apply(this,[args])`
3. `fun.call(this,arg1[,arg2,...])`

指定的 this 值不一定是真正的 this 值，非严格模式下，指定 null 或 undefined 会指向全局对象（window对象），且值为原始值（数字、字符串、布尔值）的 this 会指向原始值的自动包装对象

```javascript
var o = {
    foo:1,
    bar:function() {
        return this.foo
    }
}

o.bar() ---- 1
var a = o.bar
a()  ---undefined,此时this指向window

因此可以用
var b = o.bar.bind(o)   //此时b的作用域被绑定到o上
b() ----- 1

//要保证this指向正确，必须用obj.xxx()的形式调用！

//apply 和 call 的区别在于
apply() 和 call() 的第一个参数都是用来绑定到this
后面的传入参数：
apply是把参数打包作为一个数组array传入，然后解开将单独元素传入调用
call是将参数直接传入
console.log.apply(console,[1,2,3])
console.log.call(console,1,2,3)

bind要调用和传参数，需要额外加括号 log.bind(console)(1,2,3  )
```



## 内存分配

内存生命周期：

1. 分配你所需要的内存
2. 使用分配到的内存（读、写）
3. 不需要时将其释放\归还

第一步,Js 在定义变量时便完成了内存分配
```javascript
var n = 123; // 给数值变量分配内存
var s = "azerty"; // 给字符串分配内存
var s = "azerty";
var s2 = s.substr(0, 3); // s2 是一个新的字符串
// 因为字符串是不变量，
// JavaScript 可能决定不分配内存，
// 只是存储了 [0-3] 的范围。
```

第二步的使用是对分配内存进行读取与写入的操作
低级语言如 C 语言的第三步是清晰定义的，Js 是隐藏的，采用垃圾回收算法

### 垃圾回收

> 垃圾回收算法主要依赖于引用（reference）的概念。在内存管理的环境中，一个对象如果有访问另一个对象的权限（隐式或者显式），叫做一个对象引用另一个对象。例如，一个Javascript对象具有对它原型的引用（隐式引用）和对它属性的引用（显式引用）。



1.引用计数
    此算法把“对象是否不再需要”简化定义为“对象有没有其他对象引用到它”。如果没有引用指向该对象（零引用），对象将被垃圾回收机制回收。
    缺点：无法处理循环引用。

```javascript
function f(){
  var o = {};
  var o2 = {};
  o.a = o2; // o 引用 o2
  o2.a = o; // o2 引用 o

  return "azerty";
}

f();
//两个对象被创建，并互相引用，形成了一个循环。它们被调用之后不会离开函数作用域，所以它们已经没有用了，可以被回收了。然而，引用计数算法考虑到它们互相都有至少一次引用，所以它们不会被回收
```

2.标记-清除算法
    这个算法把“对象是否不再需要”简化定义为“对象是否可以获得”。假定设置一个叫做根（root）的对象（在Javascript里，根是全局对象）。定期的，垃圾回收器将从根开始，找所有从根开始引用的对象，然后找这些对象引用的对象……从根开始，垃圾回收器将找到所有可以获得的对象和所有不能获得的对象。
    “有零引用的对象”总是不可获得的，但是相反却不一定.在上面的示例中，函数调用返回之后，两个对象从全局对象出发无法获取。因此，他们将会被垃圾回收器回收。



## Error

RangeError 数值变量或参数超出有效范围
ReferenceError  无效引用
SyntaxError 解析过程中发生语法错误
TypeError 变量不属于有效类型
EvalError   与 eval（）有关
InternalError   js 引擎内部错误的异常，如递归太多
URIError    给 encodeURI（）传递的参数无效



## RegExp 正则

[参考](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499503920bb7b42ff6627420da2ceae4babf6c4f2000)

`\d`可以匹配一个数字
`\w`可以匹配一个字母或数字
`.`可以匹配任意字符
`*`表示任意个字符（包括0个）
`+`表示至少一个字符，
`?`表示0个或1个字符，
`{n}`表示n个字符，
`{n,m}`表示n-m个字符
`\s`可以匹配一个空格（也包括Tab等空白符）
`[0-9a-zA-Z\_]`可以匹配一个数字、字母或者下划线；
`A|B`可以匹配A或B，
`^`表示行的开头，`^\d`表示必须以数字开头。
`$`表示行的结束，`\d$`表示必须以数字结束。
javascript第一种方式是直接通过/正则表达式/写出来，`var re1 = /ABC\-001/`
第二种方式是通过newRegExp('正则表达式')创建一个RegExp对象。`var re2 = newRegExp('ABC\\-001')`



```javascript
\d{3}\s+\d{3,8}。
即
\d{3}表示匹配3个数字，例如'010'；
\s可以匹配一个空格（也包括Tab等空白符），所以\s+表示至少有一个空格，例如匹配''，'\t\t'等；
\d{3,8}表示3-8个数字，例如'1234567'。
综合起来，上面的正则表达式可以匹配以任意个空格隔开的带区号的电话号码。
如果要匹配'010-12345'这样的号码呢？由于'-'是特殊字符，在正则表达式中，要用'\'转义，所以，上面的正则是\d{3}\-\d{3,8}。
```



字符串的正则方法
`String.match()` 
`String.replace()`
`String.search()`
`String.split()`



## Date

`new Date()`
`new Date(value)`
`new Date(dateString)`
`new Date(year, month[, day[, hour[, minutes[, seconds[, milliseconds]]]]])`

value是自1970年1月1日00:00:00 (世界标准时间) 起经过的毫秒数。
dateString是表示日期的字符串值。该字符串应该能被 Date.parse() 方法识别

**方法**

`Date.now()`

> 返回时间标准时间至今所经过的毫秒数。

`Date.parse()`

> 解析一个表示日期的字符串，并返回从 1970-1-1 00:00:00 所经过的毫秒数。

`Date.UTC()`

> 接受和构造函数最长形式的参数相同的参数（从2到7），并返回从 1970-01-01 00:00:00 UTC 开始所经过的毫秒数。

**实例方法**

`dateObj.getTime() `返回时间的格林威治时间数值。从1970年1月1日0时0分0秒距离代表时间的毫秒数。

`dateObj.getTimezoneOffset()`返回协调世界时（UTC）相对于当前时区的时间差值，单位为分钟。（时区偏差）有正负值

`dateObj.getUTCXXX()`以世界时为标准，返回XXX

`dateObj.toISOString()`返回一个 ISO格式的字符串： YYYY-MM-DDTHH:mm:ss.sssZ。时区总是UTC（协调世界时），加一个后缀“Z”标识。 "2019-04-09T01:56:11.646Z"

`dateObj.toJSON()`返回一个 JSON 格式字符串

`dateObj.toDateString()`以美式英语和人类易读的形式返回日期部分的字符串。`Wed Jul 28 1993`

`dateObj.toString()` `Wed Jul 28 1993 14:39:07 GMT+0800 (CST)`
`dateObj.toTimeString()``14:39:07 GMT+0800 (CST)`
`dateObj.toLocaleDateString([locales [, options]])`根据语言环境来返回
`dateObj.toUTCString()`返回使用UTC时区表示给定日期的字符串



##Others

关闭autoprefixer对部分代码的转换
```javascript
.a {
  display: -webkit-box;
  /* autoprefixer: off */
  -webkit-box-orient: vertical;
  -webkit-line-clamp: $a;
  overflow: hidden;
}
```
从[8.4版本](https://github.com/postcss/autoprefixer/releases/tag/8.4.0)开始，更改了注释规则。
` autoprefixer: off`会使后面所有的代码都生效。在使用过程中，还是出现以下警告：
```
Second Autoprefixer control comment was ignored. Autoprefixer applies control comment to whole block, not to next rules.
```
需要使用 `autoprefixer: ignore next`
```javascript
.logo {
    /* autoprefixer: ignore next */
    user-select: none; /* ← ignored */
    mask: url(mask.jpg); /* ← will be prefixed */
}
```



### 上下文

>  JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上下文环境的堆栈（context stack）。
>
> 这个堆栈是“后进先出”的数据结构，最后产生的上下文环境首先执行完成，退出堆栈，然后再执行完成它下层的上下文，直至所有代码执行完成，堆栈清空。
>
> Generator 函数不是这样，它执行产生的上下文环境，一旦遇到yield命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行next命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。



###协程
传统的编程语言，早有异步编程的解决方案（其实是多任务的解决方案）。其中有一种叫做"协程"（coroutine），意思是多个线程互相协作，完成异步任务。

求值策略：
"传值调用"
即在进入函数体之前，就计算x + 5的值（等于 6），再将这个值传入函数f。C 语言就采用这种策略。
“传名调用”
即直接将表达式x + 5传入函数体，只在用到它的时候求值。Haskell 语言采用这种策略。



###编程风格
单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个成员以逗号结尾。
```
// bad
const a = { k1: v1, k2: v2, };
const b = {
  k1: v1,
  k2: v2
};

// good
const a = { k1: v1, k2: v2 };
const b = {
  k1: v1,
  k2: v2,
};
```

对象尽量静态化，一旦定义，就不得随意添加新的属性。如果添加属性不可避免，要使用Object.assign方法。

使用扩展运算符（...）拷贝数组。使用 Array.from 方法，将类似数组的对象转为数组。

###编码问题
八个二进制位可以组合出256种状态，即为一个字节

*ASCII码*

> 规定了128个字符的编码，占字节的后7位，首位统一为0.

*非 ASCII 码*

> 其他语言仅128个符号编码不足以表示，故规定0-127表示的符号一样，利用首位来编入新符号

*Unicode*

> 多种编码方式的集合，所有符号，每个符号的编码都不同。但 Unicode 只是一个符号集，只规定了符号的二进制代码，却没有规定这个二进制代码应该如何存储。

*UTF-8*

> Unicode 的一种实现方式，是一种变长的编码方式。它可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。

编码规则：
1）对于单字节的符号，字节的第一位设为0，后面7位为这个符号的unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的。
2）对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的unicode码。

所以说：如果一个字节的第一位是0，则这个字节单独就是一个字符；如果第一位是1，则连续有多少个1，就表示当前字符占用多少个字节。



### URI解码

| 类型       | 包含                         |
| :--------- | :--------------------------- |
| 保留字符   | `; , / ? : @ & = + $`        |
| 非转义字符 | 字母数字 `- _ . ! ~ * ' ( )` |
| 数字符号   | `#`                          |



encodeURI()

> 对传入字符串中的所有非（基本字符、Mark字符和保留字符）进行转义编码。所有的需要转义的字符都按照UTF-8编码转化成为一个、两个或者三个字节的十六进制转义字符。
> 解码：decodeURI()

encodeURI 自身无法产生能适用于HTTP GET 或 POST 请求的URI，例如对于 XMLHTTPRequests, 因为 "&", "+", 和 "=" 不会被编码，然而在 GET 和 POST 请求中它们是特殊字符。然而encodeURIComponent这个方法会对这些字符编码。

encodeURIComponent()

> 可把字符串作为 URI 组件进行编码，处理方式和encodeURI只有一个不同点，那就是对于保留字符同样做转义编码。例如，字符`:`被转义字符`%3A`代替 
> 对应解码 decodeURIComponent()

区别：encodeURI可以用来对完整的URI字符串进行编码处理。而encodeURIComponent可以对URI中一个部分进行编码，从而让这一部分可以包含一些URI保留字符。

比如下面的URI字符串： 
`www.a.com/b.aspx?url=http://www.c.com/d.html `
在这个URI字符串中。a.aspx页面会创建HTML格式的邮件内容，里面会包含一个链接，这个链接的地址就是上面URI字符串中的url值。显然上面的url值是URI中的一个部分，里面包含了URI保留关键字符。我们必须调用encodeURIComponent对它进行编码后使用，否则上面的URI字符串会被浏览器认为是一个无效的URI。

还有个例子：
为了避免服务器收到不可预知的请求，对任何用户输入的作为URI部分的内容你都需要用encodeURIComponent进行转义。比如，一个用户可能会输入`Thyme &time=again`作为comment变量的一部分。如果不使用encodeURIComponent对此内容进行转义，服务器得到的将是comment=Thyme%20&time=again`。请注意，"&"符号和"="符号产生了一个新的键值对，所以服务器得到两个键值对（一个键值对是comment=Thyme，另一个则是time=again），而不是一个键值对。所以，对于 application/x-www-form-urlencoded (POST) 这种数据方式，空格需要被替换成 '+'，所以通常使用 encodeURIComponent 的时候还会把 "%20" 替换为 "+"。



##模块

**模块系统的演进**

最原始的 JavaScript 文件加载方式，每个文件`<script>`视作一个模块，接口暴露在全局作用域下，即 window 对象中。
它的弊端有：

- 全局作用域下易造成变量冲突
- 文件只能按照标签书写顺序进行加载
- 需主观解决模块和代码库的依赖关系



###CommonJS
服务器端的 Node.js 遵循的 CommonJS 规范，是以在浏览器环境之外构建 JavaScript 生态系统为目标而产生的项目，比如在服务器和桌面环境中，后作为一套规范。这一规范是为了解决 JavaScript 的作用域问题而定义的模块形式，可以使每个模块它自身的命名空间中执行，模块必须通过`module.exports`导出对外的变量或接口，通过`require()`来导入其他模块的输出到当前模块作用域中。
CommonJS 就是同步加载模块，在浏览器端是将所有模块都定义好并通过`id`索引

优点：
1.服务器端模块便于重用
2.NPM 含大量可使用模块包
3.简单易用
缺点：
1.同步模块加载方式不适合在浏览器环境，它意味着阻塞加载，而浏览器资源是异步加载
2.不能非阻塞的并行加载多个模块
实现：
服务器端的`Node.js`
`Browserify`浏览器端的 CommonJS实现，打包体积较大
等等。。

###AMD
AMD（Asynchronous Module Definition）是异步模块定义，是为浏览器环境设计的，因为 CommonJs 模块是同步加载的，而浏览器环境还没有准备好同步加载模块的条件。AMD 定义一套 JavaScript 模块依赖异步加载标准，来解决同步加载问题。
规范只有一个主要接口
`define(id?: String, dependencies?: String[], factory: Function|Object)`。它要在声明模块的时候指定所有的依赖 dependencies，并且还要当做形参传到 factory 中，对于依赖的模块提前执行，依赖前置。

优点：

- 适合在浏览器环境中异步加载模块
- 可以并行加载多个模块

缺点：

- 提高开发成本，代码的阅读和书写比较困难
- 不符合通用的模块化思维方式

实现方式：
RequireJS
curl

###CMD
Common Module Definition 和 AMD 相似，与 CommonJS 和 Node.js 的 Modules 规范保持了很大的兼容性。

优点：
依赖就近，延迟执行
可以很容易在 Node.js 中运行
缺点：
依赖 SPM 打包，模块的加载逻辑偏重
实现：
Sea.js
coolie

###UMD
Universal Module Definition 规范类似于兼容 CommonJS 和 AMD 的语法糖，是模块定义的跨平台解决方案。

###ES6 模块

ECMAScript6 标准增加了 JavaScript 语言层面的模块体系定义。ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。
```
import "jquery";
export function doStuff() {}
module "localModule" {}
```
优点：
容易进行静态分析
面向未来的 ECMAScript 标准
缺点：
原生浏览器端还没有实现该标准
全新的命令字，新版的 Node.js才支持
实现：
Babel

对于模块加载和传输
所有模块都单独请求会造成请求次数过多，应用启动速度慢
把所有模块打包成一个文件然后只请求一次，会导致初始化过程偏慢，流量浪费。
分块传输 按需进行懒加载，在实际用到时再增量更新
要实现按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。

而且，模块并不仅仅是 JavaScript，还有样式、图片、字体、HTML 模板等众多资源，以各种形式存在，比如`coffeescript`、`less`、`sass`等等

静态分析
在编译的时候对整个代码进行静态分析，分析出各个模块的类型和依赖关系，然后将不同类型的模块提交给适配的加载器来处理，比如一个用 LESS 写的样式模块，可以先用 LESS 加载器转成一个 CSS 模块，再通过 CSS 模块把他插入到页面的`<style>`标签中执行。
webpack 就是这个作用





CommonJS: 用于服务器
AMD: 用于浏览器
ES6 模块 Module
Module 的设计思想是静态化，在编译时确定模块的依赖关系及输入输出的变量。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。
前两者只能在运行时确定这些东西，即“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”。

ES6 与 CommonJs模块的差异：
CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
```javascript
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```



ES6模块自动采用严格模式

模块之中，顶层的this关键字返回undefined，而不是指向window。也就是说，在模块顶层使用this关键字，是无意义的。
CommonJS 模块的顶层this指向当前模块，这是两者的一个重大差异。
`arguments/require/module/exports/_filename/_dirname`等顶层变量在ES6的模块也不存在

export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。



```javascript
// 报错
export 1;

// 报错
var m = 1;
export m;
//因为没有提供对外的接口。第一种写法直接输出 1，第二种写法通过变量m，还是直接输出 1。1只是一个值，不是接口。

// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```



export命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，import命令也是如此。这是因为处于条件代码块之中，就没法做静态优化了，违背了 ES6 模块的设计初衷。

import命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。。但是，如果是一个对象，改写他的属性是允许的。

import命令具有提升效果，会提升到整个模块的头部，首先执行。其本质是，import命令是编译阶段执行的，在代码运行之前。

由于import是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。

多次重复执行同一句import语句，那么只会执行一次，而不会执行多次。

使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。
可以用到`export default`命令，为模块指定默认输出。命令也可以用在非匿名函数前。且一个模块只能有一个默认输出，因此`export default`只能使用一次，所以此时 `import`命令可以不用加大括号



```javascript
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;
//此时foo函数的函数名foo，在模块外部是无效的。加载的时候，视同匿名函数加载。
```



其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。

本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。所以，下面的写法是有效的。

```javascript
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```



正是因为export default命令其实只是输出一个叫做default的变量，将后面的值，赋给default变量,所以它后面不能跟变量声明语句,但可以直接写一个值
```javascript
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;
//将变量a的值赋给变量default

// 错误
export default var a = 1;

// 正确
export default 42;

// 报错
export 42;
```

`script`标签中defer与async的区别是：defer要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；async一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，defer是“渲染完再执行”，async是“下载完就执行”。另外，如果有多个defer脚本，会按照它们在页面出现的顺序加载，而多个async脚本是不能保证加载顺序的。

Node加载模块
Node有自己的 CommonJS 模块格式，与ES6模块格式不兼容。

试验阶段要求ES6模块采用`.mjs`后缀，当脚本使用`import`或`export`，那就要`.mjs`后缀名，`require`命令不能加载这种文件，只有`import`才可以加载，同样，`.mjs`文件内也不能使用`require`命令
对于后缀名，在未写的情况下，会依次用` a.mjs`/`a.js`/`a.json`/`a.node`来查找，最后以`a/pageage.json`的`main`字段指定脚本，没有 main 则加载`a/index`,index也以上述mjs,js,json,node查找，再没有就抛出错误




###CommonJS模块
CommonJS 的输出都定义在`module.exports`属性上，Node 会自动将`module.exports`作为默认输出，等同于`export default xxx`,而`import`输入的实际上是`{ default: module.exports }`.
```javascript
//取到 module.exports 的写法
// 写法一
import baz from './a';
// baz = {foo: 'hello', bar: 'world'};

// 写法二
import {default as baz} from './a';
// baz = {foo: 'hello', bar: 'world'};

// 写法三
import * as baz from './a';
// baz = {
//   get default() {return module.exports;},
//   get foo() {return this.default.foo}.bind(baz),
//   get bar() {return this.default.bar}.bind(baz)
// }
```
对于第3种写法，baz 本身是一个对象，可以通过baz.default拿到module.exports




由于 ES6 模块是编译时确定输出接口，CommonJS 模块是运行时确定输出接口，只有在运行时才能确定readFile接口。所以采用import命令加载 CommonJS 模块时，不允许采用下面的写法。
```javascript
// 不正确
import { readFile } from 'fs';
// 正确的写法一
import * as express from 'express';
const app = express.default();

// 正确的写法二
import express from 'express';
const app = express();
```



###CommonJS 模块加载 ES6 模块

不能使用`require`命令，而要使用`import()`函数，ES6 模块的所有输出接口，会成为输入对象的属性。
```JavaScript
// es.mjs
let foo = { bar: 'my-default' };
export default foo;

// cjs.js
const es_namespace = await import('./es.mjs');
// es_namespace = {
//   get default() {
//     ...
//   }
// }
```

“循环加载”（circular dependency）指的是，a脚本的执行依赖b脚本，而b脚本的执行又依赖a脚本。表示存在强耦合，如果处理不好，还可能导致递归加载，使得程序无法执行，因此应该避免出现。

CommonJS 的一个模块，就是一个脚本文件。require命令第一次加载该脚本，就会执行整个脚本，然后在内存生成一个对象。
```JavaScript
{
  id: '...',            //模块名
  exports: { ... }, //模块输出的各个接口
  loaded: true,     //布尔，表示脚本是否执行完毕
  ...
}
```
一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。