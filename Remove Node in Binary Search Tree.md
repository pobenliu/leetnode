#  Remove Node in Binary Search Tree

 Remove Node in Binary Search Tree ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/remove-node-in-binary-search-tree/) )

```
Description
Given a root of Binary Search Tree with unique value for each node. Remove the node with given value. If there is no such a node with given value in the binary search tree, do nothing. You should keep the tree still a binary search tree after removal.

Example
Given binary search tree:
    5
   / \
  3   6
 / \
2   4

Remove 3, you can either return:
    5
   / \
  2   6
   \
    4
or
    5
   / \
  4   6
 /
2
```



### 解题思路

删除一个节点涉及的操作比较复杂，所以需要分析都有哪些情况，进行归类，在不同的情况使用不同的方法。

大的步骤分为2步：

- 找到需要删除的结点
- 如果结点存在，删除该结点

关于查找算法，第一反应就是递归算法，不过涉及删除操作，所以需要记录当前结点的父结点。

关于要删除的结点，有以下几种情况：

1. 叶子结点：

   没有左儿子和右儿子，特殊情况是只有一个根结点。

   此时比较简单，直接删除即可。

   ![](http://ww2.sinaimg.cn/mw690/600e6311jw1f7d5m5aeozj20fl05raa9.jpg)

2. 只有一个儿子结点：

   只有左结点或右结点

   此时也比较简单，将该结点的父结点的儿子指向其儿子结点即可。

   ![](http://ww2.sinaimg.cn/mw690/600e6311jw1f7d5m5njpnj208z07it8u.jpg)

   ![](http://ww1.sinaimg.cn/mw690/600e6311jw1f7d5m5xlhmj208z07i0sy.jpg)

   ![](http://ww4.sinaimg.cn/mw690/600e6311jw1f7d5m68yp1j208g05qq2z.jpg)

3. 有左儿子和右儿子：

   此时处理稍微复杂一些，要求在删除结点之后仍为二叉查找树，那就意味着树的中序遍历保持升序。为了尽可能减少对树的结点的操作，最简单的方法是找一个结点代替要删除结点，也就是中序遍历中要删除结点的下一个结点——其右子树的最小值结点。根据二叉查找树的性质，不难想到要删除结点右子树的最小值结点一定是叶子结点，这样处理起来就容易了。

   ![](http://ww4.sinaimg.cn/mw690/600e6311jw1f7d5m6nvkxj20fa08caa9.jpg)

   ![](http://ww3.sinaimg.cn/mw690/600e6311jw1f7d5m70a15j20fa08caaa.jpg)

   ![](http://ww4.sinaimg.cn/mw690/600e6311jw1f7d5m7fpbbj20fa08cdg2.jpg)

   ![](http://ww3.sinaimg.cn/mw690/600e6311jw1f7d5m7p1paj20fa08caa7.jpg)



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
     * @param root: The root of the binary search tree.
     * @param value: Remove the node with given value.
     * @return: The root of the binary search tree after removal.
     */
    public TreeNode removeNode(TreeNode root, int value) {
        // write your code here
        if (root == null) {
            return root;
        } else {
            // 要删除的是根结点，需要使用哑结点辅助
            if (root.val == value) {
                TreeNode dummy = new TreeNode(0);
                dummy.left = root;
                remove(root, value, dummy);
                root = dummy.left;
                return root;
            } else {
                remove(root, value, null);
                return root;
            }
        }
    }
    
    public void remove (TreeNode root, int value, TreeNode parent) {
        
        if (value < root.val) {         // value小于当前结点值，向左子树寻找
            if (root.left != null) {
                remove(root.left, value, root);
            } else {
                return;     // 树中不包含value值，直接返回
            }
        } else if (value > root.val) {  // value大于当前结点值，向右子树寻找
            if (root.right != null) {
                remove(root.right, value, root);
            } else {
                return;
            }
        } else {
            // 要删除结点左右子树都存在，将结点值置为右子树最小值，并删除右子树的最小值结点
            if (root.left != null && root.right != null) {
                root.val = minValue(root.right);
                remove(root.right, root.val, root);
            } else if (parent.left == root) {
                // 要删除结点只有一个子树，需要对父结点进行操作
                parent.left = (root.left != null) ? root.left : root.right;
            } else if (parent.right == root) {
                parent.right = (root.left != null) ? root.left : root.right;
            }
            return;
        }
    }
    
    public int minValue(TreeNode root){
        if (root.left == null) {
            return root.val;
        } else {
            return minValue(root.left);
        }
    }
}
```





### 参考

1. [Binary search tree. Removing a node](http://www.algolist.net/Data_structures/Binary_search_tree/Removal)