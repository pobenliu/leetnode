# Binary Tree Zigzag Level Order Traversal

Binary Tree Zigzag Level Order Traversal ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/binary-tree-zigzag-level-order-traversal/) )

```
Description
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

Example
Given binary tree {3,9,20,#,#,15,7},
    3
   / \
  9  20
    /  \
   15   7

return its zigzag level order traversal as:
[
  [3],
  [20,9],
  [15,7]
]
```



### 解题思路

#### 一、BFS（单队列）

和题目  Binary Tree Level Order Traversal 相比，不同在于相邻层结点的排列顺序相反，那么只需要设置一个标志，每到下一层时反转结点顺序即可。



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
     * @return: A list of lists of integer include 
     *          the zigzag level order traversal of its nodes' values 
     */
    public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if (root == null) {
            return result;
        }
        
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        // node_level用来记录当前处理的是哪一层根结点
        int node_level = 0;
        while (!queue.isEmpty()) {
            ArrayList<Integer> level = new ArrayList<Integer>();
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                TreeNode head = queue.poll();
                level.add(head.val);
                
                if (head.left != null) {
                    queue.offer(head.left);
                }
                if (head.right != null) {
                    queue.offer(head.right);
                }
            }
            // 结点在偶数层，按默认顺序记录；结点在奇数层，顺序反转后记录
            if (node_level % 2 == 0) {
                result.add(level);
            } else {
                Collections.reverse(level);
                result.add(level);
            }
            node_level++;
        }
        return result;
    }
}
```



#### 二、BFS（两个栈）

也可以使用两个栈来实现，这里面的入栈顺序有一些小 trick ，如果不熟悉，不太建议使用。

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
     * @return: A list of lists of integer include 
     *          the zigzag level order traversal of its nodes' values 
     */
    public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if (root == null) {
            return result;
        }
        // currLevel表示当前在处理的那一层的结点
        // nextLevel表示下一层要处理的结点
        Stack<TreeNode> currLevel = new Stack<TreeNode>();
        Stack<TreeNode> nextLevel = new Stack<TreeNode>();
        Stack<TreeNode> tmp;
        
        currLevel.push(root);
        boolean normalOrder = true;
        
        while (!currLevel.isEmpty()) {
            ArrayList<Integer> currLevelResult = new ArrayList<Integer>();
            
            while (!currLevel.isEmpty()) {
                TreeNode node = currLevel.pop();
                currLevelResult.add(node.val);
                
                if (normalOrder) {
                    if (node.left != null) {
                        nextLevel.push(node.left);
                    }
                    if (node.right != null) {
                        nextLevel.push(node.right);
                    }
                } else {
                    if (node.right != null) {
                        nextLevel.push(node.right);
                    } 
                    if (node.left != null) {
                        nextLevel.push(node.left);
                    }
                }
            }
            result.add(currLevelResult);
            // 交换currLevel和nextLevel，currLevel代表将要处理的那一层结点
            tmp = currLevel;
            currLevel = nextLevel;
            nextLevel = tmp;
            
            normalOrder = !normalOrder;
        }
        
    }
}
```





### 参考

1. [Binary Tree Zigzag Level Order Traversal | 九章算法](http://www.jiuzhang.com/solutions/binary-tree-zigzag-level-order-traversal/)