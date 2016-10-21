#  Intersection of Two Linked Lists

 Intersection of Two Linked Lists  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/intersection-of-two-linked-lists/) )

```
Description
Write a program to find the node at which the intersection of two singly linked lists begins.

Notice
If the two linked lists have no intersection at all, return null.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.

Example
The following two linked lists:
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
begin to intersect at node c1.

Challenge 
Your code should preferably run in O(n) time and use only O(1) memory.
```

### 解题思路

根据题目中给出的例子，两个链表相交是指从某一结点开始到尾结点，两个链表都指向相同结点。

思路一：比较直观的解法是分别计算两个链表的长度，然后将较长的链表向前移动 L 个结点，L 是两个链表的长度差。然后依次比较两个链表的结点是否相等。

思路二：如果两个链表相交，那么把一个链表接在另一个链表后面，会形成一个环，这样我们就可以参考 Linked List Cycle II 的解法来找相交点。这里我们实现思路二。

Java 实现

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;      
 *     }
 * }
 */
public class Solution {
    /**
     * @param headA: the first list
     * @param headB: the second list
     * @return: a ListNode 
     */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        
        ListNode tail = getTail(headA);
        tail.next = headB;
        
        ListNode result = listCycle(headA);
        tail.next = null;  // disconnect headA and headB
        return result;
        
    }
    
    private ListNode getTail (ListNode head) {
        while (head.next != null) {
            head = head.next;
        }
        return head;
    }
    
    private ListNode listCycle(ListNode head) {
        ListNode slow = head, fast = head.next;
        while (fast != slow) {
            if (fast == null || fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        
        while (head != slow.next) {
            head = head.next;
            slow = slow.next;
        }
        return head;
    }
}
```

### 参考

1. [Intersection of Two Linked Lists | 九章算法](http://www.lintcode.com/en/problem/intersection-of-two-linked-lists/)