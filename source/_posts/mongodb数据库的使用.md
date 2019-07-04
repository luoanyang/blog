---
title: mongodb数据库的使用
date: 2017-07-01 20:41:09
tags: mongodb
categories: 后端
---
学习了nodejs，肯定要学习mongodb数据库的。mongodb 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。
<!--more--> 
## 命令行
### 开机命令
最好用系统自带的cmd运行，如果用git bsah运行会显示不一样。
```bash
$ mongod --dbpath c:\mongo	
```
**--dbpath**就是选择数据库文档所在的文件夹，也就是说mongodb中，真有物理文件，对应一个个数据库。一定要保持，**开机这个cmd不能关闭，不能ctrl+c。**一旦这个cmd出现了问题，数据库就自动关闭了。
所以要另外开启一个cmd，运行以下命令：
### 常用命令
1.**输入下面命令进去mongo**
```bash
$ mongo 
```
2.**列出所有数据库**
```bash
$ show dbs	
```
3.**选择数据库**，如果use不存在的数据库，它会自己新建。
```bash
$ use 数据库名	
```
4.**查看当前所在的数据库**
```bash
$ db
```
5.**显示存在的集合**
```bash
$ show collections
```
6.**删除数据库**，删除的是当前所在的数据库
```bash
$ db.dropDatebase();
```
7.**导入数据库**,其中**test**为要导入数据库的名称。**student**为要导入集合的名称。**--drop**表示清空这个student这个集合里的数据再导入数据，不写drop表示不清空。 data.json为导入的数据库所在地址和名称(注：导入的文件格式为json格式)
```bash
$ mongoimport --db test --collection student  --drop --file data.json
```

### 插入数据
student就是集合，集合中存在很多json。这个就是向student集合插入json数据。如果db.一个不存在的集合，它就会自动新建这个集合。
```bash
$ db.student.insert({"name":"xiaoming","age":"12"});	//
```

### 查找数据
find()里面添加 **[查询条件的参数](http://www.runoob.com/mongodb/mongodb-query.html)** 规定要查询的数据。
```bash
$ db.student.find();	//不填条件参数默认查询所有
$ db.student.find({"score.shuxue":70});		//精确查询
$ db.student.find({"score.yuwen":{$gt:50}});	//大于的条件，$gt表示大于
$ db.student.find({"score.yuwen":{$lt:50}});	//小于的条件，$lt表示大于
$ db.student.find({$or:[{"score.shuxue":80},{"age":9}]});	//或条件，用“$or:[{条件参数},{条件参数}]”
$ db.student.find({"score.shuxue":80,"age":9});	//与条件，用逗号隔开
$ db.student.find().sort({"score.shuxue":1});	//升降排序，1为正序，-1为倒序
```

### 修改数据
1.查找名字叫做小明，把他年龄改成16岁。
```bash
$ db.student.update({ "name" : "xiaoming" },{$set: { "age": "16" }})
```
2.查找数学成绩70，把年龄改成33岁。
```bash
$ db.student.update({ "score.shuxue" : 70 },{$set: { "age": "33" }})
```
2.所有人，把年龄改成33岁。默认修改只修改一个，所以要加 “ multi: true ”，修改所有
```bash
$ db.student.update({},{$set: { "age": "33" }},{ multi: true})
```
### 删除数据
删除数据默认删除全部，加“justOne: true”表示只删除一次。

1.删除数学等于80分的。
```bash
$ db.student.remove({ "score.shuxue": 80 })
```

2.只删除一个数学等于80分的。
```bash
$ db.student.remove({ "score.shuxue": 80 }, { justOne: true })
```

2.将集合中的文档删除，但不删除集合本身，也不删除集合的索引。
```bash
$ db.student.remove({})
```

3.删除集合的文档，也会删除集合本身，同时也会删除在集合上创建的索引。
```bash
$ db.student.drop()
```

## mongoVue(图形化)
mongoVue是mongo图形化的一个工具。
### 遇到的问题
1.MongoDB3.2版本之后发现MongoVE连上MongoDB 却不能通过它查看数据的问题。百度搜到了 **[这篇文章](https://my.oschina.net/chiyong/blog/599326)** 解决了这个问题。
原因是因为Mongodb 3.0支持用户自定义存储引擎，用户可配置使用mmapv1或者wiredTiger存储引擎。3.2版本以后默认的开启的是wiredTiger存储引擎，mongoVue读取的是mmapv1存储引擎。对了记得删除已经生成的文件：
```bash
$ mongod  --storageEngine mmapv1 --dbpath 数据目录
```
运行这个命令行，再次运行mongoVue就能读取到mongodb的数据了。