# Rehashing

Rehashing  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/rehashing/) )

```
Description
The size of the hash table is not determinate at the very beginning. 
If the total size of keys is too large (e.g. size >= capacity / 10), we should double the size of the hash table and rehash every keys. 
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

here we have three numbers, 9, 14 and 21, where 21 and 9 share the same position as they all have the same hashcode 1 (21 % 4 = 9 % 4 = 1). 
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

依次从原 hash 表中取出结点，并装入扩展后的 hash 表即可。里面涉及链表的操作需要注意，主要是如何向链表中添加结点，这里要分两种情况，当链表为空时，当链表不为空时，此外链表结点的赋值操作也要注意： `newhashTable[j] = new ListNode(hashTable[i].val);` 。



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
        // write your code here
        if (hashTable == null || hashTable.length == 0) {
            return hashTable;
        }
        
        int len = hashTable.length;
        int newlen = len * 2;
        ListNode[] newhashTable = new ListNode[newlen];

        for (int i = 0; i < len; i++) {
            while (hashTable[i] != null) {
                // j is the index of newhashTable
                int j = (hashTable[i].val % newlen + newlen) % newlen;

                if (newhashTable[j] == null) {
                    // attention here for the assignment
                    newhashTable[j] = new ListNode(hashTable[i].val);
                } else {
                    ListNode dummy = newhashTable[j];
                    while (dummy.next != null ) {
                        dummy = dummy.next;
                    }
                        dummy.next = new ListNode(hashTable[i].val);
                }
                
                hashTable[i] = hashTable[i].next;
            }
        }
        
        return newhashTable;
    }
};

```



以下实现和上面的实现有细微的差别，但是以下测试例报错，不太清楚是哪里的问题

testcase

```
[80->null,null,null,null,null,null,null,187->null,null,49->109->null,10->50->-10->null,null,12->null,53->133->153->93->null,null,15->null,36->null,-3->null,118->null,159->139->null]
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
     * @param hashTable: A list of The first node of linked list
     * @return: A list of The first node of linked list which have twice size
     */    
    public ListNode[] rehashing(ListNode[] hashTable) {
        // write your code here
        if (hashTable == null || hashTable.length == 0) {
            return hashTable;
        }
        
        int len = hashTable.length;
        int newlen = len * 2;
        ListNode[] newhashTable = new ListNode[newlen];

        for (int i = 0; i < len; i++) {
            while (hashTable[i] != null) {
                // j is the index of newhashTable
                int j = (hashTable[i].val % newlen + newlen) % newlen;

                if (newhashTable[j] == null) {
                    newhashTable[j] = new ListNode(hashTable[i].val);
                } else {
                    // bug here but don't know why
                    while (newhashTable[j].next != null ) {
                        newhashTable[j] = newhashTable[j].next;
                    }
                        newhashTable[j].next = new ListNode(hashTable[i].val);
                }
                
                hashTable[i] = hashTable[i].next;
            }
        }
        
        return newhashTable;
    }
};
```



### 参考



