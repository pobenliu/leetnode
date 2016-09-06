# Triangle

Triangle ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/triangle/) )

```
Description
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

Notice
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

Example
Given the following triangle:
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).
```



### 解题思路

#### 一、自顶向下

求解矩阵中的最大值/最小值问题，考虑采用动态规划解题。

1. 定义状态：题目求解的是最小值路径，那么可以定义 `f[x][y]` 为从起点 `[0][0]` 走到 `[x][y]` 的最小路径。
2. 定义状态转移函数：画图观察，走到 `[x][y]` 的之前一步可以是 `[x - 1][y]` 或 `[x - 1][y - 1]` ，所以有 `f[i][j] = Math.min(f[i - 1][j], f[i - 1][j - 1]) + triangle[i][j]` 
3. 定义起点：从矩阵的左上角开始，所以起点是 `[0][0]` 。同时，边界序列，如最左边的一排，考虑到每个点都只能从上一行的点走到，所以也可以初始化。
4. 定义终点：也就是最终的结果，从最后一行中选取最小值即可。

Java 实现：

```java
public class Solution {
    /**
     * @param triangle: a list of lists of integers.
     * @return: An integer, minimum path sum.
     */
    public int minimumTotal(int[][] triangle) {
        // write your code here
        if(triangle == null || triangle.length == 0) {
            return -1;
        }
        if(triangle[0] == null || triangle[0].length == 0) {
            return -1;
        }
        
        // state: f[x][y] = minimum path value from 0,0 to x,y
        int n = triangle.length;
        int[][] f = new int[n][n];
        
        // initialize
        f[0][0] = triangle[0][0];
        for(int i = 1; i < n; i++) {
            f[i][0] = f[i - 1][0] + triangle[i][0];
            f[i][i] = f[i - 1][i - 1] + triangle[i][i];
        }
        
        // top down
        for(int i = 1; i < n; i++) {
            for(int j = 1; j < i; j++) {
                f[i][j] = Math.min(f[i - 1][j], f[i - 1][j - 1]) + triangle[i][j];
            }
        }
        
        // answer
        int best = f[n - 1][0];
        for(int i = 1; i < n; i++) {
            best = Math.min(best, f[n - 1][i]);
        }
        return best;
    }
}
```



#### 二、自底向上

1. 定义状态：题目求解的是最小值路径，那么可以定义 `f[x][y]` 为从起点 `[n - 1][i]` 走到 `[0][0]` 的最小路径。
2. 定义状态转移函数：画图观察， `[x][y]` 出发可到达的点是 `[x + 1][y]` 或 `[x + 1][y + 1]` ，所以有 `f[i][j] = Math.min(f[i + 1][j], f[i + 1][j + 1]) + triangle[i][j]` 
3. 定义起点：从矩阵最后一行开始，所以起点是 `[n - 1][i]` 。
4. 定义终点：也就是最终的结果，就是 `[0][0]`。



java 实现

```java
public class Solution {
    /**
     * @param triangle: a list of lists of integers.
     * @return: An integer, minimum path sum.
     */
    public int minimumTotal(int[][] triangle) {
        // write your code here
        if(triangle == null || triangle.length == 0) {
            return -1;
        }
        if(triangle[0] == null || triangle[0].length == 0) {
            return -1;
        }
        
        // state: f[x][y] = minimum path value from n-1,i to x,y
        int n = triangle.length;
        int[][] f = new int[n][n];
        
        // initialize
        for(int i = 0; i < n; i++) {
            f[n - 1][i] = triangle[n - 1][i];
        }
        
        // down top 
        for(int i = n - 2; i >= 0; i--) {
            for(int j = i; j >= 0; j--) {
                f[i][j] = Math.min(f[i + 1][j], f[i + 1][j + 1]) + triangle[i][j];
            }
        }
        
        // answer
        return f[0][0];
    }
}
```



### 参考

1. [Triangle | 九章算法](http://www.jiuzhang.com/solutions/triangle/)
2. [什么是动态规划？动态规划的意义是什么？ | 知乎](https://www.zhihu.com/question/23995189)

