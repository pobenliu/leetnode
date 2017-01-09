#  Binary Tree Path Sum

 Binary Tree Path Sum  ( [lintcode](http://www.lintcode.com/en/problem/binary-tree-path-sum/) )

```
Description
Given a binary tree, find all paths that sum of the nodes in the path equals to a given number target.
A valid path is from root node to any of the leaf nodes.

Example
Given a binary tree, and target = 5:
     1
    / \
   2   4
  / \
 2   3

return
[
  [1, 2, 2],
  [1, 4]
]
```

### 解题思路

跟 Permutation 题目类似，需要注意的是每次进入缩小规模都需要考虑两种情况——左子树和右子树，所以传递路径参数时需要新建对象，以免发生混乱。

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
     * @param root the root of binary tree
     * @param target an integer
     * @return all valid paths
     */
    public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {
        // Write your code here
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (root == null) {
            return result;
        }
        List<Integer> list = new ArrayList<Integer>();
        helper(result, list, root, target);
        return result;
    }
    
    private void helper(List<List<Integer>> result, List<Integer> list,
                        TreeNode root, int target) {
        if (root == null ) {
            return;
        }
        
        list.add(root.val);
        if (target == root.val && root.left == null && root.right == null) {
            result.add(new ArrayList<Integer>(list));
            return;
        }
        List<Integer> listLeft = new ArrayList<Integer>(list);
        List<Integer> listRight = new ArrayList<Integer>(list);
        helper(result, listLeft, root.left, target - root.val);
        helper(result, listRight, root.right, target - root.val);
        list.remove(list.size() - 1);
    }
}
```

