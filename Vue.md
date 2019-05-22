[TOC]



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

模板语法在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。



**数据绑定:**
使用 “Mustache”语法（双大括号）的文本插值;
`v-once `指令能执行一次性地插值，当数据改变时，插值处的内容不会更新
`v-html="rawHtml"`能将数据解释为HTML，而非纯文本

Vue 不是基于字符串的模板引擎。

模板中除了简单的属性值，也可以包含**单个表达式**，不能使用语句

对于数据绑定模板内的表达式，可采用三种方式：

**1.computed计算属性**
提供的函数将默认用作属性的 getter（必要时可以提供 setter）
且计算属性是基于它们的响应式依赖缓存的，若依赖不发生变化，则返回计算属性值会读取缓存结果
如 `return Date.now() `就不属于响应式依赖，不会及时更新

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
侦听属性，可用于执行异步操作,并在我们得到最终结果前，设置中间状态。这是计算属性无法做到的。且当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

`v-bind`可以绑定 class、添加 style
`v-bind:class=`可以传入数组、对象、{name:布尔判断}
`v-bind:style=`同理



**绑定 class**
`v-bind:class`指令动态切换，且可以和普通 class 属性共存

```html
//对象语法
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

//数组语法
<div v-bind:class="[activeClass, errorClass]"></div>
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}

<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>

```



**绑定 style**

```html
//对象语法
//CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来) 来命名
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
//数组语法
<div v-bind:style="[baseStyles, overridingStyles]"></div>
//vue 也会自动侦测并给需要添加浏览器前缀的 css 属性添加响应前缀
```



**过滤器filter**
过滤器可以用在两个地方：**mustache 插值**和 **v-bind** 表达式。

可以串联`{{ message | filterA | filterB }}`，

也可以接受参数`{{ message | filterA('arg1', arg2) }}`

这里 message 是第一个参数，arg1,arg2依次二三

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



## 生命周期

**beforeCreate**

> 实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性

**created**

> 完成数据data、属性和方法的初始化，$el属性还未显示出来

**beforeMount**

> 编译模板，把data 里面的数据和模板生成 html，但尚未挂载

**mounted**

> 用编译好的 HTML 内容替换 el 属性指向的DOM对象，完成渲染，并进行 ajax 交互

**beforeUpdate**

> 数据更新前调用，发生在虚拟 dom重新渲染和打补丁之前，可以在钩子中进一步更改状态，此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的。不会触发附加的重渲染过程

**updated**

> 在数据更改导致的虚拟DOM重新渲染后调用，调用时，组件DOM已经更新，应尽量避免在此期间更改状态，可能会导致在服务器端渲染器件不被调用

**beforeDestroy**

> 在实例销毁之前调用，实例仍然可用

**destroyed**

> 在实例销毁后调用，所有的事件监听器会被移除，子实例也会被销毁

生命周期中的事件钩子，能在控制Vue实例的过程中形成更好的逻辑



##条件渲染
`v-if`
条件渲染，应用在单一元素或`<template>`包装元素上来切换内部多个元素
后面元素可以配`v-elseif`/`v-else`

为尽可能高效渲染元素，切换过程中会**复用已有元素**而不是从头开始渲染，则元素内部的信息可能不会变化，为独立他们，可以添加`key `来独立开来

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>

  <input placeholder="Enter your email address">
</template>
//因为两个模板使用了相同的元素，<input> 不会被替换掉——仅仅是替换了它的 placeholder
```



`v-show`元素总会被渲染并保留在 DOM 中，只是简单地基于display属性切换，show 不支持 `template`  多元素切换以及 `v-else`
相比`v-show`

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。`v-if`会在切换过程中进行适当地销毁重建，且在条件第一次为真时才会渲染。

所以说，`v-if`有更高的**切换开销**，而`v-show`有更高的**初始渲染开销**，前者适用于运行时条件较少改变，后者适用于频繁切换。



##列表渲染

`v-for `指令需要以` item in items`形式的特殊语法,
还支持一个可选的第二个参数为当前项的索引`(item, index) in items`

**渲染对象**可以是
数组`(value,[index]) in array`、
对象`(value, [key],[index]) in object`
整数，即多次重复模板
遍历对象时是按`Object.keys()`来遍历的，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。

`v-for`相比`v-if`优先级更高，所以`v-if`将分别运行在每个`v-for`循环中，若想有条件的跳过循环的执行，需将 if置于外层元素。



出于高效原因，更新已渲染元素l 列表时采用**就地复用策略**，如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。这很高效，但只适用于**不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。有时候会影响，因此需要用`key`来跟踪节点身份来重用和排序

不要使用对象或数组之类的非原始类型值作为 `v-for` 的 `key`。用字符串或数类型的值取而代之。



对于`push/pop/shift/splice/sort`之类的**变异**方法改变数组是改变原有数组，而`filter/concat/slice`这一类的非变异方法则是返回新数组，而且 vue 也会在最大范围的重用。

当**利用索引**直接设置某一项
`vm.item[index] = newVal`
或直接修改数组的长度时候
`vm.items.length = newLen`
**vue 不会检测到变动**

原因：可以做到，但性能代价和获得的用户体验收益不成正比



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



同样，对于已经创建的实例，**Vue 不能检测对象属性的添加或删除**。
但是，可以使用 `Vue.set(object, key, value) `方法向嵌套对象添加响应式属性。

添加多个属性时应该用两个对象的属性创建一个新的对象。

```js
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})

//下面是错误的
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```



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
`v-on `指令,监听 DOM 事件。

`v-on` 可以接收一个需要调用的方法名称，也可以内联调用方法



```vue
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
为了让methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节，`v-on` 提供了 事件修饰符。通过由点`.`表示的指令后缀来调用修饰符。修饰符可以串联
`.stop`     阻止事件冒泡
`.prevent`  提交事件不再重载页面
`.capture`  使用事件捕获模式,即元素自身触发的事件先在此处理，然后才交由内部元素进行处理
`.self`     事件在该元素本身触发时触发回调
`.once`     事件将只会触发一次
`.passive`   能够提升移动端的性能。



使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。

因此，用 `@click.prevent.self` 会阻止**所有的点击**，而 `@click.self.prevent` 只会阻止对元素自身的点击。



```html
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
<!--不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。-->	
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



**系统修饰键**

实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`

- `.alt`

- `.shift`

- `.meta`

- `.exact`允许你控制由精确的系统修饰符组合触发的事件。

  ```html
  <!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
  <button @click.ctrl="onClick">A</button>
  
  <!-- 有且只有 Ctrl 被按下的时候才触发 -->
  <button @click.ctrl.exact="onCtrlClick">A</button>
  
  <!-- 没有任何系统修饰符被按下的时候才触发 -->
  <button @click.exact="onClick">A</button>
  ```



**鼠标按钮修饰符**

限制处理函数仅响应特定的鼠标按钮。

- `.left`
- `.right`
- `.middle`



##表单控件绑定

`v-model `指令在表单控件元素上创建双向数据绑定。负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

```vue
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```
`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` 特性的初始值而总是将 Vue 实例的数据作为数据来源。

`v-model`
在复选框`type='checkbox'`内，单个勾选框表示逻辑值，多个勾选框表示同一个数组
单选框`type='radio'`绑定的即为字符串



`v-model` 在内部使用不同的属性为不同的输入元素并抛出不同的事件：

- text 和 textarea 元素使用 `value` 属性和 `input` 事件；
- checkbox 和 radio 使用 `checked` 属性和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。



**修饰符**

1. `.lazy` 

   `v-model`在 input 事件触发后就会同步数据，而 lazy 可以转变为在 `change `事件中同步(比如输入法显示问题)
   `<input v-model.lazy="msg" >`

2. `.number`

   将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值）
   `<input v-model.number="age" type="number">`
   因为即使在 `type="number"` 时 HTML 中输入的值也总是会返回字符串类型。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

3. `.trim`

   自动过滤用户输入的首尾空格



##组件

组件是可复用的**Vue 实例**，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el`这样根实例特有的选项。

**组件内的`data`必须是一个函数**,否则可能会影响到其他实例组件

```js
data: function () {
  return {
    count: 0
  }
}
```



**单个根元素**

> 每个组件必须只有一个根元素



**全局组件**

```js
Vue.component('my-component', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

便可以在父实例的模块中以自定义元素`<my-component></my-component> `的形式使用。



**局部注册**

```js
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



组件名可以是短横线分隔命名也可以是首字母大写方式，不过用短横线分隔命名命名时引用组件也必须用驼峰，而用首字母大写方式命名则两种方式都可以用于引用之时

**对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。**

因为 PascalCase 同样适用于 JavaScript。而 HTML 是大小写不敏感的，在 DOM 模板中必须仍使用 kebab-case。

对自定义组件，HTML 本身存在一些限制，`<ul>`/`<ol>`/`<table>`/`<select>`等元素里能包含的元素是有限制的，因此在自定义组件中使用这些受限元素是会出渲染问题的

```html
<table>
  <my-row>...</my-row>
</table>
<!--这个自定义组件 <my-row> 会被作为无效的内容提升到外部，并导致最终渲染结果出错。-->
```
变通的方案是使用特殊的 is 特性：
```html
<table>
  <tr is="my-row"></tr>
</table>
```
**如果我们从以下来源使用模板的话，这条限制是不存在的**：

- 字符串 (例如：`template: '...'`)
- 单文件组件 (`.vue`)
- `<script type="text/x-template">`

而`.vue`组件、JavaScript 内联模板字符串、``是不受限制的



### prop

> 单向下行绑定，防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

注意在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身**将会**影响到父组件的状态。



```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
//指定类型与验证
props: {
  title: String,
  likes: [String, Number], //多个可能的类型
  isPublished: {
      type: Boolean,
      required: true	//必填
    },
  commentIds: {
      type: Number,
      default: 100	//默认值
    },
  // 带有默认值的对象
 	propE: {
  	type: Object,
    // 对象或数组默认值必须从一个工厂函数获取
    default: function () {
    	return { message: 'hello' }
    }
  },
  // 自定义验证函数
  propF: {
    validator: function (value) {
      // 这个值必须匹配下列字符串中的一个
      return ['success', 'warning', 'danger'].indexOf(value) !== -1
    }
  }
  contactsPromise: Promise // or any other constructor
}
//type: String;Number;Boolean;Array;Object;Date;Function;Symbol
//也可以是一个自定义的构造函数，通过 instanceof 来进行检查确认
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```

**传值**

传递对象的两种方式：
字面量语法
`<comp a='1'></comp>`
动态语法
`<comp :a='1'></comp>`
前者会传递字符串'1',而后者会被当做JavaScript表达式计算后传递
props可以对象的形式验证其规格



传布尔值时

可以不传具体值，默认 true

```html
 <!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>

<!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:is-published="false"></blog-post>
```



传递一个对象的所有属性作为 prop ，可以使用不带参数的`v-bind`，(取代 `v-bind:prop-name`)。

```html
<todo-item v-bind="todo"></todo-item>
```



对于绝大多数特性来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 `type="text"` 就会替换掉 `type="date"` 并把它破坏！庆幸的是，`class`和 `style` 特性会稍微智能一些，即两边的值会被合并起来



一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件

JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。



### 自定义事件

事件名不存在任何自动化的大小写转换。且`v-on` 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，推荐使用**短横线分隔命名**命名

`$on(eventName)` 监听事件
`$emit(eventName)` 触发事件
父组件可以在使用子组件的地方直接用` v-on` 来监听子组件触发的事件。不能用`$on`侦听子组件抛出的事件，而必须在模板里直接用`v-on`绑定，且这两个事件并不是`addEventListener`和`dispatchEvent`的别名

`.sync`修饰符
可以自动更新父组件属性

```html
<comp :foo.sync="bar"></comp>
<!--相当于
<comp :foo="bar" @update:foo="val => bar = val"></comp>-->
<!--当子组件需要更新 foo 的值时，它需要显式地触发一个更新事件：
this.$emit('update:foo', newValue)-->
<!--.sync 修饰符的 v-bind 不能和表达式一起使用 (例如 v-bind:title.sync=”doc.title + ‘!’” 是无效的)-->


<text-document v-bind.sync="doc"></text-document>
<!--用一个对象同时设置多个 prop -->
<!--v-bind.sync=”{ title: doc.title }”，是无法正常工作的-->
```



[将原生事件绑定到组件](https://cn.vuejs.org/v2/guide/components-custom-events.html#%E5%B0%86%E5%8E%9F%E7%94%9F%E4%BA%8B%E4%BB%B6%E7%BB%91%E5%AE%9A%E5%88%B0%E7%BB%84%E4%BB%B6)



### 插槽

2.6.0版本中的`v-slot` 取代了 `slot` 和 `slot-scope`

插槽内可以包含任何模板代码，包括 HTML，甚至组件



父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

插槽里使用数据也只能使用模板所在页面的数据，不能提前引用组件内的数据

```html
<navigation-link url="/profile">
  Clicking here will send you to: {{ url }}
  <!--
  这里的 `url` 会是 undefined，因为 "/profile" 是
  _传递给_ <navigation-link> 的而不是
  在 <navigation-link> 组件*内部*定义的。
  -->
</navigation-link>
```



`slot`标签内也可以插入内容作为后备内容，如果外部没有传内容，就会显示

`slot`标签有`name`属性，默认为 default

```html
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>
```

 **v-slot 只能添加在一个 <template> 上**，这一点和已经废弃的 `slot` 不同。

只有一个例外，当被提供的内容只有默认插槽时，组件的标签才可以被当作插槽的模板来使用。这样我们就可以把 `v-slot` 直接用在组件上：

```html
<current-user v-slot:default="slotProps"> 
 <!-- 即<current-user v-slot="slotProps"> -->
  {{ slotProps.user.firstName }}
</current-user>
<!-- 默认插槽的缩写语法不能和具名插槽混用，因为它会导致作用域不明确-->
<!-- 无效，会导致警告 -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里，因此在支持的环境下可以使用解构来传入具体的插槽 prop

```html
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
<!-- 2.6.0支持动态插槽 -->
<template v-slot:[dynamicSlotName]>
  ...
</template>
```



**缩写**

`#`代替`v-slot:`，只在其有参数的时候才可用。 `v-slot:header` 可以被重写为 `#header`



被废弃的 slot 属性可以用在一个普通元素上

`slot-scope` 特性，可以接收传递给插槽的 prop



**动态组件**

当在这些组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。

比如多标签切换，为了解决这个问题，我们可以用一个 `<keep-alive>` 元素将其动态组件包裹起来。

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

`<keep-alive>` 要求被切换到的组件都有自己的名字，不论是通过组件的 `name` 选项还是局部/全局注册。



**异步组件**

> 以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。

```js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
  
	//或者
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
//或者 promise
Vue.component(
  'async-webpack-example',
  // 这个 `import` 函数会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
//局部注册时
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```



访问根实例`this.$root`

访问父组件实例`this.$parent`

访问子组件实例或子元素 用 ref 特性赋予ID引用

### 模块系统

使用了诸如 Babel 和 webpack 的模块系统情况下，推荐创建一个 `components` 目录，并将每个组件放置在其各自的文件中。



## 过渡&动画

Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

```html
<transition name="fade">
  <p v-if="show">hello</p>
</transition>
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```

当插入或删除包含在 `transition` 组件中的元素时，Vue 将会做以下处理：

1. 自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
2. 如果过渡组件提供了 JavaScript 钩子函数，这些钩子函数将在恰当的时机被调用。
3. 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 `nextTick` 概念不同)

6个 class 的切换

`v-enter`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

`v-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

`v-enter-to`: **2.1.8版及以上** 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。

`v-leave`: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

`v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

`v-leave-to`: **2.1.8版及以上** 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，在过渡/动画完成之后移除。

![transition](https://cn.vuejs.org/images/transition.png)

如果使用一个没有名字的 `<transition>`，则 `v-` 是这些类名的默认前缀。如果你使用了 `<transition name="my-transition">`，那么 `v-enter` 会替换为 `my-transition-enter`



CSS 动画用法同 CSS 过渡，区别是在动画中 `v-enter` 类名在节点插入 DOM 后不会立即删除，而是在 `animationend` 事件触发时删除。

**JavaScript 钩子**

[JavaScript 钩子](https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-%E9%92%A9%E5%AD%90)

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```



一个离开过渡的时候另一个开始进入过渡。这是 `<transition>` 的默认行为 - 进入和离开同时发生。

同时生效的进入和离开的过渡不能满足所有要求，所以 Vue 提供了 **过渡模式**

- `in-out`：新元素先进行过渡，完成之后当前元素过渡离开。
- `out-in`：当前元素先进行过渡，完成之后新元素过渡进入。

用 `out-in` 重写之前的开关按钮过渡：

```html
<transition name="fade" mode="out-in">
  <!-- ... the buttons ... -->
</transition>
```



[列表的排序过渡](https://cn.vuejs.org/v2/guide/transitions.html#%E5%88%97%E8%A1%A8%E7%9A%84%E6%8E%92%E5%BA%8F%E8%BF%87%E6%B8%A1)



## 混入mixins

> 混入 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项。

```js
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})

```

当组件和混入对象含有同名选项时，这些选项将以恰当的方式混合。

比如

+ **数据对象**在内部会进行递归合并，在和组件的数据发生冲突时**以组件数据优先。**
+ **同名钩子函数将混合为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。**
+ 值为对象的选项，例如 `methods`, `components` 和 `directives`，将被混合为同一个对象。两个对象键名**冲突时，取组件对象的键值对。**

```js
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  },
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
  }
})
// => "混入对象的钩子被调用"
// => { message: "goodbye", foo: "abc", bar: "def" }
```



## 自定义指令

```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
//单文件
<script>
export default {
  directives: {
    'my-directive': {
      bind: function () {
        // 准备工作
        // 例如，添加事件处理器或只需要运行一次的高耗任务
      },
      update: function (newValue, oldValue) {
        // 值更新时的工作
        // 也会以初始值为参数调用一次
      },
      unbind: function () {
        // 清理工作
        // 例如，删除 bind() 添加的事件监听器
      }
    }
  }
}
</script>
```

有以下几个钩子函数：

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

- `componentUpdated`：指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
- `unbind`：只调用一次，指令与元素解绑时调用。

**参数**

- `el`：指令所绑定的元素，可以用来直接操作 DOM 。
- `vm`：一个对象，包含以下属性：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"`中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
- `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-%E6%8E%A5%E5%8F%A3) 来了解更多详情。
- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。



## 插件

> 插件通常会为 Vue 添加全局功能

1. 添加全局方法或者属性
2. 添加全局资源：指令/过滤器/过渡等
3. 通过全局 mixin 方法添加一些组件选项，如: vue-router
4. 添加 Vue 实例方法，通过把它们添加到 Vue.prototype 上实现。
5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能，如 vue-router



通过全局方法 `Vue.use()` 使用插件。它需要在你调用 `new Vue()` 启动应用之前完成

 会自动阻止多次注册相同插件，届时只会注册一次该插件。



## 响应式原理

> **数据劫持结合发布者-订阅者模式**
>
> MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。

为了实现双向绑定，需要实现几点：

1. 实现数据监听器Observer，能对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者
2. 实现指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，并绑定响应的更新函数
3. 实现 watcher，连接Observer和Compile，能够订阅并受到每个属性变动的通知，执行指令绑定的回调函数，从而更新视图。
4. mvvm 入口函数整合以上三点

Observer核心方法就是`Object.defineProperty`，vue会将实例的`data`选项所有属性遍历，把他们转为`getter/setter`，这就是数据劫持。同时，在 get 函数中添加了各个属性的订阅者 watcher，当给对象某值赋值时就会触发 setter，就可以监听到数据变化。watcher在实例化生成时也会将自己添加到订阅器中。

然后 vue 会有一个消息订阅器 dep，用来收集订阅者(watcher) ，数据变动触发 notify，同时调用对应订阅者的update 方法

compile解析模板指令，将模板中变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据变动，触发dep.notice()通知时，watcher 能调用自身的 update 方法，触发 compile 中的回调方法。收到通知，更新视图。

![data](https://cn.vuejs.org/images/data.png)



缺点：

Vue **不能检测到对象属性的添加或删除**。由于 Vue 会在初始化实例时对属性执行 `getter/setter` 转化过程，所以属性必须在 `data` 对象上存在才能让 Vue 转换它，这样才能让它是响应的。



**Proxy 与Object.defineProperty对比**

`Object.defineProperty` 虽然已经能够实现双向绑定了，但是他还是有缺陷的。

1. 只能对属性进行数据劫持，所以需要深度遍历整个对象
2. 对于数组不能监听到数据的变化

### 异步更新队列

> Vue **异步**执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。

如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的 `Promise.then` 和 `MessageChannel`，如果执行环境不支持，会采用 `setTimeout(fn, 0)` 代替。



`$nextTick()` 返回一个 Promise 对象,因此也可以使用 async/await

```js
methods: {
  updateMessage: async function () {
    this.message = 'updated'
    console.log(this.$el.textContent) // => '未更新'
    await this.$nextTick()
    console.log(this.$el.textContent) // => '已更新'
  }
}
```

**拓**

[JavaScript运行机制](https://link.juejin.im/?target=http%3A%2F%2Fwww.ruanyifeng.com%2Fblog%2F2014%2F10%2Fevent-loop.html)

JS是单线程的，意思就是同一时间只能做一件事情。它是基于事件轮询的，具体可以分为以下几个步骤：

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。

任务队列中主要有两大类，“macrotask”和“microtask”，这两类task会进入任务队列。常见的 macrotask 有 setTimeout、MessageChannel、postMessage、setImmediate；常见的 microtask 有 MutationObsever 和 Promise.then。当主线程上执行的所有同步任务结束后会从任务队列中抽取出所有微任务执行，当微任务也执行完毕后一轮事件循环就结束了，然后浏览器会重新渲染。之后再从队列中取出宏任务继续下一轮的事件循环，值得注意的一点是执行微任务时仍然可以继续产生微任务在本轮事件循环中不停的执行。所以本质上微任务的优先级是高于宏任务的。



下一个事件循环的`Tick`有可能是在当前的`Tick`微任务执行阶段执行，也可能是在下一个`Tick`执行，主要取决于`nextTick`函数到底是使用`Promise/MutationObserver`还是`setTimeout`）



## 代码风格

**组件 data 必须是一个函数**

当 `data` 的值是一个对象时，它会在这个组件的所有实例之间共享。

```js
data () {
  return {
    foo: 'bar'
  }
}
```

**Prop 定义应该尽量详细。**

**不要把 v-if 和 v-for 同时用在同一个元素上。**

需要在每次重渲染的时候遍历整个列表，不如在一个计算属性上遍历

+ 渲染更高效

- 解藕渲染层的逻辑，可维护性 (对逻辑的更改和扩展) 更强。
- 过滤更高效。

为了给样式设置作用域，Vue 会为元素添加一个独一无二的特性，例如 `data-v-f3f3eg9`。然后修改选择器，使得在匹配选择器的元素中，只有带这个特性才会真正生效 (比如 `button[data-v-f3f3eg9]`)。

问题在于大量的元素和特性组合的选择器 (比如 `button[data-v-f3f3eg9]`) 会比类和特性组合的选择器慢，所以应该尽可能选用类选择器。



用 vue 写代码了，核心思想转变为用数据驱动 `view`，不用像`jQuery`时代那样，频繁的操作 DOM 节点。但还是免不了有些场景还是要操作 DOM 的。我们在组件内选择节点的时候一定要切记避免使用 `document.querySelector()`等一系列的全局选择器。你应该**使用`this.$el`或者`this.refs.xxx.$el`的方式来选择 DOM**。这样就能将你的操作局限在当前的组件内，能避免很多问题。

当需要注册一些全局性的事件，比如监听页面窗口的变化`window.addEventListener('resize', this.__resizeHandler)`，声明了之后一定要在 `beforeDestroy`或者`destroyed`生命周期注销它。`window.removeEventListener('resize', this.__resizeHandler)`避免造成不必要的消耗。



## 虚拟dom

virtual dom 通过JS的Object对象模拟DOM中的节点，然后再通过特定的render方法将其渲染成真实的DOM节点

dom diff 则是通过JS层面的计算，返回一个patch对象，即补丁对象，在通过特定的操作解析patch对象，完成页面的重新渲染

实现步骤

- 用JavaScript对象模拟DOM
- 把此虚拟DOM转成真实DOM并插入页面中
- 如果有事件发生修改了虚拟DOM
- 比较两棵虚拟DOM树的差异，得到差异对象
- 把差异对象应用到真正的DOM树上



**比较只会在同层级进行, 不会跨层级比较。**
比较后会出现四种情况：

+ 此节点是否被移除 -> 添加新的节点 
  1. 遍历旧的节点列表，查看每个节点是否还存在于新的节点列表中
  2. 遍历新的节点列表，判断是否有新的节点
  3. 在第二步中同时判断节点是否有移动
+ 属性是否被改变 -> 旧属性改为新属性
+ 文本内容被改变-> 旧内容改为新内容
+ 节点要被整个替换 -> 结构完全不相同 移除整个替换

深度遍历树，将需要做变更操作的取出来

局部更新 DOM

## 其他框架

**Angular**

双向数据绑定原理：

脏数据监测：

> 触发指定事件后进入脏数据监测，会调用`$digest`循环遍历所有的数据观察者，判断当前值是否和先前的值有区别，若监测到变化的话，会调用`$watch`函数，然后再次调用`$digest`循环直到没有变化
>
> 