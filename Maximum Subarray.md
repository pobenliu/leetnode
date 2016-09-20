#  Maximum Subarray

 Maximum Subarray  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximum-subarray/) )

```
Description
Given an array of integers, find a contiguous subarray which has the largest sum.

Notice
The subarray should contain at least one number.

Example
Given the array [−2,2,−3,4,−1,2,1,−5,3], the contiguous subarray [4,−1,2,1] has the largest sum = 6.

Challenge 
Can you do it in time complexity O(n)?
```

### 解题思路

#### 一、动态规划

1. 定义状态：定义一维状态变量 `f[i]` ，表示以第 `i` 个数字 `nums[i - 1]` 为结尾的子数组的最大和。
2. 定义状态转移函数：对于当前状态 `f[i]` ，其上一个状态为 `f[i - 1]` ，
   - 当 `f[i - 1] <= 0` 时， 舍弃之前的部分，`f[i]` 取第 `i` 个数字 `nums[i - 1]` 即可。
   - 当 `f[i - 1] > 0` 时，连续求和即可。
   - 根据上述讨论可得 `f[i] = f[i - 1] <= 0 ? nums[i - 1] : f[i - 1] + nums[i - 1]` 。考虑到当前状态只取决于上一个状态，所以使用一个变量保存状态可以节省内存开销。
3. 定义起点：
   - `f[0] = 0` 。
4. 定义终点：最终结果即为 `f[i], i = 0,1,...n` 的最大值。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        // state
        int curSum = nums[0];
        int maxSum = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            curSum = curSum <= 0 ? nums[i] : curSum + nums[i];
            maxSum = Math.max(curSum, maxSum);
        }
        
        return maxSum;
    }
}
```



#### 二、前缀和

对前缀和 `sum[i] = nums[0] + nums[1] + … + nums[i]` ，

那么第 `i + 1` 到 `j + 1` 个连续数字的和为 `sum[i … j] = sum[j] - sum[i]` 。

以上是使用前缀和进行解题的基础。

具体到本题，以第 `i` 个数字结尾的子数组的最大值，等于当前的前缀和 `curSum` 减其子数组的最小前缀和 `minSum` 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int maxSum = Integer.MIN_VALUE;
        int curSum = 0, minSum = 0;
        for (int i = 0; i < nums.length; i++) {
            curSum += nums[i];
            maxSum = Math.max(maxSum, curSum - minSum);
            minSum = Math.min(minSum, curSum);
        }
        
        return maxSum;
    }
}
```



### 参考

1. [[LeetCode] Maximum Subarray | 喜刷刷](http://bangbingsyb.blogspot.jp/2014/11/leetcode-maximum-subarray.html)
2. [Maximum Subarray | 九章算法](http://www.jiuzhang.com/solutions/maximum-subarray/)
3. [Maximum Subarray — LeetCode | Code Ganker](http://blog.csdn.net/linhuanmars/article/details/21314059)

