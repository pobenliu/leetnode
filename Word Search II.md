#  Word Search II

 Word Search II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/word-search-ii/) )

```
Description
Given a matrix of lower alphabets and a dictionary. 
Find all words in the dictionary that can be found in the matrix. 
A word can start from any position in the matrix and go left/right/up/down to the adjacent position. 

Example
Given matrix:
doaf
agai
dcan
and dictionary:
{"dog", "dad", "dgdg", "can", "again"}
return {"dog", "dad", "can", "again"}

dog:
doaf
agai
dcan

dad:
doaf
agai
dcan

can:
doaf
agai
dcan

again:
doaf
agai
dcan

Challenge 
Using trie to implement your algorithm.
```

### 解题思路

暴力搜索每一个单词会导致大量的重复比较，本题目主要考察单词查找树 Trie 的使用。

- 将字典中的单词依次存储在单词查找树 Trie 中
- 使用 DFS 在 board 中查找，利用 Trie 进行剪枝
- 将查找到的单词放入列表中并返回

Java 实现

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    public ArrayList<String> wordSearchII(char[][] board, ArrayList<String> words) {
        ArrayList<String> ans = new ArrayList<String>();
        
        TrieTree tree = new TrieTree(new TrieNode());
        for (String word : words) {
            tree.insert(word);
        }
        
        String res = "";
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                search(board, i, j, tree.root, ans, res);
            }
        }
        return ans;
    }
    
    class TrieNode {
        String s;
        boolean isString;
        HashMap<Character, TrieNode> subtree;
        public TrieNode() {
            // TODO Auto-generated constructor stub
            isString = false;
            subtree = new HashMap<Character, TrieNode>();
            s = "";
        }
    };
    
    class TrieTree {
        TrieNode root;
        public TrieTree (TrieNode TrieNode) {
            root = TrieNode;
        }
        public void insert (String s) {
            TrieNode now = root;
            for (int i = 0; i < s.length(); i++) {
                if (!now.subtree.containsKey(s.charAt(i))) {
                    now.subtree.put(s.charAt(i), new TrieNode());
                }
                now = now.subtree.get(s.charAt(i));
            }
            now.s = s;
            now.isString = true;
        }
        
        public boolean find (String s) {
            TrieNode now = root;
            for (int i = 0; i < s.length(); i++) {
                if (!now.subtree.containsKey(s.charAt(i))) {
                    return false;
                }
                now = now.subtree.get(s.charAt(i));
            }
            return now.isString;
        }
    };
    
    public int []dx = {1, 0, -1, 0};
    public int []dy = {0, 1, 0, -1};
    
    public void search (char[][] board, 
                        int x, int y,
                        TrieNode root,
                        ArrayList<String> ans,
                        String res) {
        if (root.isString == true) {
            if (!ans.contains(root.s)) {
                ans.add(root.s);
            }
        }
        
        if (x < 0 || x >= board.length ||
            y < 0 || y >= board[0].length ||
            board[x][y] == 0 || root == null) {
            return;
        }
        
        if (root.subtree.containsKey(board[x][y])) {
            for (int i = 0; i < 4; i++) {
                char now = board[x][y];
                board[x][y] = 0;
                search(board, x + dx[i], y + dy[i], root.subtree.get(now), ans, res);
                board[x][y] = now;
            }
        }
    }
}
```



### 参考

1. [Word Search II | 九章算法](http://www.jiuzhang.com/solutions/word-search-ii/)

