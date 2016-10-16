#  Find Minimum in Rotated Sorted Array

Find Minimum in Rotated Sorted Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array/#) )

```
Description
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
Find the minimum element.

Notice
You may assume no duplicate exists in the array.

Example
Given [4, 5, 6, 7, 0, 1, 2] return 0
```

### 解题思路

参考题目 Search in Rotated Sorted Array ，在旋转数组中寻找最小值，可能出现以下三种情况：

![](http://ww3.sinaimg.cn/mw690/600e6311gw1f8twrukdafj21b30drtbh.jpg)

其中，在情况（a）和情况（c），end 都需要往中间移动，而在情况（b），start 需要往中间移动，由此可以得出我们的解法如下。

##### 算法复杂度

- 时间复杂度： `O(logn)` 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] nums) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < nums[end]) {
                end = mid;
            } else  {
                start = mid;
            } 
        }
        if (nums[start] < nums[end]) {
            return nums[start];
        }
        return nums[end];
    }
}
```

