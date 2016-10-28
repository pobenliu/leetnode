# Minimum Path Sum

Minimum Path Sum  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/minimum-path-sum/) )

```
Description
Given a m x n grid filled with non-negative numbers, 
find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Notice
You can only move either down or right at any point in time.
```



### 解题思路

属于矩阵类型的动态规划。

1. 定义状态：题目求解的是最小和路径，那么可以定义 `f[i][j]` 为从起点 `[0][0]` 走到 `[i][j]` 的最小路径和。
2. 定义状态转移函数：画图观察，走到 `[x][y]` 的前一步可以是 `[i - 1][j]` 或 `[i][j - 1]` ，所以有 `f[i][j] = Math.min(f[i - 1][j], f[i][j - 1]) + triangle[i][j]` 。
3. 定义起点：从矩阵的左上角开始，所以起点是 `[0][0]` 。同时，第一行或第一列，考虑到每个点都只有一种走法，可以根据状态转移特性初始化，本题的初始化需要累加。
4. 定义终点：也就是最终的结果，是右下角位置 `f[i][j]` 。

**注：两个方向下标的取值范围务必保证能够遍历到所有位置。**

Java 实现

```java
public class Solution {
    /**
     * @param grid: a list of lists of integers.
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
        // write your code here
        if (grid == null || grid.length == 0) {
            return -1;
        }
        if (grid[0] == null || grid[0].length == 0) {
            return -1;
        }
        
        // state
        int m = grid.length;
        int n = grid[0].length;
        int[][] f = new int[m][n];
        
        // start
        f[0][0] = grid[0][0];
        for(int i = 1; i < m; i++) {
            f[i][0] = f[i - 1][0] + grid[i][0];
        }
        for(int j = 1; j < n; j++) {
            f[0][j] = f[0][j - 1] + grid[0][j];
        }
        
        // top down
        // i 和 j 的取值范围务必保证能够遍历所有的位置
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                f[i][j] = Math.min(f[i - 1][j], f[i][j - 1]) + grid[i][j];
            }
        }
        
        // end
        return f[m - 1][n - 1];
    }
}
```

优化空间（滚动数组）：

观察状态转移函数，`f[i][j]` 由 `f[i - 1][j]` 或 `f[i][j - 1]` 决定，与前 `i - 2` 行无关，所以可利用滚动数组进行优化。

```java
public class Solution {
    /**
     * @param grid: a list of lists of integers.
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
        if (grid == null || grid.length == 0 || 
            grid[0] == null || grid[0].length == 0) {
            return 0;        
        }
        
        int m = grid.length;
        int n = grid[0].length;
        // status
        int[][] f = new int[2][n];
        
        // initialize
        f[0][0] = grid[0][0];

        for (int j = 1; j < n; j++) {
            f[0][j] = f[0][j - 1] + grid[0][j];
        }
        
        for (int i = 1; i < m; i++) {
            f[i % 2][0] = f[(i - 1) % 2][0] + grid[i][0];
            for (int j = 1; j < n; j++) {
                f[i % 2][j] = Math.min(f[(i - 1) % 2][j], f[i % 2][j - 1]) + grid[i][j];
            }
        }
        
        return f[(m - 1) % 2][n - 1];
    }
}

```

