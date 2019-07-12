

[TOC]

# React

##JSX

JSX是一个JavaScript语法扩展，可以很好地描述 UI 应该呈现出它应有交互的本质形式。react 考虑到渲染逻辑本质上与其他 UI 逻辑内在耦合，因此将标记与逻辑共同存放在'组件'的松散耦合单元之中

JSX 语法中可以在**大括号**`{}`放置表达式

JSX 本身也是一个表达式，也可以赋值给变量作为参数传入或在函数中返回

JSX 语法更接近 JavaScript 而不是 HTML，因此react dom 使用小驼峰命名



React DOM在渲染输入内容前会进行转义。因此可以有效地防止 XSS 攻击

Babel会把 JSX 转译为`React.createElement()`

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


##组件

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



##React 生命周期

```
//生命周期钩子：
componentDidMount() {
//挂载组件到 DOM 上时
}
componentWillUnmount() {
//卸载移除组件时
}
```


##事件处理
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



##Fragment

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

2. 当`ref`用于**自定义 class 组件**时，`ref`对象接收组件的挂载实力作为其`current`属性

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

   

3. 不能在函数组件上使用`ref`属性，因为函数组件没有实例。但可以在函数组件内部使用 ref 属性

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
```



**回调 Refs**

回调 Refs 可以更精细地控制何时refs 被设置和解除,

不同于 createRef()创建的 ref 属性，他是传递一个函数，**接收组件实例或 HTML DOM 元素作为参数**，在其他地方被存储和访问

```react
class CustomTextInput extends React.Component {
	constructor(props) {
		super(props)
		this.textInput = null
		this.setTextInputRef = element => {
			this.textInput = element
		}
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
					ref={this.setTextInputRef}
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
若以这种内联函数方式定义，在更新过程中会被执行两次，第一次传入参数 null，第二次会传入参数 DOM 元素。因为每次渲染时会创建一个新函数实例，所以会清空旧的 ref 并设置新的。
通过将回调定义成class 的绑定函数的方式可以避免此问题
```

