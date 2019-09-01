---
layout: post
title:  "ECMAScript面向对象编程（二）继承(ES6之前)"
date:   2019-03-20 18:05:12
categories: [JavaScript]
tags: [JavaScript]
comments: true
---
es6之前，ECMAScript本质上不能算是一门面向对象的语言。它引入了原型（prototype），以另一种方式模仿类，并且通过原型链的方式实现父类与子类之间共享属性的继承。

<!--more-->

**原型链：**每个构造函数都有一个原型对象，原型对象都有一个指向构造函数的指针，而实例中都包含一个指向原型对象的指针。假如让原型对象A等于另一个类型B的实例，那么原型对象A就会包含一个指向B的原型对象的指针，而该B的原型对象本来就包含指向其构造函数的指针。假如B的原型对象又是另一个类型的实例，那么此时上述关系仍然成立。照这样层层递进，就构成了实例与原型的链条，也就是原型链。

## 原型链的继承

本质是将父类的一个实例赋值给子类的原型对象

```javascript
function SuperType(){
    this.pro = true;
}
SuperType.prototype.getSuperValue = function(){
    return this.pro;
};

function SubType(){
    this.subpro = false;
}

SubType.prototype = new SuperType();  // 父类的实例赋值给子类的原型，子类继承了父类

SubType.prototype.getSubValue = function(){
    return this.subpro;
};

var subinstance = new SubType();
console.log(subinstance.getSubValue());  // false
```
<img src="/image/posts/blog81.png" style="display:block;margin:0 auto;">

给SubType换成了SuperType类型的一个实例，本质上是重写原型。所以，新原型具有作为SuperType实例的所有属性和方法，并且内部还有一个指针指向SuperType.prototype。最终是这样的：subinstance指向SubType.prototype，SubType.prototype指向SuperType.prototype.getSuperValue（）方法仍然还在SuperType.prototype中，但pro属性则位于SubType.prototype中，因为pro是一个实例属性，getSuperValue（）是原型方法。既然SubType.prototype现在是SuperType的一个实例，那么pro就应该位于该实例中了。要注意，subinstance.constructor现在指向的是SuperType，如果很是需要使用到该属性，就要特意指定一下 `subinstance.constructor=SubType`。

**缺点：** 原型链实现继承最主要问题还是前篇博客提及的引用类型值，即**引用类型值的原型属性会被所有实例所共享**，这也是为什么不在原型中而是在构造函数中定义属性的原因。在通过原型链继承时候，原型实际上就是变成了另一个类型的实例，原先的实例属性（也就是在父类的构造函数中定义的属性）就会成为现在的原型的属性，如果该属性是引用类型值，那就出现前面所说的问题了。

```javascript
function SuperType(){
    this.color = ["red","green"];
}

function SubType(){
}

// 继承了SuperType
SubType.prototype = new SuperType();

var inst1 = new SubType();
inst1.color.push("black");
console.log(inst1.color);

var inst2 = new SubType(); // "red", "green", "black"
console.log(inst2.color);  // "red", "green", "black"
```

上面的例子中，SuperType构造函数定义了一个color属性，包含引用类型的值，SuperType的每个实例都会有各自包含自己的color属性。当SubType通过原型链继承了SuperType之后，SubType.prototype就变成了了SuperType的一个实例，因此它也拥有了自己的color属性，这就跟专门创建了一个SubType.prototype.color属性一样。这会导致什么结果？这相当于在SubType原型中定义了一个引用类型值的属性，好了，又回到前面所说的，原型对象中的引用类型值属性会被所有实例所共享。所以，SubType的所有实例共享一个color属性，那修改了inst1.color，就会直接在inst2上反映出来。

为了解决原型中包含的引用类型值所带来的问题，另一种继承方法被提出。

## 借用构造函数继承

思想很简单：在子类型构造函数内部调用父类型构造函数。

```javascript
function SuperType(name){
    this.name = name;
    this.color = ["red","green"];
}

SuperType.prototype.getName = function(){
    console.log(this.name);
};

function SubType(){
    // 继承了SuperType
    SuperType.call(this,"gee");
    // 实例属性
    this.age = 18;
}

var inst1 = new SubType();
inst1.color.push("black");
console.log(inst1.color);
console.log(inst1.name); // gee

var inst2 = new SubType(); // "red", "green", "black"
console.log(inst2.color);  // "red", "green"

inst1.getName();// 报错
```
相对于原型链，借用构造函数有一个很大的优势，就是可以在子类型构造函数中向超类型构造函数传递参数。

**缺点： 子类型只能继承父类构造函数中声明的实例属性，并不能继承父类原型的属性和方法。**所以上面的例子中实例install就找不到getName方法，所以会报错。为了同时继承父类原型，从而诞生了组合继承的方式。

## 组合继承

```javascript
function SuperType(name){
    this.name = name;
    this.color = ["red","green"];
}

SuperType.prototype.getName = function(){
    console.log(this.name);
};

function SubType(name,age){
    // 继承属性
    SuperType.call(this,name);  // 第二次调用SuperType（）
    // 实例属性
    this.age = age;
}

// 继承方法
SubType.prototype = new SuperType();   // 第一次调用SuperType（）

SubType.prototype.constructor = SubType; // 指定构造函数
SubType.prototype.sayAge = function(){
    console.log(this.age);
};

var inst1 = new SubType("gee1",18);
inst1.color.push("black");
console.log(inst1.color);  // "red", "green", "black"
inst1.getName();   // gee1

var inst2 =new SubType("gee2",17);
console.log(inst2.color);  // "red", "green"
inst2.getName();   // gee2
inst2.sayAge();    // 17
```

不仅会继承构造函数中的属性，也会复制父类原型中的属性。组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为JavaScript中最常用的继承模式。但是它也有不足，组合继承最大的问题就是无论在什么情况下都会调用两次父类型的构造函数。第一次是在创建子类型原型的时候，第二次是在子类型构造函数内部。所以子类型会包含父类型的全部实例属性，我们不得不在调用子类型构造函数时候重写这些属性。简单分析一下：

<img src="/image/posts/blog82.png" style="display:block;margin:0 auto;">

第一次调用SuperType构造函数时，SubType.prototype作为SuperType的实例会得到两个属性：name和color，它们都是SuperType的实例属性。当调用SubType的构造函数时，第二次调用SuperType构造函数，这一次又在新对象上创建了实例属性name和color，这就屏蔽了原型中的同名属性。所以说，组合继承中实际上是相当于覆盖了原型中的属性，这很不优雅。

## 寄生组合方式

这是目前es5中主流的继承方式，开发人员普遍认为这是引用类型最理想的基础方式。其实可以将继承分为两步：构造函数属性继承和建立子类和父类原型的链接。不必为了指定子类型的原型而调用父类型的构造函数，所需要的无非就是父类型的原型的一个副本而已，那么就可以创建父类型原型的副本：

```javascript
function SuperType(name){
    this.name = name;
    this.color = ["red","green"];
}

SuperType.prototype.getName = function(){
    console.log(this.name);
};

function SubType(name,age){
    // 继承属性
    SuperType.call(this,name);
    // 实例属性
    this.age = age;
}

// 将父类的原型复制给了子类原型
SubType.prototype = Object.create(SuperType.prototype);

SubType.prototype.sayAge = function(){
    console.log(this.age);
};

var inst1 = new SubType("gee1",18);
inst1.color.push("black");
console.log(inst1.color);  // "red", "green", "black"
inst1.getName();   // gee1

var inst2 =new SubType("gee2",17);
console.log(inst2.color);  // "red", "green"
inst2.getName();   // gee2
inst2.sayAge();    // 17

console.log(SubType.prototype.constructor); // SuperType
```

这里用到了`Object.creat(obj)`方法，该方法会对传入的obj对象进行`浅拷贝`。**`和组合继承的主要区别就是：将父类的原型复制给了子类原型`**。高效性体现在只调用了一下SuperType构造函数，并且避免了在SubType.prototype中创建不必要的、多余的属性。这段代码做了两件事情：
* 构造函数中继承父类实例属性，并初始化父类。
* 子类原型和父类原型建立联系，继承父类原型中的属性。