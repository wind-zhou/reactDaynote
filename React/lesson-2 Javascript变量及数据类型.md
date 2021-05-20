# lesson-2 Javascript变量及数据类型

## 一、 变量的概念

变量是存储数据的内存空间

​	--内存:运行中程序数据的暂存空间

### 1.1、变量声明

```js
var x;     //变量声明
x=3;      //变量赋值
```

注：只有在浏览器运行时，js声明的变量才会在内存开辟空间。

### 1.2 变量命名规则

- 变量名以字母和下划线开头，可以包含字母数字下划线
- 不能包含特殊字符
- 不能使用JavaScript关键字和保留字 

JavaScript的关键字和保留字汇总

关键字

```javascript
break     delete    function    return    typeof
case      do        if          switch    var
catch     else      in          this      void
continue  false     instanceof  throw     while
debugger  finally   new         true      with  
default   for       null        try
```

保留字：

```javascript
abstract  boolean  byte  char  class  const  debugger  double  enum  export  extends  final  float  goto
implements  import  int  interface  long  native  package  private  protected  public  short  static
super  synchronized throws  transient  volatile
```

**区分大小写**

###1.3、国际标准命名法：

- 驼峰命名法（**常用**）
- Pascal命名法
- 匈牙利命名法

### 1.4 变量的命名及初始化

#### 先声明后初始化

```javascript
var x;
x=10;
```

#### 直接初始化

```javascript
x=10;
```

**没有声明直接使用的变量，是全局作用域。（不推荐用）**

#### 多变量集体声明

```javascript
var x,y;
```

####同时进行声明和初始化

```javascript
var x=10,y=18;
```

### 1.5、 常用的数据类型

js中的数据类型分为**基本数据类型+复合数据类型**两种。

基本数据类型存放在栈里

栈里存地址，堆里存数据。



**基本数据类型有5种**  数值型（number）、布尔型（Boolean）、空类型（undefined）、字符串（string），null

### 数值型

**数值型不区分整形和浮点型**,用双精度的浮点型来表示数据。取值范围（-2<sup>53</sup>,+2<sup>53</sup>）

3  2.32  NAN
**数值型可以做所有的算术运算**

>如何理解NaN

- 布尔型true 和false
  **布尔型可以做逻辑运算**

- 空类型 （undefined/null）
  当变量声明却没初始化时，变量的值为空类型（可以理解为占地方）

  ![mark](http://qiniu.wind-zhou.com/blog/201202/55H0Gc60Dh.png?imageslim)

  ​		

  >**深入理解undefined和null**
  >
  > js中undefined,null,NaN的区别
  >
  >js中undefined,null,NaN的区别
>
  >类型分析：js中的数据类型有undefined,boolean,number,string,object等5种，前4种为原始类型，第5种为引用类型。
  >
  >```js
  >var a1;
  >var a2 = true;
  >var a3 = 1;
  >var a4 = "Hello";
  >var a5 = new Object();
  >var a6 = null;
  >var a7 = NaN;
  >var a8 = undefined;
  >
  >alert(typeof a);  //显示"undefined"
  >alert(typeof a1); //显示"undefined"
  >alert(typeof a2); //显示"boolean"
  >alert(typeof a3); //显示"number"
  >alert(typeof a4); //显示"string"
  >alert(typeof a5); //显示"object"
  >alert(typeof a6); //显示"object"
  >alert(typeof a7); //显示"number"
  >alert(typeof a8); //显示"undefined"
  >```
  >
  >从上面的代码中可以**看出未定义的值和定义未赋值的为undefined，null是一种特殊的object,NaN是一种特殊的number。**

- 字符串类型（string）
  凡是用单引号``和双引号“ ”包裹起来的都是字符串（不区分单双）

  字符串可以进行拼接，使用“`+`”

  ```javascript
  var x="周正"+"你好"；
  ```

  注**：若数值和字符串进行拼接，数值类型会转换成字符串。**
  
  ![mark](http://qiniu.wind-zhou.com/blog/201203/eLJbh6IKHl.png?imageslim)

### 1.6、类型测试typeof

```javascript
<script>
    var x = "周正";
    var type = typeof x;
    document.write(type);
</script>
```

![mark](http://qiniu.wind-zhou.com/blog/201202/2G8l9E27l2.png?imageslim)

如果只声明却没初始化，则类型为undefined

```javascript
<script>
    var x;
    var type = typeof x;
    document.write(type);
</script
```

![mark](http://qiniu.wind-zhou.com/blog/201202/fGj4JDADg8.png?imageslim)

| 表达式           | 返回值      |
| ---------------- | ----------- |
| typeof undefined | 'undefined' |
| typeof null      | 'object'    |
| typeof true      | 'boolean'   |
| typeof 123       | 'number'    |
| typeof "abc"     | 'string'    |

思考：

```javascript
<script>
    var x = "可乐";
    var y = "雪碧";
    document.write(x, y, "<br>");

    var tmp;
    tmp = "可乐";
    x = "雪碧";
    y = tmp;
    document.write(x, y);
</script>
```

![mark](http://qiniu.wind-zhou.com/blog/201202/62cCc3jG6a.png?imageslim)



