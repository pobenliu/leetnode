# Longest Common Substring

Longest Common Substring  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/longest-common-substring/) )

```
Description
Given two strings, find the longest common substring.
Return the length of it.

Notice
The characters in substring should occur continuously in original string. This is different with subsequence.

Example
Given A = "ABCD", B = "CBCE", return 2.

Challenge 
O(n x m) time and memory.
```



### 解题思路

1. 定义状态：如果定义 `f[i][j]` 为字符串 `A` 的前 `i` 个字符 和 字符串 `B` 的前 `j` 个字符 的最长公共子字符串长度，那么当 `A[i - 1]` 和 `B[j - 1]` 相等时，不容易判断 `A[i - 2]` 和 `B[j - 2]` 是否相等，在状态 `f[i][j]` 中不含有相关信息 。

   转换一下思路，定义 `f[i][j]` 为以 `A[i - 1]` 和 `B[j - 1]` 为结尾的LCS的长度，这样就可以记忆连续的公共子字符串。

2. 定义状态转移函数：由以上定义，`A[i - 1] == B[j - 1]` 时，则有 `f[i][j] = f[i - 1][j - 1] + 1` ；`A[i - 1] != B[j - 1]` 时，`f[i][j] = 0` 。

3. 定义起点：当 `A` 为空或 `B` 为空时，对应的状态函数 `f` 为 `0`，考虑到整数数组默认初始化为 `0` ，也可以不显式初始化。

4. 定义终点：在 `f[0 ~ n][0 ~ m]` 中取最大值即为最终结果。

Java 实现

```java
public class Solution {
    /**
     * @param A, B: Two string.
     * @return: the length of the longest common substring.
     */
    public int longestCommonSubstring(String A, String B) {
        // write your code here
        if (A == null || A.length() == 0) {
            return 0;
        }
        if (B == null || B.length() == 0) {
            return 0;
        }
        
        // state
        int n = A.length();
        int m = B.length();
        int[][] f = new int[n + 1][m + 1];
        
        // initialize
        int max = 0;
        // default f[i][j] == 0
        
        // top down
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (A.charAt(i - 1) != B.charAt(j - 1)) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = f[i - 1][j - 1] + 1;
                }
                if (max < f[i][j]) {
                    max = f[i][j];
                }
            }
        }
        
        // end
        return max;
    }
}
```



### 参考





