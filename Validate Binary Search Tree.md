# Validate Binary Search Tree

Validate Binary Search Tree ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/validate-binary-search-tree/) )

```
Descriptiion
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.
- A single node tree is a BST

Example
An example:
  2
 / \
1   4
   / \
  3   5
The above binary tree is serialized as {2,1,4,#,#,3,5} (in level order).
```



### 解题思路

#### 一、分治法

在判断时需要三个信息：子树是否平衡，子树的最大值，子树的最小值。需要新建一个类型存储这三种信息。

##### 易错点

> 1. 在判断当前根结点和右子树的最小值时，考虑到空结点的最小值初始化为 Integer.MAX_VALUE，为了防止结点的值等于 Integer.MAX_VALUE，要增加右子树是否为空的判断。做了两遍都犯了这个错误。

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
     * @return: True if the binary tree is BST, or false
     */
    private class ResultType {
        boolean isBST;
        int maxValue, minValue;
        ResultType (boolean isBST, int maxValue, int minValue) {
            this.isBST = isBST;
            this.maxValue = maxValue;
            this.minValue = minValue;
        }
    }
    
    public boolean isValidBST(TreeNode root) {

        ResultType result = helper(root);
        return result.isBST;
    }
    
    private ResultType helper (TreeNode root) {
        if (root == null) {
            return new ResultType(true, Integer.MIN_VALUE, Integer.MAX_VALUE);
        }
        
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        
        if (!left.isBST || !right.isBST ) {
            return new ResultType(false, 0, 0);
        }
        if (root.left != null && left.maxValue >= root.val ||
            root.right != null && right.minValue <= root.val) {
            return new ResultType(false, 0, 0);
        }
        
        return new ResultType(true,
                              Math.max(root.val, right.maxValue),
                              Math.min(root.val, left.minValue));

    }
}
```



### 参考

1. [Validate Binary Search Tree | 九章算法](http://www.lintcode.com/en/problem/validate-binary-search-tree/)