---
title: sass的使用
date: 2017-06-05 14:43:51
tags: sass
categories: 前端
---
学习教程参考的是[阮一峰老师写的教程](http://www.ruanyifeng.com/blog/2012/06/sass.html),很久之前都知道了，这个css预编译器，但是都没有去使用过，这次刚好有个简单的项目就上手试试，虽然只是一次简单的使用，但感觉真的用的挺不错的,让我觉得以后写样式都可以用sass写了，感觉更方便。
<!--more-->
[demo1](https://luoanyang.github.io/sassDemo/demo1/)
[demo2](https://luoanyang.github.io/sassDemo/demo2/)
## 准备工作
1.先安装sass，sass是ruby语言写的。所以必须先安装ruby，然后再安装sass。当安装好了ruby，在命令行输入以下命令:
```bash
$ gem install sass
```
然后就可以使用了。

## 使用设置
1.sass提供四个编译风格的选项：
```bash  
* nested：嵌套缩进的css代码，它是默认值。
* expanded：没有缩进的、扩展的css代码。
* compact：简洁格式的css代码。
* compressed：压缩后的css代码。
```
生产环境当中，一般使用最后一个选项,使用命令行如下：
```bash
$ sass --style compressed test.sass test.css
```
2.sass监听某个文件或目录，一旦源文件有变动，就自动生成编译后的版本。
```bash
$ sass --watch input.scss:output.css //监控文件
$ sass --watch app/sass:public/stylesheets // 监控文件夹
```
**其中监听文件改动，后面编译完成后的文件的地址为 “:” 后的地址，这个地址是相对于你当前编译的文件地址**
## 常用语法
1.变量:sass允许使用变量，所有变量以$开头。
```sass
$blue : #1875e7;　
div {
　　color : $blue;
}
```
2.计算功能:sass允许在代码中使用算式
```sass
body {
　　margin: (14px/2);
　　top: 50px + 100px;
　　right: $var * 10%;
}
```
3.嵌套:sas允许选择器嵌套
```sass
div h1{
	color:red;
}
//可以写成
div{
	h1{
		color:red;
	}
}
//属性也可以嵌套，比如border-color属性，可以写成
p {
　　border: {
　　　　color: red;
　　}
}
//在嵌套的代码块内，可以使用&引用父元素。比如a:hover伪类，可以写成
a {
　　&:hover { color: #ffb3ff; }
}
```
4.注释:sass共有两种注释风格。标准的CSS注释 /* comment */ ，会保留到编译后的文件。
单行注释 // comment，只保留在SASS源文件中，编译后被省略。
在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。
5.继承:sass允许一个选择器，继承另一个选择器。比如，现有class1：
```sass
.class1 {
　　border: 1px solid #ddd;
}
//class2要继承class1，就要使用@extend命令
.class2 {
　　@extend .class1;
　　font-size:120%;
}
```
**常用的就这些，还想学习深入的可以参考[阮一峰老师写的教程](http://www.ruanyifeng.com/blog/2012/06/sass.html)**