# Linked List Cycle

Linked List Cycle ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/linked-list-cycle/) )

```
Description
Given a linked list, determine if it has a cycle in it.

Example
Given -21->10->4->5, tail connects to node index 1, return true

Challenge 
Follow up:
Can you solve it without using extra space?
```



### 解题思路

#### 一、快慢指针

- 快指针每次走两步，慢指针每次走一步
- 如果两个指针相遇，说明有循环
- 如果快指针为空或者其下一个结点为空，说明走到链表末尾，说明无循环



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
     * @return: True if it has a cycle, or false
     */
    public boolean hasCycle(ListNode head) {  
        // write your code here
        if(head == null || head.next == null) {
            return false;
        }
        
        ListNode fast, slow;
        fast = head.next;
        slow = head;
        while(fast != slow) {
            // 之所以需要同时判断fast本身及fast.next，是因为fast每次移动两步
            if(fast == null || fast.next == null) {
                return false;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        return true;
    }
}

```





### 参考

1. [Linked List Cycle | 九章算法](http://www.jiuzhang.com/solutions/linked-list-cycle/)