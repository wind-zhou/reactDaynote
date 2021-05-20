#  lesson-1 React

## 什么是React

- **react是用于构建用户界面的JavaScript框架**
- 是一个将数据渲染为html视图的开源JavaScript库（主要用于操作dom呈现界面）
- 由facebook开发，开源
- 起初由facebook的软件开发工程师创建，2012年部署在Instagram，2013年宣布开源



## 为什么学React

**因为原生js有一些固有缺点：**

（1）原生js操作dom繁琐，效率低（Dom-api操作ui）

（2）使用js直接操作dom，浏览器会进行大量重绘重排

（3）原生js没有组件化的编程方案，代码复用率低

​	

## React的特点

1. 声明式设计---React采用声明规范，可以轻松描述应用
2. 高效 ---使用虚拟dom+diff算法，，尽量减少与真实dom的交互
3. 灵活 ---React可以和已知的库和框架很好融合
4. JSX ---jsx是原生js扩展的语法。
5. 组件
6. 单向响应的数据流
7. 使用React Native中使用React语法进行移动端开发

>####什么是组件化和模块化？
>
>**模块化不等于组件化**。
>
>**模块化是在文件层面上，对代码的拆分，组件化是在设计层面上，对UI的拆分**。
>
>模块化：讲一个大文件拆分成多个相互依赖的小文件，最后进行统一的拼装和加载。
>
>组件：每个包含模板（html）+样式（css）+逻辑（js）功能完备的结构单元，称之为组件化



## 和其他框架相比的优点

### 高效的性能

**React实现原理**：Virtual DOM（虚拟DOM）



![mark](http://qiniu.wind-zhou.com/blog/210406/dD15mD1Cgk.png?imageslim)

>扩展阅读：
>
>[什么是虚拟dom](https://mp.weixin.qq.com/s/oAlVmZ4Hbt2VhOwFEkNEhw)
>
>
>
>[从 React 历史的长河来聊如何理解虚拟 DOM](https://zhuanlan.zhihu.com/p/99973075)

### diff算法

**作用：**

计算出Virtual DOM中真正变化的部分，并只针对该部分进行原生DOM操作，而非渲染整个界面。

**传统diff算法：**

通过循环递归对节点进行依次对比，算法复杂度达O(n^3)，n是节点数

**React的diff算法：**

将Virtual DOM转换成actual DOM树的最小操作过程称为调和。 diff算法是调和的具体实现。

**diff三大策略：**

- tree diff

  Web Ui中DOM节点跨层级移动操作较少，可忽略不计。

  >通过DOM
  >
  >

- component diff

  拥有相同类的两个组件，生成相似的树形结构。

  拥有不同类的两个组件，生成不同的树形结构。

- element diff

  通过同一层的一组子节点，通过唯一的id区分

### 分离的框架设计

- React.js现有版本已经将源码分为React DOM和React.js

## React 应用范围

- web 端应用
- 原生应用：ios、Android、Native 应用
- node.js服务端渲染html

















