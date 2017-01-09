#  Subtree

 Subtree  ( [lintcode](http://www.lintcode.com/en/problem/subtree/) )

```
Description
You have two every large binary trees: T1, with millions of nodes, and T2, with hundreds of nodes. 
Create an algorithm to decide if T2 is a subtree of T1.

Notice
A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. 
That is, if you cut off the tree at node n, the two trees would be identical.

Example
T2 is a subtree of T1 in the following case:
       1                3
      / \              / 
T1 = 2   3      T2 =  4
        /
       4

T2 isn't a subtree of T1 in the following case:
       1               3
      / \               \
T1 = 2   3       T2 =    4
        /
       4
```

### 解题思路

使用递归的思路，首先判断 `T2` 和 `T1` 是否相等，如果不相等则接着让 `T2` 和 `T1` 左子树、右子树比较。这里面非常重要的一点是判断最基本的情况。

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
     * @param T1, T2: The roots of binary tree.
     * @return: True if T2 is a subtree of T1, or false.
     */
    public boolean isSubtree(TreeNode T1, TreeNode T2) {
        // write your code here
        if (T2 == null) {
            return true;
        }
        if (T1 == null) {
            return false;
        }
        
        if (isEqual(T1, T2)) {
            return true;
        }
        
        // if the tree with root as current node doesn't match then
        // try left and right subtrees one by one
        return isSubtree(T1.left, T2) || isSubtree(T1.right, T2);
    }
    
    private boolean isEqual(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) {
            return true;
        }
        if (root1 == null || root2 == null) {
            return false;
        }
        
        // check if the data of both roots is same and 
        // data of left and right subtrees are also same
        return root1.val == root2.val &&
                isEqual(root1.left, root2.left) &&
                isEqual(root1.right, root2.right);
    }
    
}
```

