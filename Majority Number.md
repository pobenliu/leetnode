#  Majority Number

 Majority Number  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/majority-number/) )

```
Description
Given an array of integers, 
the majority number is the number that occurs more than half of the size of the array. 
Find it.

Notice
You may assume that the array is non-empty and the majority number always exist in the array.

Example
Given [1, 1, 1, 1, 2, 2, 2], return 1

Challenge 
O(n) time and O(1) extra space
```

### 解题思路

如果不考虑 `O(1)` 额外空间的限制，可以使用哈希表记录次数，然后判断次数大于一半的即可。

在本题要求下，使用抵消法。具体实现如下：

- 把链表中的数字视为议案，不断对某个议案进行投票
- 如果下一个提案同当前提案相同，那么投票数加一
- 如果下一个提案不同，那么投票数减一
- 如果投票数归零，那么将当前提案置为投票提案

在确定存在 Majority Number 的时候，该方法最后得到的就是 Majority Number 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: a list of integers
     * @return: find a  majority number
     */
    public int majorityNumber(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return -1;
        }
        
        int count = 0, candidate = -1;
        for (int i = 0; i < nums.size(); i++) {
            if (count == 0) {
                candidate = nums.get(i);
                count++;
            } else if (candidate == nums.get(i)) {
                count++;
            } else {
                count--;
            }
        }
        
        return candidate;
    }
}
```



### 参考

1. [Lintcode: Majority Number 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4175046.html)

