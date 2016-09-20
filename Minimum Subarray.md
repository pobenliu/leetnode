#  Minimum Subarray

 Minimum Subarray ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/minimum-subarray/) )

```
Description
Given an array of integers, find the subarray with smallest sum.
Return the sum of the subarray.

Notice
The subarray should contain one integer at least.

Example
For [1, -1, -2, 1], return -3.
```

### 解题思路

#### 一、前缀和

参考 Maximum Subarray 的前缀和方法。

Java 实现

```java
public class Solution {
    /**
     * @param nums: a list of integers
     * @return: A integer indicate the sum of minimum subarray
     */
    public int minSubArray(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return 0;
        }
        
        int curSum = 0;
        int minSum = Integer.MAX_VALUE;
        int maxSum = 0;
        for (int i = 0; i < nums.size(); i++) {
            curSum += nums.get(i);
            minSum = Math.min(minSum, curSum - maxSum);
            maxSum = Math.max(maxSum, curSum);
        }
        
        return minSum;
    }
}
```



#### 二、前缀和 II



Java 实现

```java
public class Solution {
    /**
     * @param nums: a list of integers
     * @return: A integer indicate the sum of minimum subarray
     */
    public int minSubArray(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return 0;
        }
        
        int curMin = nums.get(0);
        int minRes = nums.get(0);
        
        for (int i = 1; i < nums.size(); i++) {
            curMin = Math.min(nums.get(i), curMin + nums.get(i));
            minRes = Math.min(curMin, minRes);
        }
        
        return minRes;
    }
}
```



### 参考

1. [LintCode-Minimum Subarray | LiBlog](http://www.cnblogs.com/lishiblog/p/4196837.html)

