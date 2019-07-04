---
title: javascript面向切面编程
date: 2018-04-03 15:34:54
tags: [javascript,前端性能优化]
categories: 前端
---

## 概念
面向切面编程（Aspect Oriented Programming）简称（AOP）。

## 理解
AOP主要是实现的目的是针对业务处理过程中的切面进行提取，它所面对的是处理过程中的某个步骤或阶段，以获取逻辑过程中各部分之间低耦合性的隔离效果。
**个人理解为就是在这个函数中增加两个函数，在这个函数执行前运行，和这个函数执行后运行，相当于钩子函数，不影响原来的函数**

## 作用
1. 无侵入统计某个函数的执行时间
2. 表单校验
3. 统计埋点
4. 事务处理
5. 异常处理 等等

## 代码
```javascript
function test() {
  console.log(2);
  return 'test';
}

Function.prototype.before = function (fn) {
  var __self = this;
  return function () {
    fn.apply(this, arguments);
    return __self.apply(__self, arguments); //为了返回 test方法的 return
  };
};

Function.prototype.after = function (fn) {
  var __self = this;
  return function () {
    var result = __self.apply(__self, arguments);
    fn.apply(this, arguments);
    return result; //为了返回 test方法的 return
  };
};

test.before(function () {
  console.log(1);
}).after(function () {
  console.log(3);
})();

//chrome 输出 
1
2
3
"test"
```