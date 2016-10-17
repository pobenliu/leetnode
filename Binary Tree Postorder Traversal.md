# Binary Tree Postorder Traversal

 Binary Tree Postorder Traversal  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/binary-tree-postorder-traversal/) )

```
Description
Given a binary tree, return the postorder traversal of its nodes' values.

Example
Given binary tree {1,#,2,3},
   1
    \
     2
    /
   3
 
return [3,2,1].

Challenge 
Can you do it without recursion?
```

### 解题思路

参考 Binary Tree Preorder Traversal 。

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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        // traverse
        ArrayList<Integer> result = new ArrayList<Integer>();
        traverse(root, result);
        return result;
    }
    
    private void traverse(TreeNode root, ArrayList<Integer> result) {
        if (root == null) {
            return;
        }
        
        traverse(root.left, result);
        traverse(root.right, result);

        result.add(root.val);
    }
}
```



##### 2.分治

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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        // divide & conquer
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        
        ArrayList<Integer> left = postorderTraversal(root.left);
        ArrayList<Integer> right = postorderTraversal(root.right);
        
        result.addAll(left);
        result.addAll(right);
        result.add(root.val);
        
        return result;
    }
}
```



#### 二、迭代

##### 方法一：双栈实现

使用两个辅助栈，一个栈用来添加结点及其左右儿子，另一个栈用来翻转第一个栈的输出。该方法的好处是易于理解，缺点是两个栈使用辅助空间较多。

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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        // non-recursion
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        Stack<TreeNode> temp  = new Stack<TreeNode>();
        
        stack.push(root);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            if (node.left != null) {
                stack.push(node.left);
            }
            if (node.right != null) {
                stack.push(node.right);
            }
            temp.push(node);
        }
        
        while (!temp.empty()) {
            result.add(temp.pop().val);
        }
        
        return result;
    }
    
}
```

##### 方法二、单栈实现

后续遍历需要确保根结点出栈是在左儿子和右儿子出栈之后。因此实现步骤如下：

- 对任一非空结点，按照“右儿子-->左儿子”顺序入栈，这样确保左儿子先出栈。
- 对于以下两种情况，出栈并输出至结果。
  - 叶子结点：左儿子、右儿子都为空。
  - 子结点已经出栈的结点，通过 `prev` 指针判断。

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
     * @return: Postorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode curr = root;
        TreeNode prev = null;
        stack.push(root);
        
        while (!stack.empty()) {
            curr = stack.peek();
            // if current node has no child or its two child node have been visited
            if ((curr.left == null && curr.right == null) || 
                (prev != null && (prev == curr.left || prev == curr.right))) {
                result.add(curr.val);
                stack.pop();
                prev = curr;
            } else {
                if (curr.right != null) {
                    stack.push(curr.right);
                }
                if (curr.left != null) {
                    stack.push(curr.left);
                }
            }
        }
        return result;
    }
}
```



### 参考

1. [二叉树的非递归遍历 | 海子](http://www.cnblogs.com/dolphin0520/archive/2011/08/25/2153720.html)
2. [Leetcode – Binary Tree Postorder Traversal (Java) | ProgramCreek](http://www.programcreek.com/2012/12/leetcode-solution-of-iterative-binary-tree-postorder-traversal-in-java/) 