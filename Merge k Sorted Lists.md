# Merge k Sorted Lists

Merge k Sorted Lists ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/merge-k-sorted-lists/) )

```
Description
Merge k sorted linked lists and return it as one sorted list.
Analyze and describe its complexity.

Example
Given lists:
[
  2->4->null,
  null,
  -1->null
],
return -1->2->4->null.
```



### 解题思路

#### 一、分治法

参考数组归并排序的思路，先分再合，先局部有序，再整体有序。

**算法复杂度：**

- 时间复杂度：每个链表参与合并的次数为 `logn`， `n` 个链表合并共需要遍历 `(L1+L2+…+Ln)` 次，所以时间复杂度为 `O(logn*(L1+L2+…+Ln))`
- 空间复杂度：`O(1)`

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
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
        if(list == null || lists.size() == 0) {
            return null;
        }
        
        return merge(lists, 0, lists.size() - 1);
    }
    
    private ListNode merge(List<ListNode> lists, int start, int end) {
        if(start == end) {
            return lists.get(start);
        }
        
        int mid = start + (end - start) / 2;
        ListNode left = merge(lists, start, mid);
        ListNode right = merge(lists, mid + 1, end);
        return mergeTwoLists(left, right);
    }
    
    private ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while(list1 != null && list2 != null) {
            if(list1.val < list2.val) {
                tail.next = list1;
                list1 = list1.next;
            } else {
                tail.next = list2;
                list2 = list2.next;
            }
            tail = tail.next;
        }
        
        if (list1 != null) {
            tail.next = list1;
        } else {
            tail.next = list2;
        }
        
        return dummy.next;
    } 
}

```



### 二、优先队列

使用 Java 优先队列（priority queue）库函数 API 。

- 新建优先队列，将 n 个待排序链表的头结点分别装入
  - 取出队列首结点（最小值），加入链表
  - 如果取出结点有后续结点，将其装入队列
- 重复以上两个过程直至队列为空

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
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
        if(lists == null || lists.size() == 0) {
            return null;
        }
        
        Queue<ListNode> heap = new PriorityQueue<ListNode>(lists.size(), ListNodeComparator);
        // 共n个链表，heap的大小为n，把n个链表的头结点装入
        for(int i = 0; i < lists.size(); i++) {
            if(lists.get(i) != null) {
                heap.add(lists.get(i));
            }
        }
         
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while(!heap.isEmpty()) {
            ListNode head = heap.poll();
            tail.next = head;
            tail = tail.next;
            // 如果从队列取出结点有后续结点，将其加入队列
            if(head.next != null) {     
                heap.add(head.next);
            }
        }
        return dummy.next;
    }
    
    private Comparator<ListNode> ListNodeComparator = new Comparator<ListNode>() {
        public int compare(ListNode left, ListNode right) {
            if(left == null) {
                return 1;
            } else if(right == null) {
                return -1;
            }
            return left.val - right.val;
        }
    };
    
}

```





### 参考

1. [Merge k Sorted Lists | 九章算法](http://www.jiuzhang.com/solutions/merge-k-sorted-lists/)