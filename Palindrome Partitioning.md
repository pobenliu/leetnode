# Palindrome Partitioning

 Palindrome Partitioning  ( [leetcode](https://leetcode.com/problems/palindrome-partitioning/)  [lintcode](http://www.lintcode.com/en/problem/palindrome-partitioning/#) )

```
Description
Given a string s, partition s such that every substring of the partition is a palindrome.
Return all possible palindrome partitioning of s.

Example
Given s = "aab", return:
[
  ["aa","b"],
  ["a","a","b"]
]
```

### 解题思路

一般而言，如果题目要求所有的组合、排列、结果，都是用搜索（DFS + backtracking）来做。先来分析一下题目可能的解有多少个，如果字符串 `s` 共有 `n` 个字符，那么 `n` 个字符之间可以放置 `n - 1` 块隔板，每块隔板可能放也可能不放，所以可得到一共 `2 ^ (n-1)` 种划分情况，接下来要做的是判断哪些划分结果符合回文。

> s = " x1 | x2 | x3 | …  | x(n-1) | xn "

##### 算法复杂度

- 时间复杂度：最多有 `2^(n-1)` 个结果，遍历得到每个结果的最坏时间开销是 `O(n)` ，所以是 `O(n * 2^(n-1))`。

Java 实现

```java
public class Solution {
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<>();
        if (s == null) {
            return result;
        }
        
        List<String> path = new ArrayList<String>();
        helper(s, path, 0, result);
        
        return result;
    }
    
    private void helper(String s, 
                        List<String> path, 
                        int pos,
                        List<List<String>> result) {
        if (pos == s.length()) {
            result.add(new ArrayList<String>(path));
            return;
        }          
        
        for (int i = pos; i < s.length(); i++) {
            String prefix = s.substring(pos, i + 1);
            if (!isPalindrome(prefix)) {
                continue;
            }
            
            path.add(prefix);
            helper(s, path, i + 1, result);
            path.remove(path.size() - 1);
        }
    }
    
    private boolean isPalindrome(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (start < end) {
            if (s.charAt(start) != s.charAt(end)) {
                return false;
            }
            
            start++;
            end--;
        }
        return true;
    }
}
```



### 参考

1. [Palindrome Partitioning | 九章算法](http://www.jiuzhang.com/solutions/palindrome-partitioning/)