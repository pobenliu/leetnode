#  Maximum Subarray III

 Maximum Subarray III  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximum-subarray-iii/) )

```
Description
Given an array of integers and a number k, find k non-overlapping subarrays which have the largest sum.
The number in each subarray should be contiguous.
Return the largest sum.

Notice
The subarray should contain at least one number

Example
Given [-1,4,-2,3,-2,3], k=2, return 8
```

### 解题思路

#### 一、动态规划

1. 定义状态：定义二维状态变量 `f[i][j]` ，表示前 `i` 个数字，分为 `j` 个划分时，得到的最大和。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，其上一个状态可能为 `f[m][j - 1]` ，考虑到 `i >= j` ， `m` 的取值范围是 `j - 1 <= m < i` ，则有`f[i][j] = max{f[m][j - 1] + maxSubArray(m + 1, i)}` 。
3. 定义起点：
   - `f[i][0]` 从前 `i` 个数字，得到 `0` 个划分，得到的最大和为 `0` 。
4. 定义终点：最终结果即为 `f[n][k]` 。

疑问：

> 1. 为何在求数组的最大子数组时，`m` 从 `i - 1` 到 `j - 1` （从右向左）遍历，在 [LintCode-Maximum Subarray III](http://www.cnblogs.com/lishiblog/p/4183917.html) 一文中给出的解释是这样在 `m -> m - 1` 时，可以利用当前的 `max` 求解 `m - 1 -> i - 1` 的最大值。主页君表示还不是想得很清楚，从左向右得到的结果是错的。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: An integer denote to find k non-overlapping subarrays
     * @return: An integer denote the sum of max k non-overlapping subarrays
     */
    public int maxSubArray(int[] nums, int k) {
        if (nums == null || nums.length ==0) {
            return 0;
        }
        
        int n = nums.length;
        // state f[i][j] : select j subarrays from the first i elements, the max sum we can get
        int[][] f = new int[n + 1][k + 1];
        
        // initialize
        // default : f[i][j] = 0
        
        for (int j = 1; j <= k; j++) {
            for (int i = j; i <= n; i++) {
               f[i][j] = Integer.MIN_VALUE;
               
               int curSum = 0;
               int max = Integer.MIN_VALUE;
               // why m goes from right to left. when goes from left to right get wrong answer
               for (int m = i - 1; m >= j - 1; m--) {
                   curSum = Math.max(nums[m], curSum + nums[m]);
                   max = Math.max(curSum, max);
                   f[i][j] = Math.max(f[i][j], f[m][j - 1] + max);
               }
            }
        }
        
        return f[n][k];
    }
    
}
```



### 参考

1. [LintCode-Maximum Subarray III | LiBlog](http://www.cnblogs.com/lishiblog/p/4183917.html)

