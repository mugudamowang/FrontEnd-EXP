### js执行机制

- js是单线程的语言, 它通过webapi, 事件循环和队列回调模拟多线程

- 事件循环机制 eventloop

  - 事件类型 
    - 同步任务: 处理紧急的基础的内容, 如页面骨架和元素的渲染
    - 异步任务: 占用资源的, 耗时久的, 如视频图片等媒体资源的的加载
  - 执行过程
    1. 判断任务类型
    2. 进入主线程或者eventtable上的函数进行注册
    3. 指定的任务执行完毕, 将eventalbe中对应函数移入任务列表eventqueue
    4. 主线程执行完毕( 栈空时 )开始读取任务列表的结果, 进入主线程执行
  - 将事件执行重复循环就是event loop
  - eg: 

  ```
  let data = [];
  $.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
  })
  console.log('代码执行结束'); 
  ```

  - 过程
    1. 异步任务ajax进入event table, 注册回调函数success
    2. 执行同步任务console.log("代码执行结束")
    3. ajax事件完成, 回调汉书success进入event queue
    4. 主线程为空, 进入event queue读取回调函数success并执行
  - setTimeout函数并不一定是按时执行
    - setTimeout是一个异步函数, 执行过程符合注册并计时-计时结束并入队-等待执行-主线程执行
    - 关键点是, 如果主线程的函数比较耗时, 即使计时结束了, 也要等待主线程同步任务结束.
    - setTimeout(fn, 0)指的是当主线程空闲时就立即执行.
  - setInterval
    - 和setTimeout差不多, 只不过是循环地向event queue置入注册的函数
    - 既然不是一定按时执行, 那么其中地间隔也不是一定规律的.
    - 比如主线程耗时5s, 而定时器3s置入一次回调函数到event queue, 那么而第3s后第二个被置入, 那第一个回调在第5s后执行, 第二个则第6s执行
  - 微任务和宏任务
    - 微任务: promise, process.nextTick
    - 宏任务: setTimeout, setInterval, 整体代码script
    - 执行顺序: 宏任务-->微任务
      - 进入一个整体地sccript块, 将其作为一个宏任务运行
      - 执行其中地代码并将微任务和宏任务入队(微\宏人物event queue)
      - 先执行微任务, 执行完毕再执行宏任务.
      - 执行宏任务中的代码并查找宏\微任务入队
      - 重复这个过程直到对为空即执行完毕