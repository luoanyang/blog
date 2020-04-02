---
title: Nodejs模块加载机制
date: 2019-08-06 20:12:19
tags: nodejs
categories: 后端
---

## 模块加载方式
1. 从文件模块缓存中加载
2. 从原生模块加载
3. 从文件加载

## require方法加载模块
require方法接受以下几种参数的传递：
1. http、fs、path等，原生模块。
2. ./mod或../mod，相对路径的文件模块。
3. /public/mod, 绝对路径的文件模块。
4. mod，非原生模块的文件模块。

代码演示：

```js
// hello.js
function hello(){
  console.log('hello');
}
module.exports = hello;

// index.js
const hello = require('./hello.js');
hello(); // 输出：hello
```

## 模块加载流程图
<img src="https://raw.githubusercontent.com/luoanyang/blog-hexo/master/source/img/WX20200101-232649%402x.png" width="300px">

