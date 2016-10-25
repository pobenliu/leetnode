# Triangle

Triangle ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/triangle/) )

```
Description
Given a triangle, find the minimum path sum from top to bottom. 
Each step you may move to adjacent numbers on the row below.

Notice
Bonus point if you are able to do this using only O(n) extra space, 
where n is the total number of rows in the triangle.

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

将三角形矩阵调整一下形状，这样更容易找到两层元素坐标之间的关系。

```
[
[2],
[3,4],
[6,5,7],
[4,1,8,3]
]
```

1. 定义状态：题目求解的是最小值路径，那么可以定义 `f[i][j]` 为从起点 `[0][0]` 走到 `[i][j]` 的最小路径。
2. 定义状态转移函数：画图观察，走到 `[i][j]` 的之前一步可以是 `[i - 1][j]` 或 `[i - 1][j - 1]` ，所以有 `f[i][j] = Math.min(f[i - 1][j], f[i - 1][j - 1]) + triangle[i][j]` 
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

另外一种解法，没有初始化最外边两排的元素，而是在求解过程中考虑边界问题。Java 实现如下

```java
public class Solution {
    /**
     * @param triangle: a list of lists of integers.
     * @return: An integer, minimum path sum.
     */
    public int minimumTotal(int[][] triangle) {
        if (triangle == null || triangle[0] == null ) {
            return -1;        
        }
        
        int n = triangle.length;
        // status
        int[][] f = new int[n + 1][n + 1];
        
        // initialize
        f[0][0] = 0;
        
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                f[i][j] = Integer.MAX_VALUE;
                if (j > i - 1) {
                    f[i][j] = f[i - 1][j - 1] + triangle[i - 1][j - 1];
                } else if (j - 1 <= 0) {
                    f[i][j] = f[i - 1][j] + triangle[i - 1][j - 1];
                } else {
                    f[i][j] = Math.min(f[i - 1][j - 1], f[i - 1][j]) + triangle[i - 1][j - 1];
                }
            }
        }
        
        int min = Integer.MAX_VALUE;
        for (int i = 1; i <= n; i++) {
            if (f[n][i] < min) {
                min = f[n][i];
            }
        }
        return min;
    }
}

```





#### 二、自底向上

1. 定义状态：题目求解的是最小值路径，那么可以定义 `f[i][j]` 为从起点 `[n - 1][j]` 走到 `[i][j]` 的最小路径。
2. 定义状态转移函数：画图观察， `[i][j]` 出发可到达的点是 `[i + 1][j]` 或 `[i + 1][j + 1]` ，所以有 `f[i][j] = Math.min(f[i + 1][j], f[i + 1][j + 1]) + triangle[i][j]` 
3. 定义起点：从矩阵最后一行开始，所以起点是 `[n - 1][j]` 。
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