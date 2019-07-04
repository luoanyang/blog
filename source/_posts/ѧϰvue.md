---
title: 学习vue
date: 2017-06-18 15:37:53
tags: vue.js
categories: 前端
---
[项目地址](https://luoanyang.github.io/vueDemo/cart.html)
这个项目是根据[慕课网的vue教程](http://www.imooc.com/learn/796)写的，所用的技术有vue.js和vue-resource.js开发，自己完善了后面的增加地址和编辑地址的功能，这样一个项目写下来对vue的认识有了大概的了解，知道了实际开发中vue的(v-bind,v-click,v-for,v-if,v-model,filter,methods,mounted...)用法。
<!--more-->
## vue.js的常用指令
### v-if指令
v-if是条件渲染指令，它根据表达式的真假来删除和插入元素，它的基本语法如下：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <div id="app">
            <h1>Hello, Vue.js!</h1>
            <h1 v-if="yes">Yes!</h1>
            <h1 v-if="no">No!</h1>
            <h1 v-if="age >= 25">Age: {{ age }}</h1>
            <h1 v-if="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        
        var vm = new Vue({
            el: '#app',
            data: {
                yes: true,
                no: false,
                age: 28,
                name: 'test'
            }
        })
    </script>
</html>
```
### v-show指令
v-show也是条件渲染指令，和v-if指令不同的是，使用v-show指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <div id="app">
            <h1>Hello, Vue.js!</h1>
            <h1 v-show="yes">Yes!</h1>
            <h1 v-show="no">No!</h1>
            <h1 v-show="age >= 25">Age: {{ age }}</h1>
            <h1 v-show="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        
        var vm = new Vue({
            el: '#app',
            data: {
                yes: true,
                no: false,
                age: 28,
                name: 'keepfool'
            }
        })
    </script>
</html>
```
### v-else指令
可以用v-else指令为v-if或v-show添加一个“else块”。v-else元素必须立即跟在v-if或v-show元素的后面——否则它不能被识别。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <div id="app">
            <h1 v-if="age >= 25">Age: {{ age }}</h1>
            <h1 v-else>Name: {{ name }}</h1>
            <h1>---------------------分割线---------------------</h1>
            <h1 v-show="name.indexOf('keep') >= 0">Name: {{ name }}</h1>
            <h1 v-else>Sex: {{ sex }}</h1>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                age: 28,
                name: 'keepfool',
                sex: 'Male'
            }
        })
    </script>
</html>
```
v-else元素是否渲染在HTML中，取决于前面使用的是v-if还是v-show指令。
这段代码中v-if为true，后面的v-else不会渲染到HTML；v-show为tue，但是后面的v-else仍然渲染到HTML了。

### v-for指令
v-for指令基于一个数组渲染一个列表，它和JavaScript的遍历语法相似：
```html
v-for="item in items"
```
items是一个数组，item是当前被遍历的数组元素。
```html
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <link rel="stylesheet" href="styles/demo.css" />
    </head>
    <body>
        <div id="app">
            <table>
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Age</th>
                        <th>Sex</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="person in people">
                        <td>{{ person.name  }}</td>
                        <td>{{ person.age  }}</td>
                        <td>{{ person.sex  }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                people: [{
                    name: 'Jack',
                    age: 30,
                    sex: 'Male'
                }, {
                    name: 'Bill',
                    age: 26,
                    sex: 'Male'
                }, {
                    name: 'Tracy',
                    age: 22,
                    sex: 'Female'
                }, {
                    name: 'Chris',
                    age: 36,
                    sex: 'Male'
                }]
            }
        })
    </script>
</html>
```
### v-bind指令(可简写：:)
v-bind指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute），例如：v-bind:class
```html
v-bind:argument="expression"
```
下面这段代码构建了一个简单的分页条，v-bind指令作用于元素的class特性上。
这个指令包含一个表达式，表达式的含义是：高亮当前页。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <link rel="stylesheet" href="styles/demo.css" />
    </head>
    <body>
        <div id="app">
            <ul class="pagination">
                <li v-for="n in pageCount">
                    <a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
                </li>
            </ul>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                activeNumber: 1,
                pageCount: 10
            }
        })
    </script>
</html>
```
### v-on指令(可简写：@)
v-on指令用于给监听DOM事件，它的用语法和v-bind是类似的，例如监听<a>元素的点击事件：
```html
<a v-on:click="doSomething">
```
有两种形式调用方法：绑定一个方法（让事件指向方法的引用），或者使用内联语句。
Greet按钮将它的单击事件直接绑定到greet()方法，而Hi按钮则是调用say()方法。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <div id="app">
            <p><input type="text" v-model="message"></p>
            <p>
                <!--click事件直接绑定一个方法-->
                <button v-on:click="greet">Greet</button>
            </p>
            <p>
                <!--click事件使用内联语句-->
                <button v-on:click="say('Hi')">Hi</button>
            </p>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                message: 'Hello, Vue.js!'
            },
            // 在 `methods` 对象中定义方法
            methods: {
                greet: function() {
                    // // 方法内 `this` 指向 vm
                    alert(this.message)
                },
                say: function(msg) {
                    alert(msg)
                }
            }
        })
    </script>
</html>
```
