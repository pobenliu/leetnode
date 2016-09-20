#  Maximum Subarray Difference

 Maximum Subarray Difference  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximum-subarray-difference/) )

```
Description
Given an array with integers.
Find two non-overlapping subarrays A and B, which |SUM(A) - SUM(B)| is the largest.
Return the largest difference.

Notice
The subarray should contain at least one number

Example
For [1, 2, -3, 1], return 6.

Challenge 
O(n) time and O(n) space.
```

### 解题思路

本次可参考题目 Maximum Subarray I / II 的做法，使用隔板划分为左右两个子数组，然后依次求取左边数组的最大子数组、最小子数组，右边数组的最大子数组、最小子数组，然后依次比较其差值的绝对值。

这道题目就是体力活。

易错点：

> 1. 在求局部的最小值 `minSum` 时，比较的是当前的数字 `nums[i]` ，以及 `minSum‘ + nums[i]` 。比如 `minSum’` 为正数时，可以舍弃。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: An integer indicate the value of maximum difference between two
     *          Subarrays
     */
    public int maxDiffSubArrays(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int len = nums.length;
        
        int minSum, maxSum;
        int[] minResLeft = new int[len];
        int[] maxResLeft = new int[len];
        minSum = maxSum = minResLeft[0] = maxResLeft[0] = nums[0];

        for (int i = 1; i < len; i++) {
            minSum = Math.min(nums[i], minSum + nums[i]);
            minResLeft[i] = Math.min(minResLeft[i - 1], minSum);
            maxSum = Math.max(nums[i], maxSum + nums[i]);
            maxResLeft[i] = Math.max(maxResLeft[i - 1], maxSum);
        }
        
        int[] minResRight = new int[len];
        int[] maxResRight = new int[len];
        minSum = maxSum = minResRight[len - 1] = maxResRight[len - 1] = nums[len - 1];
      
        for (int i = len - 2; i >= 0; i--) {
            minSum = Math.min(nums[i], minSum + nums[i]);
            minResRight[i] = Math.min(minResRight[i + 1], minSum);
            maxSum = Math.max(nums[i], maxSum + nums[i]);
            maxResRight[i] = Math.max(maxResRight[i + 1], maxSum);
        }
        
        int result = 0;
        for (int i = 0; i < len - 1; i++) {
            result = Math.max(result, Math.max(Math.abs(minResLeft[i] - maxResRight[i + 1]), Math.abs(maxResLeft[i] - minResRight[i + 1])));
        }
        
        return result;
    }
}
```



### 参考