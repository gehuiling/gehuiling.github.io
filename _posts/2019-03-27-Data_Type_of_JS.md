---
layout: post
title:  "JS数据类型"
date:   2019-03-27 22:08:03
categories: [JavaScript]
tags: [JavaScript]
comments: true
---
基本类型：Number、String、Boolean、Null、Udefined；引用类型：Object、Function、Array、Data等。关于基本类型和引用类型，除了这些，你还要知道的:
<!--more-->

## 在内存中的位置不同

* **基本类型：占用空间固定，保存在栈中**
* **引用类型：占用空间不固定，保存在堆中**

>**栈（stack）**为自动分配的内存空间，它由系统自动释放；使用一级缓存，被调用时通常处于存储空间中，调用后被立即释放。
>
>**堆（heap）**是动态分配的内存，大小不定也不会自动释放。使用二级缓存，生命周期与虚拟机的GC算法有关。

当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，基础变量的值是存储在栈中，而引用变量存储在栈中的是指向堆中的数组或者对象的地址，这就是为何修改引用类型总会影响到其他指向这个地址的引用变量。

当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它。

## 赋值、浅拷贝、深拷贝
* 对于基本类型值，赋值、浅拷贝、深拷贝时都是复制基本类型的值给新的变量，之后两个变量之间的操作不再互相影响。
* 对于引用类型值：
    * **赋值**后两个变量指向同一个地址，一个变量改变时，另一个也同样改变；
    * **浅拷贝**只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存；浅拷贝是拷贝一层，深层次的对象级别的就拷贝引用；
    * **深拷贝**会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象；深拷贝是拷贝多层，每一级别的数据都会拷贝出来。

|  | 和原数据是否指向同一对象| 第一层数据为基本数据类型 |原数据中包含子对象|
|:-----------|:-------------|:------------------- |:--------:|
|    赋值    |   是    | 改变会使原数据一同改变 | 改变会使原数据一同改变  |
|    浅拷贝  |   否    | 改变不会使原数据一同改变 | 改变会使原数据一同改变 |
|    深拷贝  |   否    |  改变不会使原数据一同改变| 改变不会使原数据一同改变 |

```javascript
var obj = {
    name:'gege',
    age:'18',
    friends:['a',['b','c'],['d','e']]
};

var clone1_obj = obj;  // 赋值

var clone2_obj = shllowCopy(obj);
function shllowCopy(o){
    var clone_obj = {};
    for(var prop in o){
        if(o.hasOwnProperty(prop)){
            clone_obj[prop] = o[prop];
        }
    }
    return clone_obj;
}

clone1_obj.name = "lcuky";
clone2_obj.age = 19;

clone1_obj.friends[0] = 'A';
clone1_obj.friends[1] = ['B','C'];
clone2_obj.friends[2] = ['D','E'];

console.log(obj);
// obj = {
//     name:lucky,
//     age:18,
//     friends:['A',['B','C'],['D','E']]
// }
console.log(clone1_obj);
// clone1_obj = {
//     name:lucky,
//     age:18,
//     friends:['A',['B','C'],['D','E']]
// }
console.log(clone2_obj);
// clone2_obj = {
//     name:lucky,
//     age:19,
//     friends:['A',['B','C'],['D','E']]
// }
```

### 浅拷贝

数组常用的浅拷贝方法：`slice`，`concat`，`Array.from()` ，以及`es6的析构`。

```javascript
var arr1 = [1, 2,{a:1,b:2,c:3,d:4}];
var arr2 = arr1.slice();
var arr3 = arr1.concat();
var arr4 = Array.from(arr1);
var arr5 = [...arr1];
arr2[0]=2;
arr2[2].a=2;
arr3[0]=3;
arr3[2].b=3;
arr4[0]=4;
arr4[2].c=4;
arr5[0]=5;
arr5[2].d=5;
// arr1[1,2,{a:2,b:3,c:4,d:5}]
// arr2[2,2,{a:2,b:3,c:4,d:5}]
// arr3[3,2,{a:2,b:3,c:4,d:5}]
// arr4[4,2,{a:2,b:3,c:4,d:5}]
// arr5[5,2,{a:2,b:3,c:4,d:5}]
```
对象常用的浅拷贝方法`Object.assign()`，`es6析构`
```javascript
var obj1 = {
    x: 1, 
    y: {
        m: 1
    }
};
var obj2 = Object.assign({}, obj1);
console.log(obj1) //{x: 1, y: {m: 1}}
console.log(obj2) //{x: 1, y: {m: 1}}
obj2.x=2;
obj2.y.m = 2; 
console.log(obj1) //{x: 1, y: {m: 2}}
console.log(obj2) //{x: 2, y: {m: 2}}
```

### 深拷贝

`JSON.parse(JSON.stringify(obj))`我们一般用来深拷贝，其过程说白了 就是利用JSON.stringify 将js对象序列化（JSON字符串），再使用JSON.parse来反序列化(还原)js对象。

```javascript
var arr = ['old', 1, true, ['old1', 'old2'], {old: 1}]
var new_arr = JSON.parse( JSON.stringify(arr) );
new_arr[4].old=4;
console.log(arr); //['old', 1, true, ['old1', 'old2'], {old: 1}]
console.log(new_arr); //['old', 1, true, ['old1', 'old2'], {old: 4}]
```

但是要注意[“JSON.parse(JSON.stringify(obj))实现深拷贝应该注意的坑”](https://www.imooc.com/article/70653)，在平时的开发中JSON.parse(JSON.stringify(obj))已经满足90%的使用场景了。

**实现一个深拷贝：**
```javascript
var deepCopy = function(obj) {
    if (typeof obj !== 'object') return;
    var newObj = obj instanceof Array ? [] : {};
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key];
        }
    }
    return newObj;
}
```

## 参数的传递

所有的函数参数都是按值传递。也就是说把函数外面的值赋值给函数内部的参数，就和把一个值从一个变量赋值给另一个一样。

* 基本类型
```javascript
var a = 2;
function add(x) {
 return x = x + 2;
}
var result = add(a);
console.log(a, result); // 2 4
```
* 引用类型
```javascript
function setName(obj) {
  obj.name = 'laowang';
  obj = new Object();
  obj.name = 'Tom';
}
var person = new Object();
setName(person);
console.log(person.name); //laowang
```

## 判断方法

基本类型用typeof，引用类型用instanceof。

**特别注意`typeof null:object`, `null instanceof Object:true`**

```javascript
console.log(typeof "abcdef"); // string
console.log(typeof 8);        // number
console.log(typeof true);     // bollean
console.log(typeof undefined); // undefined
console.log(typeof null);      // object

var arr = [1,2,3];
var obj = {};
function f(){};

console.log(arr instanceof Array);   // true
console.log(obj instanceof Object);  // true
console.log(f instanceof Function);  // true
```
## 总结

<img src="/image/posts/blog1201.png" style="display:block;margin:0 auto;"> 



*转自：[面试官问你JavaScript基本类型时他想知道什么？](https://segmentfault.com/a/1190000018745719#articleHeader6)*

*参考：[深拷贝与浅拷贝的区别，实现深拷贝的几种方法](http://www.cnblogs.com/echolun/p/7889848.html)*