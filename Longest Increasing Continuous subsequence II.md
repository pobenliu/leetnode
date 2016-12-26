# Longest Increasing Continuous subsequence II

Longest Increasing Continuous subsequence II ( [lintcode](http://www.lintcode.com/en/problem/longest-increasing-continuous-subsequence-ii/) )

```
Description
Give you an integer matrix (with row size n, column size m)，
find the longest increasing continuous subsequence in this matrix. 
(The definition of the longest increasing continuous subsequence here 
can start at any row or column and go up/down/right/left any direction).

Example
Given a matrix:
[
  [1 ,2 ,3 ,4 ,5],
  [16,17,24,23,6],
  [15,18,25,22,7],
  [14,19,20,21,8],
  [13,12,11,10,9]
]
return 25

Challenge
O(nm) time and memory.
```

### 解题思路

本题与滑雪问题是同一类问题，考虑到以下几个因素，需要使用记忆化搜索解题。

1. 初始化不容易找到。
2. 状态转移比较麻烦，没有清晰的顺序性。

`dp[i][j]` 表示从 `A[i][j]` 出发向上下左右四个方向寻找，得到的最长连续子序列的长度。

`flag[i][j]` 来标记 `dp[i][j]` 求解状态，为 `1` 时表示已完成求解，直接返回 `dp[i][j]`。为 `-1` 时则为了防止重复寻找。

Java 实现

```java
// write your code here
public class Solution {
    /**
     * @param A an integer matrix
     * @return an integer
     */

    public int longestIncreasingContinuousSubsequenceII(int[][] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        int n = A.length;
        int m = A[0].length;
        int ans = 0;
        int[][] dp = new int[n][m];
        int[][] flag = new int[n][m];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                dp[i][j] = dfs(A, dp, flag, i, j, n, m);
                ans = Math.max(ans, dp[i][j]);
            }
        }
        return ans;
    }
  
    int[] dx = {1, -1, 0, 0};
    int[] dy = {0, 0, 1, -1};
    
    private int dfs(int[][] A, int[][] dp, int[][] flag, int i, int j, int n, int m) {
        if (flag[i][j] == 1) {
            return dp[i][1];
        }
        
        int ans = 1;
        flag[i][j] = -1;
        for (int k = 0; k < 4; k++) {
            int id = i + dx[k];
            int jd = j + dy[k];
            if (0 <= id && id < n && 0 <= jd && jd < n) {
                if (flag[id][jd] != -1 && A[i][j] < A[id][jd]) {
                    ans = Math.max(ans, dfs(A, dp, flag, id, jd, n, m) + 1);
                }
            }
        }
        flag[i][j] = 1;
        dp[j][j] = ans;
        return dp[i][j];
    }
}
```

### 参考

1. [Longest Increasing Continuous subsequence II.java](https://github.com/shawnfan/LintCode/blob/master/Java/Longest%20Increasing%20Continuous%20subsequence%20II.java)