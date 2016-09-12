# Max Tree

Max Tree  ( [leetcode]()  [lintcode]() )

```
Given an integer array with no duplicates. A max tree building on this array is defined as follow:
- The root is the maximum number in the array The left subtree and right subtree are the max trees of the subarray divided by the root number. 
- Construct the max tree by the given array.

Example 
Given [2, 5, 6, 0, 3, 1], the max tree constructed by this array is:
    6
   / \
  5   3
 /   / \
2   0   1

Challenge 
O(n) time and memory.
```



### 解题思路

如果是自顶向下，按照 Max Tree 的定义构造，那么时间复杂度至少是 `O(nlogn)` 。查找最大值的时间复杂度是 `O(n)` ，如果最大值刚好可以将数组分为两部分，那么复杂度递归关系如下 `T(n) = 2 * T(n / 2) + O(n)` 。最坏的情况是数组是降序/升序，时间复杂度为 `O(n^2)` 。 

考虑自底向上的方法。对一个数，考察其父亲结点是谁，它是左儿子还是右儿子。对于数 `i `，寻找左边第一个比它大的数 `x` ，和右边第一个比它大的数 `y` ，如果 `x > y` 是 `y` 的左儿子，否则是 `x` 的右儿子。

具体实现使用一个降序栈

- 将数组按从左到右顺序迭代，当处理一个新的结点 `cur` 时，所有在栈中的结点全部都在其左边，因此需要判断 `cur` 和栈中结点的关系（ 是 `cur` 的左儿子或者左父亲）。
- 当栈顶结点值大于当前结点值时，将当前结点设为栈顶结点的右儿子，进栈；当栈顶结点值小于当前结点值时，出栈，将其设置为当前结点的左儿子。
- 重复以上步骤，并返回栈底元素，即为最大数（根结点）。

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
     * @param A: Given an integer array with no duplicates.
     * @return: The root of max tree.
     */
    public TreeNode maxTree(int[] A) {
        // write your code here
        if (A == null || A.length == 0) {
            return null;
        }
        LinkedList<TreeNode> stack = new LinkedList<TreeNode>();
        for (int i = 0; i < A.length; i++) {
            TreeNode cur = new TreeNode(A[i]);
            while (!stack.isEmpty() && cur.val > stack.peek().val) {
                cur.left = stack.pop();
            }
            if (!stack.isEmpty()) {
                stack.peek().right = cur;
            }
            stack.push(cur);
        }
        TreeNode result = new TreeNode(0);
        while (!stack.isEmpty()) {
            result = stack.pop();
        }
        return result;
    }
}
```



### 参考

1. [Max Tree – Lintcode Java | WELKIN LAN ](http://blog.welkinlan.com/2015/06/29/max-tree-lintcode-java/) 

