# Vue

> Vue 实例，是一个使用 Vue 构造函数创建的 JavaScript 对象，创建 Vue 实例时，传递给 Vue

构造函数的参数是一个包含若干属性和方法的对象，被称为 Vue实例选项对象，用于声明所创建的 Vue 实例对象所要挂载的目标元素、data 数据、计算属性、模板/渲染函数、实例方法以及各种生命周期钩子回调函数等选项。创建的 Vue 实例既然是一个 JavaScript 对象，那么也必然拥有属性和方法，这些属性与方法都有前缀 $，以便与代理的 data 属性区分。

`$el`属性，是vue 选项对象中的 el 属性所对应的 DOM 元素。

`$data `属性是 Vue 选项对象中的 data 属性（或工厂函数）经响应式处理后的对象。

`$options` 属性则是经 Vue 构造函数处理过的 Vue 选项对象。



独立构建是指能够将 template 模板或者从 el 挂载元素提取的模板编译成渲染函数的 Vue 库

运行时构建:删除了模板编译的功能，因此无法支持带 template 属性的 Vue 实例选项,只能用 render 选项。但即使使用运行时构建，在单文件组件中也依然可以写模板，因为单文件组件的模板会在构建时预编译为 render 函数。
运行时库主要是为了减少体积，同时强制预编译所有模板，实现前端优化

**结论：**在使用 script 标签引入 Vue.js 的项目中，任意实例选项或组件选项中包含了 template 模板属性或从 el 挂载元素提取的模板时，均需要使用独立构建的 Vue 库。在包含单文件组件的项目中，使用 webpack 打包时已经将单文件组件中的模板预先编译成了渲染函数，因此一般情况下使用运行时构建的 Vue 库就可以了。



当 Vue 选项对象中有 render 渲染函数时，Vue 构造函数将直接使用渲染函数渲染 DOM 树，当选项对象中没有 render 渲染函数时，Vue 构造函数首先通过将 template 模板编译生成渲染函数，然后再渲染 DOM 树，而当 Vue 选项对象中既没有 render 渲染函数，也没有 template 模板时，会通过 el 属性获取挂载元素的 outerHTML 来作为模板，并编译生成渲染函数。

换言之，在进行 DOM 树的渲染时，render 渲染函数的优先级最高，template 次之且需编译成渲染函数，而挂载点 el 属性对应的元素若存在，则在前两者均不存在是，其 outerHTML 才会用于编译与渲染。

```JavaScript
<div class="app1">{{msg}}</div>
<div class="app2">{{msg}}</div>
<div class="app3">{{msg}}</div>

//Js:
new Vue({
  el: '.app1',
  data: {
    msg: 'Hello, Vue.js.'
  },
  template: '<div>Hello, world.</div>',
  render: (h) => h('div', {}, 'Hi, there.')
})
new Vue({
  el: '.app2',
  data: {
    msg: 'Hello, Vue.js.'
  },
  template: '<div>Hello, world</div>'
})

new Vue({
  el: '.app3',
  data: {
    msg: 'Hello, Vue.js.'
  }
})

//渲染结果
Hi,there.
Hello,world.
Hello,Vue.js.
```



在 Vue 生命周期的 `created() `钩子函数进行的 DOM 操作一定要放在` Vue.nextTick() `的回调函数中。原因是什么呢，原因是
在` created() `钩子函数执行的时候 DOM 其实并未进行任何渲染，而此时进行 DOM 操作无异于徒劳，所以此处一定要将 DOM 操作的 js 代码放进` Vue.nextTick() `的回调函数中。与之对应的就是 `mounted `钩子函数，因为该钩子函数执行时所有的 DOM 挂载和
渲染都已完成，此时在该钩子函数中进行任何DOM操作都不会有问题 。
在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的 DOM 结构的时候，这个操作都应该放进` Vue.nextTick() `的回调函数中。



**缩写：**
`v-bind` 指令被用来响应地更新 HTML 属性
`<a v-bind:href="url"></a>`
缩写`<a :href="url"></a>`
`v-on` 指令，用于监听 DOM 事件
`<a v-on:click="doSomething">`
缩写`<a @click="doSomething"></a>`



##数据
在vue 响应式数据对象中，只有当实例被创建时`data`已经存在的属性才是响应式的，如某属性在实例创建后新增，那么后面的改动将不会触发视图更新。



**数据绑定:**
使用 “Mustache”语法（双大括号）的文本插值;
`v-once `指令能执行一次性地插值
`v-html="rawHtml"`能将数据解释为HTML，而非纯文本

对于数据绑定模板内的表达式，可采用三种方式：

**1.computed计算属性**
提供的函数将默认用作属性的 getter（必要时可以提供 setter）
且计算属性是基于依赖缓存的，若依赖不发生变化，则返回计算属性值会读取缓存结果
如 return Date.now() 就不属于响应式依赖，不会及时更新
```JavaScript
computed: {
    // a computed getter
    reversedMessage: function () {
    // `this` points to the vm instance
    return this.message.split('').reverse().join('')}
}
```
计算属性默认只有 geter，必要时可以添加 setter
```JavaScript
computed: {
    fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```
**2.methods**
```
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

对比计算属性，调用方法没有缓存。每一次的重新渲染都会执行函数



**3.watch**
可用于执行异步操作,并在我们得到最终结果前，设置中间状态。这是计算属性无法做到的。

`v-bind`可以绑定 class、添加 style
`v-bind:class=`可以传入数组、对象、{name:布尔判断}
`v-bind:style=`同理



**绑定 class**
`v-bind:class`指令动态切换，且可以和普通 class 属性共存

```JavaScript
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>

<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
//也可以用在计算属性内
```



**过滤器filter**
过滤器可以用在两个地方：**mustache 插值**和 **v-bind** 表达式。可以串联，也可以接受参数

```JavaScript
{{ message | capitalize }}
<div 
v-bind:id="rawId | formatId"></div>

new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```



##条件渲染
`v-if`
条件渲染，应用在单一元素或`<template>`包装元素上来切换内部多个元素
后面元素可以配`v-elseif`/`v-else`

为尽可能高效渲染元素，切换过程中会复用已有元素，则元素内部的信息可能不会变化，为独立他们，可以添加`key `来独立开来

`v-show`元素总会被渲染，只是简单地基于display属性切换，show 不支持 template  多元素切换以及 else
相比`v-show`，`v-if`会在切换过程中进行适当地销毁重建，且在条件第一次为真时才会渲染。所以说，`v-if`有更高的切换开销，而`v-show`有更高的初始渲染开销，前者适用于运行时条件较少改变，后者适用于频繁切换。



##列表渲染

`v-for `指令需要以` item in items`形式的特殊语法,
还支持一个可选的第二个参数为当前项的索引`(item, index) in items`

items 可以是
数组`(value,[index]) in array`、
对象`(value, [key],[index]) in object`
整数，即多次重复模板
遍历对象是按`Object.keys()`来遍历的

`v-for`相比`v-if`优先级更高，所以`v-if`将分别运行在每个`v-for`循环中，若想有条件的跳过循环的执行，需将 if置于外层元素。

出于高效原因，更新已渲染元素l 列表时采用就地复用策略，数据顺序改变并不会影响DOM 元素，但这会影响，因此需要用`key`来跟踪节点身份来重用和排序

对于`push/pop/shift/splice/sort`之类的方法改变数组是改变原有数组，而`filter/concat/slice`则是返回新数组，而 vue 也会在最大范围的重用。

当利用索引直接设置某一项
`vm.item[index] = newVal`
或直接修改数组的长度时候
`vm.items.length = newLen`
vue 不会检测到变动



替换方法：
```JavaScript
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

//$set 是全局方法 Vue.set 的别名
vm.$set(vm.items, indexOfItem, newValue)

//长度
vm.items.splice(newLength)
```



同样，对于已经创建的实例，Vue 不能检测对象属性的添加或删除。
但是，可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性。

对于显示数组的过滤和排序的要求，但又不想改变原始数据，可以使用计算属性，或者使用 method 方法
```JavaScript
<li v-for="n in even(numbers)">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```



##事件处理器
`v-on `指令,可以直接在指令内部写事件处理代码，
也可以在 methods 对象内定义方法
或内联写法
```
  <button v-on:click="say('hi')">Say hi</button>
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```
有时也需要在内联语句处理器中访问原生 DOM 事件。可以用特殊变量 $event 把它传入方法：
**事件修饰符**
为了让methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节，`v-on` 提供了 事件修饰符。通过由点(.)表示的指令后缀来调用修饰符。
`.stop`     阻止事件冒泡
`.prevent`  
`.capture`  使用事件捕获模式
`.self`     事件在该元素本身触发时触发回调
`.once`     事件将只会触发一次
`.passive`   能够提升移动端的性能。
```
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```
**按键修饰符**
监听键盘事件时常见的键值
`.enter`
`.tab`
`.delete` (捕获 “删除” 和 “退格” 键)
`.esc`
`.space`
`.up`
`.down`
`.left`
`.right`
`.ctrl`
`.alt`
`.shift`
`.meta`
`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。
可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
```
// 可以使用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```



##表单控件绑定
`v-model `指令在表单控件元素上创建双向数据绑定
```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```
`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` 特性的初始值而总是将 Vue 实例的数据作为数据来源。

`v-model`
在复选框`type='checkbox'`内，单个勾选框表示逻辑值，多个勾选框表示同一个数组
单选框`type='radio'`绑定的即为字符串
**修饰符**
1.`.lazy``v-model`在 input 事件触发后就会同步数据，而 lazy 可以转变为在 `change `事件中同步
`<input v-model.lazy="msg" >`

2.`.number`将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值）
`<input v-model.number="age" type="number">`
因为即使在 `type="number"` 时 HTML 中输入的值也总是会返回字符串类型。

3.`.trim`自动过滤用户输入的首尾空格

##组件

全局组件`Vue.component('my-component', {template:''})`
便可以在父实例的模块中以自定义元素`<my-component></my-component> `的形式使用。
局部注册
```
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})

```

对自定义组件，HTML 本身存在一些限制，`<ul>`/`<ol>`/`<table>`/`<select>`等元素里能包含的元素是有限制的，因此在自定义组件中使用这些受限元素是会出渲染问题的
```
<table>
  <my-row>...</my-row>
</table>
```
变通的方案是使用特殊的 is 特性：
```
<table>
  <tr is="my-row"></tr>
</table>
```
而`.vue`组件、JavaScript 内联模板字符串
、`<script type="text/x-template">`是不受限制的

组件内的`data`必须是一个函数

组件间的协同关系：
props down：父组件通过此向下传递数据给子组件
events up：子组件通过此给父组件发动消息
```
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 可以用在模板内
  // 同样也可以在 vm 实例中像 “this.message” 这样使用
  template: '<span>{{ message }}</span>'
})
```
子组件也可以`v-bind`直接绑定父组件的数据

如果想把一个对象的所有属性作为 prop 传递，可以使用不带参数的`v-bind`
`<todo-item v-bind="todo"></todo-item>`
此处即绑定了 todo 对象

传递对象的两种方式：
字面量语法
`<comp a='1'></comp>`
动态语法
`<comp :a='1'></comp>`
前者会传递字符串'1',而后者会被当做JavaScript表达式计算后传递
props可以对象的形式验证其规格

事件传递
`$on(eventName)` 监听事件
`$emit(eventName)` 触发事件
父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。不能用$on侦听子组件抛出的事件，而必须在模板里直接用v-on绑定，且这两个事件并不是`addEventListener`和`dispatchEvent`的别名

`.sync`修饰符
可以自动更新父组件属性
```
<comp :foo.sync="bar"></comp>
//相当于
<comp :foo="bar" @update:foo="val => bar = val"></comp>
//当子组件需要更新 foo 的值时，它需要显式地触发一个更新事件：

this.$emit('update:foo', newValue)

```


JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。