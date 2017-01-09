#  Construct Binary Tree from Preorder and Inorder Traversal

 Construct Binary Tree from Preorder and Inorder Traversal  ( [lintcode](http://www.lintcode.com/en/problem/construct-binary-tree-from-preorder-and-inorder-traversal/) )

```
Description
Given preorder and inorder traversal of a tree, construct the binary tree.

Notice
You may assume that duplicates do not exist in the tree.

Example
Given in-order [1,2,3] and pre-order [2,1,3], return a tree:
  2
 / \
1   3 
```

### 解题思路

根据前序遍历确认当前根结点，然后在中序遍历中分别寻找左子树和右子树，递归求解即可。

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
     *@param preorder : A list of integers that preorder traversal of a tree
     *@param inorder : A list of integers that inorder traversal of a tree
     *@return : Root of a tree
     */
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // write your code here
        if (preorder == null || inorder == null ||
            preorder.length != inorder.length) {
            return null;        
        }
        
        return helper(inorder, 0, inorder.length - 1, preorder, 0, preorder.length - 1);
    }
    
    private TreeNode helper(int[] inorder, int instart, int inend,
                            int[] prorder, int prstart, int prend) {
        if (instart > inend) {
            return null;
        }               
        
        TreeNode root = new TreeNode(prorder[prstart]);
        int pos = findRootPos(inorder, instart, inend, prorder[prstart]);
        
        root.left = helper(inorder, instart, pos - 1, 
                            prorder, prstart + 1, prstart + pos - instart);
        root.right = helper(inorder, pos + 1, inend,
                            prorder, prend - inend + pos + 1, prend);
        return root;
    }
    
    private int findRootPos(int[] A, int start, int end, int target) {
        for (int i = start; i <= end; i++) {
            if (A[i] == target) {
                return i;
            }
        }
        return -1;
    }
}
```

