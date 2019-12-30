---
layout: post
title:  "🤓 CSS3选择器"
date:   2019-04-06 12:45:12
categories: [CSS]
tags: [CSS]
comments: true
---

<img src="/image/posts/blog1301.png" style="display:block;margin:0 auto;"> 
<!--more-->

## 基本选择器

选择器 |  类型  | 功能描述
-------|--------| --------
*   | 通配选择器 | 选择文档中所有的HTML元素
E   | 元素选择器 | 选择指定的类型的HTML元素
#id | ID选择器 | 选择指定ID属性值为“id”的任意类型元素
.class  | 类选择器 | 选择指定class属性值为“class”的任意类型的任意多个元素
selector1,selectorN| 群组选择器 | 将每一个选择器匹配的元素集合并

## 层次选择器

选择器 |  类型  | 功能描述
-------|--------| --------
E   F | 后代选择器 | 选择匹配的F元素，且F被包含在E内（F是E的后代）
E>F  | 子选择器 | 选择匹配的F元素，且F是E的子元素
E+F  | 相邻兄弟选择器 | 选择匹配的F元素，且F是紧位于E后面的、相邻的同级元素
E~F  | 通用兄弟选择器 | 选择匹配的F元素，且F是E的所有同级元素

## 伪类选择器

#### 动态伪类选择器

选择器 |  类型  | 功能描述
-------|--------| --------
E:link | 链接伪类选择器 | 选择匹配的E元素，且匹配元素被定义了超链接且未被访问过。常用于链接锚点上
E:visited | 链接伪类选择器 | 选择匹配的E元素，且匹配元素被定义了超联机并已被访问过。常用于链接锚点上
E:hover | 用户行为伪类选择器 | 选择匹配的E元素，且用户鼠标停留在元素E上（IE6及以下浏览器仅支持a:hover）
E:active | 用户行为伪类选择器 | 选择匹配的E元素，且匹配元素被激活。常用于锚点与按钮上
E:focus | 用户行为伪类选择器 | 选择匹配的E元素，且匹配的元素获得焦点

*锚点伪类的设置必须遵循一个“爱恨原则” LoVe/HAte，也就是`link` → `visited` → `hover` → `active`*

#### 目标伪类选择器

选择器 | 功能描述
-------|--------
E:target | 选择匹配E的所有元素，且匹配元素被相关URL指向

#### 语言伪类选择器

使用语言伪类选择器来匹配使用语言的元素，特别是对于多语言版本的网站，根据不同语言版本设置页面的字体风格。HTML5中有两种方法设置文档的语言：
```html
<!DOCTYPE HTML>
<html lang="en-US">
```
或者
```html
<body lang="fr">
```

选择器 | 功能描述
-------|--------
E:lang(language) | 选择匹配E的所有元素，且匹配元素指定了lang属性，且其值为language

```css
:lang(en) p {background:red}  <!--设置英文（en-US）版本的p段落样式-->
```

#### UI元素状态伪类选择器

选择器 |  类型  | 功能描述
-------|--------| --------
E:checked | 选中伪类选择器 | 匹配选中的复选框按钮或单选按钮表单元素
E:enabled | 启用状态伪类选择器 | 匹配所有启用的表单元素
E:disabled | 不可用状态伪类选择器 | 匹配所有禁用的表单元素

#### 结构伪类选择器

选择器 |   功能描述
-------|--------
E:first-child | 作为父元素的第一个子元素的元素E。与E:nth-child（1）等同
E:last-child | 作为父元素的最后一个子元素的元素E。与E:nth-last-child（1）等同
E:root | 选择匹配元素F所在文档的根元素。在HTML文档中，根元素始终是html，此时该选择器与html类型选择器匹配的内容相同
E  F:nth-child(n) | 选择父元素E的第n个子元素F。其中，n可以是整数（1、2、3）、关键字（even、odd）、公式（2n+1、-n+5），而且n值起始值为1，而不是0
E  F:nth-last-child(n) | 选择父元素E的倒数第n个子元素F。此选择器与 E  F:nth-child(n) 选择器计算顺序刚好相反，但使用方法都是一样的，其中nth-last-child(1)始终匹配的是最后一个元素，与 :last-child 等同
E:nth-of-type(n) | 选择在父元素内是指定类型的第n个E元素
E:nth-last-of-type(n) | 选择在父元素内是具有指定类型的倒数第n个E元素
E:first-of-type(n) | 选择父元素内具有指定类型的第一个E元素，与nth-of-type(1) 等同
E:last-of-type(n) | 选择父元素内具有指定类型的最后一个E元素，与nth-last-of-type(1) 等同
E:only-child | 选择父元素只包含一个子元素，且该子元素匹配E元素
E:only-of-type | 选择父元素只包含一个同类型的子元素，且该子元素匹配E元素
E:empty | 选择没有子元素的元素，而且该元素也不包含任何文本节点

**div p:nth-child(2)必须满足两个条件才能选到p，一：p是div的子元素；二：p刚好处在第二的位置**

***注：:`nth-child ` 和  `nth-of-type` 的区别***

```html
<div class="post">
   <p>para1</p>
   <p>para2</p>
</div>
```

先看 `div>p:nth-child(2){color: red;} ` ，结果是这样的：
<img src="/image/posts/blog1302.png" style="display:block;margin:0 auto;"> 

再看 `div>p:nth-of-type(2){color: red;} ` ，结果是这样的：
<img src="/image/posts/blog1303.png" style="display:block;margin:0 auto;"> 

没错，选择的都是第二个p标签。但是这并不代表两个选择器就是一样的，`div>p:nth-child(2)`有两层意思，首先是一个段落元素，其次这个段落p是父元素div的第二个子元素。`div>p:nth-of-type(2)` 字面上解释是选择父元素div中的第二个段落p。

看下面的对比例子，就可以马上清晰了。把上面的HTML结构改变一下：

```html
<div>
    <h1>h1</h1>
    <p>para1</p>
    <p>para2</p>
</div>
```

先看 `div>p:nth-child(2){color: red;} ` ，结果是这样的：
<img src="/image/posts/blog1304.png" style="display:block;margin:0 auto;"> 

再看 `div>p:nth-of-type(2){color: red;} ` ，结果是这样的：
<img src="/image/posts/blog1305.png" style="display:block;margin:0 auto;">

因为父元素div的第二个子元素还是p标签，所以`div>p:nth-child(2)`有两层的意思，首先是一个段落元素，其次这个段落p是父元素div的第二个子元素。这两个条件均满足，选取到了第一个p标签，修改了其字体为红色。`div>p:nth-of-type(2)` 选择了父元素div中的第二个段落p，修改了其样式。

那如果再改一下HTML结构：

```html
<div>
    <h1>h1</h1>
    <h2>h2</h1>
    <p>para1</p>
    <p>para2</p>
</div>
```

先看 `div>p:nth-child(2){color: red;} ` ，结果是这样的：
<img src="/image/posts/blog1306.png" style="display:block;margin:0 auto;"> 

再看 `div>p:nth-of-type(2){color: red;} ` ，结果是这样的：
<img src="/image/posts/blog1307.png" style="display:block;margin:0 auto;">

此时，父元素div的第二个子元素是h2标签，并不是一个p标签，所以，`div>p:nth-child(2)`的两层意思，首先是一个段落元素，其次这个段落p是父元素div的第二个子元素，第二个条件不满足，故选取不到元素，没有改变任何样式。但是，`div>p:nth-of-type(2)` 仍然是选择父元素div中的第二个段落p，修改其样式。

#### 否定伪类选择器

选择器 |   功能描述
-------|--------
E:not(F) | 匹配所有除元素F外的E元素

常用在表单元素中，如 `input:not([type]=radio) {...} ` 给除了单选按钮之外的表单元素设置样式。

## 伪元素

| 选择器  | 功能描述             | CSS      |
|:-------------|:----------------------|:--------:|
|::first-letter	|选择指定元素的第一个单词 |	     1   |
|::first-line	|选择指定元素的第一行     |  	 1   |
|::after	    |在指定元素的内容前面插入内容|	  2   |
|::before	    |在指定元素的内容后面插入内容|	  2   |
|::selection	|选择指定元素中被用户选中的内容|  3   |

## 属性选择器

选择器 |   功能描述
-------|--------
E[attr] | 选择匹配具有属性attr的E元素。其中E可以忽略，表示选择定义了attr属性的任意类型的元素
E[attr=val] | 选择匹配具有属性attr的E元素。其中E可以忽略，表示选择定义了attr属性的任意类型的元素
E[attr\|=val] | 选择匹配E元素，且E元素定义了属性attr，attr属性值是一个具有val或者以val开始的属性值。常用于lang属性（例如lang=“en-US”）。例如p[lang=en]将匹配定义为英语的任何段落，乌璐呢是英式英语还是美式英语。
E[attr~=val] | 选择匹配E元素，且E元素定义了属性attr，attr属性值具有多个空格分隔的值，其中一个等于val。例如，.info[title~=more]将匹配元素具有类名info，而且这个元素设置了一个属性title，同时title属性值以包含了“more”的任何元素，例如、&lt;a&gt;class="info" title="click here for more information"&lt;/a&gt;
E[attr*=val] | 选择匹配E元素，且E元素定义了属性attr，其属性值任意位置包含了“val”。也就是字符串val与属性值中的任意位置相匹配
E[attr^=val] | 选择匹配E元素，且E元素定义了属性attr，其属性值是以val开头的任何字符串
E[attr$=val] | 选择匹配E元素，且E元素定义了属性attr，其属性值是以val结尾的任何字符串