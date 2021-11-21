---
title: Webpack入门笔记
date: 2020-06-09 20:52:31
tags:
  - 技术
  - 百宝箱
---
## Webpack是什么

一个现代JavaScript应用程序的静态模块打包器

1. 默认：只对js进行处理，其他类型文件需要配置loader或者插件进行处理。
2. 打包：将各个依赖文件进行梳理打包，形成一个JS依赖文件。

## webpack产生的背景

为什么需要打包？因为：

1. 各个依赖文件的关系难以梳理，耦合程度较高，代码难以维护。
2. 把所有依赖包都打包成为一个js文件（bundle.js）文件，会有效降低文件请求次数，一定程度提升性能。
3. 逻辑多、文件多，项目复杂度提高

webpack除提供上述功能外，还充当了“翻译官”的角色，例如将ES6翻译为低版本的语法，将less、sass翻译为css等功能。

强大且灵活，loader和plugin可插拔。

## 前端模块化

推荐两个很好看的讲前端模块化历程的ppt

- [JS模块化七日谈——黄玄](https://huangxuan.me/2015/07/09/js-module-7day/)
- [前端模块化那点事——玉伯](https://github.com/seajs/seajs/issues/588)

一个工程由各个模块组成，理想情况是高内聚低耦合，各司其职

### 作用域

定义：运行时变量、函数、对象可访问性

作用域决定了代码中变量和其他资源的可见性

1. 全局作用域

```
var a = 1;
window.a; // 1
global.a; // 1Copy
```

1. 局部作用域

```
function a(){
    var v = 1;
}
window.v; // undefinedCopy
```

如果引入多个script 会造成全局作用域冲突而导致不可预测的风险

```
<body>
<script scr="./moduleA.js"></sciprt>
<script scr="./moduleB.js"></sciprt>
<script scr="./moduleC.js"></sciprt>
</body>Copy
```

改进步骤一，使用变量作用域形成局部作用域：

```
// 定义模块内的局部作用域，以moduleA为例
var Susan = {
	name: "susan",
    sex: "female",
    tell: function(){
    	console.log("im susan")
    }
}Copy
```

但是步骤一无法保证模块属性内部安全性，比如可能不小心改掉属性值，可以通过立即执行函数进行改写，形成闭包。

```
// 定义模块内的闭包作用域（模块作用域），以moduleA为例
var SusanModule = (function(){
    var Susan = {
    // 自由变量
	name: "susan",
    // 自由变量
    sex: "female",
    // 只允许访问tell方法，不能访问和修改其他属性
    return {
    	tell: function(){
    		console.log("im susan")
    	}
    }
})()Copy
```

如果加上参数(依赖)，就是前端模块化的基石了（来自黄玄的blog）

```
var Module = (function($){
    var _$body = $("body");     // we can use jQuery now!
    var foo = function(){
        console.log(_$body);    // 特权方法
    }

    // Revelation Pattern
    return {
        foo: foo
    }
})(jQuery)

Module.foo();Copy
```

### 模块化优点

· 模块化封装，安全性高
· 可重用
· 解除耦合

### 模块化方案进化史

随着模块化优势体现，开发者更倾向于使用模块化协同开发项目，于是在发展过程中形成了很多规范：AMD、COMMONJS、ES6 MODULE

#### AMD规范（Asynchronous Module Definition）

异步模块定义：

```
define("getSum", ["math"], funtion(math){
	return function (a,b){
    	console.log("sum:"+ math.sum(a, b))
    }
})Copy
```

#### COMMONJS

2009年出的规范，原本是为服务端的规范，后来nodejs采用commonjs模块化规范

```
// 通过require函数来引用
const math = require("./math");
// 通过exports将其导出
exports.getSum = function(a,b){
 	return a + b;
}
Copy
```

#### ES6 MODULE

```
// 通过import函数来引用
import math from "./math";
// 通过export将其导出
export function sum(a, b){
   	return a + b;
}Copy
```

## Webpack 打包机制

根据import引入等关键字，将依赖文件打包成一个文件。

输出文件的大体结构：

```
(function(module) {
   	var installedModules = {};
    function __webpack_require__(moduleId){
    	// SOME CODE
    }
    // 。。。
    return __webpack_require__(0); // entry file
})([ /* modules array */])Copy
```

上述结构中的核心方法：

```
function __webpack_require__(moduleId){
   	// check if module is in cache
    if(installedModules[moduleId]){
    	return installedModules[moduleId].exports;
    }
// create a new module (and put into cache)
var module = installedModules[moduleId] = {
       	i: moduleId,
        l: false,
        exports: {}
    };
// exe the module func
    modules[moduleId].call{
      	module.exports,
        module,
        module.exports,
        __webpack_require__
    };
// flag the module as loaded
    module.l = true;
    // return the exxports of the module
    return module.exports;
    }Copy
```

打包过程：

1. 从入口文件开始，分析整个应用的依赖树
2. 将每个依赖模块包装起来，放到一个数组中等待调用
3. 实现模块加载的方法，并把它放到模块执行的环境中，确保模块间可以互相调用
4. 把执行入口文件的逻辑放在一个函数表达式中，并立即执行这个函数

## npm

### npm install过程

- 寻找报版本信息文件（package.json）按照它进行安装
- 查找package.json中的依赖，检查项目中其他版本信息文件
- 发现新包，更新版本信息文件

## 常用Plugins

### babel

ES6 -> ES5

使用方法（直接编译）：
`babel -index.js --presets=@babel preset-env`

使用方法1：
在package.json中加入babel配置参数

```
"babel": {
	"presets" : ["@babel/preset-env"]
}
Copy
```

使用方法2：
在package.json文件同目录下，设置.babelrc文件配置，同方法1

### html-webpack-plugin

又有一篇入门文[html-webpack-plugin详解](https://www.cnblogs.com/wonyun/p/6030090.html)

- 为html文件中引入的外部资源如script、link动态添加每次compile后的hash，防止引用缓存的外部文件问题
- 可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置N个html-webpack-plugin可以生成N个页面入口

### webpack.config.js配置项

[配置文件详解](https://yq.aliyun.com/articles/595117)

1. 是否缓存，提升webpack打包执行速度
   `cacheDictionary: true/false;`
2. .js .jsx .json文件引用时候，不需要加入后缀，只需要文件名即可
   `resolve: extensions：['.js','.jsx','.json']`

### webpack-dev-server

- webpack-dev-server –open直接打开浏览器运行项目
- 提供文件变化监听，如果项目文件有更新，会自动打包，并刷新页面

### webpack HRM 模块热更新

plugin中加入：`webpack.HotModuleReplacementPlugin()`

### 打包优化

体积优化（打包出来结果大小优化）：TerserPlugin

```
optimization: {
	minimizer: [new TerserPlugin({
    	// 加快构建速度
        cache:true,
        parrlel: true,// 多线程处理
        terserOptions : {
            compress: {
            	//删除掉一些没有用的代码
                unused: true,
                drop_debugger: true,
                drop_console: true,
                dead_code:true
            }
        }
    })]
}
```