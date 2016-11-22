# Word Ladder

 Word Ladder  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/word-ladder/) )

```
Description
Given two words (start and end), and a dictionary, 
find the length of shortest transformation sequence from start to end, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

Notice
- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.

Example
Given:
start = "hit"
end = "cog"
dict = ["hot","dot","dog","lot","log"]
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

### 解题思路

本题目主要考察图的宽度优先搜索（ BFS ），我们来思考一下问题如何抽象。

1. 将每个单词看成无权图中的一个结点。
2. 如果单词 `s1` 改变一个字符可以变成 `s2` （ `s1` 和 s2 均在字典中），则 `s1` 和 `s2` 之间有连接（双向连接，权重相等皆为 `1` ）
3. 给定 `s1` 和 `s2` ，问题求解的是图中从 `s1` 到 `s2` 的最短路径长度。

在使用 BFS 求最短路径时，有几个关键步骤：

1. 如何找到与当前结点相邻的所有结点。

   有两个策略可以选择：

   - 遍历整个字典，将其中每个单词与当前单词比较，判断是否只差一个字符。时间复杂度为 `O(n * w)` ， `n` 为字典中单词数量， `w` 为单词长度。
   - 遍历当前单词的每个字符 `x` ，将其改为 `a~z` 中除 `x` 外的任意一个，判断字典中是否包含新单词。时间复杂度为 `O(26 * w)` ，`w` 为单词长度。

   一个常见的英语单词的长度一般小于 100 ，而字典中的单词数则往往成千上万，所以第二种策略相对较优。

2. 如何标记一个结点已经被访问过，以避免重复访问。

   可以将访问过的单词从字典中删除，或使用哈希表保存已访问过的单词。

3. 一旦 BFS 找到目标单词，如何回溯找到路径？

##### 易错点

- `String` 变量可以直接使用 `char[]` 进行初始化。

Java 实现

```java
public class Solution {
    /**
      * @param start, a string
      * @param end, a string
      * @param dict, a set of string
      * @return an integer
      */
    public int ladderLength(String start, String end, Set<String> dict) {
        if (start == null || end == null || 
            start.length() == 0 || end.length() == 0) {
            return 0;        
        }
        assert(start.length() == end.length());
        if (dict == null || dict.size() == 0) {
            return 0;
        }
        
        if (start.equals(end)) {
            return 1;
        }
        
        dict.add(start);
        dict.add(end);
        
        HashSet<String> hash = new HashSet<String>();
        Queue<String> queue = new LinkedList<String>();
        queue.offer(start);
        hash.add(start);
        
        int length = 1;
        while (!queue.isEmpty()) {
            length++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String word = queue.poll();
                for (String nextWord: getNextWords(word, dict)) {
                    if (hash.contains(nextWord)) {
                        continue;
                    }
                    if (nextWord.equals(end)){
                        return length;
                    }
                    hash.add(nextWord);
                    queue.offer(nextWord);
                }
            }
        }
        return 0;
    }
    
    // get connections with given word.
    // for example, given word = 'hot', dict = {'hot', 'hit', 'hog'}
    // it will return ['hit', 'hog']
    private ArrayList<String> getNextWords(String word, Set<String> dict) {
        ArrayList<String> nextWords = new ArrayList<String>();
        for (char c = 'a'; c <= 'z'; c++) {
            for (int i = 0; i < word.length(); i++) {
                if (c == word.charAt(i)) {
                    continue;
                }
                String nextWord = replace(word, i, c);
                if (dict.contains(nextWord)) {
                    nextWords.add(nextWord);
                }
            }
        }
        return nextWords;
    }
    
    // replace character of a string at given index to a given character
    // return a new string
    private String replace(String s, int index, char c) {
        char[] chars = s.toCharArray();
        chars[index] = c;
        return new String(chars);
    }
}
```



### 参考

1. [Word Ladder | 九章算法](http://www.jiuzhang.com/solutions/word-ladder/)
2. [[LeetCode] Word Ladder I, II | 喜刷刷](http://bangbingsyb.blogspot.jp/2014/11/leetcode-word-ladder-i-ii.html)