---
layout: post
title:  "👼🏻 ECMAScript面向对象编程（一）创建对象(ES6之前)"
date:   2019-02-17 21:15:02
categories: [JavaScript]
tags: [JavaScript]
comments: true
---

面向对象（Object-Oriented，OO）的语言有一个标志，那就是都有类的概念，通过类可以创建任意多个具有相同属性和方法的对象。但是ECMAScript中并没有类的概念，因此它的对象与基于类的语言中所说的对象不同。ECMAScript中的对象其实就是一组没有特定顺序的名值对，其中值可以是数据或者函数。
<!--more-->

如何创建对象？

## Object构造函数

```javascript
var person = new Object();
person.name = "gee";
person.age = 18;
person.job = "student";

person.sayName = function(){
    console.log(this.name);
};
```

## 对象字面量
```javascript
var person = {
    name: "gee",
    age: 18,
    job:  "student",
    sayName: function(){
        console.log(this.name);
    }
};
```

## 工厂模式

虽然 Object 构造函数或对象字面量都可以用来创建单个对象，它们有个明显的缺点：使用同一个接口创建很多对象，会产生大量重复的代码。

工厂模式是软件工程领域的一种设计模式，这种模式抽象了创建具体对象的过程。考虑到ECMAScript中无法创建类，开发人员就发明了一种函数，用函数来封装以特定接口创建对象的细节。
```javascript
function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        console.log(this.name);
    }
    return o;
}

var person1 = createPerson("aaa",17,"student");
var person2 = createPerson("bbb",18,"teacher");
```

函数 `createPerson（）` 能够根据接收的参数来构建一个包含所有必要信息的Person对象，可以无数次调用这个函数，每次它都会返回一个包含三个属性和一个方法的对象。

工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎样知道一个对象的类型）。随着JavaScript的发展，新的模式出现了。

## 构造函数模式

像Object、Array这样的原生构造函数在运行时会自动出现在执行环境中。也可以创建自定义的构造函数，然后定义自定义对象类型的属性和方法。

```javascript
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        console.log(this.name);
    }
}

var person1 = new Person("aaa",17,"student");
var person2 = new Person("bbb",18,"teacher");
```

**创建自定义的构造函数意味着将来可以将它的实例标识为一种特殊的类型，这正是构造函数模式优于工厂模式的所在。**

Person()函数取代了createPerson()函数，Person()中的代码除了与createPerson()中相同的部分之外，有以下不同：
* 没有显式地创建对象；
* 直接将属性和方法赋给了 `this` 对象；
* 没有 `return` 语句。

此外，函数名Person首字母大写，按照惯例构造函数都应该以一个大写字母开头。构造函数也是函数，与普通函数的唯一区别就是调用的方式不同。任何函数，只要通过 `new` 操作符来调用，那它就可以作为构造函数；否则就跟普通函数没有两样。

要创建 Person 的新实例，必须使用 `new` 操作符，这种调用构造函数实际上会经历以下4步：
* 创建一个新对象；
* 将构造函数的作用于赋给新对象（因此 `this` 就指向了这个新对象）；
* 执行构造函数中的代码（为这个新对象添加属性）；
* 返回新对象。

在这里，person1 和 person2 这两个实例对象都有一个 `constructor`（构造函数）属性，指向 Person:

```javascript
console.log(person1.constructor == Person); // true
console.log(person2.constructor == Person); // true
```

对象的 constructor 属性最初是用来标识对象类型的，但是，检测对象类型用 `instanceof` 操作符更加可靠一些。

```javascript
console.log(person1 instanceof Person); // true
console.log(person1 instanceof Object); // true
console.log(person2 instanceof Person); // true
console.log(person2 instanceof Object); // true
```

说明在例子中创建的 person1 、person2 既是Object的实例，又是Person的实例。

**使用构造函数的主要问题：每个方法都要在每个实例上重新创建一次。** 上面的例子可以证明这一点：

```javascript
console.log(person1.sayName == person2.sayName); // false
```
所以说，每个实例中的方法是不同的。创建两个完成同样任务的 function 完全没有必要，而且还有 `this` 对象在，前面的代码其实是在执行代码前就把函数绑定到特定的对象上了。所以，是不是可以向下面这样把函数定义丢到构造函数外面？就像这样：

```javascript
function Person(name,age,job){
    this.name = name;
    this,age = age;
    this.job = job;
    this.sayName = sayName;
}

function sayName(){
    console.log(this.name);
}

var person1 = new Person("aaa",17,"student");
var person2 = new Person("bbb",18,"teacher");
```

上面的代码中，构造函数内部将sayName属性设置成等于全局的sayName函数，所以sayName包含的是一个指向全局函数的指针，因此 person1 和 person2 对象就共享了全局作用域中的同一个sayName函数，解决了两个函数做同样一件事情的问题。但是新的问题产生了：在全局作用域中定义的函数实际上只能被某个对象调用，并且如果有多个方法的话，就需要定义很多的全局函数。这样的话，自定义的引用类型就丝毫没有封装性可言了，于是出现了另一种模式。

## 原型模式

创建的每个函数都有一个 `prototype（原型）` 属性，这个属性是一个指针，指向一个对象，这个对象的作用就是包含可以被所有实例共享的属性和方法。**简单来说，prototype 就是通过调用构造函数而创建的那个实例对象的原型对象**。

不在构造函数定义对象实例的信息，而是将其直接添加到原型对象中：

```javascript
function Person(){
}

Person.prototype.name = "gee";
Person.prototype.age = 18;
Person.prototype.job = "student";
Person.prototype.sayName = function(){
    console.log(this.name);
}

var person1 = new Person();
person1.sayName();  // gee

var person2 = new Person();
person2.sayName();  // gee

console.log(person1.sayName == person2.sayName);  // true
```

#### 什么是原型对象

* 只要创建了一个函数（当然适用于构造函数），就会自动为该函数创建一个 `prototype` 属性，这个属性指向函数的原型对象；
* 所有的原型对象自动获得 `constructor（构造函数）` 属性，这是一个指向prototype属性所在的函数的指针（例如Person.prototype.constructor指向Person）；
* 调用构造函数创建一个新实例后，该实例的内部包含一个指向原型对象的指针 [ [ Prototype ]]。

字面理解就是：**默认情况下，构造函数的prototype属性指向原型对象，原型对象的constructor 属性指向构造函数，实例对象的[ [ Prototype ]]指向原型对象。**

**实例中的指针仅指向原型对象，而不指向构造函数，实例与构造函数没有直接关系**

以Person函数来分析，构造函数Person、Person的原型属性、Person的两个实例对象之间的关系：

<img src="/image/posts/blog71.png" style="display:block;margin:0 auto;">

`Person.prototype 指向原型，Person.prototype.constructor指向Person，实例对象person1和person2都包含一个内部属性，指向Person.prototype。`

可以使用 `isPrototypeOf()` 和 `Object.getPrototypeOf()` 确定实例与原型之间的关系：

```javascript
console.log(Person.prototype.isPrototypeOf(person1));  // true
console.log(Person.prototype.isPrototypeOf(person2));  // true

console.log(Object.getPrototypeOf(person1) == Person.prototype);  // true
console.log(Object.getPrototypeOf(person2) == Person.prototype);  // true

console.log(Object.getPrototypeOf(person1) .name);  // 'gee'
```

每当代码读取某个对象的某个属性时，首先搜索实例对象本身，如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，就继续搜索指针指向的原型对象，在原型对象中找到了具有给定名字的属性，就返回属性的值。可以通过实例访问保存在原型中的值，但是不能通过对象实例重写原型中的值。如果在实例中添加了一个同名属性，那就会在实例中创建该属性，该属性会屏蔽原型中的同名属性。

#### 更简单的原型语法

前面的例子没添加一个属性或方法都要写一遍Person.prototype，更常见的写法是用一个包含所有属性和方法的对象字面量来重写整个原型对象。

```javascript
function Person(){
}

Person.prototype = {
    name: "gee",
    age: 18,
    job: "student",
    sayName: function(){
        console.log(this.name);
    }
};
```

表面看起来好像跟改写前没什么变化，看下面：

```javascript
var person3 = new Person();
console.log(person3.constructor == Person);  // false
```

**实例的constructor属性不再指向Person了**，这是为什么？

**前面已经说过，每创建一个函数，就会同时自动创建它的默认prototype对象，这个原型对象也自动获得指向构造函数本身的constructor属性。采用对象字面量书写原型对象，本质上是完全重写了默认的prototype对象，constructor属性也就变成了新的原型对象的constructor属性（指向Object构造函数），不再指向Person函数。后面再创建对象实例时，它们的prototype不再指向默认的原型对象，而是新的原型对象了。**

如果constructor属性很重要，可以向下面这样特意将其设置回适当的值：

```javascript
function Person(){
}

Person.prototype = {
    constructor:Person,  // 指定constructor属性
    name: "gee",
    age: 18,
    job: "student",
    sayName: function(){
        console.log(this.name);
    }
};
```

#### 原型的动态性


* **给原型对象添加属性/方法**

首先看这段代码：
```javascript
function Person(){
}

Person.prototype.name = "gee";
Person.prototype.age = 18;
Person.prototype.job = "student";
Person.prototype.sayName = function(){
    console.log(this.name);
};

var person3 = new Person();
person3.sayName();  // "gee" （正常输出）
```

如果在为原型对象添加属性之前就创建了实例：

```javascript
function Person(){
}

var person3 = new Person();

Person.prototype.name = "gee";
Person.prototype.age = 18;
Person.prototype.job = "student";
Person.prototype.sayName = function(){
    console.log(this.name);
};

person3.sayName();  // "gee" （正常输出）
```

在原型中查找值的过程是一次搜索，因此对原型做的任何修改都能立即在实例上反映出来——即使是先创建了实例，后修改原型。这归结为实例与原型之间的松散连接关系，在实例中找不到指定的属性，那就在原型中找。实例与原型之间的连接是一个指针而不是一个副本。

这两种情况的内部原理图是一致的：

<img src="/image/posts/blog72.png" style="display:block;margin:0 auto;">

* **重写原型对象**

首先看这段代码：
```javascript
function Person(){
}

Person.prototype = {
    constructor:Person,
    name: "gee",
    age: 18,
    job: "student",
    sayName: function(){
        console.log(this.name);
    }
};

var person3 = new Person();

person3.sayName();  //"gee"(没有问题，正常输出)
```

<img src="/image/posts/blog73.png" style="display:block;margin:0 auto;">

如果在重写原型之前就创建了实例：
```javascript
function Person(){
}

var person3 = new Person();

Person.prototype = {
    constructor:Person,
    name: "gee",
    age: 18,
    job: "student",
    sayName: function(){
        console.log(this.name);
    }
};

person3.sayName();  //"Uncaught TypeError: person3.sayName is not a function"(错误！)
```

<img src="/image/posts/blog74.png" style="display:block;margin:0 auto;">

这里就不一样了。尽管可以随时为原型添加属性和方法，并且修改能够立即在实例中反映出来。但是如果是重写整个原型对象，记住**用对象字面量的形式改写原型对象，实际上是重写了整个原型对象，并且constructor属性不再指向原来的构造函数。**



**调用构造函数创建实例，会为实例添加一个指向最初原型的[ [ prototype] ]指针。重写原型对象的话，原型对象的constructor属性就是新对象的constructor属性，不再指向最初的构造函数，这样就等于切断了最初的构造函数与原型之间的联系。实例中的指针仅指向原型对象，不指向构造函数。**

在这个例子中，先创建Person的一个实例，然后有重写了其原型对象。后面再调用sayName函数就会发生错误，因为person3指向的原型仍然是最初的原型，根本没有这个属性，这个属性是存在于新的原型对象上的。**记住：在创建了实例之后重写原型，就会切断现有实例与新原型之间的联系。**

#### 原型模式存在的问题

* 省略了为构造函数传递初始化参数这一环节，所以所有实例在默认情况下都取得相同的属性值；
* 原型模式最大的问题是由共享的本性导致的，对于共享的引用类型的属性来说，问题就暴露出来了。

```javascript
function Person(){
}

Person.prototype = {
    constructor:Person,
    name: "gee",
    age: 18,
    job: "student",
    friends:["aa","bb"],
    sayName: function(){
        console.log(this.name);
    }
};

var person1 = new Person();
var person2 = new Person();

console.log(person1.friends); // ["aa","bb"]
console.log(person2.friends); // ["aa","bb"]

person1.friends.push("cc");

console.log(person1.friends); // ["aa", "bb", "cc"]
console.log(person2.friends); // ["aa", "bb", "cc"]
```

可以看到，修改person1引用的数组，person2.friends同样发生改变。因为friends数组存在于Person.prototype中而不是person1中。这正是很少看到单独使用原型模式的原因所在，那么另一种模式出现了。

## 组合使用构造模式和原型模式

构造函数用于定义实例属性，原型模式用于定义方法和共享的属性。这种构造方法是ECMAScript中使用最广泛、认同度最高的一种创建自定义类型的方法。

```javascript
function Person(name,age,job){
    this.name=name;
    this.age = age;
    this.job = job;
    this.friends = ["aa","bb"];
}

Person.prototype = {
    constructor:Person,
    sayName: function(){
        console.log(this.name);
    }
};

var person1 = new Person("gee1",17,"student");
var person2 = new Person("gee2",18,"teacher");

person1.friends.push("cc");

console.log(person1.friends); // ["aa", "bb", "cc"]
console.log(person2.friends); // ["aa", "bb"]
```
## 动态原型模式

动态原型模式将构造函数和原型对象等所有信息都封装在构造函数中，在有必要的情况下才初始化原型。也就是说通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

```javascript
function Person(name,age,job){
    this.name=name;
    this.age = age;
    this.job = job;
    if(typeof this.sayName != "function"){
        Person.prototype.sayName = function(){
        console.log(this.name);
    }
}
}

var person1 = new Person("gee",17,"student");
person1.sayName(); // "gee"
```
只有在sayName（）方法不存在的情况下才将它添加到原型中。

**使用动态原型模式时，不能使用对象字面量重写原型，因为在已经创建实例的情况下重写原型，会切断现有实例与新原型之间的联系。**

## 寄生构造模式

基本思想是创建一个函数，该函数的作用仅仅是封装对象的代码，然后返回新创建的对象。表面上看很像经典的构造函数，可又感觉像工厂模式：

```javascript
function Person(name,age,job){
    var o =new Object();
    o.name=name;
    o.age = age;
    o.job = job; 
    o.sayName = function(){
        console.log(this.name);
    }

    return o;
}

var person1 = new Person("gee",17,"student");
person1.sayName(); // "gee"
```

## 稳妥构造函数模式

与计生构造函数模式类似，但是有两点不同：创建对象的实例方法不引用this；不使用new操作符调用构造函数。
```javascript
function Person(name,age,job){
    var o = new Object();

    // 可以在这里定义私有变量和函数

    // 添加方法
    o.sayName = function(){
        console.log(name);
    }

    return o;
}

var person1 = Person("gee",17,"student");
person1.sayName(); // "gee"
```

变量person1保存的是一个稳妥对象，除了调用sayName（）函数外，没有其他的方法可以访问其数据成员。即使有其他代码会给这个对象添加方法或者数据成员，但也不可能有别的方法访问传入到构造函数中的原始数据。稳妥构造函数的这种安全性，使得它非常适合在某些安全执行环境。