# Maximum Depth of Binary Tree

 Maximum Depth of Binary Tree ( [leetcode]() [lindtcode](http://www.lintcode.com/en/problem/maximum-depth-of-binary-tree/) )

```
Description
Given a binary tree, find its maximum depth.
The maximum depth is the number of nodes along the longest path 
from the root node down to the farthest leaf node.

Example
Given a binary tree as follow:
  1
 / \ 
2   3
   / \
  4   5
The maximum depth is 3.
```



### 解题思路

遍历二叉树最自然的方法是递归，递归调用层数过多可能会导致栈空间溢出，因此，需要适当考虑递归调用的层数。

#### 一、分治法

步骤：

- 求左子树的最大深度。
- 求右子树的最大深度。
- 树的最大深度等于左、右子树的最大深度加 1。

##### 算法复杂度：

- 时间复杂度：遍历树中的所有 `n` 个结点，每个结点所涉及的操作是常数个，故总的时间复杂度为 `O(n)`。
- 空间复杂度：树的深度最大为 `n` ，最小为 `logn` ，故空间复杂度介于 `O(logn)` 和 `O(n)` 之间。

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
    public int maxDepth(TreeNode root) {
        // write your code here
        if (root == null) {
            return 0;
        }
        
        // divide
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        
        // conquer
        int depth = Math.max(left, right) + 1;
        return depth;
    }
}
```

#### 二、遍历法

步骤：

- 访问结点 `node` ，将其视为根结点。

- 结点为空，返回；

  结点非空，比较当前的结点深度 curtdepth 与当前记录的深度 depth，取两者间的最大值。

- 访问左子树，当前结点深度加一。

- 访问右子树，当前结点深度加一。

注：`trick` 在于如何在进入下一层结点时将结点深度加一。

**算法复杂度**

- 时间复杂度：遍历树中的所有结点，时间复杂度 `O(n)`。
- 空间复杂度：树的深度最大为 `n` ，最小为 `logn` ，故空间复杂度介于 `O(logn)` 和 `O(n)` 之间。

Java实现：

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
    private int depth;
    
    public int maxDepth(TreeNode root) {
        depth = 0;
        traverse(root, 1);
        return depth;
    }
    
    private void traverse(TreeNode root, int curtdepth) {
        if (root == null) {
            return;
        }
        
        if (curtdepth > depth) {
            depth = curtdepth;
        }
        
        traverse(root.left, curtdepth + 1);
        traverse(root.right, curtdepth + 1);
    }
}
```



### 参考

1. [Maximum Depth of Binary Tree | 九章算法](http://www.jiuzhang.com/solutions/maximum-depth-of-binary-tree/)
2. [Maximum Depth of Binary Tree | 数据结构与算法/leetcode/lintcode题解](http://algorithm.yuanbin.me/zh-hans/binary_tree/maximum_depth_of_binary_tree.html)