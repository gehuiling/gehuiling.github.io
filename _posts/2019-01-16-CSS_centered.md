---
layout: post
title:  "🎨 CSS知识点（二）元素居中"
date:   2019-01-16 08:54:12
categories: [CSS]
tags: [CSS,布局]
comments: true
---

利用CSS进行元素的居中有很多实现方法，具体使用哪种方法取决于不同的场景。
<!--more-->

**本文所有演示方法中，当居中元素为块状元素时，共用以下HTML结构代码：**
```html 
<div class="parent"><div class="child"></div></div>
``` 

**共用以下CSS样式：**
```css
.parent {
    background: rgb(203, 245, 154);
    width: 300px;
    height: 300px;
}

.child {
    background:rgb(250, 135, 131);	
}
```

## 水平居中元素

#### 通用方法（元素的宽高未知）

* **CSS3 transform**

```css
.parent {
    position: relative;
}
.child {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
}
```

* **Flex布局**

```css
.parent {
    display: flex;
    justify-content: center;
}
```

适用于子元素为浮动，绝对定位，内联元素，均可水平居中。

<img src="/image/posts/blog52.png" style="display:block;margin:0 auto;">
<!-- ![]({{ site.url }}/image/posts/blog52.png) -->


#### 常规文档流中的内联元素(display: inline)

常见的内联元素有：span, a, img, input, label 等等

```html 
<div class="parent"><span class="child">child-center</span></div>
``` 

设置其父元素的`text-align`属性值为`center`，可实现水平居中：

```css 
.parent {
    text-align:center;
}
``` 

<img src="/image/posts/blog51.png" style="display:block;margin:0 auto;">
<!-- ![]({{ site.url }}/image/posts/blog51.png) -->

#### 常规文档流中的块元素(display: block)

常见的块元素：div, h1~h6, table, p, ul, li 等等

其水平居中可以通过以下两种方式：

 * **设置margin**

 ```css
 .child {
     width: 100px;
     height: 100px;
     margin: 0 auto;
}
 ```
 此方法只能进行定宽元素的水平居中，且对浮动元素或绝对定位元素无效。

* **display：inline-block**

```css
.parent {
    text-align:center;
}
.child {
    display:inline-block;
}
 ``` 
<img src="/image/posts/blog52.png" style="display:block;margin:0 auto;">
<!-- ![]({{ site.url }}/image/posts/blog52.png) -->

#### 绝对定位元素(已知元素宽度)

* **方法一：负margin**

绝对定位的子元素left设为50%，只能将元素的左顶点置于父元素的水平中央。但由于元素自身存在宽度和高度，为了使元素整体在父元素中水平垂直居中，需要对元素自身进行移动。

```css
.parent {
    position: relative; 
} 

.child {
    width: 100px;
    height: 100px;
    position: absolute;
    left:50%;
    margin-left: -50px;
}
``` 

这段代码在本质上做了这样2件事情：

1.先把这个元素的左上角放置在视口(或最近的、具有定位属性的祖先元素)的水平中央；

2.然后再利用负外边距把它向左移动(移动距离相当于它自身宽的一半)。

显然，这个方法最大的局限在于它要求元素的宽是固定的。

* **方法二：margin-0 auto**

```css 
.parent {
    position: relative;
}
.child {
    position: absolute;
    width: 100px;
    height:100px;
    left: 0;
    right: 0;
    margin: 0 auto;
}
```

<img src="/image/posts/blog52.png" style="display:block;margin:0 auto;">
<!-- ![]({{ site.url }}/image/posts/blog52.png) -->

**当然，对于绝对定位且宽度已知/位置的元素，其水平居中还可以使用前面提及的通用方法，也就是使用 transform.**

## 垂直居中元素

#### 通用方法，元素的宽高未知

* **CSS3 transform**

```css
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
```

* **Flex布局**

```css
.parent {
    display: flex;
    align-items: center;
}
```

适用于子元素为浮动，绝对定位，内联元素，均可垂直居中。

<img src="/image/posts/blog53.png" style="display:block;margin:0 auto;">

#### 居中元素为单行文本

```html
<div class="parent"><sapn class="child">center_child</sapn></div>
```

父元素已知高度，把子元素的 line-height 设为 父元素的高度，适用于只有一行文字的情况。

```css
.child {
    line-height: 300px; 	
}

```
<img src="/image/posts/blog54.png" style="display:block;margin:0 auto;">

#### 绝对定位元素(已知元素高度)

与水平居中类似：

* **方法一：负margin**

绝对定位的子元素top设为50%，只能将元素的左顶点置于父元素的垂直中央。但由于元素自身存在宽度和高度，为了使元素整体在父元素中垂直居中，需要对元素自身进行移动。

```css 
.parent {
    position: relative; 
} 

.child {
    width: 100px;
    height: 100px;
    position: absolute;
    top:50%;
    margin-top: -50px;
}
``` 

这段代码在本质上做了这样2件事情：

1.先把这个元素的左上角放置在视口(或最近的、具有定位属性的祖先元素)的垂直中央；

2.然后再利用负外边距把它向上移动(移动距离相当于它自身高的一半)。

显然，这个方法最大的局限在于它要求元素的高度是固定的。

* **方法二：margin-0 auto**

```css 
.parent {
    position: relative;
}
.child {
    width: 100px;
    height: 100px;
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto 0;
}
```

<img src="/image/posts/blog53.png" style="display:block;margin:0 auto;">

## 水平垂直居中

#### 绝对居中块

基本思路: 结合子元素外边距 auto 及四个方向的偏移值为0达到水平垂直居中的目的。

```css
.parent {
    position: relative;
}

.child {
    width: 100px;
    height: 100px;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: auto;
    position: absolute;
}
```

<img src="/image/posts/blog55.png" style="display:block;margin:0 auto;">

* 不仅可以实现在正中间，还可以在正左方，正右方；
* 元素的宽高支持百分比 % 属性值和 min-/max- 属性；
* 可以封装为一个公共类，可做弹出层；
* 浏览器支持性好。

#### 负边距居中

```css
.parent {
    position: relative;
}

.child {
    width: 100px;
    height: 100px;
    position: absolute;
    top:50%;
    left: 50%;
    margin-top: -50px;
    margin-left: -50px;
}
```

<img src="/image/posts/blog55.png" style="display:block;margin:0 auto;">

#### transform定位

```css
.parent {
    position: relative;
}

.child {
    width: 100px;
    height: 100px;
    position: absolute;
    top:50%;
    left: 50%;
    transform:(translate(-50%,-50%)) ;
}
```

#### Flex布局

设置父元素为的 display 为 flex，这是 CSS 布局未来的趋势。Flexbox 是 CSS3 新增属性，设计初衷是为了解决像垂直居中这样的常见布局问题。

```css
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

<img src="/image/posts/blog55.png" style="display:block;margin:0 auto;">

#### table-cell 居中

将父元素的 display 设置为 tabele-cell，居中的子元素设置为 inline-block.

```css
.parent {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}

.child {
    width: 100px;
    height: 100px;
    display: inline-block;
}
```

<img src="/image/posts/blog55.png" style="display:block;margin:0 auto;">

适用于子元素 display 为 inline-block, inline 类型的元素，需要已知父元素的宽高，且父元素的宽高不能设为百分比数。

#### 文本内容居中

```css
.parent {
    text-align: center;
}

.child {
    line-height: 300px;
}
```
<img src="/image/posts/blog56.png" style="display:block;margin:0 auto;">

