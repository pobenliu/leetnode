#   3 Sum Closest

 3 Sum Closest  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/3sum-closest/) )

```
Description
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers.

Notice
You may assume that each input would have exactly one solution.

Example
For example, given array S = [-1 2 1 -4], and target = 1. The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

Challenge 
O(n^2) time, O(1) extra space
```



### 解题思路

#### 一、双指针

本题目的解决方法可参考 3 Sum ，使用双指针，由于不要求返回每个数字的索引，复杂度降低很多，只需要遍历所有三个数字的和，并找到最接近 target 值的和即可。

Java 实现

```java
public class Solution {
    /**
     * @param numbers: Give an array numbers of n integer
     * @param target : An integer
     * @return : return the sum of the three integers, the sum closest target.
     */
    public int threeSumClosest(int[] numbers, int target) {
        if (numbers == null || numbers.length < 3) {
            return -1;
        }
        
        int len = numbers.length;
        int result = Integer.MAX_VALUE;
        
        Arrays.sort(numbers);
        for (int i = 0; i < len - 2; i++) {
            int left = i + 1;
            int right = len - 1;
            while (left < right) {
                int sum = numbers[left] + numbers[right] + numbers[i];
                result = Math.abs(sum - target) < Math.abs(result - target) ? sum : result;
                
                if (sum > target) {
                    right--;
                } else {
                    left++;
                } 
            }
        }
        
        return result;
    }
}

```



### 参考

