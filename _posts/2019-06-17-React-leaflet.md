---
layout: post
title: "🤖 React实战Demo：Leaflet地图框架"
date: 2019-06-17 00:00:00
categories: [React]
tags: [react,leaflet]
comments: true
---

基于React框架实现的一个小Demo，主要的技术栈是：`React`、`webpack`、`Leaflet`，该Demo演示了react框架下进行地图数据渲染，主要目的是熟悉webpack打包、react框架基本思想、基本运用方法。


<!--more-->

### 项目结构
本demo是基于webpack打包工具，自己构建的react开发环境，当然也可以直接用create-react-app 脚手架。为了学习webpack，所以自己动手搭了一个，项目的基本结构如下图：

<img src="/image/posts/blog1803.bmp" style="display:block;margin:0 auto;"> 

### 安装

```javascript
yarn add react-leaflet
```

### 引入
在`index.html`中引入需要的样式和脚本依赖：
<img src="/image/posts/blog1802.bmp" style="display:block;margin:0 auto;"> 

### 说明
mapbox提供了丰富的地图资源，本demo就是基于此做的实践。mapbox的访问需要通过一个accessToken，这必须通过去其官网注册，[官网地址：https://www.mapbox.com](https://www.mapbox.com)

### 演示
<img src="/image/posts/blog1801.gif" style="display:block;margin:0 auto;"> 


😀👩🏻‍💻👩🏻‍🔧😅**[  源码](https://github.com/gehuiling/react-leaflet-map-demo)**