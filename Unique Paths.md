# Unique Paths

Unique Paths  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/unique-paths/#) )

```
Description
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. 
The robot is trying to reach the bottom-right corner of the grid 
(marked 'Finish' in the diagram below).
How many possible unique paths are there?

Notice
m and n will be at most 100.

Example
| 1,1 | 1,2 | 1,3 | 1,4 | 1,5 | 1,6 | 1,7 |
|------------------------------------------
| 2,1 |                                   |
|------------------------------------------
| 3,1 |                             | 3,7 |

Above is a 3 x 7 grid. How many possible unique paths are there?
```



### 解题思路

求方案的总数量，属于矩阵类型的动态规划。

1. 定义状态：题目求解的是到达路径数量，那么可以定义 `f[i][j]` 为从起点 `[0][0]` 走到 `[i][j]` 的路径数量。
2. 定义状态转移函数：画图观察，走到 `[i][j]` 的之前一步可以是 `[i - 1][j]` 或 `[i][j - 1]` ，所以有 `f[i][j] = f[i - 1][j] + f[i][j - 1]` 。
3. 定义起点：从矩阵的左上角开始，所以起点是 `[0][0]` 。同时，在第一行或第一列，机器人都只有一种走法，所以初始化为 1。
4. 定义终点：也就是最终的结果，右下角位置。

Java 实现

```java
public class Solution {
    /**
     * @param n, m: positive integer (1 <= n ,m <= 100)
     * @return an integer
     */
    public int uniquePaths(int m, int n) {
        if (m == 0 || n == 0) {
            return 0;
        }
        
        // state
        int[][] sum = new int[m][n];
        
        // initialize
        for (int i = 0; i < m; i++) {
            sum[i][0] = 1;
        }
        for (int j = 1; j < n; j++) {
            sum[0][j] = 1;
        }
        
        // top down
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1];
            }
        }
        
        // end
        return sum[m - 1][n - 1];
    }
}
 
```



