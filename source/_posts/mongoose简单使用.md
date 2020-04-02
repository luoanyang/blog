---
title: mongoose简单使用
date: 2017-07-27 11:30:13
tags: mongodb
categories: 后端
---
使用了nodejs和mongodb一段时间，现在有接触了mongoose，说下简单的使用。
<!--more--> 
## 准备工作
1.先安装mongoose并引入。
``` bash
$ npm install mongoose
```
## 创建模块
新建student.js文件，内容如下:

``` javascript

var mongoose = require("mongoose");
//连接数据库
var db = mongoose.createConnection("mongodb://127.0.0.1:27017/sutdent");

db.once('open',function(callback){
    console.log("数据库连接成功!");
});

//设置schema
var studentSchema = new mongoose.Schema({
	name : {type:String},
	age : {type:Number},
	sex : {type:String}
})

////----------创建静态方法-----------////

//根据查找的静态方法
studentSchema.statics.findData = function(conditions,callback){
	this.model('student').find(conditions,callback);
})

//创建修改的静态方法
studentSchema.statics.updateData = function(conditions,update,options,callback){
    	this.model('student').update(conditions,update,options,callback);
}

//创建删除的静态方法
studentSchema.statics.deleteData = function(conditions,callback){
	this.model('student').remove(conditions,callback);
})



////----------创建模型-----------////
var studentModel = db.model("student",studentSchema);

//向外暴露模块
module.exports = studentModel;
```

## 使用模块
新建app.js文件，引入student.js文件使用，内容如下:
``` javascript

var studentModel = require("./student.js");

// 增加
studentModel.create({"name":"xiaoming","age":23,"sex":"nv"},function(err){
	if(err){
		console.log("增加失败")
	}
    console.log('增加成功')
})

// 查找
studentModel.findData({"name":"xiaoming"},function(err,result){
    if(err){
		console.log("查找失败")
	}
    console.log(result)
});

// 修改
studentModel.updateData({"name":"xiaoming"},{$set:{"age":"155"}},{},function(err){
    if(err){
		console.log("修改失败")
	}
    console.log("修改成功")
})

// 删除
studentModel.deleteData({"name":"xiaoming"},function(err){
	if(err){
		console.log("删除失败")
	}
    console.log("删除成功");
});

```

## 个人总结
**简单的使用mongoose，感觉结构化比较清晰。**