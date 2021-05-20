# 路由

>路由本质上是操作的history的地址的历史记录



## SPA应用(single page web application)

1. 整个应用只有一个完整的页面
2. **点击页面的链接不会刷新页面，只会做页面的局部更新**
3. 数据**都需要通过ajax请求获取**，并在前端异步展现



![mark](http://qiniu.wind-zhou.com/blog/210413/8labLKK50a.png?imageslim)

> 单页面用显示隐藏其实也可以实现，但是没人会用，因为页面初始化时所有的标签都会渲染，会增加巨大的开销。

## 理解react路由

多页面网站是通过**超链接**来实现页面的切换的， 所有每次点击超链接时浏览器都需要刷新请求新的页面，而**单页面应用程序主要通过路由来实现内容的切换**，在**react当中也就是切换组件**

**那如何实现组件的切换呢?** 

react路由是通过一种特殊的链接， **点击时会修改浏览器地址栏中的地址**，但是浏览器是会忽略掉这次变化所以界面不会被跳转，但是在**路由会监听到地址(path) 的变化从而根据不同的地址渲染出来不同的组件(component)**

>相当于给组件打标识（一种特殊的链接），并根据不同的情况进行''条件渲染''。

## react route

目前，官方同时维护2.x和4.x两个版本，**React 16及以上版本只支持router4 x版本**

react Router4.0 (以下简称RR4)它遵循React的设计理念， 即**万物皆组件**。所以**RR4只是一堆提供了导航功能的组件**(还有若干对象和方法)，具有声明式 (引入即用)，可组合性的特点。

### 安装

```js
cnpm install react-router-dom -D
```

### 三大核心内容

- 路由器组件
- 路由匹配组件
- 导航组件

#### 路由器组件

>**作用：根据地址栏的变化引导显示相应组件**,一般会包裹在最外层

>最常用的路由器分为两种：\<BrowserRouter>  \<HashRouter>

**1、\<Router>**

如果我们希望页面中某个部分的内容需要根据URL来动态显示，需要用到Router组件，**该组件是一个容器组件**，只需要用它包裹URL对应的根组件即可

**Router是所有路由组件共用的底层接口，一般我们的应用并不会使用这个接口， 而是使用高级的路由**（用来是为了监听）

**2、\<BrowserRouter>** 

\<BrowserRouter>:使用HTML5提供的**history API**来保持UI和URL的同步;(**因此可以使用前进后退**)

**3、\<HashRouter>**

使用URL的hash (例如: window.location.hash)来保持UI和URL的同步 )(**基本不用**)

**4、\<MemoryRouter>**

能在内存保存你“URL"的历史纪录(并没有对地址栏读写);

**5.\<StaticRouter>**

从不会改变地址;

> **注**:这里\<Router>组件下只允许存在一个子元素，如存在多个则会报错。



#### 路由匹配组件

> **作用：注册路由**

（1）\<Route>

​		**Route组件主要的作用就是当location匹配路由的path时， 渲染某些UI。**
（2）\<Route>组件有如下属性

- **path (sting)** :路由匹配路径。(没有path属性的Route 总是会匹配) ;
- **component**: 设置要显示的组件，该属性以**props**的方式传入
- **exact (bool)** : exact 属性表示路由使用**精确匹配模式**，非exact模式下匹配所有以“开头的路由，一般都会设置，值可以省略

```js
<Route path="/about" component={About}></Route>
<Route path="/home" component={Home}></Route>
```



#### 导航组件

> **作用：替换a标签。点击时，实现地址栏地址的改变**

点击时，其实react-router-dom拦截了a标签的默认动作，然后根据所有使用的路由模式进行处理，改变了url但不会发送请求，同时根据router中设置的对应组件来进行显示。

**导航组件有两类：Link和NavLink**

NavLink是对Link的一个特定版本，**作用是当匹配到当前的url时会给已渲染的 元素添加样式参数。**

**使用语法：**

```jsx
<li>  <NavLink to="/about"  activeClassName="navActive"> About</NavLink> </li>
<li> <NavLink to="/home"  activeClassName="navActive"> Home</NavLink> </li>
```

当点击当前link时，会自动给当前标签加一个“navActive”的类。这时我们可以自定义该类的 css样式。

### switch

**一个路径匹配多个组件的时候，会用到switch。  （效率的优化）**

**因为路由匹配到匹配到一个后还会继续向下匹配。（只匹配一个）**



**使用**：把route那switch包一下

```js
 <Switch>
        <Route path="/about" component={About}></Route>
        <Route path="/home" component={Home}></Route>
  </Switch>
```



## 精准匹配与模糊匹配

```jsx
<Link to="/home/a/b"></Link>

<Route  path="/home" component={Nav}> </Route>
```

上面可以匹配上。这是模糊匹配

如果加上exact，会开启严格匹配，地址必须精准对应。

>**注意：严格匹配不能随便开！**，**有时开启会导致无法匹配二级路由。**

## redirect 重定向

**重定向**。当用户访问某界面时，该界面并不存在，此时用Redirect重定向，重新跳到一个我们自定义的组件里。



写在注册路由最后位置，和router写在一起。

```jsx
<Switch>
    <Route path="/about" component={About}></Route>
    <Route path="/home" component={Home}></Route>
    <Redirect to="/about"  />
</Switch>
```

上面代码，在地址栏输入`localhost:3000`时，会重定向至`/about`



## 一级路由练习

组件结构：

- about
- home

（1）定义两个组件

（2）在app.js中进行Link的设置和路由的注册

（3）在index.js中使用B

**about.jsx**

```jsx
// home组件
import React from "react";
class About extends React.Component{
    render(){
        return(
            <div>
                这是About的内容
            </div>
        )
    }
}
export default About

```

**home.jsx**

```jsx
// home组件
import React from "react";
class Home extends React.Component{
    render(){
        return(
            <div>
                这是Home的内容
            </div>
        )
    }
}
export default Home

```

**app.jsx**

```jsx

import { Route, Switch,Redirect } from "react-router-dom";
import React from "react";
import MyNavLink from "./component/pages/MyNavLink/MyNavLink";

// 引入组件
import About from "./component/pages/about/about";
import Home from "./component/pages/home/home";

class App extends React.Component {
  render() {
    return (
      <div id="zz">
        <li>
          <MyNavLink to="/about">About </MyNavLink>
        </li>

        <li>
          <MyNavLink to="/home">Home</MyNavLink>
        </li>


        <Switch>
          <Route path="/about" component={About}></Route>
          <Route path="/home" component={Home}></Route>
          <Redirect to="/about"  />
        </Switch>
      </div>
    );
  }
}

export default App;

```

**index.js**

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter, Route } from "react-router-dom";
import App from './App'
import "./index.css";

ReactDOM.render(

 // BrowserRouter 组件法放在这个位置，是因为全局所有的路由都包起来，因为，不同的路由器之间不能通信
  <BrowserRouter>
    <App/>
  </BrowserRouter>,
  document.getElementById("root")
);
```

## 嵌套路由（二级路由）

组件结构：在一级路由的基础上书写二级路由

将About下设置两个子组件：aboutSon1、aboutSon2.（除了About，其他东西没动）

然后再About组件中设置二级路由。

>路由匹配先从一级路由开始匹配
>
>点二级路由时，先匹配一级的about，然后再在一级的路由向下匹配/about/left
>
>注意此时不能加exact ，开启后，子路由就废了，因为不开始模糊匹配。
>
>模糊匹配：如果书写的路由多，则可以匹配到，如果少了就不行。
>
>例如若给的是/home/a/b,则先匹配到一级的home，这就算匹配上了，这就不会出错。

**about.jsx**

```jsx
// about组件

import React from "react";

import AboutSon1 from "./aboutSon1/aboutSon1";
import AboutSon2 from "./aboutSon2/aboutSon2";
import { Route, NavLink ,Redirect} from "react-router-dom";

class About extends React.Component {
  render() {
    return (
      <ul>
        <li>
         
          <NavLink to="/about/left" activeClassName="navActive">
            左边
          </NavLink>
        </li>
        <li>
       
          <NavLink to="/about/right" activeClassName="navActive">
            右边
          </NavLink>
        </li> 
            <Route path="/about/left" component={AboutSon1}></Route>
            <Route path="/about/right" component={AboutSon2}></Route>
            <Redirect to="/about/right"  />
      </ul>
    );
  }
}

export default About;

```

**aboutSon1.jsx**

```jsx
import React, { Component } from "react";

export default class AboutSon1 extends Component {
  render() {
    return <div className="about-left">这时about右边的内容</div>;
  }
}
```

**aboutSon2.jsx**

```jsx
import React, { Component } from "react";

export default class AboutSon2 extends Component {
  render() {
    return <div className="about-right">这时about右边的内容</div>;
  }
}

```

## 路由传值

### param传值

三步走：

（1）路由时，携带参数

（2）在子组件声明接受params参数

（3）子组件取值



> 传来的值毫无疑问是在props中，我们来看一下，他在什么位置。

如图可知，在`this.props.match.params;`中。



![mark](http://qiniu.wind-zhou.com/blog/210423/f3bJle76lE.png?imageslim)





（1）传值

```jsx
     <ul>
          {this.state.messages.map(message => {
            return (
              <li key={message.id}>
                //传值
                <Link to={`/about/right/message/${message.id}/${message.title}`}> {message.title}</Link>
              </li>
            );
          })}
        </ul>
```

(2)声明接收



```jsx
       {/*声明接受 */}
        <Route path="/about/right/message/:id/:title" component={ Details}/> 
```

（3）取值

```jsx
 const { id, title } = this.props.match.params;
```



完整代码：

**message:**

```jsx
import React, { Component } from "react";
import { Route, Link } from "react-router-dom";

import Details from "./details/details";

export default class Message extends Component {
  state = {
    messages: [
      {
        id: "01",
        title: "title1"
      },
      {
        id: "02",
        title: "title2"
      },
      {
        id: "03",
        title: "title3"
      }
    ]
  };

  render() {
    return (
      <div>
        <ul>
          {this.state.messages.map(message => {
            return (
              <li key={message.id}>
                <Link to={`/about/right/message/${message.id}/${message.title}`}> {message.title}</Link>
              </li>
            );
          })}
        </ul>

        <hr />

        {/*声明接受 */}
        <Route path="/about/right/message/:id/:title" component={ Details}/> 
     
      </div>
    );
  }
}

```

**messageDetails:**

```jsx
import React, { Component } from "react";

export default class Details extends Component {
  state = {
    messageDetail: [
      {
        id: "01",
        content: "你好中国"
      },
      {
        id: "02",
        content: "你好南京"
      },
      {
        id: "03",
        content: "你好杭州"
      }
    ]
  };
  render() {
    console.log(this.props);

    const { id, title } = this.props.match.params;
    const content = this.state.messageDetail.find(value => {
      return value.id == id;
    });

    return (
      <li>
        <ul>
          <li>id:{id}</li>
          <li>title:{title}</li>
          <li> content:{content.content}</li>
        </ul>
      </li>
    );
  }
}
```



效果：

![mark](http://qiniu.wind-zhou.com/blog/210423/balk9JaCLh.png?imageslim)







![mark](http://qiniu.wind-zhou.com/blog/210414/50iKKDFeib.png?imageslim)



**有历史记录，replace没有历史记录。**



![mark](http://qiniu.wind-zhou.com/blog/210414/0hLikAKlfe.png?imageslim)



### **通过search传值：**

这个仅需两步走：

（1）传值

（2）子组件取值

>**注：无需声明接收**



再看一下传过来的参数在什么位置  ` this.props.location.search`



![mark](http://qiniu.wind-zhou.com/blog/210423/FdmA6Am2BJ.png?imageslim)







>引入一个库：` import qs from "querystring";` 
>
>这个可转么做search类型的解析转化：
>
>两个方法：
>
>qs.parse()  将`"id=01&title=title1"`解析成` {id: "01", title: "title1"}`
>
>  qs.stringify():将` {id: "01", title: "title1"}`解析成`"id=01&title=title1"`
>
>>**`"id=01&title=title1"`是encoded格式编码的字符串**



(1)传值

```jsx
<ul>
    {this.state.messages.map(message => {
        return (
            <li key={message.id}>
                <Link to={`/about/right/message/details/?id=${message.id}&title=${message.title}`}> {message.title}</Link>
            </li>
        );
    })}
</ul>
```

(2)读取



```jsx
    const { id, title } = qs.parse(search.slice(1));
```



完整代码：

**message:**

```jsx
import React, { Component } from "react";
import { Route, Link } from "react-router-dom";

import Details from "./details/details";

export default class Message extends Component {
  state = {
    messages: [
      {
        id: "01",
        title: "title1"
      },
      {
        id: "02",
        title: "title2"
      },
      {
        id: "03",
        title: "title3"
      }
    ]
  };

  render() {
    return (
      <div>
        <ul>
          {this.state.messages.map(message => {
            return (
              <li key={message.id}>
                <Link to={`/about/right/message/details/?id=${message.id}&title=${message.title}`}> {message.title}</Link>
              </li>
            );
          })}
        </ul>

        <hr />

        {/*声明接受 */}
        <Route path="/about/right/message/details" component={ Details}/> 
     
      </div>
    );
  }
}

```

**messageDetails:**

```jsx
import React, { Component } from "react";

import qs from "querystring";

export default class Details extends Component {
  state = {
    messageDetail: [
      {
        id: "01",
        content: "你好中国"
      },
      {
        id: "02",
        content: "你好南京"
      },
      {
        id: "03",
        content: "你好杭州"
      }
    ]
  };
  render() {
    console.log(this.props);
    // 接收search
    const { id, title } = qs.parse(search.slice(1));
    const result = this.state.messageDetail.find(value => {
      return value.id == id;
    });

    console.log(result)
    return (
      <li>
        <ul>
          <li>id:{id}</li>
          <li>title:{title}</li>
          <li> content:{result.content}</li>
        </ul>
      </li>
    );
  }
}

```









![mark](http://qiniu.wind-zhou.com/blog/210414/kB51FckgIh.png?imageslim)



但只要页面刷新，数据就会丢失。



### **通过state传值：**

这个需要两步走：

（1）传值

（2）读取



**也是无需声明接收**



老套路，看位置

在`this.props.location.state`中

![mark](http://qiniu.wind-zhou.com/blog/210424/f6A2kDfmi0.png?imageslim)



（1）传值

```jsx
        <ul>
          {this.state.messages.map(message => {
            return (
              <li key={message.id}>
                <Link to={ { pathname:'/about/right/message/details',state:{id:message.id,title:message.title}}}> {message.title}</Link>
              </li>
            );
          })}
        </ul>
```

（2）读取

```jsx
   const { id, title } = this.props.location.state;
```



完整代码：

**message:**

```jsx
import React, { Component } from "react";
import { Route, Link } from "react-router-dom";

import Details from "./details/details";

export default class Message extends Component {
  state = {
    messages: [
      {
        id: "01",
        title: "title1"
      },
      {
        id: "02",
        title: "title2"
      },
      {
        id: "03",
        title: "title3"
      }
    ]
  };

  render() {
    return (
      <div>
        <ul>
          {this.state.messages.map(message => {
            return (
              <li key={message.id}>
                {/* <Link to={`/about/right/message/details/?id=${message.id}&title=${message.title}`}> {message.title}</Link> */}
                <Link to={ { pathname:'/about/right/message/details',state:{id:message.id,title:message.title}}}> {message.title}</Link>

              </li>
            );
          })}
        </ul>

        <hr />

        {/*声明接受 */}
        <Route path="/about/right/message/details" component={ Details}/> 
     
      </div>
    );
  }
}

```

**messageDetails:**

```jsx
import React, { Component } from "react";

import qs from "querystring";

export default class Details extends Component {
  state = {
    messageDetail: [
      {
        id: "01",
        content: "你好中国"
      },
      {
        id: "02",
        content: "你好南京"
      },
      {
        id: "03",
        content: "你好杭州"
      }
    ]
  };
  render() {
    console.log(this.props);
    // 接收search
    const { id, title } = this.props.location.state;
    const result = this.state.messageDetail.find(value => {
      return value.id == id;
    });

    console.log(result);
    return (
      <li>
        <ul>
          <li>id:{id}</li>
          <li>title:{title}</li>
          <li> content:{result.content}</li>
        </ul>
      </li>
    );
  }
}

```



>注：
>
>- 刷新丢失。
>
>- 路径无提示



**优点：优雅，可传对象。**





![mark](http://qiniu.wind-zhou.com/blog/210414/JCf2cCflLG.png?imageslim)



## 路由组件与一般组件

![mark](http://qiniu.wind-zhou.com/blog/210415/eDB6kiAAHC.png?imageslim)



最大区别就是：路由组件会受到路由器传递过来的props信息，核心的是上面三个。

- 存放位置不同
- 写法不同

![mark](http://qiniu.wind-zhou.com/blog/210424/akIdHbljHh.png?imageslim) 

## replace和push

**路由跳转的两种模式**

浏览器的存储区是以一个栈，每次push时都会压栈。



![mark](http://qiniu.wind-zhou.com/blog/210424/chj1BfijG7.png?imageslim)





replace则会替换每次的历史记录

语法：`replace={true} `

```jsx
  <Link  replace={true} to={ { pathname:'/about/right/message/details',state:{id:message.id,title:message.tit}}}> {message.title}</Link>
```

>默认是push模式



## 编程式路由导航

之前的路由跳转都是使用**路由连接Link**组件实现。

但有些需求是路由是动态的，例如等待三秒后跳转路由。



![mark](http://qiniu.wind-zhou.com/blog/210424/2de3LgED0m.png?imageslim)



手动前进后退

![mark](http://qiniu.wind-zhou.com/blog/210424/l9070aaHk7.png?imageslim)





## 路由组件的懒加载

路由的懒加载：将路由组件进行分包。可以提升首页的加载速度，提高用户的转化率。

(1)引入

```js
import React, {lazy,Suspense} from 'react';
import {BrowserRouter as Router, NavLink, Route} from "react-router-dom"
```

(2)

```js
const One = lazy(() => import ("./pages/One"));
const Two = lazy(() => import ("./pages/Two"));
const Three = lazy(() => import ("./pages/Three"));
```

(3)使用

```js
<Router>
    <NavLink to={"/"} exact>one</NavLink>
    <NavLink to={"/two"}>two</NavLink>
    <NavLink to={"/three"}>three</NavLink>
    <Suspense fallback={<p>加载中......</p>}>
        <Route path={"/"} component={One} exact></Route>
        <Route path={"/two"} component={Two}></Route>
        <Route path={"/three"} component={Three}></Route>
    </Suspense>
</Router>
```



>#### [深入理解React：懒加载（lazy）实现原理](https://www.cnblogs.com/forcheng/p/13132582.html)



### 使用loadable 插件

(1)下载loader

```
react-loadable  :  npm i react-loadable

antd  :  npm i antd
```

(2)引入使用



```react
import React from 'react'
import {Router, Route, Switch,Redirect} from 'react-router-dom'
import { createBrowserHistory } from 'history'

import {loadable} from '@/utils/loadable'; //懒加载模块
import Login from '@/pages/login';//普通加载模块
const Main = loadable({loader: () => import('@/pages/main')});   //引用
const NotFound404 = loadable({loader: () => import('@/components/NotFound/NotFound')});  //引用

const history = createBrowserHistory();

class RouteMap extends React.Component {
  render () {
    return (
      <Router history={ history }>
        <Switch >
          <Route path="/" exact component={Login}/>
          <Route path="/login" component={Login}/>
          <Route path="/main" component={Main}/>
          <Route path="/404" component={NotFound404}/>
          <Redirect from="/*" to="/404"/>
        </Switch>
      </Router>
    )
  }
}

export default RouteMap

//_this.props.history.push('/main')//跳转可后退
//_this.props.history.replace('/main')//跳转不可后退
// <Redirect  from="/*" to="/" /> //重定向
// <Route path="*" component={NotFound404}/>//默认
```

>
>
>**1.代码分割**
>
>（1）为什么要进行代码分割？
>
>现在前端项目基本都采用打包技术，比如 Webpack，JS逻辑代码打包后会产生一个 bundle.js 文件，而随着我们引用的第三方库越来越多或业务逻辑代码越来越复杂，相应打包好的 bundle.js 文件体积就会越来越大，因为需要先请求加载资源之后，才会渲染页面，这就会严重影响到页面的首屏加载。
>
>而为了解决这样的问题，避免大体积的代码包，我们则可以通过技术手段对代码包进行分割，能够创建多个包并在运行时动态地加载。现在像 Webpack、 Browserify等打包器都支持代码分割技术。
>
>
>
>（2）什么时候应该考虑进行代码分割？
>
>这里举一个平时开发中可能会遇到的场景，比如某个体积相对比较大的第三方库或插件（比如JS版的PDF预览库）只在单页应用（SPA）的某一个不是首页的页面使用了，这种情况就可以考虑代码分割，增加首屏的加载速度。
>
>
>
>**2.React的懒加载**
>
>示例代码：
>
>```jsx
>import React, { Suspense } from 'react';
>
>const OtherComponent = React.lazy(() => import('./OtherComponent'));
>
>function MyComponent() {
>  return (
>    <div>
>      <Suspense fallback={<div>Loading...</div>}>
>        <OtherComponent />
>      </Suspense>
>    </div>
>  );
>}
>```
>
>如上代码中，通过 `import()`、`React.lazy` 和 `Suspense` 共同一起实现了 React 的懒加载，也就是我们常说了运行时动态加载，即 OtherComponent 组件文件被拆分打包为一个新的包（bundle）文件，并且只会在 OtherComponent 组件渲染时，才会被下载到本地。
>
>那么上述中的代码拆分以及动态加载究竟是如何实现的呢？让我们来一起探究其原理是怎样的。
>
>
>
>**import() 原理**
>
>[import()](https://github.com/tc39/proposal-dynamic-import) 函数是由TS39提出的一种动态加载模块的规范实现，其返回是一个 promise。在浏览器宿主环境中一个`import()`的参考实现如下：
>
>```js
>function import(url) {
>  return new Promise((resolve, reject) => {
>    const script = document.createElement("script");
>    const tempGlobal = "__tempModuleLoadingVariable" + Math.random().toString(32).substring(2);
>    script.type = "module";
>    script.textContent = `import * as m from "${url}"; window.${tempGlobal} = m;`;
>
>    script.onload = () => {
>      resolve(window[tempGlobal]);
>      delete window[tempGlobal];
>      script.remove();
>    };
>
>    script.onerror = () => {
>      reject(new Error("Failed to load module script with URL " + url));
>      delete window[tempGlobal];
>      script.remove();
>    };
>
>    document.documentElement.appendChild(script);
>  });
>}
>```
>
>当 Webpack 解析到该`import()`语法时，会自动进行代码分割。
>
>
>
>**React.lazy 原理**
>
>*以下 React 源码基于 16.8.0 版本*
>
>
>
>React.lazy 的源码实现如下：
>
>```js
>export function lazy<T, R>(ctor: () => Thenable<T, R>): LazyComponent<T> {
>  let lazyType = {
>    $$typeof: REACT_LAZY_TYPE,
>    _ctor: ctor,
>    // React uses these fields to store the result.
>    _status: -1,
>    _result: null,
>  };
>
>  return lazyType;
>}
>```
>
>可以看到其返回了一个 LazyComponent 对象。
>
>
>
>而对于 LazyComponent 对象的解析：
>
>```js
>...
>case LazyComponent: {
>  const elementType = workInProgress.elementType;
>  return mountLazyComponent(
>    current,
>    workInProgress,
>    elementType,
>    updateExpirationTime,
>    renderExpirationTime,
>  );
>}
>...
>function mountLazyComponent(
>  _current,
>  workInProgress,
>  elementType,
>  updateExpirationTime,
>  renderExpirationTime,
>) { 
>  ...
>  let Component = readLazyComponentType(elementType);
>  ...
>}
>// Pending = 0, Resolved = 1, Rejected = 2
>export function readLazyComponentType<T>(lazyComponent: LazyComponent<T>): T {
>  const status = lazyComponent._status;
>  const result = lazyComponent._result;
>  switch (status) {
>    case Resolved: {
>      const Component: T = result;
>      return Component;
>    }
>    case Rejected: {
>      const error: mixed = result;
>      throw error;
>    }
>    case Pending: {
>      const thenable: Thenable<T, mixed> = result;
>      throw thenable;
>    }
>    default: { // lazyComponent 首次被渲染
>      lazyComponent._status = Pending;
>      const ctor = lazyComponent._ctor;
>      const thenable = ctor();
>      thenable.then(
>        moduleObject => {
>          if (lazyComponent._status === Pending) {
>            const defaultExport = moduleObject.default;
>            lazyComponent._status = Resolved;
>            lazyComponent._result = defaultExport;
>          }
>        },
>        error => {
>          if (lazyComponent._status === Pending) {
>            lazyComponent._status = Rejected;
>            lazyComponent._result = error;
>          }
>        },
>      );
>      // Handle synchronous thenables.
>      switch (lazyComponent._status) {
>        case Resolved:
>          return lazyComponent._result;
>        case Rejected:
>          throw lazyComponent._result;
>      }
>      lazyComponent._result = thenable;
>      throw thenable;
>    }
>  }
>}
>```
>
>*注：如果 readLazyComponentType 函数多次处理同一个 lazyComponent，则可能进入Pending、Rejected等 case 中。*
>
>从上述代码中可以看出，对于最初 `React.lazy()` 所返回的 LazyComponent 对象，其 _status 默认是 -1，所以**首次渲染**时，会进入 readLazyComponentType 函数中的 default 的逻辑，这里才会真正异步执行 `import(url)`操作，由于并未等待，随后会检查模块是否 Resolved，如果已经Resolved了（已经加载完毕）则直接返回`moduleObject.default`（动态加载的模块的默认导出），否则将通过 throw 将 thenable 抛出到上层。
>
>为什么要 throw 它？这就要涉及到 `Suspense` 的工作原理，我们接着往下分析。
>
>
>
>**Suspense 原理**
>
>由于 React 捕获异常并处理的代码逻辑比较多，这里就不贴源码，感兴趣可以去看 [throwException](https://github.com/facebook/react/blob/16.8.3/packages/react-reconciler/src/ReactFiberUnwindWork.js#L190) 中的逻辑，其中就包含了如何处理捕获的异常。简单描述一下处理过程，React 捕获到异常之后，会判断异常是不是一个 thenable，如果是则会找到 SuspenseComponent ，如果 thenable 处于 pending 状态，则会将其 children 都渲染成 fallback 的值，一旦 thenable 被 resolve 则 SuspenseComponent 的子组件会重新渲染一次。
>
>
>
>为了便于理解，我们也可以用 componentDidCatch 实现一个自己的 Suspense 组件，如下：
>
>```jsx
>class Suspense extends React.Component {
>  state = {
>    promise: null
>  }
>
>  componentDidCatch(err) {
>    // 判断 err 是否是 thenable
>    if (err !== null && typeof err === 'object' && typeof err.then === 'function') {
>      this.setState({ promise: err }, () => {
>        err.then(() => {
>          this.setState({
>            promise: null
>          })
>        })
>      })
>    }
>  }
>
>  render() {
>    const { fallback, children } = this.props
>    const { promise } = this.state
>    return <>{ promise ? fallback : children }</>
>  }
>}
>```
>
>
>
>**小结**
>
>至此，我们分析完了 React 的懒加载原理。简单来说，React利用 `React.lazy`与`import()`实现了渲染时的动态加载 ，并利用`Suspense`来处理异步加载资源时页面应该如何显示的问题。

















