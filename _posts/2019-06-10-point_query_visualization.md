---
layout: post
title: "👩🏻‍🔧 轨迹点时空查询与可视化"
date: 2019-06-10 00:00:00
categories: [可视化]
tags: [leaflet,knn]
comments: true
---

基于leaflet的轨迹点聚合渲染，knn邻近查询结果可视化
<img src="/image/posts/blog1703.bmp" style="display:block;margin:0 auto;"> 
<!--more-->

### 轨迹点时空查询、聚合渲染
Ajax请求后台数据接口，根据设定的时间范围，框选（或手动设置）的空间范围，请求到时空范围内的轨迹点；基于leaflet的markercluster插件，实现大数据量轨迹点快速渲染。
<img src="/image/posts/blog1701.gif" style="display:block;margin:0 auto;"> 

### 点的KNN邻近查询
根据设定的KNN邻近查询参数k值，鼠标点击地图任何地方，Ajax请求后台数据接口，返回鼠标单击处的所有邻近轨迹点，直接渲染在地图上
<img src="/image/posts/blog1702.gif" style="display:block;margin:0 auto;"> 

👩🏻‍💻🥱✨🧐**[代码](https://github.com/gehuiling/point-query-visualization)**