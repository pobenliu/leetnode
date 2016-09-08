# Longest Common Subsequence

Longest Common Subsequence  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/longest-common-subsequence/) )

```
Description
Given two strings, find the longest common subsequence (LCS).
Your code should return the length of LCS.

Clarification
What's the definition of Longest Common Subsequence?
https://en.wikipedia.org/wiki/Longest_common_subsequence_problem
http://baike.baidu.com/view/2020307.htm

Example
For "ABCD" and "EDCA", the LCS is "A" (or "D", "C"), return 1.
For "ABCD" and "EACB", the LCS is "AC", return 2.
```



### 解题思路

1. 定义状态：定义 `f[i][j]` 为字符串 `A` 的前 `i` 个字符 和 字符串 `B` 的前 `j` 个字符 的最长公共子序列长度。
2. 定义状态转移函数：我们来看`f[i][j]` 的上一个状态。如果`A` 的前 `i` 个字符 `A[i - 1]` 和 字符串 `B` 的前 `j` 个字符 `B[j - 1]` 相等，那么易得到 `f[i][j] = f[i - 1][j - 1] + 1` 。如果 `A[i - 1]` 和 `B[i - 1]` 不等，那么可以在 `A` 或 `B` 前溯一个位置进行比较，取最大值 `f[i][j] = max(f[i - 1][j], f[i][j - 1])` 。
3. 定义起点：当 `A` 为空或 `B` 为空时，对应的状态函数 `f` 为 `0`，考虑到整数数组默认初始化为 `0` ，也可以不显式初始化。
4. 定义终点：最终的结果是 `f[n][m]` 。

注：需要注意的地方是，`f[i][j]` 中 `i` 和 `j` 的取值范围分别是 `n + 1` 和 `m + 1` 。

Java 实现

```java
public class Solution {
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
    public int longestCommonSubsequence(String A, String B) {
        // write your code here
        if (A == null || A.length() == 0) {
            return 0;
        }
        if (B == null || B.length() == 0) {
            return 0;
        }
        
        // state
        int m = A.length();
        int n = B.length();
        int[][] f = new int[m + 1][n + 1];
        
        // initialize
        for (int i = 0; i < m; i++) {
            f[i][0] = 0;
        }
        for (int j = 1; j < n; j++) {
            f[0][j] = 0;
        }
        
        // top down
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (A.charAt(i - 1) == B.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] +1;
                } else {
                    f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                }
            }
        }
        
        // end
        return f[m][n];
    }
}

```



九章算法的一种实现

```java
public class Solution {
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
    public int longestCommonSubsequence(String A, String B) {
        int n = A.length();
	    int m = B.length();
        int f[][] = new int[n + 1][m + 1];
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                if(A.charAt(i - 1) == B.charAt(j - 1))
                    f[i][j] = f[i - 1][j - 1] + 1;
            }
        }
        return f[n][m];
    }
}
```

​	

### 参考

1. [Longest Common Subsequence | 九章算法](http://www.jiuzhang.com/solutions/longest-common-subsequence/)