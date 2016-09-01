#  Reverse Linked List II

 Reverse Linked List II ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/reverse-linked-list-ii/) )

```
Description
Reverse a linked list from position m to n.

Notice
Given m, n satisfy the following condition: 1 ≤ m ≤ n ≤ length of list.

Example
Given 1->2->3->4->5->NULL, m = 2 and n = 4, return 1->4->3->2->5->NULL.

Challenge 
Reverse it in-place and in one-pass
```



### 解题思路

对第 m 到 n 个结点进行反转，会影响到第 m - 1 个和第 n + 1个结点。

- 首先找到第 m - 1 个结点
- 反转第 m 到 n 个结点
- 处理第 m - 1 个和第 n + 1个结点

建议在做题之前画图表示反转之前和之后的结点关系，减少出错。

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
     * @oaram m and n
     * @return: The head of the reversed ListNode
     */
    public ListNode reverseBetween(ListNode head, int m , int n) {
        // write your code
        if (head == null || m >= n) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        // 寻找第 m - 1 个结点
        for (int i = 1; i < m; i++) {
            head = head.next;
        }
        ListNode premNode = head;
        ListNode mNode = head.next;
        ListNode nNode = mNode, postnNode = mNode.next;
        // 对 n - m 个指针/引用进行反转
        for (int i = m; i < n ; i++) {
            ListNode temp = postnNode.next;
            postnNode.next = nNode;
            nNode = postnNode;
            postnNode = temp;
        }
        // 处理第 m - 1 个和第 n + 1个结点
        mNode.next = postnNode;
        premNode.next = nNode;
        
        return dummy.next;
    }
}
```













