#  Spiral Matrix II

 Spiral Matrix II  ( [lintcode](http://www.lintcode.com/en/problem/spiral-matrix-ii/) )

```
Description
Given an integer n, generate a square matrix filled with elements from 1 to n^2 in spiral order.

Notice
Example
Given n = 3,

You should return the following matrix:
[
  [ 1, 2, 3 ],
  [ 8, 9, 4 ],
  [ 7, 6, 5 ]
]
```

### 解题思路

矩阵的顺时针遍历，在这里注意有实现的小技巧，像剥洋葱一样遍历矩阵，每次遍历最外围的元素，每次遍历的元素个数都是当前行数/列数减一，在遍历最外围元素后对矩阵的长宽、起始点坐标进行更新。

注意特殊情况，只有一行或一列。

Java 实现

```java
public class Solution {
    /**
     * @param n an integer
     * @return a square matrix
     */
    public int[][] generateMatrix(int n) {
        // Write your code here
        if (n < 0) {
            return null;
        }
        
        int num = 1;
        int[][] ans = new int[n][n];
        int x = 0, y = 0;
        
        while (n > 0) {
            if (n == 1) {
                ans[x][y] = num;
                break;
            }
            
            for (int i = 0; i < n - 1; i++) {
                ans[x][y++] = num++;
            }
            for (int i = 0; i < n - 1; i++) {
                ans[x++][y] = num++;
            }
            for (int i = 0; i < n - 1; i++) {
                ans[x][y--] = num++;
            }
            for (int i = 0; i < n - 1; i++) {
                ans[x--][y] = num++;
            }
            x++;
            y++;
            n = n - 2;
        }
        return ans;
    }
}
```

