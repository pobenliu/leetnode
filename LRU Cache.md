# LRU Cache

LRU Cache  ( [leetcode]() [lintcode]() )

```
Description
Design and implement a data structure for Least Recently Used (LRU) cache. 
It should support the following operations: get and set.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
set(key, value) - Set or insert the value if the key is not already present. 
When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.
```



### 解题思路

Least Recently Used Cache 的基本思想是“最近用到的数据被重用的概率比较早用到的大得多”。

对于 Cache ，如果要求查找复杂度为 `O(1)` ，那么需要使用 HashMap 保存 key 和 对象映射。

而对于 Cache 的插入和删除操作，也需要复杂度为 `O(1)` ，使用双向链表存储 Cache。从头到尾依次按照从旧到新的顺序存储 Cache ，对于任一结点，如果被访问了，将其移至尾部，这样最近被使用过的内容向链表尾部移动，而未使用内容则向链表头部移动；如 Cache 已满，则删除头部结点（近期未使用），在尾部插入新结点。

用到的两个数据结构：

1. HashMap：保存 key 和对象位置的映射。
2. Doubly Linked List：保存对象新旧程度的队列。

Java 实现

```java
public class Solution {
    
    private class Node {
        int key, value;
        Node next, prev;
        public Node (int key, int value) {
            this.key = key;
            this.value = value;
            this.next = null;
            this.prev = null;
        }
    }
    
    private int capacity;
    private HashMap<Integer, Node> hash = new HashMap<Integer, Node>();
    private Node head = new Node(-1, -1);
    private Node tail = new Node(-1, -1);
    
    // @param capacity, an integer
    public Solution(int capacity) {
        // write your code here
        this.capacity = capacity;
        tail.prev = head;
        head.next = tail;
    }

    // @return an integer
    public int get(int key) {
        // write your code here
        if (!hash.containsKey(key)) {
            return -1;
        }
        
        // remove current
        Node current = hash.get(key);
        current.prev.next = current.next;
        current.next.prev = current.prev;
        
        // move current to tail
        move_to_tail(current);
        
        return hash.get(key).value;
    }

    // @param key, an integer
    // @param value, an integer
    // @return nothing
    public void set(int key, int value) {
        // write your code here
        if (get(key) != -1) {
            hash.get(key).value = value;
            return;
        }
        
        if (hash.size() == capacity) {
            hash.remove(head.next.key);
            head.next = head.next.next;
            head.next.prev = head;
        }
        
        Node insert = new Node(key, value);
        hash.put(key, insert);
        move_to_tail(insert);
    }
    
    private void move_to_tail (Node current) {
        current.prev = tail.prev;
        tail.prev = current;
        current.prev.next = current;
        current.next = tail;
    }
}
```



### 参考

1. [LRU Cache | 九章算法](http://www.jiuzhang.com/solutions/lru-cache/)

2. [[LeetCode] LRU Cache, Solution | 水中的鱼](http://fisherlei.blogspot.jp/2013/11/leetcode-lru-cache-solution.html)

3. [LRU Cache leetcode java | 爱做饭的小莹子](http://www.cnblogs.com/springfor/p/3869393.html)

   ​