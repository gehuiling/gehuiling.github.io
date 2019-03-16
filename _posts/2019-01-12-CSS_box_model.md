---
layout: post
title:  "CSS知识点（一）盒模型"
date:   2019-01-12 11:41:10
categories: [CSS知识点]
tags: [CSS,盒模型]
comments: true
---

盒子模型是CSS中的核心基础概念。理解了盒子模型和其中每个元素的用法，才能熟练使用CSS的定位方法和技巧，进行页面布局。下面是自己积累和总结的关于CSS盒子模型的知识。
<!--more-->
## 盒模型

盒模型，即box model，所有的页面元素都可以看成是一个盒子，占据一定的页面空间。我们可以调整盒子的边框和距离，来调整盒子（页面和页面中的元素）的位置。

以下图的头像图片为例：

<img src="/image/posts/blog4author1.png" style="display:block;margin:0 auto;">

在chrome浏览器中查看这个元素，结果是这样的：

<img src="/image/posts/blog4author2.png" style="display:block;margin:0 auto;">

蓝色部分（1920 x 162）是content（内容），绿色是padding（内边距），黄色是border（边框）, 深黄色是margin（外边距）。

对的，盒子模型很简单，其实就这个4个概念。

CSS盒模型本质上是一个盒子封装HTML元素，它包括边距、边框、填充和实际内容。盒模型分为两种：标准盒模型和怪异盒模型两种。

* **标准盒模型**：W3C标准盒子模型，指定CSS元素的height与width，设置的是内容区域的高度与宽度
	* margin(外边距)：清除边框外的区域，外边距是透明的
	* border(边框)：围绕在内边距和内容外的边框
	* padding(内边距)：清除内容周围的区域，内边距是透明的
	* content(内容)：盒子的内容，显示文本和图像

<img src="/image/posts/blog4w3cbox.png" style="display:block;margin:0 auto;">

图中最内部的框是元素的实际内容，也就是元素框，紧挨着元素框外部的是内边距（padding），其次是边框（border），然后最外层是外边距（margin），整个构成了框模型。通常我们设置的背景显示区域，就是内容、内边距、边框这一块范围。而外边距 margin是透明的，不会遮挡周边的其他元素。

标准盒模型中：CSS中的宽（width）=内容（content）的宽，CSS中的高（height）=内容（content）的高

`元素框的总宽度 = width + padding（左右） + border（左右） + margin（左右边距）`

`元素框的总高度 = height + padding（上下）+ border（上下） + margin（上下）`

举个例子：
```  
<div style="width:50px;height:50px;padding:2px;border:1px solid blue;margin:3px;">W3C模型</div>
```  

则此div的实际大小：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div高=height+（padding+border+margin）×2=50+(2+1+3)×2=62px.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div宽=width+（padding+border+margin）×2=50+(2+1+3)×2=62px.

div内容占大小：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div高=height=50px.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div宽=width=50px.


* **怪异盒模型**：IE的盒子模型，指定CSS元素的height与width，设置的是内容区域+内边距的高度与宽度，即width已经包含了padding和border值）

<img src="/image/posts/blog4iebox.png" style="display:block;margin:0 auto;">

IE模型中：CSS中的宽（width）=内容（content）的宽 + border（左右） + padding（左右），
　　　　　CSS中的高（height）=内容（content）的高 + border（上下） + padding（上下）

`元素框的总宽度 = width + margin（左右）`

`元素框的总高度 = height + margin（上下）`

*注：background设置的是content+padding的背景色。*

举个例子：
```  
<div style="width:50px;height:50px;padding:2px;border:1px solid blue;margin:3px;">W3C模型</div>
```

则此div的实际大小:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div高 = height + margin×2 =50+3×2=56px.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div宽 = width + margin×2 =50+3×2=56px.

div内容占大小：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div高=height-（border+padding）×2=50-（1+2）×2=44px.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div宽=width-（border+padding）×2=50-（1+2）×2=44px.

## 如何选择盒模型

CSS盒模型和IE盒模型的区别：

* **标准盒模型**中，width 和 height 指的是内容区域的宽度和高度。增加内边距、边框和外边距不会影响内容区域的尺寸，但是会增加元素框的总尺寸。

* **IE盒模型**中，width 和 height 指的是内容区域 + border + padding 的宽度和高度。

为了使页面在不同浏览器下呈现相同的效果，必须统一盒子模型，因为设置width或者height一般是必须用到的。在代码顶部加如下的 doctype 声明：`<!doctype html public "-//w3c//dtd xhtml 1.0 transitional//en" "http://www.w3.org/tr/xhtml1/dtd/xhtml1-transitional.dtd">`，使页面以W3C盒子模型渲染。

只要在文档首部加了doctype声明，就使用了标准盒模型；若不加，则会由浏览器自己决定。比如，IE浏览器中显示怪异盒模型，在 Firefox 浏览器中显示标准 w3c 盒模型。


按理说，我们应该遵循w3c标准使用标准盒模型，但情况并不都是如此，看下面的例子。

## CSS3指定盒子模型种类
``` 
box-sizing: content-box; /* 标准盒模型 */ 
box-sizing: border-box;  /* IE盒模型 */
``` 
box-sizing属性可以指定盒子模型种类，content-box指定盒子模型为标准W3C盒模型，后者为IE盒模型。这是CSS3属性，IE8+和其他现代浏览器支持。