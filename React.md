## 1.什么是 React
#### React是一个简单的javascriptUI库，用于构建高效、快速的用户界面。
- 轻量级库
- 遵循组件设计模式、声明式编程范式和函数式编程概念，使前端应用程序更高效
- 使用虚拟DOM来有效地操作DOM
- 遵循从 高阶组件 到 低阶组件 的 单向数据流
***

## 2.React 与 Angular 有何不同
- Angular是一个成熟的MVC框架，带有很多特定的特性，比如服务、指令、模板、模块、解析器等等。
- Angular遵循两个方向的数据流
- React是一个非常轻量级的库，它只关注MVC的视图view部分。
- React遵循从上到下的单向数据流。
- React在开发特性时给了开发人员很大的自由，例如，调用API的方式、路由等等。我们不需要包括路由器库，除非我们需要它在我们的项目。
***

## 3.什么是Virtual DOM
- virtual dom就是虚拟节点。它通过 JS 的 Object 对象模拟 DOM 中的节点，然后再通过特定的 render 方法将其渲染成真实的 DOM 节点
- 我们知道的对于页面的重新渲染一般的做法是通过操作 dom，重置 innerHTML 去完成这样一件事情。而 virtual dom 则是通过 JS 层面的计算，返回一个 patch 对象，即补丁对象，在通过特定的操作解析 patch 对象，完成页面的重新渲染
- [详细点击](https://juejin.im/post/5cac5a87e51d456e7618a67b)
- **总结**：
    - 从 Element 模拟 DOM 节点开始，然后通过 render 方法将 Element 还原成真实的 DOM 节点。
    - 然后再通过完成 diff 算法，比较新旧 Element 的不同，并记录在 patch 对象中。
    - 最后在完成 patch 方法，将 patch 对象解析，从而完成 DOM 的 update
***

## 4.什么是 JSX
- JSX是javascript的语法扩展。它就像一个拥有javascript全部功能的模板语言。
- 它生成React元素，这些元素将在DOM中呈现
- React建议在组件使用JSX。
- 在JSX中，我们结合了javascript和HTML，并生成了可以在DOM中呈现的react元素。
```
import React from 'react';

export const Header = () => {

    const heading = 'TODO App'

    return(
        <div style={{fontSize:'18px'}}>
            <h1>{heading}</h1>
        </div>
    )
}
```
***

## 5.组件和不同类型
##### React 中一切都是组件。
##### 我们通常将应用程序的整个逻辑分解为小的单个部分。我们将每个单独的部分称为组件。
##### 通常，组件是一个javascript函数，它接受输入，处理它并返回在UI中呈现的React元素。
##### 在React中有不同类型的组件:
1. 函数/无状态/展示组件

##### 函数或无状态组件是一个纯函数，它可接受接受参数，并返回react元素。
##### 这些都是没有任何副作用的纯函数。这些组件没有状态或生命周期方法，这里有一个例子。
```
import React from 'react';
import Jumbotron from 'react-bootstrap/Jumbotron';

export const Header = () => {
    return(
        <Jumbotron style={{backgroundColor:'orange'}}>
            <h1>TODO App</h1>
        </Jumbotron>
    )
}
```
2.类/有状态组件
##### 类或有状态组件具有状态和生命周期方可能通过setState()方法更改组件的状态。
##### 类组件是通过扩展React创建的。它在构造函数中初始化，也可能有子组件,这里有一个例子。
```
import React from 'react';
import '../App.css';
import { ToDoForm } from './todoform';
import { ToDolist } from './todolist';

export class Dashboard extends React.Component {
  constructor(props){
    super(props);
    this.state = {
    }
  }
  render() {
    return (
      <div className="dashboard"> 
          <ToDoForm />
          <ToDolist />
      </div>
    );
  }
}
```
3.受控组件
##### 受控组件是在 React 中处理输入表单的一种技术。
##### 表单元素通常维护它们自己的状态，而react则在组件的状态属性中维护状态。
##### 我们可以将两者结合起来控制输入表单。这称为受控组件。因此，在受控组件表单中，数据由React组件处理。
##### 这里有一个例子。当用户在 todo 项中输入名称时，调用一个javascript函数handleChange捕捉每个输入的数据并将其放入状态，这样就在 handleSubmit中的使用数据。
```
import React from 'react';
import Form from 'react-bootstrap/Form';
import Button from 'react-bootstrap/Button';
import Row from 'react-bootstrap/Row';
import Col from 'react-bootstrap/Col';

export class ToDoForm extends React.Component {
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
      alert('A name was submitted: ' + this.state.value);
      event.preventDefault();
    }
  
    render() {
      return (
          <div className="todoform">
            <Form>
                <Form.Group as={Row} controlId="formHorizontalEmail">
                    <Form.Label column sm={2}>
                    <span className="item">Item</span>
                    </Form.Label>
                    <Col sm={5}>
                        <Form.Control type="text" placeholder="Todo Item" />
                    </Col>
                    <Col sm={5}>
                        <Button variant="primary" type="submit">Add</Button>
                    </Col>
                </Form.Group>
            </Form>
         </div>
      );
    }
  }
```

4.非受控组件
##### 大多数情况下，建议使用受控组件。有一种称为非受控组件的方法可以通过使用Ref来处理表单数据。在非受控组件中，Ref用于直接从DOM访问表单值，而不是事件处理程序。
##### 我们使用Ref构建了相同的表单，而不是使用React状态。 我们使用React.createRef() 定义Ref并传递该输入表单并直接从handleSubmit方法中的DOM访问表单值。
```
import React from 'react';
import Form from 'react-bootstrap/Form';
import Button from 'react-bootstrap/Button';
import Row from 'react-bootstrap/Row';
import Col from 'react-bootstrap/Col';

export class ToDoForm extends React.Component {
    constructor(props) {
      super(props);
      this.state = {value: ''};
      this.input = React.createRef();
  
      this.handleSubmit = this.handleSubmit.bind(this);
    }
  
    handleSubmit(event) {
      alert('A name was submitted: ' + this.input.current.value);
      event.preventDefault();
    }
  
    render() {
      return (
          <div className="todoform">
            <Form>
                <Form.Group as={Row} controlId="formHorizontalEmail">
                    <Form.Label column sm={2}>
                    <span className="item">Item</span>
                    </Form.Label>
                    <Col sm={5}>
                        <Form.Control type="text" placeholder="Todo Item" ref={this.input}/>
                    </Col>
                    <Col sm={5}>
                        <Button variant="primary" onClick={this.handleSubmit} type="submit">Add</Button>
                    </Col>
                </Form.Group>
            </Form>
         </div>
      );
    }
  }
```

5.容器组件
##### 容器组件是处理获取数据、订阅 redux 存储等的组件。它们包含展示组件和其他容器组件，但是里面从来没有html。

6.高阶组件
##### 高阶组件是将组件作为参数并生成另一个组件的组件。 Redux connect是高阶组件的示例。 这是一种用于生成可重用组件的强大技术。

***
## 6.Props 和 State
#### Props 是只读属性，传递给组件以呈现UI和状态，我们可以随时更改组件的输出。
#### 下面是一个类组件的示例，它在构造函数中定义了props和state，每当使用this.setState() 修改状态时，将再次调用 render( ) 函数来更改UI中组件的输出。
```
import React from 'react';
import '../App.css';

export class Dashboard extends React.Component {
  constructor(props){
    super(props);
    this.state = {
        name: "some name"
    }
  }

  render() {
    // reading state
    const name = this.state.name;
    //reading props
    const address = this.props.address;
    return (
      <div className="dashboard"> 
          {name}
          {address}
      </div>
    );
  }
}

```

***
## 7.什么是PropTypes

##### 随着时间的推移，应用程序会变得越来越大，因此类型检查非常重要。PropTypes为组件提供类型检查，并为其他开发人员提供很好的文档。如果react项目不使用 Typescript，建议为组件添加 PropTypes。
##### 如果组件没有收到任何 props，我们还可以为每个组件定义要显示的默认 props。这里有一个例子。UserDisplay有三个 prop:name、address和age，我们正在为它们定义默认的props 和 prop类型。

```
import React from 'react';
import PropTypes from 'prop-types';

export const UserDisplay = ({name, address, age}) => {

    UserDisplay.defaultProps = {
        name: 'myname',
        age: 100,
        address: "0000 onestreet"
    };

    return (
        <>
            <div>
                <div class="label">Name:</div>
                <div>{name}</div>
            </div>
            <div>
                <div class="label">Address:</div>
                <div>{address}</div>
            </div>
            <div>
                <div class="label">Age:</div>
                <div>{age}</div>
            </div>
        </>
    )
}

UserDisplay.propTypes = {
    name: PropTypes.string.isRequired,
    address: PropTypes.objectOf(PropTypes.string),
    age: PropTypes.number.isRequired
}
```
***
## 8.如何更新状态以及如何不更新

#### 你不应该直接修改状态。可以在构造函数中定义状态值。直接使用状态不会触发重新渲染。React 使用this.setState()时合并状态。
```
//  错误方式
this.state.name = "some name"
//  正确方式
this.setState({name:"some name"})
```

#### 使用this.setState()的第二种形式总是更安全的，因为更新的props和状态是异步的。这里，我们根据这些 props 更新状态。
```
// 错误方式
this.setState({
    timesVisited: this.state.timesVisited + this.props.count
})
// 正确方式
this.setState((state, props) => {
    timesVisited: state.timesVisited + props.count
});
```
***
## 9.组件生命周期方法
##### React的生命周期从广义上分为三个阶段：挂载、渲染、卸载

##### 因此可以把React的生命周期分为两类：挂载卸载过程和更新过程。

![image](https://upload-images.jianshu.io/upload_images/16775500-8d325f8093591c76.jpg)

1. 挂载卸载过程
2. 更新过程
3. React新增的生命周期

[详细点击](https://www.jianshu.com/p/b331d0e4b398)

### 1.挂载卸载过程
- constructor()
```
constructor()中完成了React数据的初始化，它接受两个参数：props和context，
当想在函数内部使用这两个参数时，需使用super()传入这两个参数。
注意：只要使用了constructor()就必须写super(),否则会导致this指向错误。
```
- componentWillMount()
```
componentWillMount()一般用的比较少，它更多的是在服务端渲染时使用。
它代表的过程是组件已经经历了constructor()初始化数据后，但是还未渲染DOM时。
```
- componentDidMount()
```
组件第一次渲染完成，此时dom节点已经生成，
可以在这里调用ajax请求，返回数据setState后组件会重新渲染
```

- componentWillUnmount ()
```
在此处完成组件的卸载和数据的销毁。
1.clear你在组建中所有的setTimeout,setInterval
2.移除所有组建中的监听 removeEventListener
```

### 2.更新过程
- componentWillReceiveProps (nextProps)
```
1.在接受父组件改变后的props需要重新渲染组件时用到的比较多
2.接受一个参数nextProps
3.通过对比nextProps和this.props，将nextProps的state为当前组件的state，从而重新渲染组件

  componentWillReceiveProps (nextProps) {
    nextProps.openNotice !== this.props.openNotice&&this.setState({
        openNotice:nextProps.openNotice
    }，() => {
      console.log(this.state.openNotice:nextProps)
      //将state更新为nextProps,在setState的第二个参数（回调）可以打         印出新的state
  })
}
```
- shouldComponentUpdate(nextProps,nextState)
```
1.主要用于性能优化(部分更新)
2.唯一用于控制组件重新渲染的生命周期，由于在react中，setState以后，state发生变化，
组件会进入重新渲染的流程，在这里return false可以阻止组件的更新
3.因为react父组件的重新渲染会导致其所有子组件的重新渲染，
这个时候其实我们是不需要所有子组件都跟着重新渲染的，因此需要在子组件的该生命周期中做判断
```
- componentWillUpdate (nextProps,nextState)
```
shouldComponentUpdate返回true以后，组件进入重新渲染的流程，
进入componentWillUpdate,这里同样可以拿到nextProps和nextState。
```
- componentDidUpdate(prevProps,prevState)
```
组件更新完毕后，react只会在第一次初始化成功会进入componentDidmount,
之后每次重新渲染后都会进入这个生命周期，
这里可以拿到prevProps和prevState，即更新前的props和state。
```

- render()
```
render函数会插入jsx生成的dom结构，react会生成一份虚拟dom树，
在每一次组件更新时，在此react会通过其diff算法比较更新前后的新旧DOM树，
比较以后，找到最小的有差异的DOM节点，并重新渲染
```

### 3.新增生命周期

- getDerivedStateFromProps(nextProps, prevState)
```
代替componentWillReceiveProps()。
老版本中的componentWillReceiveProps()方法判断前后两个 props 是否相同，
如果不同再将新的 props 更新到相应的 state 上去。这样做一来会破坏 state数据的单一数据源，
导致组件状态变得不可预测，另一方面也会增加组件的重绘次数。
```
```
// before
componentWillReceiveProps(nextProps) {
  if (nextProps.isLogin !== this.props.isLogin) {
    this.setState({ 
      isLogin: nextProps.isLogin,   
    });
  }
  if (nextProps.isLogin) {
    this.handleClose();
  }
}

// after
static getDerivedStateFromProps(nextProps, prevState) {
  if (nextProps.isLogin !== prevState.isLogin) {
    return {
      isLogin: nextProps.isLogin,
    };
  }
  return null;
}

componentDidUpdate(prevProps, prevState) {
  if (!prevState.isLogin && this.props.isLogin) {
    this.handleClose();
  }
}
```
- getSnapshotBeforeUpdate(prevProps, prevState)
```
代替componentWillUpdate。
常见的 componentWillUpdate 的用例是在组件更新前，
读取当前某个 DOM 元素的状态，并在 componentDidUpdate 中进行相应的处理。
```
## 10.constructor中super与props参数一起使用的目的是什么

#### 使用了super(props)，可以从this上取到props
```
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        console.log(this.props);  // Prints { name: 'sudheer',age: 30 }
    }
}
```

#### 不使用super(props)，从this上取不到props
```
class MyComponent extends React.Component {
    constructor(props) {
        super();
        console.log(this.props); // Prints undefined
        // But Props parameter is still available
        console.log(props); // Prints { name: 'sudheer',age: 30 }
    }

    render() {
        // No difference outside constructor
        console.log(this.props) // Prints { name: 'sudheer',age: 30 }
    }
}
```

## 11.React中createElement 和 cloneElement 的区别

## 12.虚拟DOM的diff算法怎么个过程

## 13.key的作用
## 14.React Fiber工作原理和相关概念
https://github.com/xiaoxiaosaohuo/Note/issues/39
