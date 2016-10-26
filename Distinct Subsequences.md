# Distinct Subsequences

Distinct Subsequences  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/distinct-subsequences/) )

```
Description
Given a string S and a string T, count the number of distinct subsequences of T in S.
A subsequence of a string is a new string which is formed from the original string 
by deleting some (can be none) of the characters without disturbing the relative positions 
of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

Example
Given S = "rabbbit", T = "rabbit", return 3.

Challenge 
Do it in O(n^2) time and O(n) memory.
O(n^2) memory is also acceptable if you do not know how to optimize memory.
```



### 解题思路

#### 一、动态规划

1. 定义状态：定义 `f[i][j]` 为字符串 `S` 的前 `i` 个字符 挑出 字符串 `T` 的前 `j` 个字符有多少种方案 。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，详细讨论如下：
   - `S.charAt(i - 1) == T.charAt(j - 1)` 时。
   - **无操作**：上一个状态可能是 `f[i - 1][j - 1]` ，此时无需任何操作。
     - **删除操作**：如果需要对 `S` 末尾执行一次删除操作，删除之后 `S.charAt[i - 2] == T.charAt[j - 1]` ，那么执行操作前 `S` 的前 `i - 1` 个字符和 `T` 的前 `j` 个字符相匹配，上一个状态是 `f[i - 1][j]` 。不能对 `T` 进行删除操作。
     - 求解的是方案总数，所以 `f[i][j] = f[i - 1][j - 1] + f[i - 1][j]` 。
   - `S.charAt(i - 1) != T.charAt(j - 1)` 时。
     - **删除操作**：那么只能对 `S` 末尾执行一次删除操作，删除之后 `S.charAt[i - 2] == T.charAt[j - 1]` ，那么执行操作前 `S` 的前 `i - 1` 个字符和 `T` 的前 `j` 个字符相匹配，上一个状态是 `f[i - 1][j]` 。
     - 所以 `f[i][j] = f[i - 1][j]` 。
3. 定义起点：
   - 当 `S` 为空，`T` 不为空时，无法在 `S` 中找到 `T` ，所以 `f[0][j] = 0, j = 1 ~ m` 。
   - 当 `S` 不空，`T` 为空时，将 `S` 的所有字符删掉可得空字符串，所以 `f[i][0] = 1, i = 1 ~ n` 。<u>注：第一次做的时候没疏忽了，没考虑到这种情况</u>。
   - 当 `S` 和 `T` 都为空时，`f[0][0] = 1` 。
4. 定义终点：最终结果即为 `f[n][m]` 。注：初始化时也需要注意取值范围。

Java 实现

```java
public class Solution {
    /**
     * @param S, T: Two string.
     * @return: Count the number of distinct subsequences
     */
    public int numDistinct(String S, String T) {
        // write your code here
        if (S == null || T == null) {
            return 0;
        }
        
        int n = S.length();
        int m = T.length();
        
        if (m > n) {
            return 0;
        }
        
        // state
        int[][] f = new int[n + 1][m + 1];
        
        
        // initialize
        for (int i = 0; i <= n; i++) {
            f[i][0] = 1;
        }
        
        // top down
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (S.charAt(i - 1) == T.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + f[i - 1][j];
                } else {
                    f[i][j] = f[i - 1][j];
                }
            }
        }
        
        // end
        return f[n][m];
    }
}
```



#### 二、动态规划+状态压缩（滚动数组）