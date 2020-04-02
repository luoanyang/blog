---
title: Nodejs事件驱动机制
date: 2019-08-02  22:03:53
tags: nodejs
categories: 后端
---

## 事件驱动模型
就是通过 **事件触发（EventEmitters）** 放到 **事件队列（Events）** 中，**事件循环（Event Loop）** 逐个处理事件队列的事件，触发该事件的处理句柄。

图例：
![image](https://raw.githubusercontent.com/luoanyang/blog-hexo/master/source/img/image.png)

## 事件处理代码流程
1. 引入events对象，创建eventEmitter对象
2. 绑定事件处理对象
3. 触发对象

代码：
```js
// 引入 Event 模块,并创建eventsEmitter对象
const events = require('events');
const eventEmitter = new events.EventEmitter();

// 绑定事件处理函数
const connectHandler = function connceted(){
  console.log('connceted 被调用');
}

eventEmitter.on('connection',connectHandler);

// 触发事件
eventEmitter.emit('connection');
console.log('程序执行完毕！')

```

