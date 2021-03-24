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

```js
//辅助函数--增加可读性, 抽取公用函数
function checkIsArray(nums) {
    if (!array || array.length <= 2) return
}
function swapValue(nusm, left, right) {
    let temp = nums[right]
    nums[right] = nums[left]
    nums[left] = temp
}
```

#### 1. 插入排序

```js
//插入
// 时间复杂度：O(N^2)  976 ms    31.83 %
// 空间复杂度：O(1)    42.7MB    63.59 %
// 描述: 1. 第一和第二元素进行大小比较并交换位置 2. 选取下一个元素向前对比, 比前面值小的化就的话交换位置, 以此类推.
var sortArray = function (nums) {
    checkIsArray(nums);
    
    for (let i = 1; i < nums.length; i++) {		//第一层循环次数length-i(length-1), 设i为1让第二层判断方便
        for (let j = i - 1; j >= 0 && nums[j] > nums[j + 1]; j--) //第二层循环, 当比较到第一个元素时停止向前比较
            swapValue(nums, j, j + 1);							//通过j--来设置向前. 如果第j+1个比第j个小就往前挪
    }
    
    return nums;
}
```

#### 2. 冒泡排序

```js
// 冒泡排序
// 时间复杂度：O(N^2)  6592ms    5.08%
// 空间复杂度：O(1)    41.3MB    95.83%
// 描述: 将当前值与余下的值比较并交换位置, 直到每个值完成比较.
var sortArray = function (nums) {
    
    for (let j = 0; j < nums.length-1; j++) {		//需要循环的次数是length-1, 因为最后一个元素已经是最大数了
        for (let i = 0; i < nums.length - j; i++) {	//每个数比较的次数是length-j, 已经比较不需要再次比较.
            if (nums[i] > nums[i + 1]) {
                swapValue(nums, i, i+1)
            }
        }
    }
    
    return nums;
};
```

#### 3. 选择排序

```js
//选择排序
// 时间复杂度：O(N^2)  1236ms    28.52%
// 空间复杂度：O(1)    42.2MB     90.17%
// 描述: 1. 记录一个最小值的下标, 内层循环寻找这个下标, 之后在外层循环替换最小值, 下次可以从第二小的索引值开始, 以此循环.
var sortArray = function (nums){
    checkIsArray(nums);
    let minIndex;		//记录最小值下标

    for(let i=0; i<nums.length-1;i++){
        minIndex = i	//最小值下标
        for(let j=i+1; j<nums.length; j++){
            if(nums[minIndex]>nums[j]){
                minIndex = j //寻找length-i个数中的最小值的下标
            }
        }
        swapValue(nums, minIndex, i);	//交换最小值到前面
    }
    return nums;
}
```

#### 4. 快速排序

```js
// 快速排序
// 时间复杂度： O(logN)  156 ms    48.84 %
// 空间复杂度：O(1)   50 MB     90.17%
// 描述:	1. 使用分治的思想进行排序处理 2. 选定任意基准, 将大于它的移到右边, 小于他的移到左边 3. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止
// 原理: 一个排完序的数组, 任意一个数的右边都是大于它的, 任意一个数的左边都是小于他的, 以此反推.
var sortArray = function (nums) {

    if (nums.length <= 1) { return nums; }		//当数组被分割完时, 即所有子集只有一个元素
    var pivotIndex = Math.floor(nums.length / 2); //选取中间作为基准
    var pivot = nums.splice(pivotIndex, 1)[0];
    var left = [];		
    var right = [];
    
    for (var i = 0; i < nums.length; i++) {
        if (nums[i] < pivot) {
            left.push(nums[i]);		//存储到相应的位置
        } else {
            right.push(nums[i]);
        }
    }
    
    return sortArray(left).concat([pivot], sortArray(right));	//拼接结果
};
//来自阮一峰的
```







