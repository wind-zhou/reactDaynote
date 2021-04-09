# react bug总结

## （1） react的浏览器插件导致的运行出错

**问题描述：**

当使用`npm start`运行react项目时，报错如下：

<img src="http://qiniu.wind-zhou.com/blog/210407/bmCK96hC9g.png?imageslim" alt="mark" style="zoom:25%;" />

**问题出现原因：**应该是由于react扩展程序版本问题(好像在react项目中可以配置指定版本号)

**解决方法：**关闭此扩展程序

![mark](http://qiniu.wind-zhou.com/blog/210407/fHGE11E5J8.png?imageslim)