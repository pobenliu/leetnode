#  Median

 Median  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/median/) )

```
Given a unsorted array with integers, find the median of it.
A median is the middle number of the array after it is sorted.
If there are even numbers in the array, return the N/2-th number after sorted.

Example
Given [4, 5, 1, 2, 3], return 3.
Given [7, 9, 4, 5], return 5.

Challenge 
O(n) time.
```

### 解题思路

最简单的方法是先对数组排序，然后取中位数。快排和归并排序等基于比较的排序算法的时间复杂度为 `O(n)`，桶排序、计数排序、基数排序等线性排序算法对数据有一定限制，且空间复杂度较高。

由于只需要找出中位数即可，所以可以参考快速排序中的 partition 操作，根据 pivot 元素将数组划分为左小右大的两部分，然后对包含中位数的部分继续查找即可。当 pivot 元素的下标等于数组长度的一半时，即为中位数。

##### 易错点

> 1. 一个数组的中位数位置 `k = (length - 1)/2`。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers.
     * @return: An integer denotes the middle number of the array.
     */
    public int median(int[] nums) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        return helper(nums, 0, nums.length - 1, (nums.length - 1)/2);
    }
    
    private int helper(int[] nums, int start, int end, int size) {
        if (start >= end) {
            return nums[end];
        }
        int m = start;
        for (int i = start + 1; i < end + 1; i++) {
            if (nums[i] < nums[start]) {
                m++;
                swap(nums, i, m);
            }
        }
        swap(nums, start, m);
        
        if (m == size) {
            return nums[m];
        } else if (m  > size) {
            return helper(nums, start, m - 1, size);
        } else {
            return helper(nums, m + 1, end, size);
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}

```



### 参考

1. [Median | 数据结构与算法/leetcode/lintcode题解](https://algorithm.yuanbin.me/zh-hans/integer_array/median.html)