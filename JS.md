<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Javascript](#javascript)
    - [1. JS的执行机制](#1-js%E7%9A%84%E6%89%A7%E8%A1%8C%E6%9C%BA%E5%88%B6)
    - [2. typeof (字节笔试)](#2-typeof-%E5%AD%97%E8%8A%82%E7%AC%94%E8%AF%95)
    - [3. Array数组方法](#3-array%E6%95%B0%E7%BB%84%E6%96%B9%E6%B3%95)
    - [4. JS执行上下文](#4-js%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87)
    - [5. 闭包](#5-%E9%97%AD%E5%8C%85)
    - [6. This](#6-this)
    - [7. 原型链和继承](#7-%E5%8E%9F%E5%9E%8B%E9%93%BE%E5%92%8C%E7%BB%A7%E6%89%BF)
- [ES6](#es6)
    - [1. let const](#1-let-const)
    - [2. const](#2-const)
    - [3. 解构](#3-%E8%A7%A3%E6%9E%84)
    - [4. 模板字符串](#4-%E6%A8%A1%E6%9D%BF%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [5. 简化对象写法](#5-%E7%AE%80%E5%8C%96%E5%AF%B9%E8%B1%A1%E5%86%99%E6%B3%95)
    - [6. 箭头函数](#6-%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)
    - [7. 函数参数的默认设置](#7-%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0%E7%9A%84%E9%BB%98%E8%AE%A4%E8%AE%BE%E7%BD%AE)
    - [8. rest参数](#8-rest%E5%8F%82%E6%95%B0)
    - [9. 扩展运算符](#9-%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [10. Symbol](#10-symbol)
    - [11. Iterator 迭代器 / for...of循环](#11-iterator-%E8%BF%AD%E4%BB%A3%E5%99%A8--forof%E5%BE%AA%E7%8E%AF)
    - [12. 生成器](#12-%E7%94%9F%E6%88%90%E5%99%A8)
    - [13. Promise](#13-promise)
    - [14. set](#14-set)
    - [15. map](#15-map)
    - [16. class](#16-class)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Javascript

#### 1. JS的执行机制

这里从`event-loop`的层面解释:  当主线程的栈空了, 它就去读取`event-queue`的排队的任务事件, 并不断重复这个过程. 这就是JS的执行机制.

明确一个概念, JS是单线程的, 这是由于它的特性决定的, 避免操作DOM带来同步问题.  为了让JS充分利用CPU资源, web worker的加入让JS可以操作子线程, 完全受主线程控制, 无法操作DOM, 所以本质上还是单线程. 为了事先异步的效果, JS有事件循环机制.

主线程运行的时候, 产生堆（heap）和栈（stack）, 栈中的代码调用各种外部API，它们在"任务队列"中加入被注册的事件. 只要栈中的同步代码执行完毕, 主线程就会去读取"任务队列"中的回调, 依次执行那些事件所对应的回调函数。

事件类型

- 同步任务: 处理紧急的基础的内容, 如页面骨架和元素的渲染
- 异步任务: 占用资源的, 耗时久的, 如视频图片等媒体资源的的加载
- 微任务和宏任务
  - 微任务: promise, process.nextTick
  - 宏任务: setTimeout, setInterval, 整体代码script
  - 执行顺序: 宏任务-->微任务
    - 执行栈选择最先进入队列的宏任务（一般都是script）, 执行其**同步代码**直至结束；
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
typeof b // b 没有声明, 但是还会显示 undefined
```

对于对象, 除了函数为`function`, 都会显示`object`

!!!特别的, 当为`null`时, 也是显示object, 这是一个历史遗留问题.

常见机器标识码

| 数据类型     | 机器码标识     |
| ------------ | -------------- |
| 对象(Object) | 000            |
| 整数         | 1              |
| 浮点数       | 010            |
| 字符串       | 100            |
| 布尔         | 110            |
| undefined    | -2^31(即全为1) |
| null         | 全为0          |

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
| splice   | arr.splice(index, n)<br />arr.splice(index, n, str)<br /> | - 删除index开始的n项, 并返回被删除的<br />- 删除index开始的n项, 并插入str到index |
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

执行上下文定义了变量或函数有权访问的其他数据, 决定了它们各自的行为.

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

1. 用当前函数的**参数列表**（`arguments`）初始化一个 “变量对象” 并将当前执行上下文与之关联 , 函数代码块中声明的 **变量** 和 **函数** 将作为属性添加到这个变量对象上。**在这一阶段, 会进行变量和函数的初始化声明, 变量统一定义为 `undefined` 需要等到赋值时才会有确值, 而函数则会直接定义**。

   >  有没有发现这段加粗的描述非常熟悉？没错, 这个操作就是  **变量声明提升**（变量和函数声明都会提升, 但是函数提升更靠前）。

2. 构建作用域链

   **作用域 规定了如何查找变量, 也就是确定当前执行代码对变量的访问权限**。当查找变量的时候, 会先从当前上下文的变量对象中查找, 如果没有找到, 就会从父级（词法层面上的父级）执行上下文的变量对象中查找, 一直找到全局上下文的变量对象, 也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做 **作用域链**

   函数的作用域在函数创建时就已经确定了。当函数创建时, 会有一个名为 `[[scope]]` 的内部属性保存所有父变量对象到其中。

   当函数执行时, 会创建一个执行环境, 然后通过复制函数的 `[[scope]]`  属性中的对象构建起执行环境的作用域链, 然后, 变量对象 `VO` 被激活生成 `AO` 并添加到作用域链的前端, 完整作用域链创建完成

3. 确定 `this` 的值

   如果当前函数被作为对象方法调用或使用 `bind` `call` `apply` 等 `API` 进行委托调用, 则将当前代码块的调用者信息（`this value`）存入当前执行上下文, 否则默认为全局对象调用

ES5之后对执行上下文做了调整, 主要用词法环境和变量环境替代了变量对象和活动对象. 本质都差不多.

- **词法作用域**, 就意味着函数 **被定义的时候**, 它的作用域就已经确定了, 和拿到哪里执行没有关系, 因此词法作用域也被称为 “**静态作用域**”

[什么是执行上下文--掘金](https://juejin.cn/post/6844904158957404167#heading-4)

#### 5. 闭包

闭包是 JavaScript 中的一项功能, **其中内部函数可以访问外部（封闭）的变量, 是函数和函数变量所处作用域的混合**

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

- 内存泄漏: 由于闭包使用过度而导致的内存占用无法释放的情况, 我们称之为：**内存泄露**。内存泄漏可能会导致应用程序卡顿或者崩溃。

解决方案:

- 使用严格模式
- 关注 `DOM` 生命周期, 在销毁阶段记得解绑相关事件
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

   1. 新生成了一个对象
   2. 添加 `_proto_`,  链接到原型
   3. 绑定 this
   4. 返回新对象

   ```js
   var peo = new person("xiao ming"); // this 绑定到 peo 对象
   console.log(peo.name); // "xiao ming"
   ```

4. 通过 call/apply/bind 调用, 主要可以控制this的指向

   1. call ( this, arg1,arg2,...,args ) 参数直接传递
   2. apply ( this, args[] ) 参数以数组形式传递
   3. bind ( target )  将this绑定到target上

此外, 箭头函数是没有this的, 这个函数中的 `this` 只取决于他外面的第一个不是箭头函数的函数的 `this`

#### 7. 原型链和继承

原型: 函数的`prototype`属性和的对象的`__proto__` 属性. `prototype`就是构造函数的一个指针, 指向一个存有一些**公用方法和共享属性**的**对象**. 其中由构造函数所构造的实例对象, 他们的`__proto__`会指向构造函数的`prototype`

概括:

- `Object` 是所有对象的爸爸, 所有对象都可以通过 `__proto__` 找到它
- `Function` 是所有函数的爸爸, 所有函数都可以通过 `__proto__` 找到它
- `Function.prototype` 和 `Object.prototype` 是两个特殊的对象, 他们由引擎来创建
- 除了以上两个特殊对象, 其他对象都是通过构造器 `new` 出来的
- 函数的 `prototype` 是一个对象, 也就是原型
- 对象的 `__proto__` 指向原型,  `__proto__` 将对象和原型连接起来组成了原型链. 访问一个对象的属性时, 先在基本属性中查找, 如果没有, 再沿着`__proto__`这条链向上找, 这就是原型链.



继承: 可以使得子类具有父类的属性和方法或者重新定义、追加属性和方法等. 继承是类与类之间的关系，不是对象与对象之间的关系.

所以js实际上并没有真正意义上的类的概念,  但是js很灵活, 可以通过许多方式实现类似继承的效果, 我比较了解和最常用的就是原型继承.

所谓原型继承就是依靠原型链实现的共享属性和方法的继承方式. 通过原型链将子类原型和父类原型关联起来.

问题: 

1. 在创建子类实例的时候，不能向超类型的构造函数中传递参数。

2. 这样创建的子类原型会包含父类的实例属性，造成引用类型属性同步修改的问题。

解决办法就是实现**寄生组合模式**.

[原型链及继承](https://mitianyi.gitbook.io/frontend-interview-guide/javascript-basics/prototype-chain-and-inheritance#ji-cheng)

[解析原型中的各个难点](https://github.com/KieSun/Dream/issues/2#)

```js
//构造函数实现继承
function Phone(brand, price){
    this.price = price; this.brand = brand;
}
Phone.prototype.call = function(){console.log("打电话")};

//子类
function SmartPhone(brand, price, color, size){
    Phone.call(this, brand, price);
    this.color = color; this.size = size;
}
//设置原型
SmartPhone.prototype = new Phone;
SmartPhone.prototype.constructor = SmartPhone;
SmartPhone.prototype.photo = function(){console.log("拍照")};
//实例化
const iphone = new SmartPhone('apple',5999,red,'5.2inch');
```



## ES6

#### 1. let const

```
1. 变量不能重复声明
2. 引入块级作用域, 
3. 没有变量提升, 存在展示型死区
4. 不会影响作用域链的查询
```

#### 2. const

```
1. 对于常量声明一定要赋初始值
2. 规范使用大写进行命名
3. 常量的值不能修改(地址不能变化), 也就是说对于数组和对象, 可以修改元素, 但不能重新指向
4. 形成块级作用域
```

#### 3. 解构

```
1. 数组结构解构
const F4 = ['a', 'b', 'c', 'd'];
let [a, b, c, d] = F4;

2. 对象结构
const mugu = {
    age: 21,
    name: oliver,
    skill: function(){ console.log("skate")}
}
let {skill} = mugu
skill()
```

#### 4. 模板字符串

```
1. 可以直接出现换行赋
let str = `<ul>
			 <li>1</li>
			 <li>2</li>	
		   </ul>`;

2. 可以拼接变量
let param = "dio";
let str = `ko no ${dio} da! `;
```

#### 5. 简化对象写法

```
可以直接在对象大括号中写入变量和函数, 省略掉键名.
let age = 21, name = 'oliver';
let mugu ={
    age, name
}
```

#### 6. 箭头函数

```
let fn = ( a, b) ={
    return a+b;
}
箭头函数主要适用于不需要this的情况,如定时任务.
1. this 是静态的, 始终指向函数声明时 this 的值, 即就近作用域
2. 不能作为构造函数实例化对象.
3. 不能是用 arguments 变量
4. 箭头函数简写
```

#### 7. 函数参数的默认设置

```
1. 形参初始值设置
2. 与解构赋值结合使用
function demo( {host, port='1080'} ){
    console.log(host)
}
demo({ host: '127.0.0.1' })
```

#### 8. rest参数

```
传递rest参数, 相当于加强版的arguments, 注意要放在最后.
function demo(a, b,...args){
    console.log(args)
}
demo(1,2,3,4,5) // 1,2,[3,4,5]
```

#### 9. 扩展运算符

```
和rest类似, 但是主要是用于扩展传入的形参为独立的参数, 会将参数数组返回为序列
主要可以用于:
1. 数组拼接, 和concat效果一样
2. 数组复制, 深拷贝
3. 转化伪数组
let arr = [1,2,3]   //默认输出为{0:[1,2,3]}
function demo(){ console.log(arguments)}
demo(...arr)		//输出为{ 0:1, 1:2, 3:3 }
```

#### 10. Symbol

```
类似与字符串
1. 值是唯一的
2. 不能和其他数据类型进行运算
3. 无法用for...in遍历但可以使用 reflect.ownKeys 获取对象的所有键名

主要是给对象添加独一无二的属性和方法, 有安全性保障, 不会覆盖原来的方法.
let obj1 = {...}
let obj2 = { mymethod: Symbol() }
obj1[obj2.mymethod] = function(){...}
```

#### 11. Iterator 迭代器 / for...of循环

```
1. ES6 增加了 for..of 循环, Iterator 接口为 for of 提供支持. 实际上就是对象的一个属性.
原生具备 Iterator 接口的数据类型包括:
Array;  Arguments;
Set;  String;
Map;
NodeList;
TypeArray;

2. for...in循环保存键名; for...of循环保存键值.
for(let index in arr) //index = 0,1,2...
for(let item of arr)  //item = a, b, c......

3. Iterator工作原理
- 创建一个指针对象, 指向数据结构的起始位置
- 第一次调用next, 自动指向第一个成员, 之后不断调用指导指向最后一个成员
- 每调用next就返回包含value和done的属性和对象
```

```js
//使用场景: 面向对象实现自定义遍历对象属性.
const obj = {
    name: 'obj',
    arr: ['a', 'b', 'c'],
    [Symbol.iterator](){	//实现接口
        //自定义遍历行为, 重写next方法
        let index = 0;
        return{
            next: ()=>{
                if( index < this.arr.length){
                    const result = {value: this.arr[index], done: false}
                    index++;
                    return result;	//next 要求返回value和done属性的对象.
                }
                else{ return {value: undefined, done: true}};
            }
        }
    }
};

//对对象进行遍历:
for(let item of obj){ console.log(item); }
```

#### 12. 生成器

```
1. 生成器是一种特殊的函数用以实现异步编程
2. 不是常规的回调函数实现的异步编程, 利用迭代器解决了回调地狱问题
```

```js
// 操作实例
function getUser(){
    //异步操作
    setTimeout(()=>{
        let data = '用户';
        iterator.next(data);	//第二次调用next, 他的data是上一次next的返回结果
    },1000)
}
function getOrder(){
    setTimeout(()=>{
        let data = '订单';
        iterator.next(data);
    },1000)
}
function getGood(){
    setTimeout(()=>{
        let data = '商品';
        iterator.next(data);
    },1000)
}

function * gen(){
    //使用yield划分为不同的代码块, 并由next控制向下执行.
    let userdata = yield getUser();
    console.log(userdata);
    let order = yield getOrder();
    console.log(order);
    let goods = yield getGood();
    console.log(goods);
}
let iterator = gen();
iterator.next();	//第一次调用next, 利用next()方法, 可以接收参数
```

#### 13. Promise

```
promise是一个异步编程解决方案, 用以解决回调地狱和回调函数名不统一的问题. 可以以同步代码的形式将异步代码的嵌套表现出来, 避免了层层嵌套的回调函数

本质上是一个构造函数. 接受一个函数参数fn(resolve, reject), 进行异步处理后调用相应的函数, 让promise的状态从pending到fullfill或者rejected. 并返回一个新的promise对象. 
promise对象原型上定义了then方法, 可以根据两个状态变化注册相应的回调函数, 这个回调函数属于微任务，会在本轮事件循环的末尾执行。

缺点:
1. 无法取消, 一旦新建就会执行无法中途取消
2. 处于pending状态时无法得知当前进展如何.
```

```js
//新建
const promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
//回调
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

[阮一峰](https://es6.ruanyifeng.com/#docs/promise#Promise-%E7%9A%84%E5%90%AB%E4%B9%89)

[promise](https://www.bukun.top/2020/06/18/promise%E4%B8%8Ejsonp/)

#### 14. set

```
set--集合, 是es6的一种新的数据结构, 它的是内容不重复的数组.( 去重 )
常见方法有:
1. 元素个数 size()
2. 添加元素 add()
3. 删除元素 delete()
4. 检测包含 has()
5. 清空 clear()
6. 迭代器接口 for of
```

```js
//实例1 数组去重: set+扩展运算符
let arr = [1,2,3,4,3,2,1];
let result = [...new Set(arr)];

//实例2 交集
let arr2 = [4,5,6];
let result = [...arr2].filter(item = >{
    let set2 = new Set(arr2);
    if(set2.has(item)){ return true; }else {return false;}
});
```

#### 15. map

```
map是类似于对象的数据结构, 提供键值对的集合. 特别的, 它的'键'不限于字符串, 实现了迭代器接口. 常用方法:
1. set()/get() set(key, value)
2. size
3. clear()
4. for of
```

#### 16. class

```
本质上就是构造函数的语法糖, 让对象的原型写法更加清晰, 更像面向对象编程
```

```js
//class实现继承
class Phone{
    static name="类自己的, 实例不可访问静态类"
	constructor(brand, price){};
    call(){};	//不能使用对象语法, 即call: funciton(){};
}

class SmartPhone extends Phone{
    constructor(brand, price, color, size){
        super(brand, price);
        this.color = color;
        this.size = size;
    };
    photo(){
        console.log('拍照')
    }
    //override
    call(){
        console.log('高清通话')
    }
    //geter&setter
    get price(){}
    set price(newPrice){ newPrice }
}
```