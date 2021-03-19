---
layout: post
title: "🥫 受控组件与非受控组件"
date: 2021-03-17 00:00:00
categories: [React]
tags: [React]
comments: true
---


<!--more-->

### The Uncontrolled

非受控表单就像传统的HTML表单一样， [使用 ref](https://react.docschina.org/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

在 React 渲染生命周期时，表单元素上的 `value` 将会覆盖 DOM 节点中的值，在非受控组件中，你经常希望 React 能赋予组件一个初始值，但是不去控制后续的更新。 在这种情况下, 你可以指定一个 `defaultValue` 属性，而不是 `value`。

同样，`<input type="checkbox">` 和 `<input type="radio">` 支持 `defaultChecked`，`<select>` 和 `<textarea>` 支持 `defaultValue`。


### The Controlled

**The state gives the value to the input, and the input asks the Form to change the current value.**

在 HTML 中，表单元素（如\<input\>、 \<textarea\> 和 \<select\>）通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新。

使 React 的 state 成为表单元素的“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}

```

| Element | Value property | Change callback | New value in the callback |
| ------ | ------ | ------ | ------ |
| \<input type="text" /> | value="string" | onChange | event.target.value |
| \<input type="checkbox" /> | checked={boolean} | onChange | event.target.value |
| \<input type="radio" /> | checked={boolean} | onChange | event.target.value |
|\ <textarea \/\> | value="string" | onChange | event.target.value |
| \<select /> | value="option value" | onChange | event.target.value |

### Conclusion

受控和不受控表单都有其优点，根据具体情况选择适合的。

<img src="/image/posts/2021031901.png" style="display:block;margin:0 auto;">