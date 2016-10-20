# Reverse Linked List

 Reverse Linked List ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/reverse-linked-list/#) )  

```
Description
Reverse a linked list.

Example
For linked list 1->2->3, the reversed linked list is 3->2->1

Challenge 
Reverse it in-place and in one-pass
```



### 解题思路

这里要注意一点，就是尾结点的 `next` 是 `null`，利用这一点可以简化操作。

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
     * @return: The new head of reversed linked list.
     */
    public ListNode reverse(ListNode head) {
	    if (head == null || head.next == null) {
            return head;
        }
      
        ListNode prev = null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = prev;
            prev = head;
            head = temp;
        }
        
        return prev;
    }
}
```



