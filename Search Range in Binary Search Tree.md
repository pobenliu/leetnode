#  Search Range in Binary Search Tree

 Search Range in Binary Search Tree ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/search-range-in-binary-search-tree/) )

```
Description
Given two values k1 and k2 (where k1 < k2) and a root pointer to a Binary Search Tree. Find all the keys of tree in range k1 to k2. i.e. print all x such that k1<=x<=k2 and x is a key of given BST. Return all the keys in ascending order.

Example
If k1 = 10 and k2 = 22, then your function should return [12, 20, 22].
    20
   /  \
  8   22
 / \
4   12
```



### 解题思路

#### 一、DFS

二叉搜索树的中序遍历是升序，根据题目要求，范围内的取值按升序排列，可以对 BST 进行中序 DFS ，并对遍历到的结点值进行判断，满足大小范围加入数组即可。



Java实现 v1 ：将 `ArrayList<Integer> results` 作为一个参数传递

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
     * @param k1 and k2: range k1 to k2.
     * @return: Return all keys that k1<=key<=k2 in ascending order.
     */
    public ArrayList<Integer> searchRange(TreeNode root, int k1, int k2) {
        // write your code here
        ArrayList<Integer> results = new ArrayList<Integer>();
        
        dfs(root, results, k1, k2);
        return results;
    }
    
    private void dfs(TreeNode root,
                     ArrayList<Integer> results,
                     int k1,
                     int k2) {
        if (root == null) {
            return;
        }
        
        dfs(root.left, results, k1, k2);
        if (root.val >= k1 && root.val <= k2) {
            results.add(root.val);
        }
        dfs(root.right, results, k1, k2);
    }
} 
```



Java实现 v2 ：将 `ArrayList<Integer> results` 定义为一个全局变量

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
     * @param k1 and k2: range k1 to k2.
     * @return: Return all keys that k1<=key<=k2 in ascending order.
     */
    private ArrayList<Integer> results = new ArrayList<Integer>(); 
    
    public ArrayList<Integer> searchRange(TreeNode root, int k1, int k2) {
        // write your code here
        
        dfs(root, k1, k2);
        return results;
    }
    
    private void dfs(TreeNode root, int k1, int k2) {
        if (root == null) {
            return;
        }
        
        dfs(root.left, k1, k2);
        if (root.val >= k1 && root.val <= k2) {
            results.add(root.val);
        }
        dfs(root.right, k1, k2);
    }
} 
```



Java实现 v3 ：在 v2 版本的实现基础上增加条件判断语句，只有在当前结点取值在要求范围内时，才继续进行遍历，减少不必要的遍历操作

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
     * @param k1 and k2: range k1 to k2.
     * @return: Return all keys that k1<=key<=k2 in ascending order.
     */
    private ArrayList<Integer> results; 
    
    public ArrayList<Integer> searchRange(TreeNode root, int k1, int k2) {
        // write your code here
        results = new ArrayList<Integer>();
        dfs(root, k1, k2);
        return results;
    }
    
    private void dfs(TreeNode root, int k1, int k2) {
        if (root == null) {
            return;
        }
        
        if (root.val > k1) {
            dfs(root.left, k1, k2);
        }
        
        if (root.val >= k1 && root.val <= k2) {
            results.add(root.val);
        }
        
        if (root.val < k2) {
            dfs(root.right, k1, k2);
        }
    }
} 
```





### 参考

1. [Search Range in Binary Search Tree | 九章算法](http://www.jiuzhang.com/solutions/search-range-in-binary-search-tree/)

