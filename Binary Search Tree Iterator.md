# Binary Search Tree Iterator

Binary Search Tree Iterator ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/binary-search-tree-iterator/) )

```
Description
Design an iterator over a binary search tree with the following rules:
- Elements are visited in ascending order (i.e. an in-order traversal)
- next() and hasNext() queries run in O(1) time in average.

Example
For the following binary search tree, in-order traversal by using iterator is [1, 6, 10, 11, 12]
   10
 /    \
1      11
 \       \
  6       12

Challenge 
Extra memory usage O(h), h is the height of the tree.
Super Star: Extra memory usage O(1)
```



### 解题思路

其实相当于使用栈来实现树的中序遍历

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
 * Example of iterate a tree:
 * BSTIterator iterator = new BSTIterator(root);
 * while (iterator.hasNext()) {
 *    TreeNode node = iterator.next();
 *    do something for node
 * } 
 */
 
public class BSTIterator {
    private Stack<TreeNode> stack = new Stack<>();
    private TreeNode curt;
    
    //@param root: The root of binary tree.
    public BSTIterator(TreeNode root) {
        // write your code here
        curt = root;
    }

    //@return: True if there has next node, or false
    public boolean hasNext() {
        // write your code here
        return (curt != null || !stack.isEmpty());
    }
    
    //@return: return next node
    public TreeNode next() {
        // write your code here
        while (curt != null) {
            stack.push(curt);
            curt = curt.left;
        }
        
        curt = stack.pop();
        TreeNode node = curt;
        curt = curt.right;
        
        return node;
    }
}
```





### 参考

1. [Binary Search Tree Iterator | 九章算法](http://www.jiuzhang.com/solutions/binary-search-tree-iterator/)









