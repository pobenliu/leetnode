# Word Break

Word Break  ( [leetcode](https://leetcode.com/problems/word-break/)  [lintcode](http://www.lintcode.com/en/problem/word-break/) )

```
Description
Given a string s and a dictionary of words dict, 
determine if s can be break into a space-separated sequence of one or more dictionary words.

Example
Given s = "lintcode", dict = ["lint", "code"].
Return true because "lintcode" can be break as "lint code".
```



### 解题思路

动态规划。

1. 定义状态：题目求解的是否能按照字典提供的字符串集合切分字符串，那么可以定义 `canBreak[i]` 为切分前 `i` 个字符的状态，`true` 表示可以切分， `false` 表示无法完成指定切分。
2. 定义状态转移函数：`canBreak[i]` 的上一个状态是 `canBreak[j], j < i` ，如果 `j + 1 ~ i` 之间的字符串在字典里，且 `canBreak[j] == true`，那么 `canBreak[i] = true`。由于需要判断 `canBreak[j]`，在初始化时要先置为 `false`。
3. 定义起点：当没有字符时，将状态初始化为 `true`， `canBreak[0] = true` 。
4. 定义终点：最终的结果是指前 `n` 个字符是否可以被切分，所以是 `canBreak[n]` 。

注意：

1. 程序优化：考虑到一个单词的长度是有限的，所以先获取字典集合中单词的最大长度。然后以切分位置的枚举为外循环，单词长度枚举为内循环。
2. Java 中 `substring(start, end)` 返回的是字符串起始位置是 `start ~ end - 1` 。
3. 内层循环中变量是单词长度，要注意单词长度和上一个切分字符串长度之间的切换。

Java 实现

```java
public class Solution {
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
    public boolean wordBreak(String s, Set<String> dict) {
	    if (s.length() == 0 && dict.size() == 0) {
            return true;
        } else if (s == null || s.length() == 0 ||
                    dict == null || dict.size() == 0) {
            return false;
        }  
        
        // state
        int n = s.length();
        boolean[] canBreak = new boolean[n + 1];
        
        // initialize
        canBreak[0] = true;
        int maxLength = getMaxLength(dict);
        
        // top down
        for (int i = 1; i <= n; i++) {
            canBreak[i] = false;
            for (int wordLen = 1; wordLen <= maxLength && wordLen <= i; wordLen++) {
                // whether (i-wordLen+1, i) is a word? 
                // corrisponding to (i-wordLen, i-1) in string s
                String word = s.substring(i - wordLen, i);
                if (canBreak[i - wordLen] && dict.contains(word)) {
                    canBreak[i] = true;
                    break;
                }
            }
        }
        
        // end
        return canBreak[n];
    }
    
    private int getMaxLength (Set<String> dict) {
        int maxLength = 0;
        for (String word : dict) {
            maxLength = Math.max(maxLength, word.length());
        }
        return maxLength;
    }
}
```





### 参考

1. [Word Break | 九章算法](http://www.jiuzhang.com/solutions/word-break/)

