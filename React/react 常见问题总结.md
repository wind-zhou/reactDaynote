# react 常见问题总结

## react 虚拟dom和diff算法

**虚拟dom：**

说简单点，就是一个普通的 JavaScript 对象，包含了 `tag`、`props`、`children` 三个属性。用来模拟真实dom，减少对d真实dom的操作。

虚拟dom存在内存中，用户更新前后两次的diff算法对比。

**react的三大策略：**





- **传统Diff：**diff算法即差异查找算法；对于Html DOM结构即为tree的差异查找算法；而对于计算两颗树的差异时间复杂度为O（n^3）,显然成本太高，React不可能采用这种传统算法；

- **React Diff：**

  - 之前说过，React采用虚拟DOM技术实现对真实DOM的映射，即React Diff算法的差异查找实质是对两个JavaScript对象的差异查找；
  - 基于三个策略：

  1. Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计。（tree diff）
  2. 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结（component diff）
  3. 对于同一层级的一组子节点，它们可以通过唯一 id 进行区分。（element diff）

![clipboard.png](https://segmentfault.com/img/bVbgaj4?w=2194&h=1118)





​      **基于中Diff的开发建议**

- **基于tree diff：**
  1. 开发组件时，注意保持DOM结构的稳定；即，尽可能少地动态操作DOM结构，尤其是移动操作。
  2. 当节点数过大或者页面更新次数过多时，页面卡顿的现象会比较明显。
  3. 这时可以通过 CSS 隐藏或显示节点，而不是真的移除或添加 DOM 节点。
- **基于component diff**：
  1. 注意使用 shouldComponentUpdate() 来减少组件不必要的更新。
  2. 对于类似的结构应该尽量封装成组件，既减少代码量，又能减少component diff的性能消耗。
- **基于element diff**：
  1. 对于列表结构，尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，在一定程度上会影响 React 的渲染性能。
  2. 加上 key之后就可以了

## webpack常用配置项将要说明

```css
entry:打包的入口文件，一个字符串或者一个对象

output:配置打包的结果，一个对象

fileName：定义输出文件名，一个字符串

path：定义输出文件路径，一个字符串

module:定义对模块的处理逻辑，一个对象

loaders：定义一系列的加载器，一个数组

[

   {

​      test:正则表达式，用于匹配到的文件

​       loader/loaders：字符串或者数组，处理匹配到的文件。如果只需要用到一个模块加载器则使用

​        loader：string，如果要使用多个模块加载器，则使用loaders：array

​        include:字符串或者数组，指包含的文件夹

​        exclude：字符串或者数组，指排除的文件夹

   }

]

resolve:影响对模块的解析，一个对象

extensions：自动补全识别后缀，是一个数组

plugins:定义插件，一个数组

```

## webpack 的优化

三个大方向

- 减少前端资源体积

  - ##### webpack 4 开启 `production` 模式

  - ##### 压缩代码（使用 bundle-level minifier 和 loader options 压缩代码。）

  - ##### 压缩图片资源（url-loader）

    >`url-loader` 可以将小型静态文件内联到应用程序中。如果不进行配置，它将把接受一个传递的文件，将其放在已编译的包旁边，并返回该文件的url。但是，如果指定 limit 选项，它将把小于这个限制的文件编码为Base64 数据的 url 并返回这个url，这会将图像内联到 JavaScript 代码中，从而可以减少一个HTTP请求。

  - ##### 优化第三方依赖(大库换小库，如uuid---->nano)

- 使用长期缓存

  - ####代码（路由组件）懒加载

- 监控和分析应用程序

  - 在开发阶段使用 [webpack-dashboard](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FFormidableLabs%2Fwebpack-dashboard%2F) 和 [bundlesize](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fsiddharthkp%2Fbundlesize) 来调整应用程序的大小
  - 使用像 webpack-dashboard 和 webpack-bundle-analyzer 这样的工具来了解你的应用程序有多大。

## 理解react的单向数据流

 react的编程思想是严谨且周密的，它约束了我们的花式操作，**这是为了确保我们在使用react构建复杂项目的时候不会出现太多问题**。

​      而好处也是显而易见的——我们写react项目，**一旦出现了问题，那么我们会很轻松的发现，根源几乎集中在props和state这俩实例属性上。**

​       单向数据流是react规范的数据流向,它的作用是极大的降低了我们组件间通信的代码耦合，让组件间的通信更为清晰，debug直接往props中找(后面会介绍context)。

​      也就是说，基于react严谨且周密的编程思想，制订了单向数据流这样的通信约束，使得我们react项目中的数据传递结构稳定且易耦合

## 第一次网页打开过慢的优化

![mark](http://qiniu.wind-zhou.com/blog/210514/l87klhLL0g.png?imageslim)



## 如何判断数组中存在某个元素

## reduce使用

## 把一个数组每三个元素分为一个数组，生成二维数组

## 如何获取一个dom元素的宽高

## clientWidth

## 超出显示成省略号

## div中有多行，超出指定行显示省略号

超出一行显示省略号

```css
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
```

超出指定行，显示省略号

```css
div内显示两行或三行，超出部分用省略号显示

    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;（行数）
    -webkit-box-orient: vertical;
```

## promise的方法all，race，allSettled





## react key的作用

## react性能优化

## http状态码



## 网站主题切换



## git 冲突解决



## git stash





## mvvm

![image-20210517093649342](C:\Users\zhouzheng\AppData\Roaming\Typora\typora-user-images\image-20210517093649342.png)













