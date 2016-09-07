# Palindrome Partitioning II

Palindrome Partitioning II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/palindrome-partitioning-ii/) )

```
Description
Given a string s, cut s into some substrings such that every substring is a palindrome.
Return the minimum cuts needed for a palindrome partitioning of s.

Example
Given s = "aab",
Return 1 since the palindrome partitioning ["aa", "b"] could be produced using 1 cut.
```



### 解题思路

#### 一、动态规划

1. 定义状态：题目求解的是切分字符串的最小数量，那么可以定义 `minCut[i]` 为切分前 `i` 个字符所组成的字符串所需要的最少切分次数。
2. 定义状态转移函数：`minCut[i]` 的上一个状态是 `minCut[j], j < i` 那么 `j + 1 ~ i` 之间的字符也是回文的，我们需要做的是找到满足条件的最小的 `minCut[j]` 。
3. 定义起点：当只有一个字符时，无需切分即满足回文 `minCut[i]` 。当长度为 `n` 的字符串中不存在回文字符串时， `minCut[i] = i - 1` ，作为初始化值。此处 `minCut[0] = -1` 。
4. 定义终点：最终的结果是指前 `n` 个字符组成的字符串切分最少次数，所以是 `minCut[n]` 。

由于程序实现中存在双重循环，如果在循环中对字符串是否回文进行判断，会明显提升时间复杂度，所以使用额外空间，提前处理字符串，获取子字符串的回文状态。在预处理子字符串回文状态的时候，也使用了动态规划的思路。

**算法复杂度**

- 时间复杂度：
- 空间复杂度：

Java 实现

```java
public class Solution {
    /**
     * @param s a string
     * @return an integer
     */
    public int minCut(String s) {
        // write your code herej
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        // preparation
        // isPalindrome[start][length] 
        // start: start of the substring
        // length: length of the substring
        boolean[][] isPalindrome = getIsPalindrome(s);
        
        // state
        int n = s.length();
        // minCut[i]: how many cuts do we need to cut the i chars into palindrome substrings
        int[] minCut = new int [n + 1];
        
        // initialize
        for (int i = 0; i <= n; i++) {
            minCut[i] = i - 1;    
        }
        
        // top down
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                if ( isPalindrome[j][i - 1] ) {
                    // find the minimum minCut[j], for j < i
                    minCut[i] = Math.min(minCut[i], minCut[j] + 1);
                }
            }
        }
        
        // end
        return minCut[n];
    }
    
    private boolean isPalindrome (String s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }
    
    private boolean[][] getIsPalindrome (String s) {
        int n = s.length();
        boolean[][] isPalindrome = new boolean[n][n];
        
        // when only one char is processed
        for (int i = 0; i < n; i++) {
            isPalindrome[i][i] = true;
        }
        
        // when only two chars are processed
        for (int i = 0; i < n - 1; i++) {
            isPalindrome[i][i + 1] = (s.charAt(i) == s.charAt(i + 1));
        }
        
        for (int length = 2; length < n; length++) {
            for (int start = 0; start + length < n; start++) {
                isPalindrome[start][start + length]		// method of dynamic programming
                    = isPalindrome[start + 1][start + length - 1] &&
                      (s.charAt(start) == s.charAt(start + length));
            }
        }
        
        return isPalindrome;
    }
}
```



### 参考

1. [Palindrome Partitioning II | 九章算法](http://www.jiuzhang.com/solutions/palindrome-partitioning-ii/)