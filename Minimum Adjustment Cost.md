# Minimum Adjustment Cost

 Minimum Adjustment Cost  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/minimum-adjustment-cost/) )

```
Description
Given an integer array, adjust each integers so that 
the difference of every adjacent integers are not greater than a given number target.
If the array before adjustment is A, the array after adjustment is B, 
you should minimize the sum of |A[i]-B[i]|

Notice
You can assume each number in the array is a positive integer and not greater than 100.

Example
Given [1,4,2,3] and target = 1, one of the solutions is [2,3,2,3], 
the adjustment cost is 2 and it's minimal.
Return 2.
```

### 解题思路

1. 定义状态：定义二维状态变量 `f[i][k]` ，前 `i` 个数字，第 `i` 个数调整为 `k` 时，满足相邻两数之差 `<= target` ，所需要的最小代价。
2. 定义状态转移函数：对于当前状态 `f[i][k]` ，其上一个状态为 `f[i - 1][j]` ，从上一个状态到下一个状态需要做的调整为 `f[i - 1][j] + abs(A.get(i - 1) - k)` ，根据题意 `k` 的范围是 `0 <= k <= 100` ，所以在满足 `abs(j - k)` 的条件下，找出做出最小调整的 `k` 值。
3. 定义起点：
   - `f[0][i]` 从前 `0` 个数字，取第 `0` 个，调整为 `i` 的方案数量为0。
4. 定义终点：最终结果即为 `f[n][i], i = 0,1,...100` 中的最小值。

Java 实现

```java
public class Solution {
    /**
     * @param A: An integer array.
     * @param target: An integer.
     */
    public int MinAdjustmentCost(ArrayList<Integer> A, int target) {
        int n = A.size();
        
        // state
        int[][] f = new int[n + 1][101];
        
        // initialize
        for (int i = 0; i <= n; i++) {
            for (int j = 1; j <= 100; j++) {
                f[i][j] = Integer.MAX_VALUE;
            }
        }
        for (int i = 1; i <= 100; i++) {
            f[0][i] = 0;
        }
        
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= 100; j++) {
                if (f[i - 1][j] != Integer.MAX_VALUE) {
                    for (int k = 1; k <= 100; k++) {
                        if (Math.abs(j - k) <= target) {
                            /*
                            if (f[i][k] > f[i - 1][j] + Math.abs(A.get(i - 1) - k)) {
                                f[i][k] = f[i - 1][j] + Math.abs(A.get(i - 1) - k);
                            }*/
                            int tmp = f[i - 1][j] + Math.abs(A.get(i - 1) - k);
                            f[i][k] = Math.min(f[i][k], tmp);
                        }
                    }
                }
            }
        }
        
        int ans = Integer.MAX_VALUE;
        for (int i = 1; i <= 100; i++) {
            if (f[n][i] < ans) {
                ans = f[n][i];
            }
        }
        return ans;
    }
}
```



### 参考

1. [Minimum Adjustment Cost | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4153927.html)

   ​