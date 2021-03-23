## Algorithm

### 排序算法--leetcode912

1. 快速排序

   

2. 冒泡排序

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

   





