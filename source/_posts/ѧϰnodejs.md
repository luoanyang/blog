---
title: 学习nodejs
date: 2017-06-20 15:54:11
tags: node.js
categories: 后端
---
因为自己是web前端开发，总是要用到node、npm，所以自己就想着先把nodejs学了，到时候自己在深入的学习mvvm框架比较好理解。个人理解现阶段的前端和后端有很多类似的地方，先学nodejs对自己的好处很大。
<!--more-->
## Nodejs特点
1.单线程
2.非阻塞I/O
3.事件驱动
## Nodejs常用的原生模块
[nodejs官方API](http://nodejs.cn/api/)
### [HTTP模块](http://nodejs.cn/api/http.html)
要使用 HTTP 服务器与客户端
```javascript
//引用http模块，开启一个服务器
var http = require("http");

//创建一个服务器，回调函数表示接收到请求之后做的事情
var server = http.createServer(function(req,res){
	//req参数表示请求，res表示响应
	console.log("服务器接收到了请求" + req.url);
	//设置一个响应头
	res.writeHead(200,{"Content-Type":"text/plain;charset=UTF8"});
	res.end();
10});
//监听端口
server.listen(3000,"127.0.0.1");
```
### [FS模块](http://nodejs.cn/api/fs.html)
文件 I/O 是由简单封装的标准 POSIX 函数提供
```javascript
//引用fs模块
var fs = require("fs");
//使用fs模块的readFile读取一个文件
fs.readFile('文件名',function(err,data){
	if(err){
		return err;
	}
	//因为data是读取文件的内容，但是格式为buffer，可以用toString转化为字符串格式。
	var string1 = data.toString();
});
```
### [URL模块](http://nodejs.cn/api/url.html)
解析url对象

### [PATH模块](http://nodejs.cn/api/path.html)
模块提供了一些工具函数，用于处理文件与目录的路径。

## [NPM](https://www.npmjs.com/)
模块就是一些功能的封装，所以一些成熟的、经常使用的功能，都有人封装成为了模块。并且放到了NPM社区中，供人免费下载。
```bash
$ npm install 模块名字  //下载模块
```
### package.json
1.我们的依赖包，可能在随时更新，我们永远想保持更新，或者某持某一个版本；
2.项目越来越大的时候，给别人看的时候，没有必要再次共享我们引用的第三方模块。
3.每一个模块文件夹中，推荐都写一个package.json文件，这个文件的名字不能改。node将自动读取里面的配置。有一个main项，就是入口文件：
```
{
  "name": "name",
  "version": "1.0.1",
  "main" : "app.js"//入口文件
}
```
```bash
$ npm install  //将能安装“package.json”中配置的所有依赖
```
### exports
Node.js中，一个JavaScript文件中定义的变量、函数，都只在这个文件内部有效。当需要从此JS文件外部引用这些变量、函数时，必须使用exports对象进行暴露。使用者要用require()命令引用这个JS文件。
foo.js文件中的代码：
```
var msg = "你好";
exports.msg = msg;
```
msg这个变量，是一个js文件内部才有作用域的变量。
如果别人想用这个变量，那么就要用**exports**进行暴露。

使用者：
```
var foo = require("./test/foo.js");
console.log(foo.msg);
```
### module.exports
可以将一个JavaScript文件中，描述一个类。用
```
module.exports = 构造函数名;
```
这个方式向外暴露一个类。
## 项目实战

### [demo1](https://github.com/luoanyang/nodeReptile)
这个项目是我用了express、cheerio、superagent-charset模块，node实现一个简单的爬虫,爬取腾讯新闻，并部署到 ‘heroku’。
[效果地址](https://info-api.herokuapp.com/)

### [demo2](https://github.com/luoanyang/nodeStudent)
这个项目是我用了ejs、formidable、silly-datetime模块和原生模块实现了一个比较简陋的相册功能，实现了显示，上传功能。对fs模块和路由，http模块有了进一步的了解。
![实例](/img/nodejs_demo_02.gif)

### [demo3](https://github.com/luoanyang/littleAblum-nodejs)
这个项目是使用express、ejs、silly-datetime、body-parser、multer模块搭建的比上个版本好点的小相册，实现了显示，上传，新增相册功能
![实例](/img/nodejs_demo_03.gif)
