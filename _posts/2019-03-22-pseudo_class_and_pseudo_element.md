---
layout: post
title:  "👩🏻‍💻 伪类和伪元素"
date:   2019-03-22 09:33:00
categories: [CSS]
tags: [CSS,伪类,伪元素]
comments: true
---

CSS1和CSS2对伪类和伪元素的却别比较模糊，二者在语法上是一样的，都是以`:`开头，导致经常将伪元素`:before`，`after`误认为是伪类。CSS3对两个概念做了清晰的区分，语法上也很明显地将二者区别开来。
<!--more-->

## 伪类 pseudo-classe

>"The pseudo-class concept is introduced to permit selection based on information that lies outside of the document tree or that cannot be expressed using the other simple selectors."

* 伪类存在的意义是为了选择DOM树之外的信息以及那些不能被常规选择器获取到的信息，前者包含那些匹配指定状态的元素，如`:visited`，`:hover`，`:active`等，后者包含那些满足一定逻辑条件的DOM树中的元素，如`:first-child`，`:first-of-type`，`:target`；
* 伪类由一个冒号`:`开头，冒号后面是伪类的名称和包含在圆括号中的可选参数；

*伪类一览表*：

<!-- <img src="/image/posts/blog101.png" style="display:block;margin:0 auto;"> -->

| Pseudo Class  | Meaning             | CSS      |
|:-------------|:------------------- |:--------:|
|   :active     | 选择正在被激活的元素  |     1    |
|   :hover	    | 选择被鼠标悬浮着元素	|     1	   |
|   :link	    | 选择未被访问的元素	|     1    |
|   :visited	|  选择已被访问的元素	|     1    |
|:first-child	| 选择满足是其父元素的第一个子元素的元素| 2 |
|:lang	        |	选择带有指定 lang 属性的元素|	2 |
|:focus	        |	选择拥有键盘输入焦点的元素|	2|
|:enable	    |	选择每个已启动的元素|	3|
|:disable	    |	选择每个已禁止的元素|	3|
|:checked	    |	选择每个被选中的元素|	3|
|:target	    |	选择当前的锚点元素|	3|
|:first-of-type	|	选择满足是其父元素的第一个某类型子元素的元素|	3|
|:last-of-type	|	选择满足是其父元素的最后一个某类型子元素的元素|	3|
|:only-of-type	|	选择满足是其父元素的唯一一个某类型子元素的元素|	3|
|:nth-of-type(n)|	选择满足是其父元素的第n个某类型子元素的元素|	3|
|:nth-last-of-type(n)|	选择满足是其父元素的倒数第n个某类型的元素|	3|
|:only-child	|	选择满足是其父元素的唯一一个子元素的元素|	3|
|:last-child	|	选择满足是其父元素的最后一个元素的元素|	3|
|:nth-child(n)	|	选择满足是其父元素的第n个子元素的元素|	3|
|:nth-last-child(n)	|	选择满足是其父元素的倒数第n个子元素的元素|	3|
|:empty	|	选择满足没有子元素的元素|	3|
|:in-range	|	选择满足值在指定范围内的元素|	3|
|:out-of-range	|	选择值不在指定范围内的元素|	3|
|:invalid	|	选择满足值为无效值的元素|	3|
|:valid	|	选择满足值为有效值的元素|	3|
|:not(selector)	|	选择不满足selector的元素|	3|
|:optional	|	选择为可选项的表单元素，即没有“required”属性|	3|
|:read-only	|	选择有"readonly"的表单元素|	3|
|:read-write	|	选择没有"readonly"的表单元素|	3|
|:root	|	选择根元素|	3|

## 伪元素 pseudo-element

>"Pseudo-elements create abstractions about the document tree beyond those specified by the document language."

* 伪元素为DOM树没有定义的虚拟元素,一些伪元素可以使开发者获取到不存在于源文档中的内容（比如常见的::before,::after）。
* 不同于其他选择器，它不以元素为最小选择单元，它选择的是元素指定内容。
* 伪元素在DOM树中创建了一些抽象元素，这些抽象元素是不存在于文档语言里的（可以理解为html源码）。比如：documen接口不提供访问元素内容的第一个字或者第一行的机制，而伪元素可以使开发者可以提取到这些信息。
* 伪元素由两个冒号`::`开头，使用两个冒号::是为了区别伪类和伪元素（CSS2中并没有区别）。考虑到兼容性，CSS2中已存在的伪元素仍然可以使用一个冒号:的语法，但是CSS3中新增的伪元素必须使用两个冒号::。

*伪元素一览表*：

| Pseudo Element  | Meaning             | CSS      |
|:-------------|:----------------------|:--------:|
|::first-letter	|选择指定元素的第一个单词 |	     1   |
|::first-line	|选择指定元素的第一行     |  	 1   |
|::after	    |在指定元素的内容前面插入内容|	  2   |
|::before	    |在指定元素的内容后面插入内容|	  2   |
|::selection	|选择指定元素中被用户选中的内容|  3   |


