# Copy List with Random Pointer

 Copy List with Random Pointer ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/copy-list-with-random-pointer/) )

```
Description
A linked list is given such that each node contains an additional random pointer 
which could point to any node in the list or null.
Return a deep copy of the list.

Example

Challenge 
Could you solve it with O(1) space?
```



### 解题思路

#### 一、哈希表

哈希表需要使用额外的 `O(n)` 的空间。

将原链表的结点作为键（`Key`），将复制链表的结点作为值（`Value`），依次向哈希表中存放，在此过程中完成链表的深度复制。

实现逻辑：

- 复制 `next` 指针关系。
  - 原链表的当前结点不在哈希表中。
    - 新建结点，拷贝结点值。
    - 将原始链表结点（`Key`）和新建结点（`Value`）存放在哈希表中。
  - 原链表当前结点在哈希表中（之前已经作为 `random` 指针指向结点被存储）。
    - 指向该结点（`Key`）对应的（`Value`）结点。
  - 将新建结点放入新建链表中。
- 复制 `random` 指针关系。
  - 原链表结点的 `random` 指针不为空。
    - `random` 所指结点在哈希表中。
      - 将对应复制结点的 `random` 指针指向哈希表中原始结点 `random` 指向结点的复制结点。
    - `random` 所指结点不在哈希表中。
      - 新建 `random` 指向结点，将原始结点、复制结点 `random` 指向结点对放入哈希表。
  - 准备处理下一个结点。

**算法复杂度**

- 时间复杂度：遍历原链表一次，故为 `O(n)` 。
- 空间复杂度：建立一个哈希表做结点映射，为 `O(n)` 。

Java 实现

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    /**
     * @param head: The head of linked list with a random pointer.
     * @return: A new head of a deep copy of the list.
     */
    public RandomListNode copyRandomList(RandomListNode head) {
        // write your code here
        if(head == null) {
            return null;
        }
        
        HashMap<RandomListNode, RandomListNode> map 
          = new HashMap<RandomListNode, RandomListNode>();
        RandomListNode dummy = new RandomListNode(0);
        RandomListNode pre = dummy, newNode;
        
        while(head != null) {
            // copy next pointer
            if(map.containsKey(head)) {
                newNode = map.get(head);
            } else {
                newNode = new RandomListNode(head.label);
                map.put(head, newNode);
            } 
            pre.next = newNode;
            
            // copy random pointer
            if(head.random != null) {
                if(map.containsKey(head.random)) {
                    newNode.random = map.get(head.random);
                } else {
                    newNode.random = new RandomListNode(head.random.label);
                    map.put(head.random, newNode.random);
                }
            }
            pre = newNode;
            head = head.next;
        }
        
        return dummy.next;
    }
}
```



#### 二、复制链表

- 在原链表基础上复制结点。
  - 利用 `next` 指针，在原链表每个结点后面建立复制结点（同时复制 `value` 值， `next` 指针）。
- 调整 `random` 指针关系。
  - 将复制结点的 `random` 指针指向相应的复制结点。
- 拆分链表。

以图示说明（下面的图来自参考链接 2 中的解释）

```
比如链表是
|------------|
|            v
1  --> 2 --> 3 --> 4 

第一遍扫描利用 next 指针，扫描过程中先建立 copy 结点(包括 next 和 random 指针)，得到
|--------------------------|
|                          v
1  --> 1' --> 2 --> 2' --> 3 --> 3' --> 4 --> 4'
       |                   ^
       |-------------------|

第二遍扫描复制 random 指针的 copy ，将复制结点的 random 向后移动一位即可；
|--------------------------|
|                          v
1  --> 1' --> 2 --> 2' --> 3 --> 3' --> 4 --> 4'
       |                         ^
       |-------------------------|

最后拆分结点，一边扫描一边拆成两个链表，这里用到两个dummy node。
拆分后第一个链表为 1-->2-->3-->4，第二链表为 1`-->2`-->3`-->4`。
```

**算法复杂度**

- 时间复杂度：`O(n)` 。
- 空间复杂度：`O(1)` 。【疑问：复制结点也占用了O(n)的空间，为何空间复杂度为O(1)】

##### 易错点

> 1. 在 `copyNext, copyRandom` 函数中 `head` 是局部变量，不会对 `copyRandomList` 中的 `head` 变量产生影响，所以可以把 `head` 当作局部变量使用；考虑到 `Java` 中函数调用的机制，这两个函数却改变了链表的结点内容，所以无需返回值。【注】受限于对 `Java` 语言的理解，目前还不能解释得很清楚。

Java 实现

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    /**
     * @param head: The head of linked list with a random pointer.
     * @return: A new head of a deep copy of the list.
     */
    public RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) {
            return null;
        }
        
        copyNext(head);
        copyRandom(head);
        return split(head);
    }
    
    private void copyNext (RandomListNode head) {
        while (head != null) {
            RandomListNode newNode = new RandomListNode(head.label);
            newNode.next = head.next;
            head.next = newNode;
            head = head.next.next;
        }
    }
    
    private void copyRandom (RandomListNode head) {
        while (head != null) {
            if (head.random != null) {
                head.next.random = head.random.next;
            }
            head = head.next.next;
        }
    }
    
    private RandomListNode split (RandomListNode head) {
        RandomListNode newHead = head.next;
        while (head != null) {
            RandomListNode tmp = head.next;
            head.next = head.next.next;
            if (tmp.next != null) {
                tmp.next = tmp.next.next;
            }
            head = head.next;
        }
        return newHead;
    }
}
```





### 参考

1. [Copy List with Random Pointer | 九章算法](http://www.jiuzhang.com/solutions/copy-list-with-random-pointer/)
2. [Copy List with Random Pointer | LeetCode题解](https://siddontang.gitbooks.io/leetcode-solution/content/linked_list/copy_list_with_random_pointer.html)
3. [Copy List with Random Pointer | 数据结构与算法/leetcode/lintcode题解](https://algorithm.yuanbin.me/zh-hans/index.html)