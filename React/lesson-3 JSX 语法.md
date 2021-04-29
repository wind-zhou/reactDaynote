

# lesson-3 JSX 语法

## JSX简介

**简介：**

- **Reac的核心机制之一就是可以在内存中创建虚拟的DOM元素**， React利用虚拟DOM来减少对实际
  DOM的操作从而提升性能。
- JSX是React的核心组成部分，它使用XML 标记的方式去直接声明界面，界面组件之间可以互相嵌
  套。可以理解为在J**S中编写与XML类似的语言**，一种定义带属性树结构 (DOM结构)的语法，它的目
  的不是要在浏览器或者引擎中实现，**它的目的是通过各种编译器将这些标记编译成标准的JS语言**。
- react将XML语法直接加入JS中通过代码而非模板来高效的定义界面。**之后JSX通过翻译器转换为纯**
  **JS再由浏览器执行**。在实际开发中，JSX在产品打包阶段都已经编译成纯JavaScript, JSX的语法不
  会带来任何性能影响。另外，由于JSX只是种语法， 因此JavaScript的关键字class, for等也不能出
  现在XML中，而要如例子中所示，使用className, htmilFor代替，这和原生DOM在JavaScript中的创
  建也是一致的。JSX只是创建虚拟DOM的一种语法格式而已，除了用JSX，我们也可以用JS代码来创建
  虚拟DOM.



**jsx的本质 ：**

创建**JSX语法的本质目的是为了使用基于xml的方式表达组件的嵌套**，保持和HTML一致的结构，
语法上除了在描述组件上比较特别以外，其它和普通的Javascript没有区别。

并且**最终所有的 JSX** **都会编译为原生Javascript.**



## JSX语法规则

![mark](http://qiniu.wind-zhou.com/blog/210407/De7C2A4clg.png?imageslim)

## 创建虚拟dom的两种方法

- jsx方式
- React原生方式

### JSX方式

####编程步骤：

（1）创建虚拟节点

```js
const VDOM=<h1 id="test">hello</h1>
```

（2）将虚拟DOM渲染到页面

```react
ReactDOM.render(
    VDOM
,
    document.getElementById('root')
);
```



#### 注意事项

1. 全称: **javascript XML**

2. React定义的一种类似与XML的打展语法js+xml

3. **本质是React createElement(component pops, ... children)方法的语法糖**

4. 作用:用来简化创建虚拟dom

   a. 写法: var ele= \<h1>hello\</h1>
   b. 注意 **(1):它不是字符串，也不是html/xml标签**
   C. 注意 **(2):它最终产生的就是一个js对象**

5. 标签名任意: htm标签或其他标签

**JSX特别接近于JavaScript，而不是HTML，因此在编写它的时有一些关键的区别需要注意：**

- **使用className替代class添加css类**，因为class是JavaScript的保留关键词
- 在JSX中的属性和方法**使用小驼峰命名** —— onclick 将会变成 onClick
- **单标签(闭标签)必须使用斜杠(/)结束** ——



JavaScript表达式可以在大括号中嵌入JSX，包括变量、方法和属性。

```javascript
const name = 'Tania'
const heading = <h1>Hello, {name}</h1>
```

JSX比使用原生JavaScript创建和添加大量元素更容易编写和理解，这也是人们如此喜欢React的原因之一。

#### JSX的数组操作

**JSX允许在模板中插入数组，数组会自动展开所有成员**

基本语法：

```js
var arr=["html",'"JavaScript","css"];
 const VDOM=<h1>{arr}</h1>
ReactDOM.render(VDOM,document.getElementById('root'));
```



示例：利用模板里的数组会展开的特性，可以在里面套入元素。

```js
var arr=[<li>1</li>,<li>2</li>,<li>3</li>,<li>4</li>,<li>5</li>]

ReactDOM.render(
<div>
<NavList></NavList>
<ul>{arr}</ul>
</div>,document.getElementById('root')
);
```

输出：![mark](http://qiniu.wind-zhou.com/blog/210407/GhaaehlE2I.png?imageslim)



### React原生方式

```js
var Vdom=React.createElement("h1",{id:'test'},'hello');      //原生方式创建
ReactDOM.render(VDOM,document.getElementById('root'));
```

参数讲解：

- 第一个参数：标记
- 第二参数：属性（id或者class）
- 第三个参数：标记里的内容

>不过不建议在使用这种方法



## react 组件的创建

react推出后，出现了两种定义组件的方法

- 函数式组件
- 类式组件

### 函数式组件

做纯展示型组件	

函数式组件无状态

返回的是虚拟dom

#### 简介

- [x] 创建函数式组件形式是从React 0.14版本开始出现的。**它是为了创建纯展示组件(简单组件)**，这种
  组件**只负责根据传入的props (属性)来展示**，**不涉及到要state状态的操作**。具体的无状态函数式组
  件
- [x] 其官方指出:
  在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件
  等;这种通过多个简单然后合并成一个大应用的设计模式被提倡。

#### 语法

**无状态函数式组件形式上表现为一个只带有return的函数**，通过函数形式或者ES6 箭头函数的形式创建，并且该组件是无

state状态的。

```js
function NavList() {
    return (
        <div>
            <Nav></Nav>
            <Login> </Login>
        </div>
    )
}
```



>注：**函数首字母必须大写**，函数必须带返回值。



####函数式组件的特点：

（函数式组件与类式组件的区别）

- **组件不能访问this对象**
- 组件不能访问生命周期方法
- 无法访问state，只能访问输入的props，同样的props会得到相同的渲染结果，不会有副作用。

>为什么不能访问this。
>
>函数有react调用，调用过程中，babel会将jsx装换为js，这是会开启严格模式。
>
>严格模式规定全局模式自定义的 函数中的this不能指向window，所以this的结果是undefined



### 类式组件

#### 基本语法：

使用可创建组件，必须继承react内置的一个组件 React Component

```js
class MyDom extends React.Component{
    constructor{
        super()
    }
  render(){
      return(<h1>hello</h1>)
  }
}
ReactDOM.render(MyDOM,mountNode)
```

注意：

1. 使用类创建组件，必须继承react内置的一个组件
2. constructor()方法可以省略
3. **必须要有render()方法，用于生成虚拟dom**，该**方法被定义在类的原型对象上**，供实例使用
4. 实例在哪？实例在编程中并没有显示的呈现，但在将虚拟DOM渲染到页面上时，会实例化，在内存中。



>###**使用了ReactDOM.render()之后发生了什么？**
>
>（1）React解析组件标签，找到组件
>
>（2）发现组件是一个类定义的组件，随后new出来一个实例，并调用实例的原型上的render方法
>
>（3）调用render后，将该方法返回的虚拟dom转换为真实的dom，随后显示在页面中



注：render中的this指向该类创建的实例对象。



> ### **React类组件中自定义的方法函数中的this丢失问题**
>
> 正常情况下，方法中的this指向类实例化后的对象，但是如果此方法没有被类直接调用，而是作为回调函数传给react直接使用，此时的this不会指向window，而是指向undefined，**因为类中的方法在定义时默认 开启了严格模式**





>**函数式组件只能操作props，不能操作state和refs；但是类组件全都能玩。**

