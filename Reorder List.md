# Reorder List

Reorder List ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/reorder-list/) )

```
Description
Given a singly linked list L: L0 → L1 → … → Ln-1 → Ln
reorder it to: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

Example
Given 1->2->3->4->null, reorder it to 1->4->2->3->null.

Challenge 
Can you do this in-place without altering the nodes' values?
```



### 解题思路

- 将链表分为相等的两部分，如果结点为奇数个，左链表个数比右链表个数多1
- 反转右链表
- 合并两个链表



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
     * @param head: The head of linked list.
     * @return: void
     */
    public void reorderList(ListNode head) {  
        // write your code here
        if (head == null || head.next == null || head.next.next == null) {
            return;
        }
        
        ListNode mid = findMedian(head);
        
        ListNode left = head;
        ListNode right = mid.next;
        mid.next = null;
        
        right = reverse(right);
        head = merge(left, right);
    }
    
    private ListNode findMedian(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while(fast.next != null || fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    
    private ListNode reverse(ListNode head) {
        ListNode dummy = null;
        
        while(head.next != null) {
            ListNode temp = head.next;
            head.next = dummy;
            dummy = head;
            head = temp;
        }
        head.next = dummy;
        return head;
    }
    
    private ListNode merge(ListNode left, ListNode right) {
        ListNode dummy = new ListNode(0);
        ListNode head = dummy;
        int i = 0;
        while(left != null && right != null) {
            if(i % 2 == 0) {
                head.next = left;
                left = left.next;
            } else {
                head.next = right;
                right = right.next;
            }
            i++;
            head = head.next;
        }
        
        while(left != null) {
            head.next = left;
            left = left.next;
            head = head.next;
        }
        while(right != null) {
            head.next = right;
            right = right.next;
            head = head.next;
        }
        
        return dummy.next;
    }
}
```



