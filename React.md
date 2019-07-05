

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

