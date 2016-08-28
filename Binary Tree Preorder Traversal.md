# Binary Tree Preorder Traversal

Binary Tree Preorder Traversal ( leetcode [lintcode](http://www.lintcode.com/en/problem/binary-tree-preorder-traversal/) )

```
Description
Given a binary tree, return the preorder traversal of its nodes' values.

Example
Given:
    1
   / \
  2   3
 / \
4   5
return [1,2,4,5,3].

Challenge 
Can you do it without recursion?
```

### 解题思路

#### 一、递归法

二叉树本身就是递归定义，所以采用递归的方法实现树的遍历容易理解且代码简介。

实现过程：根据前序遍历访问的顺序，优先访问根节点，然后分别访问左儿子和右儿子。对任一节点，在循环中都可以看作是根节点，可直接访问，访问完之后，若其左儿子非空，按相同规则访问其左儿子，然后访问其右儿子。

根据递归时对返回结果处理方式的不同，可进一步分为遍历和分治两种方法：

面试时不推荐使用递归的方法实现。

**算法复杂度**

- 遍历树中的所有节点，时间复杂度 `O(n)`，未使用额外空间。

##### 1. 遍历法

实现步骤：

- 访问节点 node ，取值，将其视为根节点
- 若其左儿子非空，按相同规则访问其左儿子
- 若其右儿子非空，按相同规则访问其右儿子

其中，result 是作为参数传递的。

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
     * @return: Preorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        // 主循环中无需对边界条件进行检查，递归函数中已完成相关检查
        /*
        if (root == null) {
            return result;
        }
        */
        traverse(root, result);
        return result;
    }
    
    private void traverse(TreeNode root, ArrayList<Integer> result){
        if (root == null) {
            return;
        }
        result.add(root.val);
        traverse(root.left, result);
        traverse(root.right, result);
    }
}
```



##### 2. 分治法

步骤：

- 取左子树的遍历结果
- 取右子树的遍历结果
- 按照“根节点+左子树+右子树”的顺序对结果进行合并

其中，result 是作为结果返回的。

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
     * @return: Preorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        // null or leaf
        if (root == null) {
            return result;    
        }
        
        // divide
        ArrayList<Integer> left = preorderTraversal(root.left);
        ArrayList<Integer> right = preorderTraversal(root.right);
        
        // conquer
        result.add(root.val);
        result.addAll(left);	// 为何这里要使用addAll函数
        result.addAll(right);
        return result;
    }
}
```





#### 二、非递归法

基本原则：用递归可以解决的问题，改用非递归的方法解决，一般都需要使用栈，来模拟递归解法内存中使用的栈操作

实现步骤：

- 根节点非空，将根节点 root 入栈
- 栈非空（循环结束的条件）
  - 取栈顶节点 node 并进行出栈操作，保存当前节点值
  - 判断节点 node 的**右儿子**是否为空，若不空，将其入栈
  - 判断节点 node 的**左儿子**是否为空，若不空，将其入栈
- 遍历结束，返回结果

**算法复杂度**

- 时间复杂度：对每个节点遍历一遍，近似为 `O(n)`
- 空间复杂度：使用辅助栈，最坏情况下栈空间与节点数相等，近似为 `O(n)`

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
     * @return: Preorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        
        if (root == null) {
            return result;
        }
        
        stack.push(root);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            result.add(node.val);
            
            if (node.right != null) {
                stack.push(node.right);     //注意左子树、右子树的进栈顺序
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        return result;
    }
}
```



### 参考

1. [九章算法](http://www.jiuzhang.com/solutions/binary-tree-preorder-traversal/)
2. [Binary Tree Preorder Traversal](http://algorithm.yuanbin.me/zh-hans/binary_tree/binary_tree_preorder_traversal.html)
3. [二叉树的非递归遍历](http://www.cnblogs.com/dolphin0520/archive/2011/08/25/2153720.html)
