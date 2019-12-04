---
layout: post
title: "🧵 防抖与节流"
date: 2019-09-13 00:00:00
categories: [JavaScript]
tags: [JavaScript]
comments: true

---

<!--more-->

```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>防抖与节流</title>
  </head>
  <body>
    <div
      id="div1"
      style="height:150px;line-height:150px;text-align:center; color: #fff;background-color:#ccc;font-size:80px;"
    ></div>
    <br />

    <div
      id="div2"
      style="height:150px;line-height:150px;text-align:center; color: #fff;background-color:#ccc;font-size:80px;"
    ></div>
    <script>
      let num1 = 1;
      let mydiv1 = document.getElementById("div1");

      function count1() {
        mydiv1.innerHTML = num1++;
      }
    </script>
    <script>
      let num2 = 1;
      let mydiv2 = document.getElementById("div2");

      function count2() {
        mydiv2.innerHTML = num2++;
      }
    </script>
    <script>

      //#region 防抖：非立即执行
      function debounce1(fn, wait) {
        // 维护一个 timer，用来记录当前执行函数状态
        let timer = null;

        return function() {
          // 通过 ‘this’ 和 ‘arguments’ 获取函数的作用域和变量
          // 触发事件后函数不会立即执行，而是在 n 秒后执行，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。
          let context = this;
          let args = arguments;
          // 清理掉正在执行的函数，并重新执行
          if(timer){
            clearTimeout(timer);
          }

          timer = setTimeout(function() {
            fn.apply(context, args);
          }, wait);
        };
      }
      //#endregion



      //#region 防抖：立即执行
      // 触发事件后函数会立即执行，然后 n 秒内不触发事件才能继续执行函数的效果。
      function debounce2(func, wait) {
        let timer;
        return function() {
          let context = this;
          let args = arguments;

          if (timer) {
            clearTimeout(timer);
          }

          let callNow = !timer;
          timer = setTimeout(() => {
            timer = null;
          }, wait);

          if (callNow) {
            func.apply(context, args);
          }
        };
      }
      //#endregion



      //#region 防抖：整合立即和非立即
      /**
       * @desc 函数防抖
       * @param func 函数
       * @param wait 延迟执行毫秒数
       * @param immediate true 表立即执行，false 表非立即执行
       */
      function debounce3(func, wait, immediate) {
        let timer;
        return function() {
          let context = this;
          let args = arguments;

          if (timer) {
            clearTimeout(timer);
          }

          if (immediate) {
            let callNow = !timer;
            timer = setTimeout(() => {
              timer = null;
            }, wait);

            if (callNow) {
              func.apply(context, args);
            }
          } else {
            timer = setTimeout(() => {
              func.apply(context, args);
            }, wait);
          }
        };
      }
      //#endregion

      // 在 debounce 中包装函数，过 1 秒触发一次
      mydiv1.onclick = debounce1(count1, 1000, false);
      //  mydiv1.onclick = debounce2(count1, 1000, false);
      //  mydiv1.onclick = debounce3(count1, 1000, false);
    </script>



    <script>
      // 节流:指连续触发事件但是在 n 秒中只执行一次函数,节流会稀释函数的执行频率。
      // 节流函数不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数。
      // 两种实现方法：时间戳、定时器

      //#region 方法一：时间戳
      function throttle1(func, wait) {
        let prev = Date.now();
        return function() {
          let now = Date.now();
          let context = this;
          let args = arguments;
          if (now - prev >= wait) {
            func.apply(context, args);
            prev = now;
          }
        };
      }
      //   mydiv2.onmousemove = throttle1(count2,1000);
      //#endregion

      

      //#region 方法二：定时器
      function throttle2(func, wait) {
        let timer;
        return function() {
          let context = this;
          let args = arguments;
          if (!timer) {
            timer = setTimeout(() => {
              timer = null;
              func.apply(context, args);
            }, wait);
          }
        };
      }
      //#endregion



      //#region  其实时间戳版和定时器版的节流函数的区别就是，
      // 时间戳版的函数触发是在时间段内开始的时候，而定时器版的函数触发是在时间段内结束的时候
      // 合并两个版本的节流函数
      /**
       * @desc 函数节流
       * @param func 函数
       * @param wait 延迟执行毫秒数
       * @param type 1 表时间戳版，2 表定时器版
       */
      function throttle3(func, wait, type) {
        if (type === 1) {
          let previous = 0;
        } else if (type === 2) {
          let timeout;
        }
        return function() {
          let context = this;
          let args = arguments;
          if (type === 1) {
            let now = Date.now();

            if (now - previous > wait) {
              func.apply(context, args);
              previous = now;
            }
          } else if (type === 2) {
            if (!timeout) {
              timeout = setTimeout(() => {
                timeout = null;
                func.apply(context, args);
              }, wait);
            }
          }
        };
      }
      //#endregion
      mydiv2.onmousemove = throttle1(count2, 1000);
      // mydiv2.onmousemove = throttle2(count2, 1000);
      // mydiv2.onmousemove = throttle3(count2, 1000);
    </script>
  </body>
</html>

```