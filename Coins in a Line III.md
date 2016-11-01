# Coins in a Line III

Coins in a Line III ( [lintcode](http://www.lintcode.com/en/problem/coins-in-a-line-iii/) )

```
Description
There are n coins in a line. 
Two players take turns to take a coin from one of the ends of the line 
until there are no more coins left. 
The player with the larger amount of money wins.
Could you please decide the first player will win or lose?

Example
Given array A = [3,2,2], return true.
Given array A = [1,2,4], return true.
Given array A = [1,20,4], return false.

Challenge 
Follow Up Question:
If n is even. 
Is there any hacky algorithm that can decide 
whether first player will win or lose in O(1) memory and O(n) time?
```

### 解题思路

使用动态规划（记忆化搜索）分析。

1. 定义状态：考虑到可以从两端选取硬币，所以要使用二维状态变量，以区分选取方向。定义`f[i][j]` 为现在还剩第 `i` 个到第 `j` 个硬币，先手取硬币最终可得的最大价值。
2. 定义状态转移函数： 参考图示，剩余硬币为第 `i` 个到第 `j` 个时，先手有两种取法，
   - 取左侧硬币 `A[i]`。后手对应两种取法，这之后先手面临的是 `f[i + 2][j]` 和 `f[i + 1][j - 1]` 中的最小值。
   - 取右侧硬币 `A[j]`。后手对应两种取法，这之后先手面临的是 `f[i + 1][j - 1]` 和 `f[i][j - 2]` 中的最小值。
   - 所以对应状态转移方程为 `f[i][j] = Max{Min{f[i + 2][j], f[i + 1][j - 1]} + A[i], Min{f[i + 1][j - 1], f[i][j - 2]} + A[j]}`。
3. 定义起点：初始化状态转移函数无法涉及的状态 `f[i, i] = A[i], f[i, i + 1] = Max{A[i], A[i + 1]}` 。
4. 定义终点：为 `f[0][n - 1]`。

![](http://ww2.sinaimg.cn/mw690/600e6311jw1f9cmxsxuklj20go0angmp.jpg)

Java 实现

```java
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin (int[] values) {
        int n = values.length;
        int[][] f = new int[n + 1][n + 1];
        boolean[][] flag = new boolean[n + 1][n + 1];
        
        int sum = 0;
        for (int now : values) {
            sum += now;
        }
        
        return sum < 2 * MemorySearch(0, values.length - 1, f, flag, values);
    }
    
    private int MemorySearch (int left, int right, int[][] f, boolean[][] flag, int[] values) {
        if (flag[left][right]) {
            return f[left][right];
        }
        flag[left][right] = true;
        if (left > right) {
            f[left][right] = 0;
        } else if (left == right) {
            f[left][right] = values[left];
        } else if (left + 1 == right) {
            f[left][right] = Math.max(values[left], values[right]);
        } else {
            int pick_left = Math.min(
                            MemorySearch(left + 2, right, f, flag, values),
                            MemorySearch(left + 1, right - 1, f, flag, values) + values[left]
                            );
            int pick_right = Math.min(
                            MemorySearch(left + 1, right - 1, f, flag, values),
                            MemorySearch(left, right - 2, f, flag, values) + values[right]
                            );
            f[left][right] = Math.max(pick_left, pick_right);
        }
        
        return f[left][right];
    }
}
```

