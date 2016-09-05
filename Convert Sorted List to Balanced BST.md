#  Convert Sorted List to Balanced BST

 Convert Sorted List to Balanced BST ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/convert-sorted-list-to-balanced-bst/) )

```
Description
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

Example
               2
1->2->3  =>   / \
             1   3
```



### 解题思路

#### 一、分治法

自顶向下构建 BST 

- 获取当前链表中间结点，作为 BST 的根结点
  - 获取链表左半部分的中间结点，作为左子树的根结点
  - 获取链表右半部分的中间结点，作为右子树的根结点

**复杂度分析**

- 时间复杂度：由于采用了分治法，所以复杂度分析参考归并排序，约为 `O(nlogn)`



注：

1. 考虑到单向链表的性质，需要获取中间结点的前一个结点。如果有偶数个结点如 6 个，那么找到第 3 个结点，如果是奇数个结点如 7 个，那么找到第 3 个结点。这样将中间结点作为当前子树的根结点，将中间结点的下一个结点作为右半部分的头结点。
2. 自己又把 `findMiddle` 函数中的 `while` 写成了 `if `，标记一下，得长记性！

java 实现

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
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
     * @param head: The first node of linked list.
     * @return: a tree node
     */
    public TreeNode sortedListToBST(ListNode head) {  
        // write your code here
        if(head == null) {
            return null;
        }
        if(head.next == null) {
            return new TreeNode(head.val);
        }
        
        ListNode mid = findMiddle(head);
        ListNode leftHead = head;
        ListNode rightHead = mid.next.next;
        
        // 根结点 
        TreeNode root = new TreeNode(mid.next.val);
        mid.next = null;
        // 左子树 
        root.left = sortedListToBST(leftHead);
        // 右子树
        root.right = sortedListToBST(rightHead);
        
        return root;
    }
    // 取中间结点的前一个结点
    private ListNode findMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast != null && fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}

```



#### 二、非分治法

考虑到 BST 的中序遍历是一个非递减序列，因此可以使用自底向上的方法构建 BST ，先构建左子树，使用递归的方法从最底层的左子树开始构建，同时不断移动链表的头指针，使得链表的头指针永远是对应当前子树位置的。

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
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
     * @param head: The first node of linked list.
     * @return: a tree node
     */
    private ListNode current;
    
    public TreeNode sortedListToBST(ListNode head) {  
        // write your code here
        int size;
        
        current = head;
        size = getListLength(head);
        
        return sortedListToBSTHelper(size);
    }
    
    private int getListLength(ListNode head) {
        int size = 0;
        
        while(head != null) {
            head = head.next;
            size++;
        }
        return size;
    }
    
    public TreeNode sortedListToBSTHelper(int size) {
        if(size <= 0) {
            return null;
        }
        
        TreeNode left = sortedListToBSTHelper(size / 2);
        TreeNode root = new TreeNode(current.val);
        current = current.next;
        TreeNode right = sortedListToBSTHelper(size - 1 - size / 2);
        
        root.left = left;
        root.right = right;
        
        return root;
    }
    
}

```





### 参考

1. [Divide&Conquer Java solution Complexity? | leetcode discussion](https://discuss.leetcode.com/topic/9560/divide-conquer-java-solution-complexity)
2. [Convert Sorted List to Balanced BST | 九章算法](http://www.jiuzhang.com/solutions/convert-sorted-list-to-binary-search-tree/)

