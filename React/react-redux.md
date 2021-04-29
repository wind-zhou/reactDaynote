# react-redux

React和Redux事实上是两个独立的产品，可以使用React而不使用Redux，也可以使用Redux而不使用React，而一个名叫react-redux的库是将react和redux结合的一个库; 通过对redux api的封装形成基于react的组件，操作更佳简洁

## 原理

![mark](http://qiniu.wind-zhou.com/blog/210418/2iG1f5F0ha.png?imageslim)

##安装

```js
npm install redux -D
npm install react-redux -D
```

## 核心API

- **Provider**----提供包含store的上下文，将store传递给内部组件，内部组件可以享有这个store，并可以对其state进

  行更新。

- **connect**  ----连接容器组件与UI组件

  

### react-redux将所有组件分成两大类

###1. UI组件

```js
1) 只负责 UI 的呈现，不带有任何业务逻辑

2) 通过props接收数据(一般数据和函数)

3) 不使用任何 Redux 的 API

4) 一般保存在components文件夹下
```

###2. 容器组件

​	1) 负责管理数据和业务逻辑，不负责UI的呈现

​	2) 使用 Redux 的 API

​	3) 一般保存在containers文件夹下

## 核心API



### 1. Provider：

**让所有组件都可以得到state数据**

![mark](http://qiniu.wind-zhou.com/blog/210418/fAL1g7EC38.png?imageslim)

### 2. connect

**用于包装 UI 组件生成容器组件**

![mark](http://qiniu.wind-zhou.com/blog/210418/7jGGf9d0k6.png?imageslim)

###3. mapStateToprops：

**将外部的数据（即state对象）转换为UI组件的标签属性**

![mark](http://qiniu.wind-zhou.com/blog/210418/1gFIFggf5m.png?imageslim)

>**注：**
>
>- 形参是state（就是store的state）
>
>- 这里返回的应该是一个对象，因为要挂在props上。



### 4. mapDispatchToProps

**将分发action的函数转换为UI组件的标签属性**

就是将操作状态的方法挂在了子组件的props上。

看下面例子：

```jsx
function mapDisPatchToProps(dispatch) {
  return {
    addProject: (value, id) => {
      let proItem = {
        id,
        name: value,
        isComplete: false
      };
      dispatch(add_project(proItem));    //注：在这个位置分发 dispatch(action)
    },
    delProject: id => {
      dispatch(del_project(id));
    }
  };
}
```



>注：
>
>- 参数为dispatch
>- 返回的是一个个操作状态的方法



>思考：为什么容器组件向UI组件传值，还要使用个回调函数呢？
>
>很简单，因为react-redux规定的这种语法，没办法使用传统的父向子传值的形式。
>
>传统的
>
>```js
>
>.....
>render(){
>    return(
>    <div>
>      	  <Son  name="lily"> </Son>
>     </div>
>    )
>}
>.....
>
>```
>
>但是使用connect封装后，发现找不到书写的位置。所以人家规定了用回调函数的形式传。



## 实战练习

一个面试题：

![mark](http://qiniu.wind-zhou.com/blog/210418/h8LLh6Jh6B.png?imageslim)





>**源码：**
>
>https://github.com/wind-zhou/-CMS-



>思考：
>
>（1）上面项目可不可以不使用redux？
>
>**当然可以**！使用redux与否其实就是state由谁管理的问题，大幸的项目使用redux会比较方便，像这种小案例根本没有使用redux的必要，面试时只会增加编程难度。如果仅使用react无非就是自己管理状态而已。会更简单。
>
>下面是一个react版本的答案：
>
>```js
>import React, { Component } from "react";
>
>let proId = 0;
>let isHideActive = false;
>class ReactCms extends Component {
>  constructor(props) {
>    super(props);
>    this.state = {
>      project: []
>    };
>  }
>
>  //   增加
>  addProject = () => {
>    proId++;
>    var newproject = [
>      ...this.state.project,
>      { name: this.input.value, id: proId, isComplete: false }
>    ];
>    this.setState({
>      project: newproject,
>    //   [proId]: "block"
>    });
>  };
>  //   删除
>  delProject = id => {
>    return () => {
>      let newPoject = this.state.project.filter(value => {
>        return value.id !== id;
>      });
>      console.log(newPoject);
>
>      this.setState({
>        project: newPoject
>      });
>    };
>  };
>
>  //   checkbox 激活
>  isCheckboxActive = id => {
>    return e => {
>      //   (1)找到该数据
>      let targetProject = this.state.project.filter(value => {
>        return value.id == id;
>      });
>      //   （2）把他的iscpmplete值为true
>      targetProject[0].isComplete = !targetProject[0].isComplete;
>
>      console.log(targetProject[0]);
>      if (targetProject[0].isComplete == true) {
>        e.target.parentNode.style.color = "#0f0";
>      } else {
>        e.target.parentNode.style.color = "#f00";
>      }
>    };
>  };
>
>  //   隐藏已完成
>  hide = () => {
>    isHideActive = !isHideActive;
>    let idList = [];
>    //   (1)找到所有已完成的项
>    let didItems = this.state.project.filter(value => {
>      return value.isComplete == true;
>    });
>    console.log(didItems);
>
>    //   (2)拿到所有项的id
>
>    didItems.map(value => {
>      idList.push(value.id);
>    });
>    console.log(idList);
>    // (3)将每个id在state里存一个状态
>    idList.map(item => {
>      if (isHideActive == true) {
>        this.setState({
>          [item]: "none"
>        });
>      } else {
>        this.setState({
>          [item]: "block"
>        });
>      }
>    });
>  };
>
>  clear = () => {
>    let idList = [];
>    //   (1)找到已完成的项
>    let didItems = this.state.project.filter(value => {
>      return value.isComplete == true;
>    });
>
>    //   （2）删除已完成的数据
>    didItems.map(value => {
>      idList.push(value.id);
>    });
>
>    let newproject = this.state.project.filter(value => {
>      return !idList.includes(value.id);
>    });
>    this.setState({
>      project: newproject
>    });
>  };
>
>  render() {
>    console.log(this);
>    let con = this.state.project.map(value => {
>      return (
>        <li
>          key={value.id}
>          style={{ color: "#f00", display: this.state[value.id] }}
>        >
>          <input type="checkbox" onClick={this.isCheckboxActive(value.id)} />
>          {value.name}
>          <button onClick={this.delProject(value.id)}>-</button>
>        </li>
>      );
>    });
>    return (
>      <ul>
>        <li>
>          <input
>            type="text"
>            ref={ele => {
>              this.input = ele;
>            }}
>          />
>          <button onClick={this.addProject}>增加</button>
>        </li>
>        {con}
>        <button onClick={this.hide}>隐藏已完成</button>
>        <button onClick={this.clear}> 清空已完成</button>
>      </ul>
>    );
>  }
>}
>
>export default ReactCms;
>
>```
>
>
>







