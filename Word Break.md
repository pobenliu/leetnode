# Word Break

Word Break  ( [leetcode](https://leetcode.com/problems/word-break/)  [lintcode](http://www.lintcode.com/en/problem/word-break/) )

```
Description
Given a string s and a dictionary of words dict, determine if s can be break into a space-separated sequence of one or more dictionary words.

Example
Given s = "lintcode", dict = ["lint", "code"].
Return true because "lintcode" can be break as "lint code".
```



### 解题思路

1. 定义状态：题目求解的是否能按照字典提供的字符串集合切分字符串，那么可以定义 `canBreak[i]` 为切分前 `i` 个字符的状态，`true` 表示可以切分， `false` 表示无法完成指定切分。
2. 定义状态转移函数：`canBreak[i]` 的上一个状态是 `canBreak[j], j < i` 那么 `j + 1 ~ i` 之间的字符串属于字典集合，我们需要做的确认存在满足条件的 `canBreak[j]` 即可。
3. 定义起点：当没有字符时，将状态初始化为 `true`， `canBreak[0] = true` 。
4. 定义终点：最终的结果是指前 `n` 个字符是否可以被切分，所以是 `canBreak[n]` 。

注：

1. 这里面有一个小的优化，考虑到一个单词的长度是有限的，所以先获取字典集合中单词的最大长度。然后以切分位置的枚举为外循环，单词长度枚举为内循环
2. Java 中 `substring(start, end)` 返回的是字符串 `start ~ end - 1` 

Java 实现

```java
public class Solution {
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
    public boolean wordBreak(String s, Set<String> dict) {
        // write your code here   
        if (s == null || s.length() == 0) {
            return true;
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
            for (int lastWordLength = 1;
                     lastWordLength <= maxLength && lastWordLength <= i;
                     lastWordLength++) {
                if (!canBreak[i - lastWordLength]) {
                    continue;
                }
                String word = s.substring(i - lastWordLength, i);
                if (dict.contains(word)) {
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



