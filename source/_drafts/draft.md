---
title: 草稿
tags:
---

## HTTP2多路复用



## 网页渲染过程
1. 获取Dom分割成多层（Parse HTML）
2. 对每一个层计算样式结果（Recalculate Style）
3. 为每个节点生成图形的位置 （重排 Layout）
4. 将每个节点绘制并填充到图层的位图中 （重绘 Paint）
5. 绘制出来的纹理上传到GPU （Composite Layers）
大致流程： Layout、Paint、Composite Layers

## 网页分层
生成网页分层：根元素、position、transform、半透明、滤镜、canvas、video、overflow

GPU参与分层好处，直接跨过重排和重绘
GPU参与分层元素：CSS3D、Video、WebGl、Transform、路径