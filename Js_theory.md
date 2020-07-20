[TOC]



## 函数式编程

函数式编程是一种编程范式，也就是如何编写程序的方法论。以函数为主要载体，去拆解、抽象一般的表达式。其中有命令式和声明式

命令式是编写具体的指令去让计算机执行一些动作，这往往会涉及到很多细节。而声明式以为这要写表达式，而不是具体的指示。它指明做什么，而不是怎么做，且不需要关注它内部的实现。



声明式的特点：

+ 语义更清晰
+ 可复用性更高，以函数为调用单位
+ 可维护性更好，关注表达式内部的实现，容易定位 bug
+ 作用域局限导致副作用少



函数式的特点

+ 函数式”第一等公民“

  函数与其他数据类型一致，比如可以存在数组里，当做参数传递赋值等

+ 只使用表达式，不用语句

  表达式是一个单纯的运算过程，总是有返回值，而语句是执行某种操作，没有返回值。函数式编程要求尽可能地使用表达式，不使用语句。

+ 没有副作用

  保持独立性，每次返回新的值，而不是修改外部变量的值

+ 不修改状态

+ 引用透明

  函数的运行不依赖于外部变量或状态，只依赖输入的参数，如果参数相同，那么引用函数所得到的的返回值总是相同的。这种明确的依赖性，更易于观察理解，更容易定位bug 的位置



使用函数可以减少代码的重复，从而提高开发效率。

函数式更接近自然语言，容易理解。

函数式不依赖外部状态参数，作为一个独立单元，有利于单元测试以及模块化组合等

由于不修改变量，不必担心死锁（一个线程的数据被另一个线程修改），因此可以将工作分摊到多个线程，部署**并发编程**



## 面向对象编程

将真实世界各种复杂关系抽象成一个个对象，对象之间分工合作。每个对象都能够接收处理数据并发送消息给其他对象。对象也可以复用。用面向对象编程易于维护和开发，具有灵活、代码可复用、模块化的特点

对象有几个特点：

对象具有唯一标识性：即使完全相同的两个对象，也并非同一个对象。

对象有状态：对象具有状态，同一对象可能处于不同状态之下。

对象具有行为：即对象的状态，可能因为它的行为产生变迁。

状态和行为在别的语言可能叫属性和方法

一种面向对象语言需要向开发者提供四种基本能力：
封装 - 把相关的信息（无论数据或方法）存储在对象中的能力
聚集 - 把一个对象存储在另一个对象内的能力
继承 - 由另一个类（或多个类）得来类的属性和方法的能力
多态 - 编写能以多种方法运行的函数或方法的能力



## Event loop(单线程)

> Event Loop是一个程序结构，用于等待和发送消息和事件。ECMAScript没有 event loops，这是在HTML Standard 里定义浏览器何时进行渲染更新。

JavaScript 是一门**单线程语言**。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。所以这个特性是 JavaScript 的核心特征。而HTML5里的 Web Worker标准虽然允许 JavaScript 脚本创建多个线程，但是子线程完全受主线程控制且不得操作DOM，但并没有改变 JavaScript 单线程的本质。

而同步模式、堵塞模式或者说多线程的效率与资源占用都不合理

而Event Loop 就是在程序中设置两个线程：一个负责程序本身的运行，称为"主线程"；另一个负责主线程与其他进程（主要是各种I/O操作）的通信，被称为"Event Loop线程"（可以译为"消息线程"）。

![2013102004](http://www.ruanyifeng.com/blogimg/asset/201310/2013102004.png)



绿色部分，还是表示运行时间，而橙色部分表示空闲时间。每当遇到I/O的时候，主线程就让Event Loop线程去通知相应的I/O程序，然后接着往后运行，所以不存在红色的等待时间。等到I/O程序完成操作，Event Loop线程再把结果返回主线程。主线程就调用事先设定的回调函数，完成整个任务。

这种运行方式称为"异步模式"（asynchronous I/O）。



### 任务队列

所有任务可以分成两种，一种是同步任务，另一种是异步任务。同步任务都在主线程上执行，形成一个执行栈；异步任务指的是，不进入主线程、而进入"任务队列"的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

1. 所有同步任务都在主线程上执行，形成一个执行栈。

2. 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

3. 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

4. 主线程不断重复上面的第三步。

   

回调函数就是那些会被主线程挂起来的代码。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。



"任务队列"是一个先进先出的数据结构，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，"任务队列"上第一位的事件就自动进入主线程。但是，由于存在后文提到的"定时器"功能，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。

**主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop**



Node.js还提供了两个与任务队列有关的方法。`process.nextTick`和`setImmediate`

process.nextTick方法可以在当前"执行栈"的尾部----下一次Event Loop（主线程读取"任务队列"）之前----触发回调函数。也就是说，它指定的任务总是发生在所有异步任务之前。

setImmediate方法则是在当前"任务队列"的尾部添加事件，也就是说，它指定的任务总是在下一次Event Loop时执行，这与setTimeout(fn, 0)很像。

```js
process.nextTick(function A() {
  console.log(1);
  process.nextTick(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 1
// 2
// TIMEOUT FIRED

setImmediate(function A() {
  console.log(1);
  setImmediate(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0);
//运行结果可能是1--TIMEOUT FIRED--2，也可能是TIMEOUT FIRED--1--2。
//setImmediate总是将事件注册到下一轮Event Loop，所以函数A和timeout是在同一轮Loop执行，而函数B在下一轮Loop执行。
```

一个event loop有一个或者多个task队列。

每一个task都来源于指定的任务源，比如可以为鼠标、键盘事件提供一个task队列，其他事件又是一个单独的队列。

一个event loop里只有一个microtask 队列。



**微任务microtask**可以理解是在当前 macrotask 执行结束后立即执行的任务。也就是说，在当前task任务后，下一个task之前，在渲染之前。

包括

+ `process.nextTick` 

+ `promise` 

+ `Object.observe` (废弃)

+ `MutationObserver`

**宏任务macrotask**每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。

浏览器会在每一个 macrotask 执行结束后，下一个 macrotask 执行开始前对页面重新渲染

包括 

+ `script` 
+ `setTimeout` 
+ `setInterval` 
+ `setImmediate` (Node环境)
+ `MessageChannel`
+ `postMessage`
+ `I/O` 
+ `UI rendering`

1. 执行同步代码，这属于宏任务
2. 执行栈为空，查询是否有微任务需要执行
3. 执行所有微任务
4. 必要的话渲染 UI
5. 然后开始下一轮 Event loop，执行宏任务中的异步代码



```js
 async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}
async function async2() {
	console.log('async2');
}

console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0)

async1();

new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');


/*
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout
*/

//setTimeout作为宏任务，会放到下一个事件循环中去。
//Promise中的异步体现在then和catch中，所以写在Promise中的代码是被当做同步任务立即执行的。而在async/await中，在出现await出现之前，其中的代码也是立即执行的。遇到 await，会将 await 后面的表达式执行一遍
async1函数相当于
async function async1() {
	console.log('async1 start');
	Promise.resolve(async2()).then(() => {
    console.log('async1 end');
  })
}
//执行完一个宏任务之后，会去检查是否存在 Microtasks.当所有的 Microtasks 执行完毕之后，表示第一轮的循环就结束了。第二轮循环依旧从宏任务队列开始。此时宏任务中只有一个 setTimeout
```



### Node 事件循环

Node 由 V8引擎解析javascript脚本，然后调用Node API, libuv库负责执行时将不同的任务分配给不同的线程，形成事件循环，再以异步方式将执行结果返回给V8

事件循环分为6个阶段,每个阶段都会维持一个先进先出的可执行回调函数队列，执行这个阶段的任何操作，然后执行这个阶段队列中的回调函数直到队列为空，或者回调函数调用次数达到上限。当满足这两个条件后，事件循环会进入下一个阶段。

+ timers

  执行通过`setTimeout()`和`setInterval()`安排的回调函数

  timers 会指定一个时限，若提供的回调的等待时间超过用户期望运行的事件，会在到达时限后尽早执行，而且系统调度或正在运行的其他回调函数会延迟执行

+ I/O callbacks

  执行关于close callback（如关闭socket调用的回调）， 定时器安排的回调，调用setImmediate()设置的回调中产生的异常后调用的回调函数

+ idle，prepare:

  Node内部使用

+ poll

  检索新的 I/O 事件;执行与 I/O 相关的回调，在这个阶段node会根据实际情况进行堵塞。

  1. 为到达时限的定时器，执行脚本（不准确，其实是在poll队列中轮空时，检查定时器是否到达时限，如果到达了，则回到timers阶段执行定时器回调函数）
  2. 执行poll队列中的事件回调函数

  当事件循环进入poll阶段，并且此时没有设置定时器，将会发生下面两种情况：

  1. 如果poll队列不是空的，事件循环将同步迭代执行队列中的回调函数，直到poll队列为空，或者达到执行上限。
  2. 如果poll队列为空的，将会发生下面两种情况：
     1）如果脚本通过setImmediate()设置，事件循环会结束poll阶段，然后进入check阶段来执行这些脚本。
     2）如果此时没有通过setImmediate()设置的脚本，事件循环将停留在poll阶段，等待回调函数添加到队列中，然后立即执行。

  一旦poll队列为空，事件循环将检查已经达到时限的定时器。如果一个或多个定时器已经准备就绪，时间循环将回到timers阶段，执行这些定时器回调函数。

+ check

  执行由setImmediate()设置的回调函数。

+ close  callbacks

  如socket.on('close', ...)设置的关闭回调



setImmediate()和setTimeout()两者相似，但是调用时机不同。

1. setImmediate()设计用来当当前poll阶段完成是执行脚本。
2. setTimeout()经过给定时间后执行脚本

在主模块中调用，两者执行顺序不确定，在I/O回调中，immediate总是先执行



在一个特定阶段的任何时间调用`process.nextTick()`，所有传入process.nextTick()的回调函数将会在事件循环继续下一个阶段**之前**执行，也就是说在**同一个阶段执行**



### 

## 闭包

[闭包&原型链 参考1](http://www.cnblogs.com/wangfupeng1988/p/3977924.html)

[参考2](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/00143449934543461c9d5dfeeb848f5b72bd012e1113d15000)

> JavaScript允许函数嵌套，并且内部函数可以访问定义在外部函数中的所有变量和函数，以及外部函数能访问的所有变量和函数。但是，外部函数却不能够访问定义在内部函数中的变量和函数。这给内部函数的变量提供了一定的安全性。而且，当内部函数生存周期大于外部函数时，由于内部函数可以访问外部函数的作用域，定义在外部函数的变量和函数的生存周期就会大于外部函数本身。当内部函数以某一种方式被任何一个外部函数作用域访问时，一个闭包就产生了。

(内部函数会将外部函数的活动对象添加到它的作用域链中，当返回内部函数时，其作用域链包括外部函数的活动对象和全局变量对象，所以返回函数可以访问外部函数定义的变量，且外部函数执行后它执行环境中的作用域链已销毁，但其活动对象不会销毁，因为返回函数作用域链仍在引用这个活动对象)

**作用**

> 保存自己的私有变量，通过提供的接口方法提供给外部使用，但外部不能直接访问该变量

**原理**

作用域问题

JS垃圾回收机制

> Js 执行函数时会创建一个作用域对象（每次函数执行时都会创建新的特定的作用域对象，且无法被访问），用来保存函数所创建的局部变量。它和被传入的变量一起被初始化。那么当闭包返回一个内部函数时，Js 的垃圾回收器会回收外部函数所创建的作用域对象，但是返回的内部函数却保留一个指向该作用域对象的引用。所以该作用域对象不会被回收，直到指向的引用计数为零。作用域对象组成了一个名为作用域链。闭包就是一个函数和被创建的函数中的作用域对象的组合。
>
> JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。



内存泄露

因为内部函数始终保存外部函数的活动对象的引用，只要内部函数不被销毁，就不会减少元素的引用数，则其内存永不回收。



## 作用域链

当某一环境执行时，会创建变量对象的作用域链，以保证对执行环境有权访问的所有变量和函数的有序访问。

作用域链的前端始终是当前执行代码所在环境的变量对象，即 arguments 对象。作用域链中下一个变量对象来自包含环境(上一级外部)，在下一个则来自下一个外部，一直延伸到全局执行环境。

try-catch语句中的 catch 块和 with 语句可延长作用域链



## 原型链

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

1. 所有的对象都有`__proto__`属性，该属性 对应 该对象的原型
2. 所有的函数对象都有`prototype`属性，该属性的值会被赋值给该函数创建的对象的`_proto_`属性
3. 所有的原型对象都有`constructor`属性，该属性对应创建所有指向该原型的实例的构造函数
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



## Js设计模式

单例模式

> 产生一个类的唯一实例，用来划分命名空间并将属性、方法组织在一起的对象，用一个变量标识是否被实例化



观察者模式(发布者-订阅者模式)

>对象间一对多的关系，让多个观察者可以同时监听某一个对象，当对象发生改变时，所有依赖于他的对象都可以得到通知



模块模式

> 为单体模式添加私有变量，私有方法来减少全局变量的使用



代理模式

> 本体注重自身代码的实现，而代理负责控制对本体对象的访问，控制以及实例化

### 创建对象

1. 工厂模式

   根据接收的参数构建一个包含信息的对象，可创建多个相似对象，但无法识别对象类型

   ```js
   function createP(name, age, job) {
     var o = new Object();
     o.name = name;
     o.job = job;
     return o
   }
   ```

2. 构造函数模式

   ```js
   function P(name) {
     this.name = name;
   }
   let b = new P();
   ```

   缺点：构造函数中的每个方法，都需要在实例中重建一遍，则会产生不同的F unction 实例，导致不同作用域链与标识符解析

3. 原型模式

   由于 prototype 属性指向包含所有实例共享的属性与方法，因此直接将信息添加到原型对象中

   ```js
   function P() {
     person.prototype.sayName = function() {...}
   }
     //this.name 与 prototype.name 不同
     //前者是建立每个实例的 name 属性，后者使所有实例都访问同一个属性
   ```

   构造函数不等于原型对象

   构造函数P	原型对象P.prototype	实例P1,P2

   P.prototype指向原型对象 P.prototype，而原型对象的 constructor 指向P

   而P1,P2内部属性均执行P.prototype，与P无直接关系

   原型模式的缺点在于共享，在一个实例中修改属性也会影响另一个实例，而且当包含引用类型时也会有问题

4. 组合使用构造函数与原型模式

   构造函数用于定义实例属性，原型模式用于定义方法与共享属性

5. 动态原型模式

   将原型模式用if 语句应用到构造函数中

   ```js
   function P(name, job) {
   	this.name = name;
     ...
     if(typeof this.sayName != 'function') {
       P.prototype.sayName = function() {..};
     }
     //仅在初次调用构造函数时才会执行
   }
   ```

6. 寄生构造函数模式

   在构造函数中应用工厂模式，可用于创建一个具有额外方式的数组

   此类返回的对象与构造函数及原型无关系

   ```js
   function P(name,..) {
     let o = new Object();
     o.name = name;
     return o
   }
   ```

7. 稳妥构造函数模式

### 继承

依据原型链

1. 借用构造函数

   在子类构造函数内部用 apply、call 调用超类构造函数

   ```js
   function sub() {
     sup.call(this, val)
   }
   ```

   缺点：超类方法对于子类未知，且方法均在构造函数中定义，函数无法复用

2. 组合继承

   原型链实现对原型属性、方法的继承，构造函数实现对实例属性的继承

   ```js
   function SuperType(name){
       this.name = name;
       this.colors = ['red','blue','green'];    
   }
   
   SuperType.prototype.sayName = function(){
       alert(this.name);
   }
   
   function SubType(name,age){   
       SuperType.call(this,name);     //第二次调用SuperType() 
       this.age = age;
   }
   SubType.prototype = new SuperType();    //第一次调用SuperType()
   SubType.prototype.constructor = SubType;  
   SubType.prototype.sayAge=function(){
        alert(this.age);
   };
   
   var instance = new SubType('Greg',39);     //调用SubType构造函数，重写原型属性
   instance.colors.push('black');      //重写原型属性
   ```

3. 原型式继承

   创建临时性构造函数，将传入对象 o 作为原型，最后返回新实例，相当于对o的浅复制

   ```js
   function object(o) {
     function F() {}
     F.prototype = o;
     return new F()
   }
   ```

4. 寄生式继承

   创建一个封装继承过程的函数。

   ```js
   function create(o) {
     var clone = object(o);
     clone.attr = function() {}
     return clone
   }
   //基于 o 创建新对象，不仅有 o 的属性方法，也有自己的 attr 方法
   ```

5. 寄生组合式继承

   借用构造函数继承属性，通过原型链的形式继承方法

   ```js
   function inheritPrototype(subType,superType){
       var prototype = object(superType.prototype);    //创建对象
       prototype.constructor = subType;    //增强对象
       subType.prototype = prototype;    //指定对象
   }
   此函数接收两个参数，子类构造函数和超类构造函数。函数内部，第一步创建超类原型对象的一个副本，第二步为副本添加constructor属性，使其指向subType，弥补因为重写原型而失去默认的constructor属性。最后一步，将副本赋值给子类型的原型。整个过程说的简单点，就是将超类原型对象的一个副本复制给子类的原型对象。这样一来就可以避免继承超类的实例属性，也就是避免了在子类原型对象上创建多余的属性了。
   ```

   

## 框架

### MVVM

View	视图

Model	模型

View Model	视图模型

*视图模型*是暴露公共属性和命令的视图的抽象。MVVM没有MVC模式的控制器，也没有MVP模式的presenter，有的是一个*绑定器*。在视图模型中，绑定器在视图和[数据绑定器](https://zh.wikipedia.org/w/index.php?title=%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A%E5%99%A8&action=edit&redlink=1)之间进行通信。

### MVC

View 视图

Model 模型

Controller 控制器

### MCP

View

Model

Presenter	

包含着组件的事件处理，负责检索 Model 获取数据，和将获取的数据经过格式转换与 View 进行沟通。