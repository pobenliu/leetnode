#  Delete Node in the Middle of Singly Linked List

 Delete Node in the Middle of Singly Linked List  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/delete-node-in-the-middle-of-singly-linked-list/) )

```
Description
Implement an algorithm to delete a node in the middle of a singly linked list, given only access to that node.

Example
Given 1->2->3->4, and node 3. return 1->2->4
```

### 解题思路

本题目的意思是，如果要删除一个链表中的结点，并且你只能从这个结点开始访问链表，无法访问它的前一个结点，应该怎么做。在该结点后续结点存在的情况下，将后续结点值复制过来即可。

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
     * @param node: the node in the list should be deleted
     * @return: nothing
     */
    public void deleteNode(ListNode node) {
        if (node == null || node.next == null) {
            return;
        }
        
        ListNode next = node.next;
        node.val = next.val;
        node.next = node.next.next;
        return;
    }
}
```

