### 1.真实DOM 和 虚拟DOM 的区别

- 真实DOM
    - 更新慢
    - 消耗内存多
    - DOM操作代价很高
    - 可以直接更新HTML
    - 如果元素更新，则创建新DOM

---
- 虚拟DOM
    - 更新更快
    - 消耗更少的内存
    - DOM操作非常简单
    - 无法直接更新 HTML
    - 如果元素更新，则更新 JSX
---

### 2.React 特点

- 使用的是虚拟DOM，而不是真正的DOM
- 可以进行服务端渲染
- 遵循单项数据流或数据绑定

### 3.React有哪些限制

- 只是一个库，不能算上一个完整的框架
- 学习成本高
- 编码复杂，使用的是内联模板和JSX

### 4.JSX是什么

```
是JavaScript XML的简写
是一种JavaScript的语法扩展，运用在React架构中
- JSX执行更快
- 类型安全的，在编译过程中能发现错误
const element = <h1>Hello, world!</h1>;
这样的既不是字符串也不是HTML
我们叫做JSX

如何使用：
我们需要用像 Babel 这样的 JSX 转换器将 JSX 文件转换为 JavaScript 对象，
然后再将其传给浏览器
```

### 5.React 中 render() 的目的
```
每个React组件强制要求必须有一个 render()
返回一个 React 元素，是原生 DOM 组件的表示
必须包裹在一个根标签内
```

#### 此函数必须保持纯净，即必须每次调用时都返回相同的结果

### 6.Props
```
Props 是 React 中属性的简写。它们是只读组件，必须保持纯，即不可变。
它们总是在整个应用中从父组件传递到子组件
子组件永远不能将 prop 送回父组件。
这有助于维护单向数据流，通常用于呈现动态生成的数据
```

### 7.React中的组件主要分为无状态组件和有状态组件两类

1. 无状态组件
```
- 无状态组件主要用来定义模板，接收来自父组件props传递过来的数据，使用{props.xxx}的表达式把props塞到模板里面
- 无状态组件应该保持模板的纯粹性，以便于组件复用

var Header = (props) = (
    <div>{props.xxx}</div>
);
```

2. 有状态组件

```
有状态组件主要用来定义交互逻辑和业务数据
如果用了Redux，可以把业务数据抽离出去统一管理），
使用{this.state.xxx}的表达式把业务数据挂载到容器组件的实例上
然后传递props到展示组件，展示组件接收到props，把props塞到模板里面

class Home extends React.Component {
    constructor(props) {
        super(props);
    };
    render() {
        return (
            <Header/>  //也可以写成<Header></Header>
        )
    }
}
```
