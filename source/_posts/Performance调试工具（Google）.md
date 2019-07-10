---
title: Performance调试工具（Google）
date: 2019-07-06 17:02:29
tags: 前端性能优化
categories: 前端
---
这个工具面板来记录和分析网页运行过程中的所有活动行为信息。你可以充分利用这个面板来分析你的网页的程序性能问题。

## 动画记录区域
<img src="https://s2.ax1x.com/2019/07/06/Z0kvKe.png" width="100%">
上图解析

1. **FPS** 每秒帧数(Frames Per Second)。绿色柱状条越高，则每秒帧数越高。在FPS图表上方的红色块是标记一个长帧。
2. **CPU** 标记CPU资源的使用情况，这里的面积图标记着消耗CPU资源的各类事件。CPU面积图中各颜色的含义：蓝色代表HTML文件；黄色代表脚本文件；紫色代表样式文件；绿色代表媒体文件；灰色代表其它杂项文件。
3. **NET** 各种颜色的柱状条分别显示一种资源。柱状条越长，代表获取这个资源的时间越长。NET图表柱状条两种颜色的含义：较亮的部分表示等待时间（当资源被请求时，直到第一个字节被下载的时间)；较暗的部分表示传输时间(在第一个和最后一个字节被下载之间的时间)。


## 统计报表
<img src="https://s2.ax1x.com/2019/07/06/Z0AiPP.png" width="100%">
其中Summary块的色块分别对应：

1. 蓝色（Loading）  --- 网络通信和HTML解析
2. 黄色（Scripting）--- 执行Javascript
3. 紫色（Rendering）--- 重排
4. 绿色（Painting） --- 重绘

## 事件时长排序
<img src="https://s2.ax1x.com/2019/07/06/Z0EIt1.png" width="100%">

## 时间调用排序
<img src="https://s2.ax1x.com/2019/07/06/Z0VSht.png" width="100%">

## 时间发生先后顺序
<img src="https://s2.ax1x.com/2019/07/06/Z0Vmhq.png" width="100%">


