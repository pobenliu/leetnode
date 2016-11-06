# Rehashing

Rehashing  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/rehashing/) )

```
Description
The size of the hash table is not determinate at the very beginning. 
If the total size of keys is too large (e.g. size >= capacity / 10), 
we should double the size of the hash table and rehash every keys. 
Say you have a hash table looks like below:

size=3, capacity=4
[null, 21, 14, null]
       ↓    ↓
       9   null
       ↓
      null
      
The hash function is:
int hashcode(int key, int capacity) {
    return key % capacity;
}

here we have three numbers, 9, 14 and 21, 
where 21 and 9 share the same position as they all have the same hashcode 1 (21 % 4 = 9 % 4 = 1). 
We store them in the hash table by linked list.

rehashing this hash table, double the capacity, you will get:
size=3, capacity=8

index:   0    1    2    3     4    5    6   7
hash : [null, 9, null, null, null, 21, 14, null]
Given the original hash table, return the new hash table after rehashing .

Notice

For negative integer in hash table, the position can be calculated as follow:

- C++/Java: if you directly calculate -4 % 3 you will get -1. 
  You can use function: a % b = (a % b + b) % b to make it is a non negative integer.
- Python: you can directly use -1 % 3, you will get 2 automatically.

Example
Given [null, 21->9->null, 14->null, null],
return [null, 9->null, null, null, null, 21->null, 14->null, null]
```



### 解题思路

依次从原 hash 表中取出结点，重新进行 hash 映射，添加到扩展后的 hash 表即可。需要注意，如何向链表中添加结点，这里要分两种情况，当链表为空时，当链表不为空时。此外链表结点的赋值操作也要注意： `newhashTable[j] = new ListNode(hashTable[i].val);` 。

##### 易错点：

> 1. 向链表数组中的一个链表 `result[j]` 中插入结点时，当链表为空时，插入操作需要使用 `result[j] = new ListNode(value)` ；如果先赋值 `ListNode tmp = result[j]`，再添加结点 `tmp = new ListNode(value)`，无法正常添加结点。
>
>    当链表非空时，则需要先赋值 `ListNode tmp = result[j]`，在 `tmp.next == null` 时再添加结点 `tmp.next = new ListNode(value)`。如果直接使用 `result[j].next = new ListNode(value)` 赋值在某些测试例会出错。**这部分的具体原理目前还不是很明白。**

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
     * @param hashTable: A list of The first node of linked list
     * @return: A list of The first node of linked list which have twice size
     */    
    public ListNode[] rehashing(ListNode[] hashTable) {
        if (hashTable == null || hashTable.length == 0) {
            return null;
        }
        int len = hashTable.length;
        int newLen = len * 2;
        ListNode[] result = new ListNode[newLen];
        
        for (int i = 0; i < len; i++) {
            ListNode curt = hashTable[i];
            while (curt != null) {
                int j = (curt.val % newLen + newLen) % newLen;

                if (result[j] == null) {
                    result[j] = new ListNode(curt.val);
                } else {
                    ListNode dummy = result[j];
                    while (dummy.next != null) {
                        dummy = dummy.next;
                    }
                    dummy.next = new ListNode(curt.val);
                }
                
                curt = curt.next;
            }
        }
        
        return result;
    }
};

```



### 参考



