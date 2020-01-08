---
layout: post
title: "🚴🏻‍♀️ export 和 export default"
date: 2019-05-19 00:00:00
categories: [ES6,JavaScript]
tags: [JavaScript]
comments: true
---

ES6 模块的设计思想是尽量静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。ES6 模块是“编译时加载”，使得静态分析成为可能。

<!--more-->

### 主要区别

- `export` 与 `export default` 均可用于导出常量、函数、文件、模块等

- 你可以在其它文件或模块中通过 `import+(常量 | 函数 | 文件 | 模块)名` 的方式，将其导入，以便能够对其进行使用

- 在一个文件或模块中，`export`、 `import` 可以有多个，`export default` 仅有一个

- 通过 `export` 方式导出，在 `import` 导入时要加 `{ }`，`export default` 则不需要

### export

- 输出变量：

```javascript
// profile.js
export var firstName = "Michael";
export var lastName = "Jackson";
export var year = 1958;
```

or

```javascript
// profile.js
var firstName = "Michael";
var lastName = "Jackson";
var year = 1958;

export { firstName, lastName, year };
```

- 输出函数，对象

```javascript
export function multiply(x, y) {
  return x * y;
}
```

通常情况下，export 输出的变量就是本来的名字，但是可以使用 as 关键字重命名。

```javascript
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

_需要特别注意的是，export 命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。_

```javascript
export 1;   // 报错

var m = 1;
export m;   // 报错
```

上面两种写法都会报错，因为没有提供对外的接口。第一种写法直接输出 1，第二种写法通过变量 m，还是直接输出 1。1 只是一个值，不是接口。正确的写法是下面这样。

```javascript
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```

上面三种写法都是正确的，规定了对外的接口 m。其他脚本可以通过这个接口，取到值 1。它**们的实质是，在接口名与模块内部变量之间，建立了一一对应的关系**。

同样的，function 和 class 的输出，也必须遵守这样的写法。

```javascript
function f() {}
export f;         // 报错

export function f() {};  // 正确

function f() {}
export {f};       // 正确
```

**export 语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。**

```javascript
export var foo = "bar";
setTimeout(() => (foo = "baz"), 500);
```

上面代码输出变量 foo，值为 bar，500 毫秒之后变成 baz。

##### import

- **必须使用大括号**

- **大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同**

- **如果想为输入的变量重新取一个名字，import 命令要使用 as 关键字，将输入的变量重命名**

- **`import` 命令输入的变量都是只读的，不允许在加载模块的脚本里面，改写接口**

- **`import` 命令具有提升效果，会提升到整个模块的头部，首先执行。**

- **`import` 是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构**

```javascript
// main.js
import { firstName, lastName, year } from "./profile.js";

function setName(element) {
  element.textContent = firstName + " " + lastName;
}
```

```javascript
import { lastName as surname } from "./profile.js";
```

脚本加载了变量 a，对其重新赋值就会报错，因为 a 是一个只读的接口。但是，如果 a 是一个对象，改写 a 的属性是允许的。

```javascript
import { a } from "./xxx.js";

a = {}; // Syntax Error : 'a' is read-only;
```

```javascript
import { a } from "./xxx.js";

a.foo = "hello"; // 合法操作
```

```javascript
import { 'f' + 'oo' } from 'my_module';  // 报错

let module = 'my_module';
import { foo } from module;   // 报错

// 报错
if (x === 1) {
  import { foo } from 'module1';
} else {
  import { foo } from 'module2';
}
```

上面三种写法都会报错，因为它们用到了表达式、变量和 if 结构。**在静态分析阶段，这些语法都是没法得到值的**。

### export default

使用 export 导出的模块，`import`命令的时候，**用户需要知道所要加载的变量名或函数名，否则无法加载**。为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到 export default 命令，为模块指定默认输出。

```javascript
// export-default.js
export default function() {
  console.log("foo");
}
```

**其他模块加载该模块时，import 命令可以为该匿名函数指定任意名字**

```javascript
// import-default.js
import yourCustomName from "./export-default";
customName(); // 'foo'
```

export default 命令用在非匿名函数前，也是可以的。
```javascript
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;
```

**对比一下默认输出和正常输出**
```javascript
// 第一组
export default function crc32() { // 输出
  // ...
}

import yourCustomName from 'crc32'; // 输入

// 第二组
export function crc32() { // 输出
  // ...
};

import {crc32} from 'crc32'; // 输入
```

上面代码的两组写法，第一组是使用 **`export default`** 时，对应的 `import` 语句**不需要使用大括号**；第二组是不使用 **`export default` **时，对应的 `import` 语句**需要使用大括号**。

export default命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此**`export default` 命令只能使用一次**。所以，import命令后面才不用加大括号，因为只可能唯一对应export default命令。

本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。**正是因为 `export default` 命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句，可以直接将一个值写在export default之后。。**
```javascript
export var a = 1;   // 正确

var a = 1;
export default a;   // 正确

export default var a = 1; // 错误

export default 42;   // 正确

export 42;   // 报错


function fun(){...};
export fun;          // 报错
export {fun};        // 正确
export default fun;  // 正确
export default fucntion fun() {...}  // 正确
```

export default也可以用来输出类：
```javascript
// MyClass.js
export default class { ... }

// main.js
import MyClass from 'MyClass';
let o = new MyClass();
```