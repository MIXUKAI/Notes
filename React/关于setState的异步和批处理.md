先举个栗子
```js
import React, { Component } from "react";
import "./App.css";

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
    this.stfun = this.stfun.bind(this);
    this.stobj = this.stobj.bind(this);
  }

  stobj() {
    this.setState({ count: this.state.count + 1 });
  }
  stfun() {
    this.setState(state => ({ count: state.count + 1 }));
  }

  invoke10times = fn => {
    for (let i = 0, i < 10; i++) {
      fn()
    }
  };
  render() {
    console.log("render");
    return (
      <div className="App">
        <span>{this.state.count}</span>
        <button onClick={() => this.invoke10times(this.stfun)}>
          {"use function as argument"}
        </button>
        <button onClick={() => this.invoke10times(this.stobj)}>
          {"use obejct as argument"}
        </button>
      </div>
    );
  }
}

export default App;
```
## async/defer 和 batch 异步与批处理 

先说结论, React官方文档也明确说明了`setState`不是同步和立即响应的, 它往往会被延期执行, 也就是说你调用了`setState`, state不会立即得到更新, 要想使用得到更新后的state可以从`setState`提供的第二个回调参数里获取, 或者是`componentDidUpdate`

那延期是为了什么呢, 当然是为了更好的性能, **react16或之前的版本在默认情况下会在`react事件`内部的对`setState`进行`批处理batch`**

批处理的第一表现就是上面这个例子中两个按钮, 一个使用函数作为参数,一个使用对象作为参数, 分别调用十次`setState`, 结果是一个+10, 一个+1

**这是因为在批处理这个过程中存在`shallow merge`**

大致如下
```js
const { count } = this.state
Object.assign({}, state, {count: count + 1}, {count: count + 1}...)
```
而使用`函数updater`作为参数先是进行`updater`调用10次就是进行10次更新,
`(state, props) => stateChange`, **函数接受到的state和props都是最新的**, 因此调用10次就是在更改的基础上加+10次1
**批处理最后对函数的结果进行`shallow merge`**

大致相当与
```js
let { count } = this.state
r1 = updater() // { count: 1 }
r2 = updater() // { count: 2 }
...
r10 = updater() // { count: 10 }
Object.assign({}, state, r1, r2, r3,...r10)
```
因此可想而知如果不是count + 1而是
```js
  this.state = {
    a: false,
    b: false,
    c: false,
    d: false,
    e: false,
    f: false,
    g: false,
    h: false,
    i: false
  };
  this.setState({ a: true });
  this.setState({ b: true });
  this.setState({ c: true });
  this.setState({ d: true });
  this.setState({ e: true });
  this.setState({ f: true });
  this.setState({ g: true });
  this.setState({ h: true });
  this.setState({ i: true });
```
那使用函数和对象作为参数结果都是一样的

**因为是批处理, 真正设置state的是`shallow merge`过后的结果, 因此这个例子中点击一下虽然按钮调用10次`setState`最后`render`只会被调用一次**

记住: **只要是设置在同一个`react事件`内的`setState`, 无论`setState`被调用多少次, 无论多少组件的内的`setState`被调用, 都会被批处理**

```js
class SuperContainer extends React.Component {
  constructor(props) {
    super(props);
    this.state = { a: false };
  }

  render() {
    console.log("supercontainer render");
    return <Container setParentState={this.setState.bind(this)} />;
  }
}

class Container extends React.Component {
  constructor(props) {
    super(props);
    this.state = { b: false };
  }

  render() {
    console.log("container render");
    return <button onClick={this.handleClick}>innerButton</button>;
  }

  handleClick = () => {
    this.props.setParentState({ a: true });
    this.setState({ b: true });
  };
}
```
在这个例子中点击事件虽然涉及父子组件的`state`更新, 但是他们依旧会被批处理, 也就是不会发生子组件的state发生了变化, 然后子组件render一次, 然后父组件的state更细了, 父组件render, 最后子组件被render了两次这种情况. 
**真实的结果是点击按钮, 子组件只被render一次**, 当然这个例子涉及的知识点不是`shallow merge`, 是react批处理的内部的实现了

但是如果是`setState`没有`React 事件之外`就**不会进行批处理**即会每次调用`setState`就会re-render一次，如放在ajax的回调里面

```js
  this.state = {
    a: false,
    b: false,
    c: false,
    d: false,
    e: false,
    f: false,
    g: false,
    h: false,
    i: false
  };

  handleClick = () => {
    this.setState({ a: true });
    this.setState({ b: true });
    this.setState({ c: true });
    this.setState({ d: true });
    this.setState({ e: true });
    this.setState({ f: true });
    this.setState({ g: true });
    this.setState({ h: true });
    this.setState({ i: true });
  };

  updatefromFetch = () => {
    fetch("https://jsonplaceholder.typicode.com/todos/1")
      .then(() => {
        this.setState({ a: true });
        this.setState({ b: true });
        this.setState({ c: true });
        this.setState({ d: true });
        this.setState({ e: true });
        this.setState({ f: true });
        this.setState({ g: true });
        this.setState({ h: true });
        this.setState({ i: true });
      })
  };
  <button onClick={this.handleClick} /> // re-render once
  <button onClick={this.updatefromFetch} /> // re-render 10 times
```

最后说下`setState`的第二个回调参数, 这个函数会在`state`更新完毕并且`re-render`之后才会被调用

假设第一个例子中
```js
this.setState((state) => ({count: state.count + 1}), () => {
  console.log(this.state.count)
})
```
那么打印的结果是10个10


## Reference

- [Reactjs.component api](https://reactjs.org/docs/react-component.html)
- [When and why are setState() calls batched?](https://stackoverflow.com/questions/48563650/does-react-keep-the-order-for-state-updates)