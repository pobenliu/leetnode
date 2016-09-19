#  Single Number III

 Single Number III  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/single-number-iii/) )

```
Description
Given 2*n + 2 numbers, every numbers occurs twice except two, find them.

Example
Given [1,2,2,3,4,4,5,3] return 1 and 5

Challenge 
O(n) time, O(1) extra space.
```

### 解题思路

由于有两个不同的 single number A 和 B ，所以如果直接对所有数求异或，得到的结果是 A ^ B ，无法直接区分两者。但是由于 A 和 B 不相等，所以异或结果一定不为 0 ，可以找到两者不相同的某一位，通过按位与操作将数组分为两部分，这样 A 和 B 就被划分到不同的子数组中，再分别进行异或操作即可得到两者。

易错点

> 1. `List<Integer>` 需要初始化为 `ArrayList<Integer>` ，不能直接初始化为 `List<Integer>` ，否则会报错 `List is abstract; cannot be instantiated List results = new List();` 。

Java 实现

```java
public class Solution {
    /**
     * @param A : An integer array
     * @return : Two integers
     */
    public List<Integer> singleNumberIII(int[] A) {
        if (A == null || A.length == 0) {
            return null;
        }
        
        int xor = A[0];
        for (int i = 1; i < A.length; i++) {
            xor ^= A[i];
        }
        
        int lastBit = xor - ((xor - 1) & xor);
        
        int a1 = 0, a2 = 0;
        for (int i = 0; i < A.length; i++) {
            if ((A[i] & lastBit) == 0) {
                a1 ^= A[i];
            } else {
                a2 ^= A[i];
            }
        }
        
        List<Integer> results = new ArrayList<Integer>();
        results.add(a1);
        results.add(a2);
        return results;
    }
} 
```



### 参考

1. [[Leetcode] Single Number III, Solution | 水中的鱼](http://fisherlei.blogspot.jp/2015/10/leetcode-single-number-iii-solution.html)