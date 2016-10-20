# Remove Duplicates from Sorted List II

 Remove Duplicates from Sorted List II ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-list-ii/#) )

```
Description
Given a sorted linked list, delete all nodes that have duplicate numbers, 
leaving only distinct numbers from the original list.

Example
Given 1->2->3->3->4->4->5, return 1->2->5.
Given 1->1->1->2->3, return 2->3.
```



### 解题思路

题目要求删除所有重复的结点，那么头结点有可能被删除，所以需要使用哨兵结点 `dummy node`。

首先判断相邻结点是否相等，如相等，保存结点值，将所有等于该值的结点全部删除。

##### 易错点

> 1. 凡是涉及链表的遍历、删除，都需要确认要访问的结点是否为空，控制流语句（`while, if`）的判断条件中尤其要注意。本题两重循环的终止条件都需要注意。

Java 实现

```java
/**
 * Definition for ListNode
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
     * @param ListNode head is the head of the linked list
     * @return: ListNode head of the linked list
     */
    public static ListNode deleteDuplicates(ListNode head) {
        // write your code here
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        
        while (head.next != null && head.next.next != null) {
            if (head.next.val == head.next.next.val) {
                int val = head.next.val;
                while (head.next != null && head.next.val == val) {
                    head.next = head.next.next;
                }
            } else {
                head = head.next;
            }
        }
        
        return dummy.next;
    }
}
```

