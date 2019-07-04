---
title: 初识ES6
date: 2017-07-31 14:45:20
tags: javascript
categories: 前端
---
ES6是新的javascript的语法标准，现在前端开发应用的有很多。再不学习就真的落后了。
<!--more-->
## let和const
[参考链接](http://www.jianshu.com/p/ebfeb687eb70)
这两个的用途与var类似，都是用来声明变量的，但在实际运用中他俩都有各自的特殊用途。
首先来看下面这个例子：

### let
```javascript
var name = 'ES5'

if (true) {
    var name = 'ES6'
    console.log(name)  //ES6
}

console.log(name)  //ES6
```
使用var两次输出都是ES6，这是因为ES5只有全局作用域和函数作用域，没有块级作用域。而let为ES6新增了块级作用域。用它所声明的变量，只在let命令所在的代码块内有效。

```javascript
let name = 'ES5'

if (true) {
    let name = 'ES6'
    console.log(name)  //ES6
}

console.log(name)  //ES5
```

另外一个var带来的不合理场景就是用来计数的循环变量泄露为全局变量，看下面的例子：
```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
上面代码中，变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。而使用let则不会出现这个问题。
```javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
### const
const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。
当我们尝试去改变用const声明的常量时，浏览器就会报错。
const有一个很好的应用场景，就是当我们引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug：
```javascript
const monent = require('moment')
```

## 模板字符串
### 反撇号（`）
请看下面例子:
```javascript
let name = 'ES6';
//es5写法
console.log("hello:"+name); //hello:ES6

//es6写法
console.log(`hello:${name}`); //hello:ES6
```

## 展开运算符
### 3个点（...）
```javascript
//两个对象连接返回新的对象
let a = {aa:'aa'}
let b = {bb:'bb'}
let c = {...a,...b}
console.log(c) //{"aa":"aa","bb":"bb"}

//两个数组连接返回新的数组
let d = ['dd']
let e = ['ee']
let f = [...d,...e]
console.log(f) // ["dd","ee"]

//在中间插入数组
var aa = ['shoulder', 'knees'];
var bb = ['head', ...aa, 'and', 'toes']; // ["head", "shoulders", "knees", "and", "toes"]

//在尾部插入数组
// ES5
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
// 将arr2中的所有元素添加到arr1中
Array.prototype.push.apply(arr1, arr2);

// ES6
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1.push(...arr2);


//数组加上对象返回新的数组
let g = [{gg:'gg'}]
let h = {hh:'hh'}
let i = [...g,h]
console.log(i)  // [{"gg":"gg"},{"hh":"hh"}

//数组+字符串
let j = ['jj']
let k = 'kk'
let l = [...j,k]
console.log(l)  // ["jj","kk"]

//方法中传入不确定参数数量使用
function sum(...m){
	let total = 0;
	for(var i of m){
		total +=i;
	}
	console.log(`total:${total}`);
}
sum(2,5,8,6,4) //total:25
```
## class和extends, super
[参考链接](http://www.jianshu.com/p/ebfeb687eb70)
ES6提供了更接近传统语言的写法，引入了Class（类）这个概念。新的class写法让对象原型的写法更加清晰、更像面向对象编程的语法，也更加通俗易懂。
```javascript
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

class Cat extends Animal {
    constructor(){
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello') //cat says hello

```
上面代码首先用class定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。简单地说，constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实力对象可以共享的。

Class之间可以通过extends关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。上面定义了一个Cat类，该类通过extends关键字，继承了Animal类的所有属性和方法。

super关键字，它指代父类的实例（即父类的this对象）。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。

P.S 如果你写react的话，就会发现以上三个东西在最新版React中出现得很多。创建的每个component都是一个继承React.Component的类。详见react文档


## arrow function(箭头函数)
[参考链接](http://www.jianshu.com/p/ebfeb687eb70)
这个恐怕是ES6最最常用的一个新特性了，用它来写function比原来的写法要简洁清晰很多:
```javascript
//ES5
function(i){ return i + 1; } 

//ES6
(i) => i + 1 
```
如果方程比较复杂，则需要用{}把代码包起来：
```javascript
//ES5
function(x, y) { 
    x++;
    y--;
    return x + y;
}

//ES6
(x, y) => {x++; y--; return x+y}
```
除了看上去更简洁以外，arrow function还有一项超级无敌的功能！
长期以来，JavaScript语言的this对象一直是一个令人头痛的问题，在对象方法中使用this，必须非常小心。例如：
```javascript
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout(function(){
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}

var animal = new Animal()
animal.says('hi')  //undefined says hi

```
运行上面的代码会报错，这是因为setTimeout中的this指向的是全局对象。所以为了让它能够正确的运行，传统的解决方法有两种：
1.第一种是将this传给self,再用self来指代this
```javascript
says(say){
     var self = this;
     setTimeout(function(){
         console.log(self.type + ' says ' + say)
     }, 1000)
```
2.第二种方法是用bind(this),即
```javascript
 says(say){
     setTimeout(function(){
         console.log(self.type + ' says ' + say)
     }.bind(this), 1000)
```

但现在我们有了箭头函数，就不需要这么麻烦了：
```javascript
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
var animal = new Animal()
animal.says('hi')  //animal says hi
```
当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。

## destructuring
ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
看下面的例子：
```javascript
let cat = 'ken'
let dog = 'lili'
let zoo = {cat: cat, dog: dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}
```
用ES6完全可以像下面这么写：
```javascript
let cat = 'ken'
let dog = 'lili'
let zoo = {cat, dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}
```
反过来可以这么写：
```javascript
let dog = {type: 'animal', many: 2}
let { type, many} = dog
console.log(type, many)   //animal 2
```
## default
default很简单，意思就是默认值。大家可以看下面的例子，调用animal()方法时忘了传参数，传统的做法就是加上这一句type = type || 'cat'来指定默认值。
```javascript
function animal(type){
    type = type || 'cat'  
    console.log(type)
}
animal()
```
如果用ES6我们而已直接这么写：
```javascript
function animal(type = 'cat'){
    console.log(type)
}
animal()
```

## Promise
为了解决callback的复杂，让代码变得更清晰。
```javascript
let checkLogin = function(){
	return new Promise(function(resolve,reject){
		let flag = document.cookie.indexOf("userId")>-1?true:false;

		if(flag=true){
			resolve({
				status:1,
				result:true
			})
		}else{
			resolve({
				status:0,
				result:true
			})
		}

	})
}

let getUserInfo = ()=>{
	return new Promise((resolve,reject)=>{
		let userInfo = {
			userId:"101"
		}
		resolve(userInfo);
	})
}

checkLogin().then((res)=>{
	if(res.status==0){
		console.log("login success");
		return getUserInfo();
	}
}).catch((err)=>{
	console.log(`err:${err}`);
}).then((res2)=>{
	console.log(`userId:${res2}`)
})

Promise.all([checkLogin(),getUserInfo()]).then(([res1,res2])=>{
	console.dir(`res1:${res1}`);
	console.dir(`res2:${res2}`);
})

//输出如下：
// login success
// res1:[object Object]
// res2:[object Object]
// userId:[object Object]
```

## import和export实现模块化
[参考链接](http://www.cnblogs.com/diligenceday/p/5503777.html)
第一种导出的方式：
在lib.js文件中， 使用 export{接口} 导出接口， 大括号中的接口名字为上面定义的变量，import和export是对应的；
```javascript
//lib.js 文件
let bar = "stringBar";
let foo = "stringFoo";
let fn0 = function() {
    console.log("fn0");
};
let fn1 = function() {
    console.log("fn1");
};
export{ bar , foo, fn0, fn1}

//main.js文件
import {bar,foo, fn0, fn1} from "./lib";
console.log(bar+"_"+foo);
fn0();
fn1();
```

第二种导出的方式：
在export接口的时候， 我们可以使用 XX as YY， 把导出的接口名字改了， 比如： closureFn as sayingFn， 把这些接口名字改成不看文档就知道干什么的：
```javascript
//lib.js文件
let fn0 = function() {
    console.log("fn0");
};
let obj0 = {}
export { fn0 as foo, obj0 as bar};

//main.js文件
import {foo, bar} from "./lib";
foo();
console.log(bar);
```

第三种导出的方式：
这种方式是直接在export的地方定义导出的函数，或者变量
```javascript
//lib.js文件
export let foo = ()=> {console.log("fnFoo") ;return "foo"},bar = "stringBar";

//main.js文件
import {foo, bar} from "./lib";
console.log(foo());
console.log(bar);
```

第四种导出的方式：
这种导出的方式不需要知道变量的名字， 相当于是匿名的， 直接把开发的接口给export；如果一个js模块文件就只有一个功能， 那么就可以使用export default导出；
```javascript
export default "string";

//main.js
import defaultString from "./lib";
console.log(defaultString);
```

第五种导出方式：
export也能默认导出函数， 在import的时候， 名字随便写， 因为每一个模块的默认接口就一个：
```javascript
//lib.js
let fn = () => "string";
export {fn as default};

//main.js
import defaultFn from "./lib";
console.log(defaultFn());
```

第六种导出方式：
使用通配符*  ,重新导出其他模块的接口
```javascript
//lib.js
export * from "./other";
//如果只想导出部分接口， 只要把接口名字列出来
//export {foo,fnFoo} from "./other";

//other.js
export let foo = "stringFoo", fnFoo = function() {console.log("fnFoo")};

//main.js
import {foo, fnFoo} from "./lib";
console.log(foo);
console.log(fnFoo());
```