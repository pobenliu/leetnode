#  Insert Node in a Binary Search Tree

 Insert Node in a Binary Search Tree ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/) )

```
Description
Given a binary search tree and a new tree node, insert the node into the tree. You should keep the tree still be a valid binary search tree.

Notice
You can assume there is no duplicate values in this tree + node.

Example
Given binary search tree as follow, after Insert node 6, the tree should be:
  2             2
 / \           / \
1   4   -->   1   4
   /             / \ 
  3             3   6

Challenge 
Can you do it without recursion?
```



### 解题思路

#### 一、递归法



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
     * @param root: The root of the binary search tree.
     * @param node: insert this node into the binary search tree
     * @return: The root of the new binary search tree.
     */
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        // write your code here
        if (root == null) {
            return node;
        }
        
        if (node.val < root.val) {
            root.left = insertNode(root.left, node);
        } else if (node.val > root.val) {
            root.right = insertNode(root.right, node);
        }
        return root;
    }
}
```





#### 二、非递归法



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
     * @param root: The root of the binary search tree.
     * @param node: insert this node into the binary search tree
     * @return: The root of the new binary search tree.
     */
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        // write your code here
        if (root == null) {
            root = node;
            return root;
        }
        
        TreeNode tmp = root;
        TreeNode last = null;
        while (tmp != null) {
            last = tmp;
            if (tmp.val > node.val) {
                tmp = tmp.left;
            } else {
                tmp = tmp.right;
            }
        }
        if (last != null) {
            if (last.val > node.val) {
                last.left = node;
            } else {
                last.right = node;
            }
        }
        return root;
    }
}
```



### 参考

1. [Insert Node in a Binary Search Tree | 九章算法](http://www.jiuzhang.com/solutions/insert-node-in-binary-search-tree/)