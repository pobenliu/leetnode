# Binary Tree Level Order Traversal

 Binary Tree Level Order Traversal ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/binary-tree-level-order-traversal/) )

```
Description
Given a binary tree, return the level order traversal of its nodes' values. 
(ie, from left to right, level by level).

Example
Given binary tree {3,9,20,#,#,15,7},
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]

Challenge 
Challenge 1: Using only 1 queue to implement it.
Challenge 2: Use DFS algorithm to do it.
```



### 解题思路

#### 一、单队列实现（BFS）

 队列中记录每一层的结点，获取当前队列大小，进行分层处理。

- 获取队列中当前层的结点数。
- 对于当前队列中该层的所有结点。
  - 取出当前队列中的头结点，取其值。
  - 如果有左儿子，加入队列。
  - 如果有右儿子，加入队列。

##### 算法复杂度

- 时间复杂度：每个结点遍历一遍，每个结点的操作是常数个，所以时间复杂度是 `O(n)`。
- 空间复杂度：使用了常数个辅助变量保存参数，空间复杂度为 `O(1)`。

##### 易错点

> 1. 在初始化 `Queue` 类型时，要用 `LinkedList`，这是因为在 `Java` 里 `Queue` 是一个接口，不能直接实例化，需要通过已实现 `Queue` 接口的类（如 `Linkedlist`）来构造。

Java 实现：

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
   
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: Level order a list of lists of integer
     */
    public ArrayList<ArrayList<Integer>> levelOrder(TreeNode root) {
        ArrayList result = new ArrayList();
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            ArrayList<Integer> level = new ArrayList<Integer>();
            // 获取当前层的结点个数
            int size = queue.size();
            // 依次取出当前层的结点，并将其左儿子、右儿子放进队列
            // 下一层结点依次加入队列 
            for (int i = 0; i < size; i++) {
                TreeNode head = queue.poll();
                level.add(head.val);
                
                if (head.left != null) {
                    queue.offer(head.left);
                }
                if (head.right != null) {
                    queue.offer(head.right);
                }
            } // end for 
            // 将当前层结点对应的一些列值放入result
            result.add(level);
        } // end while
        
        return result;
    }
}
```

#### 二、DFS

DFS 每次都是在纵向增加层级，需要做的是每次在遍历某一层的时候将该层的结点全部加入到数组中。

Java 实现

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
 
   
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: Level order a list of lists of integer
     */
    public ArrayList<ArrayList<Integer>> levelOrder(TreeNode root) {
        // write your code here
        ArrayList<ArrayList<Integer>> results = new ArrayList<ArrayList<Integer>>();
        
        if (root == null) {
            return results;
        }
        
        int maxLevel = 0;
        while (true) {
            ArrayList<Integer> level = new ArrayList<Integer>();
            // 将 maxLevel 层的结点放入 level
            dfs(root, level, 0, maxLevel);
            // 在 maxLevel 层找不到结点时结束循环
            if (level.size() == 0) {
                break;
            }
            results.add(level);
            maxLevel++;
        }
        return results;
    }
    
    private void dfs (TreeNode root,
                      ArrayList<Integer> level,
                      int curtLevel,
                      int maxLevel) {
        if (root == null || curtLevel > maxLevel) {
            return;
        }
        // 在遍历到 maxLevel 层时，将结点加入队列
        if (curtLevel == maxLevel) {
            level.add(root.val);
            return;
        }
        
        dfs(root.left, level, curtLevel + 1, maxLevel);
        dfs(root.right, level, curtLevel + 1, maxLevel);
    }
}
```

#### 三、双队列（BFS）

#### 四、单队列 + 哑结点（BFS）



### 参考

1. [Binary Tree Level Order Traversal | 九章算法](http://www.jiuzhang.com/solutions/binary-tree-level-order-traversal/)
2. [How do I instantiate a Queue object in java? | stackoverflow](http://stackoverflow.com/questions/4626812/how-do-i-instantiate-a-queue-object-in-java)