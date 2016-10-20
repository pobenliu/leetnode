#  Add Two Numbers

 Add Two Numbers  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/add-two-numbers/) )

```
Description
You have two numbers represented by a linked list, where each node contains a single digit. 
The digits are stored in reverse order, such that the 1's digit is at the head of the list. 
Write a function that adds the two numbers and returns the sum as a linked list.

Example
Given 7->1->6 + 5->9->2. That is, 617 + 295.
Return 2->1->9. That is 912.
Given 3->1->5 and 5->9->2, return 8->0->8.
```

### 解题思路

依次将两个链表对应结点值相加，并新建结点即可。有两个需要注意的地方，一是两个链表长度可能不一样，二是加法要考虑进位。

##### 易错点

> 1. 进位的实现，以及求和、求模的差异。
> 2. 当一个链表 `l1` 比较长时，多出的部分要把结点复制一遍加到新建链表中，不能直接将结点指向 `l1` 。

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
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
         if (l1 == null && l2 == null) {
             return null;
         }
         
         ListNode dummy = new ListNode(0);
         ListNode head = dummy;
         
         int sum = 0, carry = 0;
         while (l1 != null && l2 != null) {
             sum = l1.val + l2.val + carry;
             head.next = new ListNode(sum % 10);
             carry = sum / 10;
             
             head = head.next;
             l1 = l1.next;
             l2 = l2.next;
         }
         
         while (l1 != null) {
             sum = l1.val + carry;
             head.next = new ListNode(sum % 10);
             carry = sum / 10;
             head = head.next;
             l1 = l1.next;
         }
         
         while (l2 != null) {
             sum = l2.val + carry;
             head.next = new ListNode(sum % 10);
             carry = sum / 10;
             head = head.next;
             l2 = l2.next;
         }
         
         if (carry != 0) {
             head.next = new ListNode(carry);
         }
         
         return dummy.next;
    }
}
```

