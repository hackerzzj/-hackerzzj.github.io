







# 1、react初步了解

### 1、特点

- 声明式 ：我们只需要更新数据就可以了，不用亲自操作dom元素。
- 组件化 ：将页面分为一个个的组建，易于编写和维护。

### 2、高效的原因

- 虚拟dom，不总是直接操作DOM。以前是一个一个修改DOM，虚拟DOM与DOM对象一一对应，更改虚拟DOM并不会直接更改DOM，而是等待都更改完成，批量更改。
- DOM Diff 算法，最小化页面重绘。

### 3、引入库文件

​		[开源库](www.bootcdn.cn)

​		react-dom.development.js（提供操作DOM对象的react扩展库）、react.development.js(react核心库)、babel.js(把ES6改为ES5或解析jsx语法为js语法)

```react
<div id="test">
    
</div>

<script type="text/javascript" src="../js/react.development.js"></script>
<script type="text/javascript" src="../js/react-dom.development.js"></script>
<script type="text/javascript" src="../js/babel.js"></script>

<script type="text/babel">
//1、创建虚拟DOM元素对象
var vDom = <h1>Hello React!</h1>
//2、将虚拟DOM渲染到页面真是DOM对象中
ReactDOM.render(vDom,document.getElementById('test'))
</script>

```

# 2、了解jsx

### 1、不使用jsx创建虚拟DOM对象

```react
var element = React.creatElement('h1',{id:'myTitle'},'hello')
```

如果要使用变量中定义的内容，要使用{ }将变量包起来。

再js中可以使用debugger来设置断点。

在React组件的构造函数中设置this.state来初始化state。

### 2、自定义组件

#### 1、定义组件

方式一、工厂函数组件

function MyComponent(){

​	return 

}

方式二、es6类组件

class MyComponent2 extends React.Component{

​	render(){

​		return 

​	}

}

#### 2、渲染组件标签

ReactDOM.render(<MyComponent/>,document.getElementById('example1'))

#### 3、组件的三打属性

- ```javascript
  const {isNext} = this.state //es6写法，等同年与 const isNext = this.state.isNext
  ```

- ```js
  const isNext = !this.state.isNext
  this.setState({isNext}) //es6写法,等同于isNext = isNext
  ```

###### ①state

​		是一个对象，如果用它则必须使用类组件的方式。在constructor（）方法中定义。可以使用setState（{xxx  = xxx}）方式来更新数据，用这种方式更新数据，所有的用到该数据的地方都会进行更新。

###### ②props

​		是一个对象，用来将数据从组件外传入到组件内部。

**指定默认值**

```js
//指定属性默认值
Person.defaultProps = {
    sex: '男',
    age:18
}
```

​	**props验证**

​	React.PropTypes 在 React v15.5 版本后已经移到了 **prop-types** 库。

```js
<script src="https://cdn.bootcss.com/prop-types/15.6.1/prop-types.js"></script>
```

```js
//对props中的属性值济宁类型限制和必要性限制
Person.propTypes = {
    name:PropTypes.string.isRequired,
    age:PropTypes.number
}
```

```js
/*
*  ...的作用
* 1、打包 
* 2、解包
* ReactDOM.render(<Person {...p1}/>,document.getElementById('test'))
* */
```

###### ③refs

**作用**:	React 支持一种非常特殊的属性 **Ref** ，你可以用来绑定到 render() 输出的任何组件上。

这个特殊的属性允许你引用 render() 返回的相应的支撑实例（ backing instance ）。这样就可以确保

在任何时间总是拿到正确的实例。

```js
<input ref = ‘content'></input>

const input = this.refs.content
```



```react
{{opacity:opacity}}
```

外边的大括号代表进行书写js文件，里面的括号代表这是一个对象。



#### react ajax

react本身没有提供ajax相关的代码。

解决方法：

- 1、jQuery：比较重，不建议使用
- 2、axios：轻量级，建议使用
  - 封装XmlHttpRequest对象的ajax
  - promise风格
  - 可以用在浏览器端和node服务器端

- 3、fetch：原生函数，但老版本浏览器不支持
  - 不再使用XmlHttpRequest对象提交的ajax请求
  - 为了兼容低版本的浏览器，可以引入兼容库fetch.js

#### 兄弟组件之间传递信息

组件：pubsub-js

```js
import PubSub from 'pubsub-js'

PubSub.publish('name',data)

PubSub.subscribe('name',(name,data)=>{
	hanshu 

})
```

#### 如何编写路由效果

1. 编写路由组件
2. 在父路由组件中指定
   - 路由链接：<NavLink>
   - 路由：<Route>

#### Redux

![image-20200624105238185](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200624105238185.png)

1.安装redex

​		npm install --save redux

2.API

- creatStore()

3.对象

- store
  - 作用:redux库最核心的管理对象
  - 内部属性
    - state 公共状态
    - reducer 改变状态的函数
  - 方法
    - getState()
    - dispatch(action)
    - subscribe(listener)
  - 编码
    - store.getState() //获取状态数据
    - store.dispatch({type:'INCREMENT',number}) //进行状态数据更改
    - store.subscribe(render) //状态数据更改后要重新执行渲染

4.核心概念

- action(用来表示执行行为的对象)

  - 属性
    - type:标识属性,值为字符串,唯一
    - xxx:数据属性,值类型任意 
  - Action Creator(创建Action的函数)

- reducer(根据老的state和action,产生新的state的纯函数)

  - function xxx(state=0,action){ //根据action的type判断执行那些操作}

``` js
//1.创建reducer
import {INCREMENT,DECREMENT} from "./action-type";

export  function counter(state = 0,action) {
    console.log('counter()',state,action)
    switch (action.type) {
        case INCREMENT:
            return state + action.data
            break
        case  DECREMENT:
            return state - action.data
            break
        default:
            return state
    }
}
//2.创建store
import {createStore} from "redux";
import {counter} from "./reducers";
const store = createStore(counter)
//2.1传递和使用store
function render() {
    ReactDOM.render(<App store={store}/>,document.getElementById('root'))
}
//2.2初始化渲染
render()
//2.3订阅监听(store中的状态变化了,就会自动调用)
store.subscribe(
    render()
)

//3.actionsCreactor actions工厂创建
import {INCREMENT,DECREMENT} from "./action-type";
export const increment = (number)=>({type:INCREMENT,data:number})
export const decrement = (number)=>({type:INCREMENT,data:number})
//4.插件中使用
import React,{Component} from "react";
import {INCREMENT,DECREMENT} from "../action-type";
import * as actions from "../actions"
export default class App extends Component{
    increment = ()=>{
        //1.读取选择的值
        const number = this.select.value*1
        //2.调用store的方法更新状态
        this.props.store.dispatch(actions.increment(number))
    }

    decrement = ()=>{
        //1.读取选择的值
        const number = this.select.value*1
        //2.调用store的方法更新状态
        this.props.store.dispatch(actions.decrement(number))
    }

    incrementIfOdd = ()=>{
        //1.读取选择的值
        const number = this.select.value*1
        //2.获取count的值
        const count = this.props.store.getState()
        //3.更新count
        if (count%2==1){
            this.props.store.dispatch(actions.increment(number))
        }
    }

    incrementAsync = ()=>{
        //1.读取选择的值
        const number = this.select.value*1
        //2.加上count的值
        const count = this.props.store.getState()
        //3.更新count
        setTimeout(()=>{
            this.props.store.dispatch(actions.increment(number))
        },1000)
    }

    render() {
        const count = this.props.store.getState()
        return(
            <div>
                <p>click {count} times</p>
                <div>
                    <select ref={select=>this.select = select}>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                    </select>&nbsp;
                    <button onClick={this.increment}>+</button>&nbsp;
                    <button onClick={this.decrement}>-</button>&nbsp;
                    <button onClick={this.incrementIfOdd}>increment if odd</button>&nbsp;
                    <button onClick={this.incrementAsync}>increment async</button>&nbsp;
                </div>
            </div>
        )
    }
}
```

  



#### 使用react-redux简化(就是使用传参的方式传递state和改变state函数)

- 引入  ```npm install --save react-redux```
- 在index中 用Provider包装组件

```js
import {Provider} from 'react-redux'
ReactDOM.render(
	<Provider store = {store}> //store不在给App组件,而是给Provider组件
    	<App/>
    </Provider>
)
```

- 在App组件中

```js
import PropTypes from 'prop-types'
import {connect} from 'react-redux'
import {increment,decrement} from "../actions";
class App extents React.Component{//不是直接暴露出来
    static propTypes = { // 判断参数
        count:PropTypes.number.isRequired,
        incrememt:PropTypes.func.isRequired,
        decrement:PropTypes.func.isRequired
    }
    ....
    incrementIfOdd = ()=>{
        //1.读取选择的值
        const number = this.select.value*1
        //2.获取count的值
        const count = this.props.count//这里获取的是props中的count不是在store获取
        //3.更新count
        if (count%2==1){
            this.props.incrememt(number)//这里直接使用参数中的函数,不是在使用store.dispatch(action)
        }
    }
    ....
    
}
export default connect(
	state=>({count:state}),
    {increment,decrement}
)(<App/>) //这里使用connect函数给App插件绑定参数,第一个括号是执行connect函数,参数为要绑定的参数,返回是一个函数.然后在把App插件作为参数传递过去,返回一个绑定好参数的插件.使用export导出去
```

- react-redux分为component和container
  - component不包含redux内容的部分
  - container是包含redux内容的部分

#### redux异步编程

- 下载redux插件(异步编程中间件)

  ```npm  install --save redux-thunk```

- 在store.js中添加applyMiddleware和thunk
```js
  import {createStore,applyMiddleware} from "redux";
  import {counter} from "./reducers";
  import thunk from 'redux-thunk'
  
  const store = createStore(counter,applyMiddleware(thunk)) //应用上异步中间件
  export  default store
```

- 在插件中异步函数使用this.props.incrementAsync()函数

```js
  incrementAsync = ()=>{
      //1.读取选择的值
      const number = this.select.value*1
      this.props.incrementAsync(number)
  }
```

- 在container中app.js中(也就是绑定参数的函数的js文件)添加incrementAsync函数

```js
import React from "react";
import {connect} from "react-redux";
import {decrement, increment,incrementAsync} from "../actions";
import Counter from "../components/counter";
export default  connect(
    state =>({count:state}),
    {increment,decrement,incrementAsync}
)(Counter)
```

- 在actions文件中添加函数(注意异步函数返回的是一个函数)

```js
/*
包含所有action creator
 */
import {INCREMENT,DECREMENT} from "./action-type";
import {number} from "prop-types";
//同步的action都是返回一个对象
//异步的action都是返回一个函数
//增加
export const increment = (number)=>({type:INCREMENT,data:number})
//减少
export const decrement = (number)=>({type:DECREMENT,data:number})
//异步增加
export const incrementAsync = (number)=>{
    return dispatch=>{
        //异步的代码
        setTimeout(()=>{
            dispatch(increment(number))
            // 1s之后才去分发一个action
        },1000)
    }
}
```