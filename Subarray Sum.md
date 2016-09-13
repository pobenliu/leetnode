# Subarray Sum

Subarray Sum  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/subarray-sum/) )

```
Description
Given an integer array, find a subarray where the sum of numbers is zero. 
Your code should return the index of the first number and the index of the last number.

Notice
There is at least one subarray that it's sum equals to zero.

Example
Given [-3, 1, 2, -3, 4], return [0, 2] or [1, 3].
```



### 解题思路

#### 一、遍历

使用双重循环依次把所有子序列全部遍历一遍，时间复杂度是 `O(n^2)` 。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number 
     *          and the index of the last number
     */
    public ArrayList<Integer> subarraySum(int[] nums) {
        // write your code here
        ArrayList<Integer> results = new ArrayList<Integer>();
        if (nums == null || nums.length == 0) {
            return results;
        }
        
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i ; j < nums.length; j++) {
                sum += nums[j];
                if (sum == 0) {
                    results.add(i);
                    results.add(j);
                    return results;
                } 
            }
        }
        
        return results;
    }
}
```



#### 二、HashMap

从数组第一个数开始求和 sum，使用 HashMap 记录当前 index 和 sum ，当出现相同的 sum 值时，说明 index1 + 1 到 index2 是一个和为零的子序列。

需要添加一个 index = -1 的虚拟结点，以确保数组第一个数为零的情况可以被记录。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number 
     *          and the index of the last number
     */
    public ArrayList<Integer> subarraySum(int[] nums) {
        // write your code here
        ArrayList<Integer> results = new ArrayList<Integer>();
        if (nums == null || nums.length == 0) {
            return results;
        }
        
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        
        map.put(0, -1);
        
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            
            if (map.containsKey(sum)) {
                results.add(map.get(sum) + 1);
                results.add(i);
                return results;
            }
            map.put(sum, i);
        }
        
        return results;
    }
}
```



### 参考

1. [Lintcode: Subarray Sum 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4174507.html)