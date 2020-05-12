---
title: 草稿
tags:
---

## 几个特殊的API
1. SetTimeout和SetInterval 线程池不参与
2. process.nextTick() 实现类似 SetTimeout(function(){},0);每次调用放入队列中，在下一轮循环中取出。
3. setImmediate();比 process.nextTick()优先级低。

```js

// 观察者

// setTime 0 时比 setImmediate执行快
setTimeout(function(){
  console.log(1);
},0);

setImmediate(function(){
  console.log(2);
});

// 同步任务执行完毕
process.nextTick(()=>{
  console.log(3);
});

new Promise((resolve,reject)=>{
  // promise 写的时候是同步， then 异步的
  console.log(4);
  resolve(4);
}).then(function(){
  // Promise 
  console.log(5)
});

console.log(6)

// 4
// 6
// 3
// 5
// 1
// 2
```