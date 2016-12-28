# Minimum Subarray

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



#### 二、动规

1. 定义状态：定义一维状态变量 `f[i]` ，表示以第 `i` 个数字 `nums[i]` 为结尾的子数组的最小和。
2. 定义状态转移函数：对于当前状态 `f[i]` ，其上一个状态为 `f[i - 1]` ，
   - 当 `f[i - 1] >= 0` 时， 舍弃之前的部分，`f[i]` 取第 `i` 个数字 `nums[i]` 即可。
   - 当 `f[i - 1] < 0` 时，连续求和即可。
   - 根据上述讨论可得 `f[i] = f[i - 1] >= 0 ? nums[i] : f[i - 1] + nums[i]`（或者直接比较两者大小取较小值即可）。考虑到当前状态只取决于上一个状态，所以使用一个变量保存状态可以节省内存开销。
3. 定义起点：
   - `f[0] = 0` 。
4. 定义终点：最终结果即为 `f[i], i = 0,1,...n` 的最小值。可使用一个全局变量保存局部变量的最小值。

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