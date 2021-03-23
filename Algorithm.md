<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Algorithm & DataStructure](#algorithm--datastructure)
  - [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
    - [1. 逻辑结构](#1-%E9%80%BB%E8%BE%91%E7%BB%93%E6%9E%84)
    - [2. 数据类型与抽象数据类型](#2-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E4%B8%8E%E6%8A%BD%E8%B1%A1%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
  - [排序算法--leetcode912](#%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95--leetcode912)
    - [1. 快速排序](#1-%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)
    - [2. 冒泡排序](#2-%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Algorithm & DataStructure

### 数据结构

#### 1. 逻辑结构

线性结构: 只有一个前趋\后继元素的有序数据元素集合. 常见的有线性表, 栈, 队列 (1对1)

非线性结构: 树结构或者图结构, 有1个或则多个前驱, 有0个或者多个后继. 常见的有二维数组, 树, 图, 广义表 ( 1对多, 多对多 )

2. 存储结构

   | 顺序存储结构                           | 链接存储结构                                     |
   | -------------------------------------- | ------------------------------------------------ |
   | 相邻元素的地址也相邻 (逻辑物理相统一). | 元素随机存放,一部分为数据域,一部分为地址域(指针) |
   | 存储密度大, 地址静态分配               | 存储密度小, 地址动态分配                         |
   | 插入或者删除不方便, 查找快             | 长度不固定, 查找难, 插入删除方便                 |


#### 2. 数据类型与抽象数据类型

| 数据类型( 数据结构为分子, 数据类型为原子 ) | 抽象数据类型                        |
| ------------------------------------------ | ----------------------------------- |
| 类型 (Numbe String Array......)            | 逻辑概念\数学模型                   |
| 定义在该类型上的一系列操作                 | 操作集合                            |
| eg:  可进行算术运算的整数定义为int数据     | eg: 定义抽象数据类型student, 接口等 |



### 排序算法--leetcode912

#### 1. 快速排序



#### 2. 冒泡排序

```js
// 冒泡排序
// 时间复杂度：O(N^2)  6592ms    5.08%
// 空间复杂度：O(1)    41.3MB    95.83%
// 描述: 将当前值与余下的值比较并交换位置, 直到每个值完成比较.
var sortArray = function (nums) {
    let i, temp;
    for (j = 0; j < nums.length-1; j++) {		//需要循环的次数是length-1, 因为最后一个不用比较
        for (i = 0; i < nums.length - j; i++) {	//每个数比较的次数是length-j, 已经比较不需要再次比较.
            if (nums[i] > nums[i + 1]) {
                temp = nums[i];
                nums[i] = nums[i + 1];
                nums[i + 1] = temp;
            }
        }
    }
    return nums;
};
```





