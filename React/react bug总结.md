# react bug总结

## （1） react的浏览器插件导致的运行出错

**问题描述：**

当使用`npm start`运行react项目时，报错如下：

<img src="http://qiniu.wind-zhou.com/blog/210407/bmCK96hC9g.png?imageslim" alt="mark" style="zoom:25%;" />

**问题出现原因：**应该是由于react扩展程序版本问题(好像在react项目中可以配置指定版本号)

**解决方法：**关闭此扩展程序

![mark](http://qiniu.wind-zhou.com/blog/210407/fHGE11E5J8.png?imageslim)





## （2）`TypeError: this.getOptions is not a function`

react项目中，使用swiper，使用自动打包时报错`TypeError: this.getOptions is not a function`

**原因：** less-loader安装的版本过高
**解决方案：**

1. npm uninstall less-loader
2. npm install less-loader@5.0.0

## （3）多级页面路由时的样式丢失问题

**问题描述**：因为css引入时的路径为相对路径，跳转到多级页面后再刷新，就会找不到css文件

解决方案：修改css引入路径为绝对路径



