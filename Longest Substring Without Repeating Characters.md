#  Longest Substring Without Repeating Characters

 Longest Substring Without Repeating Characters  ( [lintcode](http://www.lintcode.com/en/problem/longest-substring-without-repeating-characters/) )

```
Description
Given a string, find the length of the longest substring without repeating characters.

Example
For example, the longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3.
For "bbbbb" the longest substring is "b", with the length of 1.

Challenge 
O(n) time
```

### 解题思路

#### 解法一：标记数组

题目求解的是无重复字母的最长子串，考虑到字母可以使用 ASCII （ 7 bits）表示，可以建立一个数组保存已访问过的字符，然后使用指针 `i` 表示起始位置，`j` 表示当前位置，当 `j` 指向的字符已经被访问时，向前移动 `i` 。

Java 实现

```java
public class Solution {
    /**
     * @param s: a string
     * @return: an integer 
     */
    public int lengthOfLongestSubstring(String s) {
        // write your code here
        if (s == null || s.length() == 0) {
            return 0;
        }
        // map from character's ASCII to its last occured index
        int[] map = new int[256];
        int i = 0;
        int j = 0;
        int ans = 0;
        for (i = 0; i < s.length(); i++) {
            while (j < s.length() && map[s.charAt(j)] == 0) {
                map[s.charAt(j)] = 1;
                ans = Math.max(ans, j-i+1);
                j++;
            }
            map[s.charAt(i)] = 0;
         }
         
         return ans;
    }
}
```

#### 解法二：HashSet

相比解法一，稍微有点繁琐。

```java
public class Solution {
    /**
     * @param s: a string
     * @return: an integer 
     */
    public int lengthOfLongestSubstring(String s) {
        // write your code here
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int start = 0;
        int max = 0;
        
        HashSet<Character> set = new HashSet<Character>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (!s.contains(c)) {
                set.add(c);
                max = Math.max(max, i - start + 1);
            } else {
                for (int j = start; j < i; j++) {
                    set.remove(s.charAt(j));
                    if (s.charAt(j) == c) {
                        start = j + 1;
                        break;
                    }
                }
            }
            set.add(c);
        }
        
        return max;
    }
}
```



### 参考

1. [Longest Substring Without Repeating Characters | 九章算法](http://www.jiuzhang.com/solutions/longest-substring-without-repeating-characters/)