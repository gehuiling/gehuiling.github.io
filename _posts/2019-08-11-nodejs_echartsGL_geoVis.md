---
layout: post
title: "🥯 Nodejs_echartsGL_geoVis"
date: 2019-08-11 00:00:00
categories: [Node.js]
tags: [Node.js]
comments: true
---

<img src="/image/posts/blog1902.bmp" style="display:block;margin:0 auto;"> 

<!--more-->

### 使用express搭建服务器

Express是一个基于 Node.js 平台，快速、开放、极简的 web 开发框架

##### 安装 express 
```javascript
yarn add express
```

##### 编写数据接口

使用的数据为武汉市一天的出租车轨迹数据(.txt格式)，有2000多条轨迹，250万个轨迹点。使用express搭建本地服务器，提供客户端渲染的数据接口。

```javascript
const express = require('express');
const fs = require('fs');

var app = express();

app.all('*', function (req, res, next) {             //设置跨域访问
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By", ' 3.2.1');
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

app.get('/wuhan', function (req, res) {           //配置接口api
    res.status(200),
        fs.readFile('./taxidata/wuhan_taxi.txt', function (err, data) {
            res.json(data.toString())
        })
})

//配置服务端口
var server = app.listen(3000, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log('succeed...listen at http://%s:%s', host, port)
})
```

### 客户端渲染

前端利用ajax请求接口获取到轨迹数据，基于 echartGL、mapboxGL，快速、动态渲染大量出租车轨迹
```javascript

$.ajax({
    type: "get",
    url: "http://localhost:3000/wuhan",
    success: function (data) {

        var wuhan_taxi = JSON.parse(data);

        for (var traj of wuhan_taxi) {
            taxiRoutes.push({
                coords: traj,
                lineStyle: {}
            });
        }

        console.log(taxiRoutes);

        myChart.setOption({
            mapbox: {
                center: [114.354965, 30.608193],
                zoom: 7.5, //缩放级别
                altitudeScale: 2, //海拔高度
                style: "mapbox://styles/mapbox/dark-v9", //mapbox地图类型
                // style: 'mapbox://styles/our style URL',
                postEffect: {
                    enable: true,
                    SSAO: {
                        enable: true,
                        radius: 2,
                        intensity: 1.5
                    }
                },
                light: {
                    main: {
                        intensity: 1,
                        shadow: true,
                        shadowQuality: "low"
                    },
                    ambient: {
                        intensity: 0
                    },
                    ambientCubemap: {
                        exposure: 1,
                        diffuseIntensity: 0.5
                    }
                }
            },
            series: [
                {
                  type: "lines3D",
                  coordinateSystem: "mapbox",
                  effect: {
                    show: true,
                    constantSpeed: 2, // 前进线的速度
                    trailWidth: 1.2, // 前进线的宽度
                    trailLength: 0.06, // 前进的线的长度
                    trailOpacity: 1,
                    spotIntensity: 5 // 前进线顶部的亮度
                  },
                  blendMode: "lighter",
                  polyline: true,
                  lineStyle: {
                    width: 0.1, // 原始轨迹线
                    color: "#ff270a",
                    opacity: 0 // 轨迹点的透明度，0为看不到原始轨迹点
                  },
                  data: taxiRoutes
                }
              ]
        });
    },
    error: function (err) {
        console.log(err);
    }
});
```
实验效果：
<img src="/image/posts/blog1901.gif" style="display:block;margin:0 auto;"> 

😀👩🏻‍💻👩🏻‍🔧😅   **[ 源码](https://github.com/gehuiling/nodejs_echartsGL)**
