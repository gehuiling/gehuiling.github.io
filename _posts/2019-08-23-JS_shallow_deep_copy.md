---
layout: post
title: "🚀 浅拷贝与深拷贝"
date: 2019-08-23 00:00:00
categories: [JavaScript]
tags: [JavaScript]
comments: true

---
|    | **和原数据是否指向同一对象**    | **第一层数据为基本类型**      |  **原数据包含子对象** |
|:-------------|:------------------- |:--------:|:--------:|
|   赋值     | 是  |     改变会使原数据一同改变   | 改变会使原数据一同改变|
|   浅拷贝     | 是 |     改变**不**会使原数据一同改变   | 改变会使原数据一同改变|
|   深拷贝    | 否 |     改变**不**会使原数据一同改变   | 改变**不**会使原数据一同改变|

<!--more-->

### 浅拷贝

以赋值的形式拷贝引用对象，仍指向同一个地址，修改时原对象也会受到影响

##### Object.assign()

```javascript
var obj2 = Object.assgin({}, obj);
```

##### 展开运算符(...)

```javascript
var obj2 = { ...obj };
```

### 深拷贝

完全拷贝一个新对象，修改时原对象不再受到任何影响

##### JSON.parse(JSON.stringfy())

```javascript
var obj2 = JSON.parse(JSON.stringify(obj));
```

_但是必须注意的是在 JSON.stringify()做序列时，undefined、任意的函数以及 symbol 值，在序列化过程中会被忽略。也就是说当源对象中有 undefine、function、symbol 时，在序列化操作的时候会被忽略，导致拷贝生成的对象中没有对应属性及属性值。_

```javascript
var obj = { a: { b: "old" }, c: undefined, d: function() {}, e: Symbol("") };
var newObj = JSON.parse(JSON.stringify(obj));
newObj.a.b = "new";
console.log(obj); // { a: { b: 'old' }, c: undefined, d: [Function: d], e: Symbol() }
console.log(newObj); // { a: { b: 'new' } }
```

##### 递归

```javascript
function deepClone(obj) {
  if (!obj || typeof obj !== "object") {
    return;
  }
  var newObj = obj.constructor === Array ? [] : {};
  for (var key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 存在于实例中的属性
      if (typeof obj[key] === "object" && obj[key]) {
        newObj[key] = deepClone(obj[key]);
      } else {
        newObj[key] = obj[key];
      }
    }
  }
  return newObj;
}

var old = { a: "old", b: { c: "old" } };
var newObj = deepClone(old);
newObj.b.c = "new";
console.log(old); // { a: 'old', b: { c: 'old' } }
console.log(newObj); // { a: 'old', b: { c: 'new' } }
```

_更精细点：_

```javascript
var obj = {
  arr1: [1, 2, 3],
  fn: function() {
    console.log("我是一个方法");
  },
  a: "我是普通属性"
};

// 现在我要把obj字面量创建里的属性深拷贝（ 属性值是引用类型也要深拷贝 ）
function deepClone(obj) {
  // 根据类型制造一个新的数组或对象 => 指向一个新的空间
  // 由于数组的typeof也是'object',所以用Array.isArray(obj)
  var new_obj = Array.isArray(obj) ? [] : {};
  // 首先判断obj的类型
  // 普通类型
  if (typeof obj != "object") {
    // 这里不能直接返回obj,不然就是浅拷贝的性质
    return (new_obj = obj);
  }
  //引用类型
  //数组
  if (obj instanceof Array) {
    for (i = 0; i < obj.length; i++) {
      new_obj[i] = obj[i];
      if (typeof new_obj[i] == "object") {
        deepClone(new_obj[i]);
      }
    }
  } else {
    //对象
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        // 对象中的数组和对象
        if (typeof obj[key] == "object") {
          new_obj[key] = deepClone(obj[key]);
        } else {
          //对象中没有引用类型
          new_obj[key] = obj[key];
        }
      }
    }
  }
  return new_obj;
}
var deepClone = deepClone(obj);
console.log(deepClone);
// 测试是不是深拷贝
obj.fn = "我改变了方法属性";
console.log(obj); //{arr1: Array(3), fn: ƒ, a: "我是普通属性", c: "我新增了一个属性"}
console.log(deepClone); // 还是 {arr1: Array(3), fn: ƒ, a: "我是普通属性"}
```

##### \$.extend() （jQuery 中的方法）

```javascript
var $ = require('jquery);
var obj2 = $.extend(true,{},obj);
```
