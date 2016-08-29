#  Lowest Common Ancestor

Lowest Common Ancestor ( [leetcode]() [lindcode](http://www.lintcode.com/en/problem/lowest-common-ancestor/) )

```
Description
Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

Notice
Assume two nodes are exist in tree.

Example
For the following binary tree:
  4
 / \
3   7
   / \
  5   6
LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7
```



### 解题思路

#### 一、分治法

在二叉树中寻找结点 A 和 B 的 LCA ：

- 如果找到了就返回该结点
- 如果只找到 A，返回 A
- 如果只找到 B，返回 B
- 如果都没有找到，就返回 null

**算法复杂度**

- 时间复杂度：每个结点遍历一遍，每个结点的操作是常数个，所以时间复杂度是`O(n)`
- 空间复杂度：使用了常数个辅助变量保存参数，空间复杂度为`O(1)`

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
     * @param root: The root of the binary search tree.
     * @param A and B: two nodes in a Binary.
     * @return: Return the least common ancestor(LCA) of the two nodes.
     */
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {
        // write your code here
        if (root == null || root == A || root == B) {
            return root;
        }
        
        // Divide
        TreeNode left = lowestCommonAncestor(root.left, A, B);
        TreeNode right = lowestCommonAncestor(root.right, A, B);
        
        // Conquer
        // 只有在找到A或B的情况下返回值才不为null，所以以此为条件判断是否找到LCA
        if (left != null && right != null) {
            return root;
        }
        if (left != null) {
            return left;
        }
        if (right != null) {
            return right;
        }
        return null;
    }
}
```





### 参考

1. [Lowest Common Ancestor | 九章算法](http://www.jiuzhang.com/solutions/lowest-common-ancestor/)

