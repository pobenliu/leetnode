#  Remove Linked List Elements

 Remove Linked List Elements  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/remove-linked-list-elements/) )

```
Description
Remove all elements from a linked list of integers that have value val.

Example
Given 1->2->3->3->4->5->3, val = 3, you should return the list as 1->2->4->5.
```

### 解题思路

考虑到链表头结点可能被删除，所以需要使用哨兵结点 `dummy node`。

Java 实现

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    /**
     * @param head a ListNode
     * @param val an integer
     * @return a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return null;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        
        while (head.next != null) {
            if (head.next.val == val) {
                head.next = head.next.next;
            } else {
                head = head.next;
            }
        }
        
        return dummy.next;
    }
}
```

