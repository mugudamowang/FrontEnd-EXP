<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Javascript](#javascript)
    - [1. JS的执行机制](#1-js%E7%9A%84%E6%89%A7%E8%A1%8C%E6%9C%BA%E5%88%B6)
    - [2. typeof (字节笔试)](#2-typeof-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AF%95)
    - [3. Array数组方法](#3-array%E6%95%B0%E7%BB%84%E6%96%B9%E6%B3%95)
    - [4. JS执行上下文](#4-js%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87)
    - [5. 闭包](#5-%E9%97%AD%E5%8C%85)

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



#### 2. typeof (字节笔试)

typeof对于基本类型, 除了`null`都可以显示正确的类型.

```js
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
typeof b // b 没有声明，但是还会显示 undefined
```

对于对象, 除了函数为`function`, 都会显示`object`

!!!特别的, 当为`null`时, 也是显示object, 这是一个历史遗留问题.

[为什么typeof null的结果是Object?](https://juejin.cn/post/6844903895177805837)



#### 3. Array数组方法

| 方法名   | 使用方法                                                  | 描述                                                         |
| -------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| push     | arr.push(args)                                            | 接受任意数量的参数入栈到数组末尾                             |
| pop      | arr.pop()                                                 | 删除栈尾                                                     |
| shift    | arr.shift()                                               | 删除队首                                                     |
| unshift  | arr.unshift()                                             | 添加队首                                                     |
| reverse  | arr.reverse()                                             | 反转数组                                                     |
| sort     | arr.sort([fn()])                                          | 排序(默认比较字符串), 接收函数参数                           |
| slice    | arr.slice(index1, [index2])                               | 切片操作, 获取其中一部分并生成index1到index2的新数组<br />只有index1则到arr.length |
| splice   | arr.splice(index, n)<br />arr.splice(index, n, str)<br /> | - 删除index开始的n项<br />- 删除index开始的n项, 并插入str到index |
| concat   | arr1.concat(arr2)                                         | 返回拼接arr1和arr2的新数组(非直接作用)<br />无参数时直接复制当前数组并返回 |
| toString | arr.toString()                                            | 返回逗号分开的字符串                                         |
| indexOf  | arr.indexOf(n)                                            | 返回元素n对应的下标                                          |
| fliter   | arr.filter(fn)                                            | 过滤满足fn条件的项并返回新数组. (item , index , array)       |
| every    | arr.every(fn)                                             | 对于数组中的每一项运行一次function,如果都为true则返回true. (item , index , array) |
| some     | arr.some(fn)                                              | 和every类似, 但是是包含关系 (item , index , array)           |
| map      | arr.map(fn)                                               | 对每一项运行一次fn操作, 之后返回操作后的新数组 (item , index , array) |
| forEach  | arr.forEach(fn)                                           | for循环 (item , index , array)                               |
| reduce   | arr.reduce(fn)                                            | 求数组中所有值之和的操作 (pre, cur, index, arrray)           |

[JavaScript深入理解之Array类型详解](http://cavszhouyou.top/JavaScript%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E4%B9%8BArray%E8%AF%A6%E8%A7%A3.html)



#### 4. JS执行上下文

JS是解释型语言有专门的JS引擎对代码进行动态解析执行. 因此, 在执行一段代码前会先进行解析等准备工作, 这个过程就是创建执行上下文. 当解析完毕才开始执行代码.

执行上下文定义了变量或函数有权访问的其他数据，决定了它们各自的行为.

可执行代码主要有:

1. 全局可执行代码-->全局执行上下文(window对象)
2. 函数可执行代码-->函数执行上下文
3. eval可执行代码-->运行于当前上下文中

每一个执行上下文有以下三个属性:

1. 变量对象 VO: 表示变量的对象, 声明的函数(非函数表达式)和变量都是它的一个属性
2. 作用域链
3. this

使用执行上下文栈对这些上下文进行管理.

创建过程:

1. 用当前函数的**参数列表**（`arguments`）初始化一个 “变量对象” 并将当前执行上下文与之关联 ，函数代码块中声明的 **变量** 和 **函数** 将作为属性添加到这个变量对象上。**在这一阶段，会进行变量和函数的初始化声明，变量统一定义为 `undefined` 需要等到赋值时才会有确值，而函数则会直接定义**。

   >  有没有发现这段加粗的描述非常熟悉？没错，这个操作就是  **变量声明提升**（变量和函数声明都会提升，但是函数提升更靠前）。

2. 构建作用域链

   **作用域 规定了如何查找变量，也就是确定当前执行代码对变量的访问权限**。当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级（词法层面上的父级）执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做 **作用域链**

   函数的作用域在函数创建时就已经确定了。当函数创建时，会有一个名为 `[[scope]]` 的内部属性保存所有父变量对象到其中。

   当函数执行时，会创建一个执行环境，然后通过复制函数的 `[[scope]]`  属性中的对象构建起执行环境的作用域链，然后，变量对象 `VO` 被激活生成 `AO` 并添加到作用域链的前端，完整作用域链创建完成

3. 确定 `this` 的值

   如果当前函数被作为对象方法调用或使用 `bind` `call` `apply` 等 `API` 进行委托调用，则将当前代码块的调用者信息（`this value`）存入当前执行上下文，否则默认为全局对象调用

ES5之后对执行上下文做了调整, 主要用词法环境和变量环境替代了变量对象和活动对象. 本质都差不多.

- **词法作用域**，就意味着函数 **被定义的时候**，它的作用域就已经确定了，和拿到哪里执行没有关系，因此词法作用域也被称为 “**静态作用域**”

[什么是执行上下文--掘金](https://juejin.cn/post/6844904158957404167#heading-4)

#### 5. 闭包

闭包是 JavaScript 中的一项功能，**其中内部函数可以访问外部（封闭）的变量, 是函数和函数变量所处作用域的混合**

闭包有三个作用域, 分别是:

1. 函数自身的变量
2. 外部函数的变量
3. 全局变量

可见, 通过闭包通过作用域链实现.

主要应用场景是:

- 单例模式, 通过判断是否存在实例来避免了重复实例化带来的内存开销
- 模拟私有变量, 保存在外层函数的私有变量通过内层函数访问
- 函数柯里化

主要问题: (在较旧的浏览器GC算法中, 现在一般不会)

- 内存泄漏: 由于闭包使用过度而导致的内存占用无法释放的情况，我们称之为：**内存泄露**。内存泄漏可能会导致应用程序卡顿或者崩溃。

解决方案:

- 使用严格模式
- 关注 `DOM` 生命周期，在销毁阶段记得解绑相关事件
- 可以使用事件委托的手段统一处理事件
- 避免过度使用闭包

[JavaScript闭包简易指南(翻译)](https://www.bukun.top/2020/07/24/javascript%E9%97%AD%E5%8C%85%E7%AE%80%E6%98%93%E6%8C%87%E5%8D%97(%E7%BF%BB%E8%AF%91)/)

[作用域和闭包](https://mitianyi.gitbook.io/frontend-interview-guide/javascript-basics/scope-and-closure)

#### 6. This

**this 指向最后一次调用这个方法的对象**, 可以通过函数的调用来理解This.

首先this是执行上下文的一个属性, 我们一般对this的指向会做一些讨论.

1. 函数调用模式: 当一个函数不是某个对象的属性时, 作为函数直接调用, this指向window全局对象

2. 方法调用模式: 通过 `.` 运算符进行调用, 作为对象属性执行, this绑定到调用它的对象上.

3. 构造器调用模式: 通过new关键字构造的函数, 在函数执行前调创建新对象, 并将this绑定到这个新对象上.

   ```js
   var peo = new person("xiao ming"); // this 绑定到 peo 对象
   console.log(peo.name); // "xiao ming"
   ```

4. 通过 call/apply/bind 调用, 主要可以控制this的指向

   1. call ( this, arg1,arg2,...,args ) 参数直接传递
   2. apply ( this, args[] ) 参数以数组形式传递
   3. bind ( target )  将this绑定到target上

此外, 箭头函数是没有this的, 这个函数中的 `this` 只取决于他外面的第一个不是箭头函数的函数的 `this`

