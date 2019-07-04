---
title: 使用git向github提交代码
date: 2017-06-01 13:31:07
tags: git
categories: 其他
---
学习git，只学习了一些常用的命令行，因为自己想用git向github代码就没有深入的学习，系统为win10。
<!--more-->
## 准备工作
1.[git下载地址](https://git-for-windows.github.io/)，先下载完成安装。
```bash
//git常用命令行
$ git init	//初始化 git 仓库
$ git status  //查看当前 git 仓库的一些状态
$ git add .  	//跟踪所有改动过的文件
$ git commit -m"备注" //提交所有更新过的文件
$ git branch	//查看下当前分支情况
$ git branch -d		//删除分支
$ git branch -D 	//强制删除分支
$ git clone 	//克隆远程库
$ git pull 	//下载代码及合并
$ git push 	//上传代码及合并
```
**详细命令行附图一张**
![gitbash](/img/git.png)
## 提交流程
1.先向github添加ssh key，参考这个[教程](http://blog.csdn.net/binyao02123202/article/details/20130891)。
2.在git上设置你的用户名和邮箱，运行下面命令行：
```bash
$ git config --global user.name "your name"
$ git config --global user.email "your email"
```
3.在github新建你的仓库。
4.用git命令，克隆你新建的仓库。
```bash
$ git clone git@github.com:luoanyang/git.git   //注意克隆的地址是git开头，不是https开头的
```
5.克隆好了，你就可以修改你克隆的项目的文件了。
6.修改完毕后就在你克隆的文件根目录下鼠标右键运行 “git bash here”，在执行下面命令行：
```bash
$ git add .  
$ git commit -m"备注"
$ git pull origin master
$ git push origin master
```
运行完上面命令行就大功告成了。