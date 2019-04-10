在 JSX 代码的外面扩上一个小括号，这样可以防止 分号自动插入 的bug.
react 内的元素渲染，会比较元素内容先后的不同，然后在渲染过程中更新改变了的部分

自顶向下 单向数据流 
```
//组件
class Welcome extends React.Componenet {
    render() {
        return <h1>Hello,{this.props.name}</h1>;
    }
}
//简单方法
function Welcome(props) {
    return <h1>Hello,{props.name}</h1>;
}
```

```
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
this.state.comment = 'hello' //wrong
this.setState({comment:'hello'}) //correct

//一起调用
//prevState 之前的状态，props  为此次更新被应用时的 props
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
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

由于类的方法默认是不会绑定`this`的，因此在 JSX 回调函数中应绑定`this`值，用 bind/箭头函数等


对于 React 内的列表，同 Vue 一样需要加上 key。
因为 keys 可以在 DOM 中的某些元素被增加或删除时帮助 React 识别哪些元素发生了变化。