---
title: 初识React
date: 2019-07-19 22:53:53
tags: React 
categories: 前端
---

## JSX
Jsx是一个看起来很想XML的Javascript语法拓展。
### 优点
1. JSX执行更快，因为它在编译为 Javascript代码后进行了优化。
2. 它是类型安全的，在编译过程中能发现错误。
3. 使用JSX编写模版更加简单快速。

>在JSX中嵌入表达式
```jsx
const name = 'jsx';
const element = <h1>Hello,{name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
在JSX语法中，你可以在大括号内放置任何有效的 JavaScript表达式。

### JSX嵌入表达式
下面的例子中，我们调用Javascript函数 formatName(name) 的结果，并讲结果嵌入到 \<h1> 元素中。
```jsx
function formatName(name){
  return `hello,${name}`;
}

const name = 'jsx';

const element = (
  <h1>{formatName(name)}</h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
**其中官方建议将内容都包在括号里面，这样可以避免自动插入分号。**

### JSX也是一个表达式
React官网上说，JSX在编译之后，JSX表达式会被转为**普通的JavaScript函数**调用，并且对其取值后得到的Javascript对象。
也就是说，你可以把if语句和for循环的代码块中使用JSX，将JSX赋值给变量，把JSX当作参数传入，以及从函数中返回JSX。
```jsx
function getGreeting(name){
  if(name){
    return <h1>Hello,{formatName(name)}!</h1>
  }
  return <h1>Hello,React</h1>;
}
```

### JSX特定属性
可以用使用引号，来将属性值指定为字符串字面量：
```jsx
const element = <div tabIndex="0"></div>;
```
还可以使用大括号，在属性值中插入一个Javascript表达式：
```jsx
const element = <img src={user.avatar}></img>;
```
⚠️在属性中嵌入 Javascript表达式时，**不要在大括号外面加上引号**。你应该只使用引号(对于字符串值)或大括号(对于表达式)中一个，对于同一个属性不能同时使用两种符号。
⚠️JSX语法上接近Javascript而不是HTML，所以使用React DOM使用cameCase(小驼峰命名)来定义属性的名称，而不使用HTML属性名称的命名约定。例如：JSX里的class变成了className，而tabindex则变为tabIndex。

### JSX指定子元素
如果一个标签里面没有内容，可以使用 />来闭合标签。
```jsx
const element = <img src={user.avatar} />;
```
JSX标签里能够包含很多子元素：
```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>JSX</h2>
  </div>
);
```

### JSX防止注入攻击
你可以安全的在JSX中插入用户输入的内容：
```jsx
const title = response.potentiallyMaliciousInput;
//  直接使用是安全的
const element = <h1>{title}</h1>;
```
因为React DOM在渲染所有输入内容之前，默认会进行**转义**。它可以确保在你的应用中，永远都不会注入那些并非自己编写的内容。所有的内容在渲染之前都将被换成字符串。这样可以有效的防止**[XSS](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击**

### JSX表示对象
Babel会把JSX转译成一个名为React.createElement()函数调用。
以下两种示例代码完全等效：
```jsx
// 方式一
const element = (
  <h1 className="greeting">
    hello,world!;
  </h1>
)

// 方式二
const element = React.createElement(
  'h1',
  {className:'greeting'},
  'hello,world!'
)
```
**React.createElement()**会预先执行一些检查，以帮助你编写无错误代码，但实际上它创建一个这样的对象：
```jsx
// 这是简化过的结构
const element = {
  type:'h1',
  props:{
    className:'greeting',
    children:'hello,world!'
  }
}
```
这些对象被称为 “React元素”。

## 组件
组件，从概念上和Javascript函数类似。它接受任意的参数（即“props”），并返回用于描述页面展示内容的React元素。

### 函数组件和class组件

定义组件最简单的方式就是编写Javascript函数:
```jsx
function Welcome(props){
  return <h1>hello,{props.name}</h1>;
}
```
该函数是一个有效的React组件，它接受唯一带有数据“props”（代表属性）对象与并返回一个React元素。这类组件被称为**“函数组件”**，因为它本质上就是Javascript函数。

ES6的class来定义组件：
```jsx
class Welcome extends React.component{
  render(){
    return <h1>Hello,{this.props.name}</h1>;
  }
}
```
上面的两个组件在React里是一样的。

### 渲染组件
我们之前遇到的React元素都只是DOM标签：
```jsx
const element = <div />;
```
React元素也可以是用户自定义的组件：
```jsx
const element = <Welcome name="react">;
```
当React元素为用户自定义的组件时，它会将JSX接收的属性（attributes）转成单个对象传递给组件，这个对象就是“props”。
例如下面这个例子，这段代码会在页面上渲染 “hello，react”。
```jsx
function Welcome(props){
  return <h1>Hello,{props.name}</h1>;
}
const element = <Welcome name="react"/>;
ReactDOM.render(
  element,
  document.getElementById('root')
)
```

### 组合组件
组件可以输入引用的其他组件，就是嵌套别的组件。例子：
```jsx
function Welcome(props){
  return <h1>hello,{props.name}</h1>;
}
function App(){
  return(
    <div>
      <Welcome name="A" />
      <Welcome name="B" />
      <Welcome name="C" />
    </div>
  )
}
ReactDOM.render(
  <App/>,
  document.getElementById('root')
)
```

### 提取组件
就是将组件拆分为更小的组件。

提取前：
```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

提取后：
```jsx
// 单独提取Avatar部分
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />

  );
}

function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

## Props
```jsx
var ShowTitle = React.createClass({
  // 通过组件的getDefaultProps，对属性设置默认值
  getDefaultProps:function(){
    return{
      title : "默认标题"
    }
  },
  render : function(){
    return <h1>{this.props.title}</h1>
  }
});
```
### 只读性
组件无论是使用“函数声明”还是“class声明”，都不能修改自身的props。来看下面的例子：
```js
function sum(a,b){
  return a+b;
}
```
这样的函数被称为“纯函数”，因为该函数不会尝试改变传入的参数，并且调用时传入的参数相同返回的结果始终相同。
下面例子就不是存函数，刚好相反。
```js
function withdraw(account, amount) {
  account.total -= amount;
}
```
React非常灵活，但是也有一个严格的规则：
所有React组件都必须想存函数一样保护它们的props不被更改。

## 非DOM属性
### dangerouslySetInnerHTML
其实就是用来嵌入富文本编辑的文本。
```jsx
var HelloMessge = React.createClass({
　　 render: <div dangerouslySetInnerHTML={{ __html: '<h3>hello</h3>' }}> </div>
});
```
这么做的意义在于,**{__html:...}**背后的目的表明它会被当成“type/taint”类型来处理。
⚠️注意{__html:...}是两个下划线

### ref（其实就是react提供的id选择器）
```jsx
class HelloMessage extends React.component{
  handle(){
    this.refs.name = 'hello';
  }

  render(){
    return(
      <div>
        <input ref="name" type="text"/>
        <button onClick={this.handle}>自动填姓名</button>
      </div>
    )
  }
}
```

### key（提高组件渲染性能）
key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。如果没有key的话，渲染的时候li就会全部重新被渲染。
```jsx
class HelloMessage extends React.component{
  render(){
    return(
      <ul>
        <li key="1">1</li>
        <li key="2">2</li>
        <li key="3">3</li>
      </ul>
    )
  }
}
```

## State
```jsx
class Clock extends React.Component{
  constructor(props){
    super(props); // 使constructor指向子类
    this.state = {
      data : new Date()
    };
  }

  update(){
    // 通过this.setState来更新state
    this.setState({
      data:'2222'
    })
  }

  render(){
    return(
      <div>
        <h1>{this.state.data}</h1>
      </div>
    )
  }
}

ReactDOM.render(
  <Clock/>,
  document.getElementById('root')
)
```
state的更新可能是异步的，因为react处于性能考虑，react可能会把多个setState()调用合并成一个调用。

## 生命周期
当组件挂载或卸载的时候会触发时候会执行的方法，就叫生命周期。
```jsx
class Clock extends React.Component{
  constructor(props){
    super(props);
    this.state = {data : new Date()};
  }
  componentDidMount(){
    console.log('会在组件已经被渲染到DOM中后运行');
  }
  componentWillUnmount(){
    console.log('在组件从 DOM 中移除之前立刻被调用');
  }
  render(){
    return(
      <div>
        <h1>hello,world!</h1>
        <h2></h2>
      </div>
    )
  }
}
```

## 事件处理
React事件的命名采用小驼峰式。使用JSX语法式传入的式一个函数而不是一个字符串。
```js
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
// 在React中和原生不一样的是不能通过返回false的方式阻止默认行为。必须是使用 preventDefault。
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  // 可以通过这样方式来绑定this
  handleClick = () =>{};

  return (
    // 在React中使用 onClick 绑定事件
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
**向事件处理程序传递参数**
```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
上面两种方式都是等价的，分别通过箭头函数和Function.prototype.bind来实现。

## 条件渲染
### if渲染
```jsx
class Demo extends React.component{
  render(){
    return(
      const isLoggedIn = this.state.isLoggedIn;
      if (isLoggedIn) {
        return <UserGreeting />;
      }
      return <GuestGreeting />;
    )
  }
}
```

### 与运算符 &&
```jsx
class Demo extends React.component{
  render(){
    const unreadMessages = false;
    return (
      <div>
        <h1>Hello!</h1>
        {unreadMessages > 0 &&
          <h2>
            You have {unreadMessages} unread messages.
          </h2>
        }
      </div>
    )
  }
}

```

### 三元运算符
```jsx
class Demo extends React.component{
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
      <div>
        The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
      </div>
    );
  }
}
```