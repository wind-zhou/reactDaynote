# redux

##简介

单页应用开发日趋复杂，JavaScript 需要管理比任何时候都要多的state (状态)。这些state可能包括服务器响应、缓存数据、 本地生成尚未持久化到服务器的数据，也包括UI状态，如激活的路由，被选中的标签，是否显示加载动效或者分页器等等。



**管理不断变化的state非常困难，尤其是很深层次的组件嵌套**，造成state需要需要互相传递。如果一个model的变化会引起另一个 model 变化，那么当view变化时，就可能引起对应model以及另一个model的变化，依次地，可能会引起另一个view的变化。直至你搞不清楚到底发生了什么。state在什么时候，由于什么原因，如何变化已然不受控制。当系统变得错综复杂的时候， 想重现问题或者添加新功能就会变得举步维艰。

>**总结：redux就是来管理状态的。**

## 什么是redux

redux是 一个**专门用于做状态管理的js库**(不是react插件库)

它可以拥在react, angulat, vue等项目中，但基本与react配合和使用

**作用:**集中式管理react应用中多个组件共享的状态

## 什么情况下需要使用redux?

某个组件的状态，需要让其他组件可以随时拿到，需要共享自己的状态(一般在大型的项目中使用)
**总体原则**:能不用及不用，如果数据管理起来比较吃力就考虑使用



![mark](http://qiniu.wind-zhou.com/blog/210415/8cE8CE0cJm.png?imageslim)





action 相当于一个命令中心（例如，增加，删除）

​	dispatch：分发命令

store是一个数据存储区

reducers 负责命令的执行，直接操作数据



component通过触发事件；来控制action来发布命令

## 三大原则

### 单一数据源

**整个应用的state被存储在一颗object tree中，并且这个object只存在唯一的一个store中。**

### state是只读的

**唯一能改变state的方法就是触发action**，**action描述已经发生事件的普通对象**。

这样确保了视图和网络请求都不能直接修改state,相反它们只能表达想要修改的意图。**因为所有的修改都被集中化处理**，且严格按照一个接 一个的顺序执行，因此不用担心race condition的出现。Action就是普通对象而已，因此它们可以被日志打印、序列化、储存、后期调试或测试时回放出来。

### 使用纯函数来执行修改

**为了描述action如何修改state tree ，需要编写reducers。**

Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state。刚开始你可以只有一个 reducer，随着应用变大，你可以把它拆成多个小的 reducers，分别独立地操作 state tree 的不同部分，**因为 reducer 只是函数**，你可以控制它们被调用的顺序，传入附加数据，甚至编写可复用的 reducer 来处理一些通用任务，如分页器。





## action

**Action 本质上是 JavaScript 普通对象**。我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。**当应用规模越来越大时，建议使用单独的模块或文件来存放 action。**



Action是把数据从应用传到store的有效载荷。**它是store数据的唯一 来源**。一 般来说会通过`store.dispatch()`将action传到store。

- ①在Redux中， 我们用**对象来描述行为**
- ②行为有很多种，为了以示区别，我们给**每个描述行为的对象增加一个type字段**，type字段的值就
  表示当前对象所描述的是哪种具体行为。
- ③无论哪种行为，只要发生了，都会产生几个与该行为相关的最直接最重要的数据。而这些数据也在对象描述行为的范围内，所以添加任务除了type字段外，还需要text字段告诉读者添加了什么任务，也需要id字段方便索引. 
- ④综上所述，在Redux中，用种有固定格式的对象来描述行为，这种固定格式体现在:①**该对象必须有type字段**②该对象的其他字段不做任何限制，但从道理上讲其他字段不会特别多。如果存在，一定都是行为最精华的细节或数据描述。
- ⑤在Redux中，我们把这种结构固定的对象取个名字，就叫acion对象
- ⑥同种行为不太可能只触发次， 所以这才有了acio创建函数。



**创建action：**

action可以使用直接创建，也可以使用函数创建

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text:text
  }
}
```



## Reducer

**Reducers** 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的，记住 actions 只是描述了*有事情发生了*这一事实，**并没有描述应用如何更新 state**（redux只负责维护状态，更新state这是组件自己的事）。

>他就是一个干活的，受action里面描述的行为控制，加入action对象中type字段为"increasement"，则指向加操作。
>
>内部一般是一个switch选择结构。



**定义reducer：**

```js
function todoApp(state = initialState, action) { //这里state设定翻译一个默认值，以内第一次若是没有state，则使用此数据
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    default:
      return state
  }
}
```



## store

Store是用来维持应用所有state树的一个对象。 改变state的唯一方法是store dispatch-个action。

**Store不是类，而只是一 个有几个方法的对象，可以采用createStore进行创建。**

**创建store：**

```js
createStore(reducer, [initState, enhancer])
```

作用:创建一个Redux store来存放应用中所有的state,一个应用只能有 个store.函数返回store对象。

- initStatate: 初始state, 如果不为空，需要和reducer中处理的state结构一致，，如果为空，则需要在reducer中设置默认参数（因为这个参数回传给reducer）。
- enhancer:一个中间件，如logger等。

**reducer函数特点：**

Reducer函数最重要的特征是，它是个纯函数。 也就是说，只要是同样的输入，必定得到同样的输出。

> 纯函数是函数式编程的概念，必须遵守以下一些约束。
>
> - **不得改写参数**(不能改变State, 必须返回一个全新的对象)
> - 不能调用系统IO的API
> -  不能调用Date.now()或者Math.random()等不纯的方法，因为每次会得到不样的结果



**核心API：**

（1）getState() 

​	作用：返回当前的state树。（就是从store中取值）

（2）dispatch(action)

​	作用：分发action，这是改变state的唯一方法（就是分发命令）

（3）subscribe(listener)

作用：添加一个监视器，每当dispatch action的时候执行，state树种一部分已发生变化，这时就可以使用getState()拿到当前的state。返回一个解绑的函数。

添加一个变化监听器。每当 dispatch action 的时候就会执行，state 树中的一部分可能已经变化。你可以在回调函数里调用 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 来拿到当前 state。

## 实战练习

点击添加商品，点击删除按钮则删除。



```jsx
import React from "react";
import { createStore } from "redux";

let add_action = function(value) { //将数据封装成action
  return {
    type: "add",
    data: value
  };
};

let del_action = function(liIndex) { //将数据封装成action
  return {
      index:liIndex,
      type: "del",

  };
};



function reducer(prestate, action) {  //定义reducer函数，负责具体的增删改查工作
  switch (action.type) {
    case "add":
      return { goods: [...prestate.goods, action.data] };
      break;
    case "del":
      let newStateArr=[...prestate.goods] //进行数组的深拷贝
      let afterArr=[...newStateArr.slice(0,parseInt(action.index)),...newStateArr.slice(parseInt(action.index)+1)]
      return {goods:afterArr};
      break;
    default:
      return prestate;
  }
}

let object_state = { goods: ["牛奶", "苹果"] };    //初始化数据

let Store = createStore(reducer, object_state);  //创建store对象  这里面存储了原始的数据，并且绑定了reducer操作

class ReduxTest extends React.Component {
  constructor() {
    super();
    this.state = { good: Store.getState().goods };
  }

  render() {
    let g = this.state.good.map((value, index)=>{
      return(
       <li key={index}>
           <span>{value} </span> <button  onClick={this.delGoods(index)}>-</button>
      </li>)

    });
    return (
      <div>
        <p>
          <input type="text" ref="g" />
            <button onClick={this.addGoods}>添加</button>   {/*（1）绑定点击事件 */}
        </p>
        <ul>{g}</ul>
      </div>
    );
  }

  addGoods = () => { //增加商品
    Store.dispatch(add_action(this.refs.g.value));    //（2）获取数据，并打包成action，之后由store分发
  };

  delGoods=(index)=>{//删除商品

    return ()=>{
        Store.dispatch(del_action(index));
    }
  }
  

  componentDidMount() {
    Store.subscribe(() => {  //监听，使数据改变时，修改state
      this.setState({ good: Store.getState().goods });  
    });
  }
}
export default ReduxTest;
```



演示;

![mark](http://qiniu.wind-zhou.com/blog/210415/K9LLm1Eb8i.gif)



>当工程较大时：各个模块要放在不同的文件夹中，方便管理。



## 异步action

异步任务的action是一个函数：因为可以开启一个异步任务。

异步action中一般都会调用同步的action

 使用以上方式创建的redux项目， 它只有同步操作。**每当dispatch action时， state会被立即更新**。

同步redux写法: `action->reducer(state ,action)->store(reducer)`

action是个普通的avascrp对象, reduce是一个普通的方法。在reducer中根据当前的state. 接收到的action来生成新的state以达到更新状态的目的。

**那么问题来了！**，每次action被触发(dispatch), reducer就会同步 地对store进行更新，在实际开发项目的时候。有很多需求都是需要通过接口等形式获取异步教据后再进行更新操作的，如何异步地对stor进行更新呢?

>例如：点击按钮之后，需要在某个div里显示后台请求回来的数据，这是如果没有只有同步操作，则action立马被分发给了reducer，但我们需要在ajajx请求回来数据之后再对数据显示，则需要先把action的分发给阻塞起来，这是就需要到了异步操作，也就引入了中间件。

**使用中间件**

![mark](http://qiniu.wind-zhou.com/blog/210417/c1D3m5glLe.png?imageslim)

redux本身提供的与功能并不多，但是它提供了一个中间件(插件) 机制， 可以使用第三方提供的中间件或自己编写个中间件来对Redcer的功能进行增强。

>reducx 的createStore(reducer，prestate，enhancer)方法的的第三个参数就是预留给中间件的，enhancer本身有提升的意思，可以理解为通过中间件提升store，扩展其功能。



dispatch一个action之后， 到达reducer之前， 进行些额外的操作， 就需要用到middleware。你可以利用Redux middleware来进行**日志记录、创建崩溃报告、调用异步接口或者路由**等等。

> **换言之，中间件都是对store dispatch的增强**



对于中间件，redux库给我们提供了几种常用了**异步流管理问题的方案**的中间件：redux-thunk、redux-promise、redux-saga。

> **三种中间件的运行机制不同，具体体现在开发的时候就是action的定义方式有区别。**



**应用语法：**

中间件在创建store对象时就应传入，作为createStore(reducer, enhancer);的第三个参数，具体语法如下：

```js
const store=createStore(reducer, astate,pplyMiddleware(thunk));
```

上面以引入thunk中间件为例，当然也可以引入多个。

例如：

```js
const store = createStore(
    reducer,
    state,
    applyMiddleware(thunk, logger)
);
```





### redux-thunk

在创建store中引入中间件后，那我们便需要根据不同的中间件，定义不同类型的action。

#### 安装

(1)下载

redux-thunk在使用之前需要安装

```js
npm install redux-thunk -D
```

（2）导入模块

```js
import thunk from "redux-thunk";  //导入thunk
import { createStore, applyMiddleware } from "redux";   //导入中间件
```



#### 原理

通过多参数的柯里化函数以实现对函数的情性求值，从而将同步的action转为异步的action



> 上面这个解释不是人能听懂的，**简单来说就是允许action是一个函数(同步的action都是一个对象)，然后我们可以在该函数中定义相应的异步操作（像定时器或ajajx请求）。**
>
> 上面说的什么柯里化函数是指我们的action creator函数是一个高阶函数。这个高阶函数返回的函数作为我们的action。



接下来看一个案例。

```js
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk";

function reducer(state, action) {
    switch (action.type) {
        case "GET_WEATHER_SUCCESS":
            return {...state, ...action.payload };
    }
}
const store = createStore(reducer, state, applyMiddleware(thunk));

function getWeather(url, params) {//该函数返回一个函数作为action(也是一个函数)
    return (dispatch, getState) => {
        fetch(url, params)
            .then(result => {
                return result.json();
            })
            .then(data => {
                dispatch({   //手动分发action
                    type: "GET_WEATHER_SUCCESS",
                    payload: data
                });
            })
            .catch(err => {
                dispatch({
                    type: "GET_WEATHER_ERROR",
                    error: err
                });
            });
    };
}

store.dispatch(getWeather("http://www.wwather.com", { method: "POST" }));
```



上面案例表明，action是一个函数，不过这个函数的定义有一定的要求：参数必须包含`(dispatch, getState)`,

这是thunk决定的。

>**个人理解：**
>
>中间件的存在本质上是暂时阻塞了action的分发，那么在完成异步操作后，就需要手动的进行dispatch一下，上面传递的参数dispatch其实就起了个这么个作用。

>**扩展：**
>
>## **redux-thunk 源码**
>
>由于`redux-thunk`的代码量非常少，我们直接把它的代码贴上来看一下。这里我们看的是最新版的`v2.3.0`的代码：
>
>```js
>function createThunkMiddleware(extraArgument) {
>  return ({ dispatch, getState }) => next => action => {
>    if (typeof action === 'function') {
>      return action(dispatch, getState, extraArgument);
>    }
>
>    return next(action);
>  };
>}
>
>const thunk = createThunkMiddleware();
>thunk.withExtraArgument = createThunkMiddleware;
>
>export default thunk;
>```
>
>如果你看了前几篇文章，对 Redux 及它的 middleware 机制有所了解，那么上面这段代码是非常容易理解的。`redux-thunk`就是一个标准的 Redux middleware。
>**它的核心代码其实只有两行，就是判断每个经过它的`action`：如果是`function`类型，就调用这个`function`（并传入 dispatch 和 getState 及 extraArgument 为参数），而不是任由让它到达 reducer，因为 reducer 是个纯函数，Redux 规定到达 reducer 的 action 必须是一个 plain object 类型。**
>
>`redux-thunk`的原理就这么多，是不是非常简单



### redux-promise

#### 安装

(1)下载

```js
cnpm install react-promise -D
```

(2)导入

```js
import { createStore, applyMiddleware } from "redux";
import promise from "redux-promise";
```

#### 原理

如果action是一个promise，则会等待promise完成，**将完成的结果作为action触发**，如果action不是一个promise，则判断其payload是否是一个promise，如果是，等待promise完成，然后将得到的结果作为payload的值触发。

>就是说action可以有两种形式：
>
>- action是promise
>- action里的payload字段的值是promise



看下面的例子：

```js
//  (1)action本身是promise
//这种写法只能派发成功的时候（resolve），失败的时候不能触发，这种写法并不常用
add(amount) {
  return new Promise((resolve, reject) => {
    resolve ({ type: "ADD", amount: 1 })  //因为会将其结果作为action，因此这里需要返回a一个ction
  })
}

// 这种写法成功和失败都进行派发
//如果返回一个对象，对象名字中有payload，而且payload是一个promise对象，那么他会让这个promise执行
add(amount) {
  return {
    type: "ADD",
    payload: new Promise((resolve, reject) => {
        reject (100)  //reducer中可以通过action.payload中拿到reject或resolve中的100
     })
  }
}


```



> 不同的中间件都有着自己的适用场景，**react-thunk 比较适合于简单的API请求的场景**，**而Promise**
> **则更适合于输入输出操作**，通过判断返回的promise的状态返回新的action



完整代码如下：

```js
import { createStore, applyMiddleware } from "redux";
import promise from "redux-promise";

function reducer(state, action) {
    switch (action.type) {
        case "GET_WEATHER_SUCCESS":
            return {...state, ...action.payload };
    }
}
const store = createStore(reducer, applyMiddleware(promise));

const fetchData = (url, params) => fetch(url, params);
async function getWeather(url, params) {
    const result = await fetchData(url, params);  //fetch返回promise对象
    if (result.error) {
        return {
            type: "GET_WEATHER_ERROR",
            error: result.error
        };
    }
    return {
        type: "GET_WEATHER_SUCCESS",
        payload: result   //如果成功，将promise对象（result）里的值作为payload的值，注意这里payload的值不是promise而是其执行完成只有的值
    };
}

store.dispatch(getWeather("http://www.wwather.com", { method: "POST" }));
```

## redux-saga

不常用，不学了，用到在学。









