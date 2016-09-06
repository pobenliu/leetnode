# Unique Paths II

Unique Paths II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/unique-paths-ii/) )

```
Description
Follow up for "Unique Paths":
Now consider if some obstacles are added to the grids. How many unique paths would there be?
An obstacle and empty space is marked as 1 and 0 respectively in the grid.

Notice
m and n will be at most 100.

Example
For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
The total number of unique paths is 2.
```



### 解题思路

解题思路参考 Unique Paths ，不同的地方在于，需要增加对当前位置数值的判断，如果为 1，则路径数量置 0 。



Java 实现：

```java
public class Solution {
    /**
     * @param obstacleGrid: A list of lists of integers
     * @return: An integer
     */
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        // write your code here
        if (obstacleGrid == null || obstacleGrid.length == 0) {
            return -1;
        }
        if (obstacleGrid[0] == null || obstacleGrid[0].length == 0) {
            return -1;
        }
        
        // state
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] sum = new int[m][n];
        
        // initialize
        sum[0][0] = (obstacleGrid[0][0] == 1) ? 0 : 1;
        for (int i = 1; i < m; i++) {
            if (obstacleGrid[i][0] != 1) {
                sum[i][0] = sum[i - 1][0];
            } else {
                // sum[i][0] = 0;  
                break;
            } 
        }
        for (int j = 1; j < n; j++) {
            if (obstacleGrid[0][j] != 1) {
                sum[0][j] = sum[0][j - 1];
            } else {
                // sum[0][j] = 0;  
                 break;
            }
        }
        
        // top down
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    sum[i][j] = 0;
                } else {
                    sum[i][j] = sum[i - 1][j] + sum[i][j - 1];
                }
            }
        }
        
        // end
        return sum[m - 1][n - 1];
    }
}
```











