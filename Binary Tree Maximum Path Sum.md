# Binary Tree Maximum Path Sum

 Binary Tree Maximum Path Sum ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/binary-tree-maximum-path-sum/) )

```
Description
Given a binary tree, find the maximum path sum.
The path may start and end at any node in the tree.

Example
Given the below binary tree:
  1
 / \
2   3
return 6.
```



### 解题思路

#### 一、分治法

首先将题目简化，如果是求从根结点出发的最大路径长度，那么直接使用递归法即可：

- 求左子树的最大路径长度。
- 求左子树的最大路径长度。
- 取以上两者的最大值加上当前根结点数值，即为当前根结点的最大路径长度。

现在求解任意结点出发的最大路径，与上述情况相比，多了一种情况：跨越根结点的路径，这里的根结点不一定是整棵树的根结点，也可能只存在于左子树、或右子树。除了比较左、右子树的最大路径长度，还需要比较增加了根结点之后的总路径长度。

所以递归函数需要保存两个信息：子树的最大路径长度、子树中跨越“根结点”路径的最大长度

##### 算法复杂度

- 时间复杂度：每个结点遍历一遍，每个结点的操作是常数个，所以时间复杂度是 `O(n)`。
- 空间复杂度：使用常数个辅助变量保存参数，空间复杂度为 `O(1)`。

注意：

> 1. 题目中未明确说明结点值的正负，所以要考虑结点值为负的情况，一个典型的输入是只含一个结点{-1}。那么 `singlePath` 的值可能为负，这种情况我们可以直接舍弃该子树，认为 `singlePath` 为 `0`。
> 2. 关于空结点 singlePath 和 maxPath 的初始化取值，可以从实际意义上理解。目前感觉还讲不清楚，可参考方法一和方法二具体实现。

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
     * @param root: The root of binary tree.
     * @return: An integer.
     */
    private class ResultType {
        // singlePath：从root往下走到任意点的最大路径值，这条路径可以不包含任何点
        // maxPath：从树中任意点到任意点的最大路径，这条路径至少包含一个点
        int singlePath, maxPath;
        ResultType (int singlePath, int maxPath) {
            this.singlePath = singlePath;
            this.maxPath = maxPath;
        }
    } 
     
    public int maxPathSum(TreeNode root) {
        ResultType result = helper(root);
        return result.maxPath;
    }
    
    private ResultType helper(TreeNode root) {
        if (root == null) {
            // maxPath 取 MIN_VALUE 说明无满足条件路径
            return new ResultType(0, Integer.MIN_VALUE); 
        }
        // Divide
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        
        // Conquer
        int singlePath = Math.max(left.singlePath, right.singlePath) + root.val;
        singlePath = Math.max(singlePath, 0);	// 舍弃路径值为负数的部分
        
        int maxPath = Math.max(left.maxPath, right.maxPath);
        // 至少含有当前根节点的值
        maxPath = Math.max(maxPath, left.singlePath + right.singlePath + root.val);
        
        return new ResultType(singlePath, maxPath);
    }

}
```



#### 二、分治法 II

和解法一类似，具体操作上有些区别。暂时还不是很清楚这么做有什么好处。

Java 实现

```java
// Version 2:
// SinglePath也定义为，至少包含一个点。
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: An integer.
     */
    private class ResultType {
        int singlePath, maxPath;
        ResultType(int singlePath, int maxPath) {
            this.singlePath = singlePath;
            this.maxPath = maxPath;
        }
    }
  
    public int maxPathSum(TreeNode root) {
        ResultType result = helper(root);
        return result.maxPath;
    }
  
    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(Integer.MIN_VALUE, Integer.MIN_VALUE);
        }
        // Divide
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);

        // Conquer
        // 与解法一的区别主要在这里
        // singlePath 至少含一个结点就意味着这个结点的值可能为负值
        int singlePath =
            Math.max(0, Math.max(left.singlePath, right.singlePath)) + root.val;

        int maxPath = Math.max(left.maxPath, right.maxPath);
        // 由于singlePath可能为负值，所以maxPath需要检查并舍弃这种情况
        maxPath = Math.max(maxPath,
                           Math.max(left.singlePath, 0) + 
                           Math.max(right.singlePath, 0) + root.val);

        return new ResultType(singlePath, maxPath);
    }
}
```



### 参考

1. [Binary Tree Maximum Path Sum | 九章算法](http://www.jiuzhang.com/solutions/binary-tree-maximum-path-sum/) 