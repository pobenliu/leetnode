# Maximum Subarray II

 Maximum Subarray II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximum-subarray-ii/) )

```
Description
Given an array of integers, find two non-overlapping subarrays which have the largest sum.
The number in each subarray should be contiguous.
Return the largest sum.

Notice
The subarray should contain at least one number

Example
For given [1, 3, -1, 2, -1, 2], 
the two subarrays are [1, 3] and [2, -1, 2] or [1, 3, -1, 2] and [2], 
they both have the largest sum 7.

Challenge 
Can you do it in time complexity O(n) ?
```

### 解题思路

题目要求找到两个不重叠的子数组和的最大值，关键在于不重叠。考虑如何利用这个条件将题目转化为 Maximum Subarray ，也即在数组中寻找最大子数组。可以将一个数组用隔板划分为两个，然后分别在左、右两个子数组中寻找最大子数组。

如果在遍历划分点的时候，同时寻找两个最大子数组，双重循环的时间复杂度为 `O(n^2)` 。我们可以先寻找不同划分左数组的最大子数组，将和存储在数组中；然后寻找不同划分右数组的最大子数组，将和存储在数组中；最后两个数组分别求和取最大值即可。

##### 易错点：

> 1. 最后求和的时候需注意，由于有划分的存在，所以两个数组的索引差 `1` 。
> 2. 在求最大值、最小值时，变量的初始化需要使用 `Integer.MIN_VALUE` 或 `Integer.MAX_VLUAE`。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: An integer denotes the sum of max two non-overlapping subarrays
     */
    public int maxTwoSubArrays(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return 0;
        }
        
        int curSum = 0;
        int minSum = 0;
        int max = Integer.MIN_VALUE;
        int[] maxSumLeft = new int[nums.size()];
        for (int i = 0; i < nums.size(); i++) {
            curSum += nums.get(i);
            max = Math.max(max, curSum - minSum);
            minSum = Math.min(minSum, curSum);
            maxSumLeft[i] = max;
        }
        
        curSum = 0;
        minSum = 0;
        max = Integer.MIN_VALUE;
        int[] maxSumRight = new int[nums.size()];
        for (int i = nums.size() - 1; i >= 0; i--) {
            curSum += nums.get(i);
            max = Math.max(max, curSum - minSum);
            minSum = Math.min(minSum, curSum);
            maxSumRight[i] = max;
        }
        
        max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.size() - 1; i++) {
            max = Math.max(max, maxSumLeft[i] + maxSumRight[i + 1]);
        }
        
        return max;
    }
}
```



### 参考

1. [Maximum Subarray II | 九章算法](http://www.jiuzhang.com/solutions/maximum-subarray-ii/)