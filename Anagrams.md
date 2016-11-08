# Anagrams

Anagrams  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/anagrams/) )

```
Description
Given an array of strings, return all groups of strings that are anagrams.

Notice
All inputs will be in lower-case

Example
Given ["lint", "intl", "inlt", "code"], return ["lint", "inlt", "intl"].
Given ["ab", "ba", "cd", "dc", "e"], return ["ab", "ba", "cd", "dc"].

What is Anagram?
- Two strings are anagram if they can be the same after change the order of characters.
```



### 解题思路

#### 一、HashMap

- 对每个字符串 `string` 进行排序得到 `string'` 。
- 然后在 `HashMap` 中存储 `string' <—> List<string>` 。
  - 没有对应排序字符串 `string'` ，新建 `List` ，存储当前配对 。
  - 存在对应 `string'` ，将原始 `string` 加入对应链表。
- 最后输出所有满足条件的 `anagrams` 。

**算法复杂度**

- 时间复杂度：在 `for` 循环中有字符串排序，所以是 `N * L * O(logL)` ，其中 `N` 是字符串数组长度， `L` 是最长字符串的长度。

Java 实现

```java
public class Solution {
    /**
     * @param strs: A list of strings
     * @return: A list of strings
     */
    public List<String> anagrams(String[] strs) {
        List<String> results = new ArrayList<String>();
        if (strs == null) {
            return results;
        }
        
        HashMap<String, List<String>> map = new HashMap<String, List<String>>();
        for (int i = 0; i < strs.length; i++) {
            String s = strs[i];
            char[] chars = s.toCharArray();
            
            Arrays.sort(chars);
            String sSort = new String(chars);
            
            if (map.containsKey(sSort)) {
                map.get(sSort).add(s);
            } else {
                List<String> list = new ArrayList<String>();
                list.add(s);
                map.put(sSort, list);
            }
        }
        
        for (Map.Entry<String, List<String>> entry : map.entrySet()) {
            List<String> list = entry.getValue();
            if (list.size() > 1) {
                results.addAll(list);
            }
        }
        
        return results;
        
    }
}
```



#### 二、HashMap 2

思路同方法一类似，只是对 `string` 排序进行了优化，改为了对每个 `string` 求哈希值。

**算法复杂度**

- 时间复杂度：求哈希值函数的复杂度是 `O(L)` ，`L` 为最长字符串的长度。所以算法复杂度优化为 `N * O(L)` ，`N` 为字符串数组长度。

Java 实现

```java
public class Solution {
    /**
     * @param strs: A list of strings
     * @return: A list of strings
     */
    public List<String> anagrams(String[] strs) {
        List<String> results = new ArrayList<String>();
        if (strs == null) {
            return results;
        }
        
        HashMap<Integer, ArrayList<String>> map = new HashMap<Integer, ArrayList<String>>();
        
        for (String str : strs) {
            int[] count = new int[26];
            for (int i = 0; i < str.length(); i++) {
                count[str.charAt(i) - 'a']++;
            }
            
            int hash = getHash(count);
            if (!map.containsKey(hash)) {
                map.put(hash, new ArrayList<String>());
            }
            map.get(hash).add(str);
        }
        
        for (ArrayList<String> tmp : map.values()) {
            if (tmp.size() > 1) {
                results.addAll(tmp);
            }
        }
        
        return results;
    }
    
    private int getHash(int[] count) {
        int hash = 0;
        int a = 378551;
        int b = 63689;
        for (int num : count) {
            hash = hash * a + num;
            a = a * b;
        }
        return hash;
    }
}
```



### 参考

1. [Anagrams.java | shawnfan/LintCode](https://github.com/shawnfan/LintCode/blob/master/Java/Anagrams.java)
2. [Anagrams | 九章算法](http://www.jiuzhang.com/solutions/anagrams/)

