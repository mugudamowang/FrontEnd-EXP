<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Javascript](#javascript)
    - [1. JS的执行机制](#1-js%E7%9A%84%E6%89%A7%E8%A1%8C%E6%9C%BA%E5%88%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Javascript

#### 1. JS的执行机制

这里从`event-loop`的层面解释:  当主线程的栈空了, 它就去读取`event-queue`的排队的任务事件, 并不断重复这个过程. 这就是JS的执行机制.

明确一个概念, JS是单线程的, 这是由于它的特性决定的, 避免操作DOM带来同步问题.  为了让JS充分利用CPU资源, web worker的加入让JS可以操作子线程, 完全受主线程控制, 无法操作DOM, 所以本质上还是单线程. 为了事先异步的效果, JS有事件循环机制.

主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码调用各种外部API，它们在"任务队列"中加入被注册的事件. 只要栈中的同步代码执行完毕，主线程就会去读取"任务队列"中的回调，依次执行那些事件所对应的回调函数。

事件类型

- 同步任务: 处理紧急的基础的内容, 如页面骨架和元素的渲染
- 异步任务: 占用资源的, 耗时久的, 如视频图片等媒体资源的的加载
- 微任务和宏任务
  - 微任务: promise, process.nextTick
  - 宏任务: setTimeout, setInterval, 整体代码script
  - 执行顺序: 宏任务-->微任务
    - 执行栈选择最先进入队列的宏任务（一般都是script），执行其**同步代码**直至结束；
    - 检查将微任务和宏任务入队(微\宏event queue)
    - 先执行微任务队列, 执行完毕再执行**异步代码**中的宏任务队列( 开始下一个宏任务 ).
    - 重复这个过程直到event-queue为空即执行完毕

```js
let data = [];
$.ajax({					//1. 异步任务ajax进入event table, 注册回调函数success
  url:www.javascript.com,
  data:data,
  success:() => {
      console.log('发送成功!');  //3. ajax事件完成, 回调success进入event queue
  }
})
console.log('代码执行结束');   //2. 执行同步任务console.log("代码执行结束")

//4. 主线程为空, 进入event queue读取回调函数success并执行
```