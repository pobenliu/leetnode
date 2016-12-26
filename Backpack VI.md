# Backpack VI

Backpack VI  ( [lintcode](http://www.lintcode.com/en/problem/backpack-vi/) )

```
Description
Given an integer array nums with all positive numbers and no duplicates, 
find the number of possible combinations that add up to a positive integer target.

Notice
The different sequences are counted as different combinations.

Example
Given nums = [1, 2, 4], target = 4

The possible combination ways are:
[1, 1, 1, 1]
[1, 1, 2]
[1, 2, 1]
[2, 1, 1]
[2, 2]
[4]
return 6
```

### 解题思路

背包问题：重复选择+不同排列+装满可能性总数。

本题目也可以视为常规的动规问题，和 leetcode 上的 [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) 是同一道题目。

1. 定义状态：定义 `f[i]` 为填满大小为 `i` 的背包，一共有多少种方法。
2. 定义状态转移函数：对于当前状态 `f[i]` ，由于先后选取 `A[j]` 属于不同的方法，只要 `i >= A[j], 0 <= j < n`，每一个 `A[j]` 都对应一种取法，所以有 `f[i] = sum{f[i - A[j]], i >= A[j], 0<=j<n}`。 
3. 定义起点：`f[0]` 状态为 `1` 。
4. 定义终点：最终结果即为 `f[m]` 。

注：

> 1. 如果数组有负数，就必须要限制每个数使用的次数，否则会得到无穷多种排列方式, 比如 `A = [1, -1], target = 1`。

#### 算法复杂度

- 时间复杂度：`O(nm)`。
- 空间复杂度：`O(m)`。

Java 实现

```java
public class Solution {
    /**
     * @param nums an integer array and all positive numbers, no duplicates
     * @param target an integer
     * @return an integer
     */
    public int backPackVI(int[] A, int m) {
        if (A == null || A.length == 0 || m <= 0) {
            return 0;
        }
        
        int n = A.length;
        // status
        int[] f = new int[m + 1];
        // initialize
        f[0] = 1;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 0; j < n; j++) {
                if (i >= A[j]) {
                    f[i] += f[i - A[j]];    
                }
            }
        }
        return f[m];
    }
}
```



### 参考

1. [[leetcode] 377. Combination Sum IV 解题报告 | 小榕流光的专栏](http://blog.csdn.net/qq508618087/article/details/52064134)