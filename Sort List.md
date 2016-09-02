# Sort List

Sort List ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/sort-list/) )

```
Description
Sort a linked list in O(n log n) time using constant space complexity.

Example
Given 1->3->2->null, sort it to 1->2->3->null.

Challenge 
Solve it by merge sort & quick sort separately.
```



### 解题思路

#### 一、归并排序

在 `findMiddle` 函数中，让 `fast` 先走一步，是为了取得中间节点的前一个，和 `mid = start + ( end - start) / 2` 的效果相同。这样做的目的主要是解决 `1->2->null` 两个结点的情况，如果不这样做，`slow` 就会返回2，就没有办法切割了。

**复杂度分析**

- 时间复杂度：归并排序的时间复杂度是 `O(nlogn)` 
- 空间复杂度：由于使用了栈空间，所以空间复杂度是 `O(logn)` 

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
     * @return: You should return the head of the sorted linked list,
                    using constant space complexity.
     */
    public ListNode sortList(ListNode head) {  
        // 至少要有2个结点
        if (head == null || head.next == null) {
            return head;
        }
        // 取得中间结点
        ListNode mid = findMiddle(head);
        // 分割为左右2个链表
        ListNode right = mid.next;
        mid.next = null;
        // 分别对左右2个链表进行排序
        ListNode left = sortList(head);
        right = sortList(right);
        // 合并2个链表
        return merge(left, right);
    }
    
    private ListNode findMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
    
    private ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while(head1 != null && head2 != null) {
            if(head1.val < head2.val) {
                tail.next = head1;
                head1 = head1.next;
            } else {
                tail.next = head2;
                head2 = head2.next;
            }
            tail = tail.next;
        }
        if(head1 != null) {
            tail.next = head1;
        } else {
            tail.next = head2;
        }
        
        return dummy.next;
    }
}

```



#### 二、快速排序



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

class Pair {
    public ListNode first, second;
    public Pair (ListNode first, ListNode second) {
        this.first = first;
        this.second = second;
    }
}

public class Solution {
    /**
     * @param head: The head of linked list.
     * @return: You should return the head of the sorted linked list,
                    using constant space complexity.
     */
    public ListNode sortList(ListNode head) {  
        // 至少要有2个结点
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode mid = findMedian(head);  // O(n)
        Pair pair = partition(head, mid.val);  // O(n)
        
        ListNode left = sortList(pair.first);
        ListNode right = sortList(pair.second);
        
        getTail(left).next = right;  // O(n)
        
        return left;
    }
    
    // 1->2->3 return 2
    // 1->2 return 1
    private ListNode findMedian(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
    
    private Pair partition(ListNode head, int value) {
        ListNode leftDummy = new ListNode(0), leftTail = leftDummy;
        ListNode rightDummy = new ListNode(0), rightTail = rightDummy;
        ListNode equalDummy = new ListNode(0), equalTail = equalDummy;
        
        while(head != null) {
            if(head.val < value) {
                leftTail.next = head;
                leftTail = head;  // 相当于 leftTail = leftTail.next;
            } else if(head.val > value) {
                rightTail.next = head;
                rightTail = head;
            } else {
                equalTail.next = head;
                equalTail = head;
            }
            head = head.next;
        }
        
        leftTail.next = null;
        rightTail.next = null;
        equalTail.next = null;
        
        if(leftDummy.next == null && rightDummy.next == null) {
            ListNode mid = findMedian(equalDummy.next);
            leftDummy.next = equalDummy.next;
            rightDummy.next = mid.next;
            mid.next = null;
        } else if(leftDummy.next == null) {
            leftTail.next = equalDummy.next;
        } else {
            rightTail.next = equalDummy.next;  // 包含 left right 都不为 null 的情况
        }
        
        return new Pair(leftDummy.next, rightDummy.next);
    }
    
    private ListNode getTail(ListNode head) {
        if(head == null) {
            return null;
        }
        
        while(head.next != null) {
            head = head.next;
        }
        return head;
    }
}

```





自己第一次尝试着写的，但是运行到 `if (head == null || head.next == null) {` 和 `ListNode left = sortList(leftHead);` 时会出现报错 `Exception in thread "main" java.lang.StackOverflowError at ` ，暂时还没搞清楚是哪里的错误，先记录下来。

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
     * @return: You should return the head of the sorted linked list,
                    using constant space complexity.
     */
    public ListNode sortList(ListNode head) {  
        // 至少要有2个结点
        if (head == null || head.next == null) {
            return head;
        }
        ListNode part = partition(head);
        
        ListNode leftHead = head;
        ListNode rightHead = part.next;
        part.next = null;
        
        ListNode left = sortList(leftHead);
        ListNode right = sortList(rightHead);
        
        while (left != null) {
            left = left.next;
        }
        left.next = right;
        return head;
    }
    
    private ListNode partition(ListNode head) {
        int val = head.val;
        
        ListNode leftDummy = new ListNode(0);
        ListNode left = leftDummy;
        ListNode rightDummy = new ListNode(0);
        ListNode right = rightDummy;
        
        while (head != null) {
            if (head.val < val) {
                left.next = head;
                left = left.next;
            } else {
                right.next = head;
                right = right.next;
            }
            head = head.next;
        }
        
        left.next = rightDummy.next;
        right.next = null;
        head = leftDummy.next;
        return left;
    }
    
}

```





### 参考

1. [Sort List | 九章算法](http://www.jiuzhang.com/solutions/sort-list/)
2. [LeetCode: Sort List 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4131885.html)



