#  Majority Number II

 Majority Number II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/majority-number-ii/) )

```
Description
Given an array of integers, 
the majority number is the number that occurs more than 1/3 of the size of the array.
Find it.

Notice
There is only one majority number in the array.

Example
Given [1, 2, 1, 2, 1, 3, 3], return 1.

Challenge 
O(n) time and O(1) extra space.
```

### 解题思路

在 Majority Number I 中我们的做法是抵消两个不同的数，以此类推，在本题中，当遇到三个不同的数时进行抵消，由于 Majority Number 超过 1/3 ，所以最终一定会留下来。

我们需要保存两个数 `can1` 和 `can2` 及对应的次数 `c1` 和 `c2` 

- 如果 `c1 == 0` ，将当前值赋给 `can1` ，初始化 `c1` 。
- 如果 `c2 == 0` ，将当前值赋给 `can2` ，初始化 `c2` 。
- 如果当前值等于 `can1` ，增加 `c1` 。
- 如果当前值等于 `can2` ，增加 `c2` 。
- 否则，也就是出现了第三个不同的值，减少 `c1` 和 `c2` 。
- 最后，检查剩下的两个候选值。

之所以最后还要检查候选值，是因为在抵消之后，Majority Number 对应的次数不一定是最多的，比如序列[1 1 1 1 2 3 2 3 4 4 4] ，抵消后 4 出现的次数比 1 1多。

易错点：

> 1. 在 `count1 == 0` 时，未对其进行初始化 `count1 = 1` 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: The majority number that occurs more than 1/3
     */
    public int majorityNumber(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return -1;
        }
        
        int candidate1 = 0, candidate2 = 0;
        int count1 = 0, count2 = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums.get(i) == candidate1) {
                count1++;
            } else if (nums.get(i) == candidate2) {
                count2++;
            } else if (count1 == 0) {
                candidate1 = nums.get(i);
                count1 = 1;
            } else if (count2 == 0) {
                candidate2 = nums.get(i);
                count2 = 1;
            } else {
                count1--;
                count2--;
            }
        }
        
        count1 = count2 = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums.get(i) == candidate1) {
                count1++;
            } else if (nums.get(i) == candidate2) {
                count2++;
            }
        }
        return count1 > count2 ? candidate1 : candidate2;
    }
}

```



### 参考

1. [Lintcode: Majority Number II 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4175779.html) 

