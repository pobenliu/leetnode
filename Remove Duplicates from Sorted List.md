# Remove Duplicates from Sorted List

 Remove Duplicates from Sorted List ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-list/) )

```
Description
Given a sorted linked list, delete all duplicates such that each element appear only once.

Example
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.
```



### 解题思路

- 比较当前结点值与下一个结点的值，当下一个结点为 `null` 时（尾结点）停止循环
  - 如果相等，将当前结点指向其下下一个结点。
  - 如不等，将当前结点指向下一个结点。

注意：以上两个判别条件构成一个全集，所以要使用 `if … else …` 缺少 `else` 会出现不必要的错误。

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
     * @return: ListNode head of linked list
     */
    public static ListNode deleteDuplicates(ListNode head) { 
        if (head == null) {
            return null;
        }
        ListNode node = head;
        while (node.next != null ) {
            if (node.val == node.next.val) {
                node.next = node.next.next;
            } else {
                node = node.next;    
            }
        }
        return head;
    }  
}
```











