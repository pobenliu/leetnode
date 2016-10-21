#  Merge Two Sorted Lists

 Merge Two Sorted Lists ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/merge-two-sorted-lists/) )

```
Description
Merge two sorted (ascending) linked lists and return it as a new sorted list. 
The new sorted list should be made by splicing together the nodes of the two lists and sorted in ascending order.

Example
Given 1->3->8->11->15->null, 2->null , return 1->2->3->8->11->15->null.
```

### 解题思路

跟归并排序中的合并类似，要考虑一个链表比另一个链表长的情况。

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
     * @param ListNode l1 is the head of the linked list
     * @param ListNode l2 is the head of the linked list
     * @return: ListNode head of linked list
     */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null && l2 == null) {
            return null;
        }
        ListNode dummy = new ListNode(0);
        ListNode head = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                head.next = l1;
                l1 = l1.next;
            } else {
                head.next = l2;
                l2 = l2.next;
            }
            head = head.next;
        }
        
        if (l1 != null) {
            head.next = l1;
        }
        if (l2 != null) {
            head.next = l2;
        }
        
        return dummy.next;
    }
}
```

