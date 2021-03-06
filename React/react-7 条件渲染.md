#条件渲染与列表渲染

## 条件渲染

**简介：**

在React中，你可以创建不同的组件来封装各种你需要的行为。然后，依据应用的不同状态，你可以只渲染对应状态下的部分内容。React中的条件渲染和JavaScript中的一样，使用JavaScript运算符if或者条件运算符去创建元素来表现当前的状态，然后让React根据它们来更新UI。

**例如**:登录组件，登录前和登陆后渲染不同的组件

### 通过if

```jsx
import React from "react"

class IfRender extends React.Component {
    constructor() {
        super()
        this.state = { islogin: false }
    }

    render() {
     
            if(!this.state.islogin){

                return(
                    <div>
                         <button onClick={this.login}>登陆</button>
                    </div>
                )
            }else{
                return(
                    <div>恭喜登陆</div>
                )
            }
    }

    login=()=>{
        this.setState({islogin:!this.state.islogin})
    }
}

export default IfRender

```

### 通过运算符&&

>**核心**：**因为在JavaScript中，true && expression总是会返回expression，而false &&expression总是会返回false，因此， 如果条件是true, &&右侧的元素就会被道染， 如果是false, React 会忽略并跳过它。**



**原理：**

**通过花括号包裹代码**，你可以在JSX中**嵌入任何表达式**。这也包括JavaScript中的逻辑与(&&)运算符。它可以很方便地进行元素的条件渲染。



**示例：**

```jsx
import React from "react"


class AndRender extends React.Component {
    constructor() {
        super()
        this.state = { islogin: false }
    }

    render() {
        return(
            <div>
               <button onClick={this.login}>登录</button>
               {this.state.islogin&&<div>hello 周正</div>}
            </div>
        )
    }
    
    login=()=>{
        this.setState({islogin:!this.state.islogin})  //每次点击改变state里的值（取反）
    }
}

export default AndRender
```

上面的案例中，点击按钮，可是实现每次点击的显示隐藏。



### 条件渲染

> **通过三目运算符进行判断。**



**示例：**

```jsx

// 使用三目运算符进行条件渲染

import React from "react"


class IsRender extends React.Component {
    constructor() {
        super()
        this.state = { islogin: false }
    }

    render() {

        const isLogin=this.state.islogin

        
        return(
      

            <div>
               <button onClick={this.login}>登录</button>
               {isLogin?(<p> 我己经登录了</p>):(<p>请先登录</p>)}
            
            </div>
        )
    }
    
    login=()=>{
        this.setState({islogin:!this.state.islogin})
    }
}

export default IsRender
```

效果是点击按钮实现不同的渲染结果。

## 列表渲染

### Map()处理数据和对象的数据



**进行列表渲染时，最常用到的是数组的map方法。**



map()是数组的一个方法，它创建一个新数组， **其结果是该数组中的每个元素都调用个提供的函数后返回的结果**。

 map()里面的处理函数接受两个参数，分别指代**当前元素**、**当前元素的索引**。

>注：map方法不会改变原数组。



>在进行列表渲染时，还会常用到Object.values()方法，该方法的作用是遍历对象的值，并返回一个数组，接下来便可以用map去遍历。

> map中的操作一般包括建立节点，并将数据填充到相应位置。最后将map的返回值使用{}包裹，放到render中的return中。**（这里利用了jsx和数组的关系：数组在渲染时，会自动展开）**





### key 值的设置

####key值的作用：

**Keys可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化。**

（其实就是在做diff算法时使用）

因此你应当给数组中的每个元素赋予个确定的标，一个元素的key最好是这个元素在列表中拥有的一个独一 无的字符串。

####定义规则：

**通常，我们使用来自数据的id作为元素的key**,当元素没有确定的id时，你可以使用他的序列号索引index作为key (若元素没有重排，该方法效果不错，但重排会使得其变慢。)

>**注**：尽量不要用索引值作为key值。**因为索引值不是固定的**，可以用数据的id作为key（后台传过来的数据都会带有一个id，因为数据库有的数据都有一个唯一的id）



#### 深度解析key的必要性

- **Keys是React用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识**。

- 在开发过程中，我们需要保证某个元素的key在其同级元素中具有唯一 性。**在React Diff算法中React会借助元素的Key值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染**。此外，React 还需要借助Key值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中Key的重要性。（这句话怎么理解？）

##  综合练习：

###（1）react 封装ajax



```js
//使用ajax请求会菜品的数据，并在 页面系显示

import React from "react" 

class Ajax extends React.Component{
    constructor(){
        super()
        this.state={info:''}
    }

    componentDidMount(){

    }

    render(){
            let vegeCon=Object.values(this.state.info);
             var trList= vegeCon.map((values)=>{

                return(
                    <tr>
                        <td> {values.dage_coach}</td>
                        <td> {values.evaluate_coach}</td>
                        <td> {values.honor_coach}</td>
                        <td> {values.id_coach}</td>
                        <td> {values.name_coach}</td>
                        <td> {values.tage_coach}</td>
                        <td> {values.type_coach}</td>
                     </tr>
                )
            })
            

        return(
            <div>
                <button onClick={this.showData}>请求数据</button> {/*   //点便去服务器请求数据，并将请求的数据存放在state中 */}
                <table>
                    <tbody>
                        {trList}
                    </tbody>
                </table>
            </div>
        )
    }

    showData=()=>{  
        let xml=new XMLHttpRequest();
        xml.open("post", "http://www.qhdlink-student.top/student/coacha.php");
        xml.setRequestHeader("content-type", "application/x-www-form-urlencoded");
        xml.send("username=zyp&userpwd=123456&userclass=64&type=4")
        xml.onreadystatechange=()=>{
            if(xml.readyState === 4 && xml.status === 200){
                this.setState({info: JSON.parse(xml.responseText) })
            }
        }
    }
}

export default Ajax
```

### （2）二级导航

**js**

```jsx
import React from "react"  
import "./decond.css"

class SecondMenu extends React.Component{
    constructor(){
        super()
        this.state={info:''}
      
    }

    componentDidMount=()=>{
        this.showData()
    }
    render(){
     let con=this.state.info

    if(this.state.info!==""){  //第一次render的时候不用处理数据

      var  navlist= con.map((value)=>{
 
                if(value.type==="0"){  //如果没有二级菜单
                 return(  <li key={value.id_m}> <a href={value.addr_m}> {value.title_m}</a>  </li>)  
                }else{
                    return(
                        <li key={value.id_m}>
                             <a href={value.addr_m}> {value.title_m}</a>
                             <ul>
                                { value.children_m.map((second,index)=>{
                                    return(
                                        <li key={index}><a href={second.addr_c}> {second.title_c}</a></li>
                                    )
                                })}
                             </ul>
                        </li>
                    )
                }
            })
    }

    
    console.log(con)

        return(
            <div>
                <ul id={"nav"}>
                    {navlist}
                </ul>
            </div>
        )
    }


    showData=()=>{  
        let xml=new XMLHttpRequest();
        xml.open("post", "http://www.qhdlink-student.top/test/nav.php");
        xml.setRequestHeader("content-type", "application/x-www-form-urlencoded");
        xml.send("username=zyp&userpwd=123456&userclass=64&type=4")
        xml.onreadystatechange=()=>{
            if(xml.readyState === 4 && xml.status === 200){
                // console.log(xml.responseText)
                this.setState({info: JSON.parse(xml.responseText) })
            }
        }
    }
}


export default SecondMenu

```



**css**

```css
* {
    box-sizing: border-box;
}

#nav {
    width: 300px;
    margin: 0 auto;
    text-align: center;
}

#nav>li {
    width: 100px;
    float: left;
    background-color: pink;
}

#nav>li ul {
    display: none;
    background-color: green;
}

#nav>li:hover ul {
    display: block;
}
```







