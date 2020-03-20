

[TOC]

# React

### JSX

JSX是一个JavaScript语法扩展，可以很好地描述 UI 应该呈现出它应有交互的本质形式。react 考虑到渲染逻辑本质上与其他 UI 逻辑内在耦合，因此将标记与逻辑共同存放在'组件'的松散耦合单元之中

JSX 语法中可以在**大括号**`{}`放置表达式

JSX 本身也是一个表达式，也可以赋值给变量作为参数传入或在函数中返回

JSX 语法更接近 JavaScript 而不是 HTML，因此react dom 使用小驼峰命名



React DOM在渲染输入内容前会进行转义。因此可以有效地防止 XSS 攻击

Babel会把 JSX 转译为`React.createElement()`，或者说，JSX仅仅只是`React.createElement(component, props, ...children)`的语法糖

```react
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```



将 React 元素渲染到根 DOM 节点中：`ReactDOM.render(element, document.querySelector(xx))`

由于 React 元素是不可变的，一旦创建就无法更改其子元素或者属性，所以更新 UI 的唯一方式是创建新元素并传入`ReactDOM.render()`。虽然是创建新元素，但实际上会将元素及子元素与他之前的状态比较，只更新必要的实际改变了的内容。



在 JSX 代码的外面扩上一个小括号，这样可以防止 分号自动插入 的bug.



`for`在JSX 中应该被写作`htmlFor`

`<label htmlFor="namedInput">Name</label>`



JSX 会被编译为`React.createElement`形式，所以必须引用 React 库，包含在 JSX 作用于内

`import React from 'react'`



JSX类型中可以使用点语法

```react
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```



**在运行时选择类型**

不能将通用表达式作为 React 元素类型, 如果想通过通用表达式动态决定元素类型，需要提前赋值给大写字母开头的变量

```react
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 错误！JSX 类型不能是一个表达式。
  return <components[props.storyType] story=		{props.story} />;
    // 正确！JSX 类型可以是大写字母开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```



JSX标签内的字符串字面量，JSX会移除行首尾的空格以及空行，与标签相邻的空行均会被删除，文本字符串之间的新行会被压缩为一个空格



布尔类型、Null 以及 undefined是合法的子元素，但并不会被渲染

### 组件

```react
//class 组件
class Welcome extends React.Componenet {
    render() {
        return <h1>Hello,{this.props.name}</h1>;
    }
}
//函数组件
function Welcome(props) {
    return <h1>Hello,{props.name}</h1>;
}

//组件名以大写字母开头。因为 React 会将以小写字母开头的组件视为原生 DOM 标签
//props 应该是只读的，组件需保护它不被更改
//对于基于用户操作、网络响应或其他变化而动态更改内容的变量应储存在 state
```

```react
//props 继承属性
//state 添加局部状态

class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
//setState 不能直接更新某一状态
//构造函数时唯一可以给 this.state 赋值的地方
this.state.comment = 'hello' //wrong
this.setState({comment:'hello'}) //correct

//一起调用，由于 state 更新可能是异步的，所以不能依赖 this.props 和 this.state 来更新
//prevState 之前的状态，props  为此次更新被应用时的 props
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));

```
由于 state 是组件局部封装的，因此其他组件是无法直接访问的。当然组件也可以选择将 state 作为 props 传递到子组件中，子组件可以在 props 中接收参数，但组件本身是无法知道它是来自于父组件的 state 还是 props，亦或是手动输入的。———— **自顶向下 单向数据流 **



组件之间的包含关系，可以使用`props.children()`来表示哪些未知的可能会添加的子组件。或者定义具体的 props 属性名

```react
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <!--标签内部的内容就会作为 children 传入-->
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}

function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}
function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```



react 将小写字母开头的元素视为 HTML 内置组件，大写字母开头的元素对应着 JavaScript 引入或自定义的组件

对于运行时判定具体类型组件的情况`<components[props.storyType]>错误`，需要将这个类型赋值给一个大写字母开头的变量

```react
const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 错误！JSX 类型不能是一个表达式。
  return <components[props.storyType] story={props.story} />;
}
function Story(props) {
  // 正确！JSX 类型可以是大写字母开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```



`false`, `null`, `undefined`, and `true` 是合法的子元素。但它们并不会被渲染。因此如果需要渲染的话，需要转换为字符串，如`String(true)`



### Class 组件属性

**defaultProps**

函数组件或是class组件都可以设置默认props，props 传 null 的情况下则会取 null 而不是默认值

`MyComponent.defaultProps = {}`

对于非ES6 使用`createReactClass`方法创建的组件，需要在组件中定义`getDefaultProps()`函数

**displayName**多用于调试信息



**state**

对于非ES6 使用`createReactClass`方法创建的组件，需要在组件中定义`getInitialState()`函数

### 实例属性

**props**

**state**



### PropTypes类型检查

```react
import PropTypes from 'prop-types'

Class.propTypes = {} //具体类型可参考文档
Class.defaultProps = {}

//或在 class 内部声明
class A extends React.Component {
    static defaultProps = {}
	static propTypes = {}
}

```



## React 组件生命周期

[速查](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

**挂载**

+ `constructor()`

  构造函数通常用于初始化内部 state；为事件处理函数绑定实例。如果不初始化 state 或不进行方法绑定，则不需要为组件实现构造函数。此阶段**不要调用 setState()**

+ `static getDerivedStateFromProps(props, state)`

  返回一个对象来更新 state

+ `render()`  这是 class 组件中**唯一必须实现**的方法，应该是一个纯函数，即在不修改state情况下每次调用返回相同结果

  返回 React 元素、数组或fragments、Portals、字符串或数值类型、布尔类型或 null

+ `componentDidMount()`

  组件挂载（插入 DOM 树中）后立即调用

**更新**

+ `static getDerivedStateFromProps(props, state)`

  在 render 之前调用，并在初始挂载及后续更新时都会被调用

  函数返回一个对象来更新 state，如果返回 null 则不更新。

  该方法适用于如state值在任何时候都取决于props的用例

+ `shouldComponentUpdate(nextProps, nextState)`

  首次渲染或使用 `forceUpdate()` 时不会调用该方法。此处返回值为 false 时则不会调用 `componentDidUpdate()`

  此方法仅作为性能优化方式存在，不应该依靠此方法阻止渲染，可能会产生bug

+ `render()`

+ `getSnapshotBeforeUpdate()`

  在最近一次渲染输出（提交到DOM节点）之前调用，使得组件能在发生更改之前从 DOM 中获取信息如滚动位置，任何返回值都将作为参数传递到 `componentDidUpdate()`

+ `componentDidUpdate(prevProps, prevState, snapshot)`

  更新后被立即调用。首次渲染并不会执行此方法。

**卸载**

+ `componentWillUnmount()`

**错误处理**

+ `static getDerivedStateFromError(error)`

  在后代组件抛出错误后被调用，将抛出的错误作为参数并返回一个值以更新 state

+ `componentDidCatch(error, info)`

  在'提交'阶段被调用，因此允许执行副作用，通常用于记录错误之类的情况

  

**过时将废弃的生命周期方法**

`componentWillMount` 

在`render()`前调用，在此处调用 `setState()` 不会触发额外渲染

此方法是服务端渲染唯一会调用的生命周期函数



`componentWillReceiveProps(nextProps)` 

在已挂载的组件接收新的 props 之前被调用。可以在此处比较`this.props`和`nextProps`并使用 setState 来执行 state 转换



`componentWillUpdate` 

组件接收到新 props 或 state 时，会在渲染前调用此方法。初始渲染不会调用此方法

不应在此方法中调用 setState以及任何其他操作触发对组件的更新，如果需要在此方法中读取DOM信息（如保存滚动位置）则可以移至`getSnapshotBeforeUpdate()`



`setState(stateChange[, callback])` 将组件 state 的更改排入队列。这是一个**请求**而不是立即更新的命令，他会批量推迟更新，因此在调用后立即读取`this.state`不一定有效。因此可以在`componentDidUpdate`生命周期内读取，或者使用回调函数

`(state, props) => stateChange`



`component.forceUpdate(callback)`

强制组件调用 `render()` 重新渲染，他会跳过`shouldComponentUpdate()`阶段，但子组件会触发正常的生命周期方法



过程：

**Render 阶段**

`constructor`

`getDerivedStateFromProps`  new props -> setState -> forceUpdate

`shouldComponentUpdate()`

`render`

**Pre-commit 阶段**

`getSnapshotBeforeUpdate` 此阶段可以读取DOM

**Commit 阶段**

此阶段可以使用DOM， 运行副作用， 安排更新

`componentDidMount`

`componentDidUpdate`

**卸载**

`componentWillUnmount`

## 事件处理
1.React 绑定属性的命名采用驼峰式,`onClick`,在 HTML 里`onclick`
2.需要传入一个函数作为事件处理函数，而非一个字符串
3.React 不能使用返回 false 的方式阻止默认行为，必须明确使用`preventDefault`

```react
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```



由于类class的方法默认是不会绑定`this`的，因此在 JSX 回调函数中应绑定`this`值，用 bind/箭头函数等

```react
class Toggle extends React.Component {
  constructor(props) {
    super(props)
    this.state = {isToggleOn: true}
    this.handleClick = this.handleClick.bind(this)
  }
  handleClick() {
    this.setState(state => {
      isToggleOn: !state.isToggleOn
    })
    console.log('click',this)
  }
  render() {
    return (
      <button onClick={this.handleClick}>
      	{this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    )
  }
}
  
class LoggingButton extends React.Component {
  // 实验性的 public class fields 语法。此语法确保 `handleClick` 内的 `this` 已被绑定。
  // 注意: 这是 *实验性* 语法。
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}

//也可以在回调中使用箭头函数
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}

//当需要传递额外参数时，React 的事件对象 e 会被作为第二个参数传递。
//如果通过箭头函数的方式，事件对象必须显式的进行传递
//而通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```



## 条件渲染

+ if 语句判断
+ 利用花括号包裹表达式，添加 && 运算符，就近原则， `boolean && expression`条件为 true 时则渲染右侧的元素表达式，为 false 则直接返回 false
+ 三目运算符

对于 React 内的列表，同 Vue 一样需要加上 key。

在 `map()` 方法中的元素需要设置 key 属性。

因为 keys 可以在 DOM 中的某些元素被增加或删除时帮助 React 识别哪些元素发生了变化。

但是元素的 key 只有放在**就近的数组上下文中**才有意义

比如提取出一个`ListItem`组件，应该将 key 保留在数组中的`<ListItem />`元素上，而不是放在组件中具体的`<li`元素上

```react
function ListItem(props) {
  // 不需要在这里指定 key
	return <li>{props.value}</li>
}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
		//key 应该在数组的上下文中被指定
 		<ListItem key={number.toString()}
      				value={number} />
	)
	return (
		<ul>
			{listItems}
		</ul>
	)
}
```



`key`会传递信息给 react，但不会传递给组件，因此如何需要使用`key`属性的值，需要使用其他属性名来显式传递这个值



## 表单

**受控组件**

HTML 中，表单元素如`<input>/<textarea>/<select>`等通常自己维护 state，并根据用户输入进行更新，在 react 中，可变状态通常保存在组件 state 属性中，并通过`setState()`更新。因此可以使用 React 将 state 变成唯一数据源，这样渲染表单的 React 组件还控制用户输入过程中表单发生的操作。将 React 以这种方式控制取值的表单输入元素即为受控组件

对于 file 类型的 input，value 值是只读的，因此它是一个非受控组件



```react
this.setState({
  [name]: value
});
//等同于
var partialState = {};
partialState[name] = value;
this.setState(partialState);
//setState 会自动将部分 state 合并到
```

**状态提升**



## 组合与继承

对于组件无法提前知晓子组件具体内容的情况，可以使用`children` prop 来将子组件传递进去；或者也可以使用其他prop 值

```react
function FancyBorder(props) {
	return (
  	<div className={'FancyBorder FancyBorder-' + props.color}>
    	{props.children}
    </div>
  )
}

function WelcomeDialog() {
  return (
    //此时 <FancyBorder> JSX 标签中所有内容都会作为一个 children传递给组件，然后 FancyBorder 会将 {props.children} 渲染到一个 div 内
  	<FancyBorder color="blue">
    	<h1 className="Dialog-title">
      	Welcome
      </h1>
      <p className="Dialog-message">
      	Thank you!
      </p>
    </FancyBorder>
  )
}
//其他 prop
function TestA(props) {
  return (
    <div>
      <div>
      	{props.left}
      </div>
      <div>
      	{props.right}
      </div>
    </div>
  )
}
function App() {
  return (
  	<TestA 
      left={
        <Contacts />
      }
      right={
        <Chat />
      }
    >
  )
}
```



## Fragment

在项目中有语义化的 HTML 被破坏时，比如列表(`<ol>`,`<ul>`,`<dl>`),可以使用`React Fragments`来组合组件

```react
import React, {Fragment} from 'react'
function ListItem({item}) {
	return (
  	<Fragment>
    	<dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  )
}
function Glossary(props) {
  return (
  	<dl>
      {props.items.map(item => (
      	<ListItem item={item} key={item.id} />
      ))}
    </dl>
  )
}

class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```

当你不需要在 fragment 标签中添加任何 prop 且你的工具支持的时候，你可以使用 **短语法**

```react
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>
  );
}
```



Fragment 目前有**唯一**的属性就是`key`



## Refs

对于 React 典型的数据流中，props 是父子组件交互的唯一方式。但有时候需要可以进行强制修改子组件的操作。Refs 允许访问 DOM 元素或render里创建的元素。

适用场景：

+ 管理焦点，文本选择或媒体播放
+ 出发强制动画
+ 继承第三方 DOM 库



**创建 Refs**

使用`React.createRef()`创建，并通过`ref`属性附加到相应的 React 元素中

```react
class MyComponent extends React.Component {
	constructor(props) {
    super(props)
    this.myRef = React.createRef()
	}
  render() {
    return <div ref={this.myRef} />
  }
}
```



**访问 Refs**

当元素绑定 ref 后，对该节点的引用可以在 ref 的`current`属性中访问

`const node = this.myRef.current`

1. 当 `ref` 用于 HTML 元素时，构造函数中使用`React.createRef()`创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性

   ```react
   class CustomTextInput extends React.Component {
   	constructor(props) {
       super(props)
       this.textInput = React.createRef()
       this.focusTextInput = this.focusTextInput.bind(this)
     }
     focuTextInput() {
       //直接使用原生 API 使输入框获得焦点
       this.textInput.current.focus()
     }
     render() {
       return (
       	<div>
         	<input
             type="text"
             ref={this.textInput} />
           <input
             type="button"
             value="Focus the text input"
             onClick={this.focusTextInput}
           />
         </div>
       )
     }
   }
   ```

   React会在组件挂载时给`current`属性传入 DOM 元素，并在组件卸载时传入 null。

   **Ref 会在 componentDidMount 或 componentDidUpdate 生命周期钩子触发前更新**

2. 当`ref`用于**自定义 class 组件**时，`ref`对象接收组件的挂载实例作为其`current`属性

   ```react
   class AutoFocusTextInput extends React.Component {
     constructor(props) {
       super(props)
       this.textInput = React.createRef()
     }
     componentDidMount() {
       this.textInput.current.focusTextInput()
     }
     render() {
       return (
       	<CustomTextInput ref={this.textInput} />
       )
     }
   }
   //通过获取整个组件并手动调用 focusTextInput 方法来模拟挂载后立即被点击的操作
   ```

   

3. 不能在函数组件上使用 `ref` 属性，因为函数组件没有实例。但可以在函数组件内部使用 ref 属性，只要指向DOM元素或class组件

   ```react
   function MyComp() {
   	return <input />
   }
   class Parent extends React.Component {
     constructor(props) {
       super(props)
       this.textInput = React.createRef()
     }
     render() {
   		return (
         //错误
       	<MyComp ref={this.textInput} />
       )
     }
   }
   //right
   function CustomTextInput(props) {
     // 这里必须声明 textInput，这样 ref 才可以引用它
     let textInput = React.createRef();
     function handleClick() {
       textInput.current.focus();
     }
     return (
       <div>
         <input
           type="text"
           ref={textInput} />
         <input
           type="button"
           value="Focus the text input"
           onClick={handleClick}
         />
       </div>
     );
   }
   ```



**Refs 转发**

当父组件需要引用到子组件中的 DOM 元素时，向子组件添加 ref 并不是一个最优方案，因为这样只能获取组件实例而不是 DOM 节点，而且还在函数组件上无效

因此在 16.3 以后的版本可以使用 ref 转发（将 ref 自动地通过组件传递到其一子组件）

```react
const FancyButton = React.forwardRef((props, ref) => (
  //第二个参数 ref 只在 react.forwardRef 定义组件时存在。常规函数和 class 组件不接收且 props 中也不存在 ref
	<button ref={ref} className="FancyButton">
  	{props.children}
  </button>
))
//直接获取 DOM button 的 ref
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>
//FancyButon 使用 React.forwardRef 来获取传递给他的 ref，然后转发给它渲染的 DOM button 元素
//然后使用 FancyButton 的组件可以获取到底层 DOM 节点 button 的 ref，并在必要时访问

react.forwardRef 接收渲染函数，react devTools使用函数来决定为ref转发组件显示的内容
组件在devTools默认显示为"ForwardRef"
const WrappedComponent = React.forwardRef((props, ref) => {
  return <LogProps {...props} forwardedRef={ref} />;
})

也可以命名渲染函数，则名称包含 "ForwardRef(myFunction)"
const WrappedComponent = React.forwardRef(
  function myFunction(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }
);

或者设置函数的 diaplayName 来包含组件名称
function logProps(Component) {
  class LogProps extends React.Component {
    // ...
  }

  function forwardRef(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }

  // 在 DevTools 中为该组件提供一个更有用的显示名。
  // 例如 “ForwardRef(logProps(MyComponent))”
  const name = Component.displayName || Component.name;
  forwardRef.displayName = `logProps(${name})`;

  return React.forwardRef(forwardRef);
}
```



**回调 Refs**

回调 Refs 可以更精细地控制何时refs 被设置和解除,

不同于 createRef()创建的 ref 属性，他是传递一个函数，**接收组件实例或 HTML DOM 元素作为参数**，在其他地方被存储和访问

```react
class CustomTextInput extends React.Component {
	constructor(props) {
		super(props)
		this.textInput = null
		this.focusTextInput = () => {
			if(this.textInput) {
				this.textInput.focus();
			}
		}
	}
	componentDidMount() {
		this.focusTextInput()
	}
	render() {
		//使用 ref 的回调函数将 text 输入框 DOM 节点的引用存储到 React
		return (
			<div>
				<input
					type="text"
					ref={ref => this.textInput = ref}
				/>
				<input
					type="button"
					value="focus the text input"
					onClick={this.focusTextInput}
				/>
			</div>
		)
	}
}
```

```react
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
//若以这种内联函数方式定义，在更新过程中会被执行两次，第一次传入参数 null，第二次会传入参数 DOM 元素。因为每次渲染时会创建一个新函数实例，所以会清空旧的 ref 并设置新的。
//通过将回调定义成 class 的绑定函数的方式可以避免此问题, 从而只输出1次
```



除了createRef 以及callback ref，老版本还有 string 类型的 ref 被废弃

```js
class MyComponent extends React.Component {
  componentDidMount() {
    this.refs.myRef.focus();
  }
  render() {
    return <input ref="myRef" />;
  }
}
```



**废弃原因**[参考](https://zhuanlan.zhihu.com/p/40462264)

+ 因为当ref定义为 string 时，需要 React 追踪当前正在渲染的组件，在调和阶段， React Element 创建更新的过程中， ref 会被封装成一个闭包函数，等待 commit 阶段被执行，这对 React的性能产生一定影响

+ 当使用 render callback 模式时，使用 string ref 会造成 ref 挂载位置产生歧义

  ```js
  class MyComponent extends Component {
    renderRow = (index) => {
      // string ref 会挂载在 DataTable this 上
      return <input ref={'input-' + index} />;
  
      // callback ref 会挂载在 MyComponent this 上
      return <input ref={input => this['input-' + index] = input} />;
    }
   
    render() {
      return <DataTable data={this.props.data} renderRow={this.renderRow} />
    }
  }
  ```

+ string ref 无法被组合，例如当一个第三方库的父组件已经给子组件传递 ref， 那么我们就无法再在子组件上添加 ref 了， 而callback ref 可解决此问题

  ```js
  /** string ref **/
  class Parent extends React.Component {
    componentDidMount() {
      // 可获取到 this.refs.childRef
      console.log(this.refs);
    }
    render() {
      const { children } = this.props;
      return React.cloneElement(children, {
        ref: 'childRef',
      });
    }
  }
  
  class App extends React.Component {
    componentDidMount() {
      // this.refs.child 无法获取到
      console.log(this.refs);
    }
    render() {
      return (
        <Parent>
          <Child ref="child" />
        </Parent>
      );
    }
  }
  
  /** callback ref **/
  class Parent extends React.Component {
    componentDidMount() {
      // 可以获取到 child ref
      console.log(this.childRef);
    }
    render() {
      const { children } = this.props;
      return React.cloneElement(children, {
        ref: (child) => {
          this.childRef = child;
          children.ref && children.ref(child);
        }
      });
    }
  }
  
  class App extends React.Component {
    componentDidMount() {
      // 可以获取到 child ref
      console.log(this.child);
    }
    render() {
      return (
        <Parent>
          <Child ref={(child) => {
            this.child = child;
          }} />
        </Parent>
      );
    }
  }
  ```

+ 在根组件上使用无法生效

+ 对于静态类型较不友好，当使用 string ref 时， 必须显式声明 refs的类型，无法完成自动推导

+ 编译器无法将 string ref 与其refs 上对应的属性进行混淆， 而是用 callback ref 可被混淆

  ```js
  /** string ref，无法混淆 */
  this.refs.myRef
  <div ref="myRef"></div>
  
  /** callback ref, 可以混淆 */
  this.myRef
  <div ref={(dom) => { this.myRef = dom; }}></div>
  
  this.r
  <div ref={(e) => { this.r = e; }}></div>
  ```



createRef 与 callback ref 相比，更加直观，性能上可能会有微小优势，同时对于上面讲的组合问题，createRef 也是没办法的

## Context

在组件间共享某类值的方式。因为某些值是很多组件都需要的，如果使用 props逐层传递那就相当繁琐，所以 context 可以帮助将值深入传递

```react
//比如传递一个主题
const ThemeContext = React.createContext('light')	// light 为默认值
class App extends React.Component {
  render() {
    return (
      //使用 Provider 将当前的 theme 传递给后面无论多深的组件
    	<ThemeContext.Provider value="dark">
      	<Toolbar />
      </ThemeContext.Provider>
    )
  }
}

//此时就不用传递 theme 了
function Toolvar(props) {
	return (
  	<div>
    	<ThemedButton />
    </div>
  )
}

class ThemedButton extends React.Component {
  //指定 contextType 读取当前的 theme context
  // React 会往上找到最近的 theme provider 并使用它的值
  static contextType = ThemeContext
  render() {
    return <Button theme={this.context} />
  }
}
```

除了 context 方法，还可以**将底层组件作为属性直接传递**。这样可以减少要传递的 props 数量，但对高层组件来说会变更复杂，因为将逻辑提升到组件数的最高层次来处理，需要考量。



**API**

**创建 Context 对象**

`const MyContext = React.createContext(defaultValue);`

若没有匹配到 provider, defaultValue才会生效。
将 `undefined` 传递给 provider, defaultValue 并不会生效



**返回 Provider React组件允许子组件订阅 context 变化**

`<MyContext.Provider value={/* 某个值 */}>`

一个 provider 可以支持多个消费组件

provider 也可以嵌套使用，内层覆盖外层

provider value 值变化时新旧值得检测使用了与`Object.is`相同的算法

Provider及内部的 consumer 组件不受制于 `shouldComponentUpdate`函数，因此当provider 的 value 值发生变化时，即使祖先组件退出更新， consumer组件也能更新

**访问 context**

`Class.contextType = MyContext`

挂载在类组件 class 上的 contextType 属性会被重新赋值为一个由`React.createContext()`创建的 context 对象。然后可以通过`this.context`来消费最近的 Context 上的值，并在**任何生命周期**中访问。包括 render

```react
class MyClass extends React.Component {
  //第二种方法
  //实验性语法可以用 static 这个类属性来初始化 contextType
  static contextType = MyContext;
  render() {
    let value = this.context;
    /* 基于这个值进行渲染工作 */
  }
}
MyClass.contextType = MyContext;
```



**订阅 context 变更**

```react
<MyContext.Consumer>
  {value => /* 基于 context 值进行渲染*/}
</MyContext.Consumer>
```



## Render Props

`render prop`是指在 React 组件之间使用一个值为函数的 prop 共享代码的技术， 也可以说是一个用于告知组件需要渲染什么内容的函数 prop

```react
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)} />
// 组件接收一个函数，返回一个 react 元素并调用它而不是实现自己的渲染逻辑

// example
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>移动鼠标!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```



并不一定要用名为`render`的prop来使用这种模式，**任何被用于告知组件需要渲染什么内容的函数 prop 在技术上都可以被称为 render prop

```react
<Mouse children={mouse => (
  <p>鼠标的位置是 {mouse.x}，{mouse.y}</p>
)}/>
```





## 错误边界

> 一个特殊的 React 组件，可以捕获并打印**子组件内**的 JavaScript 错误并渲染出备用 UI。
>
> 他会在渲染期间、生命周期方法和整个组件数的构造函数中捕获错误.
>
> 他无法捕获自身的错误，如果一个错误边界无法渲染错误信息，则错误会冒泡至最近的上层错误边界
>
> 如果 class 组件中定义了`static getDerivedStateFromError()`或`componentDidCatch()` , 它也就变成一个错误边界

[文档](https://react.docschina.org/docs/error-boundaries.html)

+ 事件处理
+ 异步代码（如 setTimeout 或 requestAnimationFrame回调
+ 服务端渲染
+ 自身抛出来的错误

## 高阶组件 HOC

> 以参数为组件并返回新组件的函数

避免在 render 方法中使用 HOC

因为 diff 算法使用组件标识来确定他是应该更新现有子树还是将其丢弃并挂载新子树。如果从 render 返回的组件与前一个渲染中相同（===），则react通过将子树与新子树进行区分来递归更新子树，否则完全卸载前一个子树。在render中使用HOC则会在每次render时创建新的HOC，引起性能问题

## Diffing 算法

O(n)启发式算法

React 在对比两棵树时，会先比较两棵树的根节点

1. 当根节点为**不同类型的元素**时，React 会拆卸原有树并建立起新的树，拆卸时对应的DOM节点被销毁，组件执行`componentWillUnmount()`方法，新树对应的DOM节点会被创建插入到DOM，组件执行`componentWillMount()`、`componentDidMount()`,跟之前树相关联的state也会被销毁
2. 当根节点为相同类型的元素时，会保留 DOM 节点，仅**对比并更新**有改变的属性。组件更新时，实例保持不变，因此 state 在跨越不同的渲染时保持一致，组件执行`componentWillReceiveProps()`、`componentWillUpdate()`，react 会更新实例的 props 以和最新的元素保持一致
3. 处理完当前节点后继续对子节点递归。此时 React 会同时遍历两个子元素的列表，产生差异时生成一个 mutation。

当元素拥有`key`时，React通过key来匹配子元素，可以将之前的低效变得更加高效，数组下标作为key在元素不进行重新排序时比较合适，但顺序有修改时， diff 则会变得很慢，比如修改顺序时修改当前key，会导致非受控组件的state(比如输入框)可能相互篡改导致无法预期的变动



`key`应该具有稳定、可预测以及列表内唯一的特点，不稳定会导致许多组件实例和DOM节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失



## 类型检查 propTypes

`import PropTypes from 'prop-types';`

`PropTypes` 提供一系列验证器，可用于确保组件接收到的数据类型是有效的。

```react
class Greeting extends React.Component {}
Greeting.propTypes = {
  name: PropTypes.string
};
```

**限制单个元素**

可以通过 `PropTypes.element` 来确保传递给组件的 children 中只包含一个元素。

**默认 prop 值**

可以通过`defaultProps`来定义 props 的默认值

```
// 指定 props 的默认值：
Greeting.defaultProps = {
  name: 'Stranger'
};
```



## 动态加载 | 代码分割

`React.lazy()`可以定义一个动态加载的组件，有助于缩小 bundle 的体积，并延迟加载组件

这一特性需要 JS 环境支持 promise

lazy 接受一个函数，必须返回一个promise, resolve 一个 default export (默认导出) 的 react 组件，如果模块使用命名导出， 可以创建一个中间模块来重新导出为默认模块，以保证 tree shaking 不会出错，并且不必引入不需要的组件

```react
// ManyComponents.js
export const MyComponent = ...
export const MyUnusedComponent = ..

// SomeCompoent.js
export { MyComponent as default } from './ManyComponents.js'

// 这个组件是动态加载的
const SomeComponent = React.lazy(() => import('./SomeComponent'));		
```

但lazy和suspense暂不支持服务端渲染



**React.Suspense**

`React.Suspense` 可以指定加载指示器（loading indicator），以防其组件树中的某些子组件尚未具备渲染条件。目前，懒加载组件是 `<React.Suspense>` 支持的**唯一**用例：

```react
// 该组件是动态加载的
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    // 显示 <Spinner> 组件直至 OtherComponent 加载完成
    <React.Suspense fallback={<Spinner />}>
      <div>
        <OtherComponent />
      </div>
    </React.Suspense>
  );
}
// fallback 属性接收任何在组件加载过程中想展示的 react 元素
```



代码分割是由如webpack、rollup和browserify等打包器支持，能够创建多个包并在运行时动态加载，可以'懒加载'用户所需要的内容，显著提高应用性能，在初期加载时候减少所需加载的代码量



react中的动态`import()`语法也是代码分割的一种方式



## Portals

Portal是一种将子节点渲染到存在于父组件以外的 DOM 节点的方案

`React.createPortal(child, container)`

child即任何可渲染的react子元素

container即一个DOM元素

典型用例是当父组件有`overflow: hidden`或`z-index`的样式时需要子组件跳出来

```react
render() {
  // React 并*没有*创建一个新的 div。它只是把子元素渲染到 `domNode` 中。
  // `domNode` 是一个可以在任何位置的有效 DOM 节点。
  return ReactDOM.createPortal(
    this.props.children,
    domNode
  );
}
```



## Profiler 

profiler 是用于测量渲染 react 组件多久渲染一次以及渲染一次的代价，识别出应用中渲染较慢的部分

```react
render(
  <App>
    <Profiler id="Navigation" onRender={callback}>
      <Navigation {...props} />
    </Profiler>
    <Main {...props} />
  </App>
);
//两个参数，一个是id (string)
//一个是组件提交更新时候被调用的回调函数 onRender

function onRenderCallback(
  id, // 发生提交的 Profiler 树的 “id”
  phase, // "mount" （如果组件树刚加载） 或者 "update" （如果它重渲染了）之一
  actualDuration, // 本次更新在渲染Profiler和它子代上花费的
  baseDuration, // 估计不使用 memoization 的情况下渲染整颗子树需要的时间
  startTime, // 本次更新中 React 开始渲染的时间
  commitTime, // 本次更新中 React committed 的时间
  interactions // 属于本次更新的 interactions 的集合
) {
  // 合计或记录渲染时间。。。
}
```



## Hook

**引入 Hook 的原因**

1. 组件之间复用状态逻辑很难，使用 `render props`和高阶组件往往需要重新组织组件结构
2. 复杂组件难以理解， 而 Hook 可以将组件中相互关联的部分拆分成更小的函数，而非强制按照生命周期划分
3. `class` 中的 `this` 工作方式、无法被很好的压缩等因素，需要一个更易于优化的 api



Hook 是一种可以在函数组件里"钩入" React state 及生命周期等特性的**函数**，有 State Hook 的 `useState`  以及 Effect Hook `useEffect`，

`useEffect`给函数组件增加了诸如数据获取、订阅、手动修改DOM等副作用的能力，与 class 组件中 `componentDidMount`/`componentDidUpdate`/`componentWillUnmount`具有相同的用途，不过合并成一个 API， React 会在每次渲染后调用副作用函数，包括第一次渲染时。而且副作用函数也可以通过返回一个函数来指定如何清除副作用。

Hook 只能在函数最外层调用，不能在循环、条件判断或者子函数中调用

Hook 只能在 React 的函数组件中调用 Hook，不能在其他 javascript 函数中调用

对于自定义 Hook， 是一种复用状态逻辑的方式，而不复用 state 本身，因此每次调用时都拥有一个完全独立的 state，不论是不同组件里的同一 Hook, 还是单组件内的多次调用同一 Hook，都不是引用同一 state



### stateHook

以前的函数组件是一种“无状态组件”。通过 Hook 引入了使用 React state的能力，使得“函数组件”更为名副其实。

`useState` 是允许你在 React 函数组件中添加 state 的 Hook。

`useState` 的返回值是 当前 state 以及更新 state 的函数，而且更新 state 的函数不同于`this.setState`，他是**替换而不是合并**

```react
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
  };
}
    
import React, { useState } from 'react';
function Example() {
  // 声明一个叫 “count” 的 state 变量
  const [count, setCount] = useState(0);
    return (
     <div>
       <p>You clicked {count} times</p>
       <button onClick={() => setCount(count + 1)}>
        Click me
       </button>
     </div>
   );
}
// useState 与 this.state 提供的功能完全相同，定义一个 state 变量，名叫 count
// useState 唯一的参数就是初始 state，且不一定是对象，可以是数字或字符串
// 在读取时也不需要 this.state.count，而可以直接使用 count
```



### Effect Hook



## 其他

### 严格模式

`<React.StrictMode></React.StrictMode>`

会在开发模式中对子元素及所有后代元素都进行检查

+ 识别不安全的生命周期（包括使用的第三方库）

+ 使用过时字符串 ref API 的警告

+ 使用废弃 findDOMNode 的警告

+ 检测意外的副作用

  react 有两个工作阶段

  - 渲染阶段确定需要进行哪些更改，比如DOM，此时调用`render` 将结果与上次渲染结果比较

  - 提交阶段发生在当应用变化时（ React 插入、更新及删除DOM 节点的时候，此阶段会调用一系列生命周期方法。

    ```
    constructor
    componentWillMount
    componentWillReceiveProps
    componentWillUpdate
    getDerivedStateFromProps
    shouldComponentUpdate
    render
    setState
    这些阶段会被多次调用，因此需要避免在内部编写副作用相关的代码
    而严格模式虽然不能自动检测到副作用，但可以通过故意重复调用 
    constructor
    render
    setState更新函数（第一个参数）
    静态的getDerivedStateFromProps 生命周期方法
    来帮助发现诸如一些非幂等的方法函数引起的问题
    ```

    

+ 检测过时的 context API



### findDOMNode

`findDOMNode` 可以在给定 class 实例情况下在树中搜索 DOM 节点，但通常使用 ref 绑定 DOM 节点即可

理论上也可用于 class 组件，但违反抽象原则，使得父组件需单独渲染子组件时，会产生重构危险，我们不能更改组件的实现细节，因为父组件可能正在访问它的 DOM 节点。

`findDOMNode` 只返回第一个子节点，但使用 Fragments 时组件可以渲染多个 DOM 节点

`findDOMNode` 只读一次，调用后只返回第一次查询的结果，如果子组件渲染了不同的节点，则无法跟踪此更改，因此适用于组件返回单个且不可变的 DOM 节点时才有效



### 虚拟化长列表

参考虚拟滚动库 

[react-window](https://react-window.now.sh/#/examples/list/fixed-size)

[react-virtualized](https://bvaughn.github.io/react-virtualized/)



### fiber

在过去的 React 15中， 更新是同步的，调用各个组件的生命周期函数、计算对比Virtual DOM， 更新DOM等，而当组件数很大时，更新就会一层套一层逐渐深入，在更新完所有组件前不停止。

```
组件 A -> B -> C
则在挂载阶段的渲染顺序是
A - constructor 
A - willMount
A - render 
B - constructor 
B - willMount
B - render 
C - constructor 
C - willMount
C - render
C - didMount
B - didMount
A - didMount
```

这可能引起性能问题，因为JS运算、页面布局和绘制等都是运行在浏览器的主线程中，而他们往往又是互斥的。

这种同步操作时间过长的解决办法有-- 分片

React Fiber 就是把更新过程碎片化，每执行完一段更新（增量渲染），就把控制权交还给 React 负责任务协调的模块，决定是继续更新还是执行紧急任务，这也叫合作式调度（cooperative scheduling）

Fiber 实现了自己的组件调用栈，以链表的形式遍历组件树，可以灵活暂停、继续和丢弃执行的任务，从而释放浏览器主线程。实现方式是使用了浏览器的 `window.requestIdleCallback()`

`requestIdleCallback` 会在浏览器空闲的时候依次调用函数，可以在主事件循环中执行后台或低优先级的任务，而且不会对像动画和用户交互这样关键的事件产生影响。



Fiber 需要实现的有:

+ 把可中断的工作拆分成小任务
+ 对正在做的工作调整优先级、重做、复用上次成果
+ 在父子任务之间从容切换，以支持 React 执行时的布局刷新
+ 支持 render 返回多个元素



React 运行时存在3种实例：

+ DOM  -- 真实的 DOM 节点

+ Instances -- React 维护的 vDOM tree node

+ Elements -- 描述 UI 长什么样子

  Instances 根据 elements 创建，是对组件及DOM 节点的抽象展示， vDOM tree 维护组件状态以及组件与DOM树的关系

  在新增fiber 的同时也新增了几个实例:

  + effect 

    每个 workInProgress tree 节点上有一个effect list，用来存放 diff 结果，当前节点更新完会向上 merge effect list

  + workInProgress

    workInProgress tree 是reconcile过程中从 fiber tree 简历的当前进度快照，用于断点恢复

  + fiber

    fiber tree 与 vDOM tree 类似，用来描述增量更新所需的上下文信息

    

React 内部的运作分为3层：

+ Virtual DOM 层，描述页面长什么样
+ Reconciler 层, 负责调用组件生命周期方法，运行Diff运算
+ Renderer 层，根据不同平台(如 ReactDOM 和 RN) 渲染相应的页面



Fiber 主要属于 Reconciler 层，过去的 Reconciler  是一种 stack Reconciler，而 Fiber  Reconciler 每隔执行一段时间会将控制权交回给浏览器，分段执行，此时需要有一个调度器scheduler进行任务优先级分配

优先级主要有六种，高优先级(如键盘输入)可以打断低优先级任务(如diff):

+ synchronous，与之前的Stack Reconciler操作一样，同步执行

+ task，在next tick之前执行

+ animation，下一帧之前执行，这是通过`requestAnimationFrame`来调度

+ high，在不久的将来立即执行

+ low，稍微延迟执行也没关系

+ offscreen，指的是当前隐藏的、屏幕外的（看不见的）元素,下一次render时或scroll时才执行

  最后三个阶段都是由`requestIdleCallback`回调执行的

  

Fiber 本身是一种数据结构

```js
const fiber = {
    stateNode,    // 节点实例
    child,        // 子节点
    sibling,      // 兄弟节点
    return,       // 父节点
}
```

![clipboard.png](F:\Read-note\img\fiber_1)



Fiber Reconciler在执行的时候会分为两个阶段

+ render / reconciliation

  ​	生成 Fiber 树，并以此为蓝本把每个fiber作为一个工作单元，自顶向下逐节点构造 *workInProgress tree*（构建中的新fiber tree）

  1. 如果当前节点不需要更新，直接把子节点 clone 过来，跳到5，要更新的话打个 tag
  2. 更新当前节点状态（props， state， context等）
  3. 调用 `shouldComponentUpdate()` ，如为false则跳5
  4. 调用 render 获得新子节点，复用现有fiber的基础上为子节点创建fiber
  5. 如果没产生 child fiber， 该工作单元结束，把effect list 归并到 return，并把当前节点的 sibling 作为下一个工作单元，否则把 child 作为下一个工作单元
  6. 如果没有剩余可用时间了，则等下一次主线程空闲时再开始下一个工作单元
  7. 若没有下一个工作单元，则第一阶段结束，进入 pendingCommit 阶段。此时 workingProgress tree 根节点上的 effect list就是收集到的所有 side effect.

  这是一个渐进的过程，可以被打断，让优先级更高的任务先执行，从而降低页面掉帧的概率

+ commit

  将需要更新的节点批量更新，此过程不能被打断

  处理 effect list，包括更新DOM树，调用组件生命周期函数以及更新ref等内部状态

```js
// 第1阶段 render/reconciliation
componentWillMount
componentWillReceiveProps
shouldComponentUpdate
componentWillUpdate

// 第2阶段 commit
componentDidMount
componentDidUpdate
componentWillUnmount
```





Fiber Reconciler 在阶段一进行Diff计算的时候会生成一颗 Fiber 树，树是在Virtual DOM 树的基础上增加额外的信息来生成的，本质上来说是一个**链表**。Fiber 树也会在首次渲染的时候一次生成，然后在后续需要 Diff 的时候在 workingProgressTree(不是VirtualDomTree)复用已有的 Fiber 数据通过`requestIdleCallback` 调度一组任务，构建新树，每颗新树每生成一个新节点，都会将控制权交回给主线程去检查是否有高优先级任务需要执行，然后等下一次 `requestIdleCallback` 回调继续构建。在构造的过程中， Fiber Reconciler 会将需要更新的节点信息保存在 Effect List 中，然后在阶段二执行时批量更新



第一阶段： 

如果不被打断，则在执行完后会直接进入 render 函数， 构建真实的 virtualDOMTree

如果被打断，即组件只渲染到一半， react 会放弃当前组件所有干到一半的事情，去执行高优先级任务，在执行完后，通过 callback 回到之前的组件，从头开始渲染，这意味着所有阶段1的生命周期函数都可能被执行多次（这也说明其实fiber不是为了减少组件的渲染时间，而是为了提高用户体验减少卡顿）因此需要保证这里的生命周期函数每一次执行结果是一样的，最好是纯函数



**fiber tree 与 workInProgress tree**

这是一种双缓冲技术，前者为主，后者为辅，当 workInProgress tree 构造完毕，得到新的 fiber tree， 然后把current 指针指向 workInProgress tree，丢掉旧的 fiber tree。这样既能复用内部对象，又能节省内存分配、时间开销等

每个 fiber 上都有 `alternate` 属性，指向一个 fiber，在创建 workInprogress 节点时会优先取 `alternate`，没有的话则会创建一个

### API

`React.Component`

定义 class 组件



`React.PureComponent` 与前者区别在于，前者并未实现`shouldComponentUpdate()`， 而 PureComponent 以浅层对比 prop 和 state 的方法来实现了该函数

而且比 `React.Component` 的性能有一定的提高



`React.memo(function MyComponent(props) {}, areEqual)`

适用于函数组件而不是 class 组件，与 PureComponent 相似 

默认只会对复杂对象做浅层对比，因此可以使用自定义比较函数 `areEqual(prevProps, nextProps)` 来控制对比过程，但返回值和 class 组件中的 `shouldComponentUpdate()`相反，props相等返回true



`React.createElement(type, [props], [...children])`

`React.cloneElement(element, [props], [...children])`

克隆返回新react元素，返回的 props 将是新props与原始元素的props浅合并后的结果，几乎等同于`<element.type {...element.props} {...props}>{children}</element.type>`



`React.isValidElement(object)`验证对象是否是 React 元素

`React.children`提供用于处理 `this.props.children`的方法

​	`React.children.map()`

​	`React.children.forEach()`

​	`React.children.count(children)` 返回组件总数量

​	`React.children.obly(children)` 验证children是否只有一个子节点

​	`React.children.toArray(children) `返回扁平展开的数组形式,并为每个子节点分配key



`React.createRef`

`React.forwardRef`

`React.lazy(()=> import('...'))` 定义动态加载的组件，有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件

`React.Suspence`指定加载指示器



### ReactDOM

 `import ReactDOM from 'react-dom'` 提供可在应用顶层使用的 DOM 方法

`ReactDOM.render(element, container[, callback])`

`ReactDOM.hydrate(element, container[, callback])`

`ReactDOM.unmountComponentAtNode(container)`

`ReactDOM.findDOMNode(component)`

`ReactDOM.createPortal(child, container)`



### 

### 放弃 mixins 设计模式

+ mixins 引入了不清晰的依赖关系

  组件采用 mixins 的 state 和方法，mixins 采用了组件的方法或是其他的 mixins，这使得组件和 mixins  有强耦合的关系

+ mixins 导致命名空间的冲突

  mixins 的 state 和方法容易与组件或其他 mixins 发生冲突

+ mixins 导致更高的复杂度

  由于强耦合性，当新需求出现的时候，代码的复杂度会上升



### 栈调和（Stack reconciler)

React 元素并不是真实的 DOM 节点或组件实例，而是为了向 React 描述元素的类型、拥有的属性以及子元素。元素共有两种类型，DOM元素和组件元素，当创建和实例化他们的时候并不会引起任何渲染发生，只有遍历结束DOM树真正的渲染才发生。

当组件的 state 或 props 更改时， React 通过比较前后元素来决定是否需要更新实际的 DOM。当前后不相等时会找出最小变化集合再更新，这一过程叫做'调和'

栈是一种后入先出的机制。而过去的渲染采用的调和算法就是一个纯粹的递归算法。容易导致丢帧。

大多数设备刷新率是60FPS, 1/60=16.67ms。意味着每隔16ms 一帧。当 React 渲染的时间超过 16ms。浏览器就会丢帧，而实际上浏览器也有自己的工作，因此代码运行时间需要控制在10ms以内。