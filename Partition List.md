# Partition List

Partition List ( [leetcode]() [lintcode]() )

```
Description
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
You should preserve the original relative order of the nodes in each of the two partitions.

Example
Given 1->4->3->2->5->2->null and x = 3,
return 1->2->2->4->3->5->null.
```



### 解题思路

- 新建两个临时链表，表头使用 dummy node （初始化为 `new ListNode(0)`）
  - 小于 x 的结点，放在 left 链表
  - 大于 x 的结点，放在 right 链表
- 合并两个临时链表，将 right 链表挂在 left 链表尾部，并**将 right 链表尾结点指向 null**



Java 实现

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
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @param x: an integer
     * @return: a ListNode 
     */
    public ListNode partition(ListNode head, int x) {
        // write your code here
        if (head == null) {
            return null;
        }
        // 注意dummy node的初始化
        // ListNode left = leftDummy = new ListNode(0); 这种初始化方式是错误的
        ListNode leftDummy = new ListNode(0);
        ListNode left = leftDummy;
        ListNode rightDummy = new ListNode(0);
        ListNode right = rightDummy;
        
        while (head != null) {
            if (head.val < x) {
                left.next = head;
                left = left.next;
            } else {
                right.next = head;
                right = right.next;
            }
            head = head.next;
        }
        
        left.next = rightDummy.next;
        right.next = null;
        return leftDummy.next;
    }
}

```

注：

`ListNode left = leftDummy = new ListNode(0);` 这种方式是不对的，原因暂时不清楚，应该是和Java语言特性有关。



### 参考

1. [Sort List | 九章算法](http://www.jiuzhang.com/solutions/partition-list/)
2. [Partition List -- leetcod | 帮客之家](http://www.bkjia.com/cjjc/986722.html)