#  Maximal Square

 Maximal Square  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximal-square/) )

```
Description
Given a 2D binary matrix filled with 0's and 1's, 
find the largest square containing all 1's and return its area.

Example
For example, given the following matrix:
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
Return 4.
```

### 解题思路

本题目求解最大全"1"正方形边长，考虑使用动态规划，关键点在于如何定义状态。

1. 定义状态：考虑到正方形可以用一个顶点和边长来确定，我们定义 `f[i][j]` 是右下角坐标为 `[i, j]` 的全“1”正方形的最大边长。定义右下角也是为后续的状态转移做准备。
2. 定义状态转移函数：在 `matrix[i][j] == 1` 的情况下，以 `[i, j]` 为右下角的全“1”正方形的最大边长，和以 `[i - 1, j], [i, j - 1], [i - 1, j - 1]` 为右下角的三个全“1”正方形的最大边长有关，而且取决于以上三者的最小值。
3. 定义起点：初始化第一行第一列，需要考虑矩阵只有一行或者一列的情况。
4. 定义终点：`f[i][j]` 的最大值为边长，求平方即可。

Java 实现

```java
public class Solution {
    /**
     * @param matrix: a matrix of 0 and 1
     * @return: an integer
     */
    public int maxSquare(int[][] matrix) {
        if (matrix == null || matrix.length == 0 ||
            matrix[0] == null || matrix[0].length == 0) {
            return 0;        
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        // status
        int[][] f = new int[m][n];
        // initialize
        int max = 0;
        for (int i = 0; i < m; i++) {
            f[i][0] = matrix[i][0];
            max = f[i][0] > max ? f[i][0] : max;
        }
        for (int i = 1; i < n; i++) {
            f[0][i] = matrix[0][i];
            max = f[0][i] > max ? f[0][i] : max;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                f[i][j] = Integer.MAX_VALUE;
                if (matrix[i][j] == 1) {
                    f[i][j] = Math.min(f[i - 1][j - 1], Math.min(f[i - 1][j], f[i][j - 1])) + 1;
                } else {
                    f[i][j] = 0;   
                }
                if (f[i][j] > max) {
                    max = f[i][j];
                }
            }
        }
        
        return max * max;
    }
}
```

空间优化（滚动数组）：

不难发现，`f[i][j]` 的取值只与 `f[i - 1][j], f[i - 1][j - 1], f[i - 1][j - 1]` 有关，也就是说矩阵 `f` 第 `i` 行的结果只与第 `i - 1` 行有关，所以可以使用滚动数组优化空间。

Java 实现

```java
public class Solution {
    /**
     * @param matrix: a matrix of 0 and 1
     * @return: an integer
     */
    public int maxSquare(int[][] matrix) {
        if (matrix == null || matrix.length == 0 ||
            matrix[0] == null || matrix[0].length == 0) {
            return 0;        
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        // status
        int[][] f = new int[2][n];
        // initialize
        int max = 0;
        for (int i = 0; i < 2; i++) {
            f[i][0] = matrix[i][0];
            max = f[i][0] > max ? f[i][0] : max;
        }
        for (int i = 1; i < n; i++) {
            f[0][i] = matrix[0][i];
            max = f[0][i] > max ? f[0][i] : max;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                // f[i][j] = Integer.MAX_VALUE;
                if (matrix[i][j] == 1) {
                    f[i % 2][j] = Math.min(f[(i - 1) % 2][j - 1], Math.min(f[(i - 1) % 2][j], f[i % 2][j - 1])) + 1;
                } else {
                    f[i % 2][j] = 0;   
                }
                if (f[i % 2][j] > max) {
                    max = f[i % 2][j];
                }
            }
        }
        
        return max * max;
    }
}
```

 

### 参考

1. [Maximal Square | 九章算法](http://www.jiuzhang.com/solutions/maximal-square/)