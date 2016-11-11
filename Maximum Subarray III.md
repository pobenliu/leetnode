# Maximum Subarray III

 Maximum Subarray III  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximum-subarray-iii/) )

```
Description
Given an array of integers and a number k, 
find k non-overlapping subarrays which have the largest sum.
The number in each subarray should be contiguous.
Return the largest sum.

Notice
The subarray should contain at least one number

Example
Given [-1,4,-2,3,-2,3], k=2, return 8
```

### 解题思路

本题目是典型的划分型动态规划。

#### 一、常规解法

1. 定义状态：定义二维状态变量 `f[i][j]` ，表示前 `i` 个数字，取 `j` 个子数组时，得到的最大和。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，由于划分是任意的，所以不止和当前的元素相关。其上一个状态可能为 `f[m][j - 1]` ，考虑到 `i >= j` ， `m` 的取值范围是 `j - 1 <= m < i` ，则有 `f[i][j] = max{f[m][j - 1] + maxSubArray(m + 1, i)}` 。
3. 定义起点：
   - `f[i][0]` 从前 `i` 个数字， `0` 个子数组，得到的最大和为 `0` 。
   - `f[0][i]` 从前 `0` 个数字， `i` 个子数组，得到的最大和为 `0`。
4. 定义终点：最终结果即为 `f[n][k]` 。

##### 算法复杂度

- 时间复杂度：`O(k*n^3)`。
- 空间复杂度：`O(kn)`。

##### 易错点

> 1. 涉及划分时，需要注意几个自变量 `i, j, m` 之间的大小关系。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: An integer denote to find k non-overlapping subarrays
     * @return: An integer denote the sum of max k non-overlapping subarrays
     */
    public int maxSubArray(int[] nums, int k) {
        if (nums == null || nums.length == 0 
            || k <= 0 || k > nums.length) {
            return -1;
        }
        
        int n = nums.length;
        // status
        int[][] f = new int[n + 1][k + 1];
        // initialize
        
        for (int j = 1; j <= k; j++) {
            for (int i = j; i <= n; i++) {
                f[i][j] = Integer.MIN_VALUE;
                for (int m = j-1; m < i; m++) {
                    f[i][j] = Math.max(f[i][j], f[m][j-1] + maxSubArray(nums, m, i-1));
                }
            }
        }
        
        return f[n][k];
    }
    
    private int maxSubArray(int[] nums, int start, int end) {
        int curSum = 0;
        int maxSum = Integer.MIN_VALUE;
        for (int i = start; i <= end; i++) {
            curSum = Math.max(curSum + nums[i], nums[i]);
            maxSum = Math.max(maxSum, curSum);
        }
        
        return maxSum;
    }
}

```

时间复杂度太高，进一步思考，是否可以简化求数组最大和的操作，考虑到一个数组从两个方向求最大和子数组的结果是一样的，所以把 `maxSubArray` 函数和自变量 `m` 循环结合在一起，将时间复杂度将至 `O(k*n^2)`。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: An integer denote to find k non-overlapping subarrays
     * @return: An integer denote the sum of max k non-overlapping subarrays
     */
    public int maxSubArray(int[] nums, int k) {
        if (nums == null || nums.length == 0 
            || k <= 0 || k > nums.length) {
            return -1;
        }
        
        int n = nums.length;
        // status
        int[][] f = new int[n + 1][k + 1];
        // initialize
        
        for (int j = 1; j <= k; j++) {
            for (int i = j; i <= n; i++) {
                f[i][j] = Integer.MIN_VALUE;
                
                int curSum = 0;
                int maxSum = Integer.MIN_VALUE;
                for (int m = i-1; m >= j-1; m--) {
                    curSum = Math.max(curSum + nums[m], nums[m]);
                    maxSum = Math.max(maxSum, curSum);

                    f[i][j] = Math.max(f[i][j], f[m][j-1] + maxSum);
                }
            }
        }
        
        return f[n][k];
    }
    
}

```



#### 二、划分型动规解法

对方法一进行思考，其实仍然可以进行优化。对于 `f[i][j] = max{f[m][j - 1] + maxSubArray(m + 1, i)}, j - 1 <= m < i` ，每当新增一个元素的时候，之前的最大值仍是最大值，但是需要和新加进来的元素进行对比，所以只需要把之前遍历得到的结果用一个变量存储起来，就无需反复遍历了。

1. 定义状态：
   - 定义 `localMax[i][j]`，表示前 `i` 个数字，取 `j` 个划分时，包含第 `i` 个数字的最大和。
   - 定义 `globalMax[i][j]`，表示前 `i` 个数字，取 `j` 个划分时，可以不包含第 `i` 个数字的最大和。
2. 定义状态转移函数：
   - 对 `localMax[i][j]`，由于一定包含第 `i` 个数字，那么有两种情况，
     - 第 `i` 个数字属于第 `j` 个子数组，对应 `localMax[i - 1][j] + nums[i - 1]`；
     - 第 `i` 个数字不属于第 `j` 个子数组，对应 `localMax[i - 1][j - 1] + nums[i - 1]`；
     - 比较两者取最大值皆可。
   - 对 `globalMax[i][j]`，比较 `localMax[i][j]` 和 `globalMax[i - 1][j]` 中的最大值即可。
3. 定义起点：
   - `f[i][0]` 从前 `i` 个数字， `0` 个划分，得到的最大和为 `0` 。
   - `f[0][i]` 从前 `0` 个数字， `i` 个划分，得到的最大和为 `0`。
4. 定义终点：最终结果即为 `f[n][k]` 。

##### 算法复杂度

- 时间复杂度：`O(nk)`。
- 空间复杂度：`O(nk)`。

注：本题目可以对 `localMax[i][j]` 进行滚动数组优化。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: An integer denote to find k non-overlapping subarrays
     * @return: An integer denote the sum of max k non-overlapping subarrays
     */
    public int maxSubArray(int[] nums, int k) {
        if (nums == null || nums.length == 0 
            || k <= 0 || k > nums.length) {
            return -1;
        }
        
        int n = nums.length;
        // status
        int[][] globalMax = new int[n + 1][k + 1];
        int[][] localMax = new int[n + 1][k + 1];
        // initialize
        
        for (int j = 1; j <= k; j++) {
            localMax[j - 1][j] = Integer.MIN_VALUE;
            for (int i = j; i <= n; i++) {
                localMax[i][j] = Math.max(localMax[i - 1][j], globalMax[i - 1][j - 1]) + nums[i - 1];
                if (i == j) {
                    globalMax[i][j] = localMax[i][j];
                } else {
                    globalMax[i][j] = Math.max(globalMax[i - 1][j], localMax[i][j]);   
                }
            }
        }
        
        return globalMax[n][k];
    }
    
}

```



### 参考

1. [LintCode-Maximum Subarray III | LiBlog](http://www.cnblogs.com/lishiblog/p/4183917.html)
2. [Lintcode: Maximum Subarray III | 姜糖水·码农](http://www.cnphp6.com/archives/78025)
3. [Lintcode - Maximum Subarray III | 雯雯熊孩子](http://blog.csdn.net/nicaishibiantai/article/details/44585383)
4. [LintCode 43 [Maximum Subarray III] | Jason_Yuan](http://www.jianshu.com/p/5045dda5ea1f)
5. [[LintCode]Maximum Subarray III | 今際の国の呵呵君](http://hehejun.blogspot.com/2015/01/lintcodemaximum-subarray-iii.html)
6. [Maximum Subarray III | 九章算法](http://www.jiuzhang.com/solutions/maximum-subarray-iii/)