# 3 Sum

3 Sum ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/3sum/) )

```
Description
Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? 
Find all unique triplets in the array which gives the sum of zero.

Notice
Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)
The solution set must not contain duplicate triplets.

Example
For example, given array S = {-1 0 1 2 -1 -4}, A solution set is:
(-1, 0, 1)
(-1, -1, 2)
```

### 解题思路

#### 一、双指针

三个数之和为零 `a + b + c = 0` ，很容易转化为两个数求和等于一个固定值 `b + c = -a` ，由此本题可转化为题目 two sum 。由于数组中元素可能重复，所以对数组排序后处理会比较方便，而且排序不会增加算法整体的复杂度。在排序后，使用双指针从开头和结尾向中间移动寻找，注意重复元素的处理。

##### 算法复杂度

- 时间复杂度：排序为 `O(nlogn)` ，两重循环寻找三个数为 `O(n^2)` ，所以时间复杂度为 `O(n^2)` 。
- 空间复杂度：`O(n)` 。

##### 易错点

> 1. 重复元素的处理。
> 2. 双指针的边界条件限定。

Java 实现

```java
public class Solution {
    /**
     * @param numbers : Give an array numbers of n integer
     * @return : Find all unique triplets in the array which gives the sum of zero.
     */
    public ArrayList<ArrayList<Integer>> threeSum(int[] numbers) {
        ArrayList<ArrayList<Integer>> results = new ArrayList<ArrayList<Integer>>();
        if (numbers == null || numbers.length < 3) {
            return results;
        }
        
        Arrays.sort(numbers);
        for (int i = 0; i < numbers.length - 2; i++) {
            if (i != 0 && numbers[i] == numbers[i - 1]) {
                continue;   // to skip duplicate numbers. e.g. [0,0,0,0]
            }
             
            int left = i + 1;
            int right = numbers.length - 1;
            while (left < right) {
                int sum = numbers[left] + numbers[right] + numbers[i];
                if (sum == 0) {
                    ArrayList<Integer> tmp = new ArrayList<Integer>();
                    tmp.add(numbers[i]);
                    tmp.add(numbers[left]);
                    tmp.add(numbers[right]);
                    results.add(tmp);
                    left++;
                    right--;
                    while (left < right && numbers[left] == numbers[left - 1]) {
                        left++; // to skip duplicates
                    }
                    while (left < right && numbers[right] == numbers[right + 1]) {
                        right--;
                    }
                } else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        
        return results;
    }
}
```



### 参考

1. [3 Sum | 九章算法](http://www.jiuzhang.com/solutions/3sum/)