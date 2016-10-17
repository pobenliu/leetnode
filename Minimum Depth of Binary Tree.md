#  Minimum Depth of Binary Tree

 Minimum Depth of Binary Tree  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/minimum-depth-of-binary-tree/#) )

```
Description
Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path 
from the root node down to the nearest leaf node.

Example
Given a binary tree as follow:
  1
 / \ 
2   3
   / \
  4   5
The minimum depth is 2.
```

### 解题思路

#### 一、递归

求根结点到**叶子结点**的最短路径，当前结点只有一个子结点时，不能返回 0，要接着遍历该子树，直至遇到叶子结点。

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
     * @return: An integer.
     */
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (root.left == null && root.right == null) {
            return 1;
        }
        
        if (root.left == null) {
            return minDepth(root.right) + 1;
        }
        if (root.right == null) {
            return minDepth(root.left) + 1;
        }
        
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```

另一种更简洁的写法

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
     * @return: An integer.
     */
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left  = minDepth(root.left);
        int right = minDepth(root.right);
        
        if(root.left == null || root.right == null) {
            return left >= right ? left + 1 : right + 1;
        }
        
        return Math.min(left, right) + 1;
    }
}
```



### 参考

1. [Find Minimum Depth of a Binary Tree | geeksforgeeks](http://www.geeksforgeeks.org/find-minimum-depth-of-a-binary-tree/)
2. [Minimum Depth of Binary Tree leetcode java | 爱做饭的小莹子](http://www.cnblogs.com/springfor/p/3879680.html)