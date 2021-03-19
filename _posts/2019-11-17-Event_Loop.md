---
layout: post
title: "🍛 事件循环"
date: 2019-11-17 00:00:00
categories: [JavaScript]
tags: [Event Loop]
comments: true
---

JavaScript 是一门单线程语言，Event Loop 是 javascript 的执行机制。
<!--more-->

javascript 是单线程的语言，也就是说，同一个时间只能做一件事。而这个单线程的特性，与它的用途有关，作为浏览器脚本语言，JavaScript 的主要用途是与用户互动，以及操作 DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定 JavaScript 同时有两个线程，一个线程在某个 DOM 节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

### 进程与线程

- 进程是 CPU 资源分配的最小单位；线程是 CPU 调度的最小单位。
- 一个进程由一个或多个线程组成，线程是一个进程中代码的不同执行路线。

### 浏览器中的 Event Loop

#### 同步任务和异步任务

javascript 是单线程。单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。于是 js 所有任务分为两种：

- 同步任务：调用立即得到结果的任务，同步任务在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
- 异步任务：调用无法立即得到结果，需要额外的操作才能预期结果的任务，异步任务不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

JS 引擎遇到异步任务（DOM 事件监听、网络请求、setTimeout 计时器等），会交给相应的线程单独去维护异步任务，等待某个时机（计时器结束、网络请求成功、用户点击 DOM），然后由 事件触发线程 将异步对应的 回调函数 加入到消息队列中，消息队列中的回调函数等待被执行。

具体来说，异步运行机制如下：

- 所有同步任务都在主线程上执行，形成一个`执行栈`
- 主线程之外，还存在一个`任务队列（task queue）`。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
- 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
- 主线程不断重复上面的第三步。

主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为 Event Loop（事件循环）

  <img src="/image/posts/202001092.jpg" style="display:block;margin:0 auto;">

#### 宏任务和微任务

任务分为宏任务（macrotask ）和微任务（microtask ） 两种。

- 宏任务队列可以有多个，微任务队列只有一个
- 宏任务：`script（整体代码）`, `setTimeout`, `setInterval`, `setImmediate`, I/O, UI rendering 等
- 微任务：`process.nextTick`, `Promises`（这里指浏览器实现的原生 Promise）, `Object.observe`, `MutationObserver`等

在挂起任务时，JS 引擎会将所有任务按照类别分到这两个队列中，首先在 宏任务 的队列中取出第一个任务，执行完毕后取出 微任务 队列中的所有任务顺序执行；之后再取 宏任务，周而复始，直至两个队列的任务都取完。
<img src="/image/posts/202001091.jpg" style="display:block;margin:0 auto;">

#### 总结

- 事件循环是 js 实现异步的核心
- 每轮事件循环分为 3 个步骤：
  > 执行 macrotask 队列的一个任务
  > 执行完当前 microtask 队列的所有任务
  > UI render

### Node 中的 Event Loop

Node 中的 Event Loop 和浏览器中的是完全不相同的东西。Node.js 采用 V8 作为 js 的解析引擎，而 I/O 处理方面使用了自己设计的 libuv，libuv 是一个基于事件驱动的跨平台抽象层，封装了不同操作系统一些底层特性，对外提供统一的 API，事件循环机制也是它里面的实现。

#### 6 个阶段

libuv 引擎中的事件循环分为 6 个阶段，它们会按照顺序反复运行。每当进入某一个阶段的时候，都会从对应的回调队列中取出函数去执行。当队列为空或者执行的回调函数数量到达系统设定的阈值，就会进入下一阶段。
<img src="/image/posts/202001093.jpg" style="display:block;margin:0 auto;">

从上图中，大致看出 node 中的事件循环的顺序：
外部输入数据-->轮询阶段(poll)-->检查阶段(check)-->关闭事件回调阶段(close callback)-->定时器检测阶段(timer)-->I/O 事件回调阶段(I/O callbacks)-->闲置阶段(idle, prepare)-->轮询阶段（按照该顺序反复运行）...

- timers 阶段：这个阶段执行 timer（setTimeout、setInterval）的回调
- I/O callbacks 阶段：处理一些上一轮循环中的少数未执行的 I/O 回调
- idle, prepare 阶段：仅 node 内部使用
- poll 阶段：获取新的 I/O 事件, 适当的条件下 node 将阻塞在这里
- check 阶段：执行 setImmediate() 的回调
- close callbacks 阶段：执行 socket 的 close 事件回调

#### 总结

- Node.js 中的事件循环分为 6 个阶段
- 浏览器和 Node 环境下，microtask 任务队列的执行时机不同
   > Node.js中，microtask 在事件循环的各个阶段之间执行
   
   > 浏览器端，microtask 在事件循环的 macrotask 执行完之后执行

### Node 与浏览器的 Event Loop 差异

**浏览器和 Node 环境下，microtask 任务队列的执行时机不同**

- 浏览器环境下，microtask 的任务队列是每个 macrotask 执行完之后执行。
  <img src="/image/posts/202001094.png" style="display:block;margin:0 auto;">
- Node.js 中，microtask 会在事件循环的各个阶段之间执行，也就是一个阶段执行完毕，就会去执行 microtask 队列的任务。
  <img src="/image/posts/202001095.png" style="display:block;margin:0 auto;">
