# react 组件的三大属性

在react组件实例对象当中存在三个属性， 是我们在创建组件过程中经常使用到的，分别是**控制组件**
**更新的state**,**负责传值的props**以及用于**存储当前节点的refs**

- props  （负责传值）
- state （控制组件更新状态）
- refs 存储当前节点（**能不用就不用，表单中常用**）

## state状态

### 简介

组件免不了要与用户互动，**React的一大创新，就是将组件看成是一个状态机**，开始有一个初始状
态，然后用户互动，导致状态变化，从而触发重新渲染UI

React 里，只需更新组件的 state,然后根据新的 state重新渲染用户界面 (不要操作DOM).

>简单来说，react就是控制state里数据的更新来驱动ui的渲染

**那么问题来了：**

（1）状态在哪里?

（2）如何初始化状态?

（3）如何修改状态?



**状态在哪里？**

State是**在组件实例对象中**的，组件实例对象是由类生成的，所以**只有在类式组件中**才具有stale

**如何初始化?**

state是组件实例对象继承了react.component，所以要在**当前组件的构造器函数中进行初始化**，默认
值为null。

>**注：state可以在构造器中初始化，`this.state={}`，也可以使用简写形式,在构造器外初始化，直接`state={}`**

**如何修改状态?**

常用的通知react数据变化的方法是**调用setSate(data cllbalk)**,这个方法会合并data到this.state,并
重新渲染组件，渲染完成后，调用可选的callback回调， 大部分情况下不需要提供allback,因为
**react会负责把界面更新到最先状态。**

> **注意: setState()方法是 个异步方法**

this.setState():

- 组件状态改变时，可以通过this.setState修改状态

- setState方法**支持按需修改**，如state有两个字段，仅当setState传入的对象包含字段key
才会修改属性
- 每次**调用setState会导致重渲染调用render方法**
- **直接修改state不会重渲染组件**

请求的数据都存储在state中。 



>**严重注意**：状态不可直接更改，需要使用setState，否则react根本不嘞你。

### state 工作原理



![mark](http://qiniu.wind-zhou.com/blog/210411/5HfDl4heIb.png?imageslim)



这是一个React组件实现组件可交互所需的流程，**render()输出虚拟DOM,** **虚拟DOM转为DOM**,**再在DOM上注册事件**，**事件触发setState()修改数据**， **在每次调用setState方法时，React会自动执行render方法来更新虚拟DOM**,如果组件已经被渲染，那么还会更新到DOM中去。



##props

### 简介

Props相对于组件来说是外来属性， **使用props可以相件外部向组件内部传值**，类似与函数的传
参，他**经常用于组件之间的传值，一种父级向子级传递数据的方式**。



>**props是只读的。**

### 语法

直接在组件中添加属性即可，添加完毕后，传入的属性被保存到组件实例化对象的props属性中。

```js
 <TestProps me={this.state.name} hobby="swim"></TestProps>  
```

上面的例子，想组件传入了两个属性：（1）从父组件传过来的state.name （2）自定义的属性hobby



这是运行项目，查看一下\<TestProps/>组件的props值。

![mark](http://qiniu.wind-zhou.com/blog/210411/IAmAij383I.png?imageslim)



**接收到属性后，便可以使用`this.props.属性名`来进行相应处理。**



>props支持批量化的传递标签属性，例如:
>
>```jsx
>const p={name:'wind',age:24}
>ReactDom.render(<Person {...p} />,document.getElementById("root"))
>```
>
>在组件内，可以搭配解构赋值，提高开发效率。



### 扩展 类组件的构造器 constructor()

**开门见山**：类组件的constructor完全可以不写（开发中一般不写）

那么什么后需要写呢？

下面是官方文档的解释：

>在 React 组件挂载之前，会调用它的构造函数。在为 React.Component 子类实现构造函数时，应在其他语句之前前调用 `super(props)`。否则，`this.props` 在构造函数中可能会出现未定义的 bug。
>
>通常，在 React 中，构造函数仅用于以下两种情况：
>
>- 通过给 `this.state` 赋值对象来初始化[内部 state](https://zh-hans.reactjs.org/docs/state-and-lifecycle.html)。
>- 为[事件处理函数](https://zh-hans.reactjs.org/docs/handling-events.html)绑定实例

上面文档说明，constructor可以不写，因为state的设置可以写在构造器外面（简写形式），事件绑定可以使用箭头函数。

但是如果写了constructor，则要传入props参数，否则在构造器中操作this.props时会报错。



**所以说，一言以蔽之：constructor写不写，完全取决于你要不要在构造器中操作props属性。**



### 组件间的传值

#### 父组件向子组件传值

**直接使用props传参就可以。**



这里讨论两种情况：

| 父组件 |  子组件  |
| :----: | :------: |
| 类组件 |  类组件  |
| 类组件 | 函数组件 |



**组件间的传值分为两步走：1、父组件传值  2、子组件接收值**

传值时，方法都一样，都是利用props属性进行传值。

```jsx
<Son myname={this.state.name} sex={"man"} fun={this.transformData} > </Son>  //类组件 （子组件）

<SonFun x={this.state.name}></SonFun>   //函数组件  (子组件)
```

不过若子组件分别为类组件和函数组件时，他们接收值的方式有些不同。

(1)类组件直接使用`this.props.属性名`即可读取

(2)函数组件则需将props作为形参接收，然后再使用`props.属性名`读取

```jsx
function SonFun(props){
    return(
        <li>
            hello,我是摇摆洋{props.x}     {/* 读取穿过类的值*/}
        </li>
    )
}

```

#### 子组件向父组件传值

**通过回调函数传值。**

> 先在父组件中定义一个函数，用于接收子组件传过来的数据。然后使用props将该函数传递到子组件中，最后在子组件中调用该函数，同时传参。



#### 兄弟组件间的传值

**兄弟间的传值，相当于上面两者的结合，一个儿子Son1先将数据传给Father，再由Father传递给Son2**



### 综合示例

**演示兄弟组件间的传值**

组件结构：

Father---父组件

Son1----子组件

Son2----子组件

Son1和Son2互为兄弟组件



**Father.js**

```jsx
import React from "react"

import Son1 from "../son1/son1"
import Son2 from "../son2/son2"

class FatherSpecial extends React.Component{

    constructor(){
        super()
        this.state={hobby:'swim'}  // 父组件自身的状态
    }
    render(){
        return(
            <ul>

                <li>我是wind-zhou</li>
                <Son1 fun={this.transformData}> </Son1>  {/* （2）将父类定义的函数，通过props转递给子组件*/}
                <Son2  hobby={this.state.hobby}> </Son2>     {/* （3）将值从父组件的state传递给子组件Son2的props*/}
            </ul>
        )
    }


    transformData=(data)=>{ // （1）父组件定义的函数
        this.setState({hobby:data})
    }
}

export default FatherSpecial
```

**Son1.js**

```jsx
import React from "react"

class Son1 extends React.Component {
    constructor() {
        super()

        this.state={hobby:'吃面条'}
    }

    render(){
        return(
            <li>
                我是吕泽准
                <button onClick={this.giveData}>吕泽准献hobby</button>    {/* （2）子组件中调用封装后的方法*/}
            </li>
        )
    }

    giveData=()=>{     // （1）将父组件传过来的函数进行一封封装，封装成自己的方法
        this.props.fun(this.state.hobby)   //要注意这里传入的数据
    }
}

export default Son1
```

**Son2.js**

```jsx
import React from "react"

class Son2 extends React.Component {
    constructor() {
        super()
    }

    render(){
        return(
            <li>
                我是 摇摆羊<br/> 
                {this.props.hobby}       {/* 接收传递过来的参数*/}
            </li>
        )
    }

}

export default Son2
```

最开始时，三个组件的状态。

![mark](http://qiniu.wind-zhou.com/blog/210411/7KkLkkm7g5.png?imageslim)

点击完“吕泽准献hobby”后，父类的hobby应该变为“吃面条”，son2的hobby也应该变为“吃面条”。

下面我们来看一下。

![mark](http://qiniu.wind-zhou.com/blog/210411/LLhckJhglc.png?imageslim)



上面的箭头就是此时传值的数据流。

>几点总结：
>
>- 传值时会用到props属性
>- 子组件向父组件传值时的回调函数：这个函数其实是父组件的方法，父组件在定义时使用的是箭头函数定义，这样改函数内的this就指向了父组件的实力，这是最关键的地方，函数里写的是将数据写进父组件的state中，这样，即使将函数通过props转递给子组件，在子组件中调用时，操作的也是父组件的state，只不过数据使子组件的数据。
>- 在子组件接收到穿过来的回调函数后，一般会再次将其的封装为自己的方法，然后再在自己的内部元素上进行绑定

## refs

### 简介

在**常规的React数据流**中，props 是父组件与子组件交互的唯一方式。 要修改子元素，你需要用新的props去重新渲染子元素。然而，在**少数情况下，你需要在常规数据流外强制修改子元素**。被修改的子元素可以是React组件实例，或者是一个DOM元素。在这种情况下，React 提供了解决办法:使用Refs

**一般在表单中使用。**



**refs使用有三种形式：**

- 字符串形式的ref
- 回调函数形式的ref
- createRef()创建的ref  **（常用）**



**何时使用refs：**

- 处理focus、 文本选择或者媒体播放
- 触发强制动画
- 集成第三方DOM库

**注意**：**不要过度使用refs**，你可能首先会想到在你的应用程序中使用refs来更新组件。如果是这种情况，请花一点时间，更多的关注在组件层中使用state。在组件层中，通常较高级别的state更为清晰。



### 字符串形式的ref

**React支持一个特殊的属性**，你可以将这个属性加在任何通过render()返回的组件中。**这也就是说对render()返回的组件进行一个标记，可以方便的定位的这个组件实例**。这就是ref的作用。

语法：

```jsx
<input ref="myinput">
```

访问这个实例，可以通过this.refs来访问。

```jsx
this.ref.myinput
```

>**注意：字符串形式的ref已经不推荐使用了，原因是效率问题。**

### 回调函数形式的ref  （常用）

**ref属性的值除了是一个字符串，还可以是一个函数， 这个函数是一个回调函数**， **在组件初始化或更新时由react调用**，这个回调函数会得到一个参数， 即当前的节点对象，**我们可以将得到的节点对象挂在到组件的实例对象上**，在需要的地方进行调用

语法：

```jsx
    <input ref={(element)=>{this.mailRef=element}} type="text" />
```

访问这个实例，可以通过this.mailRef来访问。

> 就是将其**挂载到实例化对象上的一个自定义属性上。**



>**注意：关于回调 refs 的说明**
>
>>如果 `ref` 回调函数是以内联函数的方式定义的，在更新过程中**它会被执行两次**，第一次传入参数 `null`，然后第二次会传入参数 DOM 元素。**这是因为在每次渲染时会创建一个新的函数实例**(之前的被释放掉了)，所以 React 清空旧的 ref 并且设置新的。通过将 **ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题**，但是大多数情况下它是**无关紧要**的。
>
>>什么是类的绑定函数方式？
>
>>```jsx
>>constructor(props) {
>>super(props);
>>```
>
>>    this.textInput = null;
>
>>    this.setTextInputRef = element => {
>>      this.textInput = element;
>>    };
>>    }
>
>>```jsx
>> <input
>>     type="button"
>>     value="Focus the text input"
>>     onClick={this.focusTextInput}
>>  />
>>```
>
>



### creatRef()   （官方推荐常用）



创建16.3版本中Res通过React.crlae()的方法创建，通过ref属性关联

>React.createRef()可以返回一个容器，容器里可以存储当前ref被标识的节点。，但该容器只能存一个。

语法：

(1)现在constructor函数中创建refs节点

```jsx
this.userRef = React.createRef();
this.pwdRef = React.createRef();
```

>一个属性只能存储一个节点，因此上面存储两个input，便创建了两个。

(2)在节点中通过ref关联

```jsx
   <p>
          姓名：
          <input ref={this.userRef} type="text" />
   </p>
   <p>
          密码 <input ref={this.pwdRef} type="password" />
  </p>
```

（3）访问

>必须通过current才能获取节点

 

### 综合实例

下面通过第三种不同的方法学设置ref，并进行访问输出

```jsx
import React from "react";

class TestRef extends React.Component {
  constructor(props) {
    super(props);

    this.userRef = React.createRef();
    this.pwdRef = React.createRef();
  }

  render() {
    return (
      <div>
        <p>
            {/* 字符串形式的ref */}
          电话：
          <input ref="telephone" type="text" />
        </p>
        <p>
            {/* crrateRef()方法创建ref */}
          姓名：
          <input ref={this.userRef} type="text" />
        </p>
        <p>
             {/* crrateRef()方法创建ref */}
          密码 <input ref={this.pwdRef} type="password" />
        </p>
        <p>
             {/* 回调函数形式ref */}

          邮箱：
          <input ref={element => {this.mailRef = element}} type="text"/>
        </p>
        {/* 绑定事件，访问输出 */}
        <button onClick={this.showData}>登录</button>
      </div>
    );
  }

  showData = () => {
    console.log(this);
    let user = this.userRef.current;
    let pwd = this.pwdRef.current; //crateRef
    let tele = this.refs.telephone;  //字符串形式
    let mail = this.mailRef;  //回调函数形式

    console.log(user.value, pwd.value, tele.value, mail.value);
  };
}

export default TestRef;

```



![mark](http://qiniu.wind-zhou.com/blog/210411/Cb8djh2A8g.png?imageslim)



我们再看一下此时的组件实例化对象：

<img src="http://qiniu.wind-zhou.com/blog/210411/km5mHJle5E.png?imageslim" alt="mark" style="zoom:25%;" />



可以看到，几个节点被存储到了几个不同的属性上。



> **总结：**
>
> - 三种方法都可以创建ref，不过新版本中推荐使用createRef()来创建ref
>
> - createRef()来创建ref，语法上更严谨，结构上更完整。
>
> - 一个ref上只能存一个节点。
>
> - ref不仅可以加载节点身上，还能加载组件身上。例如将子组件的ref加载父组件身上。
>
>   因此可以访问到子组件的数据



## react中的事件处理

1. 通过onXxx属性指定事件处理函数(注意大小写)

   （1) React使用的是**自定义(合成)事件,** 而不是使用的原生DOM事件 ----更兼容

   （2) React中的**事件是通过事件委托方式处理的**(委托给组件最外层的元素)----为了高效

2. **通过event.target得到发生事件的DOM元素对象**







