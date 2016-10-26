# Edit Distance

Edit Distance  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/edit-distance/) )

```
Description
Given two words word1 and word2, 
find the minimum number of steps required to convert word1 to word2. 
(each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character

Example
Given word1 = "mart" and word2 = "karma", return 3.
```
 


### 解题思路

动态规划，本题与 LCS 类似。

1. 定义状态：定义 `f[i][j]` 为字符串 `A` 的前 `i` 个字符 匹配 字符串 `B` 的前 `j` 个字符需要执行的最少操作数量 。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，由于涉及操作较多，所以需要详细讨论：
   - `A[i - 1] == B[j - 1]` 时。
   - **无操作**：上一个状态可能是 `f[i - 1][j - 1]` ，此时无需任何操作。
     - **插入操作**：如果需要对 `A` 末尾执行一次插入操作，插入之后 `A[i] == B[j - 1]` ，那么执行操作前 `A` 的前 `i` 个字符和 `B` 的前 `j - 1` 个字符相匹配，上一个状态是 `f[i][j - 1]` 。
     - **删除操作**：如果需要对 `A` 末尾执行一次删除操作，删除之后 `A[i - 2] == B[j - 1]` ，那么执行操作前 `A` 的前 `i - 1` 个字符和 `B` 的前 `j` 个字符相匹配，上一个状态是 `f[i - 1][j]` 。
     - 综上， 当前状态需要取以上三种情况的最小值。
   - `A[i - 1] != B[j - 1]` 时。
     - **替换操作**：将 `A` 的第 `i` 个字符替换为 `B` 的前 `j` 个字符，那么执行操作前 `A` 的前 `i - 1` 个字符和 `B` 的前 `j - 1` 个字符相匹配，上一个状态是 `f[i - 1][j - 1]` 。
     - **插入操作**：如果需要对 `A` 末尾执行一次插入操作，插入之后 `A[i] == B[j - 1]` ，那么执行操作前 `A` 的前 `i` 个字符和 `B` 的前 `j - 1` 个字符相匹配，上一个状态是 `f[i][j - 1]` 。
     - **删除操作**：如果需要对 `A` 末尾执行一次删除操作，删除之后 `A[i - 2] == B[j - 1]` ，那么执行操作前 `A` 的前 `i - 1` 个字符和 `B` 的前 `j` 个字符相匹配，上一个状态是 `f[i - 1][j]` 。
     - 综上，当前状态需要取以上三种情况的最小值。
3. 定义起点：起点 `f[0][0] == 0` ，当 `A` 为空或 `B` 为空时，对应的状态函数 `f[0][j]` 或`f[i][0]` ，从起点出发分别需要 `j` 或 `i` 次操作。
4. 定义终点：最终结果即为 `f[n][m]` 。

易错点：

> 1. 在处理边界情况时，如果其中一个字符串长度为 0，程序时可以处理的，所以只要考虑字符串为空的情况即可。

Java 实现

```java
public class Solution {
    /**
     * @param word1 & word2: Two string.
     * @return: The minimum number of steps.
     */
    public int minDistance(String word1, String word2) {
        // write your code here
        if (word1 == null || word2 == null) {
            return 0;
        }
        
        int n = word1.length();
        int m = word2.length();        
        
        // state
        int[][] f = new int[n + 1][m + 1];
        
        // initialize
        for (int i = 0; i <= n; i++) {
            f[i][0] = i;
        }
        for (int j = 1; j <= m; j++) {
            f[0][j] = j;
        }
        
        // top down
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    f[i][j] = Math.min(f[i - 1][j - 1], 
                                       Math.min(f[i][j - 1] + 1, f[i - 1][j] + 1));
                } else {
                    f[i][j] = Math.min(f[i - 1][j - 1], 
                                       Math.min(f[i - 1][j], f[i][j - 1]))
                      				  + 1;
                } 
            }
        }
        
        // end
        return f[n][m];
    }
}
```



### 参考

1. [Edit Distance | 九章算法](http://www.jiuzhang.com/solutions/edit-distance/)