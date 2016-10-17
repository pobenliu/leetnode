# Binary Tree Inorder Traversal

 Binary Tree Inorder Traversal  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/binary-tree-inorder-traversal/) )

```
Description
Given a binary tree, return the inorder traversal of its nodes' values.

Example
Given binary tree {1,#,2,3},
   1
    \
     2
    /
   3
return [1,3,2].

Challenge 
Can you do it without recursion?
```

### 解题思路

#### 一、递归

##### 1.遍历

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
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // traverse
        ArrayList<Integer> result = new ArrayList<Integer>();
        traverse(root, result);
        return result;
    }
    
    private void traverse (TreeNode root, ArrayList<Integer> result) {
        if (root == null) {
            return;
        }
        traverse(root.left, result);
        result.add(root.val);
        traverse(root.right, result);
    }
}
```



##### 2.分治

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
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // divide & conquer
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        // divide
        ArrayList<Integer> left  = inorderTraversal(root.left);
        ArrayList<Integer> right = inorderTraversal(root.right);
        
        // conquer
        result.addAll(left);
        result.add(root.val);
        result.addAll(right);
        
        return result;
    }
}
```



#### 二、迭代

可参考 Binary Tree Preorder Traversal 迭代法方法二的实现，只需要把结点输出至结果的顺序调整一下即可。

使用迭代遍历二叉树一定要使用栈，可以画图理解结点应该进栈顺序、出栈时间，这里借用 ProgramCreek 的图示作为参考。

![](http://ww2.sinaimg.cn/mw690/600e6311jw1f8uf1dzwndj20a708wgm3.jpg)

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
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // non-recursion
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode node = root;
        
        while (!stack.empty() || node != null) {
            // if it is not null, push to stack and go down the tree to left
            if (node != null) {
                stack.push(node);
                node = node.left;
            } else {
            // if no left child, pop stack, process the node then let node point to the right
                TreeNode tmp = stack.pop();
                result.add(tmp.val);
                node = tmp.right;
            }
        }
        return result;
    }
    
}
```



### 参考

1. [Leetcode – Binary Tree Inorder Traversal (Java) | ProgramCreek](http://www.programcreek.com/2012/12/leetcode-solution-of-binary-tree-inorder-traversal-in-java/)