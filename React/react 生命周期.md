# react 生命周期



>推荐阅读;https://zhuanlan.zhihu.com/p/38030418

![mark](http://qiniu.wind-zhou.com/blog/210412/fc2LCf5Fa4.png?imageslim)	

## **简介：**

- 在组件的整个生命周期中，随着维护的props或者state发生改变， 它的DOM表现也将有相应的改变，一个组件就是个状态机，对于特定的输入，它总会返回致的输出。
- react为每个组件提供了生命周期的**钩子函教**去**响应不同的时刻**---实例化期间、存在期及销毁时。

**组件的三个时期：**

- 实例化期
- 存在期
- 销毁时

## 实例化期

### （1）getDefaultProps

**作用**：这个方法是用来设置组件默认的props, 组件生命周期只会调用次

但是只适合React.createClass直接创建的组件，使用ES6/ES7创建的这个方法不可使用，ES6ES7可以使用下面方式

```js
es7
class Component{
    static defaultProps={}
}

es6 

Componnet.defaultProps


```



### （2）componentWillMount

**作用**：做一些组件内部的初始化工作。（也是render调用前最后一次改变state的机会）

整个生命周期内只会调用一次。

### （3）render

**作用**：解析生成虚拟dom，并渲染成最终效果。

### （4）componentDidMount （常用）

**触发时机**：第一次直染成功过后，组件对应的DOM已经添加到页面后调用

**作用**：这个阶段表示组件对应的DOM已经存在，我们可以在这个时候做一些依赖DOM的操作或者其他的些如**请求数据**， 添加订阅和**第三方库整合的操作**。

如果嵌套了子组件，子组件会比父组件优先直染，所以这个时候可以获取子组件对应的DOM （通过ref）。



> 假如要在react里使用swiper，则需要在componentDidMount方法里
>
> 假如有ajax请求，也要写在这里面。
>
> **前后端数据交互和第三方插件设置。**



**react中数据为什么要在componentDidMount中调用?**

-  constructor()中获取数据，如果时间太长，就会导致组件渲染不出来。constructor主要目的为了初始化组件的state，并不做数据加载的工作。
- 也不放在componentWillMount，因为如果使用SSR (服务模道染) componentWillMount会执行两次， 一次在服务端，一次在客户端，而componentDidMount不会。
- React16之后采用了Fiber架构。只有componentDidMount生命周明活数是确定被执行一次的，类似
  componentWillMount的生命周期都有可能执行多次，所以不在这些生命周期中做有副作用的操作，比如请求数据之类。
- **官方设计这个componentDidMount方法就是用来加载外部数据用的，或处理其他的副作用代码。**

### 示例

有一个p标签，opacity循环变化，点击卸载按钮后，所有内容消失。

```js
import React from "react"
import ReactDOM from 'react-dom';

class Instance extends React.Component{
        state={ 
            opacity:0.5,
        }

        death=()=>{
            //卸载组件
            ReactDOM.unmountComponentAtNode(document.getElementById("root"))
        }

        componentDidMount(){//挂载完成之后打开定时器
           this.timmer= setInterval(()=>{  //为了后面可以清除，将定时器的标识设置为类的属性
                let opacity=this.state.opacity
                opacity-=0.1;
                if(opacity<0) opacity=1
    
                this.setState({
                    opacity:opacity
                })
            },200)
        }

        componentWillUnmount(){ //组件将要写在结束前，结束计时器  (收尾工作，在销毁期)
            clearInterval(this.timmer )  //这里的计时器不过不清，会出现报错，已经卸载的组件开不了计时器
        }

    render(){
        console.log("render")
        return(
            <div>
                 <div  className='test' style={  {opacity:this.state.opacity}}>生活充满诗意</div>
                  <button onClick={this.death}>点击隐藏上面</button>

            </div>
        )
    }
}

export default Instance

```

上面定时器的初始化在componentWillUnmount函数中，定时器的清除写在componentWillUnmount中。

## 存在期

​	

![mark](http://qiniu.wind-zhou.com/blog/210412/hjL63hh9KH.png?imageslim)

### （1）componentWillReceiveProps(newProps)

**作用**：这个组件可以根据获取的属性来修改状态

>**注**：
>
>这个属性一般是父组件的状态改变，导致子组件的更新，这是使用的是props、传值。
>
>**这个函数，第一次渲染时不会调用，只有再次更新时才会调用**

### （2）shouldComponentUpdate(newProps,newState)

**作用**：决定是否重渲染组件，如果返回false,那么不会重渲染组件，默认为true

> **shouldComponentUpdate是react性能优化非常重要的一环。**组件接受新的state或者props时调用，我们可以设置在此对比前后两个props和state是否相同，如果相同则返回false阻止更新，因为相同的属性状态定会生成相同的dom树，这样就不需要创造新的Jdom树和的dom树进行df算法对比，节省大量性能，尤其是在dom结构复杂的时候



**触发时机**：接收到新属性或setState时调用（forceUpdate会跳过这步）



注：当父组件更新时，子组件也默认跟着更新，但有时子组件的状态并未改变，这是我们可以阻止其更新。

### （3）componentWillUpdate

**作用**：可以执行更新前的操作

**触发时机**：组件确认会更新后

>注：**这里会禁止在更新state**，否则会在次跳回shouldComponentUpdate，形成死循环



### （4）componentDidUpdate

作用：组件更新完成后，访问真实的DOM



## 销毁期

### componentWillUnMount

**作用**：组件卸载前执行一些清理工作，例如：此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 `componentDidMount()` 中创建的订阅等。（垃圾回收）

>注：`componentWillUnmount()` 中**不应调用 `setState()`**，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

## react新的生命周期

![mark](http://qiniu.wind-zhou.com/blog/210412/b163G4FLkH.png?imageslim)



## 实例化期

### （1）constructor

**作用**：加载时调用一次。可以初始化state

## getDerivedStateFromProps(props,state)

```jsx
static getDerivedStateFromProps(props, state)
```

> 注：这个钩子函数，必须加上static，特使属于类的方法，而不是示例的方法

**调用时机**：**初始挂载及后续更新时都会被调用**

**作用**：它应返回一个对象来更新 state，如果返回 `null` 则不更新任何内容（state）。



> 此方法适用于[罕见的用例](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state)，即 **state 的值在任何时候都取决于 props**。例如，实现 `<Transition>` 组件可能很方便，该组件会比较当前组件与下一组件，以决定针对哪些组件进行转场动画。



派生状态会导致代码冗余，并使组件难以维护。 [确保你已熟悉这些简单的替代方案：](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

- 如果你需要**执行副作用**（例如，数据提取或动画）以响应 props 中的更改，请改用 [`componentDidUpdate`](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)。
- 如果只想在 **prop 更改时重新计算某些数据**，[请使用 memoization helper 代替](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization)。
- 如果你想**在 prop 更改时“重置”某些 state**，请考虑使组件[完全受控](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-controlled-component)或[使用 `key` 使组件完全不受控](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-uncontrolled-component-with-a-key) 代替。

此方法无权访问组件实例。如果你需要，可以通过提取组件 props 的纯函数及 class 之外的状态，在`getDerivedStateFromProps()`和其他 class 方法之间重用代码。



### （2）render

**作用**：解析生成虚拟dom，金子那个diff算法，并渲染成最终效果。



### （3）componentDidMount

**和旧的声明周期一样。**





## 更新期

### （1）getDerivedStateFromProps(props,state)

和之前一样



### （2）render

### （3）componentDidUpdate

## 销毁期

### （1）componentWillUnMount

### （2）componentDidCatch（err,info）

**作用**：子组件抛出错误后，便会触发，用于捕获错误，打印错误等



## 新生命周期getDerivedStateFromProps的作用

- 变更原因原来( React v16.0前)的生命周期在React v16推出的Fiber之后就不合适了，因为如果要开启async rendering，在render函数之前的所有函数，都有可能被执行多次
- 如果开发者开了async rendering，而且又在以上这些render前执行的生命周期方法做AJAX请求的话，那AJAX将被无谓地多次调用。而且在componentWilMount里发起AJAX，不管多快得到结果也赶不上首次render，而且componentWillMount在服务器端渲染也会被调用到所以除了shouldComponentUpdate，其他在render函数之前的所有函数( componentWiliMount ，componentWillReceiveProps ，componentWillUpdate) 都被
  getDerivedStateFromProps替代
- **也就是用一个 静态函数getDerivedStateFromProps来取代被deprecate的几个生命周期函数，就是强制开发者在render之前只做无副作用的操作（例如请求ajax），而且能做的操作局限在根据props和state决定新的state**













