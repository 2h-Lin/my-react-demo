
# Demo01
#### 初识react
> ReactDOM.render()： 将React元素渲染到根Dom节点中
- jsx语法

  ```javascript
  ReactDOM.render(
      <h1>Hello World</h1>,
      document.getElementById('demo1')
    )
  ```
  
  


# Demo02
JSX语法中可以放任何表达式

```javascript
 function formatName(user) {

  return user.firstName + user.lastName

 }


 const user = {

  firstName: 'Owen',

  lastName: 'Lin'

 }

 const element = <h1>Hello {formatName(user)}</h1>


 ReactDOM.render(

  element,

  document.getElementById('demo1')

 )
```

# Demo03
> React.createElement： 根据指定的第一个参数创建一个React元素
```javascript
  React.createElement(
    type,
    [props],
    [...children]
  )
```
第一个参数是必填，传入的是似HTML标签名称，eg: ul, li
第二个参数是选填，表示的是属性，eg: className，没有即为null
第三个参数是选填, 子节点，eg: 要显示的文本内容



> 在我们应用JSX进行开发的时候，其实它最终会转化成React.createElement…去创建元素。

```javascript
  const element = (
    <h1 className="greeting">
      Hello, world!
    </h1>
  );

  //上面代码最终会转换成下面去创建
  const element = React.createElement(
    'h1',
    { className: 'greeting' },
    'Hello, world!'
  );
```

# Demo04 
#### 组件

- 函数组件
```javascript 
  function Welcome(props) {
    return <h1>Hello {props.name}, this is Function Component!</h1>
  }
```

- class组件
```javascript
  class Welcome extends React.Component {
    render() {
      return <h1>Hello {this.props.name}, this is Class Component!</h1>
    }
  }
```

## Demo05
#### state
state 添加在class创建的组件中，作用相当于一个变量，必须使用setState才可更新state的状态
```javascript
 class Welcome extends React.Component {
    constructor(props) {
      super(props)
      this.state = { name: 'world', isShow: false }
    }
    handleClick = () => {
      this.setState({isShow: !this.state.isShow})
    }
    render() {
      return (
        <div>
          <button onClick={this.handleClick}>click me</button>
          {this.state.isShow && <h1>Hello {this.state.name}, this is Class Component!</h1>}
        </div>
      )
    }
  }
  ```

# Demo06
#### 事件处理

  - React 事件的命名采用小驼峰式（camelCase）
  - 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串

class里默认不会绑定this，所以有两种方法获取到this:
##### 1.使用箭头函数

```javascript
 
 class Welcome extends React.Component {
    constructor(props) {
      super(props)
      this.state = { name: 'world', isShow: false }
    }
    handleClick = () => {
      this.setState({isShow: !this.state.isShow})
    }
    render() {
      return (
        <div>
          <button onClick={this.handleClick}>click me</button>
          {this.state.isShow && <h1>Hello {this.state.name}, this is Class Component!</h1>}
        </div>
      )
    }
  }
```
  或者
```javascript 
  class Welcome extends React.Component {
    constructor(props) {
      super(props)
      this.state = { name: 'world', isShow: false }
    }s
    handleClick() {
      this.setState({isShow: !this.state.isShow})
    }
    render() {
      return (
        <div>
          <button onClick={() => this.handleClick()}>click me</button>
          {this.state.isShow && <h1>Hello {this.state.name}, this is Class Component!</h1>}
        </div>
      )
    }
  }
```

##### 2.使用bind
```javascript 
  class Welcome extends React.Component {
    constructor(props) {
      super(props)
      this.state = { name: 'world', isShow: false }
      this.handleClick = this.handleClick.bind(this)
    }s
    handleClick() {
      this.setState({isShow: !this.state.isShow})
    }
    render() {
      return (
        <div>
          <button onClick={this.handleClick}>click me</button>
          {this.state.isShow && <h1>Hello {this.state.name}, this is Class Component!</h1>}
        </div>
      )
    }
  }
```

# Demo07
#### 列表渲染与key绑定
列表渲染后每个元素要绑定一个独自的key，key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此应当给数组中的每一个元素赋予一个确定的标识。
一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用数据中的 id 来作为元素的 key：

```javascript
  class Welcome extends React.Component {
    render() {
      const todoList = this.props.todoList
      const listItems = todoList.map((item) => 
        <li key={item.id}>{item.title}</li>
      );
      return (
        <div>
          <ul>{listItems}</ul>
        </div>
      )
    }
  }
  const todoList = [{title:'one',id:1},{title: 'two',id:2},{title: 'three',id:3},{title: 'four',id:4},{title: 'five',id:5}]
  ReactDOM.render(
    <Welcome todoList={todoList}/>,
    document.getElementById('root')
  )
```

# Demo08
#### Form表单
受控组件：在 HTML 中，表单元素（如<input>、 <textarea> 和 <select>）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新。我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。
```javascript
  class Input extends React.Component {
    constructor(props) {
      super(props)
      this.state = {value: 'hello'}
    }
    handleChange = (event) => {
      this.setState({value: event.target.value})
    }
    render() {
      const value = this.state.value
      return (
        <div>
          <input type="text" value={value} onChange={this.handleChange} />
          <p>{value}</p>
        </div>
      )
    }
  }
  ReactDOM.render(
    <Input />,
    document.getElementById('root')
  )
```

# Demo09
#### 生命周期
常用生命周期：
1. componentWillMount()：在渲染前调用,将在react17之后被废弃。
2. render()： react最重要的步骤，创建虚拟dom，进行diff算法，更新dom树都在此进行。此时就不能更改state了。
3. componentDidMount(): 组件渲染之后调用，可以通过this.getDOMNode()获取和操作dom节点，只调用一次。可用于ajax请求。
4. componentWillReceivePorps(nextProps)：在已经挂在的组件(mounted component)接收到新props时触发。
5. shouldComponentUpdate(nextProps, nextState)：组件接受新的state或者props时调用，我们可以设置在此对比前后两个props和state是否相同，如果相同则返回false阻止更新，因为相同的属性状态一定会生成相同的dom树，这样就不需要创造新的dom树和旧的dom树进行diff算法对比，节省大量性能，尤其是在dom结构复杂的时候。不过调用this.forceUpdate会跳过此步骤。在componentWillReceivePorps(nextProps)后触发。
6. componentWillUpdate(nextProps, nextState): 在props或state发生改变或者shouldComponentUpdate(nextProps, nextState)触发后。
7. componentDidUpdate(prevProps, prevState)： 会在更新后会被立即调用。首次渲染不会执行此方法，可获取Dom节点。
8. componentWillUnmount()：组件将要卸载时调用，一些事件监听和定时器需要在此时清除。


# Demo 10 
#### ajax请求

```javascript
  class UserGist extends React.Component {
    constructor(props) {
      super(props)
      this.state = {
        username: '',
        lastGistUrl: '',
        source: 'https://api.github.com/users/octocat/gists'
      };
    }

    componentDidMount() {
      $.get(this.state.source, function (result) {
        var lastGist = result[0];
        this.setState({
          username: lastGist.owner.login,
          lastGistUrl: lastGist.html_url
        });
      }.bind(this));
    }

    render() {
      return (
        <div>
          {this.state.username}'s last gist is
          <a href={this.state.lastGistUrl}>here</a>.
        </div>
      );
    }
  }

  ReactDOM.render(
    <UserGist />,
    document.getElementById('root')
  );
```

# Demo11 
#### React.createRef()
使用 React.createRef() 来创建`dom`节点
```javascript
  class InputRef extends React.Component {
    constructor(props) {
      super(props)
      this.myInputRef = React.createRef()
      this.handleClick = this.handleClick.bind(this)
    }

    handleClick() {
      this.myInputRef.current.focus()
    }

    render() {
      return (
        <div>
          <input type="text" ref={this.myInputRef}/>
          <button onClick={this.handleClick}>focus input</button>
        </div>
      );
    }
  }

  ReactDOM.render(
    <InputRef />,
    document.getElementById('root')
  );
```

