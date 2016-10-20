# Reverse Linked List II

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

对第 `m` 到 `n` 个结点进行反转，会影响到第 `m - 1` 个和第 `n + 1`个结点。

- 首先找到第 `m - 1` 个结点。
- 反转第 `m` 到 `n` 个结点。
- 处理第 `m - 1` 个和第 `n + 1`个结点。

建议在做题之前画图表示反转之前和之后的结点关系，减少出错，参考博客喜刷刷的[习题解析](http://bangbingsyb.blogspot.jp/2014/11/leetcode-reverse-linked-list-ii.html)，举例如下：

```
1.找到第 m-1 个结点。
D-->1-->2-->3-->4-->5-->null
    ^
    |
   head

2.以第 m 个结点为头结点，将长度为 L=n-m+1 部分反转。
D-->1<--2<--3<--4   5-->null
    ^           ^   ^
    |           |   |
   head       prev curt

3.处理未第 m-1 和第 n+1 个结点。
        |-----------|
        |           v
D-->1   2<--3<--4   5-->null
    |           ^
    |-----------|
```



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
        if (m >= n || head == null) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        
        // move head to (m-1) node
        for (int i = 1; i < m; i++) {
            head = head.next;
        }
        
        // reverse list from prev with length n-m+1
        ListNode prev = head.next;
        ListNode curt = prev.next;
        for (int i = 1; i <= n - m; i++) {
            ListNode tmp = curt.next;
            curt.next = prev;
            prev = curt;
            curt = tmp;
        }
        
        head.next.next = curt;
        head.next = prev;
        return dummy.next;
    }
}
```

### 参考

1. [[LeetCode] Reverse Linked List II | 喜刷刷](http://bangbingsyb.blogspot.jp/2014/11/leetcode-reverse-linked-list-ii.html)