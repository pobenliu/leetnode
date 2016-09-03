# Linked List Cycle II

Linked List Cycle II ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/linked-list-cycle-ii/) )

```
Description
Given a linked list, return the node where the cycle begins.
If there is no cycle, return null.

Example
Given -21->10->4->5, tail connects to node index 1，return 10

Challenge 
Follow up:
Can you solve it without using extra space?
```



### 解题思路

#### 一、快慢指针

- 判断是否有循环
  - 如果快指针指向结点或下一个结点为空，说明到达链表末尾，没有循环
  - 如果快指针等于慢指针，说明存在循环
- 头指针和慢指针同时移动，当头指针等于慢指针的下一个结点时，为循环开始处（此处省去证明）



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
     * @return: The node where the cycle begins. 
     *           if there is no cycle, return null
     */
    public ListNode detectCycle(ListNode head) {  
        // write your code here
        if(head == null || head.next == null) {
            return null;
        }
        
        ListNode fast = head.next;
        ListNode slow = head;
        while(fast != slow) {
            if(fast == null || fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        
        while(head != slow.next) {
            head = head.next;
            slow = slow.next;
        }
        
        return head;
    }
}

```



### 参考

1. [Linked List Cycle II | 九章算法](http://www.jiuzhang.com/solutions/linked-list-cycle-ii/) 





