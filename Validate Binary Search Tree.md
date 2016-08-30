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
        // write your code here
        
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



附：自己第一次做的答案，这里面有一个 bug，记录下来提醒自己。

比如左右儿子为空的一个结点，其取值可能等于 Integer.MAX_VALUE ，此时不满足`root.val < right.minValue` ，但是，单结点是BST。

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
        // write your code here
        
        ResultType result = helper(root);
        return result.isBST;
    }
    
    private ResultType helper (TreeNode root) {
        ResultType curr = new ResultType(true, Integer.MIN_VALUE, Integer.MAX_VALUE);
        if (root == null) {
            return curr;
        }
        
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        // 当root.right为根结点，root.val == Integer.MAX_VALUE 时，此时应返回true，但是当前实现未考虑此问题
        if (left.isBST && right.isBST && (root.val > left.maxValue && root.val < right.minValue)) {
            curr.isBST = true;
        } else {
            curr.isBST = false;
            return curr;
        }
        
        curr.maxValue = Math.max(root.val, right.maxValue);
        curr.minValue = Math.min(root.val, left.minValue);
        return curr;
    }
}
```





### 参考

1. [Validate Binary Search Tree | 九章算法](http://www.lintcode.com/en/problem/validate-binary-search-tree/)