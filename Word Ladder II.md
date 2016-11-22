#  Word Ladder II

 Word Ladder II  ( [leetcode](https://leetcode.com/problems/word-ladder-ii/)  [lintcode](http://www.lintcode.com/en/problem/word-ladder-ii/#) )

```
Description
Given two words (start and end), and a dictionary, 
find all shortest transformation sequence(s) from start to end, such that:
1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

Notice
- All words have the same length.
- All words contain only lowercase alphabetic characters.

Example
Given:
start = "hit"
end = "cog"
dict = ["hot","dot","dog","lot","log"]
Return
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```

### 解题思路

要列出所有可能的单词变换路径，需要使用搜索（DFS），又要求所有路径都是最短的，所以需要使用 BFS。具体思路是：

- 使用 BFS 寻找从 `end` 出发到 `start` 的最短路径，并记录途径单词至 `end` 的距离。
- 根据每个单词到 `end` 的距离，搜索所有最短的单词路径。

```
以本题为例，从终点 cog 开始搜索至起点 hit 的最短路径，记录途径单词的距离
 hit | hot | lot | log | cog
     |     | dot | dog |    
  4     3     2     1     0 
```

##### 算法复杂度

- 时间复杂度：`O(m)`。其中 `m, m <= n^2` 为边的数量，`n` 为点的数量。

Java 实现

```java
public class Solution {
    /**
      * @param start, a string
      * @param end, a string
      * @param dict, a set of string
      * @return a list of lists of string
      */
    public List<List<String>> findLadders(String start, String end, Set<String> dict) {
        List<List<String>> ladders = new ArrayList<List<String>>();
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        Map<String, Integer> distance = new HashMap<String, Integer>();
        
        dict.add(start);
        dict.add(end);
        
        bfs(map, distance, start, end, dict);
        
        List<String> path = new ArrayList<String>();
        
        dfs(ladders, path, start, end, distance, map);
        
        return ladders;
    }
    
    private void bfs(Map<String, List<String>> map,
                        Map<String, Integer> distance,
                        String start, String end,
                        Set<String> dict) {
        Queue<String> q = new LinkedList<String>();
        q.offer(end);
        distance.put(end, 0);
        for (String s : dict) {
            map.put(s, new ArrayList<String>());
        }
        
        while (!q.isEmpty()) {
            String word = q.poll();
            List<String> nextWords = expand(word, dict);
            for (String nextWord : nextWords) {
                map.get(nextWord).add(word);
                if (!distance.containsKey(nextWord)) {
                    distance.put(nextWord, distance.get(word) + 1);
                    q.offer(nextWord);
                }
            }
        }
    }
    
    private List<String> expand(String word, Set<String> dict) {
        List<String> expansion = new ArrayList<String>();
        
        for (int i = 0; i < word.length(); i++) {
            for (char c = 'a'; c <= 'z'; c++) {
                if (c != word.charAt(i)) {
                    String expanded = word.substring(0, i) + c 
                                        + word.substring(i + 1);
                    if (dict.contains(expanded)) {
                        expansion.add(expanded);
                    }
                }
            }
        }
        return expansion;
    }
    
    void dfs(List<List<String>> ladders,
                List<String> path,
                String curt, String end,
                Map<String, Integer> distance,
                Map<String, List<String>> map) {
        
        path.add(curt);
        if(curt.equals(end)) {
            ladders.add(new ArrayList<String>(path));
        } else {
            for (String next : map.get(curt)) {
                if (distance.containsKey(next) && distance.get(curt) == distance.get(next) + 1) {
                    dfs(ladders, path, next, end, distance, map);
                }
            }
        }
        path.remove(path.size() - 1);
    }
}
```



### 参考

1. [Word Ladder II | 九章算法](http://www.jiuzhang.com/solutions/word-ladder-ii/)