# Balanced Binary Tree

 Balanced Binary Tree ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/balanced-binary-tree/) )

```
Description
Given a binary tree, determine if it is height-balanced.
For this problem, a height-balanced binary tree is defined as a binary tree in which 
the depth of the two subtrees of every node never differ by more than 1.

Example
Given binary tree A = {3,9,20,#,#,15,7}, B = {3,#,20,15,7}
A)  3            B)    3 
   / \                  \
  9  20                 20
    /  \                / \
   15   7              15  7
The binary tree A is a height-balanced binary tree, but B is not.
```



### 解题思路

判断一个二叉树是否平衡，需要判断两个方面：

1. 左子树、右子树是否平衡。
2. 当前结点是否平衡。

所以需要两个信息，一个是树的最大深度，另一个是树是否平衡。

#### 一、ResultType

新建一个类 `ResultType` 保存 `isBalanced` 和 `maxDepth` 信息。

##### 算法复杂度

- 时间复杂度：每个结点遍历一遍，所需的时间是常数，所以总的时间复杂度为 `O(n)` 。

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
class ResultType{
    public boolean isBalanced;
    public int maxDepth;
    public ResultType(boolean isBalanced, int maxDepth) {
        this.isBalanced = isBalanced;
        this.maxDepth = maxDepth;
    }
}
 
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
    public boolean isBalanced(TreeNode root) {
        return traverse(root).isBalanced;
    }
    
    private ResultType traverse(TreeNode root) {
        if (root == null) {
            return new ResultType(true, 0);
        }
        
        ResultType left = traverse(root.left);
        ResultType right = traverse(root.right);
        
        // subtree not balance
        if (!left.isBalanced || !right.isBalanced) {
            return new ResultType(false, -1);
        }
        
        // root not balance
        if (Math.abs(left.maxDepth - right.maxDepth) > 1) {
            return new ResultType(false, -1);
        }
        
        return new ResultType(true, Math.max(left.maxDepth, right.maxDepth) + 1);
    }
    
}
```

#### 二、非ResultType

方法一中递归函数需要返回两个信息，考虑把两个信息合并到一个参数中。如果当前子树不平衡，将树的深度 `depth` 置为 `-1`，每次都对其进行判断即可。该方法很是巧妙。

##### 复杂度分析

- 时间复杂度：遍历树中的所有结点，时间复杂度 `O(n)`。
- 空间复杂度：使用了部分辅助变量，空间复杂度为 `o(1)`。

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
     * @return: True if this Binary tree is Balanced, or false.
     */
    public boolean isBalanced(TreeNode root) { 
        int depth;
        depth = maxDepth(root);
        if (depth == -1) {
            return false;
        } else {
            return true;
        }
        // 以上内容可以用一句代替
        // return maxDepth(root) != -1;
    }
    
    private int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        
        if ((left == -1) || (right == -1) || Math.abs(left - right) > 1) {
            return -1;
        }
        
        return Math.max(left, right) + 1;
    }
    
}
```



### 参考

1. [Balanced Binary Tree | 九章算法](http://www.jiuzhang.com/solutions/balanced-binary-tree/)
2. [Balanced Binary Tree | 数据结构与算法/leetcode/lintcode题解](http://algorithm.yuanbin.me/zh-hans/binary_tree/balanced_binary_tree.html)