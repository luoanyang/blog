---
title: js中apply和call
date: 2017-06-01 14:33:13
tags: javascript
categories: 前端
---
参考了[这篇博客](http://blog.csdn.net/myhahaxiao/article/details/6952321),知道了javascript中的apply和call的用法。
<!--more-->
## apply
**方法能劫持另外一个对象的方法，继承另外一个对象的属性.**
```javascript
Function.apply(obj,args)方法能接收两个参数
obj：这个对象将代替Function类里this对象
args：这个是数组，它将作为参数传给Function（args-->arguments）
```
## call
和apply的意思一样,只不过是参数列表不一样.
```javascript
Function.call(obj,[param1[,param2[,…[,paramN]]]])
obj：这个对象将代替Function类里this对象
params：这个是一个参数列表
```
**apply实例**
``` javascript
<script type="text/javascript">
   /*定义一个人类*/    
   function Person(name,age)     
   {     
       this.name=name;  
       this.age=age;  
   }  
   /*定义一个学生类*/  
   functionStudent(name,age,grade)  
   {  
       Person.apply(this,arguments);  
       this.grade=grade;  
   }  
   //创建一个学生类  
   var student=new Student("zhangsan",21,"一年级");  
   //测试  
   alert("name:"+student.name+"\n"+"age:"+student.age+"\n"+"grade:"+student.grade);  
   //测试结果name:zhangsan age:21  grade:一年级  
   //学生类里面我没有给name和age属性赋值啊,为什么又存在这两个属性的值呢,这个就是apply的神奇之处.  
</script>
```
## 特殊用法
apply的一个巧妙的用处,可以将一个数组默认的转换为一个参数列表**[param1,param2,param3]**转换为**(param1,param2,param3)**
可以根据apply的特点来解决 **var max=Math.max.apply(null,array)** ,这样轻易的可以得到一 数组中最大的一项(apply会将一个数组装换为一个参数接一个参数的传递给方法)
Math.min 可以实现得到数组中最小的一项同样和 max是一个思想 **var min=Math.min.apply(null,array);**