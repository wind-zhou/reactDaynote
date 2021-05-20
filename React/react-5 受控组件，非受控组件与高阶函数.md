# 受控组件，非受控组件与高阶函数

>开发中一推荐使用受控组件，非受控组件里可能会定义多个ref，但是官方建议勿过度使用ref。

## 受控组件

###**简介：**

在HTML中，表单元素(如\<input>、 \<textarea> 和\<select>)之类的**表单元素通常自己维护state**,并根据用户输入进行更新。而在React中，可变状态(mutable state)通常保存在组件的state属性中，并且只能通过使用setState()来更新。

**我们可以把两者结合起来，使React的state成为“唯一 数据源**。**渲染表单的React组件还控制着用户输入过程中表单发生的操作**。被React以这种方式控制取值的表单输入元素就叫做受控组件。



优点：**实时获取数据**时会使用到。

> 其实这里属于**双向绑定**（state的变化会更新表单得分数据，input的数据也会更新state）
>
> 但有些复杂,每个input都需绑定一个onchange函数

### **示例：**

```jsx
// 受控组件

import React from "react" 

class ControlCom extends React.Component{
    constructor(props){
        super(props)
        this.state={name:''}
    }


    handleChange=(event)=>{  //注意这里填入一个event是做什么呢？可不可以不用？如果不用event而使用this，会出现指向问题
        this.setState({name:event.target.value})

    }

    handleSubmit=(event)=>{  //注意这里填入一个event事件作为参数，用于防止默认的提交动作
        event.preventDefault();
        alert('提交的名字'+this.state.name)
        
    }

    render(){
        return(
            <form onSubmit={this.handleSubmit}>
                <label > 名字：<input value={this.state.name}  onChange={this.handleChange} type="text"/></label>
                <input type="submit" value="提交" />
             </form>
        )
    }
}


export default ControlCom
```

>使用受控组件，实时的将数据放在state中，最好组件的state中就有该字段。
>
>例如存储input的值进入state中

## 非受控组件

### 简介：

**受控组件的替代品：**

有时使用受控组件会很麻烦，因为你**需要为数据变化的每种方式都编写事件处理函数**，并通过一个React组件传递所有的输入state。当你将之前的代码库转换为React或将React应用程序与非React库集成时，这可能会令人厌烦。在这些情况下，你可能希望使用非受控组件，这是实现输入表单的另一种方式。



在大多数情况下，**我们推荐使用受控组件来处理表单数据**。在一个受控组件中，**表单数据是由React组件来管理的**。另一种替代方案是使用非受控组件，**这时表单数据将交由DOM节点来处理**。

>受控组件：表单数据由react管理
>
>非受控组件：表单数据由DOM节点管理 



要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，**你可以使用ref来从DOM节点中获取表单数据**。

因为**非受控组件将真实数据储存在DOM节点中**，所以在使用非受控组件时，有时候反而**更容易同时集成React和非React代码**。如果你不介意代码美观性，并且希望快速编写代码，使用非受控组件往往可以减少你的代码量。否则，你应该使用受控组件

### 示例：

```jsx
//非受控组件

import React from "react"


class UnControlCom extends React.Component {
    constructor(props) {
        super(props)
        this.ageRef=React.createRef()
    }

    changeData=(event)=>{
        console.log(this)

        event.preventDefault();
        alert("输入的年龄为："+this.ageRef.current.value)

    }

    render(){
        return(
            <form  onSubmit={this.changeData}>
                <label > 年龄：<input    ref={this.ageRef}  type="text"/></label>
 {/* 为什么上上面标签内要写定义ref的值呢？因为你不用ref回去不到这个input的值呀！，用e.targrt是不好使了 ，因为绑定在了form身上*/}
 {/* 之前受控组件，在input本身绑定了个onchange事件*，所以那个可以使用e.target获取*/}

                <input type="submit" value="提交" />
            </form>
        )
    }
}

export default  UnControlCom
```

>**简单理解非受控组件：就是input的数据现用现取。（触发onsummit时才获取）**

>**注意**：文件输入的input始终是一个非受控组件

## 高阶函数和函数柯里化

> **最终受控组件的写法是高阶函数**

###**前言：**

在获取表单数据时。推荐使用受控组件进行获取但是每个表单项都需要一个处理函数将表单数据存储到State中，那有没有简化的方式呢?

###**使用高阶函数：**

如果一个函数符合下面任何一个，那该函数就是高阶函数
1. 若函数接收的参数是一个函数， 那么该函数就可以称之为高阶函数
2. 若函数调用的返回值依然是一个函数， 那么该函数就可以称之为高阶函数

>常见的高阶函数：
>
>promise、setTimeout、arr.map()、arr.filter()等

### 示例：

```jsx
//使用高阶函数，写受控组件

import React from 'react';

class HighFun extends React.Component {
    constructor(props) {
        super(props)
        this.state={name:'',pwd:''}
    }

    handleData=(data)=>{   //高阶函数
    //作用：根据传入的参数，返回一个只针对于该参数的setState函数
        return( (e)=>{
            this.setState({[data]:e.target.value})
            }   
        )
    }

    handleSunmit=(e)=>{  //验证
        event.preventDefault();
        let {name,pwd}=this.state  //解构赋值
        alert('姓名：'+name+'密码'+pwd)
    }


    render(){
        return(
            <form onSubmit={this.handleSunmit}>
                <label >  姓名：<input value={this.state.name} onChange={this.handleData("name")} type="text"/></label>
                <label >  密码：<input value={this.state.pwd} onChange={this.handleData("pwd")} type="password"/></label>
                <button>登录</button>
            </form>
        )
    }
}

export default HighFun


```





### 函数柯里化

**定义：**

通过函数调用继续返回函数的方式，**实现多次接收参数最后统一处理**的函数。

>前面高阶函数的例子就是接受了多个参数，最后统一处理。
>
>分别接收到了，e和data，最后使用` this.setState({[data]:e.target.value})`统一处理。

**高阶函数是是函数柯里化的一种实现。**



**前面受控组件onchange事件，其实可以使用非柯里化写法，如下。**

```jsx
//使用高阶函数，写受控组件

import React from 'react';

class HighFun extends React.Component {
    constructor(props) {
        super(props)
        this.state={name:'',pwd:''}
    } 
    //非柯里化写法
    
    handleData=(dataType,event)=>{   //高阶函数
        //作用：根据传入的参数，返回一个只针对于该参数的setState函数
                this.setState({[dataType]:event.target.value})
        }

    handleSunmit=(e)=>{  //验证
        event.preventDefault();
        let {name,pwd}=this.state  //解构赋值
        alert('姓名：'+name+'密码'+pwd)
    }

    render(){
        return(
            <form onSubmit={this.handleSunmit}>
               <label >  姓名：<input value={this.state.name} onChange={event=>{this.handleData("name",event)}} type="text"/></label>
                <label >  密码：<input value={this.state.pwd} onChange={event=>{this.handleData("pwd",event)}} type="password"/></label>
                <button>登录</button>
            </form>
        )
    }
}

export default HighFun
```



>对比：
>
>前面之所以使用高阶函数有两点：
>
>（1）onchange事件绑定的必须是一个函数
>
>（2）设置state的属性需要传入两个参数，但无法同时传入。
>
>​	**（dateType很容易传入，但是e没办法，因为是react生成）**
>
>那么不使用高阶函数的写法，这两点问题要怎么解决呢？
>
>（1）直接给onchange绑定个箭头函数---解决了第一个问题
>
>（2）由于直接绑定了箭头函数，因此箭头函数的形参可以是event，这时便可将两个参数同时传入。
>
>​	（**注意：此时箭头函数体内调用了this.handleData方法，所谓的两个参数同时传入，是相对于this.handleData来	说**）
>
>b

