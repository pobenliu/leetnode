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

Java 实现

```java
// write your code here
public class Solution {
    /**
     * @param A an integer matrix
     * @return an integer
     */
	int[][] visit;
    int[][] flag;
    int n, m;
    public int longestIncreasingContinuousSubsequenceII(int[][] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        n = A.length;
        m = A[0].length;
        int ans = 0;
        visit = new int[n][m];
        flag = new int[n][m];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                visit[i][j] = dfs(i, j, A);
                ans = Math.max(ans, visit[i][j]);
            }
        }
        return ans;
    }
    
    int[] dx = {1, -1, 0, 0};
    int[] dy = {0, 0, 1, -1};
    
    private int dfs(int x, int y, int[][] A) {
        if (flag[x][y] != 0) {
            return visit[x][y];
        }
        
        int ans = 1;
        int nx, ny;
        flag[x][y] = -1;
        for (int i = 0; i < 4; i++) {
            nx = x + dx[i];
            ny = y + dy[i];
            if (0 <= nx && nx < n && 0 <= ny && ny < n) {
                if (flag[nx][ny] != -1 && A[x][y] < A[nx][ny]) {
                    ans = Math.max(ans, dfs(nx, ny, A) + 1);
                }
            }
        }
        flag[x][y] = 1;
        visit[x][y] = ans;
        return ans;
    }
}
```

