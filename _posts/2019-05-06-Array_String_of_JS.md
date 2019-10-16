---
layout: post
title:  " 😙 JS中数组与字符串的操作方法总结"
date:   2019-05-06 09:21:02
categories: [JavaScript]
tags: [JavaScript]
comments: true
---

<!--more-->

## 数组

#### 不改变原数组

#####  concat()

- 连接两个或更多的数组
- 返回连接后的新数组

```javascript
var arr1 = [22, 34, 35, 36];
var arr2 = [3, 4, 5];
var arr = a.concat(b);

console.log(arr);         // [ 22, 34, 35, 36, 3, 4, 5 ]
console.log(a);           // [ 22, 34, 35, 36 ]
console.log(b);           // [ 3, 4, 5 ]
```

#####  join()

- 把数组的所有元素放入一个字符串，元素通过指定的分隔符进行分隔
- （ 数组 --> 字符串）
- 返回字符串

```javascript
var arr1 = [22, 34, 35, 36];
var arr = arr1.join(':');

console.log(arr);            //  22:34:35:36

console.log(typeof arr1);    // object
console.log(typeof arr);     // string

console.log(arr1);           // [ 22, 34, 35, 36 ] 原数组未改变
```

#####  slice()

- arrayObject.slice(start,end)
- 从数组中返回选定的元素，返回数值区间是 [ statr,...,end-1 ]的
- 返回的是新数组

```javascript
var arr1 = [22, 34, 35, 36, 12, 99];

console.log(arr1.slice(0));       // [ 22, 34, 35, 36, 12, 99 ]
console.log(arr1.slice(1));       // [ 34, 35, 36, 12, 99 ]
console.log(arr1.slice(-1));      // [ 99 ]
console.log(arr1.slice(-2));      // [ 12, 99 ]
console.log(arr1.slice(-5, -2));  // [ 34, 35, 36 ]

console.log(arr1);                // [ 22, 34, 35, 36, 12, 99 ]  原数组未改变
```

#####  toString()

- 数组转换为字符串（数组 --> 字符串）
- 返回字符串

```javascript
var arr1 = [22, 34, 35, 36, 12, 99];
var arr2 = arr1.toString();

console.log(arr2);        // '22,34,35,36,12,99'
console.log(typeof arr2); // string

console.log(arr1);        // [ 22, 34, 35, 36, 12, 99 ] 原数组未改变
```

#### 改变原数组

##### push()


-arrayObject.push(newelement1,newelement2,....,newelementX)
- 向数组的末尾添加一个或多个元素
- 改变原数组
- **返回新数组的长度**

```javascript
var arr1 = [22, 34, 35];

console.log(arr1.push(100));  // 4 返回新数组的长度

console.log(arr1);            // [ 22, 34, 35, 100 ]  // 改变原数组
```

##### unshift()

- 向数组的开头添加一个或多个元素
- **返回新数组的长度**
- 改变原数组

```javascript
var arr1 = [22, 34, 35];

console.log(arr1.unshift(99));      // 4   返回新数组的长度

console.log(arr1.unshift(100,101)); // 6   返回新数组的长度

console.log(arr1);                  // [ 100, 101, 99, 22, 34, 35 ]
```

#####  pop()

- 删除数组最后一个元素，如果数组为空，则不改变数组，返回undefined
- 改变原数组
- **返回被删除的元素**

```javascript
var arr1 = [22, 34, 35, 36, 12, 99];

console.log(arr1.pop()); // 99

console.log(arr1);       // [ 22, 34, 35, 36, 12 ] 修改了原数组
```

##### shift()

- 删除数组的第一个元素
- **返回数组的第一个元素的值**

```javascript
var arr1 = [22, 34, 35];

console.log(arr1.shift());  // 22

console.log(arr1);          // [ 34, 35 ]
```

##### reverse()

- 颠倒数组中元素的顺序
- 返回改变后的数组

```javascript
var arr1 = [22, 34, 35];

console.log(arr1.reverse());  // [ 35, 34, 22 ]

console.log(arr1);            // [ 35, 34, 22 ] 原数组改变
```

##### sort()

- 在原数组上进行排序，不生成副本
- 返回排序后的新数组

```javascript

var arr1 = [2, 34, 5, 23];

console.log(arr1.sort());  // [ 2, 23, 34, 5 ]

console.log(arr1);         // [ 2, 23, 34, 5 ]
```

##### splice()

<img src="/image/posts/blog1401.png" style="display:block;margin:0 auto;"> 

```javascript
var arr1 = [2, 3, 5, 7];

console.log(arr1.splice(2,0));  // []，表示从arr1[2]开始，删除0个元素，返回的是包含被删除项目的新数组，如果有的话。
console.log(arr1);              // [ 2, 3, 5, 7 ]
console.log(arr1.splice(2));    // [ 5, 7 ] ，删除arr1[2]及以后的元素，返回的是从arr1[2]开始，包含删除的2个元素的新数组
console.log(arr1);              // [ 2, 3 ]
console.log(arr1.splice(1,1,101,100,99)); // [ 3 ] ，从 arr1[1]开始，删除一个元素，替换为101,100,99
console.log(arr1);         // [ 2, 101, 100, 99 ]
```

## 字符串

#### split()

- 将字符串转化数组（字符串-->数组，与join是相反的操作）
- 不改变原字符串
- 返回数组

```javascript
var str = 'a,b,cd';

console.log(str.split(''));  // [ 'a', ',', 'b', ',', 'c', 'd' ]

console.log(str.split(','));  // [ 'a', 'b', 'cd' ]
 
console.log(str);     //  'a,b,cd' 未改变原字符串
```

#### slice()

- 返回字符串中提取的子字符串
- 不改变原字符串（可类比数组的slice方法）

```javascript
var str="Hello World";
var str1=str.slice(2); //如果只有一个参数，则提取开始下标到结尾处的所有字符串
var str2=str.slice(2,7); //两个参数，提取下标为2，到下标为7但不包含下标为7的字符串
var str3=str.slice(-7,-2); //如果是负数，-1为字符串的最后一个字符。提取从下标-7开始到下标-2但不包含下标-2的字符串。前一个数要小于后一个数，否则返回空字符串

console.log(str1); // llo World
console.log(str2); // llo W
console.log(str3); // o Wor

console.log(str);  // Hello World
```

#### replace()
- 在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串
- 不修改原字符串

```javascript
var str="hello WORLD";
var reg=/o/ig;        // o为要替换的关键字，不能加引号，否则替换不生效，i忽略大小写，g表示全局查找。
var str1=str.replace(reg,"**")

var str2 = str.replace('D','m');

console.log(str1);    // hell** W**RLD
console.log(str2);    // hello WORLm
console.log(str);     // hello WORLD
```



#### indexOf()

- 返回某个指定的子字符串在字符串中第一次出现的位置

```javascript
var str="Hello World";
var str1=str.indexOf("o");
var str2=str.indexOf("world");
var str3=str.indexOf("o",str1+1);

console.log(str1); // 4 默认只找第一个关键字位置，从下标0开始查找
console.log(str2); // -1 没有找到，对大小写敏感
console.log(str3); // 7
```

#### toLowerCase()、toUpperCase()

- 把字符串转为小写、大写，返回新的字符串
- 不修改原字符串

```javascript
var str = "Hello World";
var str1 = str.toLowerCase();
var str2 = str.toUpperCase();

console.log(str1); // hello world
console.log(str2); // HELLO WORLD

console.log(str);  // Hello World
```
