<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Computer Science Basic](#computer-science-basic)
  - [操作系统](#%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)
    - [1. 线程与进程](#1-%E7%BA%BF%E7%A8%8B%E4%B8%8E%E8%BF%9B%E7%A8%8B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Computer Science Basic

### 操作系统

#### 1. 线程与进程

*线程*与*进程*是操作系统里面的术语，简单来讲，每一个应用程序都有一个自己的 *进程*。

操作系统会为这些*进程*分配一些执行资源，例如内存空间等。

在*进程*中，又可以创建一些*线程*，他们共享这些内存空间，并由操作系统调用，以便并行计算.

eg: 在浏览器这个进程里, 创建JS引擎线程\GUI渲染线程\浏览器事件线程等, 进程里的线程相互协作完成任务.

