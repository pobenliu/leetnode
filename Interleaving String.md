# Interleaving String

Interleaving String  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/interleaving-string/) )

```
Description
Given three strings: s1, s2, s3, determine 
whether s3 is formed by the interleaving of s1 and s2.

Example
For s1 = "aabcc", s2 = "dbbca"
When s3 = "aadbbcbcac", return true.
When s3 = "aadbbbaccc", return false.

Challenge 
O(n2) time or better
```



### 解题思路

1. 定义状态：可定义 `f[i][j][k]` 为 `s1` 的前 `i` 个字符和 `s2` 的前 `j` 个字符是否能组成 `s3` 的前 `k` 个字符串，考虑到必然有 `i + j = k` ，所以第三个维度 `k` 可略去，直接定义 `f[i][j]` 即可。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，详细讨论如下。
   - `s1.charAt(i - 1) == s3.charAt(i + j - 1)` 时，那么对应的上一个状态为 `f[i - 1][j]` ，如果上个状态为 `true` ，那么 `f[i][j] = true` 。
   - `s2.charAt(i - 1) == s3.charAt(i + j - 1)` 时，那么对应的上一个状态为 `f[i][j - 1]` ，如果上个状态为 `true` ，那么 `f[i][j] = true` 。
3. 定义起点：
   - <u>**易错点：又一次没考虑清楚初始化情况，这里需要专门说明一下。对于二维的动规问题，`f[i][0]` 和 `f[0][j]` 都是边界条件，需要进行初始化，初始化时要同时注意边界的状态转移。**</u>
   - 当 `s1` 为空，`s2` 不为空时，依次判断 `s2` 与 `s3` 对应字符是否相等，同时还要考虑上一个状态是否为 `true` 完成初始化。
   - 当 `s2` 为空，`s1` 不为空时，依次判断 `s1` 与 `s3` 对应字符是否相等完成初始化，同时还要考虑上一个状态是否为 `true` 。
4. 定义终点：最终结果即为 `f[n][m]` 。注：初始化时也需要注意取值范围是数组长度加一。

易错点：

> 1. 在初始化时，比较 `s1` 和 `s3` 时，为何不能用 `s1.substring(0, i) == s3.substring(0, i)` ？

Java 实现

```java
public class Solution {
    /**
     * Determine whether s3 is formed by interleaving of s1 and s2.
     * @param s1, s2, s3: As description.
     * @return: true or false.
     */
    public boolean isInterleave(String s1, String s2, String s3) {
        // write your code here
        
        int lens1 = s1.length();
        int lens2 = s2.length();
        int lens3 = s3.length();
        
        if ((s1 == null || lens1 == 0) && s2 == s3) {
            return true;
        }
        if ((s2 == null || lens2 == 0) && s1 == s3) {
            return true;
        }
        
        if (lens1 + lens2 != lens3) {
            return false;
        }
        
        // state
        boolean[][] f = new boolean[lens1 + 1][lens2 + 1];
        
        // initialize
        f[0][0] = true;
        for (int i = 1; i <= lens1; i++) {
            if (s1.charAt(i - 1) == s3.charAt(i - 1) && f[i - 1][0]) {
                f[i][0] = true;
            } else {
                f[i][0] = false;
            }
        }
        for (int j = 1; j <= lens2; j++) {
            if (s2.charAt(j - 1) == s3.charAt(j - 1) && f[0][j - 1]) {
                f[0][j] = true;
            } else {
                f[0][j] = false;
            }
        }
        
        
        // top down
        for (int i = 1; i <= lens1; i++) {
            for (int j = 1; j <= lens2; j++) {
                if (f[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1) ||
                    f[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1)) {
                    f[i][j] = true;
                }   
            }
        }
        
        // end
        return f[lens1][lens2];
    }
}
```





### 参考

1. [Interleaving String | 九章算法](http://www.jiuzhang.com/solutions/interleaving-string/)