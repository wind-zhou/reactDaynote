# **项目练习总结**

> 这个案例虽然简单，但是汇聚了太多的点，以至于让我这只菜鸟第一次写完这个项目后不得不总结下，接下来是我在这个项目中学到的东西

## 关于组件间的传值

- 父传子----直接使用props即可

-  字传父----由父组件通过props传递过来一个回到函数给子组件，然后在自己的触发的事件中调用

## 几个react编程原则

- **状态数据要存放在各个组件公共的父组件中（方便后续传值）**

- **状态数据在哪里，处理状态的方法就写在哪里！**



这个案例中所有的状态都在父组件App中，所有任何操作状态的方法都定义App组件中，只不过，谁会修改状态，就把该方法通过props传给哪个子组件。

>**扩展：**
>
>这里让我想到了redux，所有操作状态（prestate）的方法都定义在了reduces中，各个组件要想操作状态，就要通过action传递给reduces函数（里面包含了一些必要的数据，例如要操作的id）。
>
>这里也能看出redux和原生react的区别：redux会使用store帮你管理状态，而原生react只能组件自己维护状态，需要跨组件通信，只能一层层传递

##   input：checkbox 的基础知识

- defaultChecked   默认的checkbox的选中状态 

  > 注意：这个属性定以后慎用，因为他只能改变一次

- checkd 选中时值为true，没选中时值为false

  >这个属性很重要！
  >
  >因为在**开发中，checkbox和对应数据中的isLogin属性往往需要双向绑定**，这个属性可以很方便的让我们知道checkbox当前的状态，并根据此状来控制（或受控）isLogin。
  >
  >否则我们就要自己定义一个状态字段来标识当前checkbox的状态，以前我就是这么写的，很麻烦。

## 事件对象（event）的几个常用属性

判断触发事件的是哪个键盘上的键，可以使用event.keyCode

## 加深了state 驱动页面渲染的理解

react中，可变的参数都可以放在状态里，例如本次本项目的每个li后面删除按钮的显示隐藏。

>**状态state如果存放的是bool值，则常会跟条件运算符结合在一起使用后**
>
>```jsx
><button
>    className="btn btn-danger"
>    onClick={() => {this.delTodo(todoObj.id);}}
>    style={{display: this.state[todoObj.id] ? "inline-block" : "none"}}
>    >
>    删除
></button>
>```

## react 声明周期的复习

以前只是硬背诵react的新旧声明周期，但这次真正的在项目中使用到了。

本例中使用到了两个钩子函数：`componentWillMount`；`componentWillReceiveProps`

```js
  componentWillMount() {
    //   在渲染吧前，先预存一个状态的默认值
    this.props.todo.map(todoObj => {
      this.setState({
        [todoObj.id]: false
      });
    });
  }

  componentWillReceiveProps(props) {
    //   每次更新前将状态写进state，先预存一个状态的默认值
    props.todo.map(todoObj => {
      this.setState({
        [todoObj.id]: false
      });
    });
  }
```

>
>
>这里我想说一个我之前常遇到的问题，就是有时可能会给某些节点触发某个事件时增加一个state的值，但这是要注意，这可能会导致第一次渲染时，state还没有存放这些值，产生报错（获取不到某个state），这就需要在初始化时给每个item先设置一个默认的值，可以放在componentWillMount中，如果新增加的item项也要设置默认值，可以放在componentWillReceiveProps中(因为componentWillMount每个生命后期只执行一次，后面增加的他就管理不到了)。
>
>这里我开始编程时还犯了一个错误，就是我第一次把新增item的state默认值的设置写在了componentWillUpdata中，这就反了大忌了，因为componentWillUpdata中不允许操作setState了，因为这会导致再次返回判断
>
>`shouldComponentUpdate(newProps,newState)`，导致程序迭代陷入死循环。

## 关于mouseEnter和mouseLeave时间触发时的简写

本例中有一个需求，就是划入li时，后面的删除button显示，划出时消失，这时可以采取下面的简写形式

```jsx
<li
    style={{
        backgroundColor: this.state[todoObj.id] ? "#eee" : "white"
    }}
    key={todoObj.id}
    onMouseEnter={this.change(true, todoObj.id)}
    onMouseLeave={this.change(false, todoObj.id)}
    >

    ----------------------------------------
    change = (state, id) => {
        return () => {
            this.setState({
                [id]: state
            });
        };
    };
```

两个事件指向的处理函数一样，只是传入的参数值不同，结合高阶函数，可以写出这么简洁的形式，这时我之前怎么也想不到的。



## 数组的几个常用方法

项目最后的步骤一定是操作和处理数据，基本上都是直接处理数组或字符串。

几个常用的方法：

- map  （更改时常用）

- filter  （删除时常用）

## 解构赋值和扩展运算符

这俩es6的新增语法，使用及其广泛。

本例中，**使用props传递过来的值，基本上第一步都进行了解构赋值**（方便后续的操作，使其不至于太长）。

**数组增加数据时，经常会和扩展运算符配合使用**，并且修改isDone的值是也用到了扩展运算符。

## 关于组件的划分

组件划分原则，如果有的组件需要复用，则拆分拆分出来，否则就写成一坨，毕竟相互传递还是有点麻烦的。

**当然也不是不能细拆，得加钱!**`(o(*￣︶￣*)o）`

