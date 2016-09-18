#  Backpack II

 Backpack II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/backpack-ii/) )

```
Description
Given n items with size Ai and value Vi, and a backpack with size m. 
What's the maximum value can you put into the backpack?

Notice
You cannot divide item into small pieces and the total size of items you choose should smaller or equal to m.

Example
Given 4 items with size [2, 3, 5, 7] and value [1, 5, 2, 4], and a backpack with size 10. The maximum value is 9.

Challenge 
O(n x m) memory is acceptable, can you do it in O(m) memory?
```



### 解题思路

#### 一、动态规划

1. 定义状态：定义 `max[i][j]` 为 `A` 的前 `i` 个数，取出若干个物品后，体积正好为 `j` 的最大值。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，
   - 如果不计入 `A[i - 1]` ，则 `f[i][j]` 对应的上一个状态是 `f[i - 1][j]` ；
   - 如果计入 `A[i - 1]` ，满足条件 `j > A[i - 1]` ，那么 `f[i][j]` 上一个状态是 `f[i - 1][j - 1]` ；
   - 以上两种情况取最大值即可。
3. 定义起点：
   - 当 `i != 0` ，`j == 0` 时，如果一个数字都不计入，即取前 `i` 个数字中的零个，那么最大取值为零，所以 `f[i][0] = 0` 。
   - 当 `i == 0` ，`j != 0` 时，前零个元素的和显然无法填满大小为 `j` 的背包， `f[0][j] = 0` 。
   - `f[0][0] = 0` 。
4. 定义终点：最终结果即为 `f[n][m]` 。

Java 实现

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A & V: Given n items with size A[i] and value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int V[]) {
        // write your code here
        if (A == null || A.length == 0 ||
            V == null || V.length == 0) {
            return 0;
        }
        
        int n = A.length;
        
        // state
        int[][] max = new int[n + 1][m + 1];
        
        // initialize
        // default
        
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                max[i][j] = max[i - 1][j];
                if (j >= A[i - 1]) {
                    max[i][j] = Math.max(max[i - 1][j], max[i - 1][j - A[i - 1]] + V[i - 1]);
                }
            }
        }
        
        // answer
        return max[n][m];
    }
}
```



#### 二、动态规划 II

按照 Backpack 问题中的思路进行优化。

Java 实现

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A & V: Given n items with size A[i] and value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int V[]) {
        // write your code here
        if (A == null || A.length == 0 ||
            V == null || V.length == 0) {
            return 0;
        }
        
        int n = A.length;
        
        // state
        int[] max = new int[m + 1];
        
        // initialize
        // default
        
        for (int i = 1; i <= n; i++) {
            for (int j = m; j >= 0; j--) {
                if (j >= A[i - 1]) {
                    max[j] = Math.max(max[j], max[j - A[i - 1]] + V[i - 1]);
                }
            }
        }
        
        // answer
        return max[m];
    }
}
```



### 参考