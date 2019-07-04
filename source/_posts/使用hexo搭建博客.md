---
title: 使用hexo搭建博客
date: 2017-05-30 23:09:07
tags: hexo
categories: 其他
---
今天从网上搜教程，用hexo搭建了一个博客，这也是我在hexo搭建写的第一篇博客，大概的说下hexo流程，和遇到的问题。
<!--more-->
## 准备工作  
1.先安装git，nodejs，hexo，可以参考[官方的教程](https://hexo.io/zh-cn/docs/index.html)

## 项目初始
只要按照下面命令行逐行输入项目就初始化了。
```bash
$ hexo init <folder>  //新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。
$ cd <folder>   //进入hexo初始的文件夹
$ npm install   //下载依赖
$ hexo generate //生成静态页面（可简写为： hexo g）
$ hexo server   //启动本地服务器，访问地址为http://localhost:4000就可以查看生成页面了
```
## 页面配置
接下来就是页面配置，配置文件为 “_config.yml”,就不全部列举了，以下为常用的配置参数。
```bash
参数	描述
title	网站标题
subtitle	网站副标题
description	网站描述
author	您的名字
language	网站使用的语言
timezone	网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。
```

## 上传到GitHub  
1.现在自己的github上新建一个仓库，名称为:“your_user_name.github.io”,其中“your_user_name”为你的用户名，这为固定写法。  
2.先要打开 “_config.yml” 文件，拉到文件最下面，找到参数 “deploy”，改成：
```bash
deploy:
     type: git
     repo: https://github.com/your_user_name/your_user_name.github.io.git
     branch: master
```
然后执行命令：
```bash
$ npm install hexo-deployer-git --save  //这步必须要执行
$ hexo deploy  //将生成的静态文件部署到github上，可以简写（hexo d）
```
然后这样就大功告成。
**注意当你把hexo部署的文件夹改名时，要执行一下命令才能再次运行（hexo g，hexo d，hexo server等命令）。**
```bash
$ npm install
$ npm install hexo-deployer-git --save
```
## 博客主题
1.主题我就直接推荐[next](http://theme-next.iissnan.com/getting-started.html)
2.使用方法我就不说了，建议看官方文档更详细。