---
title: 'Mobx:React状态管理库（入门）'
date: 2020-12-01 11:26:32
tags: mobx
---

## React、mobx之间的关系

React和mobx之间相辅相成，react将应用状态转化为可渲染的组件树进行视图的更新，mobx用来存储和更新应用状态。

## mobx的基本使用

mobx的流程

![1521798430-5a42021889036_articlex](img\1521798430-5a42021889036_articlex.png)

更简单的来讲

![2784418971-595ed7833ff78_articlex](img\2784418971-595ed7833ff78_articlex.jpg)

​		Mobx中有三个核心概念，observable、observer、action。（为了简单没有涉及到computed等概）

- observable:通过observable(state)定义组件的状态，包装后的状态是一个可观察数据（Observable Data）.

- observer：通过observer（ReactComponent）定义组件。

- action：通过action来修改状态。

  ```javascript
  // 通过 observable 定义组件的状态
  const user = mobx.observable({
      name: "Jay",
       age: 22
  })
  
  // 通过 action 定义如何修改组件的状态
  const changeName = mobx.action(name => user.name = name)
  const changeAge = mobx.action(age => user.age = age)
  
  // 通过 observer 定义 ReactComponent 组件。
  const Hello = mobxReact.observer(class Hello extends React.Component {
          componentDidMount(){
              // 视图层通过事件触发 action
          changeName('Wang') // render Wang
      }
  
      render() {
                  // 渲染
              console.log('render',user.name);
          return <div>Hello,{user.name}!</div>
      }
  })
  
  ReactDOM.render(<Hello />, document.getElementById('mount'));
  
  // 非视图层事件触发，外部直接触发 action
  changeName('Wang2')// render Wang2
  // 重点：没有触发重新渲染
  // 原因：Hello 组件并没有用到 `user.age` 这个可观察数据
  changeAge('18')  // no console
  ```

  #### **observable**

  可观察数据（Observable Data）,可观察数据就是观察到数据的读取、写入，并进行拦截。

  Mobx提供了observable接口来定义可观察数据。定义的可观察数据，通常也是组件的状态。该方法接受一个参数，参数可以是原始数据类型、普通Object、Array、或者ES6的Map类型。返回一个observable类型的参数。

  ```js
  Array.isArray(mobx.observable([1,2,3])) === false //true
  mobx.isObservale(mobx.observable([1,2,3])) === true //true
  ```

  **注意**，数组经过observable包装后，就不是Array类型了，而是Mobx定义的一个特殊observable类型。

  虽然数据类型不一样，但是使用方式基本和原来一致（原始数据类型除外）
  
  ```js
  const observableArr = mobx.observable([1,2,3]);
  const observableObj = mobx.observable({name: 'Jay'});
  const observableMap = mobx.observable(new Map([['name','Wang']]));
  
  console.log(observableArr[0]) // 1
  console.log(observableObj.name) // Jay
  console.log(observableMap.get('name')) // wang
  ```
  
  可观察数据类型的原理是，在读取数据时，通过getter来拦截，在写入数据时，通过setter来拦截
  
  ```js
  Object.defineProperty(o,key,{
      get: function(){
          // 收集依赖的组件
          return value;
      },
      set: function(newValue){
          // 通知依赖的组件更新
          value = newValue
      },
  });
  ```
  
  在可观察数据被组件读取时，Mobx会进行拦截，并记录该组件和可观察数据的依赖关系。在可观察数据被写入时，Mobx也会进行拦截，并通知依赖它的组件重新渲染。
  
  #### **observer**
  
  observer接收一个React组件作为参数，并将其转变为响应式（Reactive）组件。
  
  ```js
  // 普通组件
  const Hello = mobxReact.observer(class Hello extends React.Component{
      render(){
          return <div>Hello,{user.name}!</div>
      }
  })
  // 函数组件
  const Hello = mobxReact.observer(()=>{
      <div>Hello,{user.name}!</div>
  })
  ```
  
  响应式组件，即当且仅当组件依赖的可观察数据发生改变时，组件才回自动响应，并重新渲染。
  
  #### **action**
  
  在Mobx中是可以直接修改可观察数据，来进行更新组件的，但是不建议这样做。如果在任何地方都修改可观察数据，将导致野蛮状态难以管理。
  
  所有对可观察数据的修改，都可以在action中进行。
  
  ```js
  const changeName = mobx.action(name => user.name = name)
  ```
  
  使用Mobx可以将组件状态定义在组件外部，这样，组件逻辑和组件视图很容易分离。兄弟组件之间的状态也很容易同步。另外，也不在需要手动使用shouldComponentUpdate进行性能优化了。

